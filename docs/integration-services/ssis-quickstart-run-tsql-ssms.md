---
description: Eseguire un pacchetto SSIS da SSMS con Transact-SQL
title: Eseguire un pacchetto SSIS con Transact-SQL (SSMS) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b22c31b7e38936adec2ac7355912e2899024659c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "92197018"
---
# <a name="run-an-ssis-package-from-ssms-with-transact-sql"></a>Eseguire un pacchetto SSIS da SSMS con Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come usare SQL Server Management Studio (SSMS) per connettersi al database del catalogo SSIS e quindi usare istruzioni Transact-SQL per eseguire un pacchetto SSIS archiviato nel catalogo SSIS.

SQL Server Management Studio è un ambiente integrato per la gestione di qualsiasi infrastruttura SQL, da SQL Server al database SQL. Per altre informazioni su SSMS, vedere [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare di avere l'ultima versione di SQL Server Management Studio (SSMS). Per scaricare SSMS, vedere [Scaricare SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

Un server di Database SQL di Azure è in ascolto sulla porta 1433. Se si sta provando a connettersi a un server di database SQL di Azure dall'interno di un firewall aziendale, per stabilire correttamente la connessione questa porta deve essere aperta nel firewall aziendale.

## <a name="supported-platforms"></a>Piattaforme supportate

È possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS nelle piattaforme seguenti:

-   SQL Server in Windows.

-   Database SQL di Azure. Per altre informazioni sulla distribuzione e l'esecuzione di pacchetti in Azure, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Non è possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS in Linux. Per altre informazioni sull'esecuzione di pacchetti in Linux, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Ottenere le informazioni di connessione per il database SQL di Azure

Per eseguire il pacchetto nel database SQL di Azure, ottenere le informazioni di connessione necessarie per connettersi al database del catalogo SSIS (SSISDB). Nelle procedure che seguono sono necessari il nome completo del server e le informazioni di accesso.

1. Accedere al [Portale di Azure](https://portal.azure.com/).
2. Selezionare **Database SQL** nel menu a sinistra e quindi il database SSISDB nella pagina **Database SQL**. 
3. Nella pagina **Panoramica** del database controllare il nome completo del server. Passare il mouse sul nome del server per visualizzare l'opzione **Fare clic per copiare**. 
4. Se si dimenticano le informazioni di accesso del server di database SQL di Azure, passare alla pagina del server di database SQL per visualizzare il nome amministratore del server. Se necessario, è possibile reimpostare la password.

## <a name="connect-to-the-ssisdb-database"></a>Connettersi al database SSISDB

Usare SQL Server Management Studio per stabilire una connessione al catalogo SSIS nel server di database SQL di Azure. 

1. Aprire SQL Server Management Studio.

2. Immettere le informazioni seguenti nella finestra di dialogo **Connetti al server**:

   | Impostazione       | Valore consigliato | Altre informazioni | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Tipo di server** | Motore di database | Questo valore è obbligatorio. |
   | **Nome server** | Nome completo del server | Se si sta eseguendo la connessione a un server di database SQL di Azure, il nome è nel formato `<server_name>.database.windows.net`. |
   | **autenticazione** | Autenticazione di SQL Server | Con l'autenticazione di SQL Server è possibile connettersi a SQL Server o al database SQL di Azure. Se ci si connette a un server di database SQL di Azure, non è possibile usare l'autenticazione di Windows. |
   | **Accesso** | Account amministratore del server | Account specificato al momento della creazione del server. |
   | **Password** | Password per l'account amministratore del server | Password specificata al momento della creazione del server. |

3.  Fare clic su **Connetti**. In SSMS si apre la finestra Esplora oggetti.

4. In Esplora oggetti espandere **Cataloghi di Integration Services** e quindi espandere **SSISDB** per visualizzare gli oggetti nel database del catalogo SSIS.

## <a name="run-a-package"></a>Eseguire un pacchetto
Eseguire il codice Transact-SQL seguente per eseguire un pacchetto SSIS.

1.  In SSMS aprire una nuova finestra di query e incollare il codice seguente. Questo è il codice generato dall'opzione **Script** nella finestra di dialogo **Esegui pacchetto** di SSMS.

2.  Aggiornare i valori dei parametri nella stored procedure `catalog.create_execution` in base al sistema in uso.

3.  Verificare che SSISDB sia il database corrente.

4.  Eseguire lo script.

5. In Esplora oggetti aggiornare il contenuto di **SSISDB** se necessario e verificare la presenza del progetto distribuito.

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per eseguire un pacchetto.
    - [Eseguire un pacchetto SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con PowerShell](ssis-quickstart-run-powershell.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md)