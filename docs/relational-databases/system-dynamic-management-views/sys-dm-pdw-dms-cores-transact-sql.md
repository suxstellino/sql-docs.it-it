---
description: sys.dm_pdw_dms_cores (Transact-SQL)
title: sys.dm_pdw_dms_cores (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: b3f09b15-0863-4418-9347-a4f5fd2ab7c7
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 81a59915c6376e45ebcb0203593ea9725ac6a440
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482622"
---
# <a name="sysdm_pdw_dms_cores-transact-sql"></a>sys.dm_pdw_dms_cores (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutti i servizi DMS in esecuzione nei nodi di calcolo dell'appliance. Viene elencata una riga per ogni istanza del servizio, che è attualmente una riga per nodo.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|dms_core_id|**int**|ID numerico univoco associato a questo core DMS.<br /><br /> Chiave per questa visualizzazione.|Impostare sulla pdw_node_id del nodo su cui è in esecuzione questo core DMS.|  
|pdw_node_id|**int**|ID del nodo in cui è in esecuzione il servizio DMS.|Vedere node_id in [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|status|**nvarchar(32)**|Stato corrente del servizio DMS.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
  
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere la sezione metadati nell'argomento [limiti di capacità](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
