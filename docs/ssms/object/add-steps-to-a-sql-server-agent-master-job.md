---
description: Add Steps to a SQL Server Agent Master Job
title: Add Steps to a SQL Server Agent Master Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 9cc1e8ab-7ddc-427b-859e-203aa7e24642
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 92fe948e73a7d8439ca107fe9ed086f5b7fcbff5
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250529"
---
# <a name="add-steps-to-a-sql-server-agent-master-job"></a>Add Steps to a SQL Server Agent Master Job
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In questo argomento verrà descritto come aggiungere passaggi a un processo master di SQL Server Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
**Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
    [Limitazioni e restrizioni](#Restrictions)  
  
    [Sicurezza](#Security)  
  
-   **Per aggiungere passaggi a un processo master di SQL Server Agent tramite:**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
Un processo master di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non può essere destinato sia a server locali sia a server remoti.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
È possibile modificare solo i processi di cui si è proprietari, a meno che non si appartenga al ruolo predefinito del server **sysadmin** . Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-add-steps-to-a-sql-server-agent-master-job"></a>Per aggiungere passaggi a un processo master di SQL Server Agent  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server contenente il processo a cui si desidera aggiungere passaggi.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic sul segno più per espandere la cartella **Processi** .  
  
4.  Fare clic con il pulsante destro del mouse sul processo a cui si intende aggiungere passaggi e scegliere **Proprietà**.  
  
5.  Nella finestra di dialogo **Proprietà processo -** _nome_processo_ selezionare **Passaggi** in **Selezione pagina**. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - nuovo processo &#40;pagina passaggi&#41;](../../ssms/agent/job-properties-new-job-steps-page.md).  
 
6.  Al termine, fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-add-steps-to-a-sql-server-agent-master-job"></a>Per aggiungere passaggi a un processo master di SQL Server Agent  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- creates a job step that changes database access to read-only for the Sales database.   
    -- specifies 5 retry attempts, with each retry to occur after a 5 minute wait.   
    -- assumes that the Weekly Sales Data Backup job already exists  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Per altre informazioni, vedere [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
