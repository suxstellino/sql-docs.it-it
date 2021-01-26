---
description: DATENAME (Transact-SQL)
title: DATENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9279883c9f032574e32056b2360a5947ffba022
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766200"
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce una stringa di caratteri che rappresenta l'elemento *datepart* specificato dell'argomento *date* indicato.

Vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) per una panoramica di tutti i tipi di dati e delle funzioni di data e ora di [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATENAME ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*datepart*  
Parte specifica dell'argomento *date* che verrà restituito da `DATENAME`. Questa tabella elenca tutti gli argomenti validi per *datepart*.

> [!NOTE]
> `DATENAME` non accetta equivalenti di variabili definite dall'utente come argomenti di *datepart*.
  
|*datepart*|Abbreviazioni|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**weekday**|**dw, w**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  

Espressione che può risolversi in uno dei tipi di dati seguenti: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Per *date*, `DATENAME` accetta un'espressione di colonna, un'espressione, un valore letterale stringa o una variabile definita dall'utente. Per evitare problemi di ambiguità, esprimere gli anni nel formato a quattro cifre. Per informazioni sugli anni a due cifre, vedere [Configurare l'opzione di configurazione del server Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Tipo restituito  
**nvarchar**
  
## <a name="return-value"></a>Valore restituito  
  
-   Ogni elemento *datepart* e le relative abbreviazioni restituiscono lo stesso valore.  
  
Il valore restituito dipende dalla lingua impostata tramite [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) e in [Configurare l'opzione di configurazione del server default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) dell'account di accesso. Il valore restituito dipende da [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) se *date* è un valore letterale stringa di alcuni formati. SET DATEFORMAT non modifica il valore restituito quando la data è un'espressione della colonna di un tipo di dati ora e data.
  
Quando il parametro *date* ha un argomento del tipo di dati **date**, il valore restituito dipende dall'impostazione specificata da [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
## <a name="tzoffset-datepart-argument"></a>Argomento datepart TZoffset  
Se l'argomento *datepart* è **TZoffset** (**tz**) e l'argomento *date* non prevede una differenza di fuso orario, `DATEADD` restituisce 0.
  
## <a name="smalldatetime-date-argument"></a>Argomento date smalldatetime  
Quando *date* è [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), `DATENAME` restituisce i secondi come 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>Valore predefinito restituito per un argomento datepart non incluso nell'argomento date  
Se per il tipo di dati dell'argomento *date* non viene specificato *datepart*, `DATENAME` restituirà il valore predefinito per *datepart* solo se l'argomento *date* ha un valore letterale.
  
Ad esempio, l'anno-mese-giorno predefinito per qualsiasi tipo di dati **date** è 1900-01-01. Questa istruzione include gli argomenti datepart per *datepart*, un argomento time per *date* e `DATENAME` restituisce `1900, January, 1, 1, Monday`.
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
Se *date* viene specificato come variabile o colonna di tabella e per il tipo di dati della variabile o della colonna *datepart* non viene specificato, `DATENAME` restituirà l'errore 9810. In questo esempio la variabile *\@t* ha il tipo di dati **time**. L'esempio ha esito negativo perché la parte della data year non è valida per il tipo di dati **time**:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>Osservazioni  

Usare `DATENAME` nelle clausole seguenti:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] DATENAME consente di eseguire in modo implicito il cast di valori letterali stringa in un tipo **datetime2**. `DATENAME` non supporta quindi il formato AGM se la data viene passata come stringa. Per usare il formato AGM è necessario eseguire il cast della stringa in modo esplicito in un tipo **datetime** o **smalldatetime**.
  
## <a name="examples"></a>Esempi  
In questo esempio vengono restituite le parti della data specificata. Sostituire un valore *datepart* della tabella per l'argomento `datepart` nell'istruzione SELECT:
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valore restituito|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Ottobre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Tuesday|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

In questo esempio vengono restituite le parti della data specificata. Sostituire un valore *datepart* della tabella per l'argomento `datepart` nell'istruzione SELECT:
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valore restituito|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Ottobre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Tuesday|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

