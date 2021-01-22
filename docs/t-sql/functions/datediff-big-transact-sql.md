---
title: DATEDIFF_BIG (Transact-SQL)
description: DATEDIFF_BIG (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: 8f6078aebab5456b8867aca1b7f6987d8a1eb6fd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168200"
---
# <a name="datediff_big-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Questa funzione restituisce il numero (sotto forma di valore big integer con segno) di limiti di *datepart* specificati sovrapposti tra gli elementi *startdate* ed *enddate* indicati.
  
Vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) per una panoramica di tutti i tipi di dati e delle funzioni di data e ora di [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  

## <a name="arguments"></a>Argomenti
*datepart*  
Parte di *startdate* ed *enddate* che specifica il tipo di limite superato.

> [!NOTE]
> `DATEDIFF_BIG` non accetta valori *datepart* da variabili definite dall'utente o come stringhe tra virgolette.

Questa tabella elenca i nomi e le abbreviazioni di tutti gli argomenti *datepart* validi.
  
|Nome *datepart*| Abbreviazione *datepart*|  
|---|---|
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

Per *date*, `DATEDIFF_BIG` accetta un'espressione di colonna, un'espressione, un valore letterale stringa o una variabile definita dall'utente. Un valore stringa deve risolversi in un elemento **datetime**. Per evitare problemi di ambiguità, esprimere gli anni nel formato a quattro cifre. `DATEDIFF_BIG` sottrae *startdate* da *enddate*. Per evitare ambiguità, esprimere gli anni nel formato a quattro cifre. Per informazioni sugli anni a due cifre, vedere [Configurare l'opzione di configurazione del server Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
*enddate*  
Vedere *startdate*.
  
## <a name="return-type"></a>Tipo restituito  
**bigint** con segno  
  
## <a name="return-value"></a>Valore restituito  
Restituisce la differenza **bigint** tra *startdate* ed *enddate*, espressa nel limite impostato da *datepart*.
  
Per un valore restituito esterno all'intervallo per **bigint** (da -9.223.372.036.854.775.808 a +9.223.372.036.854.775.807), `DATEDIFF_BIG` restituisce un errore. A differenza di  che restituisce un **int** e può quindi determinare un overflow con una precisione di **minute** o superiore, `DATEDIFF_BIG` può determinare un overflow solo se si usa la precisione **nanosecond** dove la differenza tra *enddate* e *startdate* è superiore a 292 anni, 3 mesi, 10 giorni, 23 ore, 47 minuti e 16.8547758 secondi.
  
Se sia a *startdate* che a *enddate* è stato assegnato solo un valore orario e *datepart* non è un *datepart* orario, `DATEDIFF_BIG` restituisce 0.
  
Per calcolare il valore restituito, `DATEDIFF_BIG` usa un componente differenza di fuso orario *startdate* o *enddate*.
  
Per un valore **smalldatetime** usato per *startdate* o *enddate*, nel valore restituito `DATEDIFF_BIG` imposta sempre i secondi e i millisecondi su 0, perché [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) garantisce la precisione solo a livello di minuti.
  
Se a una variabile di tipo data viene assegnato solo il valore dell'ora, `DATEDIFF_BIG` imposta il valore della parte mancante della data sul valore predefinito: `1900-01-01`. Se a una variabile di tipo ora o data viene assegnato solo il valore della data, `DATEDIFF_BIG` imposta il valore della parte mancante dell'ora sul valore predefinito: `00:00:00`. Se *startdate* ed *enddate* hanno rispettivamente solo la parte relativa all'ora o solo la parte relativa alla data, `DATEDIFF_BIG` imposta le parti mancanti sui rispettivi valori predefiniti.
  
Se i valori *startdate* ed *enddate* sono di tipi data diversi e uno di questi comprende un numero maggiore di parti di ora o offre una precisione in secondi frazionari maggiore, `DATEDIFF_BIG` imposta le parti mancanti dell'altro valore su 0.
  
## <a name="datepart-boundaries"></a>Limiti di datepart
Le istruzioni seguenti hanno gli stessi valori *startdate* ed *enddate*. Queste date sono adiacenti e differiscono di un microsecondo (0,0000001 secondi). La differenza tra *startdate* e *enddate* in ogni istruzione oltrepassa un limite di calendario o di ora del rispettivo valore *datepart*. Ciascuna istruzione restituisce 1. Se *startdate* ed *enddate* hanno valori di anno diversi, ma gli stessi valori di settimana di calendario, `DATEDIFF_BIG` restituirà 0 per *datepart* **week**.

```sql
SELECT DATEDIFF_BIG(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>Osservazioni  
Usare `DATEDIFF_BIG` nelle clausole `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` e `ORDER BY`.
  
`DATEDIFF_BIG` consente di eseguire in modo implicito il cast di valori letterali stringa come tipo di dati **datetime2**. `DATEDIFF_BIG`, pertanto, non supporta il formato AGM se la data viene passata come stringa. Per usare il formato AGM è necessario eseguire il cast della stringa in modo esplicito in un tipo **datetime** o **smalldatetime**.
  
La specifica di `SET DATEFIRST` non ha alcun effetto su `DATEDIFF_BIG`. `DATEDIFF_BIG` usa sempre la domenica come primo giorno della settimana, per garantire che la funzione operi in modo deterministico.

`DATEDIFF_BIG` può determinare un overflow con **nanosecond** se la differenza tra *enddate* e *startdate* restituisce un valore non compreso nell'intervallo per **bigint**.
  
## <a name="examples"></a>Esempi 
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>Specifica di colonne per startdate ed enddate  
Questo esempio, che usa tipi diversi di espressioni come argomenti per i parametri *startdate* ed *enddate*, calcola il numero di limiti di giorno sovrapposti tra le date di due colonne di una tabella.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF_BIG(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  

### <a name="finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>Ricerca della differenza tra startdate e enddate come stringhe di parti di data

```sql
DECLARE @date1 DATETIME2, @date2 DATETIME2, @result VARCHAR(100)
DECLARE @years BIGINT, @months BIGINT, @days BIGINT, @hours BIGINT, @minutes BIGINT, @seconds BIGINT, @milliseconds BIGINT

SET @date1 = '0001-01-01 00:00:00.00000000'
SET @date2 = '2018-12-12 07:08:01.12345678'

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
          + CASE WHEN @milliseconds > 0 THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
               ELSE '' END 
          + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
2017 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```   

Per esempi più strettamente correlati, vedere [DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md).
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md)
  
  
