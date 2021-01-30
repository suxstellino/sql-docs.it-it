---
description: sys.pdw_distributions (Transact-SQL)
title: sys.pdw_distributions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 572b5187-9753-4063-adf8-65dea87d11f8
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: bc73b551c49ae7d568adec7ff84fbbe3c529ea45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199293"
---
# <a name="syspdw_distributions-transact-sql"></a>sys.pdw_distributions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni sulle distribuzioni nell'appliance. Elenca una riga per ogni distribuzione di appliance.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|distribution_id|**int**|ID numerico univoco associato alla distribuzione.<br /><br /> Chiave per questa visualizzazione.|1 per il numero di nodi di calcolo nell'appliance moltiplicato per il numero di distribuzioni per ogni nodo di calcolo.|  
|pdw_node_id|**int**|ID del nodo in cui si trova questa distribuzione.|Vedere pdw_node_id in [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|name|**nvarchar(32)**|Identificatore di stringa associato alla distribuzione, utilizzato come suffisso per le tabelle distribuite.|Stringa composta da' A-Z ', da' a-z ', da' 0-9', da' _', da'-'.|  
|position|**int**|Posizione della distribuzione all'interno di un nodo corrispondente ad altre distribuzioni in tale nodo.|1 al numero di distribuzioni per ogni nodo.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
