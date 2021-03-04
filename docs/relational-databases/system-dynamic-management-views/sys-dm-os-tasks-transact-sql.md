---
description: sys.dm_os_tasks (Transact-SQL)
title: sys.dm_os_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_tasks
- sys.dm_os_tasks_TSQL
- dm_os_tasks_TSQL
- dm_os_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_tasks dynamic management view
ms.assetid: 180a3c41-e71b-4670-819d-85ea7ef98bac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b5634bc0d041e3690c0786bc35bedae1a6a5e9a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839364"
---
# <a name="sysdm_os_tasks-transact-sql"></a>sys.dm_os_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni attività in corso nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un'attività è l'unità di base di esecuzione in SQL Server. Esempi di attività includono una query, un account di accesso, una disconnessione e attività di sistema come attività di pulizia fantasma, attività Checkpoint, writer di log, attività di rollforward parallela. Per ulteriori informazioni sulle attività, vedere la [Guida all'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md).
  
> [!NOTE]  
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_tasks**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary (8)**|Indirizzo di memoria dell'oggetto.|  
|**task_state**|**nvarchar(60)**|Stato dell'attività. I possibili valori sono i seguenti:<br /><br /> PENDING: in attesa di un thread di lavoro.<br /><br /> RUNNABLE: eseguibile, ma in attesa di ricevere un quantum.<br /><br /> RUNNING: esecuzione in corso nell'utilità di pianificazione.<br /><br /> SUSPENDED: è disponibile un thread di lavoro, ma è in attesa di un evento.<br /><br /> DONE: completato.<br /><br /> SPINLOOP: bloccato in uno spinlock.|  
|**context_switches_count**|**int**|Numero di cambi di contesto dell'utilità di pianificazione completati dall'attività.|  
|**pending_io_count**|**int**|Numero di I/O fisici eseguiti dall'attività.|  
|**pending_io_byte_count**|**bigint**|Numero totale di byte degli I/O eseguiti dall'attività.|  
|**pending_io_byte_average**|**int**|Numero medio di byte degli I/O eseguiti dall'attività.|  
|**scheduler_id**|**int**|ID dell'utilità di pianificazione padre. Si tratta di un handle per le informazioni dell'utilità di pianificazione per l'attività. Per ulteriori informazioni, vedere [sys.dm_os_schedulers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|**session_id**|**smallint**|ID della sessione associata all'attività.|  
|**exec_context_id**|**int**|ID del contesto di esecuzione associato all'attività.|  
|**request_id**|**int**|ID della richiesta dell'attività. Per ulteriori informazioni, vedere [sys.dm_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|**worker_address**|**varbinary (8)**|Indirizzo di memoria del thread di lavoro che sta eseguendo l'attività.<br /><br /> NULL = Per poter essere eseguita, l'attività è in attesa di un thread di lavoro oppure l'esecuzione dell'attività è stata completata.<br /><br /> Per ulteriori informazioni, vedere [sys.dm_os_workers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|**host_address**|**varbinary (8)**|Indirizzo di memoria dell'host.<br /><br /> 0 = Per creare l'attività non è stato utilizzato alcun host. Ciò semplifica l'identificazione dell'host utilizzato per creare l'attività.<br /><br /> Per ulteriori informazioni, vedere [sys.dm_os_hosts &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md).|  
|**parent_task_address**|**varbinary (8)**|Indirizzo di memoria dell'attività padre dell'oggetto.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="examples"></a>Esempi  
  
### <a name="a-monitoring-parallel-requests"></a>R. Monitoraggio di richieste in parallelo  
 Per le richieste eseguite in parallelo, vengono visualizzate più righe per la stessa combinazione di ( \<**session_id**> , \<**request_id**> ). Usare la query seguente per trovare l' [opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) per tutte le richieste attive.  
  
> [!NOTE]  
>  Una **request_id** è univoca all'interno di una sessione.  
  
```sql  
SELECT  
    task_address,  
    task_state,  
    context_switches_count,  
    pending_io_count,  
    pending_io_byte_count,  
    pending_io_byte_average,  
    scheduler_id,  
    session_id,  
    exec_context_id,  
    request_id,  
    worker_address,  
    host_address  
  FROM sys.dm_os_tasks  
  ORDER BY session_id, request_id;  
```  
  
### <a name="b-associating-session-ids-with-windows-threads"></a>B. Associazione di ID di sessione con thread di Windows  
 È possibile utilizzare la query seguente per associare un valore di ID di sessione a un ID di thread di Windows. È possibile monitorare le prestazioni del thread utilizzando Performance Monitor di Windows. La query seguente non restituisce informazioni per le sessioni sospese.  
  
```sql  
SELECT STasks.session_id, SThreads.os_thread_id  
  FROM sys.dm_os_tasks AS STasks  
  INNER JOIN sys.dm_os_threads AS SThreads  
    ON STasks.worker_address = SThreads.worker_address  
  WHERE STasks.session_id IS NOT NULL  
  ORDER BY STasks.session_id;  
GO  
```  

## <a name="see-also"></a>Vedere anche  
[SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)    
[Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md)     
