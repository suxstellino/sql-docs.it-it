---
description: Creare un processo
title: Creare un processo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: b35af2b6-6594-40d1-9861-4d5dd906048c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2c577bb79487db6047c173ac331ed27fdf7ba3b2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237435"
---
# <a name="create-a-job"></a>Creare un processo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene descritto come creare un processo di SQL Server Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects (SMO).  
  
Per aggiungere al processo passaggi, pianificazioni, avvisi e notifiche da inviare agli operatori, vedere i collegamenti agli argomenti nella sezione Vedere anche.  
  
-   **Prima di iniziare:**  
  
    [Limitazioni e restrizioni](#Restrictions)  
  
    [Sicurezza](#Security)  
  
-   **Per creare un processo tramite:**  
  
    [SQL Server Management Studio](#SSMSProcedure),  
  
    [Transact-SQL](#TsqlProcedure)  
  
    [SQL Server Management Objects](#SMOProcedure)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitazioni e restrizioni  
  
-   Per creare un processo, è necessario che l'utente sia membro di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent o del ruolo predefinito del server **sysadmin** . Un processo può essere modificato solo dal proprietario o dai membri del ruolo **sysadmin** . Per altre informazioni sui ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
-   L'assegnazione di un processo a un altro account di accesso non garantisce che il nuovo proprietario disponga di autorizzazioni sufficienti per eseguire correttamente il processo.  
  
-   I processi locali vengono memorizzati nella cache dall'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Eventuali modifiche, pertanto, forzano in modo implicito una nuova memorizzazione nella cache da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Poiché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non memorizza nella cache il processo fino alla chiamata di **sp_add_jobserver** , è consigliabile chiamare la store procedure **sp_add_jobserver** per ultima.  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
-   Solo un amministratore di sistema può cambiare il proprietario di un processo.  
  
-   Per motivi di sicurezza, solo il proprietario del processo o un membro del ruolo **sysadmin** può modificare la definizione del processo. Solo i membri del ruolo predefinito del server **sysadmin** possono assegnare la proprietà di un processo ad altri utenti ed eseguire qualsiasi processo, indipendentemente dal proprietario.  
  
    > [!NOTE]  
    > Se si assegna la proprietà di un processo a un utente che non è membro del ruolo predefinito del server **sysadmin** e il processo sta eseguendo operazioni per le quali sono necessari account proxy, ad esempio l'esecuzione del pacchetto [!INCLUDE[ssIS](../../includes/ssis_md.md)] , verificare che l'utente possa accedere all'account proxy. In caso contrario, verrà generato un errore.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-job"></a>Per creare un processo di SQL Server Agent  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server in cui si desidera creare un processo di SQL Server Agent.  
  
2.  Fare clic sul segno più per espandere **SQL Server Agent**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Processi** e quindi selezionare **Nuovo processo**.  
  
4.  Nella pagina **Generale** della finestra di dialogo **Nuove processo** modificare le proprietà generali del processo. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - nuovo processo &#40;pagina Generale&#41;](../../ssms/agent/job-properties-new-job-general-page.md)  
  
5.  Nella pagina **Passaggi** , organizzare i passaggi del processo. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - nuovo processo &#40;pagina Passaggi&#41;](../../ssms/agent/job-properties-new-job-steps-page.md)  
  
6.  Nella pagina **Pianificazioni** , organizzare le pianificazioni per il processo. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - Nuovo processo &#40;pagina Pianificazioni&#41;](../../ssms/agent/job-properties-new-job-schedules-page.md)  
  
7.  Nella pagina **Avvisi** , organizzare gli avvisi per il processo. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - nuovo processo &#40;pagina Avvisi&#41;](../../ssms/agent/job-properties-new-job-alerts-page.md)  
  
8.  Nella pagina **Notifiche** impostare le azioni che [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent deve eseguire al completamento del processo. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - nuovo processo &#40;pagina Notifiche&#41;](../../ssms/agent/job-properties-new-job-notifications-page.md).  
  
9. Nella pagina **Destinazioni** , gestire i server di destinazione per il processo. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Proprietà processo - nuovo processo &#40;pagina Destinazioni&#41;](../../ssms/agent/job-properties-new-job-targets-page.md).  
  
10. Al termine, fare clic su **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-create-a-sql-server-agent-job"></a>Per creare un processo di SQL Server Agent  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backup';  
    GO  
    ```  
  
Per altre informazioni, vedere:  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMOProcedure"></a>Utilizzo di SQL Server Management Objects  
**Per creare un processo di SQL Server Agent**  
  
Chiamare il metodo **Create** della classe **Job** usando un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell. Per un codice di esempio, vedere [Pianificazione delle attività amministrative automatiche in SQL Server Agent](../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md).  
