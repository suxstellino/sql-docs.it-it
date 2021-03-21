---
description: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
title: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD  (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 3c99b71fe0ba8a1a0bbfe1bcd929d4d6db093bdb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744441"
---
# <a name="dbcc-pdw_showmaterializedviewoverhead-transact-sql"></a>DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)  

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Visualizza il numero di modifiche incrementali nelle tabelle di base che vengono mantenute per le viste materializzate in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Il rapporto di overhead viene calcolato come TOTAL_ROWS / MAX (1, BASE_VIEW_ROWS).

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi

```syntaxsql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( " [ schema_name .] materialized_view_name  " )
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti

 *schema_name*     
 Nome dello schema a cui appartiene la vista.

*materialized_view_name*   
Nome della vista materializzata.

## <a name="remarks"></a>Commenti

Per mantenere aggiornate le viste materializzate in base alle modifiche ai dati nelle tabelle di base, il motore del data warehouse aggiunge righe di rilevamento a ogni vista interessata per riflettere le modifiche. La selezione da una vista materializzata include l'analisi dell'indice columnstore cluster della vista e l'applicazione di modifiche incrementali.Le righe di rilevamento (TOTAL_ROWS - BASE_VIEW_ROWS) non vengono eliminate finché gli utenti non ricompilano la vista materializzata.  

Il rapporto di overhead viene calcolato come TOTAL_ROWS/MAX(1, BASE_VIEW_ROWS).  Se è alto, le prestazioni di SELECT risulteranno ridotte.Gli utenti possono ricompilare la vista materializzata per ridurre il relativo rapporto di overhead.

## <a name="permissions"></a>Autorizzazioni  
  
È richiesta l'autorizzazione VIEW DATABASE STATE.  

## <a name="examples"></a>Esempi  

### <a name="a-this-example-returns-the-overhead-ratio-of-a-materialized-view"></a>R. Questo esempio restituisce il rapporto di overhead di una vista materializzata.

```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( "dbo.MyIndexedView" )
```

Output:

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|1234|1|3 |3.0 |

</br>

### <a name="b-this-example-shows-how-the-materialized-view-overhead-increases-as-data-changes-in-base-tables"></a>B. Questo esempio mostra come l'overhead della vista materializzata aumenta per effetto delle modifiche ai dati nelle tabelle di base

Creare una tabella
```sql
CREATE TABLE t1 (c1 INT NOT NULL, c2 INT NOT NULL, c3 INT NOT NULL)
```
Inserire cinque righe in t1
```sql
INSERT INTO t1 VALUES (1, 1, 1)
INSERT INTO t1 VALUES (2, 2, 2) 
INSERT INTO t1 VALUES (3, 3, 3) 
INSERT INTO t1 VALUES (4, 4, 4) 
INSERT INTO t1 VALUES (5, 5, 5) 
```
Creare viste materializzate MV1
```sql
CREATE MATERIALIZED VIEW MV1 
WITH (DISTRIBUTION = HASH(c1))  
AS
SELECT c1, COUNT(*) total_number 
FROM dbo.t1 WHERE c1 < 3
GROUP BY c1  
```
La selezione dalla vista materializzata restituisce due righe.

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

Controllare l'overhead della vista materializzata prima di eventuali modifiche ai dati nella tabella di base.
```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Output:

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1.00000000000000000 |

Aggiornare la tabella di base.  Questa query aggiorna 100 volte la stessa colonna nella stessa riga impostando lo stesso valore.  Il contenuto della vista materializzata non cambia.
```sql
DECLARE @p INT
SELECT @p = 1
WHILE (@p < 101)
BEGIN
UPDATE t1 SET c1 = 1 WHERE c1 = 1
SELECT @p = @p+1
END  
```

La selezione dalla vista materializzata restituisce lo stesso risultato dell'esempio precedente.  

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

Di seguito è riportato l'output di DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1").  Alla vista materializzata sono state aggiunte 100 righe (total_row - base_view_rows) e il relativo valore di overhead_ratio è aumentato. 

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|102 |51.00000000000000000 |

Dopo la ricompilazione della vista materializzata, vengono eliminate tutte le righe di rilevamento per le modifiche incrementali ai dati e il rapporto di overhead della vista viene ridotto.  

```sql
ALTER MATERIALIZED VIEW dbo.MV1 REBUILD
GO
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Output

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1.00000000000000000 |

## <a name="see-also"></a>Vedere anche

[Ottimizzazione delle prestazioni con vista materializzata](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Viste di sistema supportate in Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Istruzioni T-SQL supportate in Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)