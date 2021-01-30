---
description: sys.dm_pdw_diag_processing_stats (Transact-SQL)
title: sys.dm_pdw_diag_processing_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: df659c55-4f63-45f8-8afe-ce300031bc5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 0164a7f806afcacc35af0e2bffd2c6cbe69ee762
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99144249"
---
# <a name="sysdm_pdw_diag_processing_stats-transact-sql"></a>sys.dm_pdw_diag_processing_stats (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Visualizza le informazioni relative a tutti gli eventi di diagnostica interni che possono essere incorporati nelle sessioni di diagnostica definite dall'amministratore. Eseguire una query su questa vista per comprendere le statistiche alla base dei sottosistemi di diagnostica e di eventi che determinano il popolamento di tutti gli altri DMV. È disponibile un gruppo di code per ogni processo in ogni nodo.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|Nodo del dispositivo da cui è presente il log.|  
|**process_id**|**int**|Identificatore del processo che esegue l'invio di questa statistica.|  
|**target_name**|**nvarchar(255)**|Nome della coda.|  
|**queue_size**|**int**|Numero di elementi nella coda dei processi. Le dimensioni della coda sono in genere pari a 0. Un numero positivo indica che il sistema è sottoposto a stress e sta creando un backlog di eventi. Un conteggio positivo nelle altre colonne significa che il sistema è stato danneggiato per la coda specifica e per eventuali DMV correlati.|  
|**lost_events_count**|**bigint**|Il numero di eventi persi.|  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
