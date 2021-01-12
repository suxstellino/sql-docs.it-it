---
description: COUNT (Transact-SQL)
title: COUNT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COUNT_TSQL
- COUNT
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT function
- totals [SQL Server]
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT function [Transact-SQL]
ms.assetid: 28d39da6-bc2e-46c7-858c-b1721c938830
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ac784713b7a74eb08acdfa9478b519c0b80940c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101299"
---
# <a name="count-transact-sql"></a>COUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il numero di elementi presenti in un gruppo. Il funzionamento di `COUNT` è analogo a quello della funzione [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md). Queste funzioni differiscono solo per i tipi di dati dei valori restituiti. `COUNT` restituisce sempre un valore con tipo di dati **int**. `COUNT_BIG` restituisce sempre un valore con tipo di dati **bigint**.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT ( { [ [ ALL | DISTINCT ] expression ] | * } )  

-- Analytic Function Syntax  
COUNT ( [ ALL ]  { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
**ALL**  
Applica la funzione di aggregazione a tutti i valori. ALL funge da valore predefinito.
  
DISTINCT  
Specifica che `COUNT` restituisce il numero di valori univoci non Null.
  
*expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di qualsiasi tipo, a eccezione di **image**, **ntext** o **text**. Si noti che `COUNT` non supporta le funzioni di aggregazione o le sottoquery in un'espressione.
  
\*  
Specifica che `COUNT` deve conteggiare tutte le righe per determinare il numero totale delle righe della tabella da restituire. `COUNT(*)` non accetta parametri e non supporta l'uso di DISTINCT. `COUNT(*)` non richiede un parametro *expression* perché per definizione non usa informazioni relative a colonne particolari. `COUNT(*)` restituisce il numero di righe di una tabella specificata e mantiene le righe duplicate. Ogni riga viene contata separatamente, incluse le righe contenenti valori Null.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] [ *ROW_or_RANGE_clause* ] **)**  
*partition_by_clause* divide il set di risultati generato dalla clausola `FROM` in partizioni alle quali viene applicata la funzione `COUNT`. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo. *order_by_clause* determina l'ordine logico dell'operazione. Per altre informazioni, vedere [Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md). 

## <a name="return-types"></a>Tipi restituiti
 **int**  
  
## <a name="remarks"></a>Osservazioni  
La funzione COUNT(\*) restituisce il numero di elementi di un gruppo, inclusi valori NULL e duplicati.
  
COUNT(ALL *expression*) valuta *expression* per ogni riga in un gruppo e restituisce il numero di valori non Null.
  
COUNT(DISTINCT *expression*) valuta *expression* per ogni riga in un gruppo e restituisce il numero di valori univoci non Null.
  
Per i valori restituiti che superano 2^31-1, `COUNT` restituisce un errore. Per questi casi, usare invece `COUNT_BIG`.
  
`COUNT` è una funzione deterministica quando viene usata ***senza** _ le clausole ORDER BY e OVER. Non è deterministica quando viene usata _*_con_*_ le clausole ORDER BY e OVER. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-count-and-distinct"></a>R. Utilizzo della funzione COUNT e dell'opzione DISTINCT  
Questo esempio restituisce il numero di titoli diversi che può avere un dipendente di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(DISTINCT Title)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67  
  
(1 row(s) affected)
```
  
### <a name="b-using-count_"></a>B. Uso di COUNT(\_)  
Questo esempio restituisce il numero totale di dipendenti di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(*)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
290  
  
(1 row(s) affected)
```
  
### <a name="c-using-count-with-other-aggregates"></a>C. Uso di COUNT(\*) con altre aggregazioni  
Questo esempio illustra che `COUNT(*)` può essere usata con altre funzioni di aggregazione nell'elenco `SELECT`. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT COUNT(*), AVG(Bonus)  
FROM Sales.SalesPerson  
WHERE SalesQuota > 25000;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- ---------------------
14            3472.1428
  
(1 row(s) affected)
```
  
### <a name="d-using-the-over-clause"></a>D. Utilizzo della clausola OVER  
Questo esempio usa le funzioni `MIN`, `MAX`, `AVG` e `COUNT` con la clausola `OVER` per restituire valori aggregati per ogni reparto nella tabella `HumanResources.Department` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT DISTINCT Name  
       , MIN(Rate) OVER (PARTITION BY edh.DepartmentID) AS MinSalary  
       , MAX(Rate) OVER (PARTITION BY edh.DepartmentID) AS MaxSalary  
       , AVG(Rate) OVER (PARTITION BY edh.DepartmentID) AS AvgSalary  
       ,COUNT(edh.BusinessEntityID) OVER (PARTITION BY edh.DepartmentID) AS EmployeesPerDept  
FROM HumanResources.EmployeePayHistory AS eph  
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
     ON eph.BusinessEntityID = edh.BusinessEntityID  
JOIN HumanResources.Department AS d  
ON d.DepartmentID = edh.DepartmentID
WHERE edh.EndDate IS NULL  
ORDER BY Name;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Name                          MinSalary             MaxSalary             AvgSalary             EmployeesPerDept  
----------------------------- --------------------- --------------------- --------------------- ----------------  
Document Control              10.25                 17.7885               14.3884               5  
Engineering                   32.6923               63.4615               40.1442               6  
Executive                     39.06                 125.50                68.3034               4  
Facilities and Maintenance    9.25                  24.0385               13.0316               7  
Finance                       13.4615               43.2692               23.935                10  
Human Resources               13.9423               27.1394               18.0248               6  
Information Services          27.4038               50.4808               34.1586               10  
Marketing                     13.4615               37.50                 18.4318               11  
Production                    6.50                  84.1346               13.5537               195  
Production Control            8.62                  24.5192               16.7746               8  
Purchasing                    9.86                  30.00                 18.0202               14  
Quality Assurance             10.5769               28.8462               15.4647               6  
Research and Development      40.8654               50.4808               43.6731               4  
Sales                         23.0769               72.1154               29.9719               18  
Shipping and Receiving        9.00                  19.2308               10.8718               6  
Tool Design                   8.62                  29.8462               23.5054               6  
  
(16 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-count-and-distinct"></a>E. Utilizzo della funzione COUNT e dell'opzione DISTINCT  
Questo esempio restituisce il numero di titoli diversi che può avere un dipendente di una specifica società.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(DISTINCT Title)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67
```  
  
### <a name="f-using-count"></a>F. Uso di COUNT(\*)  
Questo esempio restituisce il numero totale di righe della tabella `dbo.DimEmployee`.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(*)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-------------
296
```  
  
### <a name="g-using-count-with-other-aggregates"></a>G. Uso di COUNT(\*) con altre aggregazioni  
Questo esempio combina `COUNT(*)` con altre funzioni di aggregazione nell'elenco `SELECT`. Restituisce il numero di agenti di vendita con una quota di vendite annua maggiore di $ 500.000 e la quota di vendite media di tali agenti di vendita.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(EmployeeKey) AS TotalCount, AVG(SalesAmountQuota) AS [Average Sales Quota]  
FROM dbo.FactSalesQuota  
WHERE SalesAmountQuota > 500000 AND CalendarYear = 2001;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
TotalCount  Average Sales Quota
----------  -------------------
10          683800.0000
```
  
### <a name="h-using-count-with-having"></a>H. Uso di COUNT con HAVING  
Questo esempio usa `COUNT` con la clausola `HAVING` per restituire i reparti di una società, ognuno dei quali ha più di 15 dipendenti.
  
```sql
USE ssawPDW;  
  
SELECT DepartmentName,   
       COUNT(EmployeeKey)AS EmployeesInDept  
FROM dbo.DimEmployee  
GROUP BY DepartmentName  
HAVING COUNT(EmployeeKey) > 15;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
DepartmentName  EmployeesInDept
--------------  ---------------
Sales           18
Production      179
```
  
### <a name="i-using-count-with-over"></a>I. Uso di COUNT con OVER  
Questo esempio usa `COUNT` con la clausola `OVER` per restituire il numero di prodotti presenti in ognuno degli ordini di vendita specificati.
  
```sql
USE ssawPDW;  
  
SELECT DISTINCT COUNT(ProductKey) OVER(PARTITION BY SalesOrderNumber) AS ProductCount  
    ,SalesOrderNumber  
FROM dbo.FactInternetSales  
WHERE SalesOrderNumber IN (N'SO53115',N'SO55981');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
ProductCount   SalesOrderID`
------------   -----------------
3              SO53115
1              SO55981
```
  
## <a name="see-also"></a>Vedere anche
[Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/count-big-transact-sql.md)  
[Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  


