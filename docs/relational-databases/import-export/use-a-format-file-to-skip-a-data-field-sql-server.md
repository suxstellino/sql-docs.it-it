---
title: Usare un file di formato per escludere un campo di dati
description: È possibile usare un file di formato con un file di dati con un numero di campi superiore a quello delle colonne della tabella. Le colonne della tabella vengono mappate ai campi dati corrispondenti e i campi aggiuntivi vengono ignorati.
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], skipping data fields
- skipping data fields when importing
ms.assetid: 6a76517e-983b-47a1-8f02-661b99859a8b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: d939948fe5644b4d8f347bf36a962b52fe22e35c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745411"
---
# <a name="use-a-format-file-to-skip-a-data-field-sql-server"></a>Usare un file di formato per escludere un campo dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È possibile che un file di dati contenga un numero di campi maggiore del numero di colonne presenti nella tabella. In questo argomento viene descritta la modifica dei file di formato XML e non XML per consentire l'utilizzo di un file di dati con un numero maggiore di campi tramite il mapping delle colonne della tabella ai campi dati corrispondenti e l'esclusione dei campi aggiuntivi.  Per altre informazioni, vedere [Creazione di un file di formato (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) .

|Riquadro|
|---|
|[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di dati di esempio](#sample_data_file)<br />[Creazione dei file di formato](#create_format_file)<br />&emsp;&#9679;&emsp;[Creazione di un file di formato non XML](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[Modifica di un file di formato non XML](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[Creazione di un file di formato XML](#xml_format_file)<br />&emsp;&#9679;&emsp;[Modifica di un file di formato XML](#modify_xml_format_file)<br />[Importazione di dati con un file di formato per escludere un campo di dati](#import_data)<br />&emsp;&#9679;&emsp;[Uso di bcp e di un file di formato non XML](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[Uso di bcp e di un file di formato XML](#bcp_xml)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e di un file di formato non XML](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e di un file di formato XML](#bulk_xml)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET(BULK...) e di un file di formato non XML](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET(BULK...) e di un file di formato XML](#openrowset_xml)<p>                                                                                                                                                                                                                  </p>|
  
> [!NOTE]
>  È possibile usare un file di formato non XML o XML per importare in blocco un file di dati nella tabella usando un comando dell'[utilità bcp](../../tools/bcp-utility.md), l'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o l'istruzione INSERT... Istruzione SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Per altre informazioni, vedere [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi di file di formato modificati contenuti in questo argomento sono basati sulla tabella e sul file di dati definiti di seguito.
  
### <a name="sample-table"></a>Tabella di esempio<a name="sample_table"></a>
Lo script seguente crea un database di prova e una tabella denominata `myTestSkipField`.  Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
 
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myTestSkipField
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30)
   );
```
  
### <a name="sample-data-file"></a>File di dati di esempio<a name="sample_data_file"></a>
Creare un file vuoto `D:\BCP\myTestSkipField.bcp` e inserire i dati seguenti: 
```
1,SkipMe,Anthony,Grosse
2,SkipMe,Alica,Fatnowna
3,SkipMe,Stella,Rosenhain
```
  
## <a name="creating-the-format-files"></a>Creazione dei file di formato<a name="create_format_file"></a>
Per eseguire un'importazione bulk dei dati da `myTestSkipField.bcp` nella tabella `myTestSkipField` , è necessario che il file di formato esegua le operazioni seguenti:
* Mapping del primo campo dati alla prima colonna `PersonID`.
* Esclusione del secondo campo dati.
* Mapping del terzo campo dati alla seconda colonna `FirstName`.
* Mapping del quarto campo dati alla terza colonna `LastName`.  

Il metodo più semplice per creare il file di formato consiste nell'usare l' [utilità bcp](../../tools/bcp-utility.md).  Prima di tutto, creare un file di formato di base dalla tabella esistente.  In secondo luogo, modificare il file di formato di base in modo da riflettere il file di dati effettivo.
  
### <a name="creating-a-non-xml-format-file"></a><a name="nonxml_format_file"></a>Creazione di un file di formato non XML 
Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) . Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myTestSkipField.fmt`, sulla base dello schema di `myTestSkipField`.  Inoltre, il qualificatore `c` viene usato per specificare dati di tipo carattere, `t,` viene usato per specificare la virgola come carattere di terminazione del campo e `T` viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere il comando seguente:

```
bcp TestDatabase.dbo.myTestSkipField format nul -c -f D:\BCP\myTestSkipField.fmt -t, -T
```

### <a name="modifying-the-non-xml-format-file"></a>Modifica del file di formato non XML <a name="modify_nonxml_format_file"></a>
Per la terminologia, vedere [Struttura dei file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) . Aprire `D:\BCP\myTestSkipField.fmt` nel Blocco note e apportare le modifiche seguenti:
1) Copiare l'intera riga del file di formato per `FirstName` e incollarla direttamente dopo `FirstName` nella riga successiva.
2) Aumentare di uno il valore di ordine dei campi del file host per la nuova riga e tutte le righe successive.
3) Aumentare il valore del numero di colonne in modo da riflettere il numero effettivo di campi nel file di dati.
3) Modificare l'ordine delle colonne del server da `2` a `0` per la seconda riga di file di formato. 

Confrontare le modifiche apportate:  
**Prima**
```
13.0
3
1       SQLCHAR 0       7       ","      1     PersonID     ""
2       SQLCHAR 0       25      ","      2     FirstName    SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       30      "\r\n"   3     LastName     SQL_Latin1_General_CP1_CI_AS
```
**Dopo**
```
13.0
4
1       SQLCHAR 0       7       ","      1     PersonID     ""
2       SQLCHAR 0       25      ","      0     FirstName    SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       25      ","      2     FirstName    SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR 0       50      "\r\n"   3     LastName     SQL_Latin1_General_CP1_CI_AS

```

Il file di formato modificato riflette ora:
* 4 campi dati
* Il primo campo dati in `myTestSkipField.bcp` è mappato alla prima colonna, `myTestSkipField.. PersonID`
* Il secondo campo dati `myTestSkipField.bcp` non è mappato ad alcuna colonna.
* Il terzo campo dati in `myTestSkipField.bcp` è mappato alla seconda colonna, `myTestSkipField.. FirstName`
* Il quarto campo dati in `myTestSkipField.bcp` è mappato alla terza colonna, `myTestSkipField.. LastName`

### <a name="creating-an-xml-format-file"></a>Creazione di un file di formato XML <a name="xml_format_file"></a>  
Per informazioni dettagliate, vedere [File in formato XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) .  Il comando seguente userà l' [utilità bcp](../../tools/bcp-utility.md) per creare un file di formato XML, `myTestSkipField.xml`, sulla base dello schema di `myTestSkipField`.  Inoltre, il qualificatore `c` viene usato per specificare dati di tipo carattere, `t,` viene usato per specificare la virgola come carattere di terminazione del campo e `T` viene usato per specificare una connessione trusted che usa la sicurezza integrata.  È necessario usare il qualificatore `x` per generare un file di formato basato su XML.  Al prompt dei comandi immettere il comando seguente:

```
bcp TestDatabase.dbo.myTestSkipField format nul -c -x -f D:\BCP\myTestSkipField.xml -t, -T
```

### <a name="modifying-the-xml-format-file"></a>Modifica del file di formato XML <a name="modify_xml_format_file"></a>
Per la terminologia, vedere [Sintassi dello schema per i file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) .  Aprire `D:\BCP\myTestSkipField.xml` nel Blocco note e apportare le modifiche seguenti:
1) Copiare tutto il secondo campo e incollarlo immediatamente dopo il secondo campo nella riga successiva.
2) Aumentare di 1 il valore "FIELD ID"per il nuovo campo e per ogni campo successivo.
3) Aumentare di 1 il valore "COLUMN SOURCE" per `FirstName`e `LastName` in modo da riflettere il mapping modificato.

Confrontare le modifiche apportate:  
**Prima**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
 </ROW>
</BCPFORMAT>
```

**Dopo**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="4" NAME="LastName" xsi:type="SQLVARYCHAR"/>
 </ROW>
</BCPFORMAT>
```

Il file di formato modificato riflette ora:
* 4 campi dati
* FIELD 1, che corrisponde a COLUMN 1, è mappato alla prima colonna della tabella, `myTestSkipField.. PersonID`
* FIELD 2 non corrisponde ad alcun valore COLUMN, per cui non è mappato ad alcuna colonna della tabella.
* FIELD 3, che corrisponde a COLUMN 3, è mappato alla seconda colonna della tabella, `myTestSkipField.. FirstName`
* FIELD 4, che corrisponde a COLUMN 4, è mappato alla terza colonna della tabella, `myTestSkipField.. LastName`

## <a name="importing-data-with-a-format-file-to-skip-a-data-field"></a>Importazione di dati con un file di formato per escludere un campo di dati<a name="import_data"></a>
Gli esempi seguenti usano il database, il file di dati e i file di formato creati in precedenza.

### <a name="using-bcp-and-non-xml-format-file"></a>Uso di [bcp](../../tools/bcp-utility.md) e di un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
Al prompt dei comandi immettere il comando seguente:
```
bcp TestDatabase.dbo.myTestSkipField IN D:\BCP\myTestSkipField.bcp -f D:\BCP\myTestSkipField.fmt -T
```

### <a name="using-bcp-and-xml-format-file"></a>Uso di [bcp](../../tools/bcp-utility.md) e di un [file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
Al prompt dei comandi immettere il comando seguente:
```
bcp TestDatabase.dbo.myTestSkipField IN D:\BCP\myTestSkipField.bcp -f D:\BCP\myTestSkipField.xml -T
```

### <a name="using-bulk-insert-and-non-xml-format-file"></a>Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e di un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
BULK INSERT dbo.myTestSkipField   
   FROM 'D:\BCP\myTestSkipField.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myTestSkipField.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e di un [file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
BULK INSERT dbo.myTestSkipField   
   FROM 'D:\BCP\myTestSkipField.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myTestSkipField.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e di un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myTestSkipField;
INSERT INTO dbo.myTestSkipField
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myTestSkipField.bcp',
        FORMATFILE = 'D:\BCP\myTestSkipField.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e di un [file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
INSERT INTO dbo.myTestSkipField 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myTestSkipField.bcp',
        FORMATFILE = 'D:\BCP\myTestSkipField.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
  
