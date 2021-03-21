---
description: datetime (Transact-SQL)
title: datetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime_TSQL
- datetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetime
- dates [SQL Server], data types
- datetime data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: 9bd1cc5b-227b-4032-95d6-7581ddcc9924
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0b1d5deb8e44107b921b16627728bff0caa5741a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735641"
---
# <a name="datetime-transact-sql"></a>datetime (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definisce una data costituita dalla combinazione di un'ora del giorno e di secondi frazionari ed espressa nel formato 24 ore.
  
> [!NOTE]  
>  Usare i tipi di dati **time**, **date**, **datetime2** e **datetimeoffset** per la creazione di nuovo codice. Questi tipi sono conformi allo standard SQL. e offrono una migliore portabilità. **time**, **datetime2** e **datetimeoffset** offrono una maggiore precisione dei secondi. **datetimeoffset** offre il supporto del fuso orario per le applicazioni distribuite globalmente.  
  
## <a name="datetime-description"></a>Descrizione di datetime  
  
|Proprietà|valore|  
|---|---|
|Sintassi|**datetime**|  
|Uso|DECLARE \@MyDatetime **datetime**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime** )|  
|Formati predefiniti dei valori letterali stringa<br /><br /> (utilizzato per client legacy)|Non applicabile|  
|Intervallo di date|Da 01.01.53 a 31.12.99|  
|Intervallo di ore|da 00:00:00 a 23:59:59.997|  
|Intervallo di differenze di fuso orario|nessuno|  
|Intervalli di elementi|AAAA rappresenta un numero di quattro cifre compreso tra 1753 e 9999 indicante l'anno.<br /><br /> MM rappresenta un numero di due cifre compreso tra 01 e 12 indicante un mese dell'anno specificato.<br /><br /> GG rappresenta un numero di due cifre compreso tra 01 e 31, a seconda del mese, indicante il giorno del mese specificato.<br /><br /> hh rappresenta un numero di due cifre compreso tra 00 e 23 indicante l'ora.<br /><br /> mm rappresenta un numero di due cifre compreso tra 00 e 59 indicante i minuti.<br /><br /> ss rappresenta un numero di due cifre compreso tra 00 e 59 indicante i secondi.<br /><br /> n* rappresenta un numero composto da un numero di cifre da 0 a 3 e compreso tra 0 e 999, indicante i secondi frazionari.|  
|Lunghezza in caratteri|Da 19 posizioni minimo a 23 massimo|  
|Dimensioni dello spazio di archiviazione|8 byte|  
|Accuratezza|Arrotondato a incrementi di 0,000, 0,003 o 0,007 secondi|  
|Valore predefinito|1900-01-01 00:00:00|  
|Calendario|Gregoriano (non include l'intervallo completo di anni)|  
|Precisione in secondi frazionari definita dall'utente|No|  
|Considerazione e conservazione delle differenze di fuso orario|No|  
|Considerazione dell'ora legale|No|  
  
## <a name="supported-string-literal-formats-for-datetime"></a>Formati di valore letterale stringa supportati per datetime  
Nelle tabelle seguenti sono elencati i formati di valore letterale stringa supportati per **datetime**. Fatta eccezione per ODBC, i valori letterali stringa **datetime** sono compresi tra virgolette singole ('), ad esempio, 'string_literaL'. Se l'ambiente è diverso da **us_english**, i valori letterali stringa devono essere nel formato N'string_literaL'.
  
|Numeric|Descrizione|  
|---|---|
|Formati di data:<br /><br /> [0]4/15/[19]96 -- (mga)<br /><br /> [0]4-15-[19]96 -- (mga)<br /><br /> [0]4.15.[19]96 -- (mga)<br /><br /> [0]4/[19]96/15 -- (mag)<br /><br /> 15/[0]4/[19]96 -- (gma)<br /><br /> 15/[19]96/[0]4 -- (gam)<br /><br /> [19]96/15/[0]4 -- (agm)<br /><br /> [19]96/[0]4/15 -- (amg)<br /><br /> Formati di ora:<br /><br /> 14:30<br /><br /> 14:30[:20:999]<br /><br /> 14:30[:20.9]<br /><br /> 4am<br /><br /> 4 PM|È possibile specificare i dati relativi alla data specificando il mese in formato numerico. Ad esempio, 5/20/97 rappresenta il ventesimo giorno di maggio 1997. Quando si utilizza il formato di data numerico, specificare il mese, il giorno e l'anno all'interno della stessa stringa utilizzando come separatori la barra (/), il segno meno (-) o il punto (.). Il formato corretto è il seguente:<br /><br /> *numero separatore numero separatore numero[ora] [ora]*<br /><br /> <br /><br /> Quando l'impostazione della lingua è **us_english**, l'ordine predefinito di visualizzazione della data è mga. È possibile modificare il formato di data mediante l'istruzione [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md).<br /><br /> L'impostazione dell'istruzione SET DATEFORMAT determina la modalità di interpretazione dei valori di data. Se l'ordine non corrisponde all'impostazione, i valori non vengono interpretati come date. Le date non in ordine potrebbero essere erroneamente interpretate come fuori intervallo o con valori errati. La data 12/10/08 può ad esempio essere interpretata in sei modi diversi a seconda dell'impostazione di DATEFORMAT. Un anno in quattro parti è interpretato come anno.|  
  
|Espressione alfabetica|Descrizione|  
|---|---|
|Apr[ile] [15][,] 1996<br /><br /> Apr[ile] 15[,] [19]96<br /><br /> Apr[ile] 1996 [15]<br /><br /> [15] Apr[ile][,] 1996<br /><br /> 15 Apr[ile][,][19]96<br /><br /> 15 [19]96 apr[ile]<br /><br /> [15] 1996 apr[ile]<br /><br /> 1996 APR[ILE] [15]<br /><br /> 1996 [15] APR[ILE]|È possibile specificare dati relativi alla data con il mese specificato con il nome completo. Ad esempio, aprile o l'abbreviazione del mese di aprile specificata nel linguaggio corrente. Le virgole sono facoltative e l'uso delle maiuscole è ignorato.<br /><br /> Di seguito vengono riportate alcune linee guida per l'utilizzo dei formati di data alfabetici:<br /><br /> 1) Racchiudere data e ora tra virgolette singole ('). Per le lingue diverse dall'inglese, utilizzare N'<br /><br /> 2) I caratteri inclusi tra parentesi sono facoltativi.<br /><br /> 3) Se si specificano solo le ultime due cifre dell'anno, i valori minori delle ultime due cifre del valore impostato nell'opzione di configurazione [Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) vengono interpretati come appartenenti allo stesso secolo dell'anno di cambio data. I valori maggiori o uguali al valore di questa opzione vengono interpretati come appartenenti al secolo che precede l'anno di cambio data. Se ad esempio il valore di **Cambio data per anno a due cifre** è 2050 (impostazione predefinita), 25 viene interpretato come 2025 mentre 50 viene interpretato come 1950. Per evitare ambiguità, esprimere gli anni nel formato a quattro cifre.<br /><br /> 4) Se manca il giorno, viene inserito il primo giorno del mese.<br /><br /> <br /><br /> Quando si specifica il mese in formato alfabetico, l'impostazione di SET DATEFORMAT della sessione non viene applicata.|  
  
|ISO 8601|Descrizione|  
|---|---|
|AAAA-MM-GGThh:mm:ss [.mmm]<br /><br /> AAAAMMGG[ hh:mm:ss[.mmm]]|Esempi:<br /><br /> 1) 2004-05-23T14:25:10<br /><br /> 2) 2004-05-23T14:25:10.487<br /><br /> <br /><br /> Per usare il formato ISO 8601 è necessario specificare ogni elemento in tale formato, includendo il carattere **T**, i due punti (:) e il punto (.) in esso presenti.<br /><br /> Le parentesi indicano che la frazione del componente dei secondi è facoltativa. Il componente dell'ora viene specificato nel formato 24 ore.<br /><br /> Il carattere T indica l'inizio della parte dell'ora del valore **datetime**.<br /><br /> Il vantaggio dell'utilizzo del formato ISO 8601 consiste nel fatto che è uno standard internazionale con specifiche chiare. L'impostazione di SET DATEFORMAT o [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) non influisce su questo formato.|  
  
|Senza separatori|Descrizione|  
|---|---|
|AAAAMMGG hh:mm:ss[.mmm]||  
  
|ODBC|Descrizione|  
|---|---|
|{ ts '1998-05-02 01:23:56.123' }<br /><br /> { d '1990-10-02' }<br /><br /> { t '13:33:41' }|L'API ODBC definisce sequenze di escape per la rappresentazione dei valori di data e ora, che in ODBC sono denominati dati timestamp. Il formato timestamp di ODBC è supportato anche dalla definizione del linguaggio OLE DB (DBGUID-SQL) supportata dal provider [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nelle applicazioni che utilizzano API basate su ADO, OLE DB e ODBC è possibile utilizzare il formato timestamp ODBC per la rappresentazione di valori di data e ora.<br /><br /> Il formato delle sequenze di escape del timestamp ODBC è il seguente: { *literal_type* '*constant_value*' }:<br /><br /> <br /><br /> - *literal_type* specifica il tipo di sequenza di escape. I timestamp sono caratterizzati da tre identificatori di tipo *literal_type*:<br />1) d = solo data<br />2) t = solo ora<br />3) ts = timestamp (ora + data)<br /><br /> <br /><br /> - "*constant_value*" rappresenta il valore della sequenza di escape. *constant_value* deve seguire questi formati per ogni elemento *literal_type:*.<br />d : yyyy-mm-dd<br />t : hh:mm:ss[.fff]<br />ts : yyyy-mm-dd hh:mm:ss[.fff]|  
  
## <a name="rounding-of-datetime-fractional-second-precision"></a>Arrotondamento della precisione in secondi frazionari dei valori datetime  
I valori **datetime** vengono arrotondati con incrementi di 0,000, 0,003 o 0,007 secondi, come illustrato nella tabella seguente.
  
|Valore specificato dall'utente|Valore archiviato dal sistema|  
|---|---|
|01/01/98 23:59:59.999|1998-01-02 00:00:00.000|  
|01/01/98 23:59:59.995<br /><br /> 01/01/98 23:59:59.996<br /><br /> 01/01/98 23:59:59.997<br /><br /> 01/01/98 23:59:59.998|1998-01-01 23:59:59.997|  
|01/01/98 23:59:59.992<br /><br /> 01/01/98 23:59:59.993<br /><br /> 01/01/98 23:59:59.994|1998-01-01 23:59:59.993|  
|01/01/98 23:59:59.990<br /><br /> 01/01/98 23:59:59.991|1998-01-01 23:59:59.990|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformità agli standard ANSI e ISO 8601  
**datetime** non è conforme agli standard ANSI e ISO 8601.
  
##  <a name="converting-date-and-time-data"></a><a name="_datetime"></a>Conversione dei dati relativi alla data e all'ora  
Quando si esegue la conversione in tipi di dati di data e ora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta tutti i valori che non può riconoscere come date o ore. Per informazioni sull'uso delle funzioni CAST e CONVERT con i dati relativi a data e ora, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-other-date-and-time-types-to-the-datetime-data-type"></a>Conversione di altri tipi di data e ora nel tipo di dati datetime 
Nella sezione seguente viene descritto il risultato della conversione di altri tipi di dati di data e ora nel tipo di dati **datetime**.  
  
Quando viene eseguita la conversione da **date**, vengono copiati anno, mese e giorno. Il componente relativo all'ora viene impostato su 00:00:00.000. Nel codice seguente vengono illustrati i risultati della conversione di un valore `date` in un valore `datetime`.  
  
```sql
DECLARE @date date = '12-21-16';  
DECLARE @datetime datetime = @date;  
  
SELECT @datetime AS '@datetime', @date AS '@date';  
  
--Result  
--@datetime               @date  
------------------------- ----------  
--2016-12-21 00:00:00.000 2016-12-21  
```  
  
Quando la conversione è da **time(n)**, il componente relativo all'ora viene copiato e quello relativo alla data viene impostato su "1900-01-01". Quando la precisione frazionaria del valore **time(n)** è maggiore di tre cifre, il valore verrà troncato. Nell'esempio seguente vengono illustrati i risultati della conversione di un valore `time(4)` in un valore `datetime`.  
  
```sql
DECLARE @time time(4) = '12:10:05.1237';  
DECLARE @datetime datetime = @time;  
  
SELECT @datetime AS '@datetime', @time AS '@time';  
  
--Result  
--@datetime               @time  
------------------------- -------------  
--1900-01-01 12:10:05.123 12:10:05.1237  
```  
  
Quando la conversione viene eseguita da **smalldatetime**, le ore e i minuti vengono copiati, mentre i secondi e i secondi frazionari vengono impostati su 0. Nel codice seguente vengono illustrati i risultati della conversione di un valore `smalldatetime` in un valore `datetime`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @datetime AS '@datetime', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetime               @smalldatetime  
------------------------- -----------------------  
--2016-12-01 12:32:00.000 2016-12-01 12:32:00  
```  
  
Quando la conversione viene eseguita da **datetimeoffset(n)**, i componenti di data e ora vengono copiati. Il fuso orario viene troncato. Quando la precisione frazionaria del valore **datetimeoffset(n)** è maggiore di tre cifre, il valore viene troncato. Nell'esempio seguente vengono illustrati i risultati della conversione di un valore `datetimeoffset(4)` in un valore `datetime`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1968-10-23 12:45:37.1234 +10:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetime AS '@datetime', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@datetime               @datetimeoffset  
------------------------- ------------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237 +01:0   
```  
  
Quando la conversione viene eseguita da **datetime2(n)**, la data e l'ora vengono copiate. Quando la precisione frazionaria del valore **datetime2(n)** è maggiore di tre cifre, il valore viene troncato. Nell'esempio seguente vengono illustrati i risultati della conversione di un valore `datetime2(4)` in un valore `datetime`.  
  
```sql
DECLARE @datetime2 datetime2(4) = '1968-10-23 12:45:37.1237';  
DECLARE @datetime datetime = @datetime2;  
  
SELECT @datetime AS '@datetime', @datetime2 AS '@datetime2';  
  
--Result  
--@datetime               @datetime2  
------------------------- ------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237  
```  
  
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
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
