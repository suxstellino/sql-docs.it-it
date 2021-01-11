---
description: date (Transact-SQL)
title: date (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- date_TSQL
- date
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], date
- date and time [SQL Server], date
- dates [SQL Server], data types
- date data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: c963e8b4-5a85-4bd0-9d48-3f8da8f6516b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7970166e9e9f0fa6b3c584d7c7d0ea82d81c5958
ms.sourcegitcommit: f2a28b761022e46da850595553dfbc3e98df83a8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2020
ms.locfileid: "97721095"
---
# <a name="date-transact-sql"></a>date (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definisce una data in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="date-description"></a>Descrizione di date
  
|Proprietà|valore|  
|--------------|-----------|  
|Sintassi|**date**|  
|Uso|DECLARE \@MyDate **date**<br /><br /> CREATE TABLE Table1 ( Column1 **date** )|  
|Formato predefinito dei valori letterali stringa<br /><br /> (utilizzato per client legacy)|YYYY-MM-DD<br /><br /> Per ulteriori informazioni, vedere la sezione seguente relativa alla compatibilità con le versioni precedenti per i client legacy.|  
|Range|da 0001-01-01 a 9999-12-31 (da 1582-10-15 a 9999-12-31 per Informatica)<br /><br /> Da 1 gennaio 1 d.C. a 31 dicembre 9999 d.C. (da 15 ottobre 1582 d.C. a 31 dicembre 9999 d.C. per Informatica)|  
|Intervalli di elementi|AAAA rappresenta un numero di quattro cifre compreso tra 0001 e 9999 indicante l'anno. Per Informatica, AAAA è limitato all'intervallo compreso tra 1582 e 9999.<br /><br /> MM rappresenta un numero di due cifre compreso tra 01 e 12 indicante un mese dell'anno specificato.<br /><br /> GG rappresenta un numero di due cifre compreso tra 01 e 31, a seconda del mese, indicante un giorno del mese specificato.|  
|Lunghezza in caratteri|10 posizioni|  
|Precisione, scala|10, 0|  
|Dimensioni dello spazio di archiviazione|3 byte, fissa|  
|Struttura di archiviazione|Valore intero di 1, 3 byte archivia la data.|  
|Accuratezza|Un giorno|  
|Valore predefinito|1900-01-01<br /><br /> Questo valore viene usato per la parte relativa all'orario aggiunta per la conversione implicita da **time** a **datetime2** o **datetimeoffset**.|  
|Calendario|Gregoriano|  
|Precisione in secondi frazionari definita dall'utente|No|  
|Considerazione e conservazione delle differenze di fuso orario|No|  
|Considerazione dell'ora legale|No|  
  
## <a name="supported-string-literal-formats-for-date"></a>Formati di valore letterale stringa supportati per date
Nelle tabelle seguenti sono illustrati i formati di valore letterale stringa supportati per il tipo di dati **date**.
  
|Numeric|Descrizione|  
|-------------|-----------------|  
|mdy<br /><br /> [m]m/dd/[yy]yy<br /><br /> [m]m-dd-[yy]yy<br /><br /> [m]m.dd.[yy]yy<br /><br /> myd<br /><br /> mm/[yy]yy/dd<br /><br /> mm-[yy]yy/dd<br /><br /> [m]m.[yy]yy.dd<br /><br /> dmy<br /><br /> dd/[m]m/[yy]yy<br /><br /> dd-[m]m-[yy]yy<br /><br /> dd.[m]m.[yy]yy<br /><br /> dym<br /><br /> dd/[yy]yy/[m]m<br /><br /> dd-[yy]yy-[m]m<br /><br /> dd.[yy]yy.[m]m<br /><br /> ymd<br /><br /> [yy]yy/[m]m/dd<br /><br /> [yy]yy-[m]m-dd<br /><br /> [yy]yy-[m]m-dd|[m]m, gg e [aa]aa rappresentano il mese, il giorno e l'anno all'interno di una stringa in cui sono usati come separatori la barra (/), il segno meno (-) o il punto (.).<br /><br /> Sono supportati solo anni a due cifre o a quattro cifre. Utilizzare sempre anni a quattro cifre quando possibile. Per specificare un valore intero compreso tra 0001 e 9999 che rappresenta l'anno di cambio data per l'interpretazione degli anni a due cifre come anni a quattro cifre, vedere [Configurare l'opzione di configurazione del server Cambio data per anno a due cifre](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).<br /><br /> **Nota.** Per Informatica, AAAA è limitato all'intervallo compreso tra 1582 e 9999.<br /><br /> Un anno a due cifre con valore minore o uguale alle ultime due cifre dell'anno di cambio data si trova nello stesso secolo dell'anno di cambio data. Un anno a due cifre con valore maggiore delle ultime due cifre dell'anno di cambio data si trova nel secolo precedente a quello dell'anno di cambio data. Ad esempio, se il valore dell'anno di cambio data è 2049 (impostazione predefinita), l'anno a due cifre 49 è interpretato come 2049 mentre l'anno a due cifre 50 è interpretato come 1950.<br /><br /> Il formato predefinito della data è determinato dall'impostazione della lingua corrente. È possibile modificare il formato di data usando le istruzioni [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) e [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md).<br /><br /> Il formato **ydm** non è supportato per **date**.|  
  
|Espressione alfabetica|Descrizione|  
|------------------|-----------------|  
|mon [dd][,] yyyy<br /><br /> mon dd[,] [yy]<br /><br /> mon yyyy [dd]<br /><br /> [dd] mon[,] yyyy<br /><br /> dd mon[,][yy]yy<br /><br /> dd [yy]yy mon<br /><br /> [dd] yyyy mon<br /><br /> yyyy mon [dd]<br /><br /> yyyy [dd] mon|**mon** (mese) rappresenta il nome completo o l'abbreviazione del mese nella lingua corrente. Le virgole sono facoltative e l'uso delle maiuscole è ignorato.<br /><br /> Per evitare ambiguità, esprimere gli anni nel formato a quattro cifre.<br /><br /> Se manca il giorno, viene inserito il primo giorno del mese.|  
  
|ISO 8601|Descrizione|  
|--------------|----------------|  
|YYYY-MM-DD<br /><br /> YYYYMMDD|Uguale allo standard SQL. Si tratta dell'unico formato definito come standard internazionale.|  
  
|Senza separatori|Descrizione|  
|-----------------|-----------------|  
|[yy]yymmdd<br /><br /> yyyy[mm][dd]|I dati relativi all'elemento **date** possono essere specificati con quattro, sei o otto cifre. Una stringa a sei o otto cifre viene sempre interpretata come **ymd**. Il mese e il giorno devono sempre essere rappresentati da due cifre. Una stringa di quattro cifre viene interpretata come anno.|  
  
|ODBC|Descrizione|  
|----------|-----------------|  
|{ d 'yyyy-mm-dd' }|Specifico delle API ODBC|  
  
|Formato W3C XML|Descrizione|  
|--------------------|-----------------|  
|yyyy-mm-ddTZD|Supportato per l'utilizzo di XML/SOAP.<br /><br /> TZD è l'identificatore del fuso orario (Z, + hh:mm o - hh:mm):<br /><br /> -   hh:mm rappresenta la differenza di fuso orario. hh è un numero di due cifre, compreso tra 0 e 14, che rappresenta il numero di ore della differenza di fuso orario.<br />-   MM è un numero di due cifre, compreso tra 0 e 59, che rappresenta il numero di minuti aggiuntivi della differenza di fuso orario.<br />-   + (più) o - (meno) è il segno obbligatorio della differenza di fuso orario. Questo simbolo indica che, per ottenere l'ora locale, la differenza di fuso orario viene aggiunta o sottratta dall'ora UTC (Coordinated Universal Time). L'intervallo valido della differenza di fuso orario è da -14:00 a +14:00.|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformità agli standard ANSI e ISO 8601  
**date** è conforme alla definizione dello standard ANSI SQL per il calendario gregoriano: "NOTE 85 - Datetime data types will allow dates in the Gregorian format to be stored in the date range 0001-01-01 CE through 9999-12-31 CE."
  
Il formato predefinito dei valori letterali stringa usato per i client legacy è conforme al formato standard SQL, definito come AAAA-MM-GG. Questo formato è lo stesso della definizione ISO 8601 per DATE.
  
> [!NOTE]  
>  Per Informatica, l'intervallo è tra 1582-10-15 (15 ottobre 1582) e 9999-12-31 (31 dicembre 9999).  
  
## <a name="backward-compatibility-for-down-level-clients"></a>Compatibilità con le versioni precedenti dei client
Alcune versioni precedenti dei client non supportano i tipi di dati **time**, **date**, **datetime2** e **datetimeoffset**. Nella tabella seguente viene illustrato il mapping del tipo tra un'istanza di livello principale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i client legacy.
  
|Tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Formato predefiniti dei valori letterali stringa passati al client legacy|ODBC delle versioni precedenti|OLEDB delle versioni precedenti|JDBC delle versioni precedenti|SQLCLIENT delle versioni precedenti|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**datetime2**|AAAA-MM-GG hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Stringa o SqString|  
  
## <a name="converting-date-and-time-data"></a>Conversione dei dati relativi a data e ora
Nella conversione di tipi di dati relativi alla data e all'ora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta tutti i valori non riconosciuti come date o orari. Per informazioni sull'uso delle funzioni CAST e CONVERT con i dati relativi a data e ora, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
### <a name="converting-date-to-other-date-and-time-types"></a>Conversione del tipo di dati date in altri tipi di dati relativi a data e ora

Questa sezione descrive il risultato della conversione di un tipo di dati **date** in altri tipi di dati relativi a data e ora.
  
Quando la conversione viene eseguita in **time(n)** , ha esito negativo e viene generato il messaggio di errore 206: "Conflitto del tipo di operando: date è incompatibile con time".
  
Se viene eseguita la conversione in **datetime**, la data viene copiata. Nel codice seguente vengono illustrati i risultati della conversione di un valore `date` in un valore `datetime`.
  
```sql
DECLARE @date date= '12-10-25';  
DECLARE @datetime datetime= @date;  
  
SELECT @date AS '@date', @datetime AS '@datetime';  
  
--Result  
--@date      @datetime  
------------ -----------------------  
--2025-12-10 2025-12-10 00:00:00.000  
--  
--(1 row(s) affected)  
```  
  
Quando viene eseguita la conversione in **smalldatetime**, se il valore di **date** è compreso nell'intervallo [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), il componente relativo alla data viene copiato mentre quello relativo all'ora viene impostato su 00:00:00.000. Quando il valore **date** non è compreso nell'intervallo di un valore **smalldatetime**, viene generato il messaggio di errore 242: "La conversione di un tipo di dati date in smalldatetime ha generato un valore non compreso nell'intervallo dei valori consentiti" e il valore **smalldatetime** viene impostato su NULL. Nel codice seguente vengono illustrati i risultati della conversione di un valore `date` in un valore `smalldatetime`.
  
```sql
DECLARE @date date= '1912-10-25';  
DECLARE @smalldatetime smalldatetime = @date;  
  
SELECT @date AS '@date', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@date      @smalldatetime  
------------ -----------------------  
--1912-10-25 1912-10-25 00:00:00  
--  
--(1 row(s) affected)  
```  
  
Per la conversione in **datetimeoffset(n)** , la data viene copiata e l'ora viene impostata su 00:00.0000000 +00:00. Nel codice seguente vengono illustrati i risultati della conversione di un valore `date` in un valore `datetimeoffset(3)`.
  
```sql
DECLARE @date date = '1912-10-25';  
DECLARE @datetimeoffset datetimeoffset(3) = @date;  
  
SELECT @date AS '@date', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@date      @datetimeoffset  
------------ ------------------------------  
--1912-10-25 1912-10-25 00:00:00.000 +00:00  
--  
--(1 row(s) affected)  
```  
  
Quando viene eseguita la conversione in **datetime2(n)** , il componente relativo alla data viene copiato e quello relativo all'ora viene impostato su 00:00.000000. Nel codice seguente vengono illustrati i risultati della conversione di un valore `date` in un valore `datetime2(3)`.
  
```sql
DECLARE @date date = '1912-10-25'  
DECLARE @datetime2 datetime2(3) = @date;  
  
SELECT @date AS '@date', @datetime2 AS '@datetime2(3)';  
  
--Result  
--@date      @datetime2(3)  
------------ -----------------------  
--1912-10-25 1912-10-25 00:00:00.000  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-date"></a>Conversione di valori letterali stringa nel tipo di dati date
Le conversioni da valori letterali stringa a tipi di data e ora sono consentite se tutte le parti delle stringhe hanno formati validi. In caso contrario, viene generato un errore di runtime. Le conversioni implicite o esplicite che non specificano uno stile, dai tipi di data e ora ai valori letterali stringa, saranno nel formato predefinito della sessione corrente. Nella tabella seguente vengono illustrate le regole per la conversione di un valore letterale stringa nel tipo di dati **date**.
  
|Valore letterale stringa di input|**date**|  
|---|---|
|ODBC DATE|Viene eseguito il mapping dei valori letterali stringa ODBC al tipo di dati **datetime**. Tutte le operazioni di assegnazione dai valori letterali di ODBC DATETIME in tipi di dati **date** determinano una conversione implicita tra **datetime** e il tipo definito dalle regole di conversione.|  
|ODBC TIME|Vedere la regola relativa a ODBC DATE.|  
|ODBC DATETIME|Vedere la regola relativa a ODBC DATE.|  
|Solo DATE|Semplice|  
|solo TIME|Vengono forniti i valori predefiniti.|  
|solo TIMEZONE|Vengono forniti i valori predefiniti.|  
|DATE + TIME|Viene utilizzata la parte di DATE della stringa di input.|  
|DATE + TIMEZONE|Non consentiti.|  
|TIME + TIMEZONE|Vengono forniti i valori predefiniti.|  
|DATE + TIME + TIMEZONE|Verrà utilizzata la parte DATE di DATETIME locale.|  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente vengono confrontati i risultati dell'esecuzione del cast di una stringa ai tipi di dati relativi a data e ora.
  
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
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29.1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  

Opzione introdotta in SQL Server 2008.

## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
