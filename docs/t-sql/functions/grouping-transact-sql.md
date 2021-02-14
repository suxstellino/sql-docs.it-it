---
description: GROUPING (Transact-SQL)
title: GROUPING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING
- GROUPING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], GROUPING function
- grouping columns
- ROLLUP operator
- GROUP BY clause, GROUPING function
- GROUPING function
- CUBE operator
ms.assetid: 4efa3868-1fc4-4626-8fb1-e863cc03e422
author: cawrites
ms.author: chadam
ms.openlocfilehash: 684e626cb1dac79ef29c838396bb4e855e3d0e23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338040"
---
# <a name="grouping-transact-sql"></a>GROUPING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Indica se un'espressione della colonna specificata in un elenco GROUP BY è aggregata. GROUPING restituisce 1 per le espressioni aggregate o 0 per le espressioni non aggregate nel set di risultati. È possibile usare GROUPING solo in un elenco \<select> SELECT e nelle clausole HAVING e ORDER BY quando GROUP BY è specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GROUPING ( <column_expression> )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 \<column_expression>  
 Colonna o espressione che contiene una colonna in una clausola [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 **tinyint**  
  
## <a name="remarks"></a>Osservazioni  
 GROUPING viene utilizzato per distinguere i valori Null restituiti da ROLLUP, CUBE o GROUPING SETS dai valori Null standard. Il risultato NULL restituito da un'operazione ROLLUP, CUBE o GROUPING SETS rappresenta un utilizzo particolare dei valori NULL. Funge infatti da segnaposto di colonna nel set di risultati e corrisponde a "tutti".  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono raggruppati gli importi di `SalesQuota` e aggregati gli importi di `SaleYTD` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La funzione `GROUPING` viene applicata alla colonna `SalesQuota`.  
  
```sql 
SELECT SalesQuota, SUM(SalesYTD) 'TotalSalesYTD', GROUPING(SalesQuota) AS 'Grouping'  
FROM Sales.SalesPerson  
GROUP BY SalesQuota WITH ROLLUP;  
GO  
```  
  
 Il set di risultati include due valori Null per `SalesQuota`. Il primo valore `NULL` rappresenta il gruppo di valori Null che derivano dalla colonna della tabella, il secondo valore `NULL` è incluso nella riga di riepilogo aggiunta dall'operazione ROLLUP. La riga di riepilogo indica gli importi della colonna `TotalSalesYTD` per tutti i gruppi `SalesQuota` ed è rappresentata dal valore `1` nella colonna `Grouping`.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 SalesQuota     TotalSalesYTD       Grouping  
------------   -----------------   --------  
NULL           1533087.5999          0  
250000.00      33461260.59           0  
300000.00      9299677.9445          0  
NULL           44294026.1344         1  

(4 row(s) affected)
```  
  
## <a name="see-also"></a>Vedere anche  
 [GROUPING_ID &#40;Transact-SQL&#41;](../../t-sql/functions/grouping-id-transact-sql.md)   
 [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
