---
description: datetimeoffset (Transact-SQL)
title: datetimeoffset (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetimeoffset_TSQL
- datetimeoffset
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetimeoffset
- datetimeoffset data type [SQL Server]
- dates [SQL Server], data types
- data types [SQL Server], date and time
- time zones [SQL Server]
ms.assetid: a0455b71-ca25-476e-a7a8-0770f1860bb7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 76913dfeded63354a8597027380e7e966ff73f31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172579"
---
# <a name="datetimeoffset-transact-sql"></a>datetimeoffset (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definisce una data in combinazione con un'ora del giorno con considerazione del fuso orario ed espressa nel formato 24 ore.

## <a name="datetimeoffset-description"></a>Descrizione di datetimeoffset
  
|Proprietà|valore|  
|---|---|
|Sintassi|**datetimeoffset** [ (*precisione in secondi frazionari*) ]|  
|Uso|DECLARE \@MyDatetimeoffset **datetimeoffset(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetimeoffset(7)** )|  
|Formati predefiniti per i valori letterali stringa (utilizzati per il client legacy)|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [{+&#124;-}hh:mm]<br /><br /> Per ulteriori informazioni, vedere la sezione seguente relativa alla compatibilità con le versioni precedenti per i client legacy.|  
|Intervallo di date|Da 01-01-0001 a 31-12-9999<br /><br /> Dal 1 gennaio 1 al 31 dicembre 9999|  
|Intervallo di ore|da 00:00:00 a 23:59:59.9999999|  
|Intervallo di differenze di fuso orario|Da -14:00 a +14:00|  
|Intervalli di elementi|AAAA rappresenta un numero di quattro cifre compreso tra 0001 e 9999 indicante l'anno.<br /><br /> MM rappresenta un numero di due cifre compreso tra 01 e 12 indicante un mese dell'anno specificato.<br /><br /> GG rappresenta un numero di due cifre compreso tra 01 e 31, a seconda del mese, indicante il giorno del mese specificato.<br /><br /> hh rappresenta un numero di due cifre compreso tra 00 e 23 indicante l'ora.<br /><br /> mm rappresenta un numero di due cifre compreso tra 00 e 59 indicante i minuti.<br /><br /> ss rappresenta un numero di due cifre compreso tra 00 e 59 indicante i secondi.<br /><br /> n* rappresenta un numero composto da un numero di cifre da 0 a 7 e compreso tra 0 e 9999999, indicante i secondi frazionari.<br /><br /> hh rappresenta un numero di due cifre compreso tra -14 e +14. <br /><br /> mm rappresenta un numero di due cifre compreso tra 00 e 59.|  
|Lunghezza in caratteri|Da un minimo di 26 posizioni (YYYY-MM-DD hh:mm:ss {+&#124;-}hh:mm) a un massimo di 34 (YYYY-MM-DD hh:mm:ss.nnnnnnn {+&#124;-}hh:mm)|  
|Precisione, scala|Vedere la tabella riportata di seguito.|  
|Dimensioni dello spazio di archiviazione|10 byte, fissa è l'impostazione predefinita con l'impostazione predefinita di 100 ns di precisione in secondi frazionari.|  
|Accuratezza|100 nanosecondi|  
|Valore predefinito|1900-01-01 00:00:00 00:00|  
|Calendario|Gregoriano|  
|Precisione in secondi frazionari definita dall'utente|Sì|  
|Considerazione e conservazione delle differenze di fuso orario|Sì|  
|Considerazione dell'ora legale|No|  
  
|Scala specificata|Risultato (precisione, scala)|Lunghezza della colonna (byte)|Precisione in secondi frazionari|  
|---|---|---|---|
|**datetimeoffset**|(34,7)|10|7|  
|**datetimeoffset(0)**|(26,0)|8|0-2|  
|**datetimeoffset(1)**|(28,1)|8|0-2|  
|**datetimeoffset(2)**|(29,2)|8|0-2|  
|**datetimeoffset(3)**|(30,3)|9|3-4|  
|**datetimeoffset(4)**|(31,4)|9|3-4|  
|**datetimeoffset(5)**|(32,5)|10|5-7|  
|**datetimeoffset(6)**|(33,6)|10|5-7|  
|**datetimeoffset(7)**|(34,7)|10|5-7|  
  
## <a name="supported-string-literal-formats-for-datetimeoffset"></a>Formati di valore letterale stringa supportati per datetimeoffset
Nella tabella seguente vengono elencati i formati di valore letterale stringa ISO 8601 supportati per **datetimeoffset**. Per informazioni sui formati alfabetico, numerico, non separato e ora per le parti della data e dell'ora di **datetimeoffset**, vedere [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) e [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Descrizione|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.nnnnnnn][{+&#124;-}hh:mm]|Su questi due formati non influiscono le impostazioni locali delle sessioni SET LANGUAGE e SET DATEFORMAT. Non sono consentiti spazi tra le parti **datetimeoffset** e **datetime**.|  
|AAAA-MM-GGThh:mm:ss[.nnnnnnn]Z (UTC)|In base alla definizione ISO questo formato indica che la parte **datetime** deve essere espressa in formato UTC (Coordinated Universal Time). Ad esempio, 1999-12-12 12:30:30.12345 -07: 00 deve essere rappresentata come 1999-12-12 19:30:30.12345Z.|  
  
## <a name="time-zone-offset"></a>Differenza di fuso orario
Una differenza di fuso orario specifica la differenza di fuso orario rispetto all'ora UTC per un valore **time** o **datetime**. La differenza di fuso orario può essere rappresentata nel formato [+|-] hh:mm:
-   hh è un numero di due cifre, compreso tra 00 e 14, che rappresenta il numero di ore della differenza di fuso orario.  
-   mm è un numero di due cifre, compreso tra 00 e 59, che rappresenta il numero di minuti aggiuntivi della differenza di fuso orario.  
-   \+ (più) o - (meno) è il segno obbligatorio per la differenza di fuso orario. Indica se la differenza di fuso orario viene aggiunta o sottratta dall'ora UTC per ottenere l'ora locale. L'intervallo valido della differenza di fuso orario è da -14:00 a +14:00.  
  
L'intervallo di differenza di fuso orario segue lo standard XML W3C per la definizione di schemi XSD, leggermente diverso dalla definizione standard di SQL 2003, da 12:59 a +14:00.
  
Il parametro facoltativo di tipo *precisione in secondi frazionari* specifica il numero di cifre per la parte relativa ai secondi frazionari. Questo valore può essere un numero intero con un numero di cifre compreso tra 0 e 7 (100 nanosecondi). Il valore predefinito della *precisione in secondi frazionari* è 100 ns (sette cifre per la parte relativa ai secondi frazionari).
  
I dati sono archiviati nel database ed elaborati, confrontati, ordinati e indicizzati nel server come in UTC. La differenza di fuso orario viene mantenuta nel database per il recupero.
  
La differenza di fuso orario specificata deve essere sensibile all'ora legale e regolata sui valori di **datetime** appartenenti al periodo in cui è in vigore l'ora legale.
  
Per il tipo **datetimeoffset**, i valori **datetime** UTC e locali (per la differenza di fuso orario persistente o convertita), vengono convalidati durante le operazioni aritmetiche e di inserimento, aggiornamento, conversione o assegnazione. Il rilevamento di qualsiasi valore **datetime** UTC o locale (per la differenza di fuso orario persistente o convertita) non valido genera un errore. Ad esempio, 9999-12-31 10:10:00 è valido in UTC ma causa overflow nell'ora locale per la differenza di fuso orario +13:50.
  
Per convertire una data in un valore **datetimeoffset** corrispondente in un fuso orario di destinazione, vedere [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md).
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformità agli standard ANSI e ISO 8601  
Le sezioni sulla conformità agli standard ANSI e ISO 8601 degli argomenti [date](../../t-sql/data-types/date-transact-sql.md) e [time](../../t-sql/data-types/time-transact-sql.md) si applicano a **datetimeoffset**.
  
## <a name="backward-compatibility-for-down-level-clients"></a>Compatibilità con le versioni precedenti dei client
Alcune versioni precedenti dei client non supportano i tipi di dati **time**, **date**, **datetime2** e **datetimeoffset**. Nella tabella seguente viene illustrato il mapping del tipo tra un'istanza di livello principale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i client legacy.
  
|Tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Formato predefiniti dei valori letterali stringa passati al client legacy|ODBC delle versioni precedenti|OLEDB delle versioni precedenti|JDBC delle versioni precedenti|SQLCLIENT delle versioni precedenti|  
|---|---|---|---|---|---|
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**datetime2**|AAAA-MM-GG hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
  
## <a name="converting-date-and-time-data"></a>Conversione dei dati relativi a data e ora
Nella conversione di tipi di dati relativi alla data e all'ora, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono rifiutati tutti i valori non riconosciuti come date o orari. Per informazioni sull'uso delle funzioni CAST e CONVERT con i dati relativi a data e ora, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
### <a name="converting-datetimeoffset-data-type-to-other-date-and-time-types"></a>Conversione del tipo di dati datetimeoffset in altri tipi di dati relativi a data e ora
Questa sezione descrive il risultato della conversione di un tipo di dati **datetimeoffset** in altri tipi di dati relativi a data e ora.
  
Nel caso della conversione in **date** vengono copiati l'anno, il mese e il giorno. Nel codice seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(4)` in un valore `date`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10 +01:00';  
DECLARE @date date= @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @date AS 'date';  
  
--Result  
--@datetimeoffset                date  
-------------------------------- ----------  
--2025-12-10 12:32:10.0000 +01:0 2025-12-10  
--  
--(1 row(s) affected)  
  
```  
  
Se viene eseguita la conversione in **time(n)**, vengono copiati ora, minuti, secondi e secondi frazionari. Il valore del fuso orario viene troncato. Quando la precisione del valore **datetimeoffset(n)** è maggiore di quella del valore **time(n)**, la prima precisione viene arrotondata. Nel codice seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(4)` in un valore `time(3)`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @time time(3) = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @time AS 'time';  
  
--Result  
--@datetimeoffset                time  
-------------------------------- ------------  
-- 2025-12-10 12:32:10.1237 +01:00    12:32:10.124  
  
--  
--(1 row(s) affected)  
  
```  
  
Quando viene eseguita la conversione in **datetime**, i valori della data e dell'ora vengono copiati, mentre il fuso orario viene troncato. Quando la precisione frazionaria del valore **datetimeoffset(n)** è maggiore di tre cifre, il valore viene troncato. Nel codice seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(4)` in un valore `datetime`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @datetime AS 'datetime';  
  
--Result  
--@datetimeoffset                datetime  
-------------------------------- -----------------------  
--2025-12-10 12:32:10.1237 +01:0 2025-12-10 12:32:10.123  
--  
--(1 row(s) affected)  
```  
  
Per le conversioni in **smalldatetime**, la data e l'ora vengono copiate, i minuti vengono arrotondati rispetto al valore dei secondi e i secondi vengono impostati su 0. Nel codice seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(3)` in un valore `smalldatetime`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(3) = '1912-10-25 12:24:32 +10:0';  
DECLARE @smalldatetime smalldatetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetimeoffset                @smalldatetime  
-------------------------------- -----------------------  
--1912-10-25 12:24:32.000 +10:00 1912-10-25 12:25:00  
--  
--(1 row(s) affected)  
```  
  
Se la conversione viene eseguita in **datetime2(n)**, data e ora vengono copiate nel valore **datetime2** e il fuso orario viene troncato. Se la precisione del valore **datetime2(n)** è maggiore di quella del valore **datetimeoffset(n)**, i secondi frazionari vengono troncati. Nel codice seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(4)` in un valore `datetime2(3)`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1912-10-25 12:24:32.1277 +10:0';  
DECLARE @datetime2 datetime2(3)=@datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @datetime2 AS '@datetime2';  
  
--Result  
@datetimeoffset                    @datetime2  
---------------------------------- ----------------------  
1912-10-25 12:24:32.1277 +10:00    1912-10-25 12:24:32.12  
  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-datetimeoffset"></a>Conversione di valori letterali stringa nel tipo di dati datetimeoffset
Le conversioni da valori letterali stringa a tipi di data e ora sono consentite se tutte le parti delle stringhe hanno formati validi. In caso contrario, viene generato un errore di runtime. Le conversioni implicite o esplicite che non specificano uno stile, dai tipi di data e ora ai valori letterali stringa, saranno nel formato predefinito della sessione corrente. Nella tabella seguente vengono illustrate le regole per la conversione di un valore letterale stringa nel tipo di dati **datetimeoffset**.
  
|Valore letterale stringa di input|**datetimeoffset(n)**|  
|---|---|
|ODBC DATE|Viene eseguito il mapping dei valori letterali stringa ODBC al tipo di dati **datetime**. Tutte le operazione di assegnazione dai valori letterali di ODBC DATETIME in tipi **datetimeoffset** determineranno una conversione implicita tra **datetime** e questo tipo in base a quanto definito dalle regole di conversione.|  
|ODBC TIME|Vedere la regola relativa a ODBC DATE.|  
|ODBC DATETIME|Vedere la regola relativa a ODBC DATE.|  
|Solo DATE|Il valore predefinito per la parte di TIME è 00:00:00. Il valore predefinito di TIMEZONE è +00:00:00.|  
|solo TIME|Il valore predefinito per la parte di DATE è 1900-1-1. Il valore predefinito di TIMEZONE è +00:00.|  
|solo TIMEZONE|Vengono forniti i valori predefiniti.|  
|DATE + TIME|Il valore predefinito di TIMEZONE è +00:00:00.|  
|DATE + TIMEZONE|Non consentito|  
|TIME + TIMEZONE|Il valore predefinito per la parte di DATE è 1900-1-1.|  
|DATE + TIME + TIMEZONE|Semplice|  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente vengono confrontati i risultati dell'esecuzione del cast di una stringa ai tipi di dati **date** e **time**.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset'  
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetimeoffset(7)) AS  
        'datetimeoffset IS08601';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Tipo di dati|Output|  
|---|---|
|**Time**|12:35:29. 1234567|  
|**Data**|2007-05-08|  
|**Smalldatetime**|2007-05-08 12:35:00|  
|**DateTime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**Datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  
