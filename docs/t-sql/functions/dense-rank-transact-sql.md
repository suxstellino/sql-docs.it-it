---
description: DENSE_RANK (Transact-SQL)
title: DENSE_RANK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENSE_RANK_TSQL
- DENSE_RANK
dev_langs:
- TSQL
helpviewer_keywords:
- row ranking [SQL Server]
- DENSE_RANK function
- tied rows [SQL Server]
- ranking rows
ms.assetid: 03871fc6-9592-4016-b0b2-ff543f132b20
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 555c01bb38d77632d64543f03f5647d671700f3b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752071"
---
# <a name="dense_rank-transact-sql"></a>DENSE_RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il rango di ogni riga all'interno della partizione di un set di risultati, senza gap nei valori di rango. Il rango di una riga specifica corrisponde a 1 più il numero di valori di rango distinti che precedono tale riga specifica.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
DENSE_RANK ( ) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 \<partition_by_clause>  
Prima suddivide il set di risultati generato dalla clausola [FROM](../../t-sql/queries/from-transact-sql.md) in partizioni e quindi la funzione `DENSE_RANK` viene applicata a ogni partizione. Vedere [Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md) per la sintassi di `PARTITION BY`.  
  
 \<order_by_clause>  
Determina l'ordine in cui la funzione `DENSE_RANK` viene applicata alle righe in una partizione.  
  
## <a name="return-types"></a>Tipi restituiti  
 **bigint**  
  
## <a name="remarks"></a>Osservazioni  
Se due o più righe hanno lo stesso valore di rango nella stessa partizione, ognuna di tali righe riceverà lo stesso rango. Se ad esempio due venditori principali hanno lo stesso valore SalesYTD, avranno entrambi il valore di rango 1. Il venditore con il valore successivo SalesYTD più alto avrà il valore di rango 2, che supera di uno il numero di righe distinte che precedono la riga in questione. I numeri restituiti dalla funzione `DENSE_RANK` sono quindi sempre valori consecutivi e senza gap.  
  
L'ordinamento usato per l'intera query determina l'ordine delle righe nel set di risultati. Ciò significa che una riga con numero di rango 1 non sarà necessariamente la prima riga nella partizione.  
  
`DENSE_RANK` è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-ranking-rows-within-a-partition"></a>R. Classificazione di righe all'interno di una partizione  
Questo esempio assegna i ranghi per i prodotti nell'inventario, in base alle ubicazioni di inventario specificate, a seconda delle quantità. `DENSE_RANK` partiziona il set di risultati per `LocationID` e ordina in modo logico il set di risultati per `Quantity`. Si noti che la quantità dei prodotti 494 e 495 è la stessa. Poiché hanno entrambi lo stesso valore relativo alla quantità, hanno entrambi il valore di rango 1.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT i.ProductID, p.Name, i.LocationID, i.Quantity  
    ,DENSE_RANK() OVER   
    (PARTITION BY i.LocationID ORDER BY i.Quantity DESC) AS Rank  
FROM Production.ProductInventory AS i   
INNER JOIN Production.Product AS p   
    ON i.ProductID = p.ProductID  
WHERE i.LocationID BETWEEN 3 AND 4  
ORDER BY i.LocationID;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductID   Name                               LocationID Quantity Rank  
----------- ---------------------------------- ---------- -------- -----  
494         Paint - Silver                     3          49       1  
495         Paint - Blue                       3          49       1  
493         Paint - Red                        3          41       2  
496         Paint - Yellow                     3          30       3  
492         Paint - Black                      3          17       4  
495         Paint - Blue                       4          35       1  
496         Paint - Yellow                     4          25       2  
493         Paint - Red                        4          24       3  
492         Paint - Black                      4          14       4  
494         Paint - Silver                     4          12       5  
  
(10 row(s) affected)  
  
```  
  
### <a name="b-ranking-all-rows-in-a-result-set"></a>B. Classificazione di tutte le righe in un set di risultati  
Questo esempio restituisce i primi dieci dipendenti classificati in base allo stipendio. Poiché l'istruzione `SELECT` non ha specificato una clausola `PARTITION BY`, la funzione `DENSE_RANK` è stata applicata a tutte le righe del set di risultati.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) BusinessEntityID, Rate,   
       DENSE_RANK() OVER (ORDER BY Rate DESC) AS RankBySalary  
FROM HumanResources.EmployeePayHistory;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Rate                  RankBySalary  
---------------- --------------------- --------------------  
1                125.50                1  
25               84.1346               2  
273              72.1154               3  
2                63.4615               4  
234              60.0962               5  
263              50.4808               6  
7                50.4808               6  
234              48.5577               7  
285              48.101                8  
274              48.101                8  
```  
  
## <a name="c-four-ranking-functions-used-in-the-same-query"></a>C. Quattro funzioni di rango usate nella stessa query  
Questo esempio illustra le quattro funzioni di rango

+ [DENSE_RANK()](./dense-rank-transact-sql.md)
+ [NTILE()](./ntile-transact-sql.md)
+ [RANK()](./rank-transact-sql.md)
+ [ROW_NUMBER()](./row-number-transact-sql.md)

usate nella stessa query. Per esempi specifici, vedere l'argomento relativo a ogni funzione di rango.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS Rank  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS Quartile  
    ,s.SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|FirstName|LastName|Row Number|Rank|Dense Rank|Quartile|SalesYTD|PostalCode|  
|---------------|--------------|----------------|----------|----------------|--------------|--------------|----------------|  
|Michael|Blythe|1|1|1|1|4557045.0459|98027|  
|Linda|Mitchell|2|1|1|1|5200475.2313|98027|  
|Jillian|Carson|3|1|1|1|3857163.6332|98027|  
|Garrett|Vargas|4|1|1|1|1764938.9859|98027|  
|Tsvi|Reiter|5|1|1|2|2811012.7151|98027|  
|Shu|Ito|6|6|2|2|3018725.4858|98055|  
|José|Saraiva|7|6|2|2|3189356.2465|98055|  
|David|Campbell|8|6|2|3|3587378.4257|98055|  
|Tete|Mensa-Annan|9|6|2|3|1931620.1835|98055|  
|Lynn|Tsoflias|10|6|2|3|1758385.926|98055|  
|Rachel|Valdez|11|6|2|4|2241204.0424|98055|  
|Jae|Pak|12|6|2|4|5015682.3752|98055|  
|Ranjit|Varkey Chudukatil|13|6|2|4|3827950.238|98055| 


## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-ranking-rows-within-a-partition"></a>D: Classificazione di righe all'interno di una partizione  
Questo esempio classifica gli addetti alle vendite in ogni territorio di vendita in base al totale delle vendite. `DENSE_RANK` partiziona il set di righe per `SalesTerritoryGroup` e ordina il set di risultati per `SalesAmountQuota`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryGroup,  
    DENSE_RANK() OVER (PARTITION BY SalesTerritoryGroup ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryGroup != N'NA'  
GROUP BY LastName, SalesTerritoryGroup;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
 LastName          TotalSales     SalesTerritoryGroup  RankResult  
----------------  -------------  -------------------  --------  
Pak               10514000.0000  Europe               1  
Varkey Chudukatil  5557000.0000  Europe               2  
Valdez             2287000.0000  Europe               3  
Carson            12198000.0000  North America        1  
Mitchell          11786000.0000  North America        2  
Blythe            11162000.0000  North America        3  
Reiter             8541000.0000  North America        4  
Ito                7804000.0000  North America        5  
Saraiva            7098000.0000  North America        6  
Vargas             4365000.0000  North America        7  
Campbell           4025000.0000  North America        8  
Ansman-Wolfe       3551000.0000  North America        9  
Mensa-Annan        2753000.0000  North America        10  
Tsoflias           1687000.0000  Pacific              1 
```  

## <a name="see-also"></a>Vedere anche  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [ROW_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/row-number-transact-sql.md)   
 [NTILE &#40;Transact-SQL&#41;](../../t-sql/functions/ntile-transact-sql.md)   
 [Funzioni di rango &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Funzioni](../../t-sql/functions/functions.md)  
  
  

