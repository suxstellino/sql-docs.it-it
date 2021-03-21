---
description: sys.dm_exec_dms_workers (Transact-SQL)
title: sys.dm_exec_dms_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.DM_EXEC_DMS_WORKERS_TSQL
- DM_EXEC_DMS_WORKERS_TSQL
- DM_EXEC_DMS_WORKERS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_dms_workers management view
- sys.dm_exec_dms_workers management view
ms.assetid: f468da29-78c3-4f10-8a3c-17905bbf46f2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cfabe8d8d7b4dff3ca81c33b7bb8e2c3dd4e65e7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752171"
---
# <a name="sysdm_exec_dms_workers-transact-sql"></a>sys.dm_exec_dms_workers (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Include informazioni su tutti i thread di lavoro che completano i passaggi DMS.  
  
 Questa visualizzazione Mostra i dati per le ultime 1000 richieste e richieste attive; le richieste attive hanno sempre i dati presenti in questa vista.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Query di cui fa parte questo ruolo di lavoro DMS. <br /><br /> execution_id, step_index e dms_step_index formano la chiave per questa visualizzazione.||  
|step_index|`int`|Passaggio della query di cui fa parte questo ruolo di lavoro DMS.|Vedere index Step in [sys.dm_exec_distributed_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md).|  
|dms_step_index|`int`|Eseguire un passaggio nel piano DMS in cui è in esecuzione il thread di lavoro.|Vedere [sys.dm_exec_dms_workers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)|  
|compute_node_id|`int`|Nodo su cui è in esecuzione il ruolo di lavoro.|Vedere [sys.dm_exec_compute_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|distribution_id|`int`|||  
|tipo|`nvarchar(32)`|Tipo di thread di lavoro DMS rappresentato da questa voce.|' DIRECT_CONVERTER ',' DIRECT_READER ',' FILE_READER ',' HASH_CONVERTER ',' HASH_READER ',' ROUNDROBIN_CONVERTER ',' EXPORT_READER ',' EXTERNAL_READER ',' EXTERNAL_WRITER ',' PARALLEL_COPY_READER ',' REJECT_WRITER ',' WRITER '|  
|status|`nvarchar(32)`|Stato di questo passaggio|' Pending ',' running ',' complete ',' failed ',' UndoFailed ',' PendingCancel ',' annullato ',' Undone ',' Aborted '|  
|bytes_per_sec|`bigint`|||  
|bytes_processed|`bigint`|||  
|rows_processed|`bigint`|||  
|start_time|`datetime`|Ora di inizio dell'esecuzione del passaggio|Minore o uguale all'ora corrente e maggiore o uguale a end_compile_time della query a cui appartiene questo passaggio.|  
|end_time|`datetime`|Ora di completamento dell'esecuzione di questo passaggio, annullato o non riuscito.|Minore o uguale all'ora corrente e maggiore o uguale a start_time, impostare su NULL per i passaggi attualmente in esecuzione o in coda.|  
|total_elapsed_time|`int`|Quantità totale di tempo di esecuzione del passaggio della query, in millisecondi|Compreso tra 0 e la differenza tra end_time e start_time. 0 per i passaggi in coda.|  
|cpu_time|`bigint`|||  
|query_time|`int`|||  
|buffers_available|`int`|||  
|dms_cpid|`int`|||  
|sql_spid|`int`|||  
|error_id|`nvarchar(36)`|||  
|source_info|`nvarchar(4000)`|||  
|destination_info|`nvarchar(4000)`|||  
|.|`nvarchar(4000)`|||
|compute_pool_id|`int`|Identificatore univoco per il pool.|

## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
