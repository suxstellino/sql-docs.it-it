---
title: Usare il formato nativo per importare ed esportare dati
description: Per le importazioni o le esportazioni in SQL Server, il formato nativo mantiene i tipi di dati nativi di un database per il trasferimento dati ad alta velocità dei dati tra tabelle di SQL Server.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 30b9d8f332c1dbb8147b836309724f2216cb9de1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750401"
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>Usare il formato nativo per importare o esportare dati (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
L'utilizzo del formato nativo è consigliabile per il trasferimento bulk dei dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante un file di dati in cui non sono inclusi caratteri estesi o DBCS (Double Byte Character Set).  

> [!NOTE]
>  Per il trasferimento bulk di dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante un file di dati contenente caratteri estesi o DBCS, è consigliabile utilizzare il formato nativo Unicode. Per altre informazioni, vedere [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).

Il formato nativo mantiene i tipi di dati nativi di un database e viene utilizzato per il trasferimento dei dati ad alta velocità tra le tabelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si utilizza un file di formato, non è necessario che le tabelle di origine e di destinazione siano identiche. Il trasferimento dei dati è costituito da due passaggi:  
  
1.  Esportazione bulk dei dati da una tabella di origine in un file di dati  
  
2.  Importazione bulk dei dati dal file di dati nella tabella di destinazione  

L'utilizzo del formato nativo tra tabelle identiche consente di evitare la conversione di tipi di dati nel/dal formato carattere, risparmiando tempo e spazio. Per raggiungere la velocità di trasferimento ottimale, tuttavia, vengono eseguiti solo alcuni controlli relativi alla formattazione dei dati. Per evitare problemi relativi ai dati caricati, vedere l'elenco di restrizioni seguente.  


## <a name="restrictions"></a>Restrizioni
Per importare correttamente i dati in formato nativo, verificare quanto segue:  
  
-   Il file di dati deve essere in formato nativo.  
  
-   È necessario che la tabella di destinazione sia compatibile con il file di dati (deve includere il numero di colonne, il tipo di dati e la lunghezza corretti, lo stato NULL e così via). In alternativa, è necessario utilizzare un file di formato per eseguire il mapping tra ogni campo e le colonne corrispondenti.  
  
    > [!NOTE]
    >  Se si importano i dati da un file non corrispondente alla tabella di destinazione, è possibile che l'operazione di importazione riesca ma i dati della tabella di destinazione potrebbero non essere corretti. Ciò è dovuto al fatto che i dati del file vengono interpretati utilizzando il formato della tabella di destinazione. Pertanto, la mancata corrispondenza causa l'inserimento di valori non corretti ma in nessun caso può determinare inconsistenze di tipo logico o fisico nel database.  
  
     Per altre informazioni sull'uso dei file di formato, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Un'importazione corretta non danneggerà la tabella di destinazione.  
  
## <a name="how-bcp-handles-data-in-native-format"></a>Gestione dei dati in formato nativo mediante l'utilità bcp
 In questa sezione sono contenute considerazioni particolari sull'importazione e sull'esportazione dei dati in formato nativo mediante l'utilità **bcp** .  
  
-   Dati non di tipo carattere  
  
     L' [utilità bcp](../../tools/bcp-utility.md) usa il formato dati binario interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per trasferire dati non di tipo carattere da una tabella in un file di dati.  
  
-   Dati[char](../../t-sql/data-types/char-and-varchar-transact-sql.md) o [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
  
     All'inizio di ogni campo [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) o [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) , l'utilità [bcp](../../tools/bcp-utility.md) aggiunge la lunghezza del prefisso.  
  
    > [!IMPORTANT]
    >  Per impostazione predefinita, quando si usa la modalità nativa, l'utilità [bcp](../../tools/bcp-utility.md) converte i caratteri di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in caratteri OEM prima di copiarli in un file di dati. L' [utilità bcp](../../tools/bcp-utility.md) converte i caratteri di un file di dati in caratteri ANSI prima dell'importazione bulk in una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Durante queste conversioni può verificarsi la perdita dei dati con caratteri estesi. Per i caratteri estesi, è necessario utilizzare il formato nativo Unicode o specificare una tabella codici.
  
-   Dati[sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
     Se i dati [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) vengono archiviati come SQLVARIANT in un file di dati in formato nativo, verranno mantenute tutte le relative caratteristiche. I metadati che registrano il tipo di dati di ogni valore vengono archiviati insieme al valore stesso. Questi metadati vengono usati per creare di nuovo il valore con lo stesso tipo di dati in una colonna di destinazione [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) .  
  
     Se il tipo di dati della colonna di destinazione è diverso da [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), ogni valore viene convertito nel tipo di dati della colonna di destinazione, in base alle regole standard di conversione implicita dei dati. Se si verifica un errore durante la conversione dei dati, viene eseguito il rollback del batch corrente. Per i valori [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) e [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) trasferiti tra colonne [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) possono verificarsi problemi relativi alla conversione di tabelle codici.  
  
     Per altre informazioni sulla conversione dei dati, vedere [Conversione del tipo di dati &#40;Motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## <a name="command-options-for-native-format"></a>Opzioni di comando per il formato nativo
È possibile importare i dati in formato nativo in una tabella con il comando [bcp](../../tools/bcp-utility.md), l'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o l'istruzione [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Per un comando [bcp](../../tools/bcp-utility.md) o un'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), è possibile specificare il formato dati nell'istruzione.  Per un'istruzione [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) è necessario specificare il formato dati in un file di formato.  

Il formato nativo è supportato dalle opzioni di comando seguenti:  

|Comando|Opzione|Descrizione|  
|-------------|------------|-----------------|  
|bcp|**-n**|Determina l'uso del tipo di dati nativo da parte dell'utilità bcp.*|  
|BULK INSERT|DATAFILETYPE **='native'**|Utilizza i tipi di dati nativi o nativi estesi. Si noti che DATAFILETYPE non è necessario se i tipi di dati vengono specificati in un file di formato.|  
|OPENROWSET|N/D|Deve usare un file di formato|

  
 \*Per caricare i dati nativi ( **-n**) in un formato compatibile con le versioni precedenti dei client di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , usare l'opzione **-V** . Per altre informazioni, vedere [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
> [!NOTE]
>  In alternativa, è possibile definire la formattazione di ogni singolo campo in un file di formato. Per altre informazioni, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  

## <a name="example-test-conditions"></a>Condizioni di test di esempio
Gli esempi riportati in questo argomento sono basati sulla tabella e sul file di formato definiti di seguito.

### <a name="sample-table"></a>Tabella di esempio
Lo script seguente crea un database di test, una tabella denominata `myNative` e popola la tabella con alcuni valori iniziali.  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="sample-non-xml-format-file"></a>File di formato non XML di esempio
SQL Server supporta due tipi di file di formato, ovvero non XML e XML.  Il formato non XML è il formato originale supportato dalle versioni precedenti di SQL Server.  Per informazioni dettagliate, vedere [File in formato non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Il comando seguente userà l' [utility bcp](../../tools/bcp-utility.md) per generare un formato di file non XML, `myNative.fmt`, sulla base dello schema di `myNative`.  Per usare un comando [bcp](../../tools/bcp-utility.md) per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati.  L'opzione format richiede anche l'opzione **-f** .  Inoltre, in questo esempio il qualificatore **c** viene usato per specificare dati di tipo carattere e **T** viene usato per specificare una connessione trusted che usa la sicurezza integrata.  Al prompt dei comandi immettere i comandi seguenti:

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> Verificare che il file di formato non XML termini con un ritorno a capo/avanzamento riga.  In caso contrario, è possibile che venga visualizzato il messaggio di errore seguente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Esempi
Gli esempi seguenti usano il database e i file di formato creati in precedenza.

### <a name="using-bcp-and-native-format-to-export-data"></a>Uso di bcp e del formato nativo per l'esportazione di dati
Opzione **-n** e comando **OUT** .  Nota: il file di dati creato in questo esempio verrà usato in tutti gli esempi successivi.  Al prompt dei comandi immettere i comandi seguenti:

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### <a name="using-bcp-and-native-format-to-import-data-without-a-format-file"></a>Uso di bcp e del formato nativo per l'importazione di dati senza un file di formato
Opzione **-n** e comando **IN** .  Al prompt dei comandi immettere i comandi seguenti:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bcp-and-native-format-to-import-data-with-a-non-xml-format-file"></a>Uso di bcp e del formato nativo per l'importazione di dati con un file di formato non XML
Opzioni **-n** e **-f** switches e **IN** comme.  Al prompt dei comandi immettere i comandi seguenti:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bulk-insert-and-native-format-without-a-format-file"></a>Uso di BULK INSERT e del formato nativo senza un file di formato
Argomento **DATAFILETYPE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-bulk-insert-and-native-format-with-a-non-xml-format-file"></a>Uso di BULK INSERT e del formato nativo con un file di formato non XML
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-openrowset-and-native-format-with-a-non-xml-format-file"></a>Uso di OPENROWSET e del formato nativo con un file di formato non XML
Argomento **FORMATFILE** .  Eseguire l'istruzione Transact-SQL seguente in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## <a name="related-tasks"></a>Attività correlate
Per utilizzare formati di dati per l'importazione o l'esportazione bulk 
  
-   [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utilizzo del formato carattere Unicode per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  
