---
description: Distribuire un progetto SSIS con codice C# in un'app .NET
title: Distribuire un progetto SSIS con codice .NET (C#) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3a5b6cb5c5c8d6afdf6bc1d59259e495ca49f10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354252"
---
# <a name="deploy-an-ssis-project-with-c-code-in-a-net-app"></a>Distribuire un progetto SSIS con codice C# in un'app .NET

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come scrivere codice C# per connettersi a un server di database e distribuire un progetto SSIS.

Per creare un'app C#, è possibile usare Visual Studio, Visual Studio Code o un altro strumento di propria scelta.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare che Visual Studio o Visual Studio Code sia installato. Scaricare l'edizione Community gratuita di Visual Studio o lo strumento gratuito Visual Studio Code dalla pagina dei [download di Visual Studio](https://www.visualstudio.com/downloads/).

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
5. Fare clic su **Mostra stringhe di connessione del database**.
6. Esaminare l'intera stringa di connessione **ADO.NET**. Facoltativamente il codice può usare `SqlConnectionStringBuilder` per ricreare questa stringa di connessione con i valori dei singoli parametri specificati.

## <a name="supported-authentication-method"></a>Metodo di autenticazione supportata

Vedere [Metodi di autenticazione per la distribuzione](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="create-a-new-visual-studio-project"></a>Creare un nuovo progetto Visual Studio

1. In Visual Studio scegliere **File**, **Nuovo**, **Progetto**. 
2. Nella finestra di dialogo **Nuovo progetto** espandere **Visual C#**.
3. Selezionare **Applicazione console** e immettere *deploy_ssis_project* come nome del progetto.
4. Fare clic su **OK** per creare e aprire il nuovo progetto in Visual Studio.

## <a name="add-references"></a>Aggiungere riferimenti
1. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **Riferimenti** e selezionare **Aggiungi riferimento**. Viene visualizzata la finestra di dialogo **Gestione riferimenti**.
2. Nella finestra di dialogo **Gestione riferimenti** espandere **Assembly** e selezionare **Estensioni**.
3. Selezionare i seguenti due riferimenti da aggiungere:
    -   Microsoft.SqlServer.Management.Sdk.Sfc
    -   Microsoft.SqlServer.Smo
4. Fare clic sul pulsante **Sfoglia** per aggiungere un riferimento a **Microsoft.SqlServer.Management.IntegrationServices**. Questo assembly viene installato nella Global Assembly Cache (GAC). Viene visualizzata la finestra di dialogo **Selezionare i file a cui fare riferimento**.
5. Nella finestra di dialogo **Selezionare i file a cui fare riferimento** passare alla cartella che contiene l'assembly nella GAC. In genere questa cartella è `C:\Windows\assembly\GAC_MSIL\Microsoft.SqlServer.Management.IntegrationServices\14.0.0.0__89845dcd8080cc91`.
6. Selezionare l'assembly, ovvero il file DLL, nella cartella e fare clic su **Aggiungi**.
7. Fare clic su **OK** per chiudere la finestra di dialogo **Gestione riferimenti** e aggiungere i tre riferimenti. Per verificare se i riferimenti sono presenti, controllare l'elenco **Riferimenti** in Esplora soluzioni.

## <a name="add-the-c-code"></a>Aggiungere il codice C# 
1. Aprire **Program.cs**.

2. Sostituire il contenuto di **Program.cs** con il codice seguente. Aggiungere i valori appropriati per server, database, utente e password.

> [!NOTE]
> L'esempio seguente usa l'autenticazione di Windows. Per usare l'autenticazione di SQL Server, sostituire l'argomento `Integrated Security=SSPI;` con `User ID=<user name>;Password=<password>;`. Se ci si connette a un server di database SQL di Azure, non è possibile usare l'autenticazione di Windows.

```csharp
using Microsoft.SqlServer.Management.IntegrationServices;
using System;
using System.Data.SqlClient;
using System.IO;

namespace deploy_ssis_project
{
    class Program
    {
        static void Main(string[] args)
        {
            // Variables
            string targetServerName = "localhost";
            string targetFolderName = "Project1Folder";
            string projectName = "Integration Services Project1";
            string projectFilePath = @"C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac";

            // Create a connection to the server
            string sqlConnectionString = "Data Source=" + targetServerName +
                ";Initial Catalog=master;Integrated Security=SSPI;";
            SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);

            // Create the Integration Services object
            IntegrationServices integrationServices = new IntegrationServices(sqlConnection);

            // Get the Integration Services catalog
            Catalog catalog = integrationServices.Catalogs["SSISDB"];

            // Create the target folder
            CatalogFolder folder = new CatalogFolder(catalog,
                targetFolderName, "Folder description");
            folder.Create();

            Console.WriteLine("Deploying " + projectName + " project.");

            byte[] projectFile = File.ReadAllBytes(projectFilePath);
            folder.DeployProject(projectName, projectFile);

            Console.WriteLine("Done.");
        }
    }
}
```

## <a name="run-the-code"></a>Eseguire il codice

1. Per eseguire l'applicazione, premere **F5**.
2. In SQL Server Management Studio verificare che il progetto sia stato distribuito.

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per distribuire un pacchetto.
    - [Distribuire un pacchetto SSIS con SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Distribuire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Distribuire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [Distribuire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-deploy-cmdline.md)
    - [Distribuire un pacchetto SSIS con PowerShell](ssis-quickstart-deploy-powershell.md)
- Eseguire un pacchetto distribuito. Per eseguire un pacchetto, è possibile scegliere tra diversi strumenti e linguaggi. Per altre informazioni, vedere gli articoli seguenti:
    - [Eseguire un pacchetto SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con PowerShell](ssis-quickstart-run-powershell.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md) 
