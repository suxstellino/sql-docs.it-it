---
title: Supporto del tipo, data e ora ODBC
description: Informazioni sui tipi ODBC che supportano SQL Server tipi di dati di data e ora, tra cui il mapping dei tipi di dati e i formati per stringhe, valori letterali e strutture di dati.
ms.custom: ''
ms.date: 12/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC], data type support
- ODBC, date/time improvements
ms.assetid: 8e0d9ba2-3ec1-4680-86e3-b2590ba8e2e9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8acb8060cb543d264f7237bb720d54ce5b9daedd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756121"
---
# <a name="data-type-support-for-odbc-date-and-time-improvements"></a>Supporto dei tipi di dati per i miglioramenti relativi a data e ora ODBC
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono fornite informazioni sui tipi ODBC che supportano i tipi di dati di data e ora [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="data-type-mapping-in-parameters-and-resultsets"></a>Mapping dei tipi di dati nei parametri e nei set di risultati  
 Oltre ai tipi di dati ODBC (SQL_TYPE_TIMESTAMP e SQL_TIMESTAMP), nel provider ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sono stati aggiunti due nuovi tipi di dati per esporre i nuovi tipi di server:  
  
-   SQL_SS_TIME2  
  
-   SQL_SS_TIMESTAMPOFFSET  
  
 Nella tabella seguente viene illustrato il mapping completo per il tipo di server. Si noti che alcune celle della tabella contengono due voci; in questi casi, la prima è il valore per ODBC 3.0 mentre la seconda è il valore per ODBC 2.0.  
  
|Tipo di dati di SQL Server|Tipo di dati SQL|valore|  
|--------------------------|-------------------|-----------|  
|Datetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
|Smalldatetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
|Data|SQL_TYPE_DATE<br /><br /> SQL_DATE|91 (SQL. h)<br /><br /> 9 (sqlext. h)|  
|Tempo|SQL_SS_TIME2|-154 (SQLNCLI. h)|  
|DatetimeOFFSET|SQL_SS_TIMESTAMPOFFSET|-155 (SQLNCLI.h)|  
|Datetime2|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
||||

 Nella tabella seguente vengono elencati i tipi ODBC C e le strutture corrispondenti. Poiché ODBC non consente i tipi C definiti dal driver, viene utilizzato SQL_C_BINARY per time e datetimeoffset come strutture binarie.  
  
|Tipo di dati SQL|Layout in memoria|Tipo di dati C predefinito|Valore (sqlext.h)|  
|-------------------|-------------------|-------------------------|------------------------|  
|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|SQL_TIMESTAMP_STRUCT<br /><br /> TIMESTAMP_STRUCT|SQL_C_TYPE_TIMESTAMP<br /><br /> SQL_C_TIMESTAMP|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|  
|SQL_TYPE_DATE<br /><br /> SQL_DATE|SQL_DATE_STRUCT<br /><br /> DATE_STRUCT|SQL_C_TYPE_DATE<br /><br /> SQL_C_DATE|SQL_TYPE_DATE<br /><br /> SQL_DATE|  
|SQL_SS_TIME2|SQL_SS_TIME2_STRUCT|SQL_C_SS_TIME2<br /><br /> SQL_C_BINARY (ODBC 3.5 e versioni precedenti)|0x4000 (sqlncli.h)<br /><br /> SQL_BINARY (-2)|  
|SQL_SS_TIMESTAMPOFFSET|SQL_SS_TIMESTAMPOFFSET_STRUCT|SQL_C_SS_TIMESTAMPOFFSET<br /><br /> SQL_C_BINARY (ODBC 3.5 e versioni precedenti)|0x4001 (sqlncli.h)<br /><br /> SQL_BINARY (-2)|  
|||||

 Quando viene specificata l'associazione SQL_C_BINARY, viene eseguito il controllo dell'allineamento e viene segnalato un errore in caso di allineamento non corretto. L'identificativo SQLSTATE per questo errore è IM016, con un messaggio indicante che l'allineamento della struttura non è corretto.  
  
## <a name="data-formats-strings-and-literals"></a>Formati di dati: stringhe e valori letterali  
 Nella tabella seguente vengono illustrati i mapping tra tipi di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tipi di dati ODBC e valori letterali stringa ODBC.  
  
|Tipo di dati di SQL Server|Tipo di dati ODBC|Formato stringa per le conversioni client|  
|--------------------------|--------------------|------------------------------------------|  
|Datetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|'yyyy-mm-dd hh:mm:ss[.999]'<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta fino a tre cifre per i secondi frazionari per datetime.|  
|Smalldatetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|'yyyy-mm-dd hh:hh:ss'<br /><br /> Questo tipo di dati ha un'accuratezza di un minuto. Il componente dei secondi sarà zero nell'output mentre verrà arrotondato dal server nell'input.|  
|Data|SQL_TYPE_DATE<br /><br /> SQL_DATE|'yyyy-mm-dd'|  
|Tempo|SQL_SS_TIME2|'hh:mm:ss[.9999999]'<br /><br /> I secondi frazionari possono essere specificati facoltativamente utilizzando fino a sette cifre.|  
|Datetime2|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|' aaaa-mm-gg hh: mm: SS [. 9999999]'<br /><br /> I secondi frazionari possono essere specificati facoltativamente utilizzando fino a sette cifre.|  
|DatetimeOFFSET|SQL_SS_TIMESTAMPOFFSET|'yyyy-mm-dd hh:mm:ss[.9999999] +/- hh:mm'<br /><br /> I secondi frazionari possono essere specificati facoltativamente utilizzando fino a sette cifre.|  
||||

 Non vi sono modifiche nelle sequenze di escape ODBC per i valori letterali di data/ora.  
  
 I secondi frazionari nei risultati utilizzano sempre il punto (.), anziché i due punti (:).  
  
 I valori stringa restituiti alle applicazioni sono sempre della stessa lunghezza per una specifica colonna. Nei componenti anno, mese, giorno, ora, minuto e secondo vengono aggiunti zero iniziali fino alla lunghezza massima e nei valori datetime è presente uno spazio tra la data e l'ora. È inoltre presente uno spazio tra l'ora e la differenza di fuso orario in un valore datetimeoffset. La differenza di fuso orario viene sempre preceduta da un segno. Quando la differenza è zero, il segno è un più (+). Se necessario, nei secondi frazionari vengono aggiunti zero finali fino alla precisione definita per la colonna. Per le colonne di tipo datetime, vi sono tre cifre per i secondi frazionari. Per le colonne di tipo smalldatetime, non vi sono cifre per i secondi frazionari e i secondi saranno sempre zero.  
  
 Una stringa vuota non è un valore letterale di data/ora valido e non rappresenta un valore NULL. Un tentativo di convertire una stringa vuota in un valore di data/ora genererà un errore con l'identificativo SQLState 22018 e il messaggio "Carattere non valido per la specifica del cast".  
  
 Nelle conversioni dai parametri di stringa si otterranno stringhe nello stesso formato, ad eccezione del fatto che il segno di un fuso orario con zero ore e zero minuti può essere sia più che meno e che sono consentiti zero finali per i secondi frazionari fino a un massimo di 9 cifre. Un componente per le ore può terminare con un separatore decimale e senza cifre per i secondi frazionari.  
  
 Il driver consente attualmente uno spazio vuoto aggiuntivo prima e dopo i caratteri di punteggiatura mentre lo spazio tra l'ora e la differenza di fuso orario è facoltativo. Nelle versioni future tuttavia queste regole potrebbero essere modificate, per cui le applicazioni non devono basarsi sul comportamento corrente.  
  
## <a name="data-formats-data-structures"></a>Formati di dati: strutture di dati  
 Nelle strutture descritte di seguito, ODBC specifica i vincoli seguenti, secondo il calendario gregoriano:  
  
-   L'intervallo per i mesi è compreso tra 1 e 12.  
  
-   L'intervallo per il campo del giorno è compreso tra 1 e il numero di giorni nel mese e deve essere coerente con i campi dell'anno e del mese, considerando gli anni bisestili.  
  
-   L'intervallo delle ore è compreso tra 0 e 23.  
  
-   L'intervallo dei minuti è compreso tra 0 e 59.  
  
-   L'intervallo per i secondi è compreso tra 0 e 61,9(n), consentendo fino a due secondi di compensazione per mantenere la sincronizzazione con l'ora siderale.  
  
     Notare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non consente i secondi di compensazione, pertanto i valori dei secondi maggiori di 59 generano un errore del server.  
  
 Sono state modificate le implementazioni per le seguenti strutture ODBC esistenti in modo da supportare i nuovi tipi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di data e ora. Le definizioni, tuttavia, non sono state modificate.  
  
-   DATE_STRUCT  
  
-   TIME_STRUCT  
  
-   TIMESTAMP_STRUCT  
  
 Sono inoltre disponibile due nuove strutture:  
  
-   SQL_SS_TIME2_STRUCT  
  
-   SQL_SS_TIMESTAMPOFFSET_STRUCT  
  
### <a name="sql_ss_time2_struct"></a>SQL_SS_TIME2_STRUCT  
 In questa struttura è possibile aggiungere fino a 12 byte nei sistemi operativi a 32 bit e a 64 bit.  
  
```cpp
typedef struct tagSS_TIME2_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;  
} SQL_SS_TIME2_STRUCT;  
```  
  
### <a name="sql_ss_timestampoffset_struct"></a>SQL_SS_TIMESTAMPOFFSET_STRUCT  
  
```cpp
typedef struct tagSS_TIMESTAMPOFFSET_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;  
   SQLSMALLINT timezone_hour;  
   SQLSMALLINT timezone_minute;  
} SQL_SS_TIMESTAMPOFFSET_STRUCT;  
```  
  
 Se la **timezone_hour** è negativa, il **timezone_minute** deve essere negativo o zero. Se il **timezone_hour** è positivo, il **timezone_minute** deve essere positivo o zero. Se il **timezone_hour** è zero, l' **timezone_minute** può avere un valore compreso nell'intervallo da-59 a + 59.  
  
## <a name="see-also"></a>Vedere anche  
 [Miglioramenti di data e ora &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
