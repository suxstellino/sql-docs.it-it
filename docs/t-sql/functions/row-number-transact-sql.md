---
title: ROW_NUMBER (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione ROW_NUMBER. Questa funzione numera l'output di un set di risultati.
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROW_NUMBER
- ROW_NUMBER_TSQL
- ROW_NUMBER()_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ROW_NUMBER function
- row numbers [SQL Server]
- sequential row numbers [SQL Server]
ms.assetid: 82fa9016-77db-4b42-b4c8-df6095b81906
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29cd02108b56799e0e23ebf0da26f33df93e28d7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735667"
---
# <a name="row_number-transact-sql"></a>ROW_NUMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Numera l'output di un set di risultati. In particolare, restituisce il numero sequenziale di una riga all'interno di una partizione di un set di risultati, a partire da 1 per la prima riga di ogni partizione. 
  
`ROW_NUMBER` e `RANK` sono simili. `ROW_NUMBER` numera tutte le righe in sequenza (ad esempio 1, 2, 3, 4, 5). `RANK` fornisce lo stesso valore numerico per i valori equivalenti (ad esempio 1, 2, 2, 4, 5).   
  
> [!NOTE]
> `ROW_NUMBER` è un valore temporaneo calcolato al momento dell'esecuzione della query. Per mantenere i numeri in una tabella, vedere [Proprietà IDENTITY](../../t-sql/statements/create-table-transact-sql-identity-property.md) e [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ROW_NUMBER ( )   
    OVER ( [ PARTITION BY value_expression , ... [ n ] ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 PARTITION BY *value_expression*  
 Suddivide il set di risultati generato dalla clausola [FROM](../../t-sql/queries/from-transact-sql.md) in partizioni alle quali viene applicata la funzione ROW_NUMBER. *value_expression* specifica la colonna in base alla quale viene partizionato il set di risultati. Se `PARTITION BY` non viene specificato, la funzione considera tutte le righe del set di risultati della query come un unico gruppo. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 *order_by_clause*  
 La clausola `ORDER BY` determina la sequenza in base alla quale alle righe viene assegnato un valore univoco `ROW_NUMBER` all'interno di una partizione specificata. Questo argomento è obbligatorio. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 **bigint**  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Non esiste alcuna garanzia che le righe restituite da una query che usa `ROW_NUMBER()` vengano ordinate esattamente allo stesso modo a ogni esecuzione, a meno che le condizioni seguenti non siano vere.  
  
1.  Univocità dei valori della colonna partizionata.  
  
2.  Univocità dei valori delle colonne `ORDER BY`.  
  
3.  Univocità delle combinazioni di valori della colonna di partizione e delle colonne `ORDER BY`.  
  
 `ROW_NUMBER()` è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-examples"></a>R. Esempi semplici 

La query seguente restituisce le quattro tabelle di sistema in ordine alfabetico.

```sql
SELECT 
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5
ORDER BY name ASC;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|name    |recovery_model_desc |  
|-----------  |------------ |  
|master |SEMPLICE |
|model |FULL |
|msdb |SEMPLICE |
|tempdb |SEMPLICE |

Per aggiungere una colonna con i numeri di riga davanti a ogni riga, aggiungere una colonna con la funzione `ROW_NUMBER`, in questo caso denominata `Row#`. È necessario spostare in alto la clausola `ORDER BY`, accanto alla clausola `OVER`.

```sql
SELECT 
  ROW_NUMBER() OVER(ORDER BY name ASC) AS Row#,
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|N. di riga |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |master |SEMPLICE |
|2 |model |FULL |
|3 |msdb |SEMPLICE |
|4 |tempdb |SEMPLICE |

Se si aggiunge una clausola `PARTITION BY` nella colonna `recovery_model_desc`, la numerazione verrà riavviata quando il valore `recovery_model_desc` viene modificato. 
 
```sql
SELECT 
  ROW_NUMBER() OVER(PARTITION BY recovery_model_desc ORDER BY name ASC) 
    AS Row#,
  name, recovery_model_desc
FROM sys.databases WHERE database_id < 5;
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|N. di riga |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |model |FULL |
|1 |master |SEMPLICE |
|2 |msdb |SEMPLICE |
|3 |tempdb |SEMPLICE |


### <a name="b-returning-the-row-number-for-salespeople"></a>B. Restituzione del numero di riga per i venditori  
 Nell'esempio seguente viene calcolato un numero di riga per i venditori in [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] in base alla classificazione delle vendite dall'inizio dell'anno alla data corrente.  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT ROW_NUMBER() OVER(ORDER BY SalesYTD DESC) AS Row,   
    FirstName, LastName, ROUND(SalesYTD,2,1) AS "Sales YTD"   
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Row FirstName    LastName               SalesYTD  
--- -----------  ---------------------- -----------------  
1   Linda        Mitchell               4251368.54  
2   Jae          Pak                    4116871.22  
3   Michael      Blythe                 3763178.17  
4   Jillian      Carson                 3189418.36  
5   Ranjit       Varkey Chudukatil      3121616.32  
6   José         Saraiva                2604540.71  
7   Shu          Ito                    2458535.61  
8   Tsvi         Reiter                 2315185.61  
9   Rachel       Valdez                 1827066.71  
10  Tete         Mensa-Annan            1576562.19  
11  David        Campbell               1573012.93  
12  Garrett      Vargas                 1453719.46  
13  Lynn         Tsoflias               1421810.92  
14  Pamela       Ansman-Wolfe           1352577.13  
```  
  
### <a name="c-returning-a-subset-of-rows"></a>C. Restituzione di un subset di righe  
 Nell'esempio seguente vengono calcolati solo i numeri di riga per tutte le righe nella tabella `SalesOrderHeader` nell'ordine di `OrderDate` e vengono restituite le righe da `50` a `60` incluse.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH OrderedOrders AS  
(  
    SELECT SalesOrderID, OrderDate,  
    ROW_NUMBER() OVER (ORDER BY OrderDate) AS RowNumber  
    FROM Sales.SalesOrderHeader   
)   
SELECT SalesOrderID, OrderDate, RowNumber    
FROM OrderedOrders   
WHERE RowNumber BETWEEN 50 AND 60;  
```  
  
### <a name="d-using-row_number-with-partition"></a>D. Utilizzo di ROW_NUMBER() con PARTITION  
 Nell'esempio seguente viene utilizzato l'argomento `PARTITION BY` per suddividere il set di risultati della query in base alla colonna `TerritoryName`. La clausola `ORDER BY` specificata nella clausola `OVER` ordina le righe in ogni partizione in base alla colonna `SalesYTD`. La clausola `ORDER BY` nell'istruzione `SELECT` ordina l'intero set di risultati della query in base al valore di `TerritoryName`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FirstName, LastName, TerritoryName, ROUND(SalesYTD,2,1) AS SalesYTD,  
ROW_NUMBER() OVER(PARTITION BY TerritoryName ORDER BY SalesYTD DESC) 
  AS Row  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0  
ORDER BY TerritoryName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName  LastName             TerritoryName        SalesYTD      Row  
---------  -------------------- ------------------   ------------  ---  
Lynn       Tsoflias             Australia            1421810.92    1  
José       Saraiva              Canada               2604540.71    1  
Garrett    Vargas               Canada               1453719.46    2  
Jillian    Carson               Central              3189418.36    1  
Ranjit     Varkey Chudukatil    France               3121616.32    1  
Rachel     Valdez               Germany              1827066.71    1  
Michael    Blythe               Northeast            3763178.17    1  
Tete       Mensa-Annan          Northwest            1576562.19    1  
David      Campbell             Northwest            1573012.93    2  
Pamela     Ansman-Wolfe         Northwest            1352577.13    3  
Tsvi       Reiter               Southeast            2315185.61    1  
Linda      Mitchell             Southwest            4251368.54    1  
Shu        Ito                  Southwest            2458535.61    2  
Jae        Pak                  United Kingdom       4116871.22    1  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-returning-the-row-number-for-salespeople"></a>E. Restituzione del numero di riga per i venditori  
 L'esempio seguente restituisce il valore `ROW_NUMBER` per i venditori in base alle rispettive quote di vendita assegnate.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) 
    AS RowNumber,  
    FirstName, LastName,   
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 Set di risultati parziale:  

```  

RowNumber  FirstName  LastName            SalesQuota  
---------  ---------  ------------------  -------------  
1          Jillian    Carson              12,198,000.00  
2          Linda      Mitchell            11,786,000.00  
3          Michael    Blythe              11,162,000.00  
4          Jae        Pak                 10,514,000.00  
```

### <a name="f-using-row_number-with-partition"></a>F. Utilizzo di ROW_NUMBER() con PARTITION  
 Nell'esempio seguente viene illustrato l'utilizzo della funzione `ROW_NUMBER` con l'argomento `PARTITION BY`. Ciò determina la numerazione, da parte della funzione `ROW_NUMBER`, delle righe in ogni partizione.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(PARTITION BY SalesTerritoryKey 
        ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    LastName, SalesTerritoryKey AS Territory,  
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName, SalesTerritoryKey;  
```  
  
 Set di risultati parziale:  
 
```  
 
RowNumber  LastName            Territory  SalesQuota  
---------  ------------------  ---------  -------------  
1          Campbell            1           4,025,000.00  
2          Ansman-Wolfe        1           3,551,000.00  
3          Mensa-Annan         1           2,275,000.00  
1          Blythe              2          11,162,000.00  
1          Carson              3          12,198,000.00  
1          Mitchell            4          11,786,000.00  
2          Ito                 4           7,804,000.00  
```
  
## <a name="see-also"></a>Vedere anche  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/dense-rank-transact-sql.md)   
 [NTILE &#40;Transact-SQL&#41;](../../t-sql/functions/ntile-transact-sql.md)  
  
  


