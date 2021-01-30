---
description: RANK (Transact-SQL)
title: RANK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RANK
- RANK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row ranking [SQL Server]
- tied rows [SQL Server]
- ranking rows
- RANK function [Transact-SQL]
ms.assetid: 2d96f6d2-5db7-4b3c-a63e-213c58e4af55
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2f5e4dc064b32f2609b5c847e2e092ded993cf2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182333"
---
# <a name="rank-transact-sql"></a>RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce il rango di ogni riga all'interno della partizione di un set di risultati. Il valore di rango di una riga è uno più il numero di ranghi che precedono la riga in questione.  

ROW_NUMBER e RANK sono simili. ROW_NUMBER numera tutte le righe in sequenza (ad esempio 1, 2, 3, 4, 5). RANK fornisce lo stesso valore numerico per i valori equivalenti (ad esempio 1, 2, 2, 4, 5).   
  
> [!NOTE]
> RANK è un valore temporaneo calcolato al momento dell'esecuzione della query. Per mantenere i numeri in una tabella, vedere [Proprietà IDENTITY](../../t-sql/statements/create-table-transact-sql-identity-property.md) e [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). 
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
RANK ( ) OVER ( [ partition_by_clause ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* suddivide il set di risultati generato dalla clausola FROM in partizioni alle quali viene applicata la funzione. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo. _order\_by\_clause_ determina l'ordine dei dati prima che venga applicata la funzione. *order_by_clause* è obbligatorio. \<rows or range clause/> della clausola OVER non può essere specificato per la funzione RANK. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 **bigint**  
  
## <a name="remarks"></a>Osservazioni  
 Se due o più righe hanno un valore equivalente per un rango, ogni riga equivalente riceve lo stesso rango. Ad esempio, se due venditori principali hanno lo stesso valore SalesYTD, viene assegnato a entrambi il rango uno. Al venditore con il valore SalesYTD più alto successivo viene assegnato il rango tre, poiché sono presenti due righe di rango più elevato. Pertanto, la funzione RANK non restituirà sempre valori interi consecutivi.  
  
 L'ordinamento utilizzato per l'intera query determina l'ordinamento in base al quale vengono visualizzate le righe in un set di risultati,  
  
 RANK è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-ranking-rows-within-a-partition"></a>R. Classificazione di righe all'interno di una partizione  
 Nell'esempio seguente vengono assegnati i ranghi per i prodotti nelle ubicazioni di inventario specificate in base alle quantità. Il set di risultati viene partizionato da `LocationID` e ordinato logicamente in base al valore di `Quantity`. Si noti che la quantità dei prodotti 494 e 495 è la stessa. Poiché tali prodotti sono collegati, vengono entrambi classificati con uno.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT i.ProductID, p.Name, i.LocationID, i.Quantity  
    ,RANK() OVER   
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
ProductID   Name                   LocationID   Quantity Rank  
----------- ---------------------- ------------ -------- ----  
494         Paint - Silver         3            49       1  
495         Paint - Blue           3            49       1  
493         Paint - Red            3            41       3  
496         Paint - Yellow         3            30       4  
492         Paint - Black          3            17       5  
495         Paint - Blue           4            35       1  
496         Paint - Yellow         4            25       2  
493         Paint - Red            4            24       3  
492         Paint - Black          4            14       4  
494         Paint - Silver         4            12       5  
 (10 row(s) affected)  
```  
  
### <a name="b-ranking-all-rows-in-a-result-set"></a>B. Classificazione di tutte le righe in un set di risultati  
 Nell'esempio seguente vengono restituiti i primi dieci dipendenti classificati in base allo stipendio. Poiché non è specificata alcuna clausola PARTITION BY, la funzione RANK è stata applicata a tutte le righe nel set di risultati.  
  
```sql  
USE AdventureWorks2012  
SELECT TOP(10) BusinessEntityID, Rate,   
       RANK() OVER (ORDER BY Rate DESC) AS RankBySalary  
FROM HumanResources.EmployeePayHistory AS eph1  
WHERE RateChangeDate = (SELECT MAX(RateChangeDate)   
                        FROM HumanResources.EmployeePayHistory AS eph2  
                        WHERE eph1.BusinessEntityID = eph2.BusinessEntityID)  
ORDER BY BusinessEntityID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
BusinessEntityID Rate                  RankBySalary  
---------------- --------------------- --------------------  
1                125.50                1  
2                63.4615               4  
3                43.2692               8  
4                29.8462               19  
5                32.6923               16  
6                32.6923               16  
7                50.4808               6  
8                40.8654               10  
9                40.8654               10  
10               42.4808               9  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-ranking-rows-within-a-partition"></a>C: Classificazione di righe all'interno di una partizione  
 Nell'esempio seguente gli addetti alle vendite in ogni territorio di vendita vengono classificati in base al totale delle vendite. Il set di righe viene partizionato in base a `SalesTerritoryGroup` e ordinato in base a `SalesAmountQuota`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryRegion,  
    RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
LastName          TotalSales     SalesTerritoryRegion  RankResult
----------------  -------------  -------------------  --------
Tsoflias          1687000.0000   Australia            1
Saraiva           7098000.0000   Canada               1
Vargas            4365000.0000   Canada               2
Carson            12198000.0000  Central              1
Varkey Chudukatil 5557000.0000   France               1
Valdez            2287000.0000   Germany              1
Blythe            11162000.0000  Northeast            1
Campbell          4025000.0000   Northwest            1
Ansman-Wolfe      3551000.0000   Northwest            2
Mensa-Annan       2753000.0000   Northwest            3
Reiter            8541000.0000   Southeast            1
Mitchell          11786000.0000  Southwest            1
Ito               7804000.0000   Southwest            2
Pak               10514000.0000  United Kingdom       1
```  
  
## <a name="see-also"></a>Vedere anche  
 [DENSE_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/dense-rank-transact-sql.md)   
 [ROW_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/row-number-transact-sql.md)   
 [NTILE &#40;Transact-SQL&#41;](../../t-sql/functions/ntile-transact-sql.md)   
 [Funzioni di rango &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
  
  



