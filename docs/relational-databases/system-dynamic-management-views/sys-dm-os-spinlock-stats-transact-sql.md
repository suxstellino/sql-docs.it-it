---
description: sys.dm_os_spinlock_stats (Transact-SQL)
title: sys.dm_os_spinlock_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: ''
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_spinlock_stats_TSQL
- dm_os_spinlock_stats_TSQL
- dm_os_spinlock_stats
- sys.dm_os_spinlock_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_spinlock_stats dynamic management view
author: bluefooted
ms.author: pamela
ms.reviewer: wiassaf
manager: amitban
ms.openlocfilehash: 396c52a6d687c554b277e6f26c290978f035ae01
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099771"
---
# <a name="sysdm_os_spinlock_stats-transact-sql"></a>sys.dm_os_spinlock_stats (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce informazioni su tutte le attese di SpinLock organizzate in base al tipo.  
  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Nome del tipo SpinLock.|  
|conflitti|**bigint**|Numero di volte in cui un thread tenta di acquisire lo spinlock ed è bloccato perché un altro thread attualmente possiede SpinLock.|  
|giri|**bigint**|Numero di volte in cui un thread esegue un ciclo durante il tentativo di acquisizione di SpinLock.|  
|spins_per_collision|**real**|Rapporto di rotazioni per conflitto.|  
|sleep_time|**bigint**|Quantità di tempo in millisecondi impiegato dai thread per la sospensione in caso di backoff.|  
|backoff|**int**|Il numero di volte in cui un thread che è "Spinning" non riesce ad acquisire il spinlock e restituisce l'utilità di pianificazione.|  


## <a name="permissions"></a>Autorizzazioni  
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.    
  
## <a name="remarks"></a>Osservazioni  
 
 sys.dm_os_spinlock_stats può essere utilizzato per identificare l'origine della contesa di SpinLock. In alcune situazioni, potrebbe essere possibile risolvere o ridurre la contesa di SpinLock. Si possono tuttavia presentare situazioni in cui è necessario contattare il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
 È possibile reimpostare il contenuto di sys.dm_os_spinlock_stats usando `DBCC SQLPERF` come indicato di seguito:  
  
```  
DBCC SQLPERF ('sys.dm_os_spinlock_stats', CLEAR);  
GO  
```  
  
 Questo comando reimposta tutti i contatori su 0.  
  
> [!NOTE]  
>  Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene riavviato, le statistiche non sono persistenti. Tutti i dati sono cumulativi a partire dall'ultima reimpostazione delle statistiche oppure dall'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="spinlocks"></a>SpinLock  
 SpinLock è un oggetto di sincronizzazione leggero usato per serializzare l'accesso alle strutture di dati che in genere vengono mantenute per un breve periodo di tempo. Quando un thread tenta di accedere a una risorsa protetta da uno spinlock che viene mantenuto da un altro thread, il thread eseguirà un ciclo o "Spin" e tenterà nuovamente di accedere alla risorsa, anziché restituire immediatamente l'utilità di pianificazione come con un latch o un'altra attesa di risorse. Il thread continuerà a ruotare fino a quando la risorsa non è disponibile o il ciclo viene completato, a quel punto il thread produrrà l'utilità di pianificazione e tornerà nella coda eseguibile. Questa procedura consente di ridurre il cambio del contesto di thread eccessivo, ma quando la contesa per uno spinlock è elevata, è possibile osservare un utilizzo significativo della CPU.
   
 La tabella seguente contiene brevi descrizioni di alcuni dei tipi più comuni di SpinLock.  
  
|Tipo SpinLock|Descrizione|  
|-----------------|-----------------|  
|ABR|Solo per uso interno.|
|ADB_CACHE|Solo per uso interno.|
|ALLOC_CACHES_HASH|Solo per uso interno.|
|APPENDONLY_STORAGE|Solo per uso interno.|
|APRC_BACK_OFF_STATS|Solo per uso interno.|
|APRC_EVENT_LIST|Solo per uso interno.|
|APRC_QUEUE_LIST|Solo per uso interno.|
|APRC_VALIDATION_QUEUE_LIST|Solo per uso interno.|
|ASYNC_OP_ADMIN_CLIENT_REGISTRATION_LIST|Solo per uso interno.|
|ASYNC_OP_ADMIN_WORK_REGISTRATION_HASH_TABLE|Solo per uso interno.|
|ASYNCSTATSLIST|Solo per uso interno.|
|BACKUP|Solo per uso interno.|
|BACKUP_COPY_CONTEXT|Solo per uso interno.|
|BACKUP_CTX|Solo per uso interno.|
|BASE_XACT_HASH|Solo per uso interno.|
|BLOCKER_ENUM|Solo per uso interno.|
|BPREPARTITION|Solo per uso interno.|
|BPWORKFILE|Solo per uso interno.|
|BUF_HASH|Solo per uso interno.|
|BUF_LINK|Solo per uso interno.|
|BUF_WRITE_LOG|Solo per uso interno.|
|CACHEOBJ_DBG|Solo per uso interno.|
|CHANNELFORCECLOSEMANAGER|Solo per uso interno.|
|CHECK_AGGREGATE_STATE|Solo per uso interno.|
|CLR_HOSTTASK|Solo per uso interno.|
|CLR_SPIN_LOCK|Solo per uso interno.|
|CMED_DATABASE|Solo per uso interno.|
|CMED_HASH_SET|Solo per uso interno.|
|COLUMNDATASETSESSIONLIST|Solo per uso interno.|
|COLUMNSTORE_HASHTABLE|Solo per uso interno.|
|COLUMNSTOREBUILDSTATE_LIST|Solo per uso interno.|
|COM_INIT|Solo per uso interno.|
|COMMIT|Solo per uso interno.|
|COMPPLAN_SKELETON|Solo per uso interno.|
|CONNECTION_MANAGER|Solo per uso interno.|
|COLLEGA|Solo per uso interno.|
|CSIBUILDMEM|Solo per uso interno.|
|CURSOR|Solo per uso interno.|
|CURSQL|Solo per uso interno.|
|DATAPORTCONSUMER|Solo per uso interno.|
|DATAPORTSOURCEINFOCREDIT|Solo per uso interno.|
|DATAPORTSOURCEINFOQUEUE|Solo per uso interno.|
|DATASET_FREELIST|Solo per uso interno.|
|DBCC_CHECK|Solo per uso interno.|
|DBSEEDING_OPERATION|Solo per uso interno.|
|DBT_HASH|Solo per uso interno.|
|DBT_IO_LIST|Solo per uso interno.|
|DBTABLE|Controlla l'accesso a una struttura di dati in memoria per ogni database in un SQL Server che contiene le proprietà di tale database. Per altre informazioni, vedere [questo articolo](https://techcommunity.microsoft.com/t5/SQL-Server/Improving-Concurrency-Scalability-of-SQL-Server-workload-by/ba-p/384789). |
|DEFERRED_WF_EXT_DROP|Solo per uso interno.|
|DEK_INSTANCE|Solo per uso interno.|
|DELAYED_PARTITIONED_STACK|Solo per uso interno.|
|DELETEBITMAP|Solo per uso interno.|
|DIAG_MANAGER|Solo per uso interno.|
|DIAG_OBJECT|Solo per uso interno.|
|DIGEST_CACHE|Solo per uso interno.|
|DINPBUF|Solo per uso interno.|
|DIRECTLOGCONSUMER|Solo per uso interno.|
|DP_LIST|Controlla l'accesso all'elenco di pagine dirty per un database in cui è attivato il Checkpoint indiretto. Per altre informazioni, vedere [questo articolo](https://techcommunity.microsoft.com/t5/SQL-Server/Indirect-Checkpoint-and-tempdb-8211-the-good-the-bad-and-the-non/ba-p/385510).|
|DROP|Solo per uso interno.|
|DROP_TEMPO|Solo per uso interno.|
|DROPPED_ALLOC_UNIT|Solo per uso interno.|
|DTC_HASHTABLE|Solo per uso interno.|
|DTT_LIST|Solo per uso interno.|
|ENDD_LIST|Solo per uso interno.|
|EXT_CACHE|Solo per uso interno.|
|EXTENT_ACTIVATION|Solo per uso interno.|
|FABRIC_DB_MGR_PTR|Solo per uso interno.|
|FABRIC_LOG_MANAGEMENT_INPUT_VALUE|Solo per uso interno.|
|FABRIC_REPLICA_TRANSPORT|Solo per uso interno.|
|FABRIC_TVF_DATA_CONSUMER_LIST|Solo per uso interno.|
|FABRIC_TVF_LOAD_LIB|Solo per uso interno.|
|FCB_REPLICA_SYNC|Solo per uso interno.|
|FGCB_PRP_FILL|Solo per uso interno.|
|FILE_HANDLE_CACHE|Solo per uso interno.|
|FILE_TABLE|Solo per uso interno.|
|FILESTREAM_CHUNKER|Solo per uso interno.|
|FREE_SPACE_CACHE_ENTRY|Solo per uso interno.|
|FS_CONTAINER_LIST_WITH_DELETE|Solo per uso interno.|
|FS_DELETED_FOLDER_CLEANUP|Solo per uso interno.|
|FSAGENT|Solo per uso interno.|
|FSGHOST_STATUS|Solo per uso interno.|
|FT_INIT|Solo per uso interno.|
|GHOST_FREE|Solo per uso interno.|
|GHOST_HASH|Solo per uso interno.|
|GLOBAL_SCHEDULER_LIST|Solo per uso interno.|
|GLOBAL_TRACE_FLAGS|Solo per uso interno.|
|GLOBALTRANS|Solo per uso interno.|
|GROUP_COMMIT_FEEDBACK_LOOP|Solo per uso interno.|
|GUARDIAN|Solo per uso interno.|
|HADR_AGH_X_ACCESS|Solo per uso interno.|
|HADR_AR_CONTROLLER_COLLECTION|Solo per uso interno.|
|HADR_AR_DB_MGR|Solo per uso interno.|
|HADR_AR_TRANSPORT|Solo per uso interno.|
|HADR_COMPRESSION_MGR_POOL|Solo per uso interno.|
|HADR_FABRIC_FACTORY|Solo per uso interno.|
|HADR_PRIORITY_QUEUE|Solo per uso interno.|
|HADR_TRANSPORT_CONTROL|Solo per uso interno.|
|HADR_TRANSPORT_LIST|Solo per uso interno.|
|HADRSEEDINGLIST|Solo per uso interno.|
|HOBT_DROPPED|Solo per uso interno.|
|HOBT_HASH|Solo per uso interno.|
|HTTP|Solo per uso interno.|
|HTTP_CONNCACHE|Solo per uso interno.|
|HTTP_ENDPOINT|Solo per uso interno.|
|IDENTITY|Solo per uso interno.|
|INDEX_CREATE|Solo per uso interno.|
|IO_DISPENSER_PAUSE|Solo per uso interno.|
|IO_RG_VOLUME_HASHTABLE|Solo per uso interno.|
|IOREQ|Solo per uso interno.|
|ISSRESOURCE|Solo per uso interno.|
|KTM_ENLISTMENT|Solo per uso interno.|
|LANG_RES_LOAD|Solo per uso interno.|
|LIVE_TARGET_TVF|Solo per uso interno.|
|LOCK_FREE_LIST|Solo per uso interno.|
|LOCK_HASH|Consente di proteggere l'accesso alla tabella hash di gestione blocchi in cui sono archiviate le informazioni sui blocchi mantenuti in un database. Per altre informazioni, vedere [questo articolo](https://support.microsoft.com/kb/2926217).|
|LOCK_NOTIFICATION|Solo per uso interno.|
|LOCK_RESOURCE_ID|Solo per uso interno.|
|LOCK_RW_ABTX_HASH_SET|Solo per uso interno.|
|LOCK_RW_AGDB_HEALTH_DIAG|Solo per uso interno.|
|LOCK_RW_CMED_HASH_SET|Solo per uso interno.|
|LOCK_RW_DPT_TABLE|Solo per uso interno.|
|LOCK_RW_IN_ROW_TRACKER|Solo per uso interno.|
|LOCK_RW_LOGIN_RATE_STATS|Solo per uso interno.|
|LOCK_RW_PVS_PAGE_TRACKER|Solo per uso interno.|
|LOCK_RW_RBIO_REQ|Solo per uso interno.|
|LOCK_RW_SECURITY_CACHE|Solo per uso interno.|
|LOCK_RW_TEST|Solo per uso interno.|
|LOCK_RW_WPR_BUCKET|Solo per uso interno.|
|LOCK_SORT_STREAM|Solo per uso interno.|
|LOCK_SQLSATELLITE_MESSAGE|Solo per uso interno.|
|LOG_CONSOLIDATION|Solo per uso interno.|
|LOG_RG_GOVERNOR|Solo per uso interno.|
|LOGCACHE_ACCESS|Solo per uso interno.|
|LOGFLUSHQ|Solo per uso interno.|
|LOGIOSEQ|Solo per uso interno.|
|LOGIOSEQMAPPENDINGMESSAGEQUEUE|Solo per uso interno.|
|LOGLC|Solo per uso interno.|
|LOGLFM|Solo per uso interno.|
|LOGON_TRIGGER_CACHE|Solo per uso interno.|
|LOGPOOL_HASHBUCKET|Solo per uso interno.|
|LOGPOOL_REFCOUNTEDOBJECT|Solo per uso interno.|
|LOGPOOL_SHAREDCACHEBUFFER|Solo per uso interno.|
|LOGPOOL_SIZEPERRESOURCEPOOL|Solo per uso interno.|
|LPE_BATCH|Solo per uso interno.|
|LPE_SESSION|Solo per uso interno.|
|LPE_SXTP|Solo per uso interno.|
|LSID|Solo per uso interno.|
|LSLIST|Solo per uso interno.|
|LSNREFLIST|Solo per uso interno.|
|LSS_SYNC_DTC|Solo per uso interno.|
|MD_CHANGE_NOTIFICATION|Solo per uso interno.|
|MDB_REMOTE_BATCH_STATS_HASH_TABLE|Solo per uso interno.|
|MDB_REMOTE_SESSION_HASH_TABLE|Solo per uso interno.|
|MEM_MGR|Solo per uso interno.|
|MGR_CACHE|Solo per uso interno.|
|MIGRATION_BUF_LIST|Solo per uso interno.|
|NETCONN_ADDRESS|Solo per uso interno.|
|ONDEMAND_TASK|Solo per uso interno.|
|ONE_PROC_SIM_NODE_CONTEXT|Solo per uso interno.|
|ONE_PROC_SIM_NODE_CONTEXT_LIST|Solo per uso interno.|
|ONE_PROC_SIM_REPLICA_CONTEXT|Solo per uso interno.|
|ONE_PROC_SIM_SERVICE_PARTITION|Solo per uso interno.|
|OPT_IDX_MISS_ID|Solo per uso interno.|
|OPT_IDX_MISS_KEY|Solo per uso interno.|
|OPT_IDX_STATS|Solo per uso interno.|
|OPT_INFO_MGR|Solo per uso interno.|
|PAGE_WORKITEMLIST|Solo per uso interno.|
|PAGECOPIER|Solo per uso interno.|
|PARALLELREDOCACHE|Solo per uso interno.|
|PARTITIONED_HEAP_FREE_LIST|Solo per uso interno.|
|PROGRESS_REPORT|Solo per uso interno.|
|QE_SHUTDOWN|Solo per uso interno.|
|QSCAN_CACHE|Solo per uso interno.|
|QUERY_EXEC_STATS|Solo per uso interno.|
|QUERY_STORE_ASYNC_PERSIST|Solo per uso interno.|
|QUERY_STORE_ASYNC_QUEUE_TLIST|Solo per uso interno.|
|QUERY_STORE_CAPTURE_POLICY_INTERVAL|Solo per uso interno.|
|QUERY_STORE_CAPTURE_POLICY_STATS|Solo per uso interno.|
|QUERY_STORE_CAPTURE_POLICY_THRESHOLD|Solo per uso interno.|
|QUERY_STORE_CURRENT_INTERVAL|Solo per uso interno.|
|QUERY_STORE_HT_CACHE|Solo per uso interno.|
|QUERY_STORE_LIST|Solo per uso interno.|
|QUERY_STORE_PLAN_COMP_AGG|Solo per uso interno.|
|QUERY_STORE_PLAN_LIST|Solo per uso interno.|
|QUERY_STORE_READ_ONLY_FLAGS|Solo per uso interno.|
|QUERY_STORE_SELF_AGG|Solo per uso interno.|
|QUERY_STORE_STMT_COMP_AGG|Solo per uso interno.|
|QUERYEXEC|Solo per uso interno.|
|QUERYSCAN|Solo per uso interno.|
|RANGE_GENERATION|Solo per uso interno.|
|READ_AHEAD|Solo per uso interno.|
|REDOMGRSTATE|Solo per uso interno.|
|REMOTE_SESSION_CACHE|Solo per uso interno.|
|REMOTEBLOCKIO|Solo per uso interno.|
|REMOTEOP|Solo per uso interno.|
|REPL_LOGREADER_HISTORY_CACHE|Solo per uso interno.|
|REPL_LOGREADER_PERDB_HISTORY_CACHE|Solo per uso interno.|
|RESMANAGER|Solo per uso interno.|
|RISORSA|Solo per uso interno.|
|RESQUEUE|Solo per uso interno.|
|RFS_THREAD_QUEUE|Solo per uso interno.|
|RG_TIMER|Solo per uso interno.|
|ROWGROUP_VERSIONS|Solo per uso interno.|
|RPCCHANNELPOOL|Solo per uso interno.|
|RPCPACKAGE|Solo per uso interno.|
|RPCREQUESTORCONTEXT|Solo per uso interno.|
|RWLOCK_LAST|Solo per uso interno.|
|SATELLITE_CONNECTION|Solo per uso interno.|
|SBS_CLIENT_ENDPOINTS|Solo per uso interno.|
|SBS_CLIENT_REQUESTS|Solo per uso interno.|
|SBS_DISPATCH|Solo per uso interno.|
|SBS_PENDING|Solo per uso interno.|
|SBS_SERVER_XACT_TASK_PROXY|Solo per uso interno.|
|SBS_TRANSPORT|Solo per uso interno.|
|SBS_UCS_DISPATCH|Solo per uso interno.|
|SICUREZZA|Solo per uso interno.|
|SECURITY_CACHE|Solo per uso interno.|
|SECURITY_FEDAUTH_AAD_BECWSCONNS|Solo per uso interno.|
|SEMANTIC_TICACHE|Solo per uso interno.|
|SEQUENCED_OBJECT|Solo per uso interno.|
|SEQUEUE_SIZED_THREADSAFE|Solo per uso interno.|
|SESSION_KILLER|Solo per uso interno.|
|SESSION_MANAGER|Solo per uso interno.|
|SESSION_SEC_CONTEXT|Solo per uso interno.|
|SETRANGE_SYNC|Solo per uso interno.|
|SHARABLE_SESSION_OBJECTS|Solo per uso interno.|
|SLO_INFO_LIST|Solo per uso interno.|
|SNI|Solo per uso interno.|
|SNI_NODE_PENDING_IO_QUEUE|Solo per uso interno.|
|SOAPSESSIONS|Solo per uso interno.|
|SOS_ABORT_TASK|Solo per uso interno.|
|SOS_ACTIVEDESCRIPTOR|Solo per uso interno.|
|SOS_BLOCKALLOCPARTIALLIST|Solo per uso interno.|
|SOS_BLOCKDESCRIPTORBUCKET|Solo per uso interno.|
|SOS_CACHESTORE|Sincronizza l'accesso a diverse cache in memoria in SQL Server come la cache dei piani o la cache della tabella temporanea. Una contesa forte su questo tipo di SpinLock può indicare molti aspetti diversi a seconda della cache specifica in conflitto. Contattare [!INCLUDE[msCoName](../../includes/msconame-md.md)] il servizio supporto tecnico clienti per informazioni sulla risoluzione di questo tipo di SpinLock. |
|SOS_CACHESTORE_CLOCK|Solo per uso interno.|
|SOS_CLOCKALG_INTERNODE_SYNC|Solo per uso interno.|
|SOS_DEBUG_HOOK|Solo per uso interno.|
|SOS_DESCDATABUFFERLIST|Solo per uso interno.|
|SOS_LARGEPAGE_ALLOCATOR|Solo per uso interno.|
|SOS_MINITHREAD|Solo per uso interno.|
|SOS_NODE|Solo per uso interno.|
|SOS_OBJECT_POOL|Solo per uso interno.|
|SOS_OBJECT_STORE|Solo per uso interno.|
|SOS_OOM_CHECK|Solo per uso interno.|
|SOS_PHYS_PAGE_CACHE|Solo per uso interno.|
|SOS_RESOURCE_CLERK_LIST|Solo per uso interno.|
|SOS_RINGBUFFER_RECORD|Solo per uso interno.|
|SOS_RW|Solo per uso interno.|
|SOS_SATELLITE_USER_POOL|Solo per uso interno.|
|SOS_SCHEDULER|Solo per uso interno.|
|SOS_SELIST_SIZED_SLOCK|Solo per uso interno.|
|SOS_SUSPEND_QUEUE|Solo per uso interno.|
|SOS_SYSTHREAD|Solo per uso interno.|
|SOS_SYSTHREAD_DISPATCHER|Solo per uso interno.|
|SOS_TASK|Solo per uso interno.|
|SOS_TLIST|Solo per uso interno.|
|SOS_VM_LOW|Solo per uso interno.|
|SOS_WAIT_STATS|Solo per uso interno.|
|SOS_WAITABLE_ADDRESS_HASHBUCKET|Solo per uso interno.|
|SPIN_EVENT_MUTEX|Solo per uso interno.|
|SPL_DISPATCHER_LIST|Solo per uso interno.|
|SPL_DISPATCHER_QUEUE|Solo per uso interno.|
|SPL_NONYIELD_ANALYSIS|Solo per uso interno.|
|SPL_QUERY_STORE_CTX_INITIALIZED|Solo per uso interno.|
|SPL_QUERY_STORE_EXEC_STATS_AGG|Solo per uso interno.|
|SPL_QUERY_STORE_EXEC_STATS_READ|Solo per uso interno.|
|SPL_QUERY_STORE_STATS_COOKIE_CACHE|Solo per uso interno.|
|SPL_SOS_DISPATCHER|Solo per uso interno.|
|SPL_TDS_PKT_QUEUE|Solo per uso interno.|
|SPL_XE_BUFFER_MGR|Solo per uso interno.|
|SPL_XE_DISPATCHER_QUEUE|Solo per uso interno.|
|SPL_XE_NOTIFICATION_CALLBACK_LIST|Solo per uso interno.|
|SPL_XE_SESSION_EVENT_MGR|Solo per uso interno.|
|SPL_XE_SESSION_MGR|Solo per uso interno.|
|SPL_XE_SESSION_TARGET_MGR|Solo per uso interno.|
|SPT_PROFILE|Solo per uso interno.|
|SQL_MGR|Solo per uso interno.|
|SQL_NORM|Solo per uso interno.|
|SQLTRACE_FILE_BUFFER|Solo per uso interno.|
|SRVPROC|Solo per uso interno.|
|STACK_HASHER|Solo per uso interno.|
|Sottolatch|Solo per uso interno.|
|SUBPDESC|Solo per uso interno.|
|SUBPDESC_LIST|Solo per uso interno.|
|SVC_BROKER_CTRL|Solo per uso interno.|
|SVC_BROKER_DEBUG_LIST|Solo per uso interno.|
|SVC_BROKER_LIST|Solo per uso interno.|
|SVC_BROKER_OBJECT|Solo per uso interno.|
|SYNCPOINT_RESOURCE|Solo per uso interno.|
|TaskElapsedExecutionMonitor|Solo per uso interno.|
|TDS_TVP|Solo per uso interno.|
|TESTTEAM|Solo per uso interno.|
|TESTTEAMEXPONENTIAL|Solo per uso interno.|
|TESTTEAMEXPONENTIALTASTAS|Solo per uso interno.|
|TESTTEAMTASTAS|Solo per uso interno.|
|TMP_SESS_KEY|Solo per uso interno.|
|TSQL_DEBUG|Solo per uso interno.|
|TXFRM_REPL|Solo per uso interno.|
|VDI_OPERATION|Solo per uso interno.|
|WINFAB_REPORT_FAULT|Solo per uso interno.|
|WRITE_PAGE_RECORDER|Solo per uso interno.|
|X_PACKET_LIST|Solo per uso interno.|
|X_PIPE|Solo per uso interno.|
|X_PIPE_DEMAND|Solo per uso interno.|
|X_PORT|Solo per uso interno.|
|XACT_LOCK_INFO|Solo per uso interno.|
|XACT_LOCKINFO_TASK|Solo per uso interno.|
|XACT_WORKSPACE|Solo per uso interno.|
|XCB|Solo per uso interno.|
|XCB_FREE_LIST|Solo per uso interno.|
|XCB_HASH|Solo per uso interno.|
|XCHNG_TRACE|Solo per uso interno.|
|XDES|Solo per uso interno.|
|XDES_HASH|Solo per uso interno.|
|XDESMGR|Solo per uso interno.|
|XDESTABLELIST|Solo per uso interno.|
|XE_RATE_LIMITER_STRETCHDB|Solo per uso interno.|
|XE_SESSION_STORAGE|Solo per uso interno.|
|XID_ARRAY|Solo per uso interno.|
|XIO_BLOCKLIST|Solo per uso interno.|
|XIO_REQSTR|Solo per uso interno.|
|XIO_SEQNUMBUMP|Solo per uso interno.|
|XIOSTATS|Solo per uso interno.|
|XTP_RT_DATA_LIST|Solo per uso interno.|
|XTS_MGR|Solo per uso interno.|
|XVB_CSN|Solo per uso interno.|
|XVB_LIST|Solo per uso interno.|
 

  
## <a name="see-also"></a>Vedere anche  
 
 [DBCC SQLPERF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)   
 
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  

 [Quando SpinLock rappresenta un driver significativo di utilizzo della CPU in SQL Server?](https://techcommunity.microsoft.com/t5/SQL-Server-Support/When-is-Spinlock-a-Significant-Driver-of-CPU-utilization-in-SQL/ba-p/530142)

 [Diagnosi e risoluzione della contesa di SpinLock nei SQL Server](../diagnose-resolve-spinlock-contention.md)
  
  


