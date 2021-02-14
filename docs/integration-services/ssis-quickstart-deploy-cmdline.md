---
description: Distribuire un progetto SSIS dal prompt dei comandi con ISDeploymentWizard.exe
title: Distribuire un progetto SSIS dal prompt dei comandi| Microsoft Docs
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb78c1ab660ff3ac0cccfdc36779b659ba654474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354265"
---
# <a name="deploy-an-ssis-project-from-the-command-prompt-with-isdeploymentwizardexe"></a>Distribuire un progetto SSIS dal prompt dei comandi con ISDeploymentWizard.exe

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come distribuire un progetto SSIS al prompt dei comandi eseguendo la Distribuzione guidata Integration Services, `ISDeploymentWizard.exe`.

Per altre informazioni sulla procedura, vedere [Distribuzione guidata Integration Services](packages/deploy-integration-services-ssis-projects-and-packages.md#integration-services-deployment-wizard).

## <a name="prerequisites"></a>Prerequisiti

La convalida descritta in questo articolo per la distribuzione al database SQL di Azure richiede SQL Server Data Tools (SSDT) 17.4 o versione successiva. Per scaricare la versione più recente di SSDT, vedere [Scaricare la versione più recente di SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).

Un server di Database SQL di Azure è in ascolto sulla porta 1433. Se si sta provando a connettersi a un server di database SQL di Azure dall'interno di un firewall aziendale, per stabilire correttamente la connessione questa porta deve essere aperta nel firewall aziendale.

## <a name="supported-platforms"></a>Piattaforme supportate

È possibile usare le informazioni di questa guida introduttiva per distribuire un progetto SSIS nelle piattaforme seguenti:

-   SQL Server in Windows.

-   Database SQL di Azure. Per altre informazioni sulla distribuzione e l'esecuzione di pacchetti in Azure, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Non è possibile usare le informazioni di questa guida introduttiva per distribuire un pacchetto SSIS a SQL Server in Linux. Per altre informazioni sull'esecuzione di pacchetti in Linux, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Ottenere le informazioni di connessione per il database SQL di Azure

Per distribuire il progetto nel database SQL di Azure, ottenere le informazioni di connessione necessarie per connettersi al database del catalogo SSIS (SSISDB). Nelle procedure che seguono sono necessari il nome completo del server e le informazioni di accesso.

1. Accedere al [Portale di Azure](https://portal.azure.com/).
2. Selezionare **Database SQL** nel menu a sinistra e quindi il database SSISDB nella pagina **Database SQL**. 
3. Nella pagina **Panoramica** del database controllare il nome completo del server. Passare il mouse sul nome del server per visualizzare l'opzione **Fare clic per copiare**. 
4. Se si dimenticano le informazioni di accesso del server di database SQL di Azure, passare alla pagina del server di database SQL per visualizzare il nome amministratore del server. Se necessario, è possibile reimpostare la password.

## <a name="supported-authentication-method"></a>Metodo di autenticazione supportata

Vedere [Metodi di autenticazione per la distribuzione](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="start-the-integration-services-deployment-wizard"></a>Avviare la Distribuzione guidata Integration Services
1. Aprire una finestra del prompt dei comandi.

2. Eseguire `ISDeploymentWizard.exe`. Si apre la Distribuzione guidata Integration Services.

    Se la cartella che contiene `ISDeploymentWizard.exe` non si trova nella variabile di ambiente `path`, può essere necessario usare il comando `cd` per passare alla directory corrispondente. Per SQL Server 2017 questa cartella è in genere `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn`.

## <a name="deploy-a-project-with-the-wizard"></a>Distribuire un progetto con la procedura guidata
1. Nella pagina **Introduzione** della procedura guidata leggere l'introduzione. Fare clic su **Avanti** per aprire la pagina **Seleziona origine**.

2. Nella pagina **Seleziona origine** selezionare il progetto SSIS esistente da distribuire.
    -   Per distribuire un file di distribuzione del progetto creato impostando un progetto nell'ambiente di sviluppo, selezionare **File di distribuzione progetto** e immettere il percorso del file con estensione ispac.
    -   Per distribuire un progetto che è già distribuito in un database del catalogo SSIS, selezionare **Catalogo di Integration Services**, quindi immettere il nome del server e il percorso del progetto nel catalogo.
    Fare clic su **Avanti** per visualizzare la pagina **Seleziona destinazione** .
  
3.  Nella pagina **Seleziona destinazione** selezionare la destinazione per il progetto.
    -   Immettere il nome completo del server. Se il server di destinazione è un server del database SQL di Azure, il nome è nel formato `<server_name>.database.windows.net`.
    -   Specificare le informazioni di autenticazione e selezionare **Connetti**. Vedere i [metodi di autenticazione per la distribuzione](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment) in questo articolo.
    -   Selezionare **Sfoglia** per selezionare la cartella di destinazione in SSISDB.
    -   In seguito selezionare **Successivo** per aprire la pagina **Verifica**. Il pulsante **Successivo** viene abilitato solo dopo che è stato selezionato **Connetti**.

4.  Nella pagina **Verifica** rivedere le impostazioni selezionate.
    -   È possibile modificare le selezioni facendo clic su **Indietro** o selezionando un qualsiasi passaggio nel riquadro sinistro.
    -   Fare clic su **Distribuisci** per avviare il processo di distribuzione.

5.  Se si esegue la distribuzione a un server di database SQL di Azure viene visualizzata la pagina **Convalida**, in cui si verifica che i pacchetti del progetto non contengano problemi noti, che potrebbero impedire la corretta esecuzione del runtime di integrazione Azure-SSIS. Per altre informazioni, vedere [Convalidare pacchetti SSIS distribuiti in Azure](lift-shift/ssis-azure-validate-packages.md).

6.  Al termine del processo di distribuzione viene visualizzata la pagina **Risultati**. Questa pagina consente di visualizzare l'esito positivo o negativo di ogni azione.
    -   Se l'azione ha avuto esito negativo, fare clic su **Non riuscito** nella colonna **Risultato** per visualizzare una spiegazione dell'errore.
    -   In alternativa, fare clic su **Salva report...** per salvare i risultati in un file XML.
    -   Per uscire dalla procedura guidata, fare clic su **Chiudi**.

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per distribuire un pacchetto.
    - [Distribuire un pacchetto SSIS con SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Distribuire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Distribuire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [Distribuire un pacchetto SSIS con PowerShell](ssis-quickstart-deploy-powershell.md)
    - [Distribuire un pacchetto SSIS con C#](./ssis-quickstart-deploy-dotnet.md) 
- Eseguire un pacchetto distribuito. Per eseguire un pacchetto, è possibile scegliere tra diversi strumenti e linguaggi. Per altre informazioni, vedere gli articoli seguenti:
    - [Eseguire un pacchetto SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con PowerShell](ssis-quickstart-run-powershell.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md) 
