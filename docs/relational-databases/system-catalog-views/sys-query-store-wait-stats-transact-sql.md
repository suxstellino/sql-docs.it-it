---
description: sys.query_store_wait_stats (Transact-SQL)
title: sys.query_store_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2517cd093bd840c8cf4cea34073acb8e03a98c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171819"
---
# <a name="sysquery_store_wait_stats-transact-sql"></a>sys.query_store_wait_stats (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Contiene informazioni sulle informazioni di attesa per la query.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|Identificatore della riga che rappresenta le statistiche di attesa per il plan_id, runtime_stats_interval_id, execution_type e wait_category. È univoco solo per gli intervalli di statistiche di runtime precedenti. Per l'intervallo attualmente attivo, è possibile che siano presenti più righe che rappresentano le statistiche di attesa per il piano a cui fa riferimento plan_id, con il tipo di esecuzione rappresentato da execution_type e la categoria di attesa rappresentata da wait_category. In genere, una riga rappresenta le statistiche di attesa scaricate su disco, mentre altre rappresentano lo stato in memoria. Quindi, per ottenere lo stato effettivo per ogni intervallo è necessario aggregare le metriche, raggruppando per plan_id, runtime_stats_interval_id, execution_type e wait_category. |  
|**plan_id**|**bigint**|Chiave esterna. Si unisce a [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Chiave esterna. Si unisce a [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**wait_category**|**tinyint**|I tipi di attesa sono suddivisi in categorie usando la tabella riportata di seguito, quindi il tempo di attesa viene aggregato tra le categorie di attesa. Diverse categorie di attesa richiedono un'analisi di completamento diversa per risolvere il problema, ma i tipi di attesa dalla stessa categoria portano a esperienze di risoluzione dei problemi simili e la query interessata, oltre alle attese, è la parte mancante per completare correttamente la maggior parte delle indagini.|
|**wait_category_desc**|**nvarchar(128)**|Per la descrizione testuale del campo categoria di attesa, esaminare la tabella seguente.|
|**execution_type**|**tinyint**|Determina il tipo di esecuzione della query:<br /><br /> 0-esecuzione regolare (completata correttamente)<br /><br /> 3-esecuzione interrotta dal client<br /><br /> 4-esecuzione interrotta eccezione|  
|**execution_type_desc**|**nvarchar(128)**|Descrizione testuale del campo tipo di esecuzione:<br /><br /> 0-normale<br /><br /> 3-interrotto<br /><br /> 4-eccezione|  
|**total_query_wait_time_ms**|**bigint**|`CPU wait`Tempo totale per il piano di query all'interno dell'intervallo di aggregazione e della categoria di attesa (in millisecondi).|
|**avg_query_wait_time_ms**|**float**|Durata media di attesa per il piano di query per esecuzione all'interno dell'intervallo di aggregazione e della categoria di attesa (in millisecondi).|
|**last_query_wait_time_ms**|**bigint**|Durata dell'ultima attesa per il piano di query all'interno dell'intervallo di aggregazione e della categoria di attesa (in millisecondi).|
|**min_query_wait_time_ms**|**bigint**|`CPU wait`Tempo minimo per il piano di query all'interno dell'intervallo di aggregazione e della categoria di attesa (in millisecondi).|
|**max_query_wait_time_ms**|**bigint**|`CPU wait`Tempo massimo per il piano di query all'interno dell'intervallo di aggregazione e della categoria di attesa (in millisecondi).|
|**stdev_query_wait_time_ms**|**float**|`Query wait` deviazione standard della durata per il piano di query entro l'intervallo di aggregazione e la categoria di attesa (in millisecondi).|

## <a name="wait-categories-mapping-table"></a>Tabella di mapping delle categorie di attesa

"%" viene usato come carattere jolly
  
|Valore intero|Categoria di attesa|I tipi di attesa includono nella categoria|  
|-----------------|---------------|-----------------|  
|**0**|**Unknown**|Sconosciuto |  
|**1**|**CPU**|SOS_SCHEDULER_YIELD|
|**2**|**Thread di lavoro**|THREADPOOL|
|**3**|**Lock**|LCK_M_%|
|**4**|**Latch**|LATCH_%|
|**5**|**Latch del buffer**|PAGELATCH_%|
|**6**|**IO buffer**|PAGEIOLATCH_%|
|**7**|**Compilazione** _|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|_ *8**|**SQL CLR**|% CLR, SQLCLR%|
|**9**|**Mirroring**|DBMIRROR|
|**10**|**Transazione**|XACT%, DTC%, TRAN_MARKLATCH_%, MSQL_XACT_%, TRANSACTION_MUTEX|
|**11**|**Inattivo**|SLEEP_%, LAZYWRITER_SLEEP, SQLTRACE_BUFFER_FLUSH, SQLTRACE_INCREMENTAL_FLUSH_SLEEP, SQLTRACE_WAIT_ENTRIES, FT_IFTS_SCHEDULER_IDLE_WAIT, XE_DISPATCHER_WAIT, REQUEST_FOR_DEADLOCK_SEARCH, LOGMGR_QUEUE, ONDEMAND_TASK_QUEUE, CHECKPOINT_QUEUE, XE_TIMER_EVENT|
|**12**|**PreEmptive**|PREEMPTIVE_%|
|**13**|**Service Broker**|BROKER_% **(ma non BROKER_RECEIVE_WAITFOR)**|
|**14**|**IO log i/o**|LOGMGR, LOGBUFFER, LOGMGR_RESERVE_APPEND, LOGMGR_FLUSH, LOGMGR_PMM_LOG, CHKPT, WRITELOG|
|**15**|**IO rete**|ASYNC_NETWORK_IO, NET_WAITFOR_PACKET, PROXY_NETWORK_IO, EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**Parallelism**|CXPACKET, EXCHANGE, HT%, BMP%, BP%|
|**17**|**Memoria**|RESOURCE_SEMAPHORE, CMEMTHREAD, CMEMPARTITIONED, EE_PMOLOCK, MEMORY_ALLOCATION_EXT, RESERVED_MEMORY_ALLOCATION_EXT MEMORY_GRANT_UPDATE|
|**18**|**Attesa utente**|ASPETTER, WAIT_FOR_RESULTS, BROKER_RECEIVE_WAITFOR|
|**19**|**Traccia**|TRACEWRITE, SQLTRACE_LOCK, SQLTRACE_FILE_BUFFER, SQLTRACE_FILE_WRITE_IO_COMPLETION, SQLTRACE_FILE_READ_IO_COMPLETION, SQLTRACE_PENDING_BUFFER_WRITERS, SQLTRACE_SHUTDOWN, QUERY_TRACEOUT, TRACE_EVTNOTIFF|
|**20**|**Ricerca full-text**|FT_RESTART_CRAWL, FULL-TEXT GATHERER, MSSEARCH, FT_METADATA_MUTEX, FT_IFTSHC_MUTEX, FT_IFTSISM_MUTEX, FT_IFTS_RWLOCK, FT_COMPROWSET_RWLOCK, FT_MASTER_MERGE, FT_PROPERTYLIST_CACHE, FT_MASTER_MERGE_COORDINATOR, PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC|
|**21**|**Altro IO disco**|ASYNC_IO_COMPLETION, IO_COMPLETION, BACKUPIO, WRITE_COMPLETION, IO_QUEUE_LIMIT, IO_RETRY|
|**22**|**Replica**|SE_REPL_%, REPL_%, HADR_% **(ma non HADR_THROTTLE_LOG_RATE_GOVERNOR)**, PWAIT_HADR_%, REPLICA_WRITES, FCB_REPLICA_WRITE, FCB_REPLICA_READ PWAIT_HADRSIM|
|**23**|**Log rate Governor**|LOG_RATE_GOVERNOR, POOL_LOG_RATE_GOVERNOR, HADR_THROTTLE_LOG_RATE_GOVERNOR, INSTANCE_LOG_RATE_GOVERNOR|

La categoria di attesa di **compilazione** non è attualmente supportata.

## <a name="permissions"></a>Autorizzazioni

 È necessaria l'autorizzazione `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Vedere anche

- [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
- [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
- [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
- [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
- [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
- [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
- [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Stored procedure di Query Store &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
