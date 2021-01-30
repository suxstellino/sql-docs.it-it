---
description: sys.dm_pdw_dms_workers (Transact-SQL)
title: sys.dm_pdw_dms_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0a284d18-3c46-4ffa-bcc9-689e660ee8b4
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 735ab65bc39b4f4ffab93f1b2bf8a705c3d895c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99143264"
---
# <a name="sysdm_pdw_dms_workers-transact-sql"></a>sys.dm_pdw_dms_workers (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutti i thread di lavoro che completano i passaggi DMS.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Query di cui fa parte questo ruolo di lavoro DMS.<br /><br /> request_id, step_index e dms_step_index formano la chiave per questa visualizzazione.|Vedere request_id in [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Passaggio della query di cui fa parte questo ruolo di lavoro DMS.<br /><br /> request_id, step_index e dms_step_index formano la chiave per questa visualizzazione.|Vedere step_index in [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|Eseguire un passaggio nel piano DMS in cui è in esecuzione il thread di lavoro.<br /><br /> request_id, step_index e dms_step_index formano la chiave per questa visualizzazione.||  
|pdw_node_id|**int**|Nodo su cui è in esecuzione il ruolo di lavoro.|Vedere node_id in [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**Int**|Distribuzione in cui il thread di lavoro è in esecuzione, se disponibile.|Vedere distribution_id in [sys.pdw_distributions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md).|  
|tipo|**nvarchar(32)**|Tipo di thread di lavoro DMS rappresentato da questa voce.|' DIRECT_CONVERTER ',' DIRECT_READER ',' FILE_READER ',' HASH_CONVERTER ',' HASH_READER ',' ROUNDROBIN_CONVERTER ',' EXPORT_READER ',' EXTERNAL_READER ',' EXTERNAL_WRITER ',' PARALLEL_COPY_READER ',' REJECT_WRITER ',' WRITER '|  
|status|**nvarchar(32)**|Stato del ruolo di lavoro DMS.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|bytes_per_sec|**bigint**|Velocità effettiva di lettura o scrittura nell'ultimo secondo.|Maggiore o uguale a 0. NULL se la query è stata annullata o non riuscita prima dell'esecuzione del thread di lavoro.|  
|bytes_processed|**bigint**|Byte totali elaborati da questo thread di lavoro.|Maggiore o uguale a 0. NULL se la query è stata annullata o non riuscita prima dell'esecuzione del thread di lavoro.|  
|rows_processed|**bigint**|Numero di righe lette o scritte per questo ruolo di lavoro.|Maggiore o uguale a 0. NULL se la query è stata annullata o non riuscita prima dell'esecuzione del thread di lavoro.|  
|start_time|**datetime**|Ora di inizio dell'esecuzione del thread di lavoro.|Maggiore o uguale all'ora di inizio del passaggio della query a cui appartiene il ruolo di lavoro. Vedere [sys.dm_pdw_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Ora in cui l'esecuzione è terminata, non riuscita o è stata annullata.|NULL per i ruoli di lavoro in corso o in coda. In caso contrario, maggiore di start_time.|  
|total_elapsed_time|**int**|Tempo totale impiegato per l'esecuzione, in millisecondi.|Maggiore o uguale a 0.<br /><br /> Tempo totale trascorso dall'avvio o dal riavvio del sistema. Se total_elapsed_time supera il valore massimo per un numero intero (24,8 giorni in millisecondi), si verificherà un errore di materializzazione causato da un overflow.<br /><br /> Il valore massimo in millisecondi equivale a 24,8 giorni.|  
|cpu_time|**bigint**|Tempo di CPU utilizzato dal thread di lavoro, in millisecondi.|Maggiore o uguale a 0.|  
|query_time|**int**|Periodo di tempo prima che SQL inizi a restituire righe al thread, in millisecondi.|Maggiore o uguale a 0.|  
|buffers_available|**int**|Numero di buffer inutilizzati.| NULL se la query è stata annullata o non riuscita prima dell'esecuzione del thread di lavoro.|  
|sql_spid|**int**|ID sessione nell'istanza SQL Server che esegue il lavoro per questo ruolo di lavoro DMS.||  
|dms_cpid|**int**|ID processo del thread effettivo che esegue.||  
|error_id|**nvarchar (36)**|Identificatore univoco dell'errore che si è verificato durante l'esecuzione del processo di lavoro, se presente.|Vedere error_id in [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|source_info|**nvarchar(4000)**|Per un Reader, specifica delle tabelle e delle colonne di origine.||  
|destination_info|**nvarchar(4000)**|Per un writer, specifica delle tabelle di destinazione.||  
  
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere la sezione metadati nell'argomento [limiti di capacità](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
