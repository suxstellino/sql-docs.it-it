---
title: DATEPART (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione DATEPART. Questa funzione restituisce un valore intero corrispondente alla parte della data di una data specificata.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adae6be66b149f930bee51c636bf86d7614e3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093672"
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Questa funzione restituisce un intero che rappresenta l'elemento *datepart* specificato dell'elemento *date* indicato.
  
Vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) per una panoramica di tutti i tipi di dati e delle funzioni di data e ora di [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATEPART ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*datepart*  
Parte specifica dell'argomento *date* per cui `DATEPART` restituirà un **intero**. Questa tabella elenca tutti gli argomenti validi per *datepart*.

> [!NOTE]
> `DATEPART` non accetta equivalenti di variabili definite dall'utente come argomenti di *datepart*.
  
|*datepart*|Abbreviazioni|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**tzoffset**|**tz**|  
|**iso_week**|**isowk**, **isoww**|  
  
*date*  
Espressione che si risolve in uno dei tipi di dati seguenti: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Per *date*, `DATEPART` accetta un'espressione di colonna, un'espressione, un valore letterale stringa o una variabile definita dall'utente. Per evitare problemi di ambiguità, esprimere gli anni nel formato a quattro cifre. Per informazioni sugli anni a due cifre, vedere [Configurare l'opzione di configurazione del server Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Tipo restituito  
 **int**  
  
## <a name="return-value"></a>Valore restituito  
Ogni elemento *datepart* e le relative abbreviazioni restituiscono lo stesso valore.
  
Il valore restituito dipende dalla lingua impostata tramite [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) e in [Configurare l'opzione di configurazione del server default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) dell'account di accesso. Il valore restituito dipende da [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) se *date* è un valore letterale stringa di alcuni formati. SET DATEFORMAT non modifica il valore restituito quando la data è un'espressione della colonna di un tipo di dati ora e data.
  
Questa tabella elenca tutti gli argomenti *datepart*, con i corrispondenti valori restituiti, per l'istruzione `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')`. L'argomento *date* ha un tipo di dati **datetimeoffset(7)** . Le ultime due posizioni del valore restituito di **nanosecond** *datepart* sono sempre `00` e questo valore ha una scala di 9:

**.123456700**
  
|*datepart*|Valore restituito|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|3|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**tzoffset, tz**|310|  
|**iso_week, isowk, isoww**|44|  
  
## <a name="week-and-weekday-datepart-arguments"></a>Argomenti datepart per settimana e giorno feriale
Per un argomento *datepart***week** (**wk**, **ww**) o **weekday** (**dw**), il valore restituito `DATEPART` dipende dal valore impostato da [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
Il 1° gennaio di qualsiasi anno definisce il numero iniziale per **week** _datepart_. Ad esempio:

DATEPART (**wk**, 'Jan 1, *xxx* x') = 1

dove *xxxx* è un anno.
  
Nella tabella seguente è riportato il valore restituito per **week** e **weekday** *datepart* per "2007-04-21" per ogni argomento SET DATEFIRST. L'1 gennaio 2007 cade di lunedì. Il 21 aprile 2007 cade di sabato. Per l'inglese Stati Uniti,

`SET DATEFIRST 7 -- ( Sunday )`

funge da valore predefinito. Dopo avere impostato DATEFIRST, usare questa istruzione SQL suggerita per i valori della tabella di datepart:

`SELECT DATEPART(week, '2007-04-21 '), DATEPART(weekday, '2007-04-21 ')`
  
|SET DATEFIRST<br /><br /> argomento|week<br /><br /> restituito|giorno feriale<br /><br /> restituito|  
|---|---|---|
|1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17|1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>Argomenti datepart per anno, mese e giorno  
I valori restituiti per DATEPART (**year**, *date*), DATEPART (**month**, *date*) e DATEPART (**day**, *date*) corrispondono a quelli restituiti rispettivamente dalle funzioni [YEAR](../../t-sql/functions/year-transact-sql.md), [MONTH](../../t-sql/functions/month-transact-sql.md) e [DAY](../../t-sql/functions/day-transact-sql.md).
  
## <a name="iso_week-datepart"></a>iso_week datepart  
ISO 8601 include il sistema settimana-relativo alla data ISO, un sistema di numerazione per settimane. Ogni settimana è associata all'anno che inizia con un giovedì. Ad esempio, la settimana 1 del 2004 (2004W01) ha coperto il periodo da lunedì 29 dicembre 2003 a domenica 4 gennaio 2004. I paesi e le aree geografiche Europai usano in genere questo stile di numerazione. I paesi e le aree geografiche non Europai in genere non lo usano.

Nota: il numero massimo di settimane in un anno può essere 52 o 53.
  
I sistemi di numerazione dei diversi paesi potrebbero non essere conformi allo standard ISO. Questa tabella illustra sei possibilità:
  
|Primo giorno della settimana|Prima settimana dell'anno|Settimane assegnate due volte|Utilizzato da/in|  
|---|---|---|---|
|Sunday|1 gennaio,<br /><br /> Primo sabato,<br /><br /> 1-7 giorni dell'anno|Sì|Stati Uniti|  
|Monday|1 gennaio,<br /><br /> Prima domenica,<br /><br /> 1-7 giorni dell'anno|Sì|La maggior parte di Europa e Regno Unito|  
|Monday|4 gennaio,<br /><br /> Primo giovedì,<br /><br /> 4-7 giorni dell'anno|No|ISO 8601, Norvegia e Svezia|  
|Monday|7 gennaio,<br /><br /> Primo lunedì,<br /><br /> 7 giorni dell'anno|No||  
|Wednesday|1 gennaio,<br /><br /> Primo martedì,<br /><br /> 1-7 giorni dell'anno|Sì||  
|Sabato|1 gennaio,<br /><br /> Primo venerdì,<br /><br /> 1-7 giorni dell'anno|Sì||  
  
## <a name="tzoffset"></a>tzoffset  
`DATEPART` restituisce il valore **tzoffset** (**tz**) come numero di minuti (firmato). Questa istruzione restituisce una differenza di fuso orario di 310 minuti:
  
```sql
SELECT DATEPART (tzoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
`DATEPART` esegue il rendering del valore tzoffset come segue:
- Per datetimeoffset e datetime2, tzoffset restituisce la differenza di orario in minuti, in cui l'offset per datetime2 è sempre 0 minuti.
- Per i tipi di dati che possono essere convertiti in modo implicito in **datetimeoffset** o **datetime2**, `DATEPART` restituisce la differenza oraria in minuti. Eccezione: altri tipi di dati date/time.
- I parametri di tutti gli altri tipi generano un errore.
  
  
## <a name="smalldatetime-date-argument"></a>Argomento date smalldatetime  
Per un valore [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) *date*, `DATEPART` restituisce i secondi come 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>Valore predefinito restituito per un datepart non incluso nell'argomento date  
Se per il tipo di dati dell'argomento *date* non viene specificato *datepart*, `DATEPART` restituirà il valore predefinito per *datepart* solo se è stato specificato un valore letterale per *date*.
  
Ad esempio, l'anno-mese-giorno predefinito per qualsiasi tipo di dati **date** è 1900-01-01. Questa istruzione include gli argomenti datepart per *datepart*, un argomento time per *date* e restituisce `1900, 1, 1, 1, 2`.
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
Se *date* viene specificato come variabile o colonna di tabella e per il tipo di dati della variabile o della colonna *datepart* non viene specificato, `DATEPART` restituirà l'errore 9810. In questo esempio la variabile *\@t* ha il tipo di dati **time**. L'esempio ha esito negativo perché la parte della data year non è valida per il tipo di dati **time**:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>Secondi frazionari
Queste istruzioni illustrano che `DATEPART` restituisce secondi frazionari:
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>Osservazioni  
`DATEPART` può essere usato nelle clausole select list, WHERE, HAVING, GROUP BY e ORDER BY.
  
DATEPART consente di eseguire in modo implicito il cast di valori letterali stringa in un tipo **datetime2** in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pertanto, DATENAME non supporta il formato AGM se la data viene passata come stringa. Per usare il formato AGM è necessario eseguire il cast della stringa in modo esplicito in un tipo **datetime** o **smalldatetime**.
  
## <a name="examples"></a>Esempi  
In questo esempio viene restituito l'anno di base. L'anno di base è utile nei calcoli delle date. Nell'esempio un numero specifica la data. Si noti che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interpreta il valore 0 come 1 gennaio 1900.
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  

-- Returns: 1900    1    1 
```  
  
Questo esempio restituisce la parte relativa al giorno della data `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 20
```  
  
Questo esempio restituisce la parte relativa all'anno della data `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 1974
```  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
