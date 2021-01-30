---
description: sys.dm_os_workers (Transact-SQL)
title: sys.dm_os_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_workers_TSQL
- sys.dm_os_workers_TSQL
- dm_os_workers
- sys.dm_os_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_workers dynamic management view
ms.assetid: 4d5d1e52-a574-4bdd-87ae-b932527235e8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3d13344aef1c0a85b9893a9a36f0222253468b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99144782"
---
# <a name="sysdm_os_workers-transact-sql"></a>sys.dm_os_workers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni thread di lavoro nel sistema. Per ulteriori informazioni sui ruoli di lavoro, vedere la [Guida all'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md). 
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_workers**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|worker_address|**varbinary (8)**|Indirizzo di memoria del thread di lavoro.|  
|status|**int**|Solo per uso interno.|  
|is_preemptive|**bit**|1 = Il thread di lavoro è in esecuzione nell'ambito di una pianificazione preemptive. Qualsiasi thread di lavoro che esegue codice esterno viene eseguito nell'ambito di una pianificazione preemptive.|  
|is_fiber|**bit**|1 = Il thread di lavoro è in esecuzione nell'ambito del lightweight pooling. Per altre informazioni, vedere [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).|  
|is_sick|**bit**|1 = Il thread di lavoro è bloccato nel tentativo di ottenere uno spinlock. Se questo bit viene impostato, ciò potrebbe indicare un problema a livello di contesa in un oggetto a cui si accede di frequente.|  
|is_in_cc_exception|**bit**|1 = Il thread di lavoro sta gestendo un'eccezione non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_fatal_exception|**bit**|Specifica se il thread di lavoro ha ricevuto un'eccezione irreversibile.|  
|is_inside_catch|**bit**|1 = Il thread di lavoro sta gestendo un'eccezione.|  
|is_in_polling_io_completion_routine|**bit**|1 = Il thread di lavoro sta eseguendo una routine di completamento dell'I/O per un I/O in sospeso. Per ulteriori informazioni, vedere [sys.dm_io_pending_io_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-pending-io-requests-transact-sql.md).|  
|context_switch_count|**int**|Numero di cambi di contesto dell'utilità di pianificazione eseguiti dal thread di lavoro.|  
|pending_io_count|**int**|Numero di I/O fisici eseguiti dal thread di lavoro.|  
|pending_io_byte_count|**bigint**|Numero totale di byte per tutti gli I/O fisici in sospeso per il thread di lavoro.|  
|pending_io_byte_average|**int**|Numero medio di byte per gli I/O fisici per il thread di lavoro.|  
|wait_started_ms_ticks|**bigint**|Temporizzazione, in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quando questo thread di lavoro entra nello stato Suspended. La sottrazione di questo valore da ms_ticks in [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) restituisce il numero di millisecondi in cui il thread di lavoro è rimasto in attesa.|  
|wait_resumed_ms_ticks|**bigint**|Temporizzazione, in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quando il processo di lavoro è passato allo stato eseguibile. Se si sottrae questo valore da ms_ticks in [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) viene restituito il numero di millisecondi per cui il thread di lavoro è rimasto nella coda eseguibile.|  
|task_bound_ms_ticks|**bigint**|Temporizzazione, in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quando un'attività è associata a questo thread di lavoro.|  
|worker_created_ms_ticks|**bigint**|Temporizzazione, in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quando viene creato un thread di lavoro.|  
|exception_num|**int**|Numero di errore dell'ultima eccezione rilevata dal thread di lavoro.|  
|exception_severity|**int**|Gravità dell'ultima eccezione rilevata dal thread di lavoro.|  
|exception_address|**varbinary (8)**|Indirizzo del codice che ha generato l'eccezione|  
|affinity|**bigint**|L'affinità del thread di lavoro. Corrisponde all'affinità del thread in [sys.dm_os_threads &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|state|**nvarchar(60)**|Stato del thread di lavoro. I possibili valori sono i seguenti:<br /><br /> INIT = Il thread di lavoro è in fase di inizializzazione.<br /><br /> RUNNING = Il thread di lavoro è in esecuzione in modalità non preemptive o preemptive.<br /><br /> RUNNABLE = Il thread di lavoro è pronto per essere eseguito nell'utilità di pianificazione.<br /><br /> SUSPENDED = Il thread di lavoro è sospeso ed è in attesa di un evento per inviare un segnale.|  
|start_quantum|**bigint**|Tempo, espresso in millisecondi, all'inizio dell'esecuzione del thread di lavoro.|  
|end_quantum|**bigint**|Tempo, espresso in millisecondi, alla fine dell'esecuzione del thread di lavoro.|  
|last_wait_type|**nvarchar(60)**|Tipo dell'ultima attesa. Per un elenco dei tipi di attesa, vedere [sys.dm_os_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|return_code|**int**|Valore restituito dall'ultima attesa. I possibili valori sono i seguenti:<br /><br /> 0 =SUCCESS<br /><br /> 3 = DEADLOCK<br /><br /> 4 = PREMATURE_WAKEUP<br /><br /> 258 = TIMEOUT|  
|quantum_used|**bigint**|Solo per uso interno.|  
|max_quantum|**bigint**|Solo per uso interno.|  
|boost_count|**int**|Solo per uso interno.|  
|tasks_processed_count|**int**|Numero di attività elaborate dal thread di lavoro.|  
|fiber_address|**varbinary (8)**|Indirizzo di memoria del fiber a cui il thread di lavoro è associato.<br /><br /> NULL = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è configurato per il lightweight pooling.|  
|task_address|**varbinary (8)**|Indirizzo di memoria dell'attività corrente. Per ulteriori informazioni, vedere [sys.dm_os_tasks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|memory_object_address|**varbinary (8)**|Indirizzo di memoria dell'oggetto memoria del thread di lavoro. Per ulteriori informazioni, vedere [sys.dm_os_memory_objects &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|thread_address|**varbinary (8)**|Indirizzo di memoria del thread associato al thread di lavoro corrente. Per ulteriori informazioni, vedere [sys.dm_os_threads &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|signal_worker_address|**varbinary (8)**|Indirizzo di memoria del thread di lavoro che ha segnalato per ultimo l'oggetto. Per ulteriori informazioni, vedere [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|scheduler_address|**varbinary (8)**|Indirizzo di memoria dell'utilità di pianificazione. Per ulteriori informazioni, vedere [sys.dm_os_schedulers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|processor_group|**smallint**|Archivia l'ID del gruppo di processori assegnato a questo thread.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="remarks"></a>Commenti  
 Se lo stato del thread di lavoro è RUNNING e il thread di lavoro è in esecuzione in modalità non preemptive, l'indirizzo del thread di lavoro corrisponde alla colonna active_worker_address in sys.dm_os_schedulers.  
  
 Quando viene segnalato un thread di lavoro in attesa di un evento, tale thread viene inserito all'inizio della coda eseguibile. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente l'esecuzione di questa operazione mille volte in una riga. Superato questo valore, il thread di lavoro viene inserito alla fine della coda. Lo spostamento di un thread di lavoro alla fine della coda è caratterizzato da alcune implicazioni a livello di prestazioni.  
  
## <a name="permissions"></a>Autorizzazioni
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Nei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] livelli Premium, richiede l' `VIEW DATABASE STATE` autorizzazione nel database. Nei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] livelli standard e Basic, richiede l'  `Server Admin` appartenenza al ruolo o un `Azure Active Directory admin` account.   

## <a name="examples"></a>Esempio  
 La query seguente consente di determinare per quanto tempo un thread di lavoro è rimasto in esecuzione in stato SUSPENDED o RUNNABLE.  
  
```sql
SELECT   
    t1.session_id,  
    CONVERT(varchar(10), t1.status) AS status,  
    CONVERT(varchar(15), t1.command) AS command,  
    CONVERT(varchar(10), t2.state) AS worker_state,  
    w_suspended =   
      CASE t2.wait_started_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_started_ms_ticks  
      END,  
    w_runnable =   
      CASE t2.wait_resumed_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_resumed_ms_ticks  
      END  
  FROM sys.dm_exec_requests AS t1  
  INNER JOIN sys.dm_os_workers AS t2  
    ON t2.task_address = t1.task_address  
  CROSS JOIN sys.dm_os_sys_info AS t3  
  WHERE t1.scheduler_id IS NOT NULL;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 session_id status     command         worker_state w_suspended w_runnable  
 ---------- ---------- --------------- ------------ ----------- --------------------  
 4          background LAZY WRITER     SUSPENDED    688         688  
 6          background LOCK MONITOR    SUSPENDED    4657        4657
 19         background BRKR TASK       SUSPENDED    603820344   603820344  
 14         background BRKR EVENT HNDL SUSPENDED    63583641    63583641  
 51         running    SELECT          RUNNING      0           0  
 2          background RESOURCE MONITO RUNNING      0           603825954  
 3          background LAZY WRITER     SUSPENDED    422         422  
 7          background SIGNAL HANDLER  SUSPENDED    603820485   603820485  
 13         background TASK MANAGER    SUSPENDED    603824704   603824704  
 18         background BRKR TASK       SUSPENDED    603820407   603820407  
 9          background TRACE QUEUE TAS SUSPENDED    454         454  
 52         suspended  SELECT          SUSPENDED    35094       35094  
 1          background RESOURCE MONITO RUNNING      0           603825954  
```

 Nell'output, i valori equivalenti di `w_runnable` e `w_suspended` indicano il tempo durante cui il thread di lavoro rimane in stato SUSPENDED. In tutti gli altri casi `w_runnable` rappresenta il tempo trascorso dal thread di lavoro in stato RUNNABLE. Nell'output la sessione `52` è in stato `SUSPENDED` per `35,094` millisecondi.  
  
## <a name="see-also"></a>Vedere anche  
[SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md)    
