---
description: sys.dm_exec_distributed_sql_requests (Transact-SQL)
title: sys.dm_exec_distributed_sql_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_DISTRIBUTED_SQL_REQUESTS_TSQL
- SYS.DM_EXEC_DISTRIBUTED_SQL_REQUESTS_TSQL
- DM_EXEC_DISTRIBUTED_SQL_REQUESTS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- sys.dm_exec_distributed_requests management view
- dm_exec_distributed_requests management view
ms.assetid: d065dc01-35d4-472f-9554-53ac41e7d104
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8c52d5fca2dfed8ff6124554cbde773496204973
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161938"
---
# <a name="sysdm_exec_distributed_sql_requests-transact-sql"></a>sys.dm_exec_distributed_sql_requests (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Include informazioni su tutte le distribuzioni di query SQL come parte di un passaggio SQL della query.  Questa visualizzazione Mostra i dati per le ultime 1000 richieste; le richieste attive hanno sempre i dati presenti in questa vista.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|execution_id e step_index costituiscono la chiave per questa visualizzazione. ID numerico univoco associato alla richiesta.|Vedere ID in [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|  
|step_index|**int**|Indice del passaggio della query di cui fa parte la distribuzione.|Vedere step_index in [sys.dm_exec_distributed_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md).|  
|compute_node_id|**int**|Tipo di operazione rappresentata da questo passaggio.|Vedere compute_node_id in [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|distribution_id|**int**|Posizione in cui è in esecuzione il passaggio.|Impostare su-1 per le richieste eseguite nell'ambito del nodo, non nell'ambito di distribuzione.|  
|status|**nvarchar(32)**|Stato di questo passaggio|Attivo, annullato, completato, non riuscito, in coda|  
|error_id|**nvarchar (36)**|ID univoco dell'errore associato a questo passaggio, se presente|Vedere ID di [sys.dm_exec_compute_node_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), null se non si è verificato alcun errore.|  
|start_time|**datetime**|Ora di inizio dell'esecuzione del passaggio|Minore o uguale all'ora corrente e maggiore o uguale a end_compile_time della query a cui appartiene questo passaggio.|  
|end_time|**datetime**|Ora di completamento dell'esecuzione di questo passaggio, annullato o non riuscito.|Minore o uguale all'ora corrente e maggiore o uguale a start_time, impostare su NULL per i passaggi attualmente in esecuzione o in coda.|  
|total_elapsed_time|**int**|Quantità totale di tempo di esecuzione del passaggio della query, in millisecondi|Compreso tra 0 e la differenza tra end_time e start_time. 0 per i passaggi in coda.|  
|row_count|**bigint**|Numero totale di righe modificate o restituite dalla richiesta|0 per i passaggi che non sono stati modificati o restituiscono dati, il numero di righe interessate in altro modo. Impostare su-1 per i passaggi DMS.|  
|spid|**int**|ID sessione nell'istanza SQL Server che esegue la distribuzione delle query||  
|.|nvarchar(4000)|Include il testo completo del comando di questo passaggio.|Qualsiasi stringa di richiesta valida per un passaggio. Troncato se è più lungo di 4000 caratteri.|  
  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
