---
description: sys.dm_pdw_wait_stats (Transact-SQL)
title: sys.dm_pdw_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: cfb8d905-c34f-44de-9574-dde81e170916
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 962841e0c8b40a2cd0443cc2d75eb2b11c21a4ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138563"
---
# <a name="sysdm_pdw_wait_stats-transact-sql"></a>sys.dm_pdw_wait_stats (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni correlate allo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stato del sistema operativo correlate alle istanze in esecuzione nei diversi nodi. Per un elenco dei tipi di attesa e la relativa descrizione, vedere [sys.dm_os_wait_stats](https://msdn.microsoft.com/library/ms179984\(v=sql.120\).aspx).  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|**pdw_node_id**|**int**|ID del nodo a cui si riferisce questa voce.||  
|**wait_name**|**nvarchar(255)**|Nome del tipo di attesa.||  
|**max_wait_time**|**bigint**|Tempo massimo di attesa per questo tipo di attesa.||  
|**request_count**|**bigint**|Numero di attese in attesa del tipo di attesa.||  
|**signal_time**|**bigint**|Differenza tra il momento in cui è stato rilevato il thread in attesa e quello in cui è stata avviata l'esecuzione del thread.||  
|**completed_count**|**bigint**|Numero totale di attese di questo tipo completate dopo l'ultimo riavvio del server.||  
|**wait_time**|**bigint**|Tempo di attesa totale per questo tipo di attesa in millisecons. Inclusione di signal_time.||  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)   
 [sys.dm_pdw_waits &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql.md)  
  
  
