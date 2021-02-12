---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 1303df98c60212c13a233d1e386c60109308e981
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072918"
---
## <a name="custom-installation-of-python"></a>Installazione personalizzata di Python

> [!NOTE]
> Se è stato installato Python 3.7 nel percorso predefinito di `/usr/lib/python3.7`, è possibile ignorare questa sezione e passare alla sezione [Registrare l'estensione del linguaggio](#register-language-extension-linux).

Se è stata creata una versione personalizzata di Python 3.7, usare i comandi seguenti per comunicare a SQL Server l'installazione personalizzata.

### <a name="add-environment-variable"></a>Aggiungere una variabile di ambiente

In primo luogo, modificare il servizio **mssql-launchpadd** per aggiungere la variabile di ambiente **PYTHONHOME** al file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

1. Aprire il file con systemctl

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Inserire il testo seguente nel file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` che si apre. Impostare il valore di **PYTHONHOME** sul percorso di installazione personalizzato di Python.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Salvare il file e chiudere l'editor.

Quindi, verificare che `libpython3.7m.so.1.0` possa essere caricato.

1. Creare un file python.conf personalizzato in `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. Nel file che si apre aggiungere il percorso **libpython3.7m.so.1.0** dall'installazione personalizzata di Python.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Salvare il nuovo file e chiudere l'editor.

1. Eseguire `ldconfig` e verificare che sia possibile caricare `libpython3.7m.so.1.0` eseguendo i comandi seguenti e verificando che tutte le librerie dipendenti siano disponibili.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Concedere l'accesso alla cartella di Python

Impostare l' `datadirectories` opzione nella sezione Extensibility del `/var/opt/mssql/mssql.conf` file sull'installazione personalizzata di Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Riavviare mssql-launchpadd

Eseguire il comando seguente per riavviare **MSSQL-launchpadd**.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Registrare l'estensione del linguaggio

Seguire questa procedura per scaricare e registrare l'estensione del linguaggio Python, che viene usata per il runtime personalizzato di Python.

1. Scaricare il file di **python-lang-extension-linux-release.zip** dal [repository GitHub SQL Server Language Extensions](https://github.com/microsoft/sql-server-language-extensions/releases).

    In alternativa, è possibile usare la versione di debug (**python-lang-extension-linux-debug.zip**) in un ambiente di sviluppo o di test. La versione di debug fornisce informazioni di registrazione dettagliate per esaminare eventuali errori e non è consigliata per gli ambienti di produzione.

1. Usare [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) per connettersi all'istanza di SQL Server ed eseguire il comando T-SQL seguente per registrare l'estensione del linguaggio Python con [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Modificare il percorso in questa istruzione in modo che corrisponda al percorso del file zip dell'estensione del linguaggio scaricato (**python-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux-release.zip', FILE_NAME = 'libPythonExtension.so.1.1');
    GO
    ```

    Eseguire l'istruzione per ogni database in cui si desidera usare l'estensione del linguaggio Python.

    > [!NOTE]
    > **Python** è una parola riservata e non può essere usata come nome per un nuovo nome di linguaggio esterno. Usare un altro nome. Ad esempio, l'istruzione precedente usa **myPython**.
