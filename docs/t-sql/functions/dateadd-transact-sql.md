---
title: DATEADD (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione DATEADD. Questa funzione restituisce una data modificata dalla parte della data specificata.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cdcddd4eee63afc89446aa8390cef8b57bb2ef60
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754611"
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione aggiunge il valore *number* specificato (ad esempio un intero con segno) a un *datepart* del valore *date* di input e quindi restituisce il valore modificato.
  
Vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) per una panoramica di tutti i tipi di dati e delle funzioni di data e ora di [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```sql
DATEADD (datepart , number , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*datepart*  
Parte di *date* a cui `DATEADD` aggiunge un valore *number* di tipo **integer**. Questa tabella elenca tutti gli argomenti validi per *datepart*. 

> [!NOTE]
> `DATEADD` non accetta equivalenti di variabili definite dall'utente come argomenti di *datepart*. 
  
|*datepart*|Abbreviazioni|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**, **w**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*number*  
Espressione che può essere risolta in un tipo [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) aggiunto da `DATEADD` a un elemento *datepart* di *date*. `DATEADD` accetta valori di variabili definite dall'utente per *number*. `DATEADD` tronca un valore *number* specificato che contenga una frazione decimale. In questa situazione, non arrotonda il valore *number*.
  
*date*  
Espressione che può risolversi in uno dei valori seguenti: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Per *date*, `DATEADD` accetta un'espressione di colonna, un'espressione, un valore letterale stringa o una variabile definita dall'utente. Un valore stringa deve risolversi in un elemento **datetime**. Per evitare problemi di ambiguità, esprimere gli anni nel formato a quattro cifre. Per informazioni sugli anni a due cifre, vedere [Configurare l'opzione di configurazione del server Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-types"></a>Tipi restituiti

Il tipo di dati del valore restituito per questo metodo è dinamico. Il tipo restituito dipende dall'argomento specificato per `date`. Se il valore per `date` è una data letterale stringa, `DATEADD` restituisce un valore **datetime**. Se viene fornito un altro tipo di dati di input valido per `date`, `DATEADD` restituisce lo stesso tipo di dati. `DATEADD` genera un errore se la scala dei secondi del valore letterale stringa supera tre posizioni decimali (.nnn) o se il valore letterale stringa contiene la parte relativa alla differenza di fuso orario.
  
## <a name="return-value"></a>Valore restituito  
  
## <a name="datepart-argument"></a>Argomento datepart  
**dayofyear**, **day**, e **weekday** restituiscono lo stesso valore.
  
Ogni elemento *datepart* e le relative abbreviazioni restituiscono lo stesso valore.
  
Se si verificano le condizioni seguenti:

+ *datepart* è **month**
+ il mese di *date* ha più giorni del mese restituito
+ il giorno di *data* non esiste nel mese restituito

`DATEADD` restituisce l'ultimo giorno del mese restituito. Settembre, ad esempio, ha 30 (trenta) giorni. Queste istruzioni, pertanto, restituiscono 2006-09-30 00:00:00.000:
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>Argomento number  
Un argomento *number* non può superare l'intervallo del tipo **int**. Nelle istruzioni seguenti l'argomento per il parametro *number* supera l'intervallo di **int** di una unità. Queste istruzioni restituiscono entrambe il messaggio di errore seguente: "`Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>Argomento date  
`DATEADD` non accetta un argomento *date* incrementato a un valore al di fuori dell'intervallo consentito per il tipo di dati corrispondente. Nelle istruzioni seguenti il valore *number* aggiunto al valore *date* supera l'intervallo del tipo di dati *date*. `DATEADD` restituisce il messaggio di errore seguente: "`Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`".
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>Valori restituiti per una data smalldatetime e un valore datepart in secondi o secondi frazionari  
La seconda parte di un valore [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) è sempre 00. Per un valore *date* **smalldatetime**, si applica quanto segue: 

-   Per un *datepart***second** e un valore *number* compreso tra -30 e + 29, `DATEADD` non apporta modifiche.  
-   Per un *datepart***second** e un valore *number* minore di -30 o maggiore di + 29, `DATEADD` esegue l'aggiunta a partire da un minuto.  
-   Per un *datepart***millisecond** e un valore *number* compreso tra -30001 e + 29998, `DATEADD` non apporta modifiche.  
-   Per un *datepart***millisecond** e un valore *number* minore di -30001 o maggiore di + 29998, `DATEADD` esegue l'aggiunta a partire da un minuto.  
  
## <a name="remarks"></a>Osservazioni  
Usare `DATEADD` nelle clausole seguenti:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
## <a name="fractional-seconds-precision"></a>Precisione in secondi frazionari
`DATEADD` non consente l'aggiunta per un elemento *datepart***microsecond** o **nanosecond** per i tipi di dati *date***smalldatetime**, **date** e **datetime**.
  
I millisecondi hanno una scala di 3 (.123), i microsecondi hanno una scala di 6 (.123456) e i nanosecondi hanno una scala di 9 (.123456789). I tipi di dati **time**, **datetime2** e **datetimeoffset** hanno una scala massima pari a 7 (0,1234567). Per un elemento *datepart***nanosecond**, *number* deve essere pari a 100 prima che i secondi frazionari di *date* aumentino. Un valore *number* compreso tra 1 e 49 viene arrotondato per difetto a 0 e un valore number da 50 a 99 viene arrotondato per eccesso a 100.
  
Queste istruzioni aggiungono un valore *datepart***millisecond**, **microsecond** o **nanosecond**.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>Differenza di fuso orario
`DATEADD` non consente l'aggiunta per la differenza di fuso orario.
  
## <a name="examples"></a>Esempi  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>R. Incremento di un datepart a intervalli di una unità  
Ognuna di queste istruzioni incrementa il valore *datepart* a intervalli di una unità:
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>B. Incremento di più livelli di datepart in un'unica istruzione  
Ognuna di queste istruzioni incrementa *datepart* di un valore *number* abbastanza grande da incrementare anche l'argomento *datepart* di livello immediatamente più alto di *date*:
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.1111111  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.1111111  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.1111111  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.1111111  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.1111111  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.1111111  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.1111111  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.1111111  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.1111111  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.1121111  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>C. Utilizzo di espressioni come argomenti per i parametri number e date  
In questi esempi vengono usati tipi diversi di espressioni come argomenti per i parametri *number* e *date*. Gli esempi usano il database AdventureWorks.
  
#### <a name="specifying-a-column-as-date"></a>Indicazione di una colonna come data  
In questo esempio vengono aggiunti `2` (due) giorni a ogni valore nella colonna `OrderDate` per derivare una nuova colonna denominata `PromisedShipDate`:
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
Set di risultati parziale:
  
```
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Indicazione di variabili definite dall'utente come argomenti number e date  
In questo esempio vengono specificate variabili definite dall'utente come argomenti per i parametri *number* e *date*:
  
```sql
DECLARE @days INT = 365,   
        @datetime DATETIME = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>Indicazione di una funzione di sistema scalare come valore date  
Questo esempio specifica `SYSDATETIME` per *date*. Il valore esatto restituito dipende dal giorno e dall'ora di esecuzione dell'istruzione:
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Indicazione di sottoquery scalari e funzioni scalari come valori number e date  
In questo esempio vengono usate sottoquery scalari, `MAX(ModifiedDate)`, come argomenti per *number* e *date*. `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` funge da argomento fittizio perché il parametro number illustri come selezionare un argomento *number* da un elenco di valori.
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Indicazione di espressioni numeriche e funzioni di sistema scalari come valori number e date  
Questo esempio usa un'espressione numerica (-`(10/2))`, [operatori unari](../../mdx/unary-operators.md) (`-`), un [operatore aritmetico](../../mdx/arithmetic-operators.md) (`/`) e funzioni di sistema scalari (`SYSDATETIME`) come argomenti per *number* e *date*.
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>Indicazione di funzioni di rango come argomenti number  
Questo esempio usa una funzione di rango come argomento per *number*.
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>Indicazione di una funzione finestra di aggregazione come argomento number  
Questo esempio usa una funzione finestra di aggregazione come argomento per un parametro *number*.
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

