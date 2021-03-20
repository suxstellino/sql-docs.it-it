---
title: Mantenere i valori Identity durante l'importazione bulk dei dati
description: Quando si esegue l'importazione bulk di dati che contengono valori Identity in un'istanza di SQL Server, per impostazione predefinita vengono assegnati nuovi valori. È possibile scegliere di mantenere i valori originali.
ms.date: 09/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server], bulk imports
- data formats [SQL Server], identity values
- bulk importing [SQL Server], identity values
ms.assetid: 45894a3f-2d8a-4edd-9568-afa7d0d3061f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: aa721f4597836068c07e5ee969c1eeb97dd20670
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752531"
---
# <a name="keep-identity-values-when-bulk-importing-data-sql-server"></a>Mantenere i valori Identity durante l'importazione bulk dei dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È possibile eseguire l'importazione bulk di file di dati contenenti valori Identity in un'istanza di Microsoft SQL Server.  Per impostazione predefinita, i valori per la colonna Identity del file di dati importato vengono ignorati e sostituiti automaticamente da valori univoci assegnati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  I valori univoci si basano sui valori di inizializzazione e incremento specificati durante la creazione della tabella.

Se il file di dati non contiene valori per la colonna dell'identificatore nella tabella, utilizzare un file di formato per specificare di ignorare la colonna dell'identificatore nella tabella durante l'importazione dei dati.  Per altre informazioni, vedere [Utilizzo di un file di formato per ignorare una colonna di una tabella (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md) .

|Riquadro|
|---|
|[Mantenere i valori Identity](#keep_identity)<br />[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di dati di esempio](#sample_data_file)<br />&emsp;&#9679;&emsp;[File di formato non XML di esempio](#nonxml_format_file)<br />[esempi](#examples)<br />&emsp;&#9679;&emsp;[Uso di bcp e mantenimento dei valori Identity senza un file di formato](#bcp_identity)<br />&emsp;&#9679;&emsp;[Uso di bcp e mantenimento dei valori Identity con un file di formato non XML](#bcp_identity_fmt)<br />&emsp;&#9679;&emsp;[Uso di bcp e dei valori Identity generati senza un file di formato](#bcp_default)<br />&emsp;&#9679;&emsp;[Uso di bcp e dei valori Identity generati con un file di formato non XML](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e mantenimento dei valori Identity senza un file di formato](#bulk_identity)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e mantenimento dei valori Identity con un file di formato non XML](#bulk_identity_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e dei valori Identity generati senza un file di formato](#bulk_default)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e dei valori Identity generati con un file di formato non XML](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET e mantenimento dei valori Identity con un file di formato non XML](#openrowset_identity_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET e dei valori Identity generati con un file di formato non XML](#openrowset_default_fmt)<br /><p>                                                                                                                                                                                                                  </p>|

## <a name="keep-identity-values"></a>Mantenere i valori Identity <a name="keep_identity"></a>  
Per impedire l'assegnazione da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di valori Identity durante l'importazione bulk delle righe di dati in una tabella, utilizzare il qualificatore per il mantenimento dei valori Identity corretto.  Quando si specifica un qualificatore per il mantenimento dei valori Identity, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza i valori Identity nel file di dati.  Sono disponibili i qualificatori seguenti:

|Comando|Qualificatore per il mantenimento dei valori Identity|Tipo di qualificatore|  
|-------------|------------------------------|--------------------|  
|bcp|-E|Opzione|  
|BULK INSERT|KEEPIDENTITY|Argomento|  
|INSERT ... SELECT * FROM OPENROWSET(BULK...).|KEEPIDENTITY|Hint di tabella|  
   
 Per altre informazioni, vedere [Utilità bcp](../../tools/bcp-utility.md), [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md), [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md), [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md), [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md) e [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  

> [!NOTE]
>  Per creare un numero a incremento automatico da usare in più tabelle o da chiamare dalle applicazioni senza fare riferimento ad alcuna tabella, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).
 
## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi riportati in questo argomento sono basati sulla tabella, il file di dati e il file di formato definiti di seguito.

### <a name="sample-table"></a>**Tabella di esempio**<a name="sample_table"></a>
Lo script seguente crea un database di prova e una tabella denominata `myIdentity`.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myIdentity ( 
   PersonID smallint IDENTITY(1,1) NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date
   );
```
 
### <a name="sample-data-file"></a>**File di dati di esempio**<a name="sample_data_file"></a>
Usando il Blocco note, creare un file `D:\BCP\myIdentity.bcp` vuoto e inserire i dati seguenti.  
```
3,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
1,Stella,Rosenhain,1992-03-02
4,Miller,Dylan,1954-01-05
```
  
In alternativa, è possibile eseguire lo script PowerShell seguente per creare e popolare il file di dati:
```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'myIdentity.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '3,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '1,Stella,Rosenhain,1992-03-02';
Add-Content -Path $bcpFile -Value '4,Miller,Dylan,1954-01-05';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```

### <a name="sample-non-xml-format-file"></a>**File di formato non XML di esempio**<a name="nonxml_format_file"></a>
SQL Server supporta due tipi di file di formato, ovvero non XML e XML.  Il formato non XML è il formato originale supportato dalle versioni precedenti di SQL Server.  Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myIdentity.fmt`, sulla base dello schema di `myIdentity`.  Per usare un comando [bcp](../../tools/bcp-utility.md) per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati.  L'opzione format richiede anche l'opzione **-f** .  Inoltre, in questo esempio il qualificatore **c** viene usato per specificare dati di tipo carattere, **t,** viene usato per specificare la virgola come [carattere di terminazione del campo](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)e **T** viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere il comando seguente:
  
```
bcp TestDatabase.dbo.myIdentity format nul -c -f D:\BCP\myIdentity.fmt -t, -T

REM Review file
Notepad D:\BCP\myIdentity.fmt
```
 
> [!IMPORTANT]
> Verificare che il file di formato non XML termini con un ritorno a capo/avanzamento riga.  In caso contrario, è possibile che venga visualizzato il messaggio di errore seguente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Esempi<a name="examples"></a>
Gli esempi seguenti usano il database, il file di dati e i file di formato creati in precedenza.
  
### <a name="using-bcp-and-keeping-identity-values-without-a-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e mantenimento dei valori Identity senza un file di formato**<a name="bcp_identity"></a>
Opzione **-E** .  Al prompt dei comandi immettere il comando seguente:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t, -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```

### <a name="using-bcp-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e mantenimento dei valori Identity con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_identity_fmt"></a>
Opzioni **-E** e **-f** .  Al prompt dei comandi immettere il comando seguente:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
 
### <a name="using-bcp-and-generated-identity-values-without-a-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e dei valori Identity generati senza un file di formato**<a name="bcp_default"></a>
Uso dei valori predefiniti.  Al prompt dei comandi immettere il comando seguente:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t,

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### <a name="using-bcp-and-generated-identity-values-with-a-non-xml-format-file"></a>**Uso di [bcp](../../tools/bcp-utility.md) e dei valori Identity generati con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_default_fmt"></a>
Uso dei valori predefiniti e dell'opzione **-f** .  Al prompt dei comandi immettere il comando seguente:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### <a name="using-bulk-insert-and-keeping-identity-values-without-a-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e mantenimento dei valori Identity senza un file di formato**<a name="bulk_identity"></a>
Argomento **KEEPIDENTITY** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
    FROM 'D:\BCP\myIdentity.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e mantenimento dei valori Identity con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_identity_fmt"></a>
Argomenti **KEEPIDENTITY** e **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt',
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-generated-identity-values-without-a-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e dei valori Identity generati senza un file di formato**<a name="bulk_default"></a>
Uso dei valori predefiniti.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-generated-identity-values-with-a-non-xml-format-file"></a>**Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e dei valori Identity generati con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_default_fmt"></a>
Uso dei valori predefiniti e dell'argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-openrowsetbulk-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e mantenimento dei valori Identity con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset_identity_fmt"></a>
Hint di tabella **KEEPIDENTITY** e argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
WITH (KEEPIDENTITY) 
(PersonID, FirstName, LastName, BirthDate)
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
 
### <a name="using-openrowsetbulk-and-generated-identity-values-with-a-non-xml-format-file"></a>**Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e dei valori Identity generati con un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset_default_fmt"></a>
Uso dei valori predefiniti e dell'argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
(FirstName, LastName, BirthDate)
    SELECT FirstName, LastName, BirthDate
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Mantenimento dei valori Null o uso dei valori predefiniti durante un'importazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
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
  
1.  [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
2.  [Specificare la lunghezza del prefisso nei file di dati con bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
3.  [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
[File di formato per l'importazione o l'esportazione di dati (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
