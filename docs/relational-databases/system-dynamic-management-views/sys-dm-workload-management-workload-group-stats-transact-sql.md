---
description: sys.dm_workload_management_workload_groups_stats (Transact-SQL)
title: sys.dm_workload_management_workload_groups_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/02/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 01dbe51a5b82ea68ef1676876a9c0fa1633bf90c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99131464"
---
# <a name="sysdm_workload_management_workload_groups_stats-transact-sql"></a>sys.dm_workload_management_workload_groups_stats (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Restituisce le statistiche del gruppo di carico di lavoro e i valori effettivi del gruppo di carico di lavoro in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|group_id|**int**|ID univoco del gruppo del carico di lavoro.||
|name|**sysname**|Nome del gruppo del carico di lavoro.||
|statistics_start_time|**datetime**|Ora di inizio della raccolta delle statistiche per il gruppo del carico di lavoro.  Il valore è quando è stato creato il gruppo del carico di lavoro o quando l'istanza viene sospesa o ridimensionata.||
|total_request_count|**bigint**|Conteggio cumulativo delle richieste completate nel gruppo del carico di lavoro.||
|total_shared_resource_reqeusts|**bigint**|Conteggio cumulativo delle richieste completate nel gruppo del carico di lavoro che ha utilizzato risorse dal pool condiviso.||
|total_queued_request_count|**bigint**|Conteggio cumulativo delle richieste accodate dopo il raggiungimento del limite max_concurrency.||
|total_request_execution_timeouts|**bigint**|Conteggio cumulativo delle richieste nel gruppo del carico di lavoro che si è verificato il timeout prima del completamento in base all'impostazione query_execution_timeout_sec.||
|effective_min_percentage_resource|**tinyint**|L'impostazione min_percentage_resource efficace consentita in considerazione del livello di servizio e delle impostazioni del gruppo di carico di lavoro. Il min_percentage_resource effettivo può essere modificato in un livello superiore rispetto ai livelli di servizio più bassi.  In DW100c, ad esempio, il min_percentage_resource più basso consentito è pari al 25%.  Il min_percentage_resource viene modificato in 0% se il valore non può essere concesso a livello di servizio.  Ad esempio, min_percentage_resource impostato sul 10% in DW6000c, avrà un effective_min_percentage_resource pari allo 0% quando viene ridotto a DW100c.||
|effective_cap_percentage_resource|**tinyint**|Cap_percentage_resource efficace per il gruppo del carico di lavoro.  Se sono presenti altri gruppi di carico di lavoro con min_percentage_resource > 0, il effective_cap_percentage_resource viene ridotto proporzionalmente.||
|effective_request_min_resource_grant_percent|**Decimal (5, 2)**|Valore di runtime effettivo per request_min_resource_grant_percent del gruppo di carico di lavoro. Valore effettivo che prende in considerazione il livello di servizio e la modalità di configurazione del gruppo di carico di lavoro.  Se min_percentage_resource viene regolato a causa del livello di servizio, effective_request_min_resource_grant_percent verrà regolato di conseguenza.||
|effective_request_max_resource_grant_percent|**Decimal (5, 2)**|Valore di runtime effettivo per request_max_resource_grant_percent del gruppo di carico di lavoro considerando la configurazione di tutti i gruppi del carico di lavoro.||
|||||

## <a name="see-also"></a>Vedi anche

 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
