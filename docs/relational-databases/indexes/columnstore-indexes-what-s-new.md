---
description: Indici columnstore - Novità
title: Indici columnstore - Novità | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5db96c7a21d5aef96615823ee275486e6f921686
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744361"
---
# <a name="columnstore-indexes---what39s-new"></a>Indici columnstore - Novità
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Riepilogo delle funzionalità columnstore disponibili per ogni versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e per la versione più recente di [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

 > [!NOTE]
 > Per [!INCLUDE[ssSDS](../../includes/sssds-md.md)], gli indici columnstore sono disponibili nei livelli Premium e Standard di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (S3 e versioni successive) e in tutti i livelli vCore. Per [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive gli indici columnstore sono disponibili in tutte le edizioni. Per [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (versioni precedenti a SP1) e versioni precedenti, gli indici columnstore sono disponibili solo nell'edizione Enterprise.
 
## <a name="feature-summary-for-product-releases"></a>Riepilogo delle funzionalità per le versioni dei prodotti  
 Questa tabella riepiloga le funzionalità principali per gli indici columnstore e i prodotti in cui sono disponibili.  

|Funzionalità indice columnstore|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]|[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]|  
|-------------------------------|---------------------------|---------------------------|---------------------------|---------------------------|--------------------------------------------|-------------------------|---|  
|Esecuzione in modalità batch per le query multithreading|sì|sì|sì|sì|sì|sì|sì| 
|Esecuzione in modalità batch per le query a thread singolo|||sì|sì|sì|sì|sì|  
|Opzione di compressione dell'archivio||sì|sì|sì|sì|sì|sì|  
|Isolamento dello snapshot e dello snapshot Read Committed|||sì|sì|sì|sì|sì| 
|Specificare l'indice columnstore durante la creazione di una tabella|||sì|sì|sì|sì|sì|  
|Always On supporta gli indici columnstore|sì|sì|sì|sì|sì|sì|sì| 
|Le repliche secondarie leggibili Always On supportano l'indice columnstore non cluster di sola lettura|sì|sì|sì|sì|sì|sì|sì|  
|Le repliche secondarie leggibili Always On supportano gli indici columnstore aggiornabili|||sì||sì|||  
|Indice columnstore non cluster di sola lettura su heap o albero B|sì|sì|sì <sup>1</sup>|sì <sup>1</sup>|sì <sup>1</sup>|sì <sup>1</sup>|sì <sup>1</sup>|  
|Indice columnstore non cluster aggiornabile su heap o albero B|||sì|sì|sì|sì|sì|  
|Indici albero B aggiuntivi consentiti su un heap o albero B che dispone di un indice columnstore non cluster|sì|sì|sì|sì|sì|sì|sì|  
|Indice columnstore cluster aggiornabile||sì|sì|sì||sì|sì|  
|Indice albero B su un indice columnstore cluster|||sì|sì||sì|sì|  
|Indice columnstore su una tabella ottimizzata per la memoria|||sì|sì||sì|sì|  
|La definizione degli indici columnstore non cluster supporta l'uso di una condizione filtrata|||sì|sì|sì|sì|sì|  
|Opzione relativa al ritardo di compressione per gli indici columnstore in `CREATE TABLE` e `ALTER TABLE`|||sì|sì|sì|sì|sì|
|L'indice columnstore può avere una colonna calcolata non persistente||||sì|sì|||   
|Supporto dell'unione in background del motore di tuple||||||sì|sì|sì|
  
 <sup>1</sup> Per creare un indice columnstore non cluster di sola lettura, archiviare l'indice in un filegroup di sola lettura.  
 
> [!NOTE]
> Il grado di parallelismo per le operazioni in [modalità batch](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) è limitato a 2 per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition e 1 per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Web Edition ed Express Edition. Questo si riferisce agli indici columnstore creati tramite le tabelle basate su disco e le tabelle ottimizzate per la memoria.

## [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 
 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] aggiunge queste nuove funzionalità.

### <a name="functional"></a>Funzionale
- A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il motore di tuple è supportato da un'attività di unione in background, che comprime automaticamente i rowgroup delta OPEN più piccoli che sono esistiti per un dato periodo di tempo (come determinato da una soglia interna) oppure unisce i rowgroup COMPRESSED da cui è stato eliminato un numero elevato di righe. In precedenza era necessaria un'operazione di riorganizzazione dell'indice per unire i rowgroup con dati eliminati parzialmente. Ciò migliora la qualità dell'indice columnstore nel tempo. 

## [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 
 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] aggiunge queste nuove funzionalità.

### <a name="functional"></a>Funzionale
- [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] supporta le colonne calcolate non persistenti in indici columnstore cluster. Le colonne calcolate persistenti non sono supportate in indici columnstore cluster. Non è possibile creare un indice non cluster su un indice columnstore che contiene una colonna calcolata. 

## [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] aggiunge miglioramenti importanti per aumentare le prestazioni e la flessibilità degli indici columnstore. In questo modo è possibile migliorare gli scenari di data warehouse e abilitare l'analisi operativa in tempo reale.  
  
### <a name="functional"></a>Funzionale  
  
-   Una tabella rowstore può avere un solo indice columnstore non cluster aggiornabile. In precedenza, l'indice columnstore non cluster era di sola lettura.  
  
-   La definizione degli indici columnstore non cluster supporta l'uso di una condizione filtrata. Per ridurre al minimo l'impatto sulle prestazioni conseguente all'aggiunta di un indice columnstore in una tabella OLTP, usare una condizione filtrata per creare un indice columnstore non cluster solo sui dati usati meno di frequente del carico di lavoro operativo. 
  
-   Una tabella in memoria può avere un solo indice columnstore. È possibile crearlo durante la creazione della tabella o aggiungerlo in un secondo momento con [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md). In precedenza, solo una tabella basata su disco poteva avere un indice columnstore.  
  
-   Un indice columnstore cluster può avere uno o più indici rowstore non cluster. In precedenza, l'indice columnstore non supportava gli indici non cluster. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestisce automaticamente gli indici non cluster per le operazioni DML.  
  
-   Supporto di chiavi primarie e chiavi esterne usando un indice albero B per imporre questi vincoli su un indice columnstore cluster.  
  
-   Gli indici columnstore hanno un'opzione relativa al ritardo di compressione che riduce al minimo l'impatto che il carico di lavoro transazionale ha sull'analisi operativa in tempo reale.  Questa opzione consente di modificare frequentemente le righe per stabilizzarle prima di comprimerle nel columnstore. Per informazioni dettagliate, vedere [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md) (Creare un indice columnstore &#40;Transact-SQL &#41;) e [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="performance-for-database-compatibility-level-120-or-130"></a>Prestazioni per il livello di compatibilità del database 120 o 130  
  
-   Gli indici columnstore supportano il livello di isolamento dello snapshot Read Committed e l'isolamento dello snapshot. Questo consente le query di analisi coerente transazionale senza alcun blocco.  
  
-   Columnstore supporta la deframmentazione degli indici rimuovendo le righe eliminate senza necessità di ricompilare l'indice in modo esplicito. L'istruzione `ALTER INDEX ... REORGANIZE` rimuove dal columnstore le righe eliminate in base a un criterio definito internamente, con un'operazione online  
  
-   Gli indici columnstore sono accessibili su una replica secondaria leggibile Always On. È possibile migliorare le prestazioni per l'analisi operativa ripartendo le query di analisi su una replica secondaria Always On.  
  
-   La distribuzione dell'aggregazione calcola le funzioni di aggregazione `MIN`, `MAX`, `SUM`, `COUNT` e `AVG` durante le scansioni di tabella quando il tipo di dati usa non più di 8 byte e non è di tipo stringa. La distribuzione dell'aggregazione è supportata con o senza clausola `GROUP BY` sia per gli indici columnstore cluster sia per quelli non cluster. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa funzionalità avanzata è riservata per Enterprise Edition.
  
-   La distribuzione del predicato stringa consente di velocizzare le query che confrontano stringhe di tipo VARCHAR/CHAR o NVARCHAR/NCHAR. Questo si applica ai comuni operatori di confronto e include operatori come `LIKE` che usano i filtri bitmap. Funziona con tutte le regole di confronto supportate. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa funzionalità avanzata è riservata per Enterprise Edition. 

-   Miglioramenti per le operazioni in modalità batch sfruttando le funzionalità hardware basate su vettori. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] rileva il livello di supporto CPU per le estensioni hardware AVX 2 (Advanced Vector Extensions) e SSE 4 (Streaming SIMD Extensions 4) e le usa se supportate. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa funzionalità avanzata è riservata per Enterprise Edition.
  
### <a name="performance-for-database-compatibility-level-130"></a>Prestazioni per il livello di compatibilità del database 130  
  
-   Nuovo supporto dell'esecuzione in modalità batch per le query che usano uno di questi operatori:  
    -   `SORT`  
    -   Funzioni di aggregazione con più funzioni distinte. Alcuni esempi: `COUNT/COUNT`, `AVG/SUM`, `CHECKSUM_AGG`, `STDEV/STDEVP`  
    -   Funzioni di aggregazione della finestra: `COUNT`, `COUNT_BIG`, `SUM`, `AVG`, `MIN`, `MAX` e `CLR`  
    -   Funzioni di aggregazione della finestra definite dall'utente: `CHECKSUM_AGG`, `STDEV`, `STDEVP`, `VAR`, `VARP` e `GROUPING`  
    -   Funzioni analitiche di aggregazione della finestra: `LAG`, `LEAD`, `FIRST_VALUE`, `LAST_VALUE`, `PERCENTILE_CONT`, `PERCENTILE_DISC`, `CUME_DIST` e `PERCENT_RANK`  

-   Le query a thread singolo in esecuzione in `MAXDOP 1` o con un piano di query seriale vengono eseguite in modalità batch. Le query multithreading venivano eseguite in modalità batch solo in precedenza.  

-   Le query delle tabelle ottimizzate per la memoria possono avere piani paralleli in modalità SQL InterOp durante l'accesso ai dati nell'indice rowstore o columnstore.
  
### <a name="supportability"></a>Facilità di supporto  
Queste viste di sistema sono una novità per columnstore:  
  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
Queste DMV basate su OLTP in memoria contengono aggiornamenti per columnstore:  

:::row:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="limitations"></a>Limitazioni    
-   Per le tabelle in memoria, un indice columnstore deve includere tutte le colonne; l'indice columnstore non può avere una condizione di filtrata.  
-   Per le tabelle in memoria, le query sugli indici columnstore vengono eseguite solo in modalità InterOP e non in modalità nativa in memoria. È supportata l'esecuzione parallela.  
  
## [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ha introdotto l'indice columnstore cluster come formato di archiviazione primario. Questo ha consentito caricamenti regolari, nonché operazioni di aggiornamento, eliminazione e inserimento.  
  
-   La tabella può usare un indice columnstore cluster come archiviazione tabella primaria. Nella tabella non è consentito nessun altro indice, ma l'indice columnstore cluster è aggiornabile, pertanto è possibile eseguire caricamenti regolari e apportare modifiche alle singole righe.  
-   L'indice columnstore non cluster continua ad avere la stessa funzionalità che aveva in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ad eccezione degli operatori aggiuntivi, che ora possono essere eseguiti in modalità batch. Al momento è aggiornabile solo tramite ricompilazione e usando un cambio di partizione. L'indice columnstore non cluster è supportato solo nelle tabelle basate su disco e non in quelle in memoria.  
-   L'indice columnstore cluster e non cluster ha un'opzione relativa alla compressione dell'archivio che comprime ulteriormente i dati. L'opzione di archiviazione è utile per ridurre le dimensioni dei dati in memoria e su disco, ma comporta un rallentamento delle prestazioni delle query. Funziona anche per i dati a cui si accede raramente.  
-   L'indice columnstore cluster e quello non cluster funzionano in modo molto simile: usano lo stesso formato di archiviazione a colonne, lo stesso motore di elaborazione delle query e lo stesso insieme di viste di gestione dinamica. La differenza risiede nei tipi di indice primario e secondario e nel fatto che l'indice columnstore non cluster è di sola lettura.  
-   Questi operatori vengono eseguiti in modalità batch per le query multithreading: SCAN, FILTER, PROJECT, JOIN, GROUP BY e UNION ALL.  
  
## [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ha introdotto l'indice columnstore non cluster come un altro tipo di indice nelle tabelle rowstore e l'elaborazione batch per le query sui dati columnstore.  
  
-   Una tabella rowstore può avere un solo indice columnstore non cluster.  
-   L'indice columnstore è di sola lettura. Dopo aver creato l'indice columnstore non è possibile aggiornare la tabella tramite operazioni `INSERT`, `DELETE` e `UPDATE`: per eseguire queste operazioni è necessario eliminare l'indice, aggiornare la tabella e ricompilare l'indice columnstore. È possibile caricare dati aggiuntivi nella tabella usando un cambio di partizione. Il vantaggio del cambio di partizione è che consente di caricare dati senza eliminare e ricompilare l'indice columnstore.  
-   L'indice columnstore richiede sempre memoria aggiuntiva, in genere un ulteriore 10% per rowstore, poiché archivia una copia dei dati.  
-   L'elaborazione batch consente di raddoppiare o migliorare le prestazioni delle query, ma è disponibile solo per l'esecuzione di query parallele.  
  
## <a name="see-also"></a>Vedere anche  
 [Indici columnstore - Linee guida per la progettazione](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Indici columnstore - Linee guida per il caricamento di dati](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Prestazioni delle query per gli indici columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Indici columnstore per il data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
  
