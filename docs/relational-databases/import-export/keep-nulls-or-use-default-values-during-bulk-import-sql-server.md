---
title: Mantenere i valori Null o i valori predefiniti durante un'importazione bulk
description: Per l'importazione bulk in SQL Server, sia bcp che BULK INSERT caricano i valori predefiniti per sostituire i valori Null. Per entrambi, è possibile scegliere di mantenere i valori Null.
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], null values
- bulk importing [SQL Server], default values
- data formats [SQL Server], null values
- bulk rowset providers [SQL Server]
- bcp utility [SQL Server], null values
- BULK INSERT statement
- default values
- OPENROWSET function, bulk importing
- data formats [SQL Server], default values
ms.assetid: 6b91d762-337b-4345-a159-88abb3e64a81
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 724b8c0df07cac555158661db6d00ccf98e7134c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752541"
---
# <a name="keep-nulls-or-default-values-during-bulk-import-sql-server"></a>Mantenere i valori Null o i valori predefiniti durante un'importazione bulk (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Per impostazione predefinita, durante l'importazione di dati in una tabella il comando [bcp](../../tools/bcp-utility.md) e l'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) osservano gli eventuali valori predefiniti che sono stati specificati per le colonne della tabella.  Ad esempio, se un file di dati contiene un campo Null, verrà caricato nel campo il valore predefinito della colonna.  Il comando [bcp](../../tools/bcp-utility.md) e l'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) consentono entrambi di specificare che dovranno essere mantenuti i valori Null.

Un'istruzione INSERT regolare mantiene invece il valore Null anziché inserire un valore predefinito. L'istruzione INSERT ... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) funziona in modo analogo a un'istruzione INSERT regolare, ma supporta inoltre un [hint di tabella](../../t-sql/queries/hints-transact-sql-table.md) per l'inserimento di valori predefiniti.

|Riquadro|
|---|
|[Mantenimento dei valori Null](#keep_nulls)<br />[Uso dei valori predefiniti con INSERT ... SELECT * FROM OPENROWSET(BULK...)](#keep_default)<br />[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di dati di esempio](#sample_data_file)<br />&emsp;&#9679;&emsp;[File di formato non XML di esempio](#nonxml_format_file)<br />[Mantenere i valori Null o usare i valori predefiniti durante un'importazione bulk](#import_data)<br />&emsp;&#9679;&emsp;[Uso di bcp e mantenimento dei valori Null senza un file di formato](#bcp_null)<br />&emsp;&#9679;&emsp;[Uso di bcp e mantenimento dei valori Null con un file di formato non XML](#bcp_null_fmt)<br />&emsp;&#9679;&emsp;[Uso di bcp e dei valori predefiniti senza un file di formato](#bcp_default)<br />&emsp;&#9679;&emsp;[Uso di bcp e dei valori predefiniti con un file di formato non XML](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e mantenimento dei valori Null senza un file di formato](#bulk_null)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e mantenimento dei valori Null con un file di formato non XML](#bulk_null_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e dei valori predefiniti senza un file di formato](#bulk_default)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e dei valori predefiniti con un file di formato non XML](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET(BULK...) e mantenimento dei valori Null con un file di formato non XML](#openrowset__null_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET(BULK...) e dei valori predefiniti con un file di formato non XML](#openrowset__default_fmt)

## <a name="keeping-null-values"></a>Mantenimento dei valori Null<a name="keep_nulls"></a>  
I qualificatori seguenti specificano che un campo vuoto del file di dati mantiene il relativo valore Null durante l'importazione bulk anziché ereditare un valore predefinito (se disponibile) per le colonne della tabella.  Per impostazione predefinita, per [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)le colonne non specificate nell'operazione di importazione vengono impostate su NULL.
  
|Comando|Qualifier|Tipo di qualificatore|  
|-------------|---------------|--------------------|  
|bcp|-k|Opzione|  
|BULK INSERT|KEEPNULLS\*|Argomento|  
|INSERT ... SELECT * FROM OPENROWSET(BULK...).|N/D|N/D|  
  
\* Per [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), se i valori predefiniti non sono disponibili, è necessario impostare la colonna della tabella in modo da consentire valori Null. 
  
> [!NOTE]
> Questi qualificatori disabilitano il controllo delle definizioni DEFAULT di una tabella mediante i comandi per l'importazione bulk.  Per le istruzioni INSERT simultanee, le definizioni DEFAULT sono tuttavia previste.
 
## <a name="using-default-values-with-insert--select--from-openrowsetbulk"></a>Uso dei valori predefiniti con INSERT ... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)<a name="keep_default"></a>  
È possibile specificare che la colonna della tabella corrispondente a un campo vuoto del file di dati userà il relativo valore predefinito (se disponibile).  Per usare i valori predefiniti, specificare l'hint di tabella [KEEPDEFAULTS](../../t-sql/queries/hints-transact-sql-table.md).
 
> [!NOTE]
>  Per altre informazioni, vedere [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md), [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md), [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md) e [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)

## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi riportati in questo argomento sono basati sulla tabella, il file di dati e il file di formato definiti di seguito.

### <a name="sample-table"></a>**Tabella di esempio**<a name="sample_table"></a>
Lo script seguente crea un database di prova e una tabella denominata `myNulls`.  Si noti che la quarta colonna della tabella, `Kids`, ha un valore predefinito.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNulls ( 
   PersonID smallint not null,
   FirstName varchar(25),
   LastName varchar(30),
   Kids varchar(13) DEFAULT 'Default Value',
   BirthDate date
   );
```

### <a name="sample-data-file"></a>**File di dati di esempio**<a name="sample_data_file"></a>
Usando il Blocco note, creare un file `D:\BCP\myNulls.bcp` vuoto e inserire i dati seguenti.  Si noti che nel terzo record della quarta colonna non è presente alcun valore.

```
1,Anthony,Grosse,Yes,1980-02-23
2,Alica,Fatnowna,No,1963-11-14
3,Stella,Rosenhain,,1992-03-02
```

In alternativa, è possibile eseguire lo script PowerShell seguente per creare e popolare il file di dati:

```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'MyNulls.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,Yes,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,No,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```
  
### <a name="sample-non-xml-format-file"></a>**File di formato non XML di esempio**<a name="nonxml_format_file"></a>
SQL Server supporta due tipi di file di formato, ovvero non XML e XML.  Il formato non XML è il formato originale supportato dalle versioni precedenti di SQL Server.  Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myNulls.fmt`, sulla base dello schema di `myNulls`.  Per usare un comando [bcp](../../tools/bcp-utility.md) per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati.  L'opzione format richiede anche l'opzione **-f** .  Inoltre, in questo esempio il qualificatore **c** viene usato per specificare dati di tipo carattere, **t,** viene usato per specificare la virgola come [carattere di terminazione del campo](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)e **T** viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere il comando seguente:

```cmd
bcp TestDatabase.dbo.myNulls format nul -c -f D:\BCP\myNulls.fmt -t, -T

REM Review file
Notepad D:\BCP\myNulls.fmt
```

> [!IMPORTANT]
> Verificare che il file di formato non XML termini con un ritorno a capo/avanzamento riga.  In caso contrario, è possibile che venga visualizzato il messaggio di errore seguente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

 Per informazioni sulla creazione di file di formato, vedere [Creare un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
## <a name="keep-nulls-or-use-default-values-during-bulk-import"></a>Mantenimento dei valori Null o utilizzo dei valori predefiniti durante un'importazione bulk<a name="import_data"></a>
Gli esempi seguenti usano il database, il file di dati e i file di formato creati in precedenza.

### <a name="using-bcp-and-keeping-null-values-without-a-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e mantenimento dei valori Null senza un file di formato**<a name="bcp_null"></a>

Opzione **-k** .  Al prompt dei comandi immettere il comando seguente:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### <a name="using-bcp-and-keeping-null-values-with-a-non-xml-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e mantenimento dei valori Null con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_null_fmt"></a>
Opzioni **-k** e **-f** . Al prompt dei comandi immettere il comando seguente:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### <a name="using-bcp-and-using-default-values-without-a-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e dei valori predefiniti senza un file di formato**<a name="bcp_default"></a>
Al prompt dei comandi immettere il comando seguente:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### <a name="using-bcp-and-using-default-values-with-a-non-xml-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e dei valori predefiniti con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_default_fmt"></a>
Opzione **-f** .  Al prompt dei comandi immettere il comando seguente:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### <a name="using-bulk-insert-and-keeping-null-values-without-a-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e mantenimento dei valori Null senza un file di formato**<a name="bulk_null"></a>
Argomento **KEEPNULLS** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO
TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
    FROM 'D:\BCP\myNulls.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-keeping-null-values-with-a-non-xml-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e mantenimento dei valori Null con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_null_fmt"></a>
Argomenti **KEEPNULLS** e **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt',
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-using-default-values-without-a-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e dei valori predefiniti senza un file di formato**<a name="bulk_default"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-using-default-values-with-a-non-xml-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e dei valori predefiniti con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_default_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-openrowsetbulk-and-keeping-null-values-with-a-non-xml-format-file"></a>**Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e mantenimento dei valori Null con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset__null_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-openrowsetbulk-and-using-default-values-with-a-non-xml-format-file"></a>**Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e dei valori predefiniti con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset__default_fmt"></a>
Hint di tabella **KEEPDEFAULTS** e argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
WITH (KEEPDEFAULTS) 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Mantenere i valori Identity durante l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)  
  
-   [Preparare i dati per l'importazione o l'esportazione in blocco &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)  
  
 **Per utilizzare un file di formato**  
  
-   [Creazione di un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
-   [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
 **Per utilizzare formati di dati per l'importazione o l'esportazione bulk**  
  
-   [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utilizzo del formato carattere Unicode per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
 **Per specificare i formati di dati per la compatibilità mediante bcp**  
  
-   [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
-   [Specificare la lunghezza del prefisso nei file di dati con bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
  
  
