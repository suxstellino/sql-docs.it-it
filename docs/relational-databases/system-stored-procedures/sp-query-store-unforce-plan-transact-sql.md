---
description: sp_query_store_unforce_plan (Transact-SQL)
title: sp_query_store_unforce_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SP_QUERY_STORE_UNFORCE_PLAN_TSQL
- SP_QUERY_STORE_UNFORCE_PLAN
- SYS.SP_QUERY_STORE_UNFORCE_PLAN
- SYS.SP_QUERY_STORE_UNFORCE_PLAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_query_store_unforce_plan
- sp_query_store_unforce_plan
ms.assetid: a52f91d0-ff1e-46ad-ba36-b32d9623c9ab
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a53c054c80a4e7fc05b50ee9b4d9310dbeb75ab0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185727"
---
# <a name="sp_query_store_unforce_plan-transact-sql"></a>sp_query_store_unforce_plan (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Consente di forzare un piano precedentemente forzato per una query specifica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_query_store_unforce_plan [ @query_id = ] query_id , [ @plan_id = ] plan_id [;]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @query_id = ] query_id` ID della query. *query_id* è di tipo **bigint** e non prevede alcun valore predefinito.  
  
`[ @plan_id = ] plan_id` ID del piano di query che non verrà più applicato. *plan_id* è di tipo **bigint** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **ALTER** per il database.
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sulle query nell'archivio query.  
  
```sql  
SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*  
FROM sys.query_store_plan AS Pl  
JOIN sys.query_store_query AS Qry  
    ON Pl.query_id = Qry.query_id  
JOIN sys.query_store_query_text AS Txt  
    ON Qry.query_text_id = Txt.query_text_id ;  
```  
  
 Dopo aver identificato il query_id e plan_id che si desidera disforzare, utilizzare l'esempio seguente per disforzare il piano.  
  
```sql  
EXEC sp_query_store_unforce_plan 3, 3;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_query_store_force_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_remove_query &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [Viste del catalogo di Query Store &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Monitoraggio delle prestazioni tramite il Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Procedure consigliate per l'archivio query](../../relational-databases/performance/best-practice-with-the-query-store.md#CheckForced)     
  
