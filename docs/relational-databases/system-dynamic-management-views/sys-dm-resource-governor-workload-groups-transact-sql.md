---
description: sys.dm_resource_governor_workload_groups (Transact-SQL)
title: sys.dm_resource_governor_workload_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_workload_groups
- sys.dm_resource_governor_workload_groups_TSQL
- dm_resource_governor_workload_groups
- dm_resource_governor_workload_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_workload_groups dynamic management view
ms.assetid: f63c4914-1272-43ef-b135-fe1aabd953e0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9e8989568fdb55bb4db326799f4b824a4df7cb7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743301"
---
# <a name="sysdm_resource_governor_workload_groups-transact-sql"></a>sys.dm_resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce le statistiche del gruppo del carico di lavoro e la configurazione in memoria corrente del gruppo del carico di lavoro. Questa vista può essere unita a sys.dm_resource_governor_resource_pools per ottenere il nome del pool di risorse.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_resource_governor_workload_groups**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|group_id|**int**|ID del gruppo del carico di lavoro. Non ammette i valori Null.|  
|name|**sysname**|Nome del gruppo del carico di lavoro. Non ammette i valori Null.|  
|pool_id|**int**|ID del pool di risorse. Non ammette i valori Null.|  
|external_pool_id|**int**|**Si applica a**: a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .<br /><br /> ID del pool di risorse esterne. Non ammette i valori Null.|  
|statistics_start_time|**datetime**|Ora di reimpostazione della raccolta di statistiche per il gruppo del carico di lavoro. Non ammette i valori Null.|  
|total_request_count|**bigint**|Conteggio cumulativo delle richieste completate nel gruppo del carico di lavoro. Non ammette i valori Null.|  
|total_queued_request_count|**bigint**|Conteggio cumulativo delle richieste messe in coda dopo che il limite di GROUP_MAX_REQUESTS è stato raggiunto. Non ammette i valori Null.|  
|active_request_count|**int**|Conteggio corrente richieste. Non ammette i valori Null.|  
|queued_request_count|**int**|Conteggio corrente richieste in coda. Non ammette i valori Null.|  
|total_cpu_limit_violation_count|**bigint**|Conteggio cumulativo delle richieste superiore al limite della CPU. Non ammette i valori Null.|  
|total_cpu_usage_ms|**bigint**|Utilizzo cumulativo della CPU, in millisecondi, da parte di questo gruppo del carico di lavoro. Non ammette i valori Null.|  
|max_request_cpu_time_ms|**bigint**|Limite massimo di utilizzo della CPU, in millisecondi, per una singola richiesta. Non ammette i valori Null.<br /><br /> **Nota:** Si tratta di un valore misurato, a differenza di request_max_cpu_time_sec, che è un'impostazione configurabile. Per altre informazioni, vedere [Classe di evento CPU Threshold Exceeded](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|blocked_task_count|**int**|Conteggio corrente delle attività bloccate. Non ammette i valori Null.|  
|total_lock_wait_count|**bigint**|Conteggio cumulativo delle attese di blocco che si sono verificate. Non ammette i valori Null.|  
|total_lock_wait_time_ms|**bigint**|Somma cumulativa del tempo per cui viene mantenuto un blocco, espressa in millisecondi. Non ammette i valori Null.|  
|total_query_optimization_count|**bigint**|Conteggio cumulativo delle ottimizzazioni di query in questo gruppo del carico di lavoro. Non ammette i valori Null.|  
|total_suboptimal_plan_generation_count|**bigint**|Conteggio cumulativo delle generazioni di piani non ottimali che si sono verificate in questo gruppo del carico di lavoro a causa della richiesta di memoria. Non ammette i valori Null.|  
|total_reduced_memgrant_count|**bigint**|Conteggio cumulativo delle concessioni di memoria che hanno raggiunto il limite massimo di dimensioni delle query. Non ammette i valori Null.|  
|max_request_grant_memory_kb|**bigint**|Dimensioni della concessione massima di memoria, in kilobyte, di una singola richiesta a partire dal ripristino delle statistiche. Non ammette i valori Null.|  
|active_parallel_thread_count|**bigint**|Conteggio corrente dell'utilizzo di thread paralleli. Non ammette i valori Null.|  
|importance|**sysname**|Valore di configurazione corrente per l'importanza relativa di una richiesta in questo gruppo del carico di lavoro. L'importanza è una delle seguenti, con il valore predefinito medio, ovvero Low, medium o High.<br /><br /> Non ammette i valori Null.|  
|request_max_memory_grant_percent|**int**|Impostazione corrente per la concessione massima di memoria, espressa in percentuale, per una singola richiesta. Non ammette i valori Null.|  
|request_max_cpu_time_sec|**int**|Impostazione corrente per il limite massimo di utilizzo della CPU, espresso in secondi, per una singola richiesta. Non ammette i valori Null.|  
|request_memory_grant_timeout_sec|**int**|Impostazione corrente per il timeout di concessione di memoria, in secondi, per una singola richiesta. Non ammette i valori Null.|  
|group_max_requests|**int**|Impostazione corrente per il numero massimo di richieste simultanee. Non ammette i valori Null.|  
|max_dop|**int**|Massimo grado di parallelismo configurato per il gruppo del carico di lavoro. Il valore predefinito, 0, utilizza le impostazioni globali. Non ammette i valori Null.| 
|effective_max_dop|**int**|**Si applica a**: a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .<br /><br />Massimo grado di parallelismo effettivo per il gruppo del carico di lavoro. Non ammette i valori Null.| 
|total_cpu_usage_preemptive_ms|**bigint**|**Si applica a**: a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .<br /><br />Tempo totale della CPU usato in modalità preemptive per la pianificazione del gruppo di carico di lavoro, misurato in ms. Non ammette i valori Null.<br /><br />Per eseguire codice esterno a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio stored procedure estese e query distribuite, è necessario che un thread venga eseguito esternamente al controllo dell'utilità di pianificazione in modalità non preemptive. A tale scopo, un thread di lavoro passa alla modalità preemptive.| 
|request_max_memory_grant_percent_numeric|**float**|**Si applica a**: a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] .<br /><br />Impostazione corrente per la concessione massima di memoria, espressa in percentuale, per una singola richiesta. Non ammette i valori Null.| 
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="remarks"></a>Commenti  
 Questa vista a gestione dinamica mostra la configurazione in memoria. Per visualizzare i metadati di configurazione archiviati, utilizzare la sys.resource_governor_workload_groups &#40;vista del catalogo [&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md) .  
  
 Quando `ALTER RESOURCE GOVERNOR RESET STATISTICS` viene eseguito correttamente, vengono reimpostati i contatori seguenti: `statistics_start_time` ,, `total_request_count` `total_queued_request_count` , `total_cpu_limit_violation_count` , `total_cpu_usage_ms` , `max_request_cpu_time_ms` , `total_lock_wait_count` , `total_lock_wait_time_ms` , `total_query_optimization_count` , `total_suboptimal_plan_generation_count` , `total_reduced_memgrant_count` e `max_request_grant_memory_kb` . Il contatore `statistics_start_time` viene impostato sulla data e sull'ora correnti del sistema, mentre gli altri contatori vengono impostati su zero (0).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_resource_pools &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [sys.resource_governor_workload_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
