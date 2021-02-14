---
description: Eseguire un pacchetto SSIS con PowerShell
title: Eseguire un pacchetto SSIS con PowerShell | Microsoft Docs
ms.date: 09/17/2020
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0f82c5746f456fe03b3fdca21b0297fbcc63d0d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348216"
---
# <a name="run-an-ssis-package-with-powershell"></a>Eseguire un pacchetto SSIS con PowerShell

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come usare uno script di PowerShell per connettersi a un server di database ed eseguire un pacchetto SSIS.

## <a name="prerequisites"></a>Prerequisiti

Un server di Database SQL di Azure è in ascolto sulla porta 1433. Se si sta provando a connettersi a un server di database SQL di Azure dall'interno di un firewall aziendale, per stabilire correttamente la connessione questa porta deve essere aperta nel firewall aziendale.

## <a name="supported-platforms"></a>Piattaforme supportate

È possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS nelle piattaforme seguenti:

-   SQL Server in Windows.

-   Runtime di integrazione SSIS (IR) in Azure Data Factory (ADF), in cui il catalogo SSIS (SSISDB) è ospitato da Istanza gestita di SQL di Azure. Per altre informazioni sulla distribuzione e l'esecuzione di pacchetti in Azure, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Non è possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS in Linux. Per altre informazioni sull'esecuzione di pacchetti in Linux, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Ottenere le informazioni di connessione per il database SQL di Azure

Per eseguire il pacchetto nel database SQL di Azure, ottenere le informazioni di connessione necessarie per connettersi al database del catalogo SSIS (SSISDB). Nelle procedure che seguono sono necessari il nome completo del server e le informazioni di accesso.

1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Selezionare **Database SQL** nel menu a sinistra e quindi il database SSISDB nella pagina **Database SQL**. 
3. Nella pagina **Panoramica** del database controllare il nome completo del server. Passare il mouse sul nome del server per visualizzare l'opzione **Fare clic per copiare**. 
4. Se si dimenticano le informazioni di accesso del server di database SQL di Azure, passare alla pagina del server di database SQL per visualizzare il nome amministratore del server. Se necessario, è possibile reimpostare la password.
5. Fare clic su **Mostra stringhe di connessione del database**.
6. Esaminare l'intera stringa di connessione **ADO.NET**.

## <a name="ssis-powershell-provider"></a>Provider PowerShell per SSIS
È possibile usare il provider PowerShell per SSIS per connettersi a un catalogo SSIS ed eseguire pacchetti in esso presenti.

Ecco un esempio di base su come eseguire un pacchetto SSIS in un catalogo pacchetti con il provider PowerShell per SSIS.

```powershell
(Get-ChildItem SQLSERVER:\SSIS\localhost\Default\Catalogs\SSISDB\Folders\Project1Folder\Projects\'Integration Services Project1'\Packages\ |
WHERE { $_.Name -eq 'Package.dtsx' }).Execute("false", $null)
```

## <a name="powershell-script"></a>Script PowerShell
Specificare i valori appropriati per le variabili nella parte superiore dello script seguente e quindi eseguire lo script per eseguire il pacchetto SSIS.

> [!NOTE]
> L'esempio seguente usa l'autenticazione di Windows. Per usare l'autenticazione di SQL Server, sostituire l'argomento `Integrated Security=SSPI;` con `User ID=<user name>;Password=<password>;`. Se ci si connette a un server di database SQL di Azure, non è possibile usare l'autenticazione di Windows. 

```powershell
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectName = "Integration Services Project1"
$PackageName = "Package.dtsx"

# Load the IntegrationServices assembly
$loadStatus = [System.Reflection.Assembly]::Load("Microsoft.SQLServer.Management.IntegrationServices, "+
    "Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91, processorArchitecture=MSIL")

# Create a connection to the server
$sqlConnectionString = `
    "Data Source=" + $TargetServerName + ";Initial Catalog=master;Integrated Security=SSPI;"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $SSISNamespace".IntegrationServices" $sqlConnection

# Get the Integration Services catalog
$catalog = $integrationServices.Catalogs["SSISDB"]

# Get the folder
$folder = $catalog.Folders[$TargetFolderName]

# Get the project
$project = $folder.Projects[$ProjectName]

# Get the package
$package = $project.Packages[$PackageName]

Write-Host "Running " $PackageName "..."

$result = $package.Execute("false", $null)

Write-Host "Done."
```

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per eseguire un pacchetto.
    - [Eseguire un pacchetto SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md) 
