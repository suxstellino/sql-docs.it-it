---
description: ALTER INDEX (Transact-SQL)
title: ALTER INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER INDEX
- ALTER_INDEX_TSQL
dev_langs:
- t-sql
helpviewer_keywords:
- indexes [SQL Server], reorganizing
- ALTER INDEX statement
- indexes [SQL Server], disabling
- online index operations
- index reorganization [SQL Server]
- ALLOW_ROW_LOCKS option
- ALL keyword
- reorganizing indexes
- constraints [SQL Server], indexes
- row locks [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- locking [SQL Server], indexes
- partitioned indexes [SQL Server], rebuilding
- defragmenting indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- index modifications [SQL Server]
- indexes [SQL Server], modifying
- index options [SQL Server]
- modifying indexes
- index disabling [SQL Server]
- MAXDOP index option, ALTER INDEX statement
- spatial indexes [SQL Server], modifying
- indexes [SQL Server], options
- ALLOW_PAGE_LOCKS option
- page locks [SQL Server]
- index rebuild [SQL Server]
- index reorganize [SQL Server]
ms.assetid: b796c829-ef3a-405c-a784-48286d4fb2b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7854c78f0643294b1b5111c1b6f2e0b0ef07afa6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099588"
---
# <a name="alter-index-transact-sql"></a>ALTER INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Consente di modificare un indice di tabella o di vista esistente, di tipo rowstore, columnstore o XML, tramite la disabilitazione, la ricompilazione o la riorganizzazione dell'indice oppure tramite l'impostazione di opzioni per l'indice.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
ALTER INDEX { index_name | ALL } ON <object>  
{  
      REBUILD {  
            [ PARTITION = ALL ] [ WITH ( <rebuild_index_option> [ ,...n ] ) ]   
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> ) [ ,...n ] ]  
      }  
    | DISABLE  
    | REORGANIZE  [ PARTITION = partition_number ] [ WITH ( <reorganize_option>  ) ]  
    | SET ( <set_index_option> [ ,...n ] )   
    | RESUME [WITH (<resumable_index_options>,[...n])]
    | PAUSE
    | ABORT
}  
[ ; ]  
  
<object> ::=   
{  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
}  
  
<rebuild_index_option > ::=  
{  
      PAD_INDEX = { ON | OFF }  
    | FILLFACTOR = fillfactor   
    | SORT_IN_TEMPDB = { ON | OFF }  
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | STATISTICS_INCREMENTAL = { ON | OFF }  
    | ONLINE = {   
          ON [ ( <low_priority_lock_wait> ) ]   
        | OFF } 
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }   
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]  
}  
  
<single_partition_rebuild_index_option> ::=  
{  
      SORT_IN_TEMPDB = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
    | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<reorganize_option>::=  
{  
       LOB_COMPACTION = { ON | OFF }  
    |  COMPRESS_ALL_ROW_GROUPS =  { ON | OFF}  
}  
  
<set_index_option>::=  
{  
      ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | COMPRESSION_DELAY= { 0 | delay [Minutes] }  
}  

<resumable_index_option> ::=
 { 
    MAXDOP = max_degree_of_parallelism
    | MAX_DURATION =<time> [MINUTES]
    | <low_priority_lock_wait>  
 }
 
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                          ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )  
}  

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse 
  
ALTER INDEX { index_name | ALL }  
    ON   [ schema_name. ] table_name  
{  
      REBUILD {  
            [ PARTITION = ALL [ WITH ( <rebuild_index_option> ) ] ] 
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> )] ] 
      }  
    | DISABLE  
    | REORGANIZE [ PARTITION = partition_number ]  
}  
[;]  

<rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]   
}

<single_partition_rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
}  
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

 *index_name*  
 Nome dell'indice. I nomi di indice devono essere univoci all'interno di una tabella o di una vista, ma non all'interno di un database. Devono essere anche conformi alle regole degli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 ALL  
 Specifica tutti gli indici associati alla tabella o alla vista indipendentemente dal tipo di indice. Se viene specificata la parola chiave ALL e uno o più indici si trovano in un filegroup offline o di sola lettura oppure se l'operazione specificata non è consentita per uno o più tipi di indice, l'istruzione ha esito negativo. Nella tabella seguente vengono elencati le operazioni sugli indici e i tipi di indice non supportati.  
  
|Uso della parola chiave ALL con questa operazione|Indici non supportati (l'istruzione ha esito negativo se la tabella include uno o più di questi indici)|  
|----------------------------------------|----------------------------------------|  
|REBUILD WITH ONLINE = ON|Indice XML<br /><br /> Indice spaziale<br /><br /> Indice columnstore: **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|REBUILD PARTITION = *partition_number*|Indice non partizionato, indice XML, indice spaziale o indice disabilitato|  
|REORGANIZE|Indici con ALLOW_PAGE_LOCKS impostato su OFF|  
|REORGANIZE PARTITION = *partition_number*|Indice non partizionato, indice XML, indice spaziale o indice disabilitato|  
|IGNORE_DUP_KEY = ON|Indice XML<br /><br /> Indice spaziale<br /><br /> Indice columnstore: **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|ONLINE = ON|Indice XML<br /><br /> Indice spaziale<br /><br /> Indice columnstore: **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|RESUMABLE = ON| Indici ripristinabili non supportati con la parola chiave **ALL**. <br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |   
  
> [!WARNING]
> Per informazioni più dettagliate sulle operazioni di indice eseguibili online, vedere [Linee guida per operazioni di indice online](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

 Se la parola chiave ALL viene specificata con PARTITION = *partition_number*, tutti gli indici devono essere allineati, il che significa che devono essere partizionati in base a funzioni di partizione equivalenti. L'uso di ALL con PARTITION comporta la ricompilazione o la riorganizzazione di tutte le partizioni degli indici con lo stesso *partition_number*. Per ulteriori informazioni sugli indici partizionati, vedere [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 *database_name*  
 Nome del database.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la tabella o la vista.  
  
 *table_or_view_name*  
 Nome della tabella o della vista associata all'indice. Per visualizzare un report degli indici di un oggetto, usare la vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Il [!INCLUDE[ssSDS](../../includes/sssds-md.md)] supporta il formato del nome in tre parti database_name.[schema_name].table_or_view_name, dove database_name è il database corrente o tempdb e table_or_view_name inizia con #.  
  
 REBUILD [ WITH **(** \<rebuild_index_option> [ **,** ... *n*] **)** ]  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Specifica che l'indice verrà ricompilato con le stesse colonne, lo stesso tipo di indice, lo stesso attributo di univocità e lo stesso tipo di ordinamento. Questa clausola equivale a [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md). REBUILD abilita un indice disabilitato. La ricompilazione di un indice cluster non comporta la ricompilazione degli indici non cluster associati, a meno che non venga specificata la parola chiave ALL. Se non vengono specificate opzioni di indice, vengono applicati i valori esistenti delle opzioni di indice archiviati in [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md). Per le opzioni di indice il cui valore non è archiviato in **sys.indexes**, viene applicato il valore predefinito indicato nella definizione dell'argomento dell'opzione.  
  
 Se viene specificata la parola chiave ALL e la tabella sottostante è un heap, l'operazione di ricompilazione non ha effetto sulla tabella. Vengono ricompilati tutti gli indici non cluster associati alla tabella.  
  
 L'operazione di ricompilazione può essere sottoposta a una registrazione minima se viene utilizzato il modello di recupero del database con registrazione minima o con registrazione minima delle operazioni bulk.  
  
> [!NOTE]
> Quando si ricompila un indice XML primario, la tabella utente sottostante non è disponibile per tutta la durata dell'operazione sull'indice.  
 
Per gli indici columnstore, l'operazione di ricompilazione:  
  
-  Non usa l'ordinamento.  
-  Acquisisce un blocco esclusivo sulla tabella o partizione durante la ricompilazione.  I dati sono "offline" e non disponibili durante la ricompilazione, anche quando si usa NOLOCK, RCSI o SI.  
-  Ricomprime tutti i dati nel columnstore. Durante la ricompilazione esistono due copie dell'indice columnstore. Al termine della ricompilazione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina l'indice columnstore originale.  

Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
PARTITION  

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica che verrà ricompilata o riorganizzata solo una partizione di un indice. Non è possibile specificare PARTITION, se l'indice in *index_name* non è di tipo partizionato.  
  
 PARTITION = ALL consente di ricompilare tutte le partizioni.  
  
> [!WARNING]
> La creazione e la ricompilazione di indici non allineati per una tabella con oltre 1.000 partizioni sono possibili, ma non supportate. Questo tipo di operazioni può causare riduzioni delle prestazioni e un eccessivo consumo della memoria. Quando il numero di partizioni supera 1000, Microsoft consiglia di usare solo indici allineati.  
  
 *partition_number*  
   
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Numero di partizioni di un indice partizionato da ricompilare o riorganizzare. *partition_number* è un'espressione costante che può fare riferimento a variabili, incluse variabili o funzioni con tipo definito dall'utente (UDT) e funzioni definite dall'utente, ma non a istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. È necessario che *partition_number* esista o l'istruzione avrà esito negativo.  
  
 WITH **(** \<single_partition_rebuild_index_option> **)**  
   
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 `SORT_IN_TEMPDB`, `MAXDOP` e `DATA_COMPRESSION` sono le opzioni che è possibile specificare quando si ricompila una singola partizione (PARTITION = *numero_partizioni*). Gli indici XML non possono essere specificati in un'operazione di ricompilazione di una singola partizione.  
  
 DISABLE  
 Contrassegna l'indice come disabilitato e non disponibile per l'utilizzo nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. È possibile disabilitare qualsiasi indice. La definizione di un indice disabilitato rimane nel catalogo di sistema senza i dati dell'indice sottostante. La disabilitazione di un indice cluster impedisce all'utente di accedere ai dati della tabella sottostante. Per abilitare un indice, utilizzare ALTER INDEX REBUILD oppure CREATE INDEX WITH DROP_EXISTING. Per altre informazioni, vedere [Disabilitare indici e vincoli](../../relational-databases/indexes/disable-indexes-and-constraints.md) e [Abilitare indici e vincoli](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
 Operazione REORGANIZE per un indice **rowstore**  
 Per gli indici rowstore, REORGANIZE specifica la riorganizzazione del livello foglia dell'indice. L'operazione REORGANIZE:  
  
-   Viene eseguita sempre online. Ciò significa che i blocchi di tabella a lungo termine non vengono mantenuti attivi e le query o gli aggiornamenti inerenti la tabella sottostante possono continuare durante la transazione ALTER INDEX REORGANIZE.
-   Non è consentita per un indice disabilitato.
-   Non è consentita quando ALLOW_PAGE_LOCKS è impostato su OFF.
-   Non viene sottoposta a rollback quando viene eseguita all'interno di una transazione che è sottoposta a rollback.

> [!NOTE]
> Quando ALTER INDEX REORGANIZE usa transazioni esplicite (ad esempio ALTER INDEX all'interno di un'istruzione BEGIN TRAN ... COMMIT/ROLLBACK) invece della modalità di transazione implicita predefinita, il comportamento di blocco di REORGANIZE diventa più restrittivo, causando potenzialmente il blocco. Per altre informazioni sulle transazioni implicite, vedere [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md).

Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

REORGANIZE WITH **(** LOB_COMPACTION = { **ON** | OFF } **)**  
 Si applica agli indici rowstore.  
  
LOB_COMPACTION = ON  
  
-   Specifica di comprimere tutte le pagine che contengono i dati di questi tipi di dati LOB: image, text, ntext, varchar(max), nvarchar(max), varbinary(max) e xml. Con la compattazione di questi dati, è possibile ridurre le dimensioni dei dati su disco.  
-   Per un indice cluster, vengono compattate tutte le colonne LOB contenute nella tabella.  
-   Per un indice non cluster, vengono compattate tutte le colonne LOB che sono colonne non chiave (incluse) nell'indice.  
-   REORGANIZE ALL esegue LOB_COMPACTION in tutti gli indici. Per ogni indice, vengono compattate tutte le colonne LOB nell'indice cluster, nella tabella sottostante o le colonne incluse in un indice non cluster.  
  
LOB_COMPACTION = OFF  
  
-   Le pagine contenenti dati LOB non vengono compattate.  
-   OFF non ha alcun effetto su un heap.  
  
 Operazione REORGANIZE per un indice **columnstore**  
 Per gli indici columnstore, REORGANIZE comprime tutti i rowgroup differenziali CLOSED nel columnstore come rowgroup compresso. L'operazione REORGANIZE viene sempre eseguita online. Ciò significa che i blocchi di tabella a lungo termine non vengono mantenuti attivi e le query o gli aggiornamenti inerenti la tabella sottostante possono continuare durante la transazione ALTER INDEX REORGANIZE. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
-   Non è necessario eseguire l'operazione REORGANIZE per spostare i rowgroup differenziali CLOSED in rowgroup compressi. Il processo del motore di tuple in background viene attivato periodicamente per comprimere i rowgroup differenziali CLOSED. È consigliabile usare REORGANIZE, quando il motore di tuple è in ritardo. Con REORGANIZE è possibile comprimere i rowgroup in modo più aggressivo.  
-   Per comprimere tutti i rowgroup OPEN e CLOSED, vedere l'opzione `REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS)` in questa sezione.  
  
Per gli indici columnstore in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) e nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], l'operazione REORGANIZE esegue online le ottimizzazioni di deframmentazione aggiuntive seguenti:  
  
-   Rimuove fisicamente le righe da un rowgroup quando più del 10% delle righe è stato eliminato in modo logico. I byte eliminati vengono recuperati sui supporti fisici. Ad esempio, se in un rowgroup compresso di 1 milione di righe 100.000 righe sono state eliminate, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimuoverà le righe eliminate e ricomprimerà il rowgroup con 900.000 righe. Esegue il salvataggio nell'archiviazione rimuovendo le righe eliminate.  
  
-   Combina uno o più rowgroup compressi per aumentare le righe in ogni rowgroup, fino a un massimo di 1.048.576 righe. Ad esempio, se si importano globalmente 5 batch di 102.400 righe si otterranno 5 rowgroup compressi. Se si esegue l'operazione REORGANIZE, questi rowgroup verranno uniti in un rowgroup compresso di 512.000 righe. Si presuppone che non vi siano limiti di memoria o di dimensioni del dizionario.  
  
-   Per i rowgroup in cui almeno il 10% delle righe è stato eliminato in modo logico, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenterà di combinare questo rowgroup con uno o più rowgroup. Ad esempio, il rowgroup 1 viene compresso con 500.000 righe e il rowgroup 21 viene compresso con il numero massimo di 1.048.576 righe. Nel rowgroup 21 il 60% delle righe è stato eliminato in modo da lasciare 409.830 righe. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di combinare questi due rowgroup per comprimere un nuovo rowgroup con 909.830 righe.  
  
REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = { ON | **OFF** } )  
 Si applica agli indici columnstore. 

 **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

COMPRESS_ALL_ROW_GROUPS consente di forzare i rowgroup differenziali OPEN o CLOSED nel columnstore. Con questa opzione, non è necessario ricompilare l'indice columnstore per svuotare i rowgroup differenziali.  Grazie a questa opzione e ad altre funzionalità di deframmentazione per la rimozione e l'unione, nella maggior parte dei casi non è più necessario ricompilare l'indice.    

-   L'opzione ON forza tutti i rowgroup nel columnstore, indipendentemente dalle dimensioni e dallo stato (CLOSED o OPEN).  
-   L'opzione OFF forza tutti i rowgroup nel columnstore.  

Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

SET **(** \<set_index option> [ **,** ... *n*] **)**  
 Specifica alcune opzioni per l'indice senza ricompilare né riorganizzare l'indice. La parola chiave SET non può essere specificata per un indice disabilitato.  
  
PAD_INDEX = { ON | OFF }  
   
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

 Specifica il riempimento dell'indice. Il valore predefinito è OFF.  
  
 ON  
 La percentuale di spazio disponibile specificata in FILLFACTOR viene applicata alle pagine di livello intermedio dell'indice. Se l'opzione FILLFACTOR non viene specificata e l'opzione PAD_INDEX è impostata su ON, viene usato il valore del fattore di riempimento archiviato in [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 OFF o *fillfactor* non è specificato  
 Le pagine di livello intermedio vengono riempite poco al di sotto della capacità massima. Lo spazio residuo è sufficiente per almeno una riga della dimensione massima supportata dall'indice, in base al set di chiavi nelle pagine intermedie.  
  
 Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
FILLFACTOR = *fillfactor*  
 
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Specifica una percentuale indicante il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la modifica dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Il valore predefinito è 0. I valori 0 e 100 relativi al fattore di riempimento sono equivalenti.  
  
 Un'impostazione esplicita dell'opzione FILLFACTOR viene applicata solo in fase di creazione o ricompilazione dell'indice. La percentuale specificata di spazio vuoto delle pagine non viene mantenuta in modo dinamico da [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 Per visualizzare l'impostazione del fattore di riempimento, usare **sys.indexes**.  
  
> [!IMPORTANT]
> La creazione o la modifica di un indice cluster con un valore FILLFACTOR influisce sulla quantità di spazio di archiviazione occupata dai dati, perché i dati vengono ridistribuiti dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione dell'indice cluster.  
  
 SORT_IN_TEMPDB = { ON | **OFF** }  
 
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica se i risultati dell'ordinamento devono essere archiviati in **tempdb**. Il valore predefinito è OFF, tranne per il database SQL di Azure con servizio Hyperscale. Per tutte le operazioni di ricompilazione dell'indice in Hyperscale, SORT_IN_TEMPDB è sempre ON, indipendentemente dall'opzione specificata, a meno che non venga usata la ricompilazione dell'indice ripristinabile.  
  
 ON  
 I risultati intermedi dell'ordinamento usati per la compilazione dell'indice vengono archiviati in **tempdb**. Se **tempdb** si trova in un set di dischi diverso rispetto al database utente, il tempo necessario per creare un indice potrebbe essere minore. La quantità di spazio su disco utilizzata durante la compilazione dell'indice sarà tuttavia maggiore.  
  
 OFF  
 I risultati intermedi dell'ordinamento vengono archiviati nello stesso database dell'indice.  
  
 Se non è necessario eseguire un'operazione di ordinamento oppure se l'ordinamento può essere eseguito in memoria, l'opzione SORT_IN_TEMPDB viene ignorata.  
  
 Per altre informazioni, vedere [Opzione SORT_IN_TEMPDB per gli indici](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=** { ON | OFF }  
 Specifica l'errore restituito quando un'operazione di inserimento tenta di inserire valori di chiave duplicati in un indice univoco. L'opzione IGNORE_DUP_KEY viene applicata solo alle operazioni di inserimento eseguite dopo la creazione o la ricompilazione dell'indice. Il valore predefinito è OFF.  
  
 ON  
 Viene visualizzato un messaggio di avviso quando i valori di chiave duplicati vengono inseriti in un indice univoco. Avranno esito negativo solo le righe che violano il vincolo di unicità.  
  
 OFF  
 Viene visualizzato un messaggio di errore quando i valori di chiave duplicati vengono inseriti in un indice univoco. Viene eseguito il rollback dell'intera operazione INSERT.  
  
 L'opzione IGNORE_DUP_KEY non può essere impostata su ON per gli indici creati in una vista, negli indici non univoci, negli indici XML, spaziali e filtrati.  
  
 Per visualizzare IGNORE_DUP_KEY, usare [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH IGNORE_DUP_KEY equivale a WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | OFF }  
 Specifica se le statistiche di distribuzione vengono ricalcolate. Il valore predefinito è OFF.  
  
 ON  
 Le statistiche non aggiornate non vengono ricalcolate automaticamente.  
  
 OFF  
 Abilita l'aggiornamento automatico delle statistiche.  
  
 Per ripristinare l'aggiornamento automatico delle statistiche, impostare l'opzione STATISTICS_NORECOMPUTE su OFF oppure eseguire UPDATE STATISTICS senza la clausola NORECOMPUTE.  
  
> [!IMPORTANT]
> La disabilitazione del ricalcolo automatico delle statistiche di distribuzione può compromettere la selezione di piani di esecuzione ottimali per le query riguardanti la tabella in Query Optimizer.  
  
STATISTICS_INCREMENTAL = { ON | **OFF** }  

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Se è specificato **ON**, le statistiche create sono statistiche per partizione. Se è specificato **OFF**, l'albero delle statistiche viene eliminato e le statistiche vengono ricalcolate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore predefinito è **OFF**.  
  
 Se le statistiche per partizione non sono supportate, l'opzione viene ignorata e viene generato un avviso. Le statistiche incrementali non sono supportate per i seguenti tipi di statistiche:  
  
-   Statistiche create con indici il cui partizionamento non è allineato con la tabella di base  
-   Statistiche create per i database secondari leggibili Always On  
-   Statistiche create per i database di sola lettura  
-   Statistiche create per gli indici filtrati  
-   Statistiche create per le viste  
-   Statistiche create per le tabelle interne  
-   Statistiche create con indici spaziali o indici XML  
  
 ONLINE **=** { ON | **OFF** } \<as applies to rebuild_index_option>  
 Specifica se le tabelle sottostanti e gli indici associati sono disponibili per le query e la modifica dei dati durante l'operazione sugli indici. Il valore predefinito è OFF.  
  
 Per un indice XML o spaziale, è supportata solo l'opzione `ONLINE = OFF` e se ONLINE è impostata su ON viene generato un errore.  
  
> [!IMPORTANT]
> Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]](../../sql-server/editions-and-supported-features-for-sql-server-2016.md) e [Edizioni e funzionalità supportate per SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
 ON  
 I blocchi di tabella a lungo termine non vengono mantenuti per la durata dell'operazione sugli indici. Durante la fase principale dell'operazione viene mantenuto solo un blocco preventivo condiviso (IS, Intent Shared) sulla tabella di origine. In questo modo, le query o gli aggiornamenti relativi alla tabella e agli indici sottostanti possono continuare. All'inizio dell'operazione viene mantenuto brevemente un blocco condiviso (S) sull'oggetto di origine. Al termine dell'operazione, se è in corso la creazione di un indice non cluster, viene mantenuto un blocco S sull'origine per un periodo di tempo molto breve. Se è in corso la creazione o l'eliminazione online di un indice cluster o la ricompilazione di un indice cluster o non cluster, viene acquisito un blocco di modifica dello schema (SCH-M). L'opzione ONLINE non può essere impostata su ON quando viene creato un indice per una tabella temporanea locale.  
  
 OFF  
 I blocchi di tabella vengono applicati per la durata dell'operazione sugli indici. Un'operazione sull'indice offline che crea, ricompila o elimina un indice cluster, spaziale o XML oppure che ricompila o elimina un indice non cluster, acquisisce un blocco di modifica dello schema (SCH-M) sulla tabella. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione. Un'operazione sugli indici offline che crea un indice non cluster acquisisce un blocco condiviso (S) sulla tabella. Tale blocco impedisce l'aggiornamento della tabella sottostante ma consente operazioni di lettura, ad esempio l'esecuzione di istruzioni SELECT.  
  
Per altre informazioni, vedere [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
È possibile ricompilare online tutti gli indici, inclusi quelli di tabelle temporanee globali, ad eccezione dei casi seguenti:
- Indice XML
- Indice di una tabella temporanea locale
- Indice cluster univoco iniziale su una vista
- Indici columnstore
- Indice cluster, se la tabella sottostante contiene tipi di dati LOB (**image**, **ntext**, **text**) e tipi spaziali
- Le colonne **varchar(max)** e **varbinary(max)** non possono fare parte di un indice. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], quando una tabella contiene le colonne **varchar(max)** o **varbinary(max)** , è possibile compilare o ricompilare un indice cluster contenente altre colonne usando l'opzione **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non consente l'opzione **ONLINE** quando la tabella di base contiene le colonne **varchar(max)** o **varbinary(max)**

Per altre informazioni, vedere [Funzionamento delle operazioni sugli indici online](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 Specifica se un'operazione sull'indice online è ripristinabile.

 L'operazione sull'indice ON è ripristinabile.

 L'operazione sull'indice OFF non è ripristinabile.

MAX_DURATION **=** *time* [**MINUTES**] usato con **RESUMABLE = ON** (richiede **ONLINE = ON**).

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Indica il tempo (un valore intero specificato in minuti) di esecuzione di un'operazione sull'indice online ripristinabile prima di essere sospesa. 

> [!IMPORTANT]
> Per informazioni più dettagliate sulle operazioni di indice eseguibili online, vedere [Linee guida per operazioni di indice online](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Negli indici columnstore non sono supportate le ricompilazioni degli indici online ripristinabili.

ALLOW_ROW_LOCKS **=** { **ON** | OFF }  

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.  
  
 ON  
 I blocchi di riga sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando usare blocchi di riga.  
  
 OFF  
 I blocchi di riga non vengono utilizzati.  
  
ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Specifica se sono consentiti blocchi a livello di pagina. Il valore predefinito è ON.  
  
 ON  
 I blocchi a livello di pagina sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando utilizzare blocchi a livello di pagina.  
  
 OFF  
 I blocchi a livello di pagina non vengono utilizzati.  
  
> [!NOTE]
> Quando l'opzione ALLOW_PAGE_LOCKS è impostata su OFF, non è possibile eseguire operazioni di riorganizzazione degli indici.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica se eseguire o meno l'ottimizzazione per la contesa di inserimento dell'ultima pagina. Il valore predefinito è OFF. Per altre informazioni, vedere le sezione [Chiavi sequenziali](./create-index-transact-sql.md#sequential-keys) della pagina CREATE INDEX.

 MAXDOP **=** max_degree_of_parallelism  
 
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Sostituisce l'opzione di configurazione **max degree of parallelism** per la durata dell'operazione sull'indice. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
> [!IMPORTANT]
>  Sebbene l'opzione MAXDOP sia supportata a livello di sintassi per tutti gli indici XML, per un indice XML primario o spaziale ALTER INDEX utilizza attualmente solo un processore singolo.  
  
 *max_degree_of_parallelism* può essere:  
  
 1  
 Disattiva la generazione di piani paralleli.  
  
 \>1  
 Limita il numero massimo di processori utilizzati in un'operazione parallela sull'indice in base al numero specificato.  
  
 0 (predefinito)  
 Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
 Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Le operazioni sugli indici parallele sono disponibili solo in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
COMPRESSION_DELAY **=** { **0** |*duration [Minutes]* }  

**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])  
  
 Per una tabella basata su disco, il ritardo indica il numero minimo di minuti in cui un rowgroup differenziale deve rimanere nello stato CLOSED nel rowgroup differenziale prima che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo comprima nel rowgroup compresso. Poiché le tabelle basate su disco non tengono traccia delle ore di inserimento e aggiornamento per le singole righe, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applica il ritardo ai rowgroup differenziali nello stato CLOSED.  
  
 Il valore predefinito è 0 minuti.  
  
 Per indicazioni su quando usare COMPRESSION_DELAY, vedere l'[introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
 DATA_COMPRESSION  
 Specifica l'opzione di compressione dei dati per l'indice, il numero di partizione o l'intervallo di partizioni specificato. descritte di seguito:  
  
 NONE  
 L'indice o le partizioni specificate non vengono compressi. Non si applica agli indici columnstore.  
  
 ROW  
 L'indice o le partizioni specificate vengono compressi utilizzando la compressione di riga. Non si applica agli indici columnstore.  
  
 PAGE  
 L'indice o le partizioni specificate vengono compressi utilizzando la compressione di pagina. Non si applica agli indici columnstore.  
  
 COLUMNSTORE  
   
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Si applica solo agli indici columnstore, inclusi gli indici columnstore cluster e quelli non cluster. COLUMNSTORE specifica di decomprimere l'indice o le partizioni specificate compresse con l'opzione COLUMNSTORE_ARCHIVE. Quando i dati vengono ripristinati, continueranno a essere compressi con la compressione columnstore utilizzata per tutti gli indici columnstore.  
  
 COLUMNSTORE_ARCHIVE  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Si applica solo agli indici columnstore, inclusi gli indici columnstore cluster e quelli non cluster. COLUMNSTORE_ARCHIVE comprimerà ulteriormente la partizione specificata a una dimensione inferiore. Può essere utilizzata per l'archiviazione o in altre situazioni in cui sono richieste dimensioni di archiviazione inferiori ed è possibile concedere più tempo per l'archiviazione e il recupero.  
  
 Per altre informazioni sulla compressione, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
 ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...n] **)**  
    
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Specifica le partizioni alle quali si applica l'impostazione DATA_COMPRESSION. Se l'indice non è partizionato, l'argomento ON PARTITIONS genererà un errore. Se la clausola ON PARTITIONS non viene fornita, l'opzione DATA_COMPRESSION si applica a tutte le partizioni di un indice partizionato.  
  
 \<partition_number_expression> può essere specificato nei modi seguenti:  
  
-   Specificare il numero di una partizione, ad esempio: `ON PARTITIONS (2)`.  
-   Specificare i numeri di partizione per più partizioni singole separati da virgole, ad esempio: `ON PARTITIONS (1, 5)`.  
-   Specificare sia intervalli che partizioni singole: `ON PARTITIONS (2, 4, 6 TO 8)`.  
  
 È possibile specificare \<range> sotto forma di numeri di partizione separati dalla parola TO, ad esempio: `ON PARTITIONS (6 TO 8)`.  
  
 Per impostare tipi diversi di compressione dei dati per partizioni diverse, specificare più volte l'opzione DATA_COMPRESSION, ad esempio:  
  
```sql  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
);  
```  
  
 ONLINE **=** { ON  | **OFF** } \<as applies to single_partition_rebuild_index_option>  
 Specifica se un indice o una partizione di indice di una tabella sottostante può essere ricompilata online o offline. Se **REBUILD** viene eseguito online (**ON**), i dati nella tabella sono disponibili per le query e le modifiche durante l'operazione sull'indice.  Il valore predefinito è **OFF**.  
  
 ON  
 I blocchi di tabella a lungo termine non vengono mantenuti per la durata dell'operazione sugli indici. Durante la fase principale dell'operazione viene mantenuto solo un blocco preventivo condiviso (IS, Intent Shared) sulla tabella di origine. È necessario un blocco S sulla tabella all'inizio della ricompilazione dell'indice e un blocco Sch-M sulla tabella alla fine della ricompilazione dell'indice online. Sebbene entrambi i blocchi siano blocchi di metadati brevi, soprattutto il blocco Sch-M deve attendere il completamento di tutte le transazioni bloccanti. Durante il tempo di attesa il blocco Sch-M impedisce tutte le altre transazioni in attesa dietro il blocco stesso per l'accesso alla stessa tabella.  
  
> [!NOTE]
>  Con la ricompilazione dell'indice online è possibile impostare le opzioni *low_priority_lock_wait* descritte più avanti in questa sezione.  
  
 OFF  
 I blocchi di tabella vengono applicati per la durata dell'operazione sugli indici. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione.  
  
 WAIT_AT_LOW_PRIORITY usato con **ONLINE=ON** only.  
 
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Per una ricompilazione di indice online è necessario attendere il blocco delle operazioni su questa tabella. **WAIT_AT_LOW_PRIORITY** indica che l'operazione di ricompilazione dell'indice online rimarrà in attesa di blocchi a bassa priorità, consentendo alle altre operazioni di proseguire mentre la compilazione dell'indice online è in attesa. L'omissione dell'opzione **WAIT AT LOW PRIORITY** equivale a WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minuti, ABORT_AFTER_WAIT = NONE). Per altre informazioni, vedere [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 
  
 MAX_DURATION = *time* [**MINUTES**]  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Il tempo (valore intero specificato in minuti) di attesa con priorità bassa dei blocchi di ricompilazione di indice online durante l'esecuzione del comando DDL. Se l'operazione viene bloccata per il tempo specificato in **MAX_DURATION**, una delle azioni **ABORT_AFTER_WAIT** verrà eseguita. Il tempo **MAX_DURATION** è sempre espresso in minuti e la parola **MINUTES** può essere omessa.  
 
 ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
   
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 NONE  
 Continuare ad attendere il blocco con priorità normale (regolare).  
  
 SELF  
 Esce dall'operazione DDL di ricompilazione dell'indice online attualmente in esecuzione senza eseguire alcuna azione.  
  
 BLOCKERS  
 Termina tutte le transazioni utente che bloccano l'operazione DDL di ricompilazione dell'indice online in modo da poter continuare l'operazione. Per l'opzione **BLOCKERS** è necessario l'account di accesso per avere l'autorizzazione **ALTER ANY CONNECTION**.  
 
 RESUME 
 
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

Riprende un'operazione sull'indice che è stata sospesa manualmente o a causa di un errore.

MAX_DURATION usato con **RESUMABLE=ON**
 
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Tempo (un valore intero espresso in minuti) di esecuzione di un'operazione sull'indice online ripristinabile dopo il ripristino. Trascorso questo tempo, l'operazione ripristinabile viene sospesa se è ancora in esecuzione.

WAIT_AT_LOW_PRIORITY usato con **RESUMABLE=ON** e **ONLINE = ON**.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Per una ricompilazione di indice online dopo una sospensione è necessario attendere il blocco delle operazioni su questa tabella. **WAIT_AT_LOW_PRIORITY** indica che l'operazione di ricompilazione dell'indice online rimarrà in attesa di blocchi a bassa priorità, consentendo alle altre operazioni di proseguire mentre la compilazione dell'indice online è in attesa. L'omissione dell'opzione **WAIT AT LOW PRIORITY** equivale a WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minuti, ABORT_AFTER_WAIT = NONE). Per altre informazioni, vedere [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 

PAUSE
 
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
Sospende un'operazione di ricompilazione dell'indice online ripristinabile.

ABORT

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

Interrompe un'operazione sull'indice in esecuzione o sospesa, dichiarata ripristinabile. È necessario eseguire in modo esplicito un comando **ABORT** per terminare un'operazione di ricompilazione dell'indice ripristinabile. L'errore o la sospensione di un'operazione sull'indice ripristinabile non termina l'esecuzione. Lascia invece l'operazione in uno stato di sospensione indefinito.
  
## <a name="remarks"></a>Osservazioni  
L'istruzione ALTER INDEX non può essere utilizzata per ripartizionare un indice o spostarlo in un filegroup diverso né per modificare la definizione dell'indice, ad esempio per aggiungere o eliminare colonne oppure per modificarne l'ordine. Per eseguire queste operazioni, utilizzare CREATE INDEX con la clausola DROP_EXISTING.  
  
Quando un'opzione non viene specificata in modo esplicito, viene applicata l'impostazione corrente. Se, ad esempio, non viene specificata un'impostazione per FILLFACTOR nella clausola REBUILD, verrà utilizzato il valore del fattore di riempimento archiviato nel catalogo di sistema durante il processo di ricompilazione. Per visualizzare le impostazioni correnti delle opzioni per gli indici, usare [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
I valori per `ONLINE`, `MAXDOP` e `SORT_IN_TEMPDB` non vengono archiviati nel catalogo di sistema. Se non viene specificato un valore nell'istruzione dell'indice, viene utilizzato il valore predefinito dell'opzione.
  
Nei computer multiprocessore l'istruzione `ALTER INDEX REBUILD` usa automaticamente più processori per eseguire le operazioni di analisi e ordinamento associate alla modifica dell'indice, in modo identico ad altre query. Quando si esegue `ALTER INDEX REORGANIZE`, con o senza LOB_COMPACTION, il valore di **max degree of parallelism** corrisponde a un'operazione a thread singolo. Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]
> Non è possibile riorganizzare o ricompilare indici contenuti in un filegroup offline o di sola lettura. Quando viene specificata la parola chiave ALL e uno o più indici si trovano in un filegroup offline o di sola lettura, l'istruzione ha esito negativo.  
  
## <a name="rebuilding-indexes"></a><a name="rebuilding-indexes"></a> Ricompilazione degli indici  
La ricompilazione di un indice consiste nell'eliminazione e nella ricreazione dell'indice. Questa operazione consente di rimuovere la frammentazione, rendere disponibile spazio su disco grazie alla compattazione delle pagine in base all'impostazione del fattore di riempimento esistente o specificata e riordinare le righe dell'indice in pagine contigue. Quando viene specificata la parola chiave ALL, tutti gli indici della tabella vengono eliminati e ricompilati in una singola transazione. Non è necessario eliminare in anticipo i vincoli di chiave esterna. Quando vengono ricompilati indici con un numero di extent pari o superiore a 128, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] posticipa le effettive deallocazioni delle pagine e i blocchi associati fino al termine del commit della transazione.  
 
Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
## <a name="reorganizing-indexes"></a><a name="reorganizing-indexes"></a> Riorganizzazione degli indici
La riorganizzazione di un indice richiede una quantità minima di risorse di sistema. Questa operazione deframmenta il livello foglia di indici cluster e non cluster di tabelle e viste tramite il riordinamento fisico delle pagine al livello foglia in base all'ordine logico, da sinistra verso destra, dei nodi foglia. La riorganizzazione consente inoltre di compattare le pagine di indice in base al valore del fattore di riempimento esistente. 
  
Quando viene specificato `ALL`, vengono riorganizzati gli indici relazionali, sia cluster sia non cluster, e gli indici XML della tabella. Quando si specifica ALL, vengono applicate alcune restrizioni. Vedere la definizione di ALL nella sezione Argomenti di questo articolo.  
  
Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  

> [!IMPORTANT]
> Per una tabella [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] con un indice columnstore cluster ordinato, `ALTER INDEX REORGANIZE` non riordina i dati. Per riordinare i dati, usare `ALTER INDEX REBUILD`.
  
## <a name="disabling-indexes"></a><a name="disabling-indexes"></a> Disabilitazione degli indici  
La disabilitazione di un indice impedisce agli utenti di accedere all'indice e, nel caso di indici cluster, ai dati della tabella sottostante. La definizione dell'indice rimane archiviata nel catalogo di sistema. La disabilitazione di un indice non cluster o di un indice cluster di una vista elimina fisicamente i dati dell'indice. La disabilitazione di un indice cluster impedisce l'accesso ai dati, i quali tuttavia rimangono archiviati in forma non gestita nell'albero B fino all'eliminazione o alla ricompilazione dell'indice. Per visualizzare lo stato di un indice abilitato o disabilitato, eseguire una query sulla colonna **is_disabled** della vista del catalogo **sys.indexes**.  
  
Se una tabella è inclusa in una pubblicazione per la replica transazionale, non è possibile disabilitare gli indici associati a colonne chiave primaria. Questi indici sono necessari per la replica. Per disabilitare un indice, è innanzitutto necessario eliminare la tabella dalla pubblicazione. Per altre informazioni, vedere [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
Per abilitare l'indice, usare l'istruzione `ALTER INDEX REBUILD` o `CREATE INDEX WITH DROP_EXISTING`. Quando l'opzione ONLINE è impostata su ON, non è possibile ricompilare un indice cluster disabilitato. Per altre informazioni, vedere [Disabilitazione di indici e vincoli](../../relational-databases/indexes/disable-indexes-and-constraints.md).  
  
## <a name="setting-options"></a>Impostazione delle opzioni  
È possibile impostare le opzioni `ALLOW_ROW_LOCKS`, `ALLOW_PAGE_LOCKS`, `OPTIMIZE_FOR_SEQUENTIAL_KEY`, `IGNORE_DUP_KEY` e `STATISTICS_NORECOMPUTE` per un indice specificato senza ricompilare o riorganizzare l'indice. I valori modificati vengono applicati immediatamente all'indice. Per visualizzare tali impostazioni, usare **sys.indexes**. Per altre informazioni vedere [Impostare le opzioni di indice](../../relational-databases/indexes/set-index-options.md).  
  
### <a name="row-and-page-locks-options"></a>Opzioni per blocchi di riga e di pagina  
Se `ALLOW_ROW_LOCKS = ON` e `ALLOW_PAGE_LOCK = ON`, sono consentiti blocchi a livello di riga, pagina e tabella quando si accede all'indice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] sceglie il blocco appropriato e può eseguire un'escalation del blocco da un blocco di riga o di pagina a un blocco di tabella.  
  
Se `ALLOW_ROW_LOCKS = OFF` e `ALLOW_PAGE_LOCK = OFF`, è consentito solo un blocco a livello di tabella quando si accede all'indice.  
  
Se si specifica la parola chiave ALL durante l'impostazione di opzioni per blocchi di riga o di pagina, le impostazioni vengono applicate a tutti gli indici. Se la tabella sottostante è un heap, le impostazioni vengono applicate nei modi seguenti:  
  
|Opzione|Dettagli|
|------|-------|
|ALLOW_ROW_LOCKS = ON o OFF|Viene applicata all'heap e a tutti gli indici non cluster associati.|  
|ALLOW_PAGE_LOCKS = ON|Viene applicata all'heap e a tutti gli indici non cluster associati.|  
|ALLOW_PAGE_LOCKS = OFF|Viene applicata agli indici non cluster. Ciò significa che negli indici non cluster non è consentito alcun blocco a livello di pagina. Nell'heap non sono consentiti solo i blocchi condivisi (S), di aggiornamento (U) ed esclusivi (X) per la pagina. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] può comunque acquisire un blocco preventivo a livello di pagina (IS, IU o IX) per scopi interni.|  
  
## <a name="online-index-operations"></a><a name="online-index-operations"></a> Operazioni sull'indice online  
Quando si ricompila un indice e l'opzione ONLINE è impostata su ON, gli oggetti sottostanti, ovvero le tabelle e gli indici associati, risultano disponibili per query e operazioni di modifica dei dati. È inoltre possibile ricompilare online una parte di indice che risiede in una singola partizione. I blocchi di tabella esclusivi vengono mantenuti attivi per un periodo di tempo molto limitato durante il processo di modifica.  
  
La riorganizzazione di un indice viene sempre eseguita online. Questo processo non mantiene attivi i blocchi a lungo termine e non blocca pertanto le query o gli aggiornamenti in corso.  
  
In una stessa tabella o partizione di tabella è possibile eseguire in modo simultaneo solo le operazioni sugli indici online seguenti:  
  
-   Creazione di più indici non cluster.  
-   Riorganizzazione di indici diversi della stessa tabella.  
-   Riorganizzazione di indici diversi durante la ricompilazione di indici non sovrapposti della stessa tabella.  
  
Qualsiasi altra operazione sugli indici online eseguita nello stesso istante avrà esito negativo. Non è ad esempio possibile ricompilare due o più indici della stessa tabella simultaneamente né creare un nuovo indice durante la ricompilazione di un indice esistente nella stessa tabella.  

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a>Operazioni sull'indice ripristinabili

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La ricompilazione dell'indice online viene specificata come ripristinabile usando l'opzione RESUMABLE=ON. 
-  L'opzione RESUMABLE non è persistente nei metadati per un determinato indice e si applica solo alla durata di un'istruzione DDL corrente. Per abilitare la funzione di ripristino, è necessario quindi che la clausola RESUMABLE=ON sia specificata in modo esplicito.
-  L'opzione MAX_DURATION è supportata per l'opzione RESUMABLE = ON o l'opzione dell'argomento **low_priority_lock_wait**. 
   -  MAX_DURATION per l'opzione RESUMABLE specifica l'intervallo di tempo per la ricompilazione di un indice. Trascorso questo tempo, la ricompilazione dell'indice viene sospesa oppure viene completata. È l'utente a decidere quando riprendere la ricompilazione di un indice che è stata sospesa. Il valore espresso in minuti in **time** per MAX_DURATION deve essere maggiore di 0 e minore o uguale a una settimana (7 \* 24 \* 60 = 10080 minuti). Una sospensione prolungata di un'operazione sull'indice può compromettere le prestazioni DML su una tabella specifica, nonché la capacità del disco del database poiché entrambi gli indici, quello originale e quello appena creato, richiedono spazio su disco e devono essere aggiornati durante le operazioni DML. Se l'opzione MAX_DURATION viene omessa, l'operazione sull'indice continuerà fino al suo completamento o fino a quando non si verificherà un errore. 
   -  L'opzione dell'argomento \<low_priority_lock_wait> consente di definire il comportamento dell'operazione sull'indice quando è bloccata sul blocco SCH-M.
 
-  Se si esegue nuovamente l'istruzione ALTER INDEX REBUILD originale con gli stessi parametri viene ripresa un'operazione di ricompilazione dell'indice sospesa. È possibile riprendere un'operazione di ricompilazione dell'indice sospesa anche eseguendo l'istruzione ALTER INDEX RESUME.
-  L'opzione SORT_IN_TEMPDB=ON non è supportata per l'indice ripristinabile 
-  Il comando DDL con RESUMABLE=ON non può essere eseguito all'interno di una transazione esplicita (non può far parte del blocco BEGIN TRAN …).
-  Solo le operazioni sugli indici sospese sono ripristinabili.
-  Quando si riprende un'operazione sull'indice che è stata sospesa, è possibile modificare il valore MAXDOP impostandone uno nuovo.  Se l'opzione MAXDOP non viene specificata quando si riprende un'operazione sull'indice sospesa, viene usato l'ultimo valore MAXDOP. Se l'opzione MAXDOP non è affatto specificata per un'operazione di ricompilazione dell'indice, viene usato il valore predefinito.
- Per sospendere immediatamente l'operazione sull'indice, è possibile arrestare il comando in corso (CTRL-C) oppure eseguire il comando ALTER INDEX PAUSE o il comando KILL di *session_id*. Quando il comando viene sospeso, è possibile riprenderlo usando l'opzione RESUME.
-  Il comando ABORT termina la sessione che ha ospitato la ricompilazione dell'indice originale e interrompe l'operazione sull'indice  
-  Non sono necessarie risorse aggiuntive per la ricompilazione dell'indice ripristinale ad eccezione di queste:
   -    Uno spazio aggiuntivo necessario per mantenere l'indice compilato, incluso il tempo di sospensione dell'indice
   -    Uno stato DDL per impedire eventuali modifiche DDL
-  La pulizia fantasma verrà eseguita durante la fase di sospensione dell'indice, ma sarà sospesa durante l'esecuzione dell'indice   
La funzionalità seguente è disabilitata per le operazioni di ricompilazione dell'indice ripristinabili
   -    La ricompilazione di un indice disabilitato non è supportata con RESUMABLE=ON
   -    Il comando ALTER INDEX REBUILD ALL
   -    ALTER TABLE con l'uso della ricompilazione dell'indice  
   -    Il comando DDL con "RESUMABLE=ON" non può essere eseguito all'interno di una transazione esplicita (non può far parte del blocco di commit BEGIN TRAN...)
   -    La ricompilazione di un indice contenente colonne calcolate o colonne TIMESTAMP come colonne chiave.
-   Nel caso in cui la tabella di base contenga colonne LOB, la ricompilazione dell'indice cluster ripristinabile richiede un blocco Sch-M all'inizio di questa operazione 

> [!NOTE]
> Il comando DDL viene eseguito fin tanto che è completato, sospeso o non riuscito. Nel caso in cui il comando sia sospeso, verrà generato un errore a indicare che l'operazione è stata sospesa e che la creazione dell'indice non è stata completata. Altre informazioni sullo stato corrente dell'indice sono disponibili in [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Come in precedenza, in caso di errore verrà generato un errore. 

 Per altre informazioni, vedere [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
 ### <a name="wait_at_low_priority-with-online-index-operations"></a>WAIT_AT_LOW_PRIORITY con operazioni sull'indice online  
  
 Per eseguire l'istruzione DDL per una ricompilazione dell'indice online, è necessario completare tutte le transazioni bloccanti attive in esecuzione in una specifica tabella. Quando la ricompilazione dell'indice online viene eseguita, blocca tutte le nuove transazioni pronte per l'esecuzione in questa tabella. Sebbene la durata del blocco della ricompilazione dell'indice online sia molto breve, l'attesa del completamento di tutte le transazioni aperte in una tabella specificata e il blocco dell'avvio di nuove transazioni potrebbero influire in modo significativo sulla velocità effettiva, provocando un rallentamento o un timeout del carico di lavoro e limitando notevolmente l'accesso alla tabella sottostante. L'opzione **WAIT_AT_LOW_PRIORITY** consente agli amministratori di database di gestire i blocchi S e Sch-M necessari per le ricompilazioni degli indici online, nonché di selezionare una tra 3 opzioni. In tutti e tre i casi, se durante il tempo di attesa, ( (MAX_DURATION = n [minuti]) ),non sono presenti attività di blocco, la ricompilazione dell'indice online viene eseguita immediatamente senza attendere il completamento dell'istruzione DDL.  
  
## <a name="spatial-index-restrictions"></a>Restrizioni relative agli indici spaziali  
 Quando si ricompila un indice spaziale, la tabella utente sottostante non è disponibile per tutta la durata dell'operazione sull'indice, in quanto l'indice spaziale acquisisce un blocco di schema.  
  
 Il vincolo PRIMARY KEY nella tabella utente non può essere modificato se in una colonna di tale tabella è definito un indice spaziale. Per modificare il vincolo PRIMARY KEY, eliminare innanzitutto ogni indice spaziale dalla tabella. Dopo avere modificato il vincolo PRIMARY KEY, è possibile ricreare ognuno degli indici spaziali.  
  
 In un'operazione di ricompilazione di una singola partizione, non è possibile specificare indici spaziali. È tuttavia possibile specificare indici spaziali in una ricompilazione di partizioni completa.  
  
 Per modificare opzioni specifiche di un indice spaziale, ad esempio BOUNDING_BOX o GRID, è possibile utilizzare un'istruzione CREATE SPATIAL INDEX che specifica DROP_EXISTING = ON oppure eliminare l'indice spaziale e crearne un nuovo. Per un esempio vedere [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
## <a name="data-compression"></a>Compressione dei dati  
 Per altre informazioni sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
 Per valutare il modo in cui la modifica della compressione di PAGE e ROW influirà su una tabella, un indice o una partizione, usare la stored procedure [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).  
  
Agli indici partizionati vengono applicate le restrizioni seguenti:  
  
-   Quando si utilizza ALTER INDEX ALL ... non è possibile modificare l'impostazione di compressione di una singola partizione se la tabella include indici non allineati.  
-   La sintassi ALTER INDEX \<index> ... La sintassi REBUILD PARTITION ricompila la partizione specificata dell'indice.  
-   La sintassi ALTER INDEX \<index> ... La sintassi REBUILD WITH ricompila tutte le partizioni dell'indice.  
  
## <a name="statistics"></a>Statistiche  
 Quando si esegue **ALTER INDEX ALL ...** su una tabella, vengono aggiornate solo le statistiche associate agli indici. Le statistiche automatiche o manuali create sulla tabella, anziché su un indice, non vengono aggiornate.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire l'istruzione ALTER INDEX, è necessario disporre almeno dell'autorizzazione ALTER per la tabella o la vista.  
  
## <a name="version-notes"></a>Note sulla versione  
  
-  Il [!INCLUDE[ssSDS](../../includes/sssds-md.md)] non usa le opzioni filegroup e FileStream.  
-  Gli indici columnstore non sono disponibili nelle versioni precedenti a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. 
-  Le operazioni sugli indici ripristinabili sono disponibili a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   
  
## <a name="basic-syntax-example"></a>Esempio della sintassi di base:   
  
```sql 
ALTER INDEX index1 ON table1 REBUILD;  
  
ALTER INDEX ALL ON table1 REBUILD;  
  
ALTER INDEX ALL ON dbo.table1 REBUILD;  
```

## <a name="examples-columnstore-indexes"></a>Esempi: Indici Columnstore  
 Questi esempio si applicano agli indici columnstore.  
  
### <a name="a-reorganize-demo"></a>R. Demo di REORGANIZE  
 In questo esempio viene illustrato il funzionamento del comando ALTER INDEX REORGANIZE.  Viene creata una tabella con più rowgroup e viene illustrata l'unione dei rowgroup tramite REORGANIZE.  
  
```sql  
-- Create a database   
CREATE DATABASE [ columnstore ];  
GO  
  
-- Create a rowstore staging table  
CREATE TABLE [ staging ] (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey     int  
     )  
  
-- Insert 10 million rows into the staging table.   
DECLARE @loop int  
DECLARE @AccountDescription varchar(50)  
DECLARE @AccountKey int  
DECLARE @AccountType varchar(50)  
DECLARE @AccountCode int  
  
SELECT @loop = 0  
BEGIN TRAN  
    WHILE (@loop < 300000)   
      BEGIN  
        SELECT @AccountKey = CAST (RAND()*10000000 as int);  
        SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountCode =  CAST (RAND()*10000000 as int);  
  
        INSERT INTO  staging VALUES (@AccountKey, @AccountDescription, @AccountType, @AccountCode);  
  
        SELECT @loop = @loop + 1;  
    END  
COMMIT  
  
-- Create a table for the clustered columnstore index  
  
CREATE TABLE cci_target (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey int  
     )  
  
-- Convert the table to a clustered columnstore index named inxcci_cci_target;  
CREATE CLUSTERED COLUMNSTORE INDEX idxcci_cci_target ON cci_target;  
```  
  
 Usare l'opzione TABLOCK per inserire righe in parallelo. A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], l'operazione INSERT INTO può essere eseguita in parallelo quando si usa TABLOCK.  
  
```sql  
INSERT INTO cci_target WITH (TABLOCK) 
SELECT TOP 300000 * FROM staging;  
```  
  
 Eseguire questo comando per visualizzare i rowgroup differenziali OPEN. Il numero di rowgroup dipende dal grado di parallelismo.  
  
```sql  
SELECT *   
FROM sys.dm_db_column_store_row_group_physical_stats   
WHERE object_id  = object_id('cci_target');  
```  
  
 Eseguire questo comando per forzare tutti i rowgroup OPEN e CLOSED nel columnstore.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
 Eseguire nuovamente il comando per visualizzare i rowgroup più piccoli uniti in un rowgroup compresso.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="b-compress-closed-delta-rowgroups-into-the-columnstore"></a>B. Comprimere i rowgroup differenziali CLOSED nel columnstore  
 In questo esempio l'opzione REORGANIZE viene usata per comprimere tutti i rowgroup differenziali CLOSED nel columnstore come rowgroup compresso.   Questa opzione non è necessaria ma può essere utile quando il motore di tuple non comprime i rowgroup in modo sufficientemente rapido.  
  
```sql  
-- Uses AdventureWorksDW  
-- REORGANIZE all partitions  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
  
-- REORGANIZE a specific partition  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0;  
```  
  
### <a name="c-compress-all-open-and-closed-delta-rowgroups-into-the-columnstore"></a>C. Comprimere tutti i rowgroup differenziali OPEN e CLOSED nel columnstore  
 **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Il comando REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = ON ) comprime tutti i rowgroup differenziali OPEN e CLOSED nel columnstore come rowgroup compresso. In questo modo i deltastore vengono svuotati e tutte le righe vengono forzate per essere compresse nel columnstore. Si tratta di un'operazione utile soprattutto dopo aver eseguito numerose operazioni di inserimento, in quanto tali operazioni archiviano le righe in uno o più rowgroup delta.  
  
 L'operazione REORGANIZE combina i rowgroup per ottenere rowgroup contenenti fino a un numero massimo di righe \<= 1.024.576. Quando si comprimono tutti i rowgroup OPEN e CLOSED, non si ottengono pertanto tanti rowgroup compressi che contengono solo poche righe. I rowgroup devono essere il più possibile completi in modo da ridurre le dimensioni compresse e migliorare le prestazioni delle query.  
  
```sql  
-- Uses AdventureWorksDW2016  
-- Move all OPEN and CLOSED delta rowgroups into the columnstore.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
-- For a specific partition, move all OPEN AND CLOSED delta rowgroups into the columnstore  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0 WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="d-defragment-a-columnstore-index-online"></a>D. Deframmentare un indice columnstore online  
 Non si applica a: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], REORGANIZE non solo comprime i rowgroup differenziali nel columnstore, ma esegue anche la deframmentazione online. In primo luogo, riduce le dimensioni del columnstore rimuovendo fisicamente le righe quando oltre il 10% delle righe in un rowgroup è stato eliminato.  Poi combina i rowgroup per formare rowgroup di dimensioni più grandi che contengono fino a 1.024.576 righe per rowgroup.  Tutti i rowgroup modificati vengono nuovamente compressi.  
  
> [!NOTE]
> A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la ricompilazione di un indice columnstore non è più necessaria nella maggior parte dei casi perché REORGANIZE rimuove fisicamente le righe eliminate e unisce i rowgroup. L'opzione COMPRESS_ALL_ROW_GROUPS forza tutti i rowgroup differenziali OPEN o CLOSED nel columnstore. Questo risultato era ottenibile prima solo con una ricompilazione. L'operazione REORGANIZE viene eseguita online e in background in modo che le query non siano interrotte quando l'operazione è in esecuzione.  
  
```sql  
-- Uses AdventureWorks  
-- Defragment by physically removing rows that have been logically deleted from the table, and merging rowgroups.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
```  
  
### <a name="e-rebuild-a-clustered-columnstore-index-offline"></a>E. Ricompilare un indice columnstore cluster offline  
Si applica a: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])   
  
> [!TIP]
> A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è consigliabile usare ALTER INDEX REORGANIZE anziché l'istruzione ALTER INDEX REBUILD.  
  
> [!NOTE]
> In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] l'operazione REORGANIZE viene usata solo per comprimere i rowgroup CLOSED nel columnstore. L'unico modo per eseguire operazioni di deframmentazione e forzare tutti i rowgroup differenziali nel columnstore consiste nella ricompilazione dell'indice.  
  
 In questo esempio viene illustrato come ricompilare un indice columnstore cluster e forzare tutti i rowgroup differenziali nel columnstore. Il primo passaggio consiste nel preparare una tabella FactInternetSales2 con un indice columnstore cluster e nell'inserimento di dati dalle prime quattro colonne.  
  
```sql  
-- Uses AdventureWorksDW  
  
CREATE TABLE dbo.FactInternetSales2 (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_FactInternetSales2  
ON dbo.FactInternetSales2;  
  
INSERT INTO dbo.FactInternetSales2  
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey  
FROM dbo.FactInternetSales;  
  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Si ottiene un rowgroup OPEN, il che significa che in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si attenderà che vengano aggiunte altre righe prima che il rowgroup venga chiuso e i dati vengano spostati nel columnstore. L'istruzione successiva ricompila l'indice columnstore cluster, il quale forza tutte le righe nel columnstore.  
  
```sql  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REBUILD;  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 I risultati dell'istruzione SELECT mostrano che il rowgroup è COMPRESSED, il che significa che i segmenti di colonna del rowgroup vengono compressi e archiviati nel columnstore.  
  
### <a name="f-rebuild-a-partition-of-a-clustered-columnstore-index-offline"></a>F. Ricompilare una partizione di un indice columnstore cluster offline  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  
 
 Usare ALTER INDEX REBUILD con l'opzione partizione per ricompilare una partizione di un indice columnstore cluster di grandi dimensioni. In questo esempio viene ricompilata la partizione 12. A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], è consigliabile sostituire REBUILD con REORGANIZE.  
  
```sql  
ALTER INDEX cci_fact3   
ON fact3  
REBUILD PARTITION = 12;  
```  
  
### <a name="g-change-a-clustered-columstore-index-to-use-archival-compression"></a>G. Modificare un indice columstore cluster per usare la compressione dell'archivio  
 Non si applica a: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 È possibile scegliere di ridurre le dimensioni di un indice columnstore cluster ulteriormente tramite l'opzione di compressione dati COLUMNSTORE_ARCHIVE. Si tratta di una procedura utile per i dati meno recenti che si vuole mantenere usando un'archiviazione più economica. È consigliabile usare questo approccio solo per i dati a quali non si accede spesso poiché l'operazione di decompressione risulta più lenta rispetto alla normale compressione COLUMNSTORE.  
  
 Nell'esempio seguente viene ricompilato un indice columnstore cluster per l'utilizzo della compressione dell'archivio e viene illustrato come rimuovere tale compressione. Il risultato finale consisterà nell'utilizzo della sola compressione columnstore.  
  
```sql  
--Prepare the example by creating a table with a clustered columnstore index.  
CREATE TABLE SimpleTable (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL  
);  
  
CREATE CLUSTERED INDEX cci_SimpleTable ON SimpleTable (ProductKey);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_SimpleTable  
ON SimpleTable  
WITH (DROP_EXISTING = ON);  
  
--Compress the table further by using archival compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE);  
  
--Remove the archive compression and only use columnstore compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE);  
GO  
```  
  
## <a name="examples-rowstore-indexes"></a>Esempi: Indici rowstore  
  
### <a name="a-rebuilding-an-index"></a>R. Ricompilazione di un indice  
 Nell'esempio seguente viene ricompilato un singolo indice della tabella `Employee` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX PK_Employee_EmployeeID ON HumanResources.Employee REBUILD;  
```  
  
### <a name="b-rebuilding-all-indexes-on-a-table-and-specifying-options"></a>B. Ricompilazione di tutti gli indici di una tabella e impostazione di opzioni  
 Nell'esempio seguente viene specificata la parola chiave ALL. In questo modo vengono ricompilati tutti gli indici associati alla tabella Production.Product nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Vengono inoltre specificate tre opzioni.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, STATISTICS_NORECOMPUTE = ON);  
```  
  
Nell'esempio seguente viene aggiunta l'opzione ONLINE, inclusa l'opzione di blocco con priorità bassa, e viene aggiunta l'opzione di compressione di riga.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH   
(  
    FILLFACTOR = 80,   
    SORT_IN_TEMPDB = ON,  
    STATISTICS_NORECOMPUTE = ON,  
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES, ABORT_AFTER_WAIT = BLOCKERS ) ),   
    DATA_COMPRESSION = ROW  
);  
```  
  
### <a name="c-reorganizing-an-index-with-lob-compaction"></a>C. Ricompilazione di un indice con la compattazione di dati LOB  
 Nell'esempio seguente viene riorganizzato un singolo indice cluster nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Poiché l'indice contiene un tipo di dati LOB al livello foglia, l'istruzione compatta inoltre tutte le pagine contenenti dati LOB. Si noti che non è necessario specificare l'opzione WITH (LOB_COMPACTION = ON) perché il valore predefinito è ON.  
  
```sql  
ALTER INDEX PK_ProductPhoto_ProductPhotoID ON Production.ProductPhoto REORGANIZE WITH (LOB_COMPACTION = ON);  
```  
  
### <a name="d-setting-options-on-an-index"></a>D. Impostazione di opzioni per un indice  
 Nell'esempio seguente vengono impostate diverse opzioni per l'indice `AK_SalesOrderHeader_SalesOrderNumber` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX AK_SalesOrderHeader_SalesOrderNumber ON  
    Sales.SalesOrderHeader  
SET (  
    STATISTICS_NORECOMPUTE = ON,  
    IGNORE_DUP_KEY = ON,  
    ALLOW_PAGE_LOCKS = ON  
    ) ;  
GO
```  
  
### <a name="e-disabling-an-index"></a>E. Disabilitazione di un indice  
 Nell'esempio seguente viene disabilitato un indice non cluster della tabella `Employee` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX IX_Employee_ManagerID ON HumanResources.Employee DISABLE;
```  
  
### <a name="f-disabling-constraints"></a>F. Disabilitazione dei vincoli  
 Nell'esempio seguente viene disabilitato un vincolo PRIMARY KEY disabilitando l'indice PRIMARY KEY nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Il vincolo FOREIGN KEY della tabella sottostante viene disabilitato automaticamente e viene visualizzato un messaggio di avviso.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department DISABLE;  
```  
  
Nel set di risultati viene restituito il messaggio di avviso seguente.  
  
```  
Warning: Foreign key 'FK_EmployeeDepartmentHistory_Department_DepartmentID'  
on table 'EmployeeDepartmentHistory' referencing table 'Department'  
was disabled as a result of disabling the index 'PK_Department_DepartmentID'.
```  
  
### <a name="g-enabling-constraints"></a>G. Abilitazione di vincoli  
 Nell'esempio seguente vengono abilitati i vincoli PRIMARY KEY e FOREIGN KEY disabilitati nell'esempio F.  
  
Il vincolo PRIMARY KEY viene abilitato tramite la ricompilazione dell'indice PRIMARY KEY.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department REBUILD;  
```  
  
Viene quindi abilitato il vincolo FOREIGN KEY.  
  
```sql  
ALTER TABLE HumanResources.EmployeeDepartmentHistory  
CHECK CONSTRAINT FK_EmployeeDepartmentHistory_Department_DepartmentID;  
GO  
```  
  
### <a name="h-rebuilding-a-partitioned-index"></a>H. Ricompilazione di un indice partizionato  
 Nell'esempio seguente viene ricompilata una singola partizione, con numero `5`, dell'indice partizionato `IX_TransactionHistory_TransactionDate` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La partizione 5 viene ricompilata online e il tempo di attesa di 10 minuti per il blocco con priorità bassa viene applicato separatamente a ogni blocco acquisito dall'operazione di ricompilazione dell'indice. Se durante questo periodo di tempo non è possibile ottenere il blocco per completare la ricompilazione dell'indice, l'istruzione dell'operazione di ricompilazione viene interrotta.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
-- Verify the partitioned indexes.  
SELECT *  
FROM sys.dm_db_index_physical_stats (DB_ID(),OBJECT_ID(N'Production.TransactionHistory'), NULL , NULL, NULL);  
GO  
--Rebuild only partition 5.  
ALTER INDEX IX_TransactionHistory_TransactionDate  
ON Production.TransactionHistory  
REBUILD Partition = 5   
   WITH (ONLINE = ON (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 10 minutes, ABORT_AFTER_WAIT = SELF)));  
GO  
```  
  
### <a name="i-changing-the-compression-setting-of-an-index"></a>I. Modifica dell'impostazione di compressione di un indice  
 Nell'esempio seguente viene ricompilato un indice in una tabella rowstore non partizionata.  
  
```sql
ALTER INDEX IX_INDEX1   
ON T1  
REBUILD   
WITH (DATA_COMPRESSION = PAGE);  
GO  
```  
  
Per altri esempi sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
 
### <a name="j-online-resumable-index-rebuild"></a>J. Ricompilazione dell'indice ripristinabile online

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 Negli esempi seguenti viene illustrato come usare la ricompilazione dell'indice ripristinabile online. 

1. Eseguire una ricompilazione dell'indice online come operazione ripristinabile con MAXDOP = 1.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, MAXDOP=1, RESUMABLE=ON) ;
   ```

2. Se si esegue nuovamente lo stesso comando (vedere sopra) dopo aver sospeso un'operazione sull'indice, l'operazione di ricompilazione dell'indice riprende automaticamente.

3. Eseguire una ricompilazione dell'indice online come operazione ripristinabile con MAX_DURATION impostato su 240 minuti.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, RESUMABLE=ON, MAX_DURATION=240) ; 
   ```
4. Sospendere un'operazione di ricompilazione dell'indice online ripristinabile in esecuzione.

   ```sql
   ALTER INDEX test_idx on test_table PAUSE ;
   ```   
5. Riprendere una ricompilazione dell'indice online per una ricompilazione dell'indice che è stata eseguita come operazione ripristinabile specificando un nuovo valore per MAXDOP impostato su 4.

   ```sql
   ALTER INDEX test_idx on test_table RESUME WITH (MAXDOP=4) ;
   ```
6. Riprendere un'operazione di ricompilazione dell'indice online per una ricompilazione dell'indice online che è stata eseguita come ripristinabile. Impostare MAXDOP su 2, il tempo di esecuzione dell'indice in esecuzione come ripristinabile su 240 minuti e in caso di un indice bloccato attendere 10 minuti e successivamente terminare tutti i blocchi. 

   ```sql
      ALTER INDEX test_idx on test_table  
         RESUME WITH (MAXDOP=2, MAX_DURATION= 240 MINUTES, 
         WAIT_AT_LOW_PRIORITY (MAX_DURATION=10, ABORT_AFTER_WAIT=BLOCKERS)) ;
   ```      
7. Interrompere l'operazione di ricompilazione dell'indice ripristinabile che è stata sospesa o è in esecuzione.

   ```sql
   ALTER INDEX test_idx on test_table ABORT ;
   ``` 
  
## <a name="see-also"></a>Vedere anche  
[Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md)    
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
[CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)   
[Disabilitare indici e vincoli](../../relational-databases/indexes/disable-indexes-and-constraints.md)   
[Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
[Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)   
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
