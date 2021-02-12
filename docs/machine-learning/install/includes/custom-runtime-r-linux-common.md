---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 07bd68eaee05893174813ed43dc5358104016e4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072777"
---
## <a name="custom-installation-of-r"></a>Installazione personalizzata di R

> [!NOTE]
> Se R è stato installato nel percorso predefinito di **/usr/lib/R**, è possibile ignorare questa sezione e passare alla sezione [Install RCPP Package](#install-rcpp-package-linux) .

### <a name="update-the-environment-variables"></a>Aggiornare le variabili di ambiente

Prima di tutto, modificare il servizio **MSSQL-launchpadd** per aggiungere la variabile di ambiente **R_HOME** al file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

1. Aprire il file con systemctl

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Inserire il testo seguente nel file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` che si apre. Impostare il valore di **R_HOME** sul percorso di installazione di R personalizzato.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

1. Salvare e chiudere.

Quindi, verificare che `libR.so` possa essere caricato.

1. Creare un file custom-r.conf in **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

1. Nel file che si apre aggiungere il percorso di **libR.so** dall'installazione di R personalizzata.

    ```
    /path/to/installation/of/R/lib
    ```

1. Salvare il nuovo file e chiudere l'editor.

1. Eseguire `ldconfig` e verificare che sia possibile caricare **libR.so** eseguendo il comando seguente e verificando che tutte le librerie dipendenti siano disponibili.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Concedere l'accesso alla cartella di installazione personalizzata di R

Impostare l' `datadirectories` opzione nella sezione Extensibility del `/var/opt/mssql/mssql.conf` file sull'installazione di R personalizzata.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Riavviare il servizio mssql-launchpadd

Eseguire il comando seguente per riavviare **MSSQL-launchpadd**.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="install-rcpp-package-linux"></a>

## <a name="install-rcpp-package"></a>Installare il pacchetto Rcpp

Per installare il pacchetto **RCPP** , seguire questa procedura.

1. Avviare R da una shell:

    ```bash
    sudo ${R_HOME}/bin/R
    ```

1. Eseguire lo script seguente per installare il pacchetto RCPP nella cartella $ {R_HOME} \Library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="register-language-extension"></a>Registrare l'estensione del linguaggio

Seguire questa procedura per scaricare e registrare l'estensione del linguaggio R, che viene usata per il runtime personalizzato di R.

1. Scaricare il file di **R-lang-extension-linux-release.zip** dal [repository GitHub SQL Server Language Extensions](https://github.com/microsoft/sql-server-language-extensions/releases).

    In alternativa, è possibile usare la versione di debug (**R-lang-extension-linux-debug.zip**) in un ambiente di sviluppo o di test. La versione di debug fornisce informazioni di registrazione dettagliate per esaminare eventuali errori e non è consigliata per gli ambienti di produzione.

1. Usare [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) per connettersi all'istanza di SQL Server ed eseguire il comando T-SQL seguente per registrare l'estensione del linguaggio R con [Create external language](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Modificare il percorso in questa istruzione in modo che corrisponda al percorso del file zip dell'estensione del linguaggio scaricato (**R-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'/path/to/R-lang-extension-linux-release.zip', FILE_NAME = 'libRExtension.so.1.0');
    GO
    ```

    Eseguire l'istruzione per ogni database in cui si vuole usare l'estensione del linguaggio R.

    > [!NOTE]
    > **R** è una parola riservata e non può essere usata come nome per un nuovo nome di lingua esterna. Usare un altro nome. Ad esempio, l'istruzione precedente USA **myR**.
