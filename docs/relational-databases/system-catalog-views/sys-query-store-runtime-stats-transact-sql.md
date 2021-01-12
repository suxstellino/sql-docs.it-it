---
description: sys.query_store_runtime_stats (Transact-SQL)
title: sys.query_store_runtime_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.QUERY_STORE_RUNTIME_STATS_TSQL
- QUERY_STORE_RUNTIME_STATS_TSQL
- SYS.QUERY_STORE_RUNTIME_STATS
- QUERY_STORE_RUNTIME_STATS
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_runtime_stats catalog view
- sys.query_store_runtime_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a6f0f21100ca68575c8db8dcde8e70f688d0ef5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098015"
---
# <a name="sysquery_store_runtime_stats-transact-sql"></a>sys.query_store_runtime_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene informazioni sulle statistiche di esecuzione di runtime per la query.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**runtime_stats_id**|**bigint**|Identificatore della riga che rappresenta le statistiche di esecuzione di runtime per il **plan_id**, **execution_type** e **runtime_stats_interval_id**. È univoco solo per gli intervalli di statistiche di runtime precedenti. Per l'intervallo attualmente attivo possono essere presenti più righe che rappresentano le statistiche di runtime per il piano a cui fa riferimento **plan_id**, con il tipo di esecuzione rappresentato da **execution_type**. In genere, una riga rappresenta le statistiche di runtime che vengono scaricate su disco, mentre altre rappresentano lo stato in memoria. Quindi, per ottenere lo stato effettivo per ogni intervallo è necessario aggregare le metriche, raggruppando per **plan_id**, **execution_type** e **runtime_stats_interval_id**.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**plan_id**|**bigint**|Chiave esterna. Si unisce a [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Chiave esterna. Si unisce a [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**execution_type**|**tinyint**|Determina il tipo di esecuzione della query:<br /><br /> 0-esecuzione regolare (completata correttamente)<br /><br /> 3-esecuzione interrotta dal client<br /><br /> 4-esecuzione interrotta eccezione|  
|**execution_type_desc**|**nvarchar(128)**|Descrizione testuale del campo tipo di esecuzione:<br /><br /> 0-normale<br /><br /> 3-interrotto<br /><br /> 4-eccezione|  
|**first_execution_time**|**datetimeoffset**|Primo tempo di esecuzione per il piano di query all'interno dell'intervallo di aggregazione. Si riferisce all'ora di fine dell'esecuzione della query.|  
|**last_execution_time**|**datetimeoffset**|Ora dell'ultima esecuzione del piano di query all'interno dell'intervallo di aggregazione. Si riferisce all'ora di fine dell'esecuzione della query.|  
|**count_executions**|**bigint**|Numero totale di esecuzioni per il piano di query all'interno dell'intervallo di aggregazione.|  
|**avg_duration**|**float**|Durata media del piano di query all'interno dell'intervallo di aggregazione (segnalato in microsecondi).|  
|**last_duration**|**bigint**|Ultima durata del piano di query all'interno dell'intervallo di aggregazione (segnalato in microsecondi).|  
|**min_duration**|**bigint**|Durata minima del piano di query all'interno dell'intervallo di aggregazione (segnalato in microsecondi).|  
|**max_duration**|**bigint**|Durata massima del piano di query all'interno dell'intervallo di aggregazione (segnalato in microsecondi).|  
|**stdev_duration**|**float**|Deviazione standard della durata per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).|  
|**avg_cpu_time**|**float**|Tempo medio della CPU per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_cpu_time**|**bigint**|Ultimo tempo CPU per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_cpu_time**|**bigint**|Tempo minimo di CPU per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_cpu_time**|**bigint**|Tempo massimo di CPU per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_cpu_time**|**float**|Deviazione standard del tempo CPU per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_logical_io_reads**|**float**|Numero medio di letture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione. (espressa come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_logical_io_reads**|**bigint**|Ultimo numero di letture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione. (espressa come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_logical_io_reads**|**bigint**|Numero minimo di letture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione. (espressa come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_logical_io_reads**|**bigint**|Numero massimo di letture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione. (espressa come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_logical_io_reads**|**float**|Il numero di I/O logici legge la deviazione standard per il piano di query all'interno dell'intervallo di aggregazione. (espressa come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_logical_io_writes**|**float**|Numero medio di scritture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_logical_io_writes**|**bigint**|Ultimo numero di scritture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_logical_io_writes**|**bigint**|Numero minimo di scritture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_logical_io_writes**|**bigint**|Numero massimo di scritture di I/O logiche per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_logical_io_writes**|**float**|Il numero di operazioni di I/O logiche scrive la deviazione standard per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_physical_io_reads**|**float**|Numero medio di letture I/O fisiche per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_physical_io_reads**|**bigint**|Ultimo numero di letture I/O fisiche per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_physical_io_reads**|**bigint**|Numero minimo di letture di I/O fisiche per il piano di query nell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_physical_io_reads**|**bigint**|Numero massimo di letture I/O fisiche per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_physical_io_reads**|**float**|Il numero di I/O fisici legge la deviazione standard per il piano di query nell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_clr_time**|**float**|Tempo medio CLR per il piano di query all'interno dell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_clr_time**|**bigint**|Ora dell'ultimo CLR per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_clr_time**|**bigint**|Tempo CLR minimo per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_clr_time**|**bigint**|Tempo CLR massimo per il piano di query all'interno dell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_clr_time**|**float**|Deviazione standard del tempo CLR per il piano di query nell'intervallo di aggregazione (segnalato in microsecondi).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_dop**|**float**|Media DOP (grado di parallelismo) per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_dop**|**bigint**|Ultimo DOP (grado di parallelismo) per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_dop**|**bigint**|DOP minimo (grado di parallelismo) per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_dop**|**bigint**|Massimo DOP (grado di parallelismo) per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_dop**|**float**|Deviazione standard DOP (degree of parallelism) per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_query_max_used_memory**|**float**|Concessione di memoria media (indicata come numero di pagine da 8 KB) per il piano di query nell'intervallo di aggregazione. Sempre 0 per le query che usano procedure con ottimizzazione per la memoria compilate in modo nativo<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_query_max_used_memory**|**bigint**|Ultima concessione di memoria (indicata come numero di pagine da 8 KB) per il piano di query all'interno dell'intervallo di aggregazione. Sempre 0 per le query che usano procedure con ottimizzazione per la memoria compilate in modo nativo<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_query_max_used_memory**|**bigint**|Concessione di memoria minima (indicata come numero di pagine da 8 KB) per il piano di query nell'intervallo di aggregazione. Sempre 0 per le query che usano procedure con ottimizzazione per la memoria compilate in modo nativo<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_query_max_used_memory**|**bigint**|Concessione massima di memoria (indicata come numero di pagine da 8 KB) per il piano di query nell'intervallo di aggregazione. Sempre 0 per le query che usano procedure con ottimizzazione per la memoria compilate in modo nativo<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_query_max_used_memory**|**float**|Deviazione standard di concessione della memoria (indicata come numero di pagine da 8 KB) per il piano di query all'interno dell'intervallo di aggregazione. Sempre 0 per le query che usano procedure con ottimizzazione per la memoria compilate in modo nativo<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**avg_rowcount**|**float**|Numero medio di righe restituite per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_rowcount**|**bigint**|Numero di righe restituite dall'ultima esecuzione del piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_rowcount**|**bigint**|Numero minimo di righe restituite per il piano di query all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_rowcount**|**bigint**|Numero massimo di righe restituite per il piano di query all'interno dell'intervallo di aggregazione.|  
|**stdev_rowcount**|**float**|Numero di righe restituite deviazione standard per il piano di query all'interno dell'intervallo di aggregazione.|
|**avg_log_bytes_used**|**float**|Numero medio di byte nel log del database usato dal piano di query, all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**last_log_bytes_used**|**bigint**|Numero di byte nel log del database utilizzato dall'ultima esecuzione del piano di query, entro l'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**min_log_bytes_used**|**bigint**|Numero minimo di byte nel log del database utilizzato dal piano di query, entro l'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**max_log_bytes_used**|**bigint**|Numero massimo di byte nel log del database utilizzato dal piano di query, entro l'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|
|**stdev_log_bytes_used**|**float**|Deviazione standard del numero di byte nel log del database usato da un piano di query, all'interno dell'intervallo di aggregazione.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] restituirà sempre zero (0).|  
|**avg_tempdb_space_used**|**float**|Numero medio di pagine utilizzate in tempdb per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB).<br><br/>**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**last_tempdb_space_used**|**bigint**|Ultimo numero di pagine utilizzate in tempdb per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB).<br><br/>**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**min_tempdb_space_used**|**bigint**|Numero minimo di pagine utilizzate in tempdb per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB).<br><br/>**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**max_tempdb_space_used**|**bigint**|Numero massimo di pagine utilizzate in tempdb per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB).<br><br/>**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**stdev_tempdb_space_used**|**float**|Numero di pagine utilizzate nella deviazione standard di tempdb per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB).<br><br/>**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**avg_page_server_io_reads**|**float**|Numero medio di letture di I/O del server di paging per il piano di query nell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br><br/>**Si applica a:** Iperscalabilità del database SQL di Azure</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non iperscalabile) restituirà sempre zero (0).|
|**last_page_server_io_reads**|**bigint**|Ultimo numero di letture di I/O del server di paging per il piano di query nell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br><br/>**Si applica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Con iperscalabilità</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non iperscalabile) restituirà sempre zero (0).|
|**min_page_server_io_reads**|**bigint**|Numero minimo di letture di I/O del server di paging per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br><br/>**Si applica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Con iperscalabilità</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non iperscalabile) restituirà sempre zero (0).|
|**max_page_server_io_reads**|**bigint**|Numero massimo di letture di I/O del server di paging per il piano di query all'interno dell'intervallo di aggregazione (espresso come numero di pagine 8KB lette).<br><br/>**Si applica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Con iperscalabilità</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non iperscalabile) restituirà sempre zero (0).|
|**stdev_page_server_io_reads**|**float**|Il numero di pagine I/O del server di paging indica la deviazione standard per il piano di query nell'intervallo di aggregazione (espresso come numero di pagine di 8KB lette).<br><br/>**Si applica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Con iperscalabilità</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non iperscalabile) restituirà sempre zero (0).|
  
## <a name="permissions"></a>Autorizzazioni  
È necessaria l'autorizzazione `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Stored procedure di Archivio query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)    
 [Procedure consigliate per l'archivio query](../../relational-databases/performance/best-practice-with-the-query-store.md)   
  
