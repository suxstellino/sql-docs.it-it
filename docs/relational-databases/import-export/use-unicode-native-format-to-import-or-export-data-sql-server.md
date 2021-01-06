---
title: Usare il formato nativo Unicode per importare o esportare dati (SQL Server) | Microsoft Docs
description: Usare il formato Unicode nativo per il trasferimento bulk dei dati tra istanze di SQL Server, che elimina la conversione dei tipi di dati da e verso il formato carattere.
ms.custom: ''
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- Unicode [SQL Server], bulk importing and exporting
- data formats [SQL Server], Unicode native
ms.assetid: a6213308-f3d5-406e-9029-19d8bb3367f3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 977e7c4222a200d697304be0be774214fbc4376a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643462"
---
# <a name="use-unicode-native-format-to-import-or-export-data-sql-server"></a>Utilizzare il formato Unicode nativo per importare o esportare dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Il formato Unicode nativo risulta particolarmente utile quando è necessario copiare informazioni da un'installazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un'altra. L'utilizzo del formato nativo per i dati non carattere consente di risparmiare tempo evitando di dover eseguire la conversione dei tipi di dati in formato carattere e viceversa. L'utilizzo del formato carattere Unicode per tutti i dati di tipo carattere consente di evitare la perdita dei caratteri estesi durante il trasferimento bulk dei dati tra server che utilizzano tabelle codici diverse. Un file di dati in formato nativo Unicode è leggibile con qualsiasi metodo di importazione bulk.  
  
 Il formato nativo Unicode è consigliato per i trasferimenti bulk di dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando un file di dati che include caratteri estesi o DBCS. Per i dati non carattere, il formato nativo Unicode utilizza i tipi di dati (database) nativi. Per i dati di tipo carattere quali [char](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [text](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)e [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)il formato nativo Unicode usa il formato di dati di tipo carattere Unicode.  
  
 I dati [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) memorizzati come SQLVARIANT in un file di dati in formato nativo Unicode vengono gestiti in modo analogo a quanto avviene per un file di dati in formato nativo, fatta eccezione per il fatto che i valori [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) e [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) vengono convertiti in [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) e [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), raddoppiando lo spazio necessario per le colonne interessate. I metadati originali vengono mantenuti e i valori riconvertiti nel tipo di dati [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) e [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) originale quando viene eseguita l'importazione in blocco in una colonna di tabella.  
 
 |Contenuto dell'argomento:|
|---|
|[Opzioni di comando per il formato nativo Unicode](#command_options)|
|[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di formato non XML di esempio](#nonxml_format_file)|
|[esempi](#examples)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato nativo Unicode per l'esportazione di dati](#bcp_widenative_export)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato nativo Unicode per l'importazione di dati senza un file di formato](#bcp_widenative_import)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato nativo Unicode per l'importazione di dati con un file di formato non XML](#bcp_widenative_import_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e del formato nativo Unicode senza un file di formato](#bulk_widenative)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e del formato nativo Unicode con un file di formato non XML](#bulk_widenative_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET e del formato nativo Unicode con un file di formato non XML](#openrowset_widenative_fmt)|
|[Attività correlate](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
  
## <a name="command-options-for-unicode-native-format"></a>Opzioni di comando per il formato nativo Unicode<a name="command_options"></a>  
È possibile importare dati in formato nativo Unicode in una tabella usando [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Per un comando [bcp](../../tools/bcp-utility.md) o un'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), è possibile specificare il formato dati nell'istruzione.  Per un'istruzione [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) è necessario specificare il formato dati in un file di formato.  
  
Il formato nativo Unicode è supportato dalle opzioni di comando seguenti:  
  
|Comando|Opzione|Descrizione|  
|-------------|------------|-----------------|  
|bcp|**-N**|Determina l'uso del formato nativo Unicode da parte dell'utilità **bcp** . Questo formato usa i tipi di dati (database) nativi per tutti i dati di tipo non carattere e il formato di dati carattere Unicode per tutti i dati di tipo carattere (**char**, **nchar**, **varchar**, **nvarchar**, **text** e **ntext**).|  
|BULK INSERT|DATAFILETYPE **='widenative'**|Usa il formato nativo Unicode per l'importazione bulk dei dati.|  
|OPENROWSET|N/D|Deve usare un file di formato|
    
> [!NOTE]
>  In alternativa, è possibile definire la formattazione di ogni singolo campo in un file di formato. Per altre informazioni, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi riportati in questo argomento sono basati sulla tabella e sul file di formato definiti di seguito.

### <a name="sample-table"></a>**Tabella di esempio**<a name="sample_table"></a>
Lo script seguente crea un database di test, una tabella denominata `myWidenative` e popola la tabella con alcuni valori iniziali.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidenative ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidenative
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="sample-non-xml-format-file"></a>**File di formato non XML di esempio**<a name="nonxml_format_file"></a>
SQL Server supporta due tipi di file di formato, ovvero non XML e XML.  Il formato non XML è il formato originale supportato dalle versioni precedenti di SQL Server.  Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myWidenative.fmt`, sulla base dello schema di `myWidenative`.  Per usare un comando [bcp](../../tools/bcp-utility.md) per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati.  L'opzione format richiede anche l'opzione **-f** .  Inoltre, in questo esempio il qualificatore **c** viene usato per specificare dati di tipo carattere e **T** viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere i comandi seguenti:

```
bcp TestDatabase.dbo.myWidenative format nul -f D:\BCP\myWidenative.fmt -T -N

REM Review file
Notepad D:\BCP\myWidenative.fmt
```

> [!IMPORTANT]
> Verificare che il file di formato non XML termini con un ritorno a capo/avanzamento riga.  In caso contrario, è possibile che venga visualizzato il messaggio di errore seguente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Esempi<a name="examples"></a>
Gli esempi seguenti usano il database e i file di formato creati in precedenza.

### <a name="using-bcp-and-unicode-native-format-to-export-data"></a>**Uso di bcp e del formato nativo Unicode per l'esportazione di dati**<a name="bcp_widenative_export"></a>
Opzione **-N** e comando **OUT** .  Nota: il file di dati creato in questo esempio verrà usato in tutti gli esempi successivi.  Al prompt dei comandi immettere i comandi seguenti:
```
bcp TestDatabase.dbo.myWidenative OUT D:\BCP\myWidenative.bcp -T -N

REM Review results
NOTEPAD D:\BCP\myWidenative.bcp
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-without-a-format-file"></a>**Uso di bcp e del formato nativo Unicode per l'importazione di dati senza un file di formato**<a name="bcp_widenative_import"></a>
Opzione **-N** e comando **IN** .  Al prompt dei comandi immettere i comandi seguenti:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -T -N

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-with-a-non-xml-format-file"></a>**Uso di bcp e del formato nativo Unicode per l'importazione di dati con un file di formato non XML**<a name="bcp_widenative_import_fmt"></a>
Opzioni **-N** e **-f** switches e **IN** comme.  Al prompt dei comandi immettere i comandi seguenti:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -f D:\BCP\myWidenative.fmt -T -N

REM Review results is SSMS
```

### <a name="using-bulk-insert-and-unicode-native-format-without-a-format-file"></a>**Uso di BULK INSERT e del formato nativo Unicode senza un file di formato**<a name="bulk_widenative"></a>
Argomento **DATAFILETYPE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
    FROM 'D:\BCP\myWidenative.bcp'
    WITH (
        DATAFILETYPE = 'widenative'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-bulk-insert-and-unicode-native-format-with-a-non-xml-format-file"></a>**Uso di BULK INSERT e del formato nativo Unicode con un file di formato non XML**<a name="bulk_widenative_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
   FROM 'D:\BCP\myWidenative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidenative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-openrowset-and-unicode-native-format-with-a-non-xml-format-file"></a>**Uso di OPENROWSET e del formato nativo Unicode con un file di formato non XML**<a name="openrowset_widenative_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative;  -- for testing
INSERT INTO TestDatabase.dbo.myWidenative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidenative.bcp', 
        FORMATFILE = 'D:\BCP\myWidenative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

## <a name="related-tasks"></a>Attività correlate<a name="RelatedTasks"></a>
Per utilizzare formati di dati per l'importazione o l'esportazione bulk  
-   [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utilizzo del formato carattere Unicode per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
