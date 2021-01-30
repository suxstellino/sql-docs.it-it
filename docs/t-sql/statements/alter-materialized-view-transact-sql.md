---
description: ALTER MATERIALIZED VIEW (Transact-SQL)
title: ALTER MATERIALIZED VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: reference
f1_keywords:
- ALTER_VIEW_TSQL
- ALTER VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], modifying
- views [SQL Server], modifying
- modifying views
- ALTER VIEW statement
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 6797bba26dfa81e3c369d03837fb6f4222003663
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204141"
---
# <a name="alter-materialized-view-transact-sql"></a>ALTER MATERIALIZED VIEW (Transact-SQL)  

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Modifica una vista materializzata creata in precedenza. ALTER VIEW non influisce sulle stored procedure o i trigger dipendenti e non comporta modifiche delle autorizzazioni.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER MATERIALIZED VIEW [ schema_name . ] view_name
{
      REBUILD | DISABLE
}
[;]
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti

 *schema_name*     
 Nome dello schema a cui appartiene la vista.  
  
 *view_name*     
 Vista materializzata da modificare.  
  
*REBUILD*   
Riattiva la vista materializzata.

*DISABLE*   
Sospende la manutenzione per la vista materializzata, mantenendo i metadati e le autorizzazioni.Tutte le query a fronte della vista materializzata in uno stato disabilitato vengono risolte in base alle tabelle sottostanti.
  
## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione ALTER per la tabella o la vista.
  
## <a name="examples"></a>Esempi

Questo esempio disabilita una vista materializzata e la mette in modalità sospensione.
  
```sql
ALTER MATERIALIZED VIEW My_Indexed_View DISABLE;  
```  
  
Questo esempio riattiva la vista materializzata ricostruendola.  
  
```sql
ALTER MATERIALIZED VIEW My_Indexed_View REBUILD;  
```  
  
## <a name="see-also"></a>Vedere anche

[Ottimizzazione delle prestazioni con vista materializzata](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](./create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] Viste](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Viste di sistema supportate in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Istruzioni T-SQL supportate in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)