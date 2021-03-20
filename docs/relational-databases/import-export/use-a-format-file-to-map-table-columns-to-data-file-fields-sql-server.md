---
title: Eseguire il mapping tra le colonne della tabella e i campi del file di dati con un file di formato
description: In SQL Server i file di formato XML e non XML possono essere usati per file di dati con campi disposti in un ordine diverso rispetto alle colonne della tabella.
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- mapping columns to fields during import [SQL Server]
- format files [SQL Server], mapping columns to fields
ms.assetid: e7ee4f7e-24c4-4eb7-84d2-41e57ccc1ef1
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 6b2a501eafa04901aa72e6adedbdcd99854377db
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745781"
---
# <a name="use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server"></a>Usare un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Un file di dati può includere campi disposti in un ordine diverso da quello delle colonne corrispondenti presenti nella tabella. In questo argomento vengono descritti file di formato sia non XML che XML, modificati per adattarli a un file di dati contenente campi disposti un ordine diverso da quello delle colonne della tabella. Il file di formato modificato esegue il mapping tra i campi dati e le colonne corrispondenti della tabella.  Per altre informazioni, vedere [Creazione di un file di formato (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) .

|Riquadro|
|---|
|[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di dati di esempio](#sample_data_file)<br />[Creazione dei file di formato](#create_format_file)<br />&emsp;&#9679;&emsp;[Creazione di un file di formato non XML](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[Modifica del file di formato non XML](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[Creazione di un file di formato XML](#xml_format_file)<br />&emsp;&#9679;&emsp;[Modifica del file di formato XML](#modify_xml_format_file)<br />[Importazione dei dati con un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati](#import_data)<br />&emsp;&#9679;&emsp;[Uso di bcp e di un file di formato non XML](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[Uso di bcp e di un file di formato XML](#bcp_xml)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e di un file di formato non XML](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e di un file di formato XML](#bulk_xml)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET(BULK...) e di un file di formato non XML](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET(BULK...) e di un file di formato XML](#openrowset_xml)|

> [!NOTE]  
>  È possibile usare un file di formato non XML o XML per importare in blocco un file di dati nella tabella usando un comando dell'[utilità bcp](../../tools/bcp-utility.md), l'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o l'istruzione INSERT... Istruzione SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Per altre informazioni, vedere [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  

## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi di file di formato modificati contenuti in questo argomento sono basati sulla tabella e sul file di dati definiti di seguito.

### <a name="sample-table"></a>Tabella di esempio<a name="sample_table"></a>
Lo script seguente crea un database di prova e una tabella denominata `myRemap`.  Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myRemap
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   Gender char(1)
   );
```

### <a name="sample-data-file"></a>File di dati di esempio<a name="sample_data_file"></a>
I dati che seguono presentano `FirstName` e `LastName` in ordine inverso come illustrato nella tabella `myRemap`.  Usando il Blocco note, creare un file `D:\BCP\myRemap.bcp` vuoto e inserire i dati seguenti:
```
1,Grosse,Anthony,M
2,Fatnowna,Alica,F
3,Rosenhain,Stella,F
```

## <a name="creating-the-format-files"></a>Creazione dei file di formato<a name="create_format_file"></a>
Per eseguire un'importazione bulk dei dati da `myRemap.bcp` nella tabella `myRemap` , è necessario che il file di formato esegua le operazioni seguenti:
* Mapping del primo campo dati alla prima colonna `PersonID`.
* Mapping del secondo campo dati alla terza colonna `LastName`.
* Mapping del terzo campo dati alla seconda colonna `FirstName`.
* Mapping del quarto campo dati alla quarta colonna `Gender`.

Il metodo più semplice per creare il file di formato consiste nell'usare l' [utilità bcp](../../tools/bcp-utility.md).  Prima di tutto, creare un file di formato di base dalla tabella esistente.  In secondo luogo, modificare il file di formato di base in modo da riflettere il file di dati effettivo.

### <a name="creating-a-non-xml-format-file"></a>Creazione di un file di formato non XML<a name="nonxml_format_file"></a>
Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) . Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myRemap.fmt`, sulla base dello schema di `myRemap`.  Inoltre, il qualificatore `c` viene usato per specificare dati di tipo carattere, `t,` viene usato per specificare la virgola come carattere di terminazione del campo e `T` viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere il comando seguente:
```
bcp TestDatabase.dbo.myRemap format nul -c -f D:\BCP\myRemap.fmt -t, -T
```
### <a name="modifying-the-non-xml-format-file"></a>Modifica del file di formato non XML <a name="modify_nonxml_format_file"></a>
Per la terminologia, vedere [Struttura dei file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) .  Aprire `D:\BCP\myRemap.fmt` nel Blocco note e apportare le modifiche seguenti:
1.  Riorganizzare l'ordine delle righe del file di formato in modo che le righe siano nello stesso ordine dei dati in `myRemap.bcp`.
2.  Assicurarsi che i valori dell'ordine dei campi nel file host siano sequenziali.
3.  Verificare che dopo l'ultima riga del file di formato ci sia un ritorno a capo.

Confrontare le modifiche:     
**Prima**
```
13.0
4
1       SQLCHAR    0       7       ","      1     PersonID               ""
2       SQLCHAR    0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR    0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR    0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
**Dopo**
```
13.0
4
1       SQLCHAR    0       7       ","      1     PersonID               ""
2       SQLCHAR    0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR    0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR    0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
Il file di formato modificato riflette ora:
* Il primo campo dati in `myRemap.bcp` è mappato alla prima colonna, `myRemap.. PersonID`
* Il secondo campo dati in `myRemap.bcp` è mappato alla terza colonna, `myRemap.. LastName`
* Il terzo campo dati in `myRemap.bcp` è mappato alla seconda colonna, `myRemap.. FirstName`
* Il quarto campo dati in `myRemap.bcp` è mappato alla quarta colonna, `myRemap.. Gender`

### <a name="creating-an-xml-format-file"></a>Creazione di un file di formato XML <a name="xml_format_file"></a>  
Per informazioni dettagliate, vedere [File in formato XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) .  Il comando seguente userà l' [utilità bcp](../../tools/bcp-utility.md) per creare un file di formato XML, `myRemap.xml`, sulla base dello schema di `myRemap`.  Inoltre, il qualificatore `c` viene usato per specificare dati di tipo carattere, `t,` viene usato per specificare la virgola come carattere di terminazione del campo e `T` viene usato per specificare una connessione trusted che usa la sicurezza integrata.  È necessario usare il qualificatore `x` per generare un file di formato basato su XML.  Al prompt dei comandi immettere il comando seguente:
```
bcp TestDatabase.dbo.myRemap format nul -c -x -f D:\BCP\myRemap.xml -t, -T
```
### <a name="modifying-the-xml-format-file"></a>Modifica del file di formato XML <a name="modify_xml_format_file"></a>
Per la terminologia, vedere [Sintassi dello schema per i file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) .  Aprire `D:\BCP\myRemap.xml` nel Blocco note e apportare le modifiche seguenti:
1. Poiché l'ordine con cui gli elementi \<FIELD> sono dichiarati nel file di formato corrisponde a quello con cui i campi sono riportati nel file di dati, invertire l'ordine degli elementi \<FIELD> con attributi ID 2 e 3.
2. Assicurarsi che i valori dell'attributo ID \<FIELD> siano sequenziali.
3. L'ordine degli elementi \<COLUMN> nell'elemento \<ROW> definisce l'ordine in cui l'operazione in blocco invia gli elementi alla destinazione.  Il file di formato XML assegna ogni elemento \<COLUMN> a un nome locale senza alcuna relazione con la colonna nella tabella di destinazione di un'operazione di importazione in blocco.  L'ordine degli elementi \<COLUMN> è indipendente da quello degli elementi \<FIELD> in una definizione \<RECORD>.  Ogni elemento \<COLUMN> corrisponde a un elemento \<FIELD>, il cui ID viene specificato nell'attributo SOURCE dell'elemento \<COLUMN>.  Di conseguenza, i valori per \<COLUMN> SOURCE sono gli unici attributi che richiedono revisione.  Invertire l'ordine per gli attributi \<COLUMN> SOURCE 2 e 3.

Confrontare le modifiche  
**Prima**
```
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
**Dopo**
```
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="2" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
Il file di formato modificato riflette ora:
* FIELD 1, che corrisponde a COLUMN 1, è mappato alla prima colonna della tabella, `myRemap.. PersonID`
* FIELD 2, che corrisponde a COLUMN 2, è di nuovo mappato alla terza colonna della tabella, `myRemap.. LastName`
* FIELD 3, che corrisponde a COLUMN 3, è di nuovo mappato alla seconda colonna della tabella, `myRemap.. FirstName`
* FIELD 4, che corrisponde a COLUMN 4, è mappato alla quarta colonna della tabella, `myRemap.. Gender`


## <a name="importing-data-with-a-format-file-to-map-table-columns-to-data-file-field"></a>Importazione dei dati con un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati<a name="import_data"></a>
Gli esempi seguenti usano il database, il file di dati e i file di formato creati in precedenza.

### <a name="using-bcp-and-non-xml-format-file"></a>Uso di [bcp](../../tools/bcp-utility.md) e di un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
Al prompt dei comandi immettere il comando seguente:
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.fmt -T
```

### <a name="using-bcp-and-xml-format-file"></a>Uso di [bcp](../../tools/bcp-utility.md) e di un [file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
Al prompt dei comandi immettere il comando seguente:
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.xml -T
```

### <a name="using-bulk-insert-and-non-xml-format-file"></a>Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e di un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>Uso di [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) e di un [file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e di un [file di formato non XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>Uso di [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) e di un [file di formato XML](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
Eseguire l'istruzione Transact-SQL seguente in Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```


  
## <a name="see-also"></a>Vedere anche  
[Utilità bcp](../../tools/bcp-utility.md)   
 [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
  
