---
description: SUM (Transact-SQL)
title: SUM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUM
- SUM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], sum of all
- SUM function [Transact-SQL]
- values [SQL Server]
- summation [SQL Server]
- expressions [SQL Server], sum of all values
- DISTINCT keyword
- totals [SQL Server], SUM
- summary values [SQL Server]
ms.assetid: 9af94d0f-55d4-428f-a840-ec530160f379
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d85ce8bbff1d00afffa0d50cf24e66cc98d5b3ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183477"
---
# <a name="sum-transact-sql"></a>SUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce la somma di tutti i valori o solo dei valori DISTINCT dell'espressione. La funzione SUM può essere utilizzata solo con colonne numeriche. I valori Null vengono ignorati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Aggregate Function Syntax    
SUM ( [ ALL | DISTINCT ] expression )  

-- Analytic Function Syntax   
SUM ([ ALL ] expression) OVER ( [ partition_by_clause ] order_by_clause)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 ALL  
 Applica la funzione di aggregazione a tutti i valori. Il valore predefinito è ALL.  
  
 DISTINCT  
 Specifica che l'istruzione SUM restituisce la somma dei soli valori univoci.  
  
 *expression*  
 Costante, colonna o funzione e qualsiasi combinazione di operatori aritmetici, bit per bit e stringa. *expression* è un'espressione della categoria dei tipi di dati numerici esatti o numerici approssimativi, ad eccezione del tipo di dati **bit**. Non è possibile utilizzare funzioni di aggregazione e sottoquery. Per altre informazioni, vedere [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* suddivide il set di risultati generato dalla clausola FROM in partizioni alle quali viene applicata la funzione. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo. _order\_by\_clause_ determina l'ordine logico in cui viene eseguita l'operazione. _order\_by\_clause_ è obbligatorio. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce la somma di tutti i valori dell'*espressione* nel tipo di dati *expression* più preciso.  
  
|Risultato dell'espressione|Tipo restituito|  
|-----------------------|-----------------|  
|**tinyint**|**int**|  
|**smallint**|**int**|  
|**int**|**int**|  
|**bigint**|**bigint**|  
|Categoria **decimal** (p, s)|**decimal(38, s)**|  
|Categoria **money** e **smallmoney**|**money**|  
|Categoria **float** e **real**|**float**|  
  
## <a name="remarks"></a>Osservazioni  
 SUM è una funzione deterministica quando viene utilizzato senza le clausole ORDER BY e OVER. Non è deterministica quando viene specificata con le clausole ORDER BY e OVER. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-sum-to-return-summary-data"></a>R. Utilizzo di SUM per restituire dati di riepilogo  
 Negli esempi seguenti viene mostrato l'uso della funzione SUM per restituire dati di riepilogo nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT Color, SUM(ListPrice), SUM(StandardCost)  
FROM Production.Product  
WHERE Color IS NOT NULL   
    AND ListPrice != 0.00   
    AND Name LIKE 'Mountain%'  
GROUP BY Color  
ORDER BY Color;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Color
--------------- --------------------- ---------------------
Black           27404.84              5214.9616
Silver          26462.84              14665.6792
White           19.00                 6.7926

(3 row(s) affected)
 ```  
  
### <a name="b-using-the-over-clause"></a>B. Utilizzo della clausola OVER  
 Nell'esempio seguente viene utilizzata la funzione SUM con la clausola OVER per fornire un totale cumulativo delle vendite annuali per ogni area presente nella tabella `Sales.SalesPerson` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. I dati vengono partizionati in base a `TerritoryID` e ordinati logicamente in base a `SalesYTD`. La funzione SUM viene pertanto calcolata per ogni area in base all'anno di vendita. Si noti che per `TerritoryID` 1, sono presenti due righe per l'anno di vendita 2005, a indicare due venditori con vendite in tale anno. Viene calcolato il totale delle vendite cumulative delle due righe e nel calcolo viene quindi inclusa la terza riga che rappresenta le vendite per l'anno 2006.  
  
```sql
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                           ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY TerritoryID,SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           559,697.56           559,697.56  
287              NULL        2006        519,905.93           539,801.75           1,079,603.50  
285              NULL        2007        172,524.45           417,375.98           1,252,127.95  
283              1           2005        1,573,012.94         1,462,795.04         2,925,590.07  
280              1           2005        1,352,577.13         1,462,795.04         2,925,590.07  
284              1           2006        1,576,562.20         1,500,717.42         4,502,152.27  
275              2           2005        3,763,178.18         3,763,178.18         3,763,178.18  
277              3           2005        3,189,418.37         3,189,418.37         3,189,418.37  
276              4           2005        4,251,368.55         3,354,952.08         6,709,904.17  
281              4           2005        2,458,535.62         3,354,952.08         6,709,904.17  
  
(10 row(s) affected)  
  
```  
  
 In questo esempio la clausola OVER non include PARTITION BY. La funzione verrà pertanto applicata a tutte le righe restituite dalla query. La clausola ORDER BY specificata nella clausola OVER determina l'ordine logico in base al quale viene applicata la funzione SUM. La query restituisce un totale cumulativo delle vendite annuali per tutte le aree di vendita specificate nella clausola WHERE. La clausola ORDER BY specificata nell'istruzione SELECT determina l'ordine in cui vengono visualizzate le righe della query.  
  
```sql
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           2,449,684.05         17,147,788.35  
275              2           2005        3,763,178.18         2,449,684.05         17,147,788.35  
276              4           2005        4,251,368.55         2,449,684.05         17,147,788.35  
277              3           2005        3,189,418.37         2,449,684.05         17,147,788.35  
280              1           2005        1,352,577.13         2,449,684.05         17,147,788.35  
281              4           2005        2,458,535.62         2,449,684.05         17,147,788.35  
283              1           2005        1,573,012.94         2,449,684.05         17,147,788.35  
284              1           2006        1,576,562.20         2,138,250.72         19,244,256.47  
287              NULL        2006        519,905.93           2,138,250.72         19,244,256.47  
285              NULL        2007        172,524.45           1,941,678.09         19,416,780.93  
(10 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-a-simple-sum-example"></a>C. Esempio di SUM semplice  
 L'esempio seguente restituisce il numero totale di ogni prodotto venduto nell'anno 2003.  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductKey, SUM(SalesAmount) AS TotalPerProduct  
FROM dbo.FactInternetSales  
WHERE OrderDateKey >= '20030101'   
      AND OrderDateKey < '20040101'  
GROUP BY ProductKey  
ORDER BY ProductKey;  
  
```  
  
 Set di risultati parziale:  
  
 ```
ProductKey  TotalPerProduct
----------  ---------------
214         31421.0200
217         31176.0900
222         29986.4300
225          7956.1500
 ```
  
### <a name="d-calculating-group-totals-with-more-than-one-column"></a>D. Calcolo dei totali di gruppi con più di una colonna  
 Nell'esempio seguente viene calcolata la somma di `ListPrice` e `StandardCost` per ogni colore incluso nella tabella `Product`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT Color, SUM(ListPrice)AS TotalList,   
       SUM(StandardCost) AS TotalCost  
FROM dbo.DimProduct  
GROUP BY Color  
ORDER BY Color;  
```  
  
 La prima parte del set di risultati è illustrata sotto:  
  
 ```
Color       TotalList      TotalCost
----------  -------------  --------------
Black       101295.7191    57490.5378
Blue         24082.9484    14772.0524
Grey           125.0000       51.5625
Multi          880.7468      526.4095
NA            3162.3564     1360.6185
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
  

