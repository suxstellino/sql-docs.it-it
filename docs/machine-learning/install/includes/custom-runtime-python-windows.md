---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 9f58ddf6b58e32d40b2962b47255aba2e553acca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072954"
---
## <a name="prerequisites"></a>Prerequisiti

Prima di installare un runtime personalizzato di Python, installare:

+ Se si usa un'istanza di SQL Server esistente, installare l' [aggiornamento cumulativo (Cu) 3 o versione successiva](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) per SQL Server 2019.

## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è installato in SQL Server 2019, le estensioni del linguaggio sono già installate ed è possibile ignorare questo passaggio.

Attenersi alla procedura seguente per installare le [Estensioni del linguaggio di SQL Server](../../../language-extensions/language-extensions-overview.md), che vengono usate per il runtime personalizzato di Python.

1. Avviare l'Installazione guidata di SQL Server 2019.
  
1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

1. Nella pagina **Selezione funzionalità** selezionare queste opzioni:
  
    + **Servizi motore di database**
  
        Per usare le estensioni del linguaggio con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza nuova o esistente.
  
    + **Machine Learning Services ed estensioni del linguaggio**

        Selezionare **Machine Learning Services ed estensioni del linguaggio**. Non selezionare Python, perché in seguito verrà installato il runtime di Python personalizzato.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Installazione delle estensioni del linguaggio di SQL Server 2019.":::

1. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.
  
    + Servizi motore di database
    + Machine Learning Services ed estensioni del linguaggio

1. Al termine dell'installazione, riavviare il computer, se richiesto.

> [!IMPORTANT]
> Se si installa una nuova istanza di SQL Server 2019 con le estensioni del linguaggio, installare l'[aggiornamento cumulativo (CU) 3 o versione successiva](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) prima di continuare con il passaggio successivo.

## <a name="install-python"></a>Installare Python

L'estensione del linguaggio Python usata per il runtime Python personalizzato attualmente supporta solo Python 3.7. Se si vuole usare una versione diversa di Python, seguire le istruzioni nel [repository GitHub Estensione del linguaggio Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) per modificare e ricompilare l'estensione.

1. Scaricare [Python 3,7](https://www.python.org/downloads/windows/) per Windows ed eseguire il programma di installazione nel server.

1. Selezionare **Aggiungi Python 3,7 a percorso** , quindi selezionare **Personalizza installazione**.

    :::image type="content" source="../media/python-install-add-to-path.png" alt-text="Installazione di Python 3,7-aggiungere Python 3,7 al percorso":::

1. In **funzionalità facoltative** lasciare le impostazioni predefinite e fare clic su **Avanti**.

1. Selezionare **Installa per tutti gli utenti** e prendere nota del percorso di installazione.

    :::image type="content" source="../media/python-install-for-all-users.png" alt-text="Installazione di Python 3,7-installazione per tutti gli utenti":::

1. Selezionare **Installa**.

## <a name="install-pandas"></a>Installare pandas

Installare il pacchetto [Pandas](https://pandas.pydata.org/) per Python da un prompt dei comandi con *privilegi elevati* (Esegui come amministratore):

```bash
python.exe -m pip install pandas
```

## <a name="grant-access-to-python-folder"></a>Concedere l'accesso alla cartella di Python

Eseguire i comandi **icacls** seguenti da un nuovo prompt dei comandi con *privilegi elevati* per concedere l'accesso in **lettura & esecuzione** al percorso di installazione di Python per **launchpad di SQL Server servizio** e SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

Gli esempi seguenti usano il percorso di installazione di Python come `C:\Program Files\Python37` . Se il percorso è diverso, modificarlo nel comando.

1. Concedere le autorizzazioni a **nome utente del servizio Launchpad di SQL Server**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Per l'istanza denominata, il comando sarà `icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` per un'istanza denominata **SQL01**.

2. Concedere le autorizzazioni al **SID S-1-15-2-1**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Il comando precedente concede le autorizzazioni al SID del computer **S-1-15-2-1**, che equivale a **ALL APPLICATION PACKAGES** (TUTTI I PACCHETTI APPLICAZIONI) in una versione di Windows in inglese. In alternativa è possibile usare `icacls "C:\Program Files\Python37" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in una versione di Windows in inglese.

## <a name="restart-sql-server-launchpad"></a>Riavviare Launchpad di SQL Server

Per riavviare il servizio Launchpad di SQL Server, attenersi alla seguente procedura.

1. Aprire [Gestione configurazione SQL Server](../../../relational-databases/sql-server-configuration-manager.md).

1. In **Servizi di SQL Server** fare clic con il pulsante destro del mouse su **Launchpad di SQL Server (MSSQLSERVER)** e selezionare **Riavvia**. Se si utilizza un'istanza denominata, il nome dell'istanza verrà visualizzato anziché **(MSSQLSERVER)**.

## <a name="register-language-extension"></a>Registrare l'estensione del linguaggio

Seguire questa procedura per scaricare e registrare l'estensione del linguaggio Python, che viene usata per il runtime personalizzato di Python.

1. Scaricare il file di **python-lang-extension-windows-release.zip** dal [repository GitHub SQL Server Language Extensions](https://github.com/microsoft/sql-server-language-extensions/releases).

    In alternativa, è possibile usare la versione di debug (**python-lang-extension-windows-debug.zip**) in un ambiente di sviluppo o di test. La versione di debug fornisce informazioni di registrazione dettagliate per esaminare eventuali errori e non è consigliata per gli ambienti di produzione.

1. Usare [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) per connettersi all'istanza di SQL Server ed eseguire il comando T-SQL seguente per registrare l'estensione del linguaggio Python con [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md).

    Modificare il percorso in questa istruzione in modo da riflettere il percorso del file zip dell'estensione del linguaggio scaricato (**python-lang-extension-windows-release.zip**) e il percorso di installazione di Python ( `C:\\Program Files\\Python3.7` ).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'C:\path\to\python-lang-extension-windows-release.zip', 
        FILE_NAME = 'pythonextension.dll', 
        ENVIRONMENT_VARIABLES = N'{"PYTHONHOME": "C:\\Program Files\\Python3.7"}');
    GO
    ```

    Eseguire l'istruzione per ogni database in cui si desidera usare l'estensione del linguaggio Python.

    > [!NOTE]
    > **Python** è una parola riservata e non può essere usata come nome per un nuovo nome di linguaggio esterno. Usare un altro nome. Ad esempio, l'istruzione precedente usa **myPython**.
