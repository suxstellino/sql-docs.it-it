---
description: sys.dm_pdw_component_health_alerts (Transact-SQL)
title: sys.dm_pdw_component_health_alerts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 88f05392-1e97-4693-ba60-a4910af3c000
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 83a3a237da01eb677ef8282940b334fa28b42637
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093982"
---
# <a name="sysdm_pdw_component_health_alerts-transact-sql"></a>sys.dm_pdw_component_health_alerts (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Archivia gli avvisi rilasciati in precedenza sui componenti del dispositivo.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Identificatore univoco di un [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] nodo.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|component_id|**int**|ID del componente. Vedere [sys.pdw_health_components &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|component_instance_id|**nvarchar(255)**|pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|alert_id|**int**|ID per il tipo di avviso. Vedere [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|alert_instance_id|**nvarchar (36)**|Identifica un'istanza di un determinato avviso.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|previous_value|**nvarchar(255)**|Utilizzato quando l'avviso è di tipo StatusChange. Questo è lo stato del componente precedente. Il valore è NULL per gli avvisi di tipo soglia. Per un elenco dei tipi di avviso, vedere [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|current_value|**nvarchar(255)**|Utilizzato quando l'avviso è di tipo StatusChange. Questo è lo stato del componente corrente. Il valore è NULL per gli avvisi di tipo soglia. Per un elenco dei tipi di avviso, vedere [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|create_time|**datetime**|Data e ora di generazione dell'avviso.|NOT NULL|  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
