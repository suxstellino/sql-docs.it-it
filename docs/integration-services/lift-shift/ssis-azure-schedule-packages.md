---
title: Pianificare i pacchetti SSIS in Azure | Microsoft Docs
description: Panoramica dei metodi disponibili per la pianificazione dell'esecuzione di pacchetti SSIS distribuiti nel database SQL di Azure.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 7b7c6d785910425aa6388c23bc51b4690d7782c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345697"
---
# <a name="schedule-the-execution-of-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Pianificare l'esecuzione dei pacchetti di SQL Server Integration Services (SSIS) distribuiti in Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



È possibile pianificare l'esecuzione di pacchetti SSIS distribuiti nel catalogo SSISDB in un server di database SQL di Azure scegliendo uno dei metodi descritti in questo articolo. Un pacchetto può essere pianificato direttamente oppure indirettamente nell'ambito di una pipeline di Azure Data Factory. Per una panoramica di SSIS in Azure, vedere [Migrazione lift-and-shift dei carichi di lavoro di SQL Server Integration Services nel cloud](ssis-azure-lift-shift-ssis-packages-overview.md).

- Pianificare un pacchetto direttamente

  - [Pianificare con l'opzione di pianificazione in SQL Server Management Studio (SSMS)](#ssms)

  - [Processi elastici del database SQL](#elastic)

  - [SQL Server Agent](#agent)

- [Pianificare un pacchetto indirettamente nell'ambito di una pipeline di Azure Data Factory](#activity)


## <a name="schedule-a-package-with-ssms"></a><a name="ssms"></a> Pianificare un pacchetto con SSMS

In SQL Server Management Studio (SSMS) è possibile fare clic con il pulsante destro del mouse su un pacchetto distribuito nel database del catalogo SSIS, SSISDB, e scegliere **Pianifica** per aprire la finestra di dialogo **Nuova pianificazione**. Per altre informazioni, vedere [Pianificare pacchetti SSIS in Azure con SQL Server Management Studio](ssis-azure-schedule-packages-ssms.md).

Questa funzionalità richiede SQL Server Management Studio 17.7 o versione successiva. Per ottenere la versione più recente di SSMS, vedere [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="schedule-a-package-with-sql-database-elastic-jobs"></a><a name="elastic"></a> Pianificare un pacchetto con i processi elastici del database SQL

Per altre informazioni sui processi elastici del database SQL, vedere [Gestione dei database cloud con scalabilità orizzontale](/azure/sql-database/sql-database-elastic-jobs-overview).

### <a name="prerequisites"></a>Prerequisites

Per poter usare i processi elastici per pianificare i pacchetti SSIS archiviati nel database del catalogo SSISDB in un server di database SQL di Azure, è necessario eseguire queste operazioni:

1.  Installare e configurare i componenti dei processi di database elastico. Per altre informazioni, vedere [Installazione dei processi di database elastico (panoramica)](/azure/sql-database/sql-database-elastic-jobs-service-installation).

2. Creare le credenziali con ambito database che i processi possono usare per inviare comandi al database del catalogo SSIS. Per altre informazioni, vedere [CREARE le CREDENZIALI nell'ambito del DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

### <a name="create-an-elastic-job"></a>Creare un processo elastico

Creare il processo usando uno script Transact-SQL simile allo script riportato nell'esempio seguente:

```sql
-- Create Elastic Jobs target group
EXEC jobs.sp_add_target_group 'TargetGroup'

-- Add Elastic Jobs target group member
EXEC jobs.sp_add_target_group_member @target_group_name='TargetGroup',
    @target_type='SqlDatabase', @server_name='YourSQLDBServer.database.windows.net',
    @database_name='SSISDB' 

-- Add a job to schedule SSIS package execution
EXEC jobs.sp_add_job @job_name='ExecutePackageJob', @description='Description', 
    @schedule_interval_type='Minutes', @schedule_interval_count=60

-- Add a job step to create/start SSIS package execution using SSISDB catalog stored procedures
EXEC jobs.sp_add_jobstep @job_name='ExecutePackageJob', 
    @command=N'DECLARE @exe_id bigint 
        EXEC [SSISDB].[catalog].[create_execution]
            @folder_name=N''folderName'', @project_name=N''projectName'',
            @package_name=N''packageName'', @use32bitruntime=0,
            @runinscaleout=1, @useanyworker=1, 
            @execution_id=@exe_id OUTPUT         
        EXEC [SSISDB].[catalog].[start_execution] @exe_id, @retry_count=0', 
    @credential_name='YourDBScopedCredentials', 
    @target_group_name='TargetGroup' 

-- Enable the job schedule 
EXEC jobs.sp_update_job @job_name='ExecutePackageJob', @enabled=1, 
    @schedule_interval_type='Minutes', @schedule_interval_count=60 
```

## <a name="schedule-a-package-with-sql-server-agent-on-premises"></a><a name="agent"></a> Pianificare un pacchetto con SQL Server Agent in locale

Per altre informazioni su SQL Server Agent, vedere [Processi di SQL Server Agent per i pacchetti](../packages/sql-server-agent-jobs-for-packages.md).

### <a name="prerequisite---create-a-linked-server"></a>Prerequisito: creare un server collegato

Prima di usare SQL Server Agent in locale per pianificare l'esecuzione di pacchetti archiviati in un server di database SQL di Azure, è necessario aggiungere il server di database SQL a SQL Server locale come server collegato.

1.  **Configurare il server collegato**

    ```sql
    -- Add the SSISDB database on your Azure SQL Database as a linked server to your SQL Server on premises
    EXEC sp_addlinkedserver
        @server='myLinkedServer', -- Name your linked server
        @srvproduct='',     
        @provider='sqlncli', -- Use SQL Server native client
        @datasrc='<server_name>.database.windows.net', -- Add your Azure SQL Database server endpoint
        @location='',
        @provstr='',
        @catalog='SSISDB'  -- Add SSISDB as the initial catalog
    ```

2.  **Impostare le credenziali del server collegato**

    ```sql
    -- Add your Azure SQL Database server admin credentials
    EXEC sp_addlinkedsrvlogin
        @rmtsrvname = 'myLinkedServer',
        @useself = 'false',
        @rmtuser = 'myUsername', -- Add your server admin username
        @rmtpassword = 'myPassword' -- Add your server admin password
    ```

3.  **Impostare le opzioni del server collegato**

    ```sql
    EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;
    ```

Per altre informazioni, vedere [Creazione di server collegati](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md) e [Server collegati](../../relational-databases/linked-servers/linked-servers-database-engine.md).

### <a name="create-a-sql-server-agent-job"></a>Creare un processo di SQL Server Agent

Per pianificare un pacchetto con SQL Server Agent in locale, creare un processo con un passaggio di processo che chiami le stored procedure del catalogo SSIS `[catalog].[create_execution]` e quindi `[catalog].[start_execution]`. Per altre informazioni, vedere [Processi di SQL Server Agent per i pacchetti](../packages/sql-server-agent-jobs-for-packages.md).

1.  In SQL Server Management Studio connettersi al database di SQL Server locale in cui si vuole creare il processo.

2.  Fare clic con il pulsante destro del mouse sul nodo **SQL Server Agent**, selezionare **Nuovo** e quindi selezionare **Processo** per aprire la finestra di dialogo **Nuovo processo**.

3.  Nella finestra di dialogo **Nuovo processo** selezionare la pagina **Passaggi** e quindi selezionare **Nuovo** per aprire la finestra di dialogo **Nuovo passaggio di processo**.

4.  Nella finestra di dialogo **Nuovo passaggio di processo** selezionare `SSISDB` come **Database.**

5.  Nel campo **Comando** immettere uno script Transact-SQL simile a quello riportato nell'esempio seguente:

    ```sql
    -- T-SQL script to create and start SSIS package execution using SSISDB stored procedures
    DECLARE @return_value int, @exe_id bigint 

    EXEC @return_value = [YourLinkedServer].[SSISDB].[catalog].[create_execution] 
        @folder_name=N'folderName', @project_name=N'projectName', 
        @package_name=N'packageName', @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [YourLinkedServer].[SSISDB].[catalog].[set_execution_parameter_value] @exe_id,
        @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [YourLinkedServer].[SSISDB].[catalog].[start_execution] @execution_id=@exe_id
    ```

6.  Completare la configurazione e la pianificazione del processo.

## <a name="schedule-a-package-as-part-of-an-azure-data-factory-pipeline"></a><a name="activity"></a> Pianificare un pacchetto nell'ambito di una pipeline di Azure Data Factory

È possibile pianificare un pacchetto indirettamente tramite un trigger per l'esecuzione di una pipeline di Azure Data Factory in cui viene eseguito un pacchetto SSIS.

Per pianificare una pipeline di Data Factory, usare uno dei trigger seguenti:

- [Trigger di pianificazione](/azure/data-factory/how-to-create-schedule-trigger)

- [Trigger di finestra a cascata](/azure/data-factory/how-to-create-tumbling-window-trigger)

- [Trigger basato su eventi](/azure/data-factory/how-to-create-event-trigger)

Per eseguire un pacchetto SSIS nell'ambito di una pipeline di Data Factory, usare una delle attività seguenti:

- [Attività Esegui pacchetto SSIS](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

- [Attività stored procedure](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Passaggi successivi

Esaminare le opzioni per l'esecuzione dei pacchetti SSIS distribuiti in Azure. Per altre informazioni, vedere [Eseguire pacchetti SSIS in Azure](ssis-azure-run-packages.md).