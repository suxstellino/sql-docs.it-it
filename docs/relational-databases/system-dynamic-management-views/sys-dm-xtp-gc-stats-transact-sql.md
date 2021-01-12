---
description: sys.dm_xtp_gc_stats (Transact-SQL)
title: sys.dm_xtp_gc_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xtp_gc_stats
- dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_gc_stats dynamic management view
ms.assetid: 8287d611-50e3-43e1-ba8d-3e3793d3ba0e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a25b5e02c4a89bce85ae65575f80a588163074f7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096406"
---
# <a name="sysdm_xtp_gc_stats-transact-sql"></a>sys.dm_xtp_gc_stats (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Fornisce informazioni (statistiche generali) sul comportamento corrente del processo di Garbage Collection [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
 Le righe vengono sottoposte al Garbage Collection durante l'elaborazione delle transazioni regolare o dal thread principale di Garbage Collection a cui si fa riferimento come thread di lavoro inattivo. Quando viene eseguito il commit di una transazione utente, viene rimosso un elemento di lavoro dalla coda Garbage Collection ([sys.dm_xtp_gc_queue_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)). Le righe che sono state sottoposte a Garbage Collection ma senza accesso da parte della transazione principale dell'utente vengono sottoposte al Garbage Collection dal thread di lavoro inattivo, come parte dell'analisi di elementi nascosti (analisi per le aree dell'indice a cui si accede di meno).  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|rows_examined|**bigint**|Numero di righe esaminate dal sottosistema di Garbage Collection dal momento in cui è stato avviato il server.|  
|rows_no_sweep_needed|**bigint**|Numero di righe che sono state rimosse senza analisi di elementi nascosti.|  
|rows_first_in_bucket|**bigint**|Numero di righe esaminate dal processo di Garbage Collection che sono state le prime righe nel bucket di hash.|  
|rows_first_in_bucket_removed|**bigint**|Numero di righe esaminate dal processo di Garbage Collection che sono state le prime righe nel bucket di hash che sono state rimosse.|  
|rows_marked_for_unlink|**bigint**|Numero di righe esaminate dal processo di Garbage Collection che erano già contrassegnate come non collegate nei relativi indici con ref count =0.|  
|parallel_assist_count|**bigint**|Numero di righe elaborate dalle transazioni utente.|  
|idle_worker_count|**bigint**|Numero di righe di Garbage Collection elaborate dal thread di lavoro inattivo.|  
|sweep_scans_started|**bigint**|Numero di analisi di elementi nascosti eseguite dal sottosistema di Garbage Collection.|  
|sweep_scans_retries|**bigint**|Numero di analisi di elementi nascosti eseguite dal sottosistema di Garbage Collection.|  
|sweep_rows_touched|**bigint**|Righe lette dall'elaborazione degli elementi nascosti.|  
|sweep_rows_expiring|**bigint**|Righe in scadenza lette dall'elaborazione degli elementi nascosti.|  
|sweep_rows_expired|**bigint**|Righe scadute lette dall'elaborazione degli elementi nascosti.|  
|sweep_rows_expired_removed|**bigint**|Righe scadute rimosse dall'elaborazione degli elementi nascosti.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per l'istanza.  
  
## <a name="usage-scenario"></a>Scenario di utilizzo  
 Di seguito è riportato l'output campione:  
  
```  
rows_examined        rows_no_sweep_needed rows_first_in_bucket rows_first_in_bucket_removed  
280085               209512               69905  
rows_first_in_bucket_removed rows_marked_for_unlink parallel_assist_count idle_worker_count  
69905                        0                      8953  
  
idle_worker_count    sweep_scans_started  sweep_scan_retries   sweep_rows_touched  
10306473             670                  0                    1343  
  
sweep_rows_expiring  sweep_rows_expired   sweep_rows_expired_removed  
               0                 673673  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
