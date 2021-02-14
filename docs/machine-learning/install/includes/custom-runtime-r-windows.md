---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 2a156ab122f0eadce71da9f4fcaf9e99584c8e32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072768"
---
## <a name="prerequisites"></a>Prerequisiti

Prima di installare un runtime personalizzato di R, installare quanto segue:

+ Se si usa un'istanza di SQL Server esistente, installare l' [aggiornamento cumulativo (Cu) 3 o versione successiva](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) per SQL Server 2019.

## <a name="install-language-extensions"></a>Installare le estensioni del linguaggio

> [!NOTE]
> Se [Machine Learning Services](../../sql-server-machine-learning-services.md) è installato in SQL Server 2019, le estensioni del linguaggio sono già installate ed è possibile ignorare questo passaggio.

Attenersi alla procedura seguente per installare [SQL Server Language Extensions](../../../language-extensions/language-extensions-overview.md), che viene usato per il runtime di R Custom.

1. Avviare l'Installazione guidata di SQL Server 2019.
  
1. Nella scheda **Installazione** selezionare **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.

1. Nella pagina **Selezione funzionalità** selezionare queste opzioni:
  
    + **Servizi motore di database**
  
        Per usare le estensioni del linguaggio con SQL Server, è necessario installare un'istanza del motore di database. È possibile usare un'istanza nuova o esistente.
  
    + **Machine Learning Services ed estensioni del linguaggio**

        Selezionare **Machine Learning Services ed estensioni del linguaggio**. Non selezionare R, perché verrà installato il runtime di R personalizzato in un secondo momento.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Installazione delle estensioni del linguaggio di SQL Server 2019.":::

1. Nella pagina **Inizio installazione** verificare che le opzioni selezionate siano incluse e selezionare **Installa**.
  
    + Servizi motore di database
    + Machine Learning Services ed estensioni del linguaggio

1. Al termine dell'installazione, riavviare il computer, se richiesto.

> [!IMPORTANT]
> Se si installa una nuova istanza di SQL Server 2019 con le estensioni del linguaggio, installare l'[aggiornamento cumulativo (CU) 3 o versione successiva](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) prima di continuare con il passaggio successivo.

## <a name="install-r"></a>Installare R

Scaricare e installare la versione di R che si userà come runtime personalizzato. Sono supportate le versioni R 3,3 o successive.

1. Scaricare [R versione 3,3 o successiva](https://cran.r-project.org/bin/windows/base/).

1. Eseguire l'installazione di R.

1. Prendere nota del percorso in cui è installato R. In questo articolo, ad esempio, è `C:\Program Files\R\R-4.0.3` .

    :::image type="content" source="../media/r-setup-path.png" alt-text="Installazione di R":::

## <a name="update-system-environment-variable"></a>Aggiorna variabile di ambiente di sistema

Attenersi alla seguente procedura per modificare le variabili di ambiente di sistema **path** .

1. Nella casella di ricerca di Windows cercare **modifica le variabili di ambiente di sistema** e aprirla.

1. In **Avanzate** selezionare **variabili di ambiente**.

1. Modificare la variabile di ambiente di sistema **path** .

    Selezionare **percorso** e fare clic su **modifica**.

    Selezionare **nuovo** e aggiungere il percorso della `\bin\x64` cartella nel percorso di installazione di R. Ad esempio: `C:\Program Files\R\R-4.0.3\bin\x64`.

## <a name="install-rcpp-package"></a>Installare il pacchetto Rcpp

Per installare il pacchetto **RCPP** , seguire questa procedura.

1. Avviare un prompt dei comandi con *privilegi elevati* (Esegui come amministratore).

1. Avviare R dal prompt dei comandi. Eseguire `\bin\R.exe` nella cartella nel percorso di installazione di R. Ad esempio: `C:\Program Files\R\R-4.0.3\bin\R.exe`.

    ```CMD
    "C:\Program Files\R\R-4.0.3\bin\R.exe"
    ```

1. Eseguire lo script seguente per installare il pacchetto RCPP nella `\library` cartella nel percorso di installazione di R. Ad esempio: `C:\Program Files\R\R-4.0.3\library`.

    ```R
    install.packages("Rcpp", lib="C:\Program Files\R\R-4.0.3\library");
    ```

## <a name="grant-access-to-r-folder"></a>Concedi accesso alla cartella R

> [!NOTE]
> Se R è stato installato nel percorso predefinito di `C:\Program Files\R\R-version` (ad esempio, `C:\Program Files\R\R-4.0.3` ), è possibile ignorare questo passaggio.

Eseguire i comandi **icacls** seguenti da un nuovo prompt dei comandi con *privilegi elevati* per concedere l'accesso in **lettura & esecuzione** al **nome utente del servizio launchpad di SQL Server** e al SID **S-1-15-2-1** (**tutti i pacchetti di applicazioni**). Il nome utente del servizio Launchpad è nel formato `NT Service\MSSQLLAUNCHPAD$INSTANCENAME` dove `INSTANCENAME` è il nome dell'istanza di SQL Server.

I comandi concederanno l'accesso in modo ricorsivo a tutti i file e le cartelle nel percorso di directory specificato.

1. Concedere le autorizzazioni per **launchpad di SQL Server nome utente del servizio** al percorso di installazione di R. Ad esempio: `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Per l'istanza denominata, il comando sarà `icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` per un'istanza denominata **SQL01**.

2. Concedere le autorizzazioni per **SID S-1-15-2-1** al percorso di installazione di R. Ad esempio: `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Il comando precedente concede le autorizzazioni al SID del computer **S-1-15-2-1**, che equivale a **ALL APPLICATION PACKAGES** (TUTTI I PACCHETTI APPLICAZIONI) in una versione di Windows in inglese. In alternativa è possibile usare `icacls "C:\Program Files\R\R-4.0.3" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in una versione di Windows in inglese.

## <a name="restart-sql-server-launchpad"></a>Riavviare Launchpad di SQL Server

Per riavviare il servizio Launchpad di SQL Server, attenersi alla seguente procedura.

1. Aprire [Gestione configurazione SQL Server](../../../relational-databases/sql-server-configuration-manager.md).

1. In **Servizi di SQL Server** fare clic con il pulsante destro del mouse su **Launchpad di SQL Server (MSSQLSERVER)** e selezionare **Riavvia**. Se si utilizza un'istanza denominata, il nome dell'istanza verrà visualizzato al posto di **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Registrare l'estensione del linguaggio

Seguire questa procedura per scaricare e registrare l'estensione del linguaggio R, che viene usata per il runtime personalizzato di R.

1. Scaricare il file di **R-lang-extension-windows-release.zip** dal [repository GitHub SQL Server Language Extensions](https://github.com/microsoft/sql-server-language-extensions/releases).

    In alternativa, è possibile usare la versione di debug (**R-lang-extension-windows-debug.zip**) in un ambiente di sviluppo o di test. La versione di debug fornisce informazioni di registrazione dettagliate per esaminare eventuali errori e non è consigliata per gli ambienti di produzione.

1. Usare [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) per connettersi all'istanza di SQL Server ed eseguire il comando T-SQL seguente per registrare l'estensione del linguaggio R con [Create external language](../../../t-sql/statements/create-external-language-transact-sql.md).

    Modificare il percorso in questa istruzione in modo da riflettere il percorso del file zip dell'estensione del linguaggio scaricato (**R-lang-extension-windows-release.zip**) e il percorso dell'installazione di R ( `C:\\Program Files\\R\\R-4.0.3` ).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'C:\path\to\R-lang-extension-windows-release.zip', 
        FILE_NAME = 'libRExtension.dll',
        ENVIRONMENT_VARIABLES = N'{"R_HOME": "C:\\Program Files\\R\\R-4.0.3"}'););
    GO
    ```

    Eseguire l'istruzione per ogni database in cui si vuole usare l'estensione del linguaggio R.

    > [!NOTE]
    > **R** è una parola riservata e non può essere usata come nome per un nuovo nome di lingua esterna. Usare un altro nome. Ad esempio, l'istruzione precedente USA **myR**.
