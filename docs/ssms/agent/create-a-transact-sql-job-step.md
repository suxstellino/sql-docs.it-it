---
description: Create a Transact-SQL Job Step
title: Create a Transact-SQL Job Step
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL job step
- job steps [Transact-SQL]
- SQL Server Agent jobs, Transact-SQL step
ms.assetid: 69c571a7-debe-4063-9d38-e4b6a1e8e84c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: f21241af1666ec4aa5ea497515838e04e98b692c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236736"
---
# <a name="create-a-transact-sql-job-step"></a>Create a Transact-SQL Job Step
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come creare un passaggio di processo di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che esegue script [!INCLUDE[tsql](../../includes/tsql-md.md)] in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] o SQL Server Management Objects.  
  
Gli script per passaggi di processo possono chiamare stored procedure e stored procedure estese. Un singolo passaggio di processo [!INCLUDE[tsql](../../includes/tsql-md.md)] può contenere più batch e comandi GO incorporati. Per ulteriori informazioni sulla creazione di un processo, vedere [Creazione di processi](../../ssms/agent/create-jobs.md).  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-transact-sql-job-step"></a>Per creare un passaggio di processo Transact-SQL  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.  
  
2.  Espandere **SQL Server Agent**, creare un nuovo processo oppure fare clic con il pulsante destro del mouse su un processo esistente e quindi scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà processo** fare clic sulla pagina **Passaggi** e quindi su **Nuovo**.  
  
4.  Nella finestra di dialogo **Nuovo passaggio di processo** digitare il nome del passaggio del processo nella casella **Nome passaggio**.  
  
5.  Nell'elenco **Tipo** fare clic su **Transact-SQL Script (TSQL)**.  
  
6.  Nella casella **Comando** digitare i batch di comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] oppure fare clic su **Apri** per selezionare un file [!INCLUDE[tsql](../../includes/tsql-md.md)] da utilizzare come comando.  
  
7.  Fare clic su **Analizza** per controllare la sintassi.  
  
8.  Se la sintassi è corretta, viene visualizzato un messaggio che informa che l'analisi è stata completata. Se viene rilevato un errore, correggere la sintassi prima di continuare.  
  
9. Fare clic sulla pagina **Avanzate** per impostare le opzioni del passaggio di processo, ad esempio l'azione che verrà eseguita se il passaggio di processo ha esito positivo o negativo, il numero di tentativi di esecuzione del passaggio che verranno eseguiti da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e il file o la tabella in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent scriverà l'output del passaggio di processo. L'output del passaggio di processo può essere scritto in un file di sistema unicamente dai membri del ruolo predefinito del server **sysadmin** . Gli utenti di SQL Server Agent possono registrare l'output in una tabella.  
  
10. Se l'utente è membro del ruolo predefinito del server **sysadmin** e intende eseguire questo passaggio di processo con un diverso account di accesso SQL, selezionare l'account di accesso SQL dall'elenco **Esegui come utente** .  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-create-a-transact-sql-job-step"></a>Per creare un passaggio di processo Transact-SQL  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- creates a job step that uses Transact-SQL  
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
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per creare un passaggio di processo Transact-SQL**  
  
Usare la classe **JobStep** tramite un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell.  
