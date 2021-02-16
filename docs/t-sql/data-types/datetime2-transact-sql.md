---
description: datetime2 (Transact-SQL)
title: datetime2 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime2
- datetime2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- dates [SQL Server], data types
- date and time [SQL Server], datetime2
- data types [SQL Server], date and time
- datetime2 data type [SQL Server]
ms.assetid: 868017f3-214f-43ef-8536-cc1632a2288f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bdacc09933666e1deb2d238f867441a8cbc8f6c
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525188"
---
# <a name="datetime2-transact-sql"></a>datetime2 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definisce una data costituita dalla combinazione di un'ora del giorno espressa nel formato 24 ore. **datetime2** può essere considerato un'estensione del tipo **datetime** esistente con un più ampio intervallo di date, una maggiore precisione frazionaria predefinita e una precisione specificata dall'utente facoltativa.
  
## <a name="datetime2-description"></a>Descrizione di datetime2
  
|Proprietà|valore|  
|--------------|-----------|  
|Sintassi|**datetime2** [ (*precisione in secondi frazionari*) ]|  
|Uso|DECLARE \@MyDatetime2 **datetime2(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime2(7)** )|  
|Formato predefinito dei valori letterali stringa<br /><br /> (utilizzato per client legacy)|AAAA-MM-GG hh:mm:ss[.secondi frazionari]<br /><br /> Per ulteriori informazioni, vedere la sezione seguente relativa alla compatibilità con le versioni precedenti per i client legacy.|  
|Intervallo di date|Da 01-01-0001 a 31-12-9999<br /><br /> Dal 1º gennaio 1 e.c. al 31 dicembre 9999 e.c.|  
|Intervallo di ore|da 00:00:00 a 23:59:59.9999999|  
|Intervallo di differenze di fuso orario|nessuno|  
|Intervalli di elementi|AAAA rappresenta un numero di quattro cifre compreso tra 0001 e 9999, indicante l'anno.<br /><br /> MM rappresenta un numero di due cifre compreso tra 01 e 12, indicante un mese dell'anno specificato.<br /><br /> GG rappresenta un numero di due cifre compreso tra 01 e 31, a seconda del mese, indicante il giorno del mese specificato.<br /><br /> hh rappresenta un numero di due cifre compreso tra 00 e 23, indicante l'ora.<br /><br /> mm rappresenta un numero di due cifre compreso tra 00 e 59, indicante i minuti.<br /><br /> ss rappresenta un numero di due cifre compreso tra 00 e 59, indicante i secondi.<br /><br /> n* rappresenta un numero composto da 0 a 7 cifre e compreso tra 0 e 9999999, indicante i secondi frazionari. In Informatica i secondi frazionari verranno troncati quando n > 3.|  
|Lunghezza in caratteri|Da un minimo di 19 posizioni (AAAA-MM-GG hh:mm:ss ) a un massimo di 27 posizioni (AAAA-MM-GG hh:mm:ss.0000000)|  
|Precisione, scala|Da 0 a 7 cifre, con un'accuratezza di 100 nanosecondi. La precisione predefinita è 7 cifre.|  
|Dimensioni dello spazio di archiviazione <sup>1</sup>|6 byte per una precisione inferiore a 3.<br/>7 byte per una precisione compresa tra 3 e 4.<br/>Tutte le altre precisioni richiedono 8 byte.<sup>2</sup>|  
|Accuratezza|100 nanosecondi|  
|Valore predefinito|1900-01-01 00:00:00|  
|Calendario|Gregoriano|  
|Precisione in secondi frazionari definita dall'utente|Sì|  
|Considerazione e conservazione delle differenze di fuso orario|No|  
|Considerazione dell'ora legale|No|  

<sup>1</sup> I valori specificati si riferiscono a rowstore non compresso. L'uso della [compressione dei dati](../../relational-databases/data-compression/data-compression.md) o di [columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) potrebbe alterare le dimensioni dello spazio di archiviazione per ogni precisione. Inoltre, le dimensioni dello spazio di archiviazione su disco e in memoria potrebbero essere diverse. Ad esempio, i valori di **datetime2** richiedono sempre 8 byte in memoria quando si usa la modalità batch.

<sup>2</sup> Quando viene eseguito il cast di un valore **datetime2** a un valore **varbinary**, viene aggiunto un altro byte al valore **varbinary** per archiviare la precisione.

Per i metadati del tipo di dati, vedere [sys.systypes &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md) o [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md). Precisione e scala sono variabili per alcuni tipi di dati di data e ora. Per ottenere precisione e scala per una colonna, vedere [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md), [COL_LENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/col-length-transact-sql.md) o [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).
  
## <a name="supported-string-literal-formats-for-datetime2"></a>Formati di valore letterale stringa supportati per datetime2
Nelle tabelle seguenti sono elencati i formati di valore letterale stringa ISO 8601 e ODBC supportati per **datetime2**. Per informazioni sui formati alfabetico, numerico, non separato e ora per le parti della data e dell'ora di **datetime2**, vedere [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) e [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Descrizioni|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.nnnnnnn]<br /><br /> YYYY-MM-DDThh:mm:ss[.nnnnnnn]|Su questo formato non influiscono le impostazioni locali delle sessioni SET LANGUAGE e SET DATEFORMAT. Il carattere **T**, i due punti (:) e il punto (.) sono inclusi nel valore letterale stringa, ad esempio "2007-05-02T19:58:47.1234567".|  
  
|ODBC|Descrizione|  
|---|---|
|{ ts 'aaaa-mm-gg hh:mm:ss[.secondi frazionari]' }|Specifico delle API ODBC:<br /><br /> Il numero di cifre a destra del separatore decimale che rappresenta i secondi frazionari comprende da 0 a 7 cifre (100 nanosecondi).|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformità agli standard ANSI e ISO 8601  
La conformità agli standard ANSI e ISO 8601 di [date](../../t-sql/data-types/date-transact-sql.md) e [time](../../t-sql/data-types/time-transact-sql.md) si applica a **datetime2**.
  
##  <a name="backward-compatibility-for-down-level-clients"></a>Compatibilità con le versioni precedenti dei client legacy  
Alcune versioni precedenti dei client non supportano i tipi di dati **time**, **date**, **datetime2** e **datetimeoffset**. Nella tabella seguente viene illustrato il mapping del tipo tra un'istanza di livello principale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i client legacy.
  
|Tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Formato predefiniti dei valori letterali stringa passati al client legacy|ODBC delle versioni precedenti|OLEDB delle versioni precedenti|JDBC delle versioni precedenti|SQLCLIENT delle versioni precedenti|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**datetime2**|AAAA-MM-GG hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
  
## <a name="converting-date-and-time-data"></a>Conversione dei dati relativi a data e ora
Nella conversione di tipi di dati relativi alla data e all'ora, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono rifiutati tutti i valori non riconosciuti come date o orari. Per informazioni sull'uso delle funzioni CAST e CONVERT con i dati relativi a data e ora, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
### <a name="converting-other-date-and-time-types-to-the-datetime2-data-type"></a>Conversione di altri tipi di data e ora nel tipo di dati datetime2
Nella sezione seguente viene descritto il risultato della conversione di altri tipi di dati di data e ora nel tipo di dati **datetime2**.  
  
Quando la conversione è da **date** vengono copiati l'anno, il mese e il giorno.  Il componente relativo all'ora viene impostato su 00:00:00.0000000.  Nel codice seguente vengono illustrati i risultati della conversione di un valore `date` in un valore `datetime2`.  
  
```sql
DECLARE @date date = '12-21-16';
DECLARE @datetime2 datetime2 = @date;

SELECT @datetime2 AS '@datetime2', @date AS '@date';
  
--Result  
--@datetime2                  @date
----------------------------- ----------
--2016-12-21 00:00:00.0000000 2016-12-21
```  
  
Quando la conversione è da **time(n)**, il componente relativo all'ora viene copiato e quello relativo alla data viene impostato su "1900-01-01". Nell'esempio seguente vengono illustrati i risultati della conversione di un valore `time(7)` in un valore `datetime2`.  
  
```sql
DECLARE @time time(7) = '12:10:16.1234567';
DECLARE @datetime2 datetime2 = @time;

SELECT @datetime2 AS '@datetime2', @time AS '@time';
  
--Result  
--@datetime2                  @time
----------------------------- ----------------
--1900-01-01 12:10:16.1234567 12:10:16.1234567
```  
  
Quando la conversione viene eseguita da **smalldatetime**, le ore e i minuti vengono copiati, mentre i secondi e i secondi frazionari vengono impostati su 0. Nel codice seguente vengono illustrati i risultati della conversione di un valore `smalldatetime` in un valore `datetime2`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';
DECLARE @datetime2 datetime2 = @smalldatetime;

SELECT @datetime2 AS '@datetime2', @smalldatetime AS '@smalldatetime'; 
  
--Result  
--@datetime2                  @smalldatetime
----------------------------- -----------------------
--2016-12-01 12:32:00.0000000 2016-12-01 12:32:00 
```  
  
Quando la conversione viene eseguita da **datetimeoffset(n)**, i componenti di data e ora vengono copiati. Il fuso orario viene troncato. Nell'esempio seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(7)` in un valore `datetime2`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(7) = '2016-10-23 12:45:37.1234567 +10:0';
DECLARE @datetime2 datetime2 = @datetimeoffset;

SELECT @datetime2 AS '@datetime2', @datetimeoffset AS '@datetimeoffset'; 
  
--Result  
--@datetime2                  @datetimeoffset
----------------------------- ----------------------------------
--2016-10-23 12:45:37.1234567 2016-10-23 12:45:37.1234567 +10:00
```  

Quando la conversione è da **datetime**, la data e l'ora vengono copiate. La precisione frazionaria viene estesa a 7 cifre. Nell'esempio seguente vengono illustrati i risultati della conversione di un valore `datetime` in un valore `datetime2`.

```sql
DECLARE @datetime datetime = '2016-10-23 12:45:37.333';
DECLARE @datetime2 datetime2 = @datetime;

SELECT @datetime2 AS '@datetime2', @datetime AS '@datetime';
   
--Result  
--@datetime2                  @datetime
------------------------- ---------------------------
--2016-10-23 12:45:37.3333333 2016-10-23 12:45:37.333
```  

> [!NOTE]
> Nel livello di compatibilità del database 130, le conversioni implicite dai tipi di dati datetime a datetime2 mostrano una maggiore precisione prevedendo i millisecondi frazionari, risultanti in diversi valori convertiti (come appare nell'esempio precedente). Usare il cast esplicito per il tipo di dati datetime2 ogni volta che si presenta uno scenario di confronto misto tra tipi di dati datetime e datetime2. Per altre informazioni, fare riferimento a questo [articolo del supporto tecnico Microsoft](https://support.microsoft.com/help/4010261).

### <a name="converting-string-literals-to-datetime2"></a>Conversione di valori letterali stringa nel tipo di dati datetime2  
Le conversioni da valori letterali stringa a tipi di data e ora sono consentite se tutte le parti delle stringhe hanno formati validi. In caso contrario, viene generato un errore di runtime. Le conversioni implicite o esplicite che non specificano uno stile, dai tipi di data e ora ai valori letterali stringa, saranno nel formato predefinito della sessione corrente. Nella tabella seguente vengono illustrate le regole per la conversione di un valore letterale stringa nel tipo di dati **datetime2**.
  
|Valore letterale stringa di input|**datetime2(n)**|  
|---|---|
|ODBC DATE|Viene eseguito il mapping dei valori letterali stringa ODBC al tipo di dati **datetime**. Tutte le operazione di assegnazione dai valori letterali di ODBC DATETIME in tipi **datetime2** determineranno una conversione implicita tra **datetime** e questo tipo in base a quanto definito dalle regole di conversione.|  
|ODBC TIME|Vedere la regola relativa a ODBC DATE.|  
|ODBC DATETIME|Vedere la regola relativa a ODBC DATE.|  
|Solo DATE|Il valore predefinito per la parte di TIME è 00:00:00.|  
|solo TIME|Il valore predefinito per la parte di DATE è 1900-1-1.|  
|solo TIMEZONE|Vengono forniti i valori predefiniti.|  
|DATE + TIME|Semplice|  
|DATE + TIMEZONE|Non consentiti.|  
|TIME + TIMEZONE|Il valore predefinito per la parte di DATE è 1900-1-1. L'input TIMEZONE viene ignorato.|  
|DATE + TIME + TIMEZONE|Verrà utilizzato DATETIME locale.|  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente vengono confrontati i risultati dell'esecuzione del cast di una stringa ai tipi di dati **date** e **time**.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Tipo di dati|Output|  
|---|---|
|**time**|12:35:29.1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29.1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
