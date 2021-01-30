---
description: CREATE EXTERNAL FILE FORMAT (Transact-SQL)
title: CREATE EXTERNAL FILE FORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL FILE FORMAT
- CREATE_EXTERNAL_FILE_FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, file format
- PolyBase, external file format
ms.assetid: abd5ec8c-1a0e-4d38-a374-8ce3401bc60c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a6ac8da6485d34f330303db20a4eb85d5a86b9f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186858"
---
# <a name="create-external-file-format-transact-sql"></a>CREATE EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

Crea un oggetto formato di file esterno che definisce i dati esterni archiviati in Hadoop, in Archiviazione BLOB di Azure o in Azure Data Lake Store o per i flussi di input e output associati a flussi esterni. La creazione di un formato di file esterno è un prerequisito per la creazione di una tabella esterna. Creando un formato di file esterno, si specifica il layout effettivo dei dati a cui fa riferimento una tabella esterna.  
  
Sono supportati i seguenti formati di file:
  
- Testo delimitato  
  
- Hive RCFile  - Non si applica ad Azure Synapse Analytics.
  
- Hive ORC
  
- Parquet

- JSON - Si applica solo a SQL Edge di Azure.


Per creare una tabella esterna, vedere [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md).
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi
  
### <a name="delimited-text"></a>[Testo delimitato](#tab/delimited)
```syntaxsql
-- Create an external file format for DELIMITED (CSV/TSV) files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
        FORMAT_TYPE = DELIMITEDTEXT  
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    [ , DATA_COMPRESSION = {  
           'org.apache.hadoop.io.compress.GzipCodec'  
         | 'org.apache.hadoop.io.compress.DefaultCodec'  
        }  
     ]);

<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter 
    | First_Row = integer -- ONLY AVAILABLE FOR AZURE SYNAPSE ANALYTICS
    | DATE_FORMAT = datetime_format  
    | USE_TYPE_DEFAULT = { TRUE | FALSE } 
    | Encoding = {'UTF8' | 'UTF16'} 
}
```
### <a name="rc"></a>[RC](#tab/rc)
```syntaxsql
--Create an external file format for RC files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = {  
        'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'  
      | 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'  
    }  
    [ , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec' ]);
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="orc"></a>[ORC](#tab/orc)
```syntaxsql  
--Create an external file format for ORC file.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (
         FORMAT_TYPE = ORC  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.DefaultCodec'      }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="parquet"></a>[Parquet](#tab/parquet)
```syntaxsql
--Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
         FORMAT_TYPE = PARQUET  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);    
```
### <a name="json"></a>[JSON](#tab/json)
```syntaxsql
-- Create an external file format for JSON files.
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = JSON  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      
      | 'org.apache.hadoop.io.compress.DefaultCodec'  }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

---
  
## <a name="arguments"></a>Argomenti  
*file_format_name*  
Specifica un nome per il formato di file esterno.
 
### <a name="format_type"></a>FORMAT_TYPE 
`FORMAT_TYPE = [ PARQUET | ORC | RCFILE | DELIMITEDTEXT]`

Specifica il formato dei dati esterni.
  
- PARQUET specifica un formato Parquet.
  
- ORC  
  Specifica un formato ORC (Optimized Row Columnar). Questa opzione richiede Hive 0.11 o versione successiva nel cluster Hadoop esterno. In Hadoop il formato file ORC offre una migliore qualità di compressione e prestazioni rispetto al formato di file RCFILE.

- RCFILE (in combinazione con SERDE_METHOD = *SERDE_method*) specifica un formato di file RCFILE (Record Columnar). Questa opzione richiede di specificare un metodo SerDe (Serializer and Deserializer) di Hive. Il requisito è lo stesso se si usa Hive/HiveQL in Hadoop per eseguire query sui file RC. Si noti che il metodo SerDe fa distinzione tra maiuscole e minuscole.

  Esempi di definizione di file RC con i due metodi SerDe supportati da PolyBase.

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'

- DELIMITEDTEXT specifica un formato di testo con delimitatori di colonna, detti anche caratteri di terminazione del campo.
   
- JSON specifica un formato JSON. Si applica solo a SQL Edge di Azure. 

### <a name="data_compression"></a>DATA\_COMPRESSION
 `DATA_COMPRESSION = *data_compression_method*`  
 Specifica il metodo di compressione dei dati per il file esterno. Quando DATA_COMPRESSION non è specificato, per impostazione predefinita vengono usati i dati non compressi.
Per funzionare correttamente, i file compressi Gzip devono avere estensione ".gz".
 
 #### <a name="delimited-text"></a>[Testo delimitato](#tab/delimited)
 Il tipo di formato DELIMITEDTEXT supporta questi metodi di compressione:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'

#### <a name="rc"></a>[RC](#tab/rc)
 Il tipo di formato RCFILE supporta questo metodo di compressione:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
#### <a name="orc"></a>[ORC](#tab/orc)
 Il tipo di formato ORC supporta questi metodi di compressione:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="parquet"></a>[Parquet](#tab/parquet)
 Il tipo di formato PARQUET supporta i seguenti metodi di compressione:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="json"></a>[JSON](#tab/json)
 Il tipo di formato JSON supporta i seguenti metodi di compressione:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
---  
### <a name="delimited-text-format-options"></a>Opzioni per il formato di testo delimitato

Le opzioni di formato descritte in questa sezione sono facoltative e si applicano solo ai file di testo delimitato.

#### <a name="field_terminator"></a>FIELD_TERMINATOR
`FIELD_TERMINATOR = *field_terminator*`  
Si applica solo ai file di testo delimitato. Il carattere di terminazione del campo specifica uno o più caratteri che contrassegnano la fine di ogni campo (colonna) nel file di testo delimitato. Il valore predefinito è il carattere pipe "|". Per garantire il supporto, si consiglia di usare uno o più caratteri ASCII.
  
  
Esempi:  
  
-   FIELD_TERMINATOR = '|'  
  
-   FIELD_TERMINATOR = ' '  
  
-   FIELD_TERMINATOR = ꞌ\tꞌ  
  
-   FIELD_TERMINATOR = '~|~'  
  
#### <a name="string_delimiter"></a>STRING_DELIMITER
`STRING_DELIMITER = *string_delimiter*`

Specifica il carattere di terminazione del campo per i dati di tipo stringa nel file di testo delimitato. Il delimitatore di stringa è costituito da uno o più caratteri ed è racchiuso tra virgolette singole. Il valore predefinito è la stringa vuota "". Per garantire il supporto, si consiglia di usare uno o più caratteri ASCII.
 
  
 Esempi:  

-   STRING_DELIMITER = '"'

-   STRING_DELIMITER = '0x22' -- Esadecimale con virgolette doppie
  
-   STRING_DELIMITER = '*'  
  
-   STRING_DELIMITER = ꞌ,ꞌ  
  
-   STRING_DELIMITER = '0x7E0x7E'  -- Due caratteri tilde (ad esempio, ~~)
 
#### <a name="first_row"></a>FIRST_ROW
 `FIRST_ROW = *First_row_int*`  
Specifica il numero di righe che verranno lette prima in tutti i file durante un caricamento di PolyBase. Questo parametro può accettare valori da 1 a 15. Se il valore è impostato su due, la prima riga di ogni file (riga di intestazione) viene ignorata quando i dati vengono caricati. Le righe vengono ignorate in base all'esistenza di caratteri di terminazione della riga (r/n, /r, /n). Quando si usa questa opzione per l'esportazione, le righe vengono aggiunti ai dati per garantire che il file possa essere letto senza perdere dati. Se il valore è impostato su >2, la prima riga esportata è quella dei nomi delle colonne della tabella esterna.

#### <a name="date_format"></a>DATE\_FORMAT
 `DATE_FORMAT = *datetime_format*`  
Specifica un formato personalizzato per tutti i dati di data e ora che possono apparire in un file di testo delimitato. Se il file di origine usa formati di data e ora predefiniti, questa opzione non è necessaria. Per ogni file è consentito un solo formato di data e ora personalizzato. Non è possibile specificare più di un formato di data e ora personalizzato per ogni file. Tuttavia, è possibile usare più formati di data e ora se ognuno di essi è il formato predefinito per il rispettivo tipo di dati nella definizione della tabella esterna.

> [!IMPORTANT]
> PolyBase usa solo il formato di data personalizzato per l'importazione dei dati. Non usa il formato personalizzato per scrivere i dati in un file esterno.

 Se DATE_FORMAT non è specificata o è la stringa vuota, PolyBase usa i formati predefiniti seguenti:
  
-   DateTime: 'yyyy-MM-dd HH:mm:ss'  
  
-   SmallDateTime: 'yyyy-MM-dd HH:mm'  
  
-   Date: 'yyyy-MM-dd'  
  
-   DateTime2: 'yyyy-MM-dd HH:mm:ss'  
  
-   DateTimeOffset: 'yyyy-MM-dd HH:mm:ss'  
  
-   Ora: 'HH:mm:ss'  

> [!IMPORTANT]
> Se si specifica un valore personalizzato per `DATE_FORMAT`, verrà eseguito l'override di tutti i tipi di file predefiniti. Sarà quindi necessario applicare gli stessi formati di dati in tutte le celle di tipo datetime, date e time nei file. Con il valore `DATE_FORMAT` sottoposto a override non è possibile usare formati diversi per i valori di data e ora.

Nella tabella seguente sono riportati **esempi di formati di data**:
  
Note sulla tabella:  
  
-   Anno, mese e giorno possono avere diversi formati e ordini. Nella tabella è riportato solo il formato **ymd**. Il mese può avere una o due cifre o tre caratteri. Il giorno può avere una o due cifre. L'anno può avere due o quattro cifre.
  
-   I millisecondi (fffffff) non sono obbligatori.
  
-   Am e pm (tt) non sono obbligatori. L'impostazione predefinita è AM.
  
|Tipo di data|Esempio|Descrizione|  
|---------------|-------------|-----------------|  
|Datetime|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 24 ore, da 00 a 59 minuti, da 00 a 59 secondi e 3 cifre per i millisecondi.|  
|Datetime|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffftt'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 12 ore, da 00 a 59 minuti, da 00 a 59 secondi, 3 cifre per i millisecondi e AM, am, PM o pm. |  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd HH:mm'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 23 ore e da 00 a 59 minuti.|  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd hh:mmtt'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 11 ore, da 00 a 59 minuti, non i secondi e AM, am, PM o pm.|  
|Data|DATE_FORMAT =  'yyyy-MM-dd'|Anno, mese e giorno. Non sono inclusi elementi di ora.|  
|Data|DATE_FORMAT = 'yyyy-MMM-dd'|Anno, mese e giorno. Se il mese è specificato con 3 M, il valore di input è una delle stringhe Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov o Dec.|  
|DateTime2|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 23 ore, da 00 a 59 minuti, da 00 a 59 secondi e 7 cifre per i millisecondi.|  
|DateTime2|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 11 ore, da 00 a 59 minuti, da 00 a 59 secondi, 7 cifre per i millisecondi e AM, am, PM o pm.|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff zzz'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 23 ore, da 00 a 59 minuti, da 00 a 59 secondi, 7 cifre per i millisecondi e la differenza di fuso orario specificata nel file di input come `{+&#124;-}HH:ss`. Ad esempio, poiché l'ora di Los Angeles senza ora legale è 8 ore indietro rispetto all'ora UTC, il valore -08:00 nel file di input specifica il fuso orario per Los Angeles.|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt zzz'|Oltre ad anno, mese e giorno questo formato di data include da 00 a 11 ore, da 00 a 59 minuti, da 00 a 59 secondi, 7 cifre per i millisecondi, AM, am, PM o pm e la differenza di fuso orario. Vedere la descrizione della riga precedente.|  
|Tempo|DATE_FORMAT = 'HH:mm:ss'|Non vi è alcun valore di data, solo da 00 a 23 ore, da 00 a 59 minuti e da 00 a 59 secondi.|  
  
 ##### <a name="supported-date-and-time-formats"></a>Formati di data e ora supportati
 
 Il formato di file esterno può descrivere una quantità elevata di formati di data e ora:
  
|Datetime|smalldatetime|Data|datetime2|datetimeoffset|  
|--------------|-------------------|----------|---------------|--------------------|  
|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fff]|[M[M]]M-[d]d-[yy]yy HH:mm[:00]|[M[M]]M-[d]d-[yy]yy|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff]|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm[:00][tt]||[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fff]|[M[M]]M-[yy]yy-[d]d HH:mm[:00]|[M[M]]M-[yy]yy-[d]d|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff]|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm[:00][tt]||[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fff]|[yy]yy-[M[M]]M-[d]d HH:mm[:00]|[yy]yy-[M[M]]M-[d]d|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm[:00][tt]||[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fff]|[yy]yy-[d]d-[M[M]]M HH:mm[:00]|[yy]yy-[d]d-[M[M]]M|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm[:00][tt]||[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fff]|[d]d-[M[M]]M-[yy]yy HH:mm[:00]|[d]d-[M[M]]M-[yy]yy|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff]|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm[:00][tt]||[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fff]|[d]d-[yy]yy-[M[M]]M HH:mm[:00]|[d]d-[yy]yy-[M[M]]M|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm[:00][tt]||[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
  
 Dettagli:  
  
-   Per separare i valori di mese, giorno e anno, è possibile usare "-", "/" o ".". Per semplicità, nella tabella viene usato solo il separatore "-".
  
-   Per specificare il mese come testo, usare tre o più caratteri. I mesi con uno o due caratteri vengono interpretati come un numero.
  
-   Per separare i valori di ora, usare il simbolo ":".
  
-   Le lettere tra parentesi quadre sono facoltative.
  
-   Le lettere "tt" designano [AM|PM|am|pm]. AM è l'impostazione predefinita. Se si specifica "tt", il valore dell'ora (hh) deve essere compreso nell'intervallo da 0 a 12.
  
-   Le lettere "zzz" designano la differenza di fuso orario per il fuso orario corrente del sistema nel formato {+|-}HH:ss].
 
#### <a name="use_type_default"></a>USE_TYPE_DEFAULT 
 `USE_TYPE_DEFAULT = { TRUE | FALSE }`
 
 Specifica come gestire valori mancanti nei file di testo delimitato quando PolyBase recupera i dati dal file di testo.
  
 TRUE  
 Quando si recuperano dati dal file di testo, archiviare ogni valore mancante usando il valore predefinito per il tipo di dati della colonna corrispondente nella definizione della tabella esterna. Ad esempio, sostituire un valore mancante con:  
  
-   0 se la colonna viene definita come colonna numerica. Le colonne decimali non sono supportate e generano un errore.
  
-   Una stringa vuota "" se la colonna è una colonna stringa.
  
-   1900-01-01 se la colonna è una colonna di data.
  
 **FALSE**  
 Archiviare tutti i valori mancanti come NULL. Tutti i valori NULL archiviati usando la parola NULL nel file di testo delimitato vengono importati come stringa "NULL".
 
#### <a name="encoding"></a>ENCODING
   `Encoding = {'UTF8' | 'UTF16'}`
   
 In [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] (APS CU7.4) PolyBase può leggere file di testo delimitati con codifica UTF8 e UTF16-LE. In SQL Server, PolyBase non supporta la lettura di file con codifica UTF16.

## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione ALTER ANY EXTERNAL FILE FORMAT.
  
## <a name="general-remarks"></a>Osservazioni generali
 Il formato di file esterno è con ambito database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. È con ambito server in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 Le opzioni di formato sono tutte facoltative e si applicano solo ai file di testo delimitato.  
  
 Quando i dati vengono archiviati in uno dei formati compressi, PolyBase decomprime i dati prima di restituire i record di dati.
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni
  
 Il delimitatore di riga nei file di testo delimitato deve essere supportato da LineRecordReader di Hadoop. Ovvero, deve essere "\r", "\n" o "\r\n". Questi delimitatori non sono configurabili dall'utente.
  
 Le combinazioni dei metodi SerDe supportati con i file RC e i metodi di compressione dei dati supportati sono elencati in precedenza in questo articolo. Non tutte le combinazioni sono supportate.
  
 Il numero massimo di query PolyBase simultanee è 32. Quando si eseguono contemporaneamente 32 query, ogni query è in grado di leggere al massimo 33.000 file dal percorso del file esterno. Anche la cartella radice e ogni sottocartella vengono considerate file. Se il livello di concorrenza è inferiore a 32, il percorso del file esterno può contenere più di 33.000 file.
  
 A causa della limitazione del numero di file nella tabella esterna, si consiglia di archiviare meno di 30.000 file nella radice e nelle sottocartelle del percorso del file esterno. Si consiglia inoltre di usare un numero ridotto di sottocartelle nella cartella radice. Quando si fa riferimento a troppi file, può verificarsi un'eccezione di memoria insufficiente in Java Virtual Machine.
  
  Quando si esportano dati in Hadoop o nell'archiviazione BLOB di Azure usando PolyBase, vengono esportati solo i dati e non i nomi di colonne (metadati) definiti nel comando CREATE EXTERNAL TABLE.

## <a name="locking"></a>Blocco  
 Acquisisce un blocco condiviso per l'oggetto EXTERNAL FILE FORMAT.
  
## <a name="performance"></a>Prestazioni
 L'uso dei file compressi implica sempre un compromesso tra il trasferimento di una minore quantità di dati tra l'origine dati esterna e SQL Server e l'incremento dell'utilizzo della CPU per comprimere e decomprimere i dati.
  
 I file di testo compressi Gzip non sono possono essere suddivisi. Per migliorare le prestazioni dei file di testo compressi Gzip, si consiglia di generare più file da archiviare nella stessa directory all'interno dell'origine dati esterna. Questa struttura di file consente a PolyBase di leggere e decomprimere i dati più rapidamente usando più processi di lettura e decompressione. Il numero ideale di file compressi è il numero massimo di processi del lettore dati per ogni nodo di calcolo. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] il numero massimo di processi di lettura dati è di 8 per ogni nodo, ad eccezione di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Gen2, in cui è di 20 processi per ogni nodo. In [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] il numero massimo di processi del lettore dati per ogni nodo varia in base al punto di disconnessione singolo. Per informazioni dettagliate, vedere [Strategie e modelli di caricamento di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-create-a-delimitedtext-external-file-format"></a>R. Creare un formato di file esterno DELIMITEDTEXT  
 In questo esempio viene creato un formato di file esterno denominato *textdelimited1* per un file delimitato da testo. Le opzioni indicate per FORMAT\_OPTIONS specificano che i campi nel file devono essere separati usando un carattere pipe "|". Il file di testo viene anche compresso con il codec Gzip. Se non si specifica DATA\_COMPRESSION, il file di testo non è compresso.
  
 Per un file di testo delimitato, il metodo di compressione dei dati può essere il codec predefinito, "org.apache.hadoop.io.compress.DefaultCodec" o il codec Gzip, "org.apache.hadoop.io.compress.GzipCodec".
  
```sql  
CREATE EXTERNAL FILE FORMAT textdelimited1  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,  
    FORMAT_OPTIONS (  
        FIELD_TERMINATOR = '|',  
        DATE_FORMAT = 'MM/dd/yyyy' ),  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
);  
```  
  
### <a name="b-create-an-rcfile-external-file-format"></a>B. Creare un formato di file esterno RCFILE  
 In questo esempio viene creato un formato di file esterno per un file RC che usa il metodo di serializzazione/deserializzazione org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe. Specifica anche di usare il codec predefinito per il metodo di compressione dei dati. Se non si specifica DATA_COMPRESSION, per impostazione predefinita la compressione non viene usata.
  
```sql  
CREATE EXTERNAL FILE FORMAT rcfile1  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe',  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'  
);  
```  
  
### <a name="c-create-an-orc-external-file-format"></a>C. Creare un formato di file esterno ORC  
 In questo esempio viene creato un formato di file esterno per un file ORC che comprime i dati con il metodo di compressione org.apache.io.compress.SnappyCodec. Se non si specifica DATA_COMPRESSION, per impostazione predefinita la compressione non viene usata.
  
```sql 
CREATE EXTERNAL FILE FORMAT orcfile1  
WITH (  
    FORMAT_TYPE = ORC,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
  
### <a name="d-create-a-parquet-external-file-format"></a>D. Creare un formato di file esterno PARQUET  
 In questo esempio viene creato un formato di file esterno per un file Parquet che comprime i dati con il metodo di compressione org.apache.io.compress.SnappyCodec. Se non si specifica DATA_COMPRESSION, per impostazione predefinita la compressione non viene usata.  
  
```sql  
CREATE EXTERNAL FILE FORMAT parquetfile1  
WITH (  
    FORMAT_TYPE = PARQUET,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
### <a name="e-create-a-delimited-text-file-skipping-header-row-azure-synapse-analytics-only"></a>E. Creare un file di testo delimitato ignorando la riga di intestazione (solo Azure Synapse Analytics)
 In questo esempio viene creato un formato di file esterno per un file CSV con un'unica riga di intestazione. 
  
```sql  
CREATE EXTERNAL FILE FORMAT skipHeader_CSV
WITH (FORMAT_TYPE = DELIMITEDTEXT,
      FORMAT_OPTIONS(
          FIELD_TERMINATOR = ',',
          STRING_DELIMITER = '"',
          FIRST_ROW = 2, 
          USE_TYPE_DEFAULT = True)
)
```   
### <a name="f-create-a-json-external-file-format"></a>F. Creare un formato di file esterno JSON  
 In questo esempio viene creato un formato di file esterno per un file JSON che comprime i dati con il metodo di compressione org.apache.io.compress.SnappyCodec. Se non si specifica DATA_COMPRESSION, per impostazione predefinita la compressione non viene usata. Questo esempio si applica a SQL Edge di Azure e non è attualmente valido per altri prodotti SQL. 
  
```sql  
CREATE EXTERNAL FILE FORMAT jsonFileFormat  
WITH (  
    FORMAT_TYPE = JSON,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  

## <a name="see-also"></a>Vedere anche
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)  
