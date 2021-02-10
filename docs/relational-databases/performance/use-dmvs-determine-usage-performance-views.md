---
title: DMV - Statistiche d'uso e prestazioni delle viste
description: Informazioni su come usare le viste a gestione dinamica (DMV) ys.dm_exec_query_optimizer_info, sys.views e sys.dmv_exec_cached_plans per ottenere le statistiche sulle prestazioni delle query SQL.
ms.custom: seo-dt-2019
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.date: 09/27/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.openlocfilehash: ed8611e945a6af654a0f1304ddee64b24af2efbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015401"
---
# <a name="use-dmvs-to-determine-usage-statistics-and-performance-of-views"></a>Usare DMV per determinare le statistiche di utilizzo e le prestazioni delle viste
Questo articolo illustra la metodologia e gli script usati per ottenere informazioni sulle **prestazioni delle query che usano viste**. La finalità di questi script è fornire indicatori dell'uso e delle prestazioni delle varie viste disponibili all'interno di un database. 

## <a name="sysdm_exec_query_optimizer_info"></a>sys.dm_exec_query_optimizer_info
La vista DMV [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) espone le statistiche sulle ottimizzazioni eseguite da Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questi valori sono cumulativi e la registrazione inizia all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni su Query Optimizer, vedere [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md).   

L'espressione di tabella comune (CTE, common_table_expression) seguente usa questa DMV per fornire informazioni sul carico di lavoro, ad esempio la percentuale di query che fanno riferimento a una vista. I risultati restituiti da questa query non indicano un problema di prestazioni da soli, ma possono esporre problemi sottostanti in combinazione con i reclami degli utenti per query lente. 

```sql
WITH CTE_QO AS
(
  SELECT
    occurrence
  FROM
    sys.dm_exec_query_optimizer_info
  WHERE
    ([counter] = 'optimizations')
),
QOInfo AS
(
  SELECT
    [counter]
    ,[%] = CAST((occurrence * 100.00)/(SELECT occurrence FROM CTE_QO) AS DECIMAL(5, 2))
  FROM
    sys.dm_exec_query_optimizer_info
  WHERE
    [counter] IN ('optimizations'
                  ,'trivial plan'
                  ,'no plan'
                  ,'search 0'
                  ,'search 1'
                  ,'search 2'
                  ,'timeout'
                  ,'memory limit exceeded'
                  ,'insert stmt'
                  ,'delete stmt'
                  ,'update stmt'
                  ,'merge stmt'
                  ,'contains subquery'
                  ,'view reference'
                  ,'remote query'
                  ,'dynamic cursor request'
                  ,'fast forward cursor request'
             )
)
SELECT
  [optimizations] AS [optimizations %]
  ,[trivial plan] AS [trivial plan %]
  ,[no plan] AS [no plan %]
  ,[search 0] AS [search 0 %]
  ,[search 1] AS [search 1 %]
  ,[search 2] AS [search 2 %]
  ,[timeout] AS [timeout %]
  ,[memory limit exceeded] AS [memory limit exceeded %]
  ,[insert stmt] AS [insert stmt %]
  ,[delete stmt] AS [delete stmt]
  ,[update stmt] AS [update stmt]
  ,[merge stmt] AS [merge stmt]
  ,[contains subquery] AS [contains subquery %]
  ,[view reference] AS [view reference %]
  ,[remote query] AS [remote query %]
  ,[dynamic cursor request] AS [dynamic cursor request %]
  ,[fast forward cursor request] AS [fast forward cursor request %]
FROM
  QOInfo
PIVOT (MAX([%]) FOR [counter] 
  IN ([optimizations]
      ,[trivial plan]
      ,[no plan]
      ,[search 0]
      ,[search 1]
      ,[search 2]
      ,[timeout]
      ,[memory limit exceeded]
      ,[insert stmt]
      ,[delete stmt]
      ,[update stmt]
      ,[merge stmt]
      ,[contains subquery]
      ,[view reference]
      ,[remote query]
      ,[dynamic cursor request]
      ,[fast forward cursor request])) AS p;
GO
```

Combinare i risultati di questa query con i risultati della vista di sistema [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md) per identificare le statistiche sulle query, il testo delle query e il piano di esecuzione memorizzato nella cache. 

## <a name="sysviews"></a>sys.views
L'espressione CTE seguente fornisce informazioni sul numero di esecuzioni, il tempo totale di esecuzione e le pagine lette dalla memoria. I risultati possono essere usati per identificare le query potenziali candidate per l'ottimizzazione. 
  
> [!NOTE]
> I risultati di questa query possono variare a seconda della versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  


```sql
WITH CTE_VW_STATS AS
(
  SELECT
    SCHEMA_NAME(vw.schema_id) AS schemaname
    ,vw.name AS viewname
    ,vw.object_id AS viewid
  FROM
    sys.views AS vw
  WHERE
    (vw.is_ms_shipped = 0)
  INTERSECT
  SELECT
    SCHEMA_NAME(o.schema_id) AS schemaname
    ,o.Name AS name
    ,st.objectid AS viewid
  FROM
    sys.dm_exec_cached_plans cp
  CROSS APPLY
    sys.dm_exec_sql_text(cp.plan_handle) st
  INNER JOIN
    sys.objects o ON st.[objectid] = o.[object_id]
  WHERE
    st.dbid = DB_ID()
)
SELECT
  vw.schemaname
  ,vw.viewname
  ,vw.viewid
  ,DB_NAME(t.databaseid) AS databasename
  ,t.databaseid
  ,t.*
FROM
  CTE_VW_STATS AS vw
CROSS APPLY
  (
    SELECT
      st.dbid AS databaseid
      ,st.text
      ,qp.query_plan
      ,qs.*
    FROM
      sys.dm_exec_query_stats AS qs
    CROSS APPLY
      sys.dm_exec_sql_text(qs.plan_handle) AS st
    CROSS APPLY
      sys.dm_exec_query_plan(qs.plan_handle) AS qp
    WHERE
      (CHARINDEX(vw.schemaname, st.text, 1) > 0)
      AND (st.dbid = DB_ID())
  ) AS t;
GO
```

## <a name="sysdmv_exec_cached_plans"></a>sys.dmv_exec_cached_plans
La query finale offre informazioni sulle viste inutilizzate usando la DMV [sys.dmv_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md). Tuttavia, la cache dei piani di esecuzione è dinamica e i risultati possono variare. Di conseguenza, usare questa query nel tempo per determinare se una vista viene effettivamente usata. 

```sql
SELECT
  SCHEMA_NAME(vw.schema_id) AS schemaname
  ,vw.name AS name
  ,vw.object_id AS viewid
FROM
  sys.views AS vw
WHERE
  (vw.is_ms_shipped = 0)
EXCEPT
SELECT
  SCHEMA_NAME(o.schema_id) AS schemaname
  ,o.name AS name
  ,st.objectid AS viewid
FROM
  sys.dm_exec_cached_plans cp
CROSS APPLY
  sys.dm_exec_sql_text(cp.plan_handle) st
INNER JOIN
  sys.objects o ON st.[objectid] = o.[object_id]
WHERE
  st.dbid = DB_ID();
GO
```

## <a name="see-also"></a>Vedere anche
[Viste e funzioni a gestione dinamica](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) 
