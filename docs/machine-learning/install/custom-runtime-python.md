---
title: Installare il runtime personalizzato di Python
description: Informazioni su come installare un runtime personalizzato di Python per SQL Server tramite le estensioni del linguaggio. Il runtime personalizzato di Python può essere usato per l'apprendimento automatico.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 91aace4333b4496338b782344e64cdfea2b886bd
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804308"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Installare un runtime personalizzato di Python per SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive come installare un runtime personalizzato di Python per l'esecuzione di script Python esterni con SQL Server. Il runtime personalizzato usa le [Estensioni del linguaggio di SQL Server](../../language-extensions/language-extensions-overview.md) e può essere usato per l'esecuzione di script di machine learning.

Il runtime personalizzato di Python consente di usare la propria versione del runtime Python con SQL Server, anziché la versione predefinita del runtime installata con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="python-custom-runtime-windows"

## <a name="prerequisites"></a>Prerequisiti

Prima di installare un runtime personalizzato di Python, installare quanto segue:

+ Installare [l'aggiornamento cumulativo (CU) 3 o versione successiva](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) per SQL Server 2019.

+ Installare [Python 3.7](https://www.python.org/downloads/) sul server.

    L'estensione del linguaggio Python usata per il runtime Python personalizzato attualmente supporta solo Python 3.7. Se si vuole usare una versione diversa di Python, seguire le istruzioni nel [repository GitHub Estensione del linguaggio Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) per modificare e ricompilare l'estensione.

    > [!IMPORTANT]
    > Durante l'installazione di Python, selezionare **Add Python 3.7 to PATH** (Aggiungi Python 3.7 a PATH).

## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../sql-server-machine-learning-services.md) è installato in SQL Server 2019, le estensioni del linguaggio sono già installate ed è possibile ignorare questo passaggio.

Attenersi alla procedura seguente per installare le [Estensioni del linguaggio di SQL Server](../../language-extensions/language-extensions-overview.md), che vengono usate per il runtime personalizzato di Python.

1. Avviare l'Installazione guidata di SQL Server 2019.
  
1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

1. Nella pagina **Selezione funzionalità** selezionare queste opzioni:
  
    + **Servizi motore di database**
  
        Per usare le estensioni del linguaggio con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza nuova o esistente.
  
    + **Machine Learning Services ed estensioni del linguaggio**

        Selezionare **Machine Learning Services ed estensioni del linguaggio**. Non selezionare Python, perché in seguito verrà installato il runtime di Python personalizzato.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Installazione delle estensioni del linguaggio di SQL Server 2019.":::

1. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.
  
    + Servizi motore di database
    + Machine Learning Services ed estensioni del linguaggio

1. Al termine dell'installazione, riavviare il computer, se richiesto.

> [!IMPORTANT]
> Se si installa una nuova istanza di SQL Server 2019 con le estensioni del linguaggio, installare l'[aggiornamento cumulativo (CU) 3 o versione successiva](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) prima di continuare con il passaggio successivo.

## <a name="install-pandas"></a>Installare pandas

Installare il pacchetto [pandas](https://pandas.pydata.org/) per Python da un prompt dei comandi con *privilegi elevati*:

```bash
python.exe -m pip install pandas
```

## <a name="add-environment-variable"></a>Aggiungere una variabile di ambiente

Aggiungere o modificare la variabile di ambiente del sistema **PYTHONHOME**.

1. Nella casella di ricerca di Windows digitare *ambiente* e selezionare **Modifica le variabili di ambiente relative al sistema**.
1. Nella scheda **Avanzate** selezionare **Variabili di ambiente**.
1. In **Variabili di sistema** selezionare **Nuova** per creare **PYTHONHOME** in modo che punti al percorso di installazione di Python 3.7. Se la variabile PYTHONHOME esiste già, selezionare **Modifica** per puntare al percorso di installazione di Python 3.7.
1. Selezionare **OK** per chiudere tutte le finestre.

    :::image type="content" source="media/pythonhome-env-variable.png" alt-text="Variabile di ambiente PYTHONHOME.":::

## <a name="grant-access-to-python-folder"></a>Concedere l'accesso alla cartella di Python

Eseguire i seguenti comandi **icacls** da un nuovo prompt dei comandi *con privilegi elevati* per concedere l'accesso in **lettura ed esecuzione** a **PYTHONHOME** per il **servizio Launchpad di SQL Server** e il SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

1. Concedere le autorizzazioni a **nome utente del servizio Launchpad di SQL Server**.

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Per l'istanza denominata, il comando sarà `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` per un'istanza denominata **SQL01**.

2. Concedere le autorizzazioni al **SID S-1-15-2-1**.

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Il comando precedente concede le autorizzazioni al SID del computer **S-1-15-2-1**, che equivale a **ALL APPLICATION PACKAGES** (TUTTI I PACCHETTI APPLICAZIONI) in una versione di Windows in inglese. In alternativa è possibile usare `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in una versione di Windows in inglese.

## <a name="restart-sql-server-launchpad"></a>Riavviare Launchpad di SQL Server

Per riavviare il servizio Launchpad di SQL Server, attenersi alla seguente procedura.

1. Aprire [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

1. In **Servizi di SQL Server** fare clic con il pulsante destro del mouse su **Launchpad di SQL Server (MSSQLSERVER)** e selezionare **Riavvia**. Se si utilizza un'istanza denominata, il nome dell'istanza verrà visualizzato al posto di **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Registrare l'estensione del linguaggio

Seguire questa procedura per scaricare e registrare l'estensione del linguaggio Python, che viene usata per il runtime personalizzato di Python.

1. Scaricare il file **python-lang-extension-windows.zip** dal [repository GitHub Estensioni del linguaggio di SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    In alternativa, è possibile usare la versione di debug (**python-lang-extension-windows-debug.zip**) in un ambiente di sviluppo o di test. La versione di debug fornisce informazioni di registrazione dettagliate per esaminare eventuali errori e non è consigliata per gli ambienti di produzione.

1. Usare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) per connettersi all'istanza di SQL Server ed eseguire il comando T-SQL seguente per registrare l'estensione del linguaggio Python con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). 

    Modificare il percorso in questa istruzione in modo che corrisponda al percorso del file ZIP dell'estensione del linguaggio scaricato (**python-lang-extension-windows.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-windows.zip', FILE_NAME = 'pythonextension.dll');
    GO
    ```

    Eseguire l'istruzione per ogni database in cui si desidera usare l'estensione del linguaggio Python.

    > [!NOTE]
    > **Python** è una parola riservata e non può essere usata come nome per un nuovo nome di linguaggio esterno. Usare un altro nome. Ad esempio, l'istruzione precedente usa **myPython**.

::: zone-end

::: zone pivot="python-custom-runtime-linux"

## <a name="prerequisites"></a>Prerequisiti

Prima di installare un runtime personalizzato di Python, installare quanto segue:

+ Installare SQL Server 2019 per Linux. È possibile installare SQL Server in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) e Ubuntu. Per altre informazioni, vedere le [Linee guida per l'installazione di SQL Server in Linux](../../linux/sql-server-linux-setup.md).

+ Installare l'aggiornamento cumulativo (CU) 3 o versione successiva per SQL Server 2019. Seguire questa procedura:
    1. Configurare i repository per gli aggiornamenti cumulativi. Per altre informazioni, vedere [Configurare i repository per l'installazione e l'aggiornamento di SQL Server in Linux](../../linux/sql-server-linux-change-repo.md).

    1. Aggiornare il pacchetto **mssql-server** all'aggiornamento cumulativo più recente. Per altre informazioni, vedere [la sezione sull'aggiornamento di SQL Server nelle linee guida per l'installazione di SQL Server in Linux](../../linux/sql-server-linux-setup.md#upgrade).

+ Installare [Python 3.7](https://www.python.org/downloads/) sul server.

    L'estensione del linguaggio Python usata per il runtime Python personalizzato attualmente supporta solo Python 3.7. Se si vuole usare una versione diversa di Python, seguire le istruzioni nel [repository GitHub Estensione del linguaggio Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) per modificare e ricompilare l'estensione.

## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../sql-server-machine-learning-services.md) è installato in SQL Server 2019, il pacchetto **mssql-server-extensibility** per le estensioni del linguaggio è già installato ed è possibile ignorare questo passaggio.

Attenersi ai comandi seguenti per installare le [Estensioni del linguaggio di SQL Server](../../language-extensions/language-extensions-overview.md) in Linux, che vengono usate per il runtime personalizzato di Python.

#### <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

1. Se possibile, eseguire questo comando per aggiornare i pacchetti nel sistema prima dell'installazione.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu potrebbe non avere l'opzione di trasporto https apt. Per installarla, usare questo comando.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Installare **mssql-server-extensibility** con questo comando.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

#### <a name="red-hat-enterprise-linux-rhel"></a>[Red Hat Enterprise Linux (RHEL)](#tab/rhel)

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

#### <a name="suse-linux-enterprise-server-sles"></a>[SUSE Linux Enterprise Server (SLES)](#tab/sles)

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

---

## <a name="install-python-37-and-pandas"></a>Installare Python 3.7 e pandas

Installare Python 3.7, la libreria libpython3.7 e il pacchetto pandas. 

Di seguito sono riportati alcuni comandi di esempio per Ubuntu:

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

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

Impostare l'opzione `datadirectories` nella sezione extensibility del file /var/opt/mssql/mssql.conf sull'installazione personalizzata di Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Riavviare mssql-launchpadd

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Registrare l'estensione del linguaggio

Seguire questa procedura per scaricare e registrare l'estensione del linguaggio Python, che viene usata per il runtime personalizzato di Python.

1. Scaricare il file **python-lang-extension-linux.zip** dal [repository GitHub Estensioni del linguaggio di SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    In alternativa, è possibile usare la versione di debug (**python-lang-extension-linux-debug.zip**) in un ambiente di sviluppo o di test. La versione di debug fornisce informazioni di registrazione dettagliate per esaminare eventuali errori e non è consigliata per gli ambienti di produzione.

1. Usare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) per connettersi all'istanza di SQL Server ed eseguire il comando T-SQL seguente per registrare l'estensione del linguaggio Python con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md). 

    Modificare il percorso in questa istruzione in modo che corrisponda al percorso del file ZIP dell'estensione del linguaggio scaricato (**python-lang-extension-linux.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux.zip', FILE_NAME = 'libPythonExtension.so.1.0');
    GO
    ```

    Eseguire l'istruzione per ogni database in cui si desidera usare l'estensione del linguaggio Python.

    > [!NOTE]
    > **Python** è una parola riservata e non può essere usata come nome per un nuovo nome di linguaggio esterno. Usare un altro nome. Ad esempio, l'istruzione precedente usa **myPython**.

::: zone-end

## <a name="enable-external-script"></a>Abilitare lo script esterno

È possibile eseguire uno script esterno di Python con la stored procedure [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Per abilitare gli script esterni, usare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) per eseguire l'istruzione seguente.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Verificare l'installazione

Usare lo script SQL seguente per verificare l'installazione e la funzionalità del runtime personalizzato di Python.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Passaggi successivi

+ [Installare un runtime personalizzato di R per SQL Server](custom-runtime-r.md)
+ [Framework di estendibilità in SQL Server](../concepts/extensibility-framework.md)
+ [Panoramica delle estensioni del linguaggio](../../language-extensions/language-extensions-overview.md)
