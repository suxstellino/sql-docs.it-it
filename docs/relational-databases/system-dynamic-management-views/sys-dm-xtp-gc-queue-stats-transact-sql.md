---
description: sys.dm_xtp_gc_queue_stats (Transact-SQL)
title: sys.dm_xtp_gc_queue_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
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
ms.assetid: addef774-318d-46a7-85df-f93168a800cb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 98d27fff17b897c7dfaba34b7afd2e21181a5cef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096444"
---
# <a name="sysdm_xtp_gc_queue_stats-transact-sql"></a>sys.dm_xtp_gc_queue_stats (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Informazioni di output su ogni coda di lavoro di Garbage Collection nel server e varie statistiche su ognuna. Esiste una coda per CPU logica.  
  
 Il thread principale di Garbage Collection (thread inattivo) tiene traccia delle righe aggiornate, eliminate e inserite per tutte le transazioni completate adll'ultima chiamata del thred principale di Garbage Collection. Il thread di Garbage Collection una volta attivato determina se il timestamp della transazione attiva meno recente è cambiato. Se la transazione attiva meno recente è cambiata, il thread inattivo accoda gli elementi di lavoro (in blocchi di 16 righe) delle transazioni i cui set di scrittura non sono più necessari. Ad esempio, se si eliminano 1.024 righe, verranno accodati 64 elementi di lavoro di Garbage Collection ciascuno contenente 16 righe eliminate.  Dopo il commit di una transazione utente, vengono selezionati tutti gli elementi accodati nell'utilità di pianificazione. Se non vi sono elementi accodati nell'utilità di pianificazione, la transazione utente cercherà in ogni coda del nodo NUMA corrente.  
  
 È possibile determinare se Garbage Collection libera la memoria delle righe eliminate eseguendo sys.dm_xtp_gc_queue_stats per vedere se il lavoro accodato viene eseguito. Se le voci nell'current_queue_depth non vengono elaborate o se non vengono aggiunti nuovi elementi di lavoro al current_queue_depth, questo indica che Garbage Collection non libera la memoria. Ad esempio, non è possibile eseguire Garbage Collection se è presente una transazione a esecuzione prolungata.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  

|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|queue_id|**int**|Identificatore univoco della coda.|  
|total_enqueues|**bigint**|Numero totale degli elementi di lavoro di Garbage Collection nella coda dall'avvio del server.|  
|total_dequeues|**bigint**|Numero totale degli elementi di lavoro di Garbage Collection rimossi dalla coda dall'avvio del server.|  
|current_queue_depth|**bigint**|Numero corrente di elementi di lavoro di Garbage Collection presenti nella coda. Questo elemento potrebbe implicare la raccolta di uno o più elementi da parte di Garbage Collection.|  
|maximum_queue_depth|**bigint**|Profondità massima della coda.|  
|last_service_ticks|**bigint**|Tick della CPU quando è stata servita la coda.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE.  
  
## <a name="user-scenario"></a>Scenario utente  
 In questo output viene mostrato che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione in 4 core oppure è stata creata un'affinità tra l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e 4 core:  
  
 Nell'output viene mostrato che non esistono elementi di lavoro nelle code da elaborare. Per la coda 0, gli elementi di lavoro totali rimossi dalla coda dall'avvio SQL sono 15625 e la profondità massima della coda è stata 15625.  
  
```  
queue_id total_enqueues total_dequeues current_queue_depth  maximum_queue_depth  last_service_ticks  
----------------------------------------------------------------------------------------------------  
0        15625                15625    0                    15625                1233573168347  
1        15625                15625    0                    15625                1234123295566  
2        15625                15625    0                    15625                1233569418146  
3        15625                15625    0                    15625                1233571605761  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
