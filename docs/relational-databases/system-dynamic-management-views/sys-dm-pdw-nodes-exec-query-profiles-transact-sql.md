---
title: sys.dm_pdw_nodes_exec_query_profiles (Transact-SQL) | Microsoft Docs
description: Vista a gestione dinamica che può essere usata per monitorare lo stato di avanzamento delle query in tempo reale data warehouse mentre la query è in esecuzione.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: f1917899d58262f670b58843c118864618702950
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169963"
---
# <a name="sysdm_pdw_nodes_exec_query_profiles-transact-sql"></a>sys.dm_pdw_nodes_exec_query_profiles (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Monitora lo stato di avanzamento della query data warehouse in tempo reale mentre la query è in esecuzione.   
  
## <a name="table-returned"></a>Tabella restituita
  
I contatori restituiti sono specifici per ogni operatore per ogni thread. I risultati sono dinamici e non corrispondono ai risultati delle opzioni esistenti, ad esempio la `SET STATISTICS XML ON` creazione di output solo al termine della query.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|pdw_node_id|**int**|ID numerico univoco associato al nodo.|
|session_id|**smallint**|Identifica la sessione in cui viene eseguita la query. Fa riferimento a dm_exec_sessions.session_id.|  
|request_id|**int**|Identifica la richiesta di destinazione. Fa riferimento a dm_exec_sessions.request_id.|  
|sql_handle|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure di cui fa parte la query. Fa riferimento a dm_exec_query_stats.sql_handle.|  
|plan_handle|**varbinary(64)**|È un token che identifica in modo univoco un piano di esecuzione di query per un batch eseguito e il relativo piano risiede nella cache dei piani oppure è attualmente in esecuzione. Fa riferimento a dm_exec_query_stats. plan_handle.|  
|physical_operator_name|**nvarchar(256)**|Nome dell'operatore fisico.|  
|node_id|**int**|Identifica un nodo operatore nell'albero della query.|  
|thread_id|**int**|Distingue i thread (per una query parallela) che appartengono allo stesso nodo operatore della query.|  
|task_address|**varbinary (8)**|Identifica l'attività SQLOS utilizzata da questo thread. Fa riferimento a dm_os_tasks.task_address.|  
|row_count|**bigint**|Numero di righe restituite finora dall'operatore.|  
|rewind_count|**bigint**|Numero di ripristini finora.|  
|rebind_count|**bigint**|Numero di riassociazioni finora.|  
|end_of_scan_count|**bigint**|Numero di analisi terminate finora.|  
|estimate_row_count|**bigint**|Numero stimato di righe. Può essere utile per confrontare il valore estimated_row_count con il valore row_count effettivo.|  
|first_active_time|**bigint**|Ora, in millisecondi, in cui l'operatore è stato chiamato la prima volta.|  
|last_active_time|**bigint**|Ora, in millisecondi, in cui l'operatore è stato chiamato l'ultima volta.|  
|open_time|**bigint**|Timestamp apertura in millisecondi.|  
|first_row_time|**bigint**|Timestamp in cui è stata aperta la prima riga in millisecondi.|  
|last_row_time|**bigint**|Timestamp in cui è stata aperta l'ultima riga in millisecondi.|  
|close_time|**bigint**|Timestamp chiusura in millisecondi.|  
|elapsed_time_ms|**bigint**|Tempo totale trascorso (in millisecondi) utilizzato dalle operazioni del nodo di destinazione fino a questo punto.|  
|cpu_time_ms|**bigint**|Tempo totale CPU (in millisecondi) utilizzato dalle operazioni del nodo di destinazione fino a questo punto.|  
|database_id|**smallint**|ID del database contenente l'oggetto in cui vengono eseguite le letture e le scritture.|  
|object_id|**int**|Identificatore dell'oggetto in cui vengono eseguite le letture e le scritture. Fa riferimento a sys.objects.object_id.|  
|index_id|**int**|Indice in cui viene aperto il set di righe.|  
|scan_count|**bigint**|Numero di analisi tabella/indice.|  
|logical_read_count|**bigint**|Numero di letture logiche.|  
|physical_read_count|**bigint**|Numero di letture fisiche.|  
|read_ahead_count|**bigint**|Numero di letture anticipate.|  
|write_page_count|**bigint**|Numero di scritture di pagina a causa dello spill.|  
|lob_logical_read_count|**bigint**|Numero di letture logiche LOB.|  
|lob_physical_read_count|**bigint**|Numero di letture fisiche LOB.|  
|lob_read_ahead_count|**bigint**|Numero di letture anticipate LOB.|  
|segment_read_count|**int**|Numero di letture anticipate di segmenti.|  
|segment_skip_count|**int**|Numero di segmenti ignorati finora.| 
|actual_read_row_count|**bigint**|Numero di righe lette da un operatore prima dell'applicazione del predicato residuo.| 
|estimated_read_row_count|**bigint**|**Si applica a:** A partire da [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1. <br/>Numero di righe stimate per la lettura da parte di un operatore prima dell'applicazione del predicato residuo.|  
  
## <a name="remarks"></a>Commenti

Si applicano le stesse osservazioni in [sys.dm_exec_query_profiles](./sys-dm-exec-query-profiles-transact-sql.md) .  

## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW SERVER STATE` per il server.  

## <a name="see-also"></a>Vedi anche

 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
   

 ## <a name="next-steps"></a>Passaggi successivi 

Panoramica sullo sviluppo di analisi delle sinapsi di Azure] (/Azure/SQL-data-warehouse/SQL-data-warehouse-Overview-develop).