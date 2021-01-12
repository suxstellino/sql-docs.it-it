---
description: sys.dm_pdw_component_health_active_alerts (Transact-SQL)
title: sys.dm_pdw_component_health_active_alerts (Transact-SQL
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c53e4a36-b841-424a-b8e2-255b1878deb6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: e73c8ea75b6a3613865eb16cd203b207452bfd20
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101411"
---
# <a name="sysdm_pdw_component_health_active_alerts-transact-sql"></a>sys.dm_pdw_component_health_active_alerts (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Archivia gli avvisi attivi sui [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] componenti.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Identificatore univoco di un [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] nodo.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|component_id|**int**|ID del componente. Vedere [sys.pdw_health_components &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|component_instance_id|**nvarchar(255)**|pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|alert_id|**int**|ID per il tipo di avviso. Vedere [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|alert_instance_id|**nvarchar (36)**|Identifica un'istanza di un determinato avviso.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id e alert_instance_id costituiscono la chiave per questa visualizzazione.|NOT NULL|  
|current_value|**nvarchar(255)**|Utilizzato quando l'avviso è di tipo StatusChange. Questo è lo stato del componente corrente. Il valore è NULL per gli avvisi di tipo soglia. Per un elenco dei tipi di avviso, vedere [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|previous_value|**nvarchar(255)**|Utilizzato quando l'avviso è di tipo StatusChange. Questo è lo stato del componente precedente. Il valore è NULL per gli avvisi di tipo soglia. Per un elenco dei tipi di avviso, vedere [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|create_time|**datetime**|Data e ora di generazione dell'avviso.|NOT NULL|  
  
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere "valori minimi e massimi" in [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
