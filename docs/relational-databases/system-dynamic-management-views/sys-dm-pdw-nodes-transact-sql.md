---
description: sys.dm_pdw_nodes (Transact-SQL)
title: sys.dm_pdw_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 93966909-d758-4d50-950b-f5066d104fa6
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: b6ba594d39222549d8f906d3846085afc5ec7bea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99140361"
---
# <a name="sysdm_pdw_nodes-transact-sql"></a>sys.dm_pdw_nodes (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutti i nodi di [!INCLUDE[ssAPS](../../includes/ssaps-md.md)] . Elenca una riga per nodo nell'appliance.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|ID numerico univoco associato al nodo.<br /><br /> Chiave per questa visualizzazione.|Univoco nell'appliance, indipendentemente dal tipo.|  
|tipo|**nvarchar(32)**|Tipo del nodo.|' COMPUTE ',' CONTROL ',' MANAGEMENT '|  
|name|**nvarchar(32)**|Nome logico del nodo.|Qualsiasi stringa di lunghezza appropriata.|  
|address|**nvarchar(32)**|Indirizzo IP del nodo.|Nel formato [0-255]. [0-255]. [0-255]. [0-255].|  
|is_passive|**int**|Indica se la macchina virtuale che esegue il nodo è in esecuzione nel server assegnato oppure è stata sottoposta a failover sul server di riserva.|0-la macchina virtuale del nodo è in esecuzione nel server originale.<br /><br /> una macchina virtuale a 1 nodo è in esecuzione nel server di riserva.|  
|region|**nvarchar(32)**|Area in cui è in esecuzione il nodo.|' PDW ',' HDINSIGHT '|  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
