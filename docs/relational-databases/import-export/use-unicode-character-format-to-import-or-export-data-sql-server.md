---
title: Usare il formato carattere Unicode per importare o esportare dati
description: Il formato di dati carattere Unicode consente di esportare i dati da un'istanza di SQL Server usando una tabella codici diversa da quella usata dal client.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], Unicode character
- Unicode [SQL Server], bulk importing and exporting
ms.assetid: 74342a11-c1c0-4746-b482-7f3537744a70
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 8e763f88431b2ac605a39c7bd5d43b3d51176d2a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751461"
---
# <a name="use-unicode-character-format-to-import-or-export-data-sql-server"></a>Usare il formato carattere Unicode per importare o esportare dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È consigliabile utilizzare il formato carattere Unicode per il trasferimento bulk di dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite un file di dati contenente caratteri estesi o DBCS. Questo formato di dati consente di trasferire i dati da un server utilizzando una tabella codici diversa da quella del client che esegue l'operazione. In questi casi, l'utilizzo del formato di dati carattere Unicode offre i vantaggi seguenti:  
  
* Se i dati di origine e di destinazione sono di tipo Unicode, il formato carattere Unicode consente di mantenere tutti i dati di tipo carattere.  
  
* Se i dati di origine e di destinazione non sono di tipo Unicode, il formato carattere Unicode consente di ridurre al minimo la perdita dei caratteri estesi nei dati di origine che non possono essere rappresentati nella destinazione.

|Contenuto dell'argomento:|
|---|
|[Considerazioni sull'uso del formato carattere Unicode](#considerations)|
|[Considerazioni speciali sull'uso del formato carattere Unicode, di bcp e di un file di formato](#special_considerations)|
|[Opzioni di comando per il formato carattere Unicode](#command_options)|
|[Condizioni di test di esempio](#etc)<br />&emsp;&#9679;&emsp;[Tabella di esempio](#sample_table)<br />&emsp;&#9679;&emsp;[File di formato non XML di esempio](#nonxml_format_file)|
|[esempi](#examples)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato carattere Unicode per l'esportazione di dati](#bcp_widechar_export)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato carattere Unicode per l'importazione di dati senza un file di formato](#bcp_widechar_import)<br />&emsp;&#9679;&emsp;[Uso di bcp e del formato carattere Unicode per l'importazione di dati con un file di formato non XML](#bcp_widechar_import_fmt)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e del formato carattere Unicode senza un file di formato](#bulk_widechar)<br />&emsp;&#9679;&emsp;[Uso di BULK INSERT e del formato carattere Unicode con un file di formato non XML](#bulk_widechar_fmt)<br />&emsp;&#9679;&emsp;[Uso di OPENROWSET e del formato carattere Unicode con un file di formato non XML](#openrowset_widechar_fmt)|
|[Attività correlate](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
 
## <a name="considerations-for-using-unicode-character-format"></a>Considerazioni sull'uso del formato carattere Unicode<a name="considerations"></a>
Quando si usa il formato carattere Unicode, è necessario tenere presenti i fattori seguenti:  

* Per impostazione predefinita, quando si esegue l' [utilità bcp](../../tools/bcp-utility.md) i campi dei dati di tipo carattere vengono separati da un carattere di tabulazione e alla fine dei record viene inserito un carattere di nuova riga.  Per informazioni su come specificare caratteri di terminazione alternativi, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

* I dati [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) archiviati in un file di dati in formato carattere Unicode funzionano così come in un file in formato carattere, con la differenza che vengono archiviati come dati [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) anziché [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) . Per altre informazioni sul formato carattere, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  

## <a name="special-considerations-for-using-unicode-character-format-bcp-and-a-format-file"></a>Considerazioni speciali sull'uso del formato carattere Unicode, di bcp e di un file di formato<a name="special_considerations"></a>
Per i file in formato carattere Unicode vengono rispettate le convenzioni dei file Unicode.  I primi due byte del file sono rappresentati da numeri esadecimali, 0xFFFE.  Tali byte hanno la funzione di indicatori per l'ordine dei byte (BOM) e specificano se archiviare il byte più significativo come primo o ultimo byte del file.  L' [utilità bcp](../../tools/bcp-utility.md) può interpretare il BOM in modo errato causando la non riuscita di parte del processo di importazione. È possibile che venga visualizzato un messaggio di errore simile a quello indicato di seguito:
```
Starting copy...
SQLState = 22005, NativeError = 0
Error = [Microsoft][ODBC Driver 13 for SQL Server]Invalid character value for cast specification
```

Il BOM potrebbe essere interpretato in modo errato nelle condizioni seguenti:
* L' [utilità bcp](../../tools/bcp-utility.md) viene usata insieme all'opzione **-w** per indicare il carattere Unicode

* Viene usato un file di formato

* Il primo campo nel file di dati è di tipo non carattere

Provare a usare una delle soluzioni alternative seguenti, a seconda della situazione *specifica* :
* Non usare un file di formato.  Un esempio di questa soluzione alternativa viene fornito più avanti. Vedere [Uso di bcp e del formato carattere Unicode per l'importazione di dati senza un file di formato](#bcp_widechar_import).

* Usare l'opzione **- c** anziché **-w**.

* Esportare nuovamente i dati usando un formato nativo.

* Usare [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  Esempi di queste soluzioni alternative vengono forniti più avanti. Vedere [Uso di BULK INSERT e del formato carattere Unicode con un file di formato non XML](#bulk_widechar_fmt) e [Uso di OPENROWSET e del formato carattere Unicode con un file di formato non XML](#openrowset_widechar_fmt).
 
* Inserire manualmente il primo record nella tabella di destinazione e quindi usare l'opzione **-F 2** affinché l'importazione venga avviata nel secondo record.

* Inserire manualmente il primo record fittizio nel file di dati e quindi usare l'opzione **-F 2** affinché l'importazione venga avviata nel secondo record.  Un esempio di questa soluzione alternativa viene fornito più avanti. Vedere [Uso di bcp e del formato carattere Unicode per l'importazione di dati con un file di formato non XML](#bcp_widechar_import_fmt).

* Usare una tabella di staging in cui la prima colonna è un tipo di dati carattere.

* Esportare nuovamente i dati e modificare l'ordine dei campi dati in modo che il primo campo dati sia di tipo carattere.  Usare quindi un file di formato per modificare il mapping del campo dati in base all'ordine effettivo della tabella.  Per un esempio, vedere [Usare un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md).
  
## <a name="command-options-for-unicode-character-format"></a>Opzioni di comando per il formato carattere Unicode<a name="command_options"></a>  
È possibile importare dati in formato carattere Unicode in una tabella usando [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Per un comando [bcp](../../tools/bcp-utility.md) o un'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), è possibile specificare il formato dati nell'istruzione.  Per un'istruzione [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) è necessario specificare il formato dati in un file di formato.  
  
Il formato carattere Unicode è supportato dalle opzioni di comando seguenti:  
  
|Comando|Opzione|Descrizione|  
|-------------|------------|-----------------|  
|bcp|**-w**|Utilizza il formato carattere Unicode.|  
|BULK INSERT|DATAFILETYPE **='widechar'**|Utilizza il formato carattere Unicode durante l'importazione bulk di dati.|  
|OPENROWSET|N/D|Deve usare un file di formato|
  
> [!NOTE]
>  In alternativa, è possibile definire la formattazione di ogni singolo campo in un file di formato. Per altre informazioni, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## <a name="example-test-conditions"></a>Condizioni di test di esempio<a name="etc"></a>  
Gli esempi riportati in questo argomento sono basati sulla tabella e sul file di formato definiti di seguito.

### <a name="sample-table"></a>**Tabella di esempio**<a name="sample_table"></a>
Lo script seguente crea un database di test, una tabella denominata `myWidechar` e popola la tabella con alcuni valori iniziali.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidechar ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidechar
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidechar;
```

### <a name="sample-non-xml-format-file"></a>**File di formato non XML di esempio**<a name="nonxml_format_file"></a>
SQL Server supporta due tipi di file di formato, ovvero non XML e XML.  Il formato non XML è il formato originale supportato dalle versioni precedenti di SQL Server.  Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myWidechar.fmt`, sulla base dello schema di `myWidechar`.  Per usare un comando [bcp](../../tools/bcp-utility.md) per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati.  L'opzione format richiede anche l'opzione **-f** .  Inoltre, in questo esempio il qualificatore **c** viene usato per specificare dati di tipo carattere e **T** viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere i comandi seguenti:

```
bcp TestDatabase.dbo.myWidechar format nul -f D:\BCP\myWidechar.fmt -T -w

REM Review file
Notepad D:\BCP\myWidechar.fmt
```

> [!IMPORTANT]
> Verificare che il file di formato non XML termini con un ritorno a capo/avanzamento riga.  In caso contrario, è possibile che venga visualizzato il messaggio di errore seguente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`


## <a name="examples"></a>Esempi<a name="examples"></a>
Gli esempi seguenti usano il database e i file di formato creati in precedenza.

### <a name="using-bcp-and-unicode-character-format-to-export-data"></a>**Uso di bcp e del formato carattere Unicode per l'esportazione di dati**<a name="bcp_widechar_export"></a>
Opzione **-w** e comando **OUT** .  Nota: il file di dati creato in questo esempio verrà usato in tutti gli esempi successivi.  Al prompt dei comandi immettere i comandi seguenti:
```
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w

REM Review results
NOTEPAD D:\BCP\myWidechar.bcp
```

### <a name="using-bcp-and-unicode-character-format-to-import-data-without-a-format-file"></a>**Uso di bcp e del formato carattere Unicode per l'importazione di dati senza un file di formato**<a name="bcp_widechar_import"></a>
Opzione **-w** e comando **IN** .  Al prompt dei comandi immettere i comandi seguenti:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Import data
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -T -w

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-character-format-to-import-data-with-a-non-xml-format-file"></a>**Uso di bcp e del formato carattere Unicode per l'importazione di dati con un file di formato non XML**<a name="bcp_widechar_import_fmt"></a>
Opzioni **-w** e **-f** switches e **IN** comme.  Sarà necessario adottare una soluzione alternativa poiché in questo esempio vengono usati bcp, un file di formato, un carattere Unicode e il primo campo dati del file di dati è di tipo non carattere.  Vedere [Considerazioni speciali sull'uso del formato carattere Unicode, di bcp e di un file di formato](#special_considerations)più indietro.  Il file di dati `myWidechar.bcp` verrà modificato aggiungendo un record "fittizio" che successivamente verrà ignorato con l'opzione `-F 2`.

Al prompt dei comandi, immettere i comandi seguenti e seguire i passaggi di modifica:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Open data file
Notepad D:\BCP\myWidechar.bcp
REM Copy first record and then paste as new first record.  This additional record is the "dummy" record.
REM Close file.

REM Import data instructing bcp to skip dummy record with the -F 2 switch.
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -f D:\BCP\myWidechar.fmt -T -F 2

REM Review results is SSMS

REM Return data file to original state for usage in other examples
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w
```
  
### <a name="using-bulk-insert-and-unicode-character-format-without-a-format-file"></a>**Uso di BULK INSERT e del formato carattere Unicode senza un file di formato**<a name="bulk_widechar"></a>
Argomento **DATAFILETYPE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
    FROM 'D:\BCP\myWidechar.bcp'
    WITH (
        DATAFILETYPE = 'widechar'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### <a name="using-bulk-insert-and-unicode-character-format-with-a-non-xml-format-file"></a>**Uso di BULK INSERT e del formato carattere Unicode con un file di formato non XML**<a name="bulk_widechar_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
   FROM 'D:\BCP\myWidechar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidechar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### <a name="using-openrowset-and-unicode-character-format-with-a-non-xml-format-file"></a>**Uso di OPENROWSET e del formato carattere Unicode con un file di formato non XML**<a name="openrowset_widechar_fmt"></a>
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar;  -- for testing
INSERT INTO TestDatabase.dbo.myWidechar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidechar.bcp', 
        FORMATFILE = 'D:\BCP\myWidechar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
 
  
## <a name="related-tasks"></a>Attività correlate<a name="RelatedTasks"></a>
Per utilizzare formati di dati per l'importazione o l'esportazione bulk  
-   [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  
