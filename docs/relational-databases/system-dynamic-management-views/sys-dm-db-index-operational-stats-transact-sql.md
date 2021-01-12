---
description: sys.dm_db_index_operational_stats (Transact-SQL)
title: sys.dm_db_index_operational_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_index_operational_stats
- sys.dm_db_index_operational_stats_TSQL
- sys.dm_db_index_operational_stats
- dm_db_index_operational_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_operational_stats dynamic management function
ms.assetid: 13adf2e5-2150-40a6-b346-e74a33ce29c6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a6eb4083361d07fee44557d20dd4be4625cbdb12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095181"
---
# <a name="sysdm_db_index_operational_stats-transact-sql"></a>sys.dm_db_index_operational_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce l'attività di I/O, blocco, latch e metodo di accesso di livello inferiore correnti per ogni partizione di una tabella o di un indice nel database.    
    
 Gli indici con ottimizzazione per la memoria non vengono visualizzati in questa DMV.    
    
> [!NOTE]    
>  **sys.dm_db_index_operational_stats** non restituisce informazioni sugli indici ottimizzati per la memoria. Per informazioni sull'utilizzo dell'indice ottimizzato per la memoria, vedere [sys.dm_db_xtp_index_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).    
        
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Sintassi    
    
```    
sys.dm_db_index_operational_stats (    
    { database_id | NULL | 0 | DEFAULT }    
  , { object_id | NULL | 0 | DEFAULT }    
  , { index_id | 0 | NULL | -1 | DEFAULT }    
  , { partition_number | NULL | 0 | DEFAULT }    
)    
```    
    
## <a name="arguments"></a>Argomenti    

*database_id* | NULL | 0 | PREDEFINITA

  ID del database. *database_id* è **smallint**. Gli input validi sono il numero di ID di un database, NULL, 0 o DEFAULT. Il valore predefinito è 0. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto.    
    
 Specificare NULL per restituire informazioni per tutti i database presenti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si specifica NULL per *database_id*, è necessario specificare null anche per *object_id*, *index_id* e *partition_number*.    
    
 È possibile specificare la funzione predefinita [DB_ID](../../t-sql/functions/db-id-transact-sql.md).    

*object_id* | NULL | 0 | PREDEFINITA

 ID oggetto della tabella o vista in cui si trova l'indice. *object_id* è di **tipo int**.    
    
 Gli input validi sono il numero di ID di una tabella o vista, NULL, 0 o DEFAULT. Il valore predefinito è 0. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto.    
    
 Specificare NULL per restituire le informazioni memorizzate nella cache per tutte le tabelle e le viste nel database specificato. Se si specifica NULL per *object_id*, è necessario specificare null anche per *index_id* e *partition_number*.    

*index_id* | 0 | NULL | -1 | PREDEFINITA

 ID dell'indice. *index_id* è di **tipo int**. Gli input validi sono il numero di ID di un indice, 0 se *object_id* è un heap, null,-1 o default. Il valore predefinito è -1. NULL, -1 e DEFAULT sono valori equivalenti in questo contesto.    
    
 Specificare NULL per restituire le informazioni memorizzate nella cache per tutti gli indici per una vista o tabella di base. Se si specifica NULL per *index_id*, è necessario specificare null anche per *partition_number*.    

*partition_number* | NULL | 0 | PREDEFINITA

 Numero di partizione nell'oggetto. *partition_number* è di **tipo int**. Gli input validi sono la *partion_number* di un indice o di un heap, null, 0 o default. Il valore predefinito è 0. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto.    
    
 Specificare NULL per restituire informazioni memorizzate nella cache per tutte le partizioni dell'indice o dell'heap.    
    
 *partition_number* è in base 1. Un indice o heap non partizionato ha *partition_number* impostato su 1.    
    
## <a name="table-returned"></a>Tabella restituita    
    
|Nome colonna|Tipo di dati|Descrizione|    
|-----------------|---------------|-----------------|    
|**database_id**|**smallint**|ID del database.|    
|**object_id**|**int**|ID della tabella o vista.|    
|**index_id**|**int**|ID dell'indice o dell'heap.<br /><br /> 0 = heap| 
|**partition_number**|**int**|Numero di partizione in base 1 all'interno dell'indice o heap.| 
|**hobt_id**|**bigint**|**Si applica a: (da alla** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .<br /><br /> ID del set di righe di dati heap o albero B che tiene traccia dei dati interni per un indice columnstore.<br /><br /> NULL: non è un set di righe columnstore interno.<br /><br /> Per informazioni dettagliate, vedere [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)|       
|**leaf_insert_count**|**bigint**|Conteggio cumulativo degli inserimenti al livello foglia.|    
|**leaf_delete_count**|**bigint**|Conteggio cumulativo delle eliminazioni al livello foglia. leaf_delete_count viene incrementato solo per i record eliminati che non sono contrassegnati prima come Ghost. Per i record eliminati per primi, viene incrementato il **leaf_ghost_count** .|    
|**leaf_update_count**|**bigint**|Conteggio cumulativo degli aggiornamenti al livello foglia.|    
|**leaf_ghost_count**|**bigint**|Conteggio cumulativo delle righe al livello foglia contrassegnate come eliminate ma non ancora rimosse. Questo conteggio non include record che vengono immediatamente eliminati senza essere contrassegnati come Ghost. Queste righe vengono rimosse da un thread di pulizia a intervalli impostati. Questo valore non include righe memorizzate a causa di una transazione di isolamento dello snapshot in sospeso.|    
|**nonleaf_insert_count**|**bigint**|Conteggio cumulativo degli inserimenti sopra il livello foglia.<br /><br /> 0 = heap o columnstore|    
|**nonleaf_delete_count**|**bigint**|Conteggio cumulativo delle eliminazioni sopra il livello foglia.<br /><br /> 0 = heap o columnstore|    
|**nonleaf_update_count**|**bigint**|Conteggio cumulativo degli aggiornamenti sopra il livello foglia.<br /><br /> 0 = heap o columnstore|    
|**leaf_allocation_count**|**bigint**|Conteggio cumulativo delle allocazioni di pagina al livello foglia nell'indice o heap.<br /><br /> Per un indice un'allocazione di pagina corrisponde a una suddivisione di pagina.|    
|**nonleaf_allocation_count**|**bigint**|Conteggio cumulativo delle allocazioni di pagina provocate da suddivisioni di pagina sopra il livello foglia.<br /><br /> 0 = heap o columnstore|    
|**leaf_page_merge_count**|**bigint**|Conteggio cumulativo delle unioni di pagina in corrispondenza del livello foglia. Sempre 0 per un indice columnstore.|    
|**nonleaf_page_merge_count**|**bigint**|Conteggio cumulativo delle unioni di pagina sopra il livello foglia.<br /><br /> 0 = heap o columnstore|    
|**range_scan_count**|**bigint**|Conteggio cumulativo delle analisi di intervallo e tabella avviate nell'indice o nell'heap.|    
|**singleton_lookup_count**|**bigint**|Conteggio cumulativo dei recuperi di singole righe dall'indice o heap.|    
|**forwarded_fetch_count**|**bigint**|Conteggio delle righe recuperate tramite un record di inoltro.<br /><br /> 0 = Indici|    
|**lob_fetch_in_pages**|**bigint**|Conteggio cumulativo delle pagine LOB recuperate dall'unità di allocazione LOB_DATA. Queste pagine contengono dati archiviati in colonne di tipo **Text**, **ntext**, **Image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)** e **XML**. Per altre informazioni, vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|    
|**lob_fetch_in_bytes**|**bigint**|Conteggio cumulativo dei byte di dati LOB recuperati.|    
|**lob_orphan_create_count**|**bigint**|Conteggio cumulativo dei valori LOB isolati (orfani) creati per le operazioni bulk.<br /><br /> 0 = Indice non cluster|    
|**lob_orphan_insert_count**|**bigint**|Conteggio cumulativo dei valori LOB isolati (orfani) inseriti durante le operazioni bulk.<br /><br /> 0 = Indice non cluster|    
|**row_overflow_fetch_in_pages**|**bigint**|Conteggio cumulativo delle pagine di dati di overflow della riga recuperate dall'unità di allocazione ROW_OVERFLOW_DATA.<br /><br /> Queste pagine contengono dati archiviati in colonne di tipo **varchar (n)**, **nvarchar (n)**, **varbinary (n)** e **sql_variant** di cui è stato eseguito il push all'esterno di righe.|    
|**row_overflow_fetch_in_bytes**|**bigint**|Conteggio cumulativo dei byte di dati di overflow della riga recuperati.|    
|**column_value_push_off_row_count**|**bigint**|Conteggio cumulativo dei valori di colonna per i dati LOB e di overflow della riga spostati all'esterno di righe per adattare una riga inserita o aggiornata all'interno di una pagina.|    
|**column_value_pull_in_row_count**|**bigint**|Conteggio cumulativo dei valori di colonna per i dati LOB e di overflow della riga esclusi dalla riga. Ciò si verifica quando un'operazione di aggiornamento libera spazio in un record e offre l'opportunità di includere uno o più valori all'esterno di righe dall'unità di allocazione LOB_DATA o ROW_OVERFLOW_DATA nell'unità di allocazione IN_ROW_DATA.|    
|**row_lock_count**|**bigint**|Numero cumulativo di blocchi di riga richiesti.|    
|**row_lock_wait_count**|**bigint**|Numero cumulativo di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha atteso un blocco di riga.|    
|**row_lock_wait_in_ms**|**bigint**|Numero totale di millisecondi che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha atteso un blocco di riga.|    
|**page_lock_count**|**bigint**|Numero cumulativo di blocchi di pagina richiesti.|    
|**page_lock_wait_count**|**bigint**|Numero cumulativo di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha atteso un blocco di pagina.|    
|**page_lock_wait_in_ms**|**bigint**|Numero totale di millisecondi che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha atteso un blocco di pagina.|    
|**index_lock_promotion_attempt_count**|**bigint**|Numero cumulativo di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha tentato di alzare di livello i blocchi.|    
|**index_lock_promotion_count**|**bigint**|Numero cumulativo di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha alzato di livello i blocchi.|    
|**page_latch_wait_count**|**bigint**|Numero cumulativo di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] è rimasto in attesa a causa di una contesa di latch.|    
|**page_latch_wait_in_ms**|**bigint**|Numero cumulativo di millisecondi che [!INCLUDE[ssDE](../../includes/ssde-md.md)] è rimasto in attesa a causa di una contesa di latch.|    
|**page_io_latch_wait_count**|**bigint**|Numero cumulativo di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha atteso un latch della pagina di I/O.|    
|**page_io_latch_wait_in_ms**|**bigint**|Numero cumulativo di millisecondi che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha atteso un latch di I/O della pagina.|    
|**tree_page_latch_wait_count**|**bigint**|Subset di **page_latch_wait_count** che include solo le pagine dell'albero B di livello superiore. Sempre 0 per un heap o un indice columnstore.|    
|**tree_page_latch_wait_in_ms**|**bigint**|Subset di **page_latch_wait_in_ms** che include solo le pagine dell'albero B di livello superiore. Sempre 0 per un heap o un indice columnstore.|    
|**tree_page_io_latch_wait_count**|**bigint**|Subset di **page_io_latch_wait_count** che include solo le pagine dell'albero B di livello superiore. Sempre 0 per un heap o un indice columnstore.|    
|**tree_page_io_latch_wait_in_ms**|**bigint**|Subset di **page_io_latch_wait_in_ms** che include solo le pagine dell'albero B di livello superiore. Sempre 0 per un heap o un indice columnstore.|    
|**page_compression_attempt_count**|**bigint**|Numero di pagine valutate per la compressione di tipo PAGE per partizioni specifiche di una tabella, un indice o una vista indicizzata. Sono incluse le pagine che non sono state compresse perché la compressione non avrebbe comportato risparmi significativi. Sempre 0 per un indice columnstore.|    
|**page_compression_success_count**|**bigint**|Numero di pagine di dati valutate compresse utilizzando la compressione di tipo PAGE per partizioni specifiche di una tabella, un indice o una vista indicizzata. Sempre 0 per un indice columnstore.|    
    
## <a name="remarks"></a>Osservazioni    
 Questo oggetto a gestione dinamica non accetta parametri correlati da `CROSS APPLY` e `OUTER APPLY` .    
    
 È possibile utilizzare **sys.dm_db_index_operational_stats** per tenere traccia della quantità di tempo che gli utenti devono attendere per leggere o scrivere in una tabella, un indice o una partizione e per identificare tabelle e indici in cui viene rilevata una significativa attività di I/O o aree critiche.    
    
 Utilizzare le colonne seguenti per identificare le aree di contesa.    
    
 **Per analizzare un comune modello di accesso alla partizione di tabelle o indici**, utilizzare le colonne seguenti:    
    
-   **leaf_insert_count**    
    
-   **leaf_delete_count**    
    
-   **leaf_update_count**    
    
-   **leaf_ghost_count**    
    
-   **range_scan_count**    
    
-   **singleton_lookup_count**    
    
 Per identificare le contese a livello di latch e blocchi, utilizzare le colonne seguenti:    
    
-   **page_latch_wait_count** e **page_latch_wait_in_ms**    
    
     Queste colonne indicano se è presente una contesa di latch nell'indice o nell'heap e specificano l'importanza di tale contesa.    
    
-   **row_lock_count** e **page_lock_count**    
    
     Queste colonne indicano il numero di volte che [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha cercato di acquisire blocchi di riga e pagina.    
    
-   **row_lock_wait_in_ms** e **page_lock_wait_in_ms**    
    
     Queste colonne indicano se è presente una contesa di blocchi nell'indice o heap e l'importanza di tale contesa.    
    
 **Per analizzare le statistiche degli I/O fisici in una partizione di indice o heap**    
    
-   **page_io_latch_wait_count** e **page_io_latch_wait_in_ms**    
    
     Queste colonne indicano se gli I/O fisici sono stati eseguiti per inserire le pagine di indice o heap in memoria e il numero di I/O eseguiti.    
    
## <a name="column-remarks"></a>Osservazioni relative alle colonne    
 I valori nelle colonne **lob_orphan_create_count** e **lob_orphan_insert_count** devono essere sempre uguali.    
    
 I valori nelle colonne **lob_fetch_in_pages** e **lob_fetch_in_bytes** possono essere maggiori di zero per indici non cluster contenenti una o più colonne LOB come colonne incluse. Per altre informazioni, vedere [Creare indici con colonne incluse](../../relational-databases/indexes/create-indexes-with-included-columns.md). In modo analogo, il valore nelle colonne **row_overflow_fetch_in_pages** e **row_overflow_fetch_in_bytes** può essere maggiore di 0 per indici non cluster se l'indice include colonne che possono essere spostate all'esterno di righe.    
    
## <a name="how-the-counters-in-the-metadata-cache-are-reset"></a>Modalità di reimpostazione dei contatori nella cache dei metadati    
 I dati restituiti da **sys.dm_db_index_operational_stats** esistono solo finché è disponibile l'oggetto cache dei metadati che rappresenta l'heap o l'indice. Questi dati non sono persistenti, né consistenti dal punto di vista transazionale. Ciò significa che non è possibile utilizzare questi contatori per determinare se un indice è stato utilizzato o meno oppure quando l'indice è stato utilizzato per l'ultima volta. Per informazioni, vedere [sys.dm_db_index_usage_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md).    
    
 I valori di ogni colonna vengono impostati su zero ogni volta che i metadati per l'heap o l'indice vengono inseriti nella cache dei metadati e le statistiche vengono accumulate finché l'oggetto cache non viene rimosso dalla cache dei metadati. È pertanto possibile che un heap o un indice attivo disponga sempre dei relativi metadati nella cache e che il conteggio cumulativo rifletta l'attività dall'ultimo avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I metadati di un heap o un indice meno attivo verranno inseriti nella e rimossi dalla cache in base al loro utilizzo. Ne consegue che i valori potrebbero non essere disponibili. L'eliminazione di un indice comporterà la rimozione delle statistiche corrispondenti dalla memoria e tali dati non verranno più rilevati dalla funzione. Altre operazioni DDL nell'indice potrebbero provocare l'azzeramento del valore delle statistiche.    
    
## <a name="using-system-functions-to-specify-parameter-values"></a>Utilizzo di funzioni di sistema per specificare i valori dei parametri    
 È possibile utilizzare le [!INCLUDE[tsql](../../includes/tsql-md.md)] funzioni [DB_ID](../../t-sql/functions/db-id-transact-sql.md) e [object_id](../../t-sql/functions/object-id-transact-sql.md) per specificare un valore per i parametri *database_id* e *object_id* . Se si passano valori non validi a queste funzioni, tuttavia, si potrebbero provocare risultati imprevisti. Quando si usa DB_ID o OBJECT_ID, verificare sempre che venga restituito un ID valido. Per ulteriori informazioni, vedere la sezione Osservazioni in [sys.dm_db_index_physical_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).    
    
## <a name="permissions"></a>Autorizzazioni    
 Sono richieste le autorizzazioni seguenti:    
    
-   `CONTROL` autorizzazione per l'oggetto specificato all'interno del database    
    
-   `VIEW DATABASE STATE` autorizzazione per la restituzione di informazioni su tutti gli oggetti all'interno del database specificato, tramite il carattere jolly di oggetto @*object_id* = null    
    
-   `VIEW SERVER STATE` autorizzazione per la restituzione di informazioni su tutti i database, tramite il carattere jolly di database @*database_id* = null    
    
 La concessione `VIEW DATABASE STATE` consente la restituzione di tutti gli oggetti nel database, indipendentemente dalle autorizzazioni di controllo negate per oggetti specifici.    
    
 La negazione `VIEW DATABASE STATE` impedisce la restituzione di tutti gli oggetti nel database, indipendentemente dalle autorizzazioni di controllo concesse per oggetti specifici. Inoltre, quando viene specificato il carattere jolly del database `@database_id=NULL` , il database viene omesso.    
    
 Per altre informazioni, vedere [viste a gestione dinamica e funzioni &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).    
    
## <a name="examples"></a>Esempi    
    
### <a name="a-returning-information-for-a-specified-table"></a>R. Visualizzazione di informazioni per una tabella specifica    
 Nell'esempio seguente vengono restituite informazioni per tutti gli indici e le partizioni della tabella `Person.Address` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Per eseguire questa query, è necessario disporre almeno dell'autorizzazione CONTROL per la tabella `Person.Address`.    
    
> [!IMPORTANT]    
> In caso di utilizzo delle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] DB_ID e OBJECT_ID per restituire un valore di parametro, assicurarsi sempre che venga restituito un ID valido. Se risulta impossibile trovare il nome del database o dell'oggetto, ad esempio quando tali nomi non esistono o sono stati immessi con un'ortografia errata, entrambe le funzioni restituiranno NULL. La funzione **sys.dm_db_index_operational_stats** interpreta NULL come un valore di carattere jolly che specifica tutti i database o tutti gli oggetti. Poiché può trattarsi di un'operazione accidentale, gli esempi riportati in questa sezione dimostrano la procedura sicura per determinare gli ID di database e oggetti.    
    
```sql    
DECLARE @db_id int;    
DECLARE @object_id int;    
SET @db_id = DB_ID(N'AdventureWorks2012');    
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');    
IF @db_id IS NULL     
  BEGIN;    
    PRINT N'Invalid database';    
  END;    
ELSE IF @object_id IS NULL    
  BEGIN;    
    PRINT N'Invalid object';    
  END;    
ELSE    
  BEGIN;    
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);    
  END;    
GO    
```    
    
### <a name="b-returning-information-for-all-tables-and-indexes"></a>B. Restituzione delle informazioni per tutti gli indici e le tabelle    
 Nell'esempio seguente vengono restituite informazioni per tutte le tabelle e tutti gli indici nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per eseguire questa query, è necessario disporre dell'autorizzazione VIEW SERVER STATE.    
    
```sql    
SELECT * FROM sys.dm_db_index_operational_stats( NULL, NULL, NULL, NULL);    
GO        
```    
    
## <a name="see-also"></a>Vedere anche    
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
 [Funzioni a gestione dinamica e DMV correlate all'indice &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)     
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)     
 [sys.dm_db_index_usage_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)     
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)     
 [sys.dm_db_partition_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)     
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)    
    
