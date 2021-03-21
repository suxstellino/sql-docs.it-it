---
title: Usare il formato carattere per importare o esportare dati
description: Il formato carattere usa il formato dati di tipo carattere per tutte le colonne. Ciò è utile per l'uso di altri programmi o per la copia in un'istanza di un altro fornitore di database.
ms.date: 09/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], character
- character formats [SQL Server]
ms.assetid: d925e66a-1a73-43cd-bc06-1cbdf8174a4d
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: cb3a2d60145ceeadd1a7d13cb8a6cb788e2a0131
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753651"
---
# <a name="use-character-format-to-import-or-export-data-sql-server"></a>Usare il formato carattere per importare o esportare dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È consigliabile adottare il formato carattere per l'esportazione bulk in file di testo utilizzati in altri programmi o per l'importazione bulk da file di testo creati in altri programmi.  

Quando si utilizza il formato carattere, in tutte le colonne viene applicato il formato dati di tipo carattere. L'archiviazione in formato carattere risulta utile quando i dati vengono utilizzati in altri programmi, ad esempio in un foglio di calcolo, o quando è necessario copiare in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i dati di database di altri fornitori, ad esempio Oracle.  
  
> [!NOTE]
>  Quando si esegue il trasferimento bulk dei dati tra le istanze di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il file di dati contiene dati di tipo carattere Unicode ma nessun carattere esteso o DBCS, usare il formato carattere Unicode. Per altre informazioni, vedere [Usare il formato carattere Unicode per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).
  
|Contenuto dell'argomento:|
|---|
|[Considerazioni sull'utilizzo del formato carattere](#considerations)|
|[Opzioni del comando per il formato carattere](#command_options)|
|[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di formato non XML di esempio](#nonxml_format_file)<br />|
|[esempi](#examples)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato carattere per l'esportazione di dati](#bcp_char_export)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato carattere per l'importazione di dati senza un file di formato](#bcp_char_import)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato carattere per l'importazione di dati con un file di formato non XML](#bcp_char_import_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e del formato carattere senza un file di formato](#bulk_char)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e del formato carattere con un file di formato non XML](#bulk_char_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET e del formato carattere con un file di formato non XML](#openrowset_char_fmt)|
|[Attività correlate](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|


  
## <a name="considerations-for-using-character-format"></a>Considerazioni sull'utilizzo del formato carattere<a name="considerations"></a>
Quando si utilizza il formato carattere, è necessario tenere presenti i fattori seguenti:  
  
-   Per impostazione predefinita, quando si esegue l' [utilità bcp](../../tools/bcp-utility.md) i campi dei dati di tipo carattere vengono separati da un carattere di tabulazione e alla fine dei record viene inserito un carattere di nuova riga.  Per informazioni su come specificare caratteri di terminazione alternativi, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
-   Per impostazione predefinita, prima dell'esportazione o dell'importazione bulk di dati in modalità carattere, vengono eseguite le conversioni seguenti:  
  
    |Direzione dell'operazione bulk|Conversione|  
    |---------------------------------|----------------|  
    |Esportazione|I dati vengono convertiti in rappresentazione dei caratteri. Se richiesto in modo esplicito, la conversione viene eseguita in base alla tabella codici specificata per le colonne di tipo carattere. Se non è stata specificata alcuna tabella codici, i dati di tipo carattere vengono convertiti in base alla tabella codici OEM del computer client.|  
    |Importa|I dati di tipo carattere vengono convertiti in rappresentazione nativa, se necessario, e tradotti dalla tabella codici del client alla tabella codici delle colonne di destinazione.|  
  
-   Per impedire eventuali perdite di caratteri estesi durante la conversione, utilizzare il formato di carattere Unicode o specificare una tabella codici.  
  
-   I dati [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) archiviati in un file di formato carattere risultano privi di metadati. Ogni valore viene convertito nel formato [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) in base alle regole di conversione dei dati implicita. I dati importati in colonne [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) risultano di tipo [char](../../t-sql/data-types/char-and-varchar-transact-sql.md). I dati importati in colonne con tipo di dati diverso da [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)vengono convertiti dal tipo di dati [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) tramite una conversione implicita. Per altre informazioni sulla conversione dei dati, vedere [Conversione del tipo di dati &#40;Motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
-   L [utilità bcp](../../tools/bcp-utility.md) esporta i valori [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) in file di dati in formato carattere con quattro cifre dopo il separatore dei decimali e senza simboli di raggruppamento delle cifre, quali i separatori delle migliaia. Ad esempio, una colonna [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) contenente il valore 1,234,567.123456 viene copiata in un file di dati come stringa di caratteri 1234567.1235 tramite l'esportazione bulk.  
  
## <a name="command-options-for-character-format"></a>Opzioni del comando per il formato carattere<a name="command_options"></a>  
È possibile importare dati in formato carattere in una tabella con [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Per un comando [bcp](../../tools/bcp-utility.md) o un'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), è possibile specificare il formato dati nell'istruzione.  Per un'istruzione [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) è necessario specificare il formato dati in un file di formato.  
  
Il formato carattere è supportato dalle opzioni di comando seguenti:  
  
|Comando|Opzione|Descrizione|  
|-------------|------------|-----------------|  
|bcp|**-c**|Determina l'uso dei dati di tipo carattere da parte dell'utilità bcp.\*|  
|BULK INSERT|DATAFILETYPE **='char'**|Durante l'importazione bulk dei dati viene applicato il formato carattere.|  
|OPENROWSET|N/D|Deve usare un file di formato|
  
 \*Per caricare dati di tipo carattere ( **-c**) in un formato compatibile con le versioni precedenti dei client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , usare l'opzione **-V** . Per altre informazioni, vedere [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
   
> [!NOTE]
>  In alternativa, è possibile definire la formattazione di ogni singolo campo in un file di formato. Per altre informazioni, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).

## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi riportati in questo argomento sono basati sulla tabella e sul file di formato definiti di seguito.

### <a name="sample-table"></a>**Tabella di esempio**<a name="sample_table"></a>
Lo script seguente crea un database di test, una tabella denominata `myChar` e popola la tabella con alcuni valori iniziali.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myChar ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myChar
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="sample-non-xml-format-file"></a>**File di formato non XML di esempio**<a name="nonxml_format_file"></a>
SQL Server supporta due tipi di file di formato, ovvero non XML e XML.  Il formato non XML è il formato originale supportato dalle versioni precedenti di SQL Server.  Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myChar.fmt`, sulla base dello schema di `myChar`.  Per usare un comando [bcp](../../tools/bcp-utility.md) per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati.  L'opzione format richiede anche l'opzione **-f** .  Inoltre, in questo esempio il qualificatore **c** viene usato per specificare dati di tipo carattere e **T** viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere il comando seguente:

```cmd
bcp TestDatabase.dbo.myChar format nul -f D:\BCP\myChar.fmt -T -c 

REM Review file
Notepad D:\BCP\myChar.fmt
```

> [!IMPORTANT]
> Verificare che il file di formato non XML termini con un ritorno a capo/avanzamento riga.  In caso contrario, è possibile che venga visualizzato il messaggio di errore seguente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Esempi<a name="examples"></a>
Gli esempi seguenti usano il database e i file di formato creati in precedenza.

### <a name="using-bcp-and-character-format-to-export-data"></a>**Uso di bcp e del formato carattere per l'esportazione di dati**<a name="bcp_char_export"></a>
Opzione **-c** e comando **OUT** .  Nota: il file di dati creato in questo esempio verrà usato in tutti gli esempi successivi.  Al prompt dei comandi immettere il comando seguente:

```cmd
bcp TestDatabase.dbo.myChar OUT D:\BCP\myChar.bcp -T -c

REM Review results
NOTEPAD D:\BCP\myChar.bcp
```

### <a name="using-bcp-and-character-format-to-import-data-without-a-format-file"></a>**Uso di bcp e del formato carattere per l'importazione di dati senza un file di formato**<a name="bcp_char_import"></a>
Opzione **-c** e comando **IN** .  Al prompt dei comandi immettere il comando seguente:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -T -c

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### <a name="using-bcp-and-character-format-to-import-data-with-a-non-xml-format-file"></a>**Uso di bcp e del formato carattere per l'importazione di dati con un file di formato non XML**<a name="bcp_char_import_fmt"></a>
Opzioni **-c** e **-f** switches e **IN** comme.  Al prompt dei comandi immettere il comando seguente:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -f D:\BCP\myChar.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### <a name="using-bulk-insert-and-character-format-without-a-format-file"></a>**Uso di BULK INSERT e del formato carattere senza un file di formato**<a name="bulk_char"></a>
Argomento **DATAFILETYPE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
    FROM 'D:\BCP\myChar.bcp'
    WITH (
        DATAFILETYPE = 'Char'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="using-bulk-insert-and-character-format-with-a-non-xml-format-file"></a>**Uso di BULK INSERT e del formato carattere con un file di formato non XML**<a name="bulk_char_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
   FROM 'D:\BCP\myChar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myChar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="using-openrowset-and-character-format-with-a-non-xml-format-file"></a>**Uso di OPENROWSET e del formato carattere con un file di formato non XML**<a name="openrowset_char_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar;  -- for testing
INSERT INTO TestDatabase.dbo.myChar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myChar.bcp', 
        FORMATFILE = 'D:\BCP\myChar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```
  
## <a name="related-tasks"></a>Attività correlate<a name="RelatedTasks"></a>  
Per utilizzare formati di dati per l'importazione o l'esportazione bulk 
  
-   [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utilizzo del formato carattere Unicode per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
  
