---
description: sys.dm_os_waiting_tasks (Transact-SQL)
title: sys.dm_os_waiting_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_waiting_tasks
- sys.dm_os_waiting_tasks_TSQL
- dm_os_waiting_tasks_TSQL
- sys.dm_os_waiting_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_waiting_tasks dynamic management view
ms.assetid: ca5e6844-368c-42e2-b187-6e5f5afc8df3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c02bb3523eda26afb5aac7e4e904cb1aec8dd62b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097627"
---
# <a name="sysdm_os_waiting_tasks-transact-sql"></a>sys.dm_os_waiting_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni sulla coda di attesa relativa alle attività che sono in attesa di una risorsa. Per ulteriori informazioni sulle attività, vedere la [Guida all'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md).
   
> [!NOTE]  
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_waiting_tasks**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**waiting_task_address**|**varbinary (8)**|Indirizzo dell'attività in attesa.|  
|**session_id**|**smallint**|ID della sessione associata all'attività.|  
|**exec_context_id**|**int**|ID del contesto di esecuzione associato all'attività.|  
|**wait_duration_ms**|**bigint**|Tempo totale di attesa per questo tipo di attesa, in millisecondi. Questa volta è incluso **signal_wait_time**.|  
|**wait_type**|**nvarchar(60)**|Nome del tipo di attesa.|  
|**resource_address**|**varbinary (8)**|Indirizzo della risorsa attesa dall'attività.|  
|**blocking_task_address**|**varbinary (8)**|Attività che mantiene bloccata la risorsa.|  
|**blocking_session_id**|**smallint**|ID della sessione che sta bloccando la richiesta. Se questa colonna è NULL, la richiesta non è bloccata oppure non sono disponibili o identificabili informazioni di sessione per la sessione da cui è bloccata.<br /><br /> -2 = La risorsa di blocco appartiene a una transazione distribuita orfana.<br /><br /> -3 = La risorsa di blocco appartiene a una transazione di recupero posticipata.<br /><br /> -4 = Non è possibile determinare l'ID di sessione del proprietario del latch di blocco a causa di transizioni nello stato del latch interno.|  
|**blocking_exec_context_id**|**int**|ID del contesto di esecuzione dell'attività di blocco.|  
|**resource_description**|**nvarchar (3072)**|Descrizione della risorsa attualmente occupata. Per ulteriori informazioni, vedere l'elenco riportato di seguito.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="resource_description-column"></a>Colonna resource_description  
 La colonna resource_description dispone dei valori possibili seguenti.  
  
 **Proprietario risorsa pool di thread:**  
  
-   ID ThreadPool = utilità di pianificazione\<hex-address>  
  
 **Proprietario risorsa query parallela:**  
  
-   ID exchangeEvent = {Port | Pipe} \<hex-address> WaitType = \<exchange-wait-type> NodeId =\<exchange-node-id>  
  
 **Tipo di attesa Exchange**  
  
-   e_waitNone  
  
-   e_waitPipeNewRow  
  
-   e_waitPipeGetRow  
  
-   e_waitSynchronizeConsumerOpen  
  
-   e_waitPortOpen  
  
-   e_waitPortClose  
  
-   e_waitRange  
  
 **Proprietario risorsa di blocco:**  
  
-   \<type-specific-description> ID = \<lock-hex-address> modalità blocco = \<mode> associatedObjectId =\<associated-obj-id>  
  
     **\<type-specific-description> può essere:**  
  
    -   Per DATABASE: databaselock subresource = \<databaselock-subresource> dbid =\<db-id>  
  
    -   Per FILE: filelock fileid = \<file-id> subresource = \<filelock-subresource> dbid =\<db-id>  
  
    -   Per OBJECT: objectlock lockPartition = \<lock-partition-id> objid = \<obj-id> subresource = \<objectlock-subresource> dbid =\<db-id>  
  
    -   Per la pagina: PageLock fileid = \<file-id> pageid = \<page-id> dbid = \<db-id> subresource =\<pagelock-subresource>  
  
    -   Per Key: tasto di blocco hobtid = \<hobt-id> dbid =\<db-id>  
  
    -   Per EXTENT: extentlock fileid = \<file-id> pageid = \<page-id> dbid =\<db-id>  
  
    -   Per RID: ridlock fileid = \<file-id> pageid = \<page-id> dbid =\<db-id>  
  
    -   Per l'applicazione: applicationlock hash = \<hash> databasePrincipalId = \<role-id> dbid =\<db-id>  
  
    -   Per i metadati: metadatalock subresource = \<metadata-subresource> ClassID = \<metadatalock-description> dbid =\<db-id>  
  
    -   Per HOBT: hobtlock hobtid = \<hobt-id> subresource = \<hobt-subresource> dbid =\<db-id>  
  
    -   Per ALLOCATION_UNIT: allocunitlock hobtid = \<hobt-id> subresource = \<alloc-unit-subresource> dbid =\<db-id>  
  
     **\<mode> può essere:**  
  
     Sch-S, Sch-M, S, U, X, IS, IU, IX, SIU, SIX, UIX, BU, RangeS-S, RangeS-U, RangeI-N, RangeI-S, RangeI-U, RangeI-X, RangeX-, RangeX-U, RangeX-X  
  
 **Proprietario risorsa esterna:**  
  
-   ExternalResource esterno =\<wait-type>  
  
 **Proprietario risorsa generica:**  
  
-   Area di lavoro TransactionMutex TransactionInfo =\<workspace-id>  
  
-   Mutex  
  
-   CLRTaskJoin  
  
-   CLRMonitorEvent  
  
-   CLRRWLockEvent  
  
-   resourceWait  
  
 **Proprietario risorsa latch:**  
  
-   \<db-id>:\<file-id>:\<page-in-file>  
  
-   \<GUID>  
  
-   \<latch-class> (\<latch-address>)  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="example"></a>Esempio
### <a name="a-identify-tasks-from-blocked-sessions"></a>R. Identificare le attività da sessioni bloccate. 

```sql
SELECT * FROM sys.dm_os_waiting_tasks 
WHERE blocking_session_id IS NOT NULL; 
```   

### <a name="b-view-waiting-tasks-per-connection"></a>B. Visualizza le attività in attesa per connessione

```sql
SELECT st.text AS [SQL Text], c.connection_id, w.session_id, 
  w.wait_duration_ms, w.wait_type, w.resource_address, 
  w.blocking_session_id, w.resource_description, c.client_net_address, c.connect_time
FROM sys.dm_os_waiting_tasks AS w
INNER JOIN sys.dm_exec_connections AS c ON w.session_id = c.session_id 
CROSS APPLY (SELECT * FROM sys.dm_exec_sql_text(c.most_recent_sql_handle)) AS st 
              WHERE w.session_id > 50 AND w.wait_duration_ms > 0
ORDER BY c.connection_id, w.session_id
GO
```

### <a name="c-view-waiting-tasks-for-all-user-processes-with-additional-information"></a>C. Visualizzare le attività in attesa per tutti i processi utente con informazioni aggiuntive

```sql
SELECT 'Waiting_tasks' AS [Information], owt.session_id,
    owt.wait_duration_ms, owt.wait_type, owt.blocking_session_id,
    owt.resource_description, es.program_name, est.text,
    est.dbid, eqp.query_plan, er.database_id, es.cpu_time,
    es.memory_usage*8 AS memory_usage_KB
FROM sys.dm_os_waiting_tasks owt
INNER JOIN sys.dm_exec_sessions es ON owt.session_id = es.session_id
INNER JOIN sys.dm_exec_requests er ON es.session_id = er.session_id
OUTER APPLY sys.dm_exec_sql_text (er.sql_handle) est
OUTER APPLY sys.dm_exec_query_plan (er.plan_handle) eqp
WHERE es.is_user_process = 1
ORDER BY owt.session_id;
GO
```
  
## <a name="see-also"></a>Vedere anche  
[SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)      
[Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md)     
   
 
