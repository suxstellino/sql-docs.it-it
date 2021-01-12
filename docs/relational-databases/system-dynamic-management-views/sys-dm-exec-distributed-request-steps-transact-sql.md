---
description: sys.dm_exec_distributed_request_steps (Transact-SQL)
title: sys.dm_exec_distributed_request_steps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_distributed_request_steps
- sys.dm_exec_distributed_request_steps management view
ms.assetid: 1954541d-b716-4e03-8fcc-7022f428e01d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57ea8cc29caae50f91c8351bf72af494b000b178
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094081"
---
# <a name="sysdm_exec_distributed_request_steps-transact-sql"></a>sys.dm_exec_distributed_request_steps (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Include informazioni su tutti i passaggi che compongono una query o una richiesta di polibase specificata. Elenca una riga per ogni passaggio della query.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**int**|execution_id e step_index costituiscono la chiave per questa visualizzazione. ID numerico univoco associato alla richiesta.|Vedere ID in [sys.dm_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|**int**|Posizione di questo passaggio nella sequenza di passaggi che costituiscono la richiesta.|da 0 a (n-1) per una richiesta con n passaggi.|  
|operation_type|**nvarchar(128)**|Tipo di operazione rappresentata da questo passaggio.|' MoveOperation ',' OnOperation ',' RandomIDOperation ',' RemoteOperation ',' ReturnOperation ',' ShuffleMoveOperation ',' TempTablePropertiesOperation ',' DropDiagnosticsNotifyOperation ',' HadoopShuffleOperation ',' HadoopBroadCastOperation ',' HadoopRoundRobinOperation '|  
|distribution_type|**nvarchar(32)**|Posizione in cui è in esecuzione il passaggio.|' AllComputeNodes ',' AllDistributions ',' ComputeNode ',' Distribution ',' AllNodes ',' SubsetNodes ',' SubsetDistributions ',' Unspecified '.|  
|location_type|**nvarchar(32)**|Posizione in cui è in esecuzione il passaggio.|' Compute ',' Head ' o ' DMS '. Tutti i passaggi di spostamento dei dati mostrano "DMS".|  
|status|**nvarchar(32)**|Stato di questo passaggio|' Pending ',' running ',' complete ',' failed ',' UndoFailed ',' PendingCancel ',' annullato ',' Undone ',' Aborted '|  
|error_id|**nvarchar (36)**|ID univoco dell'errore associato a questo passaggio, se presente|Vedere ID di [sys.dm_exec_compute_node_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), null se non si è verificato alcun errore.|  
|start_time|**datetime**|Ora di inizio dell'esecuzione del passaggio|Minore o uguale all'ora corrente e maggiore o uguale a end_compile_time della query a cui appartiene questo passaggio.|  
|end_time|**datetime**|Ora di completamento dell'esecuzione di questo passaggio, annullato o non riuscito.|Minore o uguale all'ora corrente e maggiore o uguale a start_time, impostare su NULL per i passaggi attualmente in esecuzione o in coda.|  
|total_elapsed_time|**int**|Quantità totale di tempo di esecuzione del passaggio della query, in millisecondi|Compreso tra 0 e la differenza tra end_time e start_time. 0 per i passaggi in coda.|  
|row_count|**bigint**|Numero totale di righe modificate o restituite dalla richiesta|0 per i passaggi che non sono stati modificati o restituiscono dati, il numero di righe interessate in altro modo. Impostare su-1 per i passaggi DMS.|  
|.|nvarchar(4000)|Include il testo completo del comando di questo passaggio.|Qualsiasi stringa di richiesta valida per un passaggio. Troncato se è più lungo di 4000 caratteri.|  
  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
