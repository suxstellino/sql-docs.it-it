---
title: Funzioni e tipi di dati di data e ora
description: Collegamenti a funzioni e tipi di dati di data e ora.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- dates [SQL Server]
- date and time [SQL Server], all data types and functions
- date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 83e378a2-6e89-4c80-bc4f-644958d9e0a9
author: cawrites
ms.author: chadam
monikerRange: = azure-sqldw-latest||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: b1147dbb76217a819716a7ec740017ee5f4d4397
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183551"
---
# <a name="date-and-time-data-types-and-functions-transact-sql"></a>Funzioni e tipi di dati di data e ora (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Le sezioni in questo argomento trattano tutti i tipi di dati e le funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)].

- [Tipi di dati di data e ora](#DateandTimeDataTypes)
- [Funzioni di data e ora](#DateandTimeFunctions)
  - [Funzioni che restituiscono i valori di data e ora di sistema](#GetSystemDateandTimeValues)
  - [Funzioni che restituiscono parti di data e ora](#GetDateandTimeParts)
  - [Funzioni che restituiscono i valori di data e ora dalle relative parti](#fromParts)
  - [Funzioni che restituiscono valori di differenza di data e ora](#GetDateandTimeDifference)
  - [Funzioni che modificano i valori di data e ora](#ModifyDateandTimeValues)
  - [Funzioni che impostano o restituiscono il formato della sessione](#SetorGetSessionFormatFunctions)
  - [Funzioni che convalidano i valori di data e ora](#ValidateDateandTimeValues)
- [Argomenti correlati a data e ora](#DateandTimeRelatedTopics)

## <a name="date-and-time-data-types"></a><a name="DateandTimeDataTypes"></a> Tipi di dati di data e ora

I tipi di dati di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)] sono elencati nella tabella seguente:

|Tipo di dati|Format|Range|Accuratezza|Dimensioni dello spazio di archiviazione (in byte)|Precisione in secondi frazionari definita dall'utente|Differenza di fuso orario|
|---|---|---|---|---|---|---|
|[time](../../t-sql/data-types/time-transact-sql.md)|hh:mm:ss[.nnnnnnn]|da 00.00.00.0000000 a 23.59.59.9999999|100 nanosecondi|da 3 a 5|Sì|No|
|[date](../../t-sql/data-types/date-transact-sql.md)|YYYY-MM-DD|Da 01-01-0001 a 31-12-9999|1 giorno|3|No|No|
|[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)|AAAAA-MM-GG hh.mm.ss|da 01-01-1900 a 06-06-2079|1 minuto|4|No|No|
|[datetime](../../t-sql/data-types/datetime-transact-sql.md)|AAAA-MM-GG hh:mm:ss[.nnn]|da 01-01-1753 a 31-12-9999|0,00333 secondi|8|No|No|
|[datetime2](../../t-sql/data-types/datetime2-transact-sql.md)|AAAA-MM-GG hh:mm:ss[.nnnnnnn]|da 01-01-0001 00.00.00.0000000 a 31-12-9999 23.59.59.9999999|100 nanosecondi|da 6 a 8|Sì|No|
|[datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|da 0001-01-01 00:00:00.0000000 a 9999-12-31 23:59:59.9999999 (in UTC)|100 nanosecondi|da 8 a 10|Sì|Sì|

> [!NOTE]
> Il tipo di dati di [!INCLUDE[tsql](../../includes/tsql-md.md)] [rowversion](../../t-sql/data-types/rowversion-transact-sql.md) non è relativo a data e ora. **timestamp** è un sinonimo deprecato per **rowversion**.

## <a name="date-and-time-functions"></a><a name="DateandTimeFunctions"></a> Funzioni di data e ora

Le seguenti tabelle elencano le funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per altre informazioni sul determinismo, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

### <a name="function-that-return-system-date-and-time-values"></a><a name="GetSystemDateandTimeValues"></a> Funzioni che restituiscono i valori di data e ora di sistema

[!INCLUDE[tsql](../../includes/tsql-md.md)] deriva tutti i valori di data e ora dal sistema operativo del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

#### <a name="higher-precision-system-date-and-time-functions"></a>Funzioni di data e ora di sistema con precisione superiore

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] deriva i valori di data e ora tramite l'API Windows GetSystemTimeAsFileTime(). L'accuratezza dipende dall'hardware e dalla versione di Windows del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa API ha una precisione fissata a 100 nanosecondi. Per determinare l'accuratezza, usare l'API Windows GetSystemTimeAdjustment().

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[SYSDATETIME](../../t-sql/functions/sysdatetime-transact-sql.md)|SYSDATETIME ()|Restituisce un valore **datetime2(7)** contenente la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore restituito non include la differenza di fuso orario.|**datetime2(7)**|Non deterministica|
|[SYSDATETIMEOFFSET](../../t-sql/functions/sysdatetimeoffset-transact-sql.md)|SYSDATETIMEOFFSET ( )|Restituisce un valore **datetimeoffset(7)** contenente la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore restituito include la differenza di fuso orario.|**datetimeoffset(7)**|Non deterministica|
|[SYSUTCDATETIME](../../t-sql/functions/sysutcdatetime-transact-sql.md)|SYSUTCDATETIME ( )|Restituisce un valore **datetime2(7)** contenente la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La funzione restituisce i valori di data e ora in formato ora UTC (Coordinated Universal Time).|**datetime2(7)**|Non deterministica|

#### <a name="lower-precision-system-date-and-time-functions"></a>Funzioni di data e ora di sistema con precisione inferiore

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[CURRENT_TIMESTAMP](../../t-sql/functions/current-timestamp-transact-sql.md)|CURRENT_TIMESTAMP|Restituisce un valore **datetime** contenente la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore restituito non include la differenza di fuso orario.|**datetime**|Non deterministica|
|[GETDATE](../../t-sql/functions/getdate-transact-sql.md)|GETDATE ( )|Restituisce un valore **datetime** contenente la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore restituito non include la differenza di fuso orario.|**datetime**|Non deterministica|
|[GETUTCDATE](../../t-sql/functions/getutcdate-transact-sql.md)|GETUTCDATE ( )|Restituisce un valore **datetime** contenente la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La funzione restituisce i valori di data e ora in formato ora UTC (Coordinated Universal Time).|**datetime**|Non deterministica|

### <a name="functions-that-return-date-and-time-parts"></a><a name="GetDateandTimeParts"></a> Funzioni che restituiscono parti di data e ora

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|--------------|------------|------------------|----------------------|-----------------|
|[DATENAME](../../t-sql/functions/datename-transact-sql.md)|DATENAME ( *datepart* , *date* )|Restituisce una stringa di caratteri che rappresenta l'elemento *datepart* specificato della data indicata.|**nvarchar**|Non deterministica|
|[DATEPART](../../t-sql/functions/datepart-transact-sql.md)|DATEPART ( *datepart* , *date* )|Restituisce un intero che rappresenta l'elemento *datepart* specificato dell'elemento *date* indicato.|**int**|Non deterministica|
|[DAY](../../t-sql/functions/day-transact-sql.md)|DAY ( *date* )|Restituisce un intero che rappresenta la parte relativa al giorno dell'elemento *date* specificato.|**int**|Deterministico|
|[MONTH](../../t-sql/functions/month-transact-sql.md)|MONTH ( *date* )|Restituisce un intero che rappresenta la parte relativa al mese dell'elemento *date* specificato.|**int**|Deterministico|
|[YEAR](../../t-sql/functions/year-transact-sql.md)|YEAR ( *date* )|Restituisce un intero che rappresenta la parte relativa all'anno dell'elemento *date* specificato.|**int**|Deterministico|

### <a name="functions-that-return-date-and-time-values-from-their-parts"></a><a name="fromParts"></a> Funzioni che restituiscono i valori di data e ora dalle relative parti

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[DATEFROMPARTS](../../t-sql/functions/datefromparts-transact-sql.md)|DATEFROMPARTS ( *year*, *month*, *day* )|Restituisce un valore di tipo **date** per l'anno, il mese e il giorno specificati.|**date**|Deterministico|
|[DATETIME2FROMPARTS](../../t-sql/functions/datetime2fromparts-transact-sql.md)|DATETIME2FROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *fractions*, *precision*)|Restituisce un valore **datetime2** per la data e l'ora specificate, con la precisione indicata.|**datetime2(** _precision_ **)**|Deterministico|
|[DATETIMEFROMPARTS](../../t-sql/functions/datetimefromparts-transact-sql.md)|DATETIMEFROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *milliseconds*)|Restituisce un valore di tipo **datetime** per la data e l'ora specificate.|**datetime**|Deterministico|
|[DATETIMEOFFSETFROMPARTS](../../t-sql/functions/datetimeoffsetfromparts-transact-sql.md)|DATETIMEOFFSETFROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *fractions*, *hour_offset*, *minute_offset*, *precision*)|Restituisce un valore **datetimeoffset** per la data e l'ora specificate, con gli offset e la precisione indicati.|**datetimeoffset(** _precision_ **)**|Deterministico|
|[SMALLDATETIMEFROMPARTS](../../t-sql/functions/smalldatetimefromparts-transact-sql.md)|SMALLDATETIMEFROMPARTS ( *year*, *month*, *day*, *hour*, *minute* )|Restituisce un valore di tipo **smalldatetime** per la data e l'ora specificate.|**smalldatetime**|Deterministico|
|[TIMEFROMPARTS](../../t-sql/functions/timefromparts-transact-sql.md)|TIMEFROMPARTS ( *hour*, *minute*, *seconds*, *fractions*, *precision* )|Restituisce un valore **time** per l'ora specificata, con la precisione indicata.|**time(** _precision_ **)**|Deterministico|

### <a name="functions-that-return-date-and-time-difference-values"></a><a name="GetDateandTimeDifference"></a> Funzioni che restituiscono valori di differenza di data e ora

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[DATEDIFF](../../t-sql/functions/datediff-transact-sql.md)|DATEDIFF ( *datepart* , *startdate* , *enddate* )|Restituisce il numero di limiti degli elementi *datepart* di data o ora che si sovrappongono tra due date specificate.|**int**|Deterministico|
|[DATEDIFF_BIG](../../t-sql/functions/datediff-big-transact-sql.md)|DATEDIFF_BIG ( *datepart* , *startdate* , *enddate* )|Restituisce il numero di limiti degli elementi *datepart* di data o ora che si sovrappongono tra due date specificate.|**bigint**|Deterministico|

### <a name="functions-that-modify-date-and-time-values"></a><a name="ModifyDateandTimeValues"></a> Funzioni che modificano i valori di data e ora

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[DATEADD](../../t-sql/functions/dateadd-transact-sql.md)|DATEADD (*datepart* , *number* , *date* )|Restituisce un nuovo valore **datetime** aggiungendo un intervallo alla *datepart* specificata della *data* specificata.|Tipo di dati dell'argomento *date*|Deterministico|
|[EOMONTH](../../t-sql/functions/eomonth-transact-sql.md)|EOMONTH ( *start_date* [, *month_to_add* ] )|Restituisce l'ultimo giorno del mese contenente la data specificata, con un offset facoltativo.|Il tipo restituito corrisponde al tipo dell'argomento *start_date* o, in alternativa, al tipo di dati **date**.|Deterministico|
|[SWITCHOFFSET](../../t-sql/functions/switchoffset-transact-sql.md)|SWITCHOFFSET (*DATETIMEOFFSET* , *time_zone*)|SWITCHOFFSET modifica la differenza di fuso orario di un valore DATETIMEOFFSET e mantiene il valore UTC.|**datetimeoffset** con la precisione frazionaria di *DATETIMEOFFSET*|Deterministico|
|[TODATETIMEOFFSET](../../t-sql/functions/todatetimeoffset-transact-sql.md)|TODATETIMEOFFSET (*expression* , *time_zone*)|TODATETIMEOFFSET trasforma il valore datetime2 in un valore datetimeoffset. *TODATETIMEOFFSET* interpreta il valore datetime2 come ora locale in base al valore time_zone specificato.|**datetimeoffset** con la precisione frazionaria dell'argomento *datetime*|Deterministico|

### <a name="functions-that-set-or-return-session-format-functions"></a><a name="SetorGetSessionFormatFunctions"></a> Funzioni che impostano o restituiscono il formato della sessione

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[@@DATEFIRST](../../t-sql/functions/datefirst-transact-sql.md)|@@DATEFIRST|Restituisce il valore corrente, per la sessione, di SET DATEFIRST.|**tinyint**|Non deterministica|
|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)|SET DATEFIRST { *number* &#124; **\@** _number_var_ }|Imposta il primo giorno della settimana su un numero compreso tra 1 e 7.|Non applicabile|Non applicabile|
|[SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|SET DATEFORMAT { *format* &#124; **@** _format_var_ }|Imposta l'ordine delle datepart (mese/giorno/anno) per l'immissione di dati **datetime** o **smalldatetime**.|Non applicabile|Non applicabile|
|[@@LANGUAGE](../../t-sql/functions/language-transact-sql.md)|@@LANGUAGE|Restituisce il nome della lingua attualmente in uso. @@LANGUAGE non è una funzione di data o ora. Comunque, l'impostazione della lingua può influire sull'output di funzioni di data.|Non applicabile|Non applicabile|
|[SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)|SET LANGUAGE { [ N ] **'** _language_ **'** &#124; **\@** _language_var_ }|Imposta la lingua per la sessione e i messaggi di sistema. SET LANGUAGE non è una data o una funzione dell'ora. Comunque, l'impostazione della lingua influisce sull'output di funzioni di data.|Non applicabile|Non applicabile|
|[sp_helplanguage](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md)|**sp_helplanguage** [ [ **@language =** ] **'** _language_ **'** ]|Restituisce informazioni su formati della data di tutte le lingue supportate. **sp_helplanguage** non è una stored procedure relativa a data o ora. Comunque, l'impostazione della lingua influisce sull'output di funzioni di data.|Non applicabile|Non applicabile|

### <a name="functions-that-validate-date-and-time-values"></a><a name="ValidateDateandTimeValues"></a> Funzioni che convalidano i valori di data e ora

|Funzione|Sintassi|Valore restituito|Tipo di dati restituito|Determinismo|
|---|---|---|---|---|
|[ISDATE](../../t-sql/functions/isdate-transact-sql.md)|ISDATE ( *expression* )|Determina se un'espressione di input **datetime** o **smalldatetime** ha un valore di data o ora valido.|**int**|La funzione ISDATE è deterministica solo se usata con la funzione CONVERT, quando viene specificato il parametro di stile della funzione CONVERT e se lo stile è diverso da 0, 100, 9 o 109.|

## <a name="date-and-time-related-topics"></a><a name="DateandTimeRelatedTopics"></a> Argomenti correlati a data e ora

|Argomento|Descrizione|
|-----------|-----------------|
|[FORMAT](../../t-sql/functions/format-transact-sql.md)|Restituisce un valore formattato con il formato specificato e impostazioni cultura facoltative. Utilizzare la funzione FORMAT per formattare in base alle impostazioni locali i valori numerici e di data/ora come stringhe.|
|[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)|Offre informazioni sulla conversione di valori data e ora in e da valori letterali stringa e altri formati di data e ora.|
|[Scrittura di istruzioni Transact-SQL internazionali](../../relational-databases/collations/write-international-transact-sql-statements.md)|Offre linee guida per la portabilità di database e applicazioni database che usano istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] da una lingua all'altra o che supportano più lingue.|
|[Funzioni scalari ODBC &#40;Transact-SQL&#41;](../../t-sql/functions/odbc-scalar-functions-transact-sql.md)|Offre informazioni sulle funzioni scalari ODBC disponibili per l'uso all'interno di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], incluse le funzioni di data e ora ODBC.|
|[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)|Offre informazioni sulla conversione del fuso orario.|

## <a name="see-also"></a>Vedere anche

- [Funzioni](../../t-sql/functions/functions.md)
- [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
