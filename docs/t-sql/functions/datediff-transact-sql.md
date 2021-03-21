---
title: DATEDIFF (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione DATEDIFF. Restituisce la differenza numerica tra una data di inizio e di fine in base a datepart.
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEDIFF_TSQL
- DATEDIFF
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATEDIFF function [SQL Server]
- time [SQL Server], crossed boundaries
- differences in date and time [SQL Server]
- counting crossed date time boundaries [SQL Server]
- date and time [SQL Server], DATEDIFF
- dates [SQL Server], crossed boundaries
- boundary differences date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- interval dates [SQL Server]
- time [SQL Server], functions
- crossing date time boundaries [SQL Server]
- calculating dates times [SQL Server]
ms.assetid: eba979f2-1a8d-4cce-9d75-b74f9b519b37
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9a8ad83b30d62b2b5c6b7348eed99e013259081f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754601"
---
# <a name="datediff-transact-sql"></a>DATEDIFF (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il numero (sotto forma di valore intero con segno) di limiti del datepart specificato sovrapposti tra gli elementi *startdate* ed *enddate* indicati.
  
Vedere [DATEDIFF_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-big-transact-sql.md) per una funzione che gestisca differenze maggiori tra i valori *startdate* ed *enddate*. Vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) per una panoramica di tutti i tipi di dati e delle funzioni di data e ora di [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATEDIFF ( datepart , startdate , enddate )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*datepart*  
Unità usata da **DATEDIFF** per indicare la differenza tra _startdate_ ed _enddate_. Le unità _datepart_ comunemente usate includono `month` o `second`.

Non è possibile specificare il valore _datepart_ in una variabile, né come stringa tra virgolette come `'month'`.

Nella tabella seguente sono elencati tutti i valori _datepart_ validi. **DATEDIFF** accetta il nome completo o _datepart_ o qualsiasi abbreviazione elencata del nome completo.

|Nome *datepart*|Abbreviazione *datepart*|  
|-----------|------------|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
| &nbsp; | &nbsp; |

> [!NOTE]
> Ogni nome *datepart* specifico e le abbreviazioni *datepart* corrispondenti restituiscono lo stesso valore.

*startdate*  
Espressione che può risolversi in uno dei valori seguenti:

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**
  
Per evitare ambiguità, esprimere gli anni nel formato a quattro cifre. Per informazioni sui valori relativi agli anni a due cifre, vedere [Configurare l'opzione di configurazione del server Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
*enddate*  
Vedere *startdate*.
  
## <a name="return-type"></a>Tipo restituito  
 **int**  
  
## <a name="return-value"></a>Valore restituito  

La differenza **int** tra *startdate* ed *enddate*, espressa nel limite impostato da *datepart*.
  
Ad esempio, `SELECT DATEDIFF(day, '2036-03-01', '2036-02-28');` restituisce -2, a indicare che il 2036 deve essere un anno bisestile. Questo caso significa che se si inizia da _startdate_ '2036-03-01' e quindi si sottraggono 2 giorni, _enddate_ sarà '2036-02-28'.
  
Per un valore restituito esterno all'intervallo per **int** (da -2.147.483.648 a +2.147.483.647), `DATEDIFF` restituisce un errore.  Per **millisecond**, la differenza massima tra *startdate* e *enddate* è 24 giorni, 20 ore, 31 minuti e 23,647 secondi. Per **second**, la differenza massima è di 68 anni, 19 giorni, 3 ore, 14 minuti e 7 secondi.
  
Se sia a *startdate* che a *enddate* è stato assegnato solo un valore orario e *datepart* non è un *datepart* orario, `DATEDIFF` restituisce 0.
  
Per calcolare il valore restituito, `DATEDIFF` usa il componente di differenza di fuso orario di *startdate* o *enddate*.
  
Dal momento che [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) garantisce la precisione solo a livello di minuti, i secondi e i millisecondi vengono sempre impostati su 0 nel valore restituito quando *startdate* o *enddate* contiene un valore **smalldatetime**.
  
Se a una variabile di tipo data viene assegnato solo il valore dell'ora, `DATEDIFF` imposta il valore della parte mancante della data sul valore predefinito: `1900-01-01`. Se a una variabile di tipo ora o data viene assegnato solo il valore della data, `DATEDIFF` imposta il valore della parte mancante dell'ora sul valore predefinito: `00:00:00`. Se *startdate* ed *enddate* hanno rispettivamente solo la parte relativa all'ora o solo la parte relativa alla data, `DATEDIFF` imposta le parti mancanti sui rispettivi valori predefiniti.
  
Se i valori *startdate* ed *enddate* sono di tipi data diversi e uno di questi comprende un numero maggiore di parti di ora o offre una precisione in secondi frazionari maggiore, `DATEDIFF` imposta le parti mancanti dell'altro valore su 0.
  
## <a name="_datepart_-boundaries"></a>Limiti di _datepart_

Le istruzioni seguenti hanno gli stessi valori *startdate* ed *enddate*. Queste date sono adiacenti e differiscono di cento nanosecondi (0,0000001 secondi). La differenza tra *startdate* e *enddate* in ogni istruzione oltrepassa un limite di calendario o di ora del rispettivo valore *datepart*. Ciascuna istruzione restituisce 1.
  
```sql
SELECT DATEDIFF(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(microsecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```

Se *startdate* ed *enddate* hanno valori di anno diversi, ma gli stessi valori di settimana di calendario, `DATEDIFF` restituirà 0 per *datepart* **week**.

## <a name="remarks"></a>Osservazioni  
Usare `DATEDIFF` nelle clausole `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` e `ORDER BY`.
  
`DATEDIFF` consente di eseguire in modo implicito il cast di valori letterali stringa come tipo di dati **datetime2**. `DATEDIFF`, pertanto, non supporta il formato AGM se la data viene passata come stringa. Per usare il formato AGM è necessario eseguire il cast della stringa in modo esplicito in un tipo **datetime** o **smalldatetime**.
  
La specifica di `SET DATEFIRST` non ha alcun effetto su `DATEDIFF`. `DATEDIFF` usa sempre la domenica come primo giorno della settimana, per garantire che la funzione operi in modo deterministico.

`DATEDIFF` può determinare un overflow con una precisione di **minute** o superiore se la differenza tra *enddate* e *startdate* restituisce un valore non compreso nell'intervallo per **int**.
  
## <a name="examples"></a>Esempi  
Questi esempi usano tipi diversi di espressioni come argomenti per i parametri *startdate* ed *enddate*.
  
### <a name="a-specifying-columns-for-startdate-and-enddate"></a>R. Specifica di colonne per startdate ed enddate  
Questo esempio calcola il numero di limiti di giorno sovrapposti tra le date di due colonne in una tabella.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="b-specifying-user-defined-variables-for-startdate-and-enddate"></a>B. Specifica di variabili definite dall'utente per startdate ed enddate  
In questo esempio, variabili definite dall'utente fungono da argomenti per *startdate* ed *enddate*.
  
```sql
DECLARE @startdate DATETIME2 = '2007-05-05 12:10:09.3312722';  
DECLARE @enddate   DATETIME2 = '2007-05-04 12:10:09.3312722';   
SELECT DATEDIFF(day, @startdate, @enddate);  
```  
  
### <a name="c-specifying-scalar-system-functions-for-startdate-and-enddate"></a>C. Specifica di funzioni di sistema scalari per startdate ed enddate  
Questo esempio usa funzioni di sistema scalari come argomenti per *startdate* ed *enddate*.
  
```sql
SELECT DATEDIFF(millisecond, GETDATE(), SYSDATETIME());  
```  
  
### <a name="d-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>D. Specifica di sottoquery scalari e di funzioni scalari per startdate ed enddate  
Questo esempio usa sottoquery e funzioni scalari come argomenti per *startdate* ed *enddate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day,
    (SELECT MIN(OrderDate) FROM Sales.SalesOrderHeader),  
    (SELECT MAX(OrderDate) FROM Sales.SalesOrderHeader));  
```  
  
### <a name="e-specifying-constants-for-startdate-and-enddate"></a>E. Specifica di costanti per startdate ed enddate  
Questo esempio usa costanti di tipo carattere come argomenti per *startdate* ed *enddate*.
  
```sql
SELECT DATEDIFF(day,
   '2007-05-07 09:53:01.0376635',
   '2007-05-08 09:53:01.0376635');  
```  
  
### <a name="f-specifying-numeric-expressions-and-scalar-system-functions-for-enddate"></a>F. Specifica di espressioni numeriche e funzioni di sistema scalari per enddate  
Questo esempio usa un'espressione numerica, `(GETDATE() + 1)`, e funzioni di sistema scalari, `GETDATE` e `SYSDATETIME`, come argomenti per *enddate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day, '2007-05-07 09:53:01.0376635', GETDATE() + 1)
    AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
USE AdventureWorks2012;  
GO  
SELECT
    DATEDIFF(
            day,
            '2007-05-07 09:53:01.0376635',
            DATEADD(day, 1, SYSDATETIME())
        ) AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
```  
  
### <a name="g-specifying-ranking-functions-for-startdate"></a>G. Specifica di funzioni di rango per startdate  
Questo esempio usa una funzione di rango come argomento per *startdate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        a.PostalCode), SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson s   
    INNER JOIN Person.Person p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
### <a name="h-specifying-an-aggregate-window-function-for-startdate"></a>H. Specifica di una funzione finestra di aggregazione per startdate  
Questo esempio usa una funzione finestra di aggregazione come argomento per un parametro *startdate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT soh.SalesOrderID, sod.ProductID, sod.OrderQty, soh.OrderDate,
    DATEDIFF(day, MIN(soh.OrderDate)   
        OVER(PARTITION BY soh.SalesOrderID), SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail sod  
    INNER JOIN Sales.SalesOrderHeader soh  
        ON sod.SalesOrderID = soh.SalesOrderID  
WHERE soh.SalesOrderID IN(43659, 58918);  
GO  
```  

### <a name="i-finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>I. Ricerca della differenza tra startdate e enddate come stringhe di parti di data

```sql
-- DOES NOT ACCOUNT FOR LEAP YEARS
DECLARE @date1 DATETIME, @date2 DATETIME, @result VARCHAR(100);
DECLARE @years INT, @months INT, @days INT,
    @hours INT, @minutes INT, @seconds INT, @milliseconds INT;

SET @date1 = '1900-01-01 00:00:00.000'
SET @date2 = '2018-12-12 07:08:01.123'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
     + CASE
            WHEN @milliseconds > 0
                THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
            ELSE ''
       END 
     + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
118 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Questi esempi usano tipi diversi di espressioni come argomenti per i parametri *startdate* ed *enddate*.
  
### <a name="j-specifying-columns-for-startdate-and-enddate"></a>J. Specifica di colonne per startdate ed enddate  
Questo esempio calcola il numero di limiti di giorno sovrapposti tra le date di due colonne in una tabella.
  
```sql
CREATE TABLE dbo.Duration 
    (startDate datetime2, endDate datetime2);
    
INSERT INTO dbo.Duration (startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT TOP(1) DATEDIFF(day, startDate, endDate) AS Duration  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="k-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>K. Specifica di sottoquery scalari e di funzioni scalari per startdate ed enddate  
Questo esempio usa sottoquery e funzioni scalari come argomenti per *startdate* ed *enddate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day, (SELECT MIN(HireDate) FROM dbo.DimEmployee),  
    (SELECT MAX(HireDate) FROM dbo.DimEmployee))   
FROM dbo.DimEmployee;  
  
```  
  
### <a name="l-specifying-constants-for-startdate-and-enddate"></a>L. Specifica di costanti per startdate ed enddate  
Questo esempio usa costanti di tipo carattere come argomenti per *startdate* ed *enddate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day,
    '2007-05-07 09:53:01.0376635',
    '2007-05-08 09:53:01.0376635') FROM DimCustomer;  
```  
  
### <a name="m-specifying-ranking-functions-for-startdate"></a>M. Specifica di funzioni di rango per startdate  
Questo esempio usa una funzione di rango come argomento per *startdate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName,
    DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        DepartmentName), SYSDATETIME()) AS RowNumber  
FROM dbo.DimEmployee;  
```  
  
### <a name="n-specifying-an-aggregate-window-function-for-startdate"></a>N. Specifica di una funzione finestra di aggregazione per startdate  
Questo esempio usa una funzione finestra di aggregazione come argomento per un parametro *startdate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, DepartmentName,
    DATEDIFF(year, MAX(HireDate)  
        OVER (PARTITION BY DepartmentName), SYSDATETIME()) AS SomeValue  
FROM dbo.DimEmployee  
```  
  
## <a name="see-also"></a>Vedere anche
[DATEDIFF_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-big-transact-sql.md)  
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
