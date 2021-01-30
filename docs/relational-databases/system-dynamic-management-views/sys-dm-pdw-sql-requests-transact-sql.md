---
description: sys.dm_pdw_sql_requests (Transact-SQL)
title: sys.dm_pdw_sql_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 44e19609-902c-46cf-acdf-19ea75011365
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: fbb924edb6bd5d6b246ee986b7f600576a179a61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138815"
---
# <a name="sysdm_pdw_sql_requests-transact-sql"></a>sys.dm_pdw_sql_requests (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutte le distribuzioni di query SQL Server come parte di un passaggio SQL della query.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Identificatore univoco della query a cui appartiene questa distribuzione di query SQL.<br /><br /> request_id, step_index e distribution_id formano la chiave per questa visualizzazione.|Vedere request_id in [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Indice del passaggio della query di cui fa parte la distribuzione.<br /><br /> request_id, step_index e distribution_id formano la chiave per questa visualizzazione.|Vedere step_index in [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|pdw_node_id|**int**|Identificatore univoco del nodo in cui viene eseguita la distribuzione di query.|Vedere node_id in [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**int**|Identificatore univoco della distribuzione in cui viene eseguita la distribuzione di query.<br /><br /> request_id, step_index e distribution_id formano la chiave per questa visualizzazione.|Vedere distribution_id in [sys.pdw_distributions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md). Impostare su-1 per le richieste eseguite nell'ambito del nodo, non nell'ambito di distribuzione.|  
|status|**nvarchar(32)**|Stato corrente della distribuzione della query.|In sospeso, in esecuzione, non riuscito, annullato, completo, interrotto, CancelSubmitted|  
|error_id|**nvarchar (36)**|Identificatore univoco dell'errore associato a questa distribuzione di query, se disponibile.|Vedere error_id in [sys.dm_pdw_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). Impostare su NULL se non si è verificato alcun errore.|  
|start_time|**datetime**|Ora di inizio dell'esecuzione della distribuzione delle query.|Minore o uguale all'ora corrente e maggiore o uguale a start_time del passaggio della query a cui appartiene questa distribuzione di query|  
|end_time|**datetime**|Ora di completamento dell'esecuzione della distribuzione delle query, è stata annullata o non riuscita.|Maggiore o uguale all'ora di inizio oppure è impostato su NULL se la distribuzione delle query è in corso o in coda.|  
|total_elapsed_time|**int**|Rappresenta l'ora in cui è stata eseguita la distribuzione delle query, in millisecondi.|Maggiore o uguale a 0. Uguale al Delta di start_time e end_time per le distribuzioni di query completate, non riuscite o annullate.<br /><br /> Se total_elapsed_time supera il valore massimo per un numero intero, total_elapsed_time continuerà a essere il valore massimo. Questa condizione genererà l'avviso "è stato superato il valore massimo".<br /><br /> Il valore massimo in millisecondi equivale a 24,8 giorni.|  
|row_count|**bigint**|Numero di righe modificate o lette dalla distribuzione di query.|-1 per le operazioni che non modificano o restituiscono dati, ad esempio CREATE TABLE e DROP TABLE.|  
|spid|**int**|ID sessione nell'istanza SQL Server che esegue la distribuzione delle query.||  
|.|**nvarchar(4000)**|Testo completo del comando per la distribuzione di query.|Qualsiasi stringa di query o richiesta valida.|  
  
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere la sezione metadati nell'argomento [limiti di capacità](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
