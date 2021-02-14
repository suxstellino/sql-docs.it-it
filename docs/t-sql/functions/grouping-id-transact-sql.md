---
description: GROUPING_ID (Transact-SQL)
title: GROUPING_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING_ID_TSQL
- GROUPING_ID
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, GROUPING_ID
- GROUPING_ID function
ms.assetid: c1050658-b19f-42ee-9a05-ecd6a73b896c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 828a4c0e33722a22aae4779c7197a7630578a438
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338024"
---
# <a name="grouping_id-transact-sql"></a>GROUPING_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  È una funzione che calcola il livello di raggruppamento. È possibile usare GROUPING_ID solo in un elenco \<select> di SELECT o nelle clausole HAVING o ORDER BY quando è specificato GROUP BY.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GROUPING_ID ( <column_expression>[ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 \<column_expression>  
 *column_expression* in una clausola [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md).  
  
## <a name="return-type"></a>Tipo restituito  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 GROUPING_ID \<column_expression> deve corrispondere esattamente all'espressione presente nell'elenco GROUP BY. Ad esempio, se si esegue il raggruppamento per DATEPART (yyyy, \<*column name*>), usare GROUPING_ID (DATEPART (yyyy, \<*column name*>)). Se invece si esegue il raggruppamento per \<*column name*>, usare GROUPING_ID (\<*column name*>).  
  
## <a name="comparing-grouping_id--to-grouping-"></a>Confronto tra GROUPING_ID () e GROUPING ()  
 GROUPING_ID (\<column_expression> [ **,** ...*n* ]) immette l'equivalente del valore GROUPING (\<column_expression>) restituito per ogni colonna nell'elenco di colonne in ogni riga di output come stringa di valori uno e zero. GROUPING_ID interpreta tale stringa come numero in base 2 e restituisce il valore intero equivalente. Si consideri ad esempio l'istruzione seguente: `SELECT a, b, c, SUM(d),``GROUPING_ID(a,b,c)``FROM T GROUP BY <group by list>`. Nella tabella seguente vengono mostrati i valori di input e output di GROUPING_ID ().  
  
|Colonne aggregate|Input di GROUPING_ID (a, b, c) = GROUPING (a) + GROUPING (b) + GROUPING (c)|Output di GROUPING_ID ()|  
|------------------------|---------------------------------------------------------------------------------------|------------------------------|  
|`a`|`100`|`4`|  
|`b`|`010`|`2`|  
|`c`|`001`|`1`|  
|`ab`|`110`|`6`|  
|`ac`|`101`|`5`|  
|`bc`|`011`|`3`|  
|`abc`|`111`|`7`|  
  
## <a name="technical-definition-of-grouping_id-"></a>Definizione tecnica di GROUPING_ID ()  
 Ogni argomento GROUPING_ID deve essere un elemento dell'elenco GROUP BY. GROUPING_ID () restituisce una mappa di bit **integer** i cui N bit più bassi possono essere attivati. Un **bit** attivato indica che l'argomento corrispondente non è una colonna di raggruppamento per la riga di output specificata. Il **bit** dell'ordine più basso corrisponde all'argomento N <sup></sup> e il **bit** dell'ordine più basso alla posizione N-1 corrisponde all'argomento 1.  
  
## <a name="grouping_id--equivalents"></a>Equivalenti GROUPING_ID ()  
 Per una singola query di raggruppamento, GROUPING (\<column_expression>) è equivalente a GROUPING_ID (\<column_expression>) ed entrambi restituiscono 0.  
  
 Ad esempio, le istruzioni seguenti sono equivalenti:  
  
 Istruzione A:  
  
```sql  
SELECT GROUPING_ID(A,B)  
FROM T   
GROUP BY CUBE(A,B)   
```  
  
 Istruzione B:  
  
```sql  
SELECT 3 FROM T GROUP BY ()  
UNION ALL  
SELECT 1 FROM T GROUP BY A  
UNION ALL  
SELECT 2 FROM T GROUP BY B  
UNION ALL  
SELECT 0 FROM T GROUP BY A,B  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-grouping_id-to-identify-grouping-levels"></a>R. Utilizzo di GROUPING_ID per identificare i livelli di raggruppamento  
 Nell'esempio seguente viene restituito il conteggio di dipendenti per `Name` e `Title`, `Name,` e totale della società nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. `GROUPING_ID()` viene utilizzato per creare un valore per ogni riga nella colonna `Title` per identificare il livello di raggruppamento.  
  
```sql  
SELECT D.Name  
    ,CASE   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 0 THEN E.JobTitle  
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 1 THEN N'Total: ' + D.Name   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 3 THEN N'Company Total:'  
        ELSE N'Unknown'  
    END AS N'Job Title'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle);  
```  
  
### <a name="b-using-grouping_id-to-filter-a-result-set"></a>B. Utilizzo di GROUPING_ID per filtrare un set di risultati  
  
#### <a name="simple-example"></a>Esempio semplice  
 Nel codice seguente, per restituire solo le righe che hanno un conteggio dei dipendenti per titolo, rimuovere i caratteri di commento da `HAVING GROUPING_ID(D.Name, E.JobTitle); = 0` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Per restituire solo le righe con un conteggio dei dipendenti per reparto, rimuovere i caratteri di commento da `HAVING GROUPING_ID(D.Name, E.JobTitle) = 1;`.  
  
```sql  
SELECT D.Name  
    ,E.JobTitle  
    ,GROUPING_ID(D.Name, E.JobTitle) AS 'Grouping Level'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee AS E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department AS D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle)  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 0; --All titles  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 1; --Group by Name;  
```  
  
 Di seguito è riportato il set di risultati non filtrato.  
  
|Nome|Titolo|Livello di raggruppamento|Conteggio dipendenti|Nome|  
|----------|-----------|--------------------|--------------------|----------|  
|Controllo documenti|Specialista controllo|0|2|Controllo documenti|  
|Controllo documenti|Assistente controllo documenti|0|2|Controllo documenti|  
|Controllo documenti|Responsabile controllo documenti|0|1|Controllo documenti|  
|Controllo documenti|NULL|1|5|Controllo documenti|  
|Facilities and Maintenance|Assistente amministrativo Facilities|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Responsabile strutture|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Custode|0|4|Facilities and Maintenance|  
|Facilities and Maintenance|Supervisore di manutenzione|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|NULL|1|7|Facilities and Maintenance|  
|NULL|NULL|3|12|NULL|  
  
#### <a name="complex-example"></a>Esempio complesso  
 Nell'esempio seguente `GROUPING_ID()` viene utilizzato per filtrare un set di risultati che contiene più livelli di raggruppamento suddivisi per livello di raggruppamento. Un codice simile può essere utilizzato per creare una vista contenente molti livelli di raggruppamento e una stored procedure che chiama la vista passando un parametro che filtra la vista per livello di raggruppamento. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DECLARE @Grouping NVARCHAR(50);  
DECLARE @GroupingLevel SMALLINT;  
SET @Grouping = N'CountryRegionCode Total';  
  
SELECT @GroupingLevel = (  
    CASE @Grouping  
        WHEN N'Grand Total'             THEN 15  
        WHEN N'SalesPerson Total'       THEN 14  
        WHEN N'Store Total'             THEN 13  
        WHEN N'Store SalesPerson Total' THEN 12  
        WHEN N'CountryRegionCode Total' THEN 11  
        WHEN N'Group Total'             THEN 7  
        ELSE N'Unknown'  
    END);  
  
SELECT   
    T.[Group]  
    ,T.CountryRegionCode  
    ,S.Name AS N'Store'  
    ,(SELECT P.FirstName + ' ' + P.LastName   
        FROM Person.Person AS P   
        WHERE P.BusinessEntityID = H.SalesPersonID)  
        AS N'Sales Person'  
    ,SUM(TotalDue)AS N'TotalSold'  
    ,CAST(GROUPING(T.[Group])AS char(1)) +   
        CAST(GROUPING(T.CountryRegionCode)AS char(1)) +   
        CAST(GROUPING(S.Name)AS char(1)) +   
        CAST(GROUPING(H.SalesPersonID)AS char(1))   
        AS N'GROUPING base-2'  
    ,GROUPING_ID((T.[Group])  
        ,(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
        ) AS N'GROUPING_ID'  
    ,CASE   
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 15 THEN N'Grand Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 14 THEN N'SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 13 THEN N'Store Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 12 THEN N'Store SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 11 THEN N'CountryRegionCode Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) =  7 THEN N'Group Total'  
        ELSE N'Error'  
        END AS N'Level'  
FROM Sales.Customer AS C  
    INNER JOIN Sales.Store AS S  
        ON C.StoreID  = S.BusinessEntityID   
    INNER JOIN Sales.SalesTerritory AS T  
        ON C.TerritoryID  = T.TerritoryID   
    INNER JOIN Sales.SalesOrderHeader AS H  
        ON C.CustomerID = H.CustomerID  
GROUP BY GROUPING SETS ((S.Name,H.SalesPersonID)  
    ,(H.SalesPersonID),(S.Name)  
    ,(T.[Group]),(T.CountryRegionCode),()  
    )  
HAVING GROUPING_ID(  
    (T.[Group]),(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
    ) = @GroupingLevel  
ORDER BY   
    GROUPING_ID(S.Name,H.SalesPersonID),GROUPING_ID((T.[Group])  
    ,(T.CountryRegionCode)  
    ,(S.Name)  
    ,(H.SalesPersonID))ASC;  
```  
  
### <a name="c-using-grouping_id--with-rollup-and-cube-to-identify-grouping-levels"></a>C. Utilizzo di GROUPING_ID () con ROLLUP e CUBE per identificare i livelli di raggruppamento  
 Il codice negli esempi seguenti mostra l'utilizzo di `GROUPING()` per calcolare la colonna `Bit Vector(base-2)`. `GROUPING_ID()` viene utilizzato per calcolare la colonna `Integer Equivalent` corrispondente. L'ordine delle colonne nella funzione `GROUPING_ID()` è il contrario dell'ordine delle colonne delle colonne concatenate dalla funzione `GROUPING()`.  
  
 In questi esempi, `GROUPING_ID()` viene utilizzato per creare un valore per ogni riga nella colonna `Grouping Level` per identificare il livello di raggruppamento. I livelli di raggruppamento non sono sempre un elenco consecutivo di valori interi che iniziano per 1 (0, 1, 2...*n*).  
  
> [!NOTE]  
>  GROUPING e GROUPING_ID possono essere utilizzati in una clausola HAVING per filtrare un set di risultati.  
  
#### <a name="rollup-example"></a>Esempio ROLLUP  
 In questo esempio, tutti i livelli di raggruppamento non vengono visualizzati nel modo presentato nell'esempio CUBE seguente. Se l'ordine delle colonne nell'elenco `ROLLUP` viene modificato, i valori del livello nella colonna `Grouping Level` vengono anch'essi modificati. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
     AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY ROLLUP(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(mm,OrderDate)  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 Set di risultati parziale:  
  
|Year|Month|Giorno|Total Due|Vettore di bit (base 2)|Equivalente integer|Livello di raggruppamento|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452.6066|7000|0|Anno Mese Giorno|  
|2007|1|2|21772.3494|7000|0|Anno Mese Giorno|  
|2007|2|1|2705653.5913|7000|0|Anno Mese Giorno|  
|2007|2|2|21684.4068|7000|0|Anno Mese Giorno|  
|2008|1|1|1908122.0967|7000|0|Anno Mese Giorno|  
|2008|1|2|46458.0691|7000|0|Anno Mese Giorno|  
|2008|2|1|3108771.9729|7000|0|Anno Mese Giorno|  
|2008|2|2|54598.5488|7000|0|Anno Mese Giorno|  
|2007|1|NULL|1519224.956|100|1|Year Month|  
|2007|2|NULL|2727337.9981|100|1|Year Month|  
|2008|1|NULL|1954580.1658|100|1|Year Month|  
|2008|2|NULL|3163370.5217|100|1|Year Month|  
|2007|NULL|NULL|4246562.9541|110|3|Year|  
|2008|NULL|NULL|5117950.6875|110|3|Year|  
|NULL|NULL|NULL|9364513.6416|111|7|Grand Total|  
  
#### <a name="cube-example"></a>Esempio CUBE  
 In questo esempio, la funzione `GROUPING_ID()` viene utilizzata per creare un valore per ogni riga nella colonna `Grouping Level` per identificare il livello di raggruppamento.  
  
 A differenza di `ROLLUP` nell'esempio precedente, `CUBE` restituisce tutti i livelli di raggruppamento. Se l'ordine delle colonne nell'elenco `CUBE` viene modificato, i valori del livello nella colonna `Grouping Level` vengono anch'essi modificati. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
        AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'Year Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY CUBE(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 Set di risultati parziale:  
  
|Year|Month|Giorno|Total Due|Vettore di bit (base 2)|Equivalente integer|Livello di raggruppamento|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452.6066|7000|0|Anno Mese Giorno|  
|2007|1|2|21772.3494|7000|0|Anno Mese Giorno|  
|2007|2|1|2705653.5913|7000|0|Anno Mese Giorno|  
|2007|2|2|21684.4068|7000|0|Anno Mese Giorno|  
|2008|1|1|1908122.0967|7000|0|Anno Mese Giorno|  
|2008|1|2|46458.0691|7000|0|Anno Mese Giorno|  
|2008|2|1|3108771.9729|7000|0|Anno Mese Giorno|  
|2008|2|2|54598.5488|7000|0|Anno Mese Giorno|  
|2007|1|NULL|1519224.956|100|1|Year Month|  
|2007|2|NULL|2727337.9981|100|1|Year Month|  
|2008|1|NULL|1954580.1658|100|1|Year Month|  
|2008|2|NULL|3163370.5217|100|1|Year Month|  
|2007|NULL|1|4203106.1979|010|2|Anno Giorno|  
|2007|NULL|2|43456.7562|010|2|Anno Giorno|  
|2008|NULL|1|5016894.0696|010|2|Anno Giorno|  
|2008|NULL|2|101056.6179|010|2|Anno Giorno|  
|2007|NULL|NULL|4246562.9541|110|3|Year|  
|2008|NULL|NULL|5117950.6875|110|3|Year|  
|NULL|1|1|3405574.7033|001|4|Mese Giorno|  
|NULL|1|2|68230.4185|001|4|Mese Giorno|  
|NULL|2|1|5814425.5642|001|4|Mese Giorno|  
|NULL|2|2|76282.9556|001|4|Mese Giorno|  
|NULL|1|NULL|3473805.1218|101|5|Month|  
|NULL|2|NULL|5890708.5198|101|5|Month|  
|NULL|NULL|1|9220000.2675|011|6|Giorno|  
|NULL|NULL|2|144513.3741|011|6|Giorno|  
|NULL|NULL|NULL|9364513.6416|111|7|Grand Total|  
  
## <a name="see-also"></a>Vedere anche  
 [GROUPING &#40;Transact-SQL&#41;](../../t-sql/functions/grouping-transact-sql.md)   
 [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
