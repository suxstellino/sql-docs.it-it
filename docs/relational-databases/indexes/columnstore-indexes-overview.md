---
description: 'Indici columnstore: Panoramica'
title: 'Indici columnstore: Panoramica | Microsoft Docs'
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes creation, columnstore
- indexes [SQL Server], columnstore
- columnstore index
- batch mode execution
- columnstore index, described
- xVelocity, columnstore indexes
ms.assetid: f98af4a5-4523-43b1-be8d-1b03c3217839
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bbc4cb7f94fdbd2788005f1e277e70b7b0c4247d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744271"
---
# <a name="columnstore-indexes-overview"></a>Indici columnstore: Panoramica
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gli indici columnstore rappresentano lo standard per l'archiviazione di tabelle dei fatti di data warehousing di grandi dimensioni e per l'esecuzione di query su queste tabelle. Questo indice usa l'archiviazione dei dati basata su colonne e l'elaborazione di query per ottenere miglioramenti fino a 10 volte per le prestazioni delle query nel data warehouse rispetto all'archiviazione tradizionale orientata alle righe. È anche possibile migliorare fino a 10 volte la compressione dei dati rispetto alla dimensione dei dati non compressi. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, gli indici columnstore consentono l'analisi operativa, rendendo possibile l'esecuzione di analisi in tempo reale ad alte prestazioni su carichi di lavoro transazionali.  
  
Informazioni su uno scenario correlato:  
  
-   [Indici columnstore per il data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
-   [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
  
## <a name="what-is-a-columnstore-index"></a>Che cos'è un indice columnstore?  
L'indice columnstore è una tecnologia per l'archiviazione, il recupero e la gestione dei dati tramite un formato di dati in colonna, detto *columnstore*.  
  
### <a name="key-terms-and-concepts"></a>Termini e concetti chiave  
I seguenti concetti e termini chiave sono associati agli indici columnstore.  
  
#### <a name="columnstore"></a>columnstore
Un indice columnstore è costituito da dati organizzati logicamente in una tabella con righe e colonne e archiviati fisicamente in un formato di dati a colonne.  
  
#### <a name="rowstore"></a>Rowstore
Un indice rowstore è costituito da dati organizzati logicamente in una tabella con righe e colonne e archiviati fisicamente in un formato di dati a righe. Questo formato ha rappresentato il metodo tradizionale per archiviare dati relazionali di tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], rowstore indica una tabella in cui il formato di archiviazione dati sottostante è un heap, un indice cluster o una tabella ottimizzata per la memoria.  
  
> [!NOTE]  
> Quando si parla di indici columnstore, i termini rowstore e columnstore vengono usati per sottolineare il formato per l'archiviazione dei dati.  
  
#### <a name="rowgroup"></a>Rowgroup
Un rowgroup è un gruppo di righe che vengono compresse contemporaneamente nel formato columnstore. Un rowgroup contiene in genere il numero massimo di righe per rowgroup, pari a 1.048.576 righe.  
  
Per garantire prestazioni elevate e un alto tasso di compressione, l'indice columnstore suddivide la tabella in rowgroup, quindi comprime ogni rowgroup per colonne. Il numero di righe nel rowgroup deve essere sufficientemente grande da migliorare il tasso di compressione e sufficientemente ridotto da poter trarre vantaggio dall'esecuzione delle operazioni in memoria.    

Un rowgroup da cui sono stati eliminati tutti i dati passa dallo stato COMPRESSED allo stato TOMBSTONE e viene successivamente rimosso da un processo in background denominato motore di tuple. Per altre informazioni sugli stati dei rowgroup, vedere [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md).

> [!TIP]
> La presenza di un numero eccessivo di rowgroup di piccole dimensioni riduce la qualità dell'indice columnstore. Fino a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], è necessaria un'operazione di riorganizzazione per unire i rowgroup COMPRESSED più piccoli in base a un criterio di soglia interna, che determina come rimuovere le righe eliminate e combinare i rowgroup compressi.    
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], un'attività di unione in background funziona anche per unire i rowgroup COMPRESSED da cui è stato eliminato un numero elevato di righe.     
> Dopo l'unione di rowgroup di dimensioni minori, è necessario migliorare la qualità dell'indice. 

> [!NOTE]
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il motore di tuple viene aiutato da un'attività di unione in background, che comprime automaticamente i rowgroup delta OPEN più piccoli che sono esistiti per un dato periodo di tempo (come determinato da una soglia interna) oppure unisce i rowgroup COMPRESSED da cui è stato eliminato un numero elevato di righe. Ciò migliora la qualità dell'indice columnstore nel tempo.         

#### <a name="column-segment"></a>segmento di colonna
Un segmento di colonna è una colonna di dati all'interno del rowgroup.  
  
-   Ogni rowgroup contiene un segmento di colonna per ogni colonna della tabella.  
-   Ogni segmento di colonna è compresso e archiviato su un supporto fisico.  
  
![Segmento di colonna](../../relational-databases/indexes/media/sql-server-pdw-columnstore-columnsegment.gif "segmento di colonna")  
  
#### <a name="clustered-columnstore-index"></a>Indice columnstore cluster
Un indice columnstore cluster rappresenta l'archivio fisico per l'intera tabella.    
  
![Indice columnstore cluster](../../relational-databases/indexes/media/sql-server-pdw-columnstore-physicalstorage.gif "Indice columnstore cluster")  
  
Per ridurre la frammentazione dei segmenti di colonna e migliorare le prestazioni, l'indice columnstore può archiviare temporaneamente alcuni dati in un indice cluster, detto *deltastore*, e in un BTree di ID per le righe eliminate. Le operazioni deltastore sono gestite in modo automatico. Per tornare ai risultati della query corretti, l'indice columnstore cluster combina i risultati della query da columnstore e deltastore.  
  
#### <a name="delta-rowgroup"></a>Rowgroup differenziale
Un rowgroup delta è un indice cluster dell'albero B che viene usato solo con indici columnstore. Migliora la compressione e le prestazioni dei columnstore archiviando le righe finché il numero di queste non raggiunge una soglia specifica (1.048.576 righe) e spostandole quindi nel columnstore.  

Quando un rowgroup delta raggiunge il numero massimo di righe, passa dallo stato OPEN allo stato CLOSED. Un processo in background denominato motore di tuple controlla la presenza di rowgroup chiusi. Se il processo trova un rowgroup chiuso, comprime il rowgroup delta e lo archivia nel columnstore come rowgroup COMPRESSED. 

Quando un rowgroup delta è stato compresso, il rowgroup delta esistente passa allo stato TOMBSTONE per essere poi rimosso dal motore di tuple quando non riceve nessun riferimento. 

Per altre informazioni sugli stati dei rowgroup, vedere [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

> [!NOTE]
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il motore di tuple viene aiutato da un'attività di unione in background, che comprime automaticamente i rowgroup delta OPEN più piccoli che sono esistiti per un dato periodo di tempo (come determinato da una soglia interna) oppure unisce i rowgroup COMPRESSED da cui è stato eliminato un numero elevato di righe. Ciò migliora la qualità dell'indice columnstore nel tempo.         
  
#### <a name="deltastore"></a>Deltastore
Un indice columnstore può includere più di un rowgroup differenziale. Tutti i rowgroup differenziali sono denominati collettivamente deltastore.   

Durante un caricamento bulk di grandi dimensioni, la maggior parte delle righe viene direttamente indirizzata al columnstore senza passare per il deltastore. È possibile che alla fine del caricamento bulk il numero delle righe sia insufficiente a soddisfare le dimensioni minime di un rowgroup, pari a 102.400. Ne consegue che le righe finali vengono indirizzate al deltastore anziché al columnstore. Per i caricamenti bulk di piccole dimensioni, con meno di 102.400 righe, tutte le righe passano direttamente al deltastore.  
  
#### <a name="nonclustered-columnstore-index"></a>indice columnstore non cluster
Un indice columnstore non cluster e un indice columnstore cluster funzionano allo stesso modo. La differenza è che un indice non cluster è un indice secondario creato per una tabella rowstore, mentre un indice columnstore cluster rappresenta l'archiviazione primaria per l'intera tabella.  
  
L'indice non cluster contiene una copia totale o parziale delle righe e delle colonne della tabella sottostante. L'indice è definito sotto forma di una o più colonne della tabella e ha una condizione facoltativa che consente di filtrare le righe.  
  
Un indice columnstore non cluster consente l'analisi operativa in tempo reale, durante la quale il carico di lavoro OLTP usa l'indice cluster sottostante, mentre l'analisi viene eseguita simultaneamente sull'indice columnstore. Per altre informazioni, vedere [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
#### <a name="batch-mode-execution"></a>Esecuzione in modalità batch
L'esecuzione in modalità batch è un metodo di elaborazione delle query con cui le query elaborano più righe contemporaneamente. L'esecuzione in modalità batch è strettamente integrata nel formato di archiviazione columnstore, per il quale è ottimizzata. L'esecuzione in modalità batch talvolta è detta esecuzione *basata su vettore* o *vettorizzata*. Le query sugli indici columnstore usano l'esecuzione in modalità batch, che migliora le prestazioni delle query in genere da due a quattro volte. Per altre informazioni, vedere [Guida sull'architettura di elaborazione delle query](../query-processing-architecture-guide.md#execution-modes). 
  
##  <a name="why-should-i-use-a-columnstore-index"></a><a name="benefits"></a> Perché usare un indice columnstore?  
Un indice columnstore può garantire un livello di compressione dei dati molto elevato, in genere di 10 volte, riducendo in modo significativo i costi di archiviazione del data warehouse. Per l'analisi, gli indici columnstore offrono anche prestazioni decisamente migliori rispetto agli indici BTree e rappresentano il formato di archiviazione di dati preferito per i carichi di lavoro di analisi e data warehousing. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]è possibile usare gli indici columnstore per l'analisi in tempo reale del carico di lavoro operativo.  
  
Ecco perché gli indici columnstore sono così rapidi:  
  
-   Le colonne archiviano valori provenienti dallo stesso dominio. Si tratta in genere di valori simili, il che consente un tasso di compressione elevato. I colli di bottiglia per le operazioni di I/O nel sistema sono ridotti al minimo o eliminati e il footprint di memoria viene ridotto significativamente.  
  
-   Le frequenze di compressione elevate migliorano le prestazioni delle query utilizzando un footprint di memoria più piccolo. A loro volta, le prestazioni delle query costituiscono un miglioramento perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può eseguire un numero maggiore di operazioni di dati e query in memoria.  
  
-   L'esecuzione batch migliora le prestazioni delle query, in genere, da due a quattro volte grazie all'elaborazione di più righe contemporaneamente.  
  
-   Le query spesso selezionano solo alcune colonne di una tabella, riducendo il totale delle operazioni di I/O su un supporto fisico.  
  
## <a name="when-should-i-use-a-columnstore-index"></a>Quando usare un indice columnstore?  
Usi consigliati:  
  
-   Usare un indice columnstore cluster per archiviare tabelle dei fatti e tabelle di grandi dimensioni per i carichi di lavoro di data warehousing. Questo metodo migliora le prestazioni delle query e la compressione dei dati fino a 10 volte. Per altre informazioni, vedere [Indici columnstore per il data warehousing](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md).  
  
-   Usare un indice columnstore non cluster per eseguire l'analisi in tempo reale di un carico di lavoro OLTP. Per altre informazioni, vedere [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="how-do-i-choose-between-a-rowstore-index-and-a-columnstore-index"></a>Come scegliere tra un indice rowstore e un indice columnstore?  
Gli indici rowstore offrono prestazioni ottimali con le query che eseguono la ricerca di un valore specifico o all'interno di un intervallo di valori di piccole dimensioni. Usare gli indici rowstore con carichi di lavoro transazionali, poiché per i carichi di lavoro di questo tipo sono in genere necessarie ricerche all'interno delle tabelle anziché scansioni di queste.  
  
Gli indici columnstore garantiscono prestazioni notevolmente elevate per le query analitiche su grandi quantità di dati, soprattutto per le tabelle di grandi dimensioni. Usare gli indici columnstore per i carichi di lavoro di data warehousing e analisi, soprattutto sulle tabelle dei fatti, poiché in genere questi carichi di lavoro richiedono scansioni complete delle tabelle anziché ricerche all'interno di queste.  
  
### <a name="can-i-combine-rowstore-and-columnstore-on-the-same-table"></a>È possibile combinare indici rowstore e columnstore nella stessa tabella?  
Sì. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile creare un indice columnstore non cluster aggiornabile in una tabella rowstore. L'indice columnstore archivia una copia delle colonne selezionate. È quindi necessario spazio aggiuntivo per questi dati, ma la compressione applicata ai dati selezionati è in media di 10 volte. È possibile eseguire allo stesso tempo analisi sull'indice columnstore e transazioni sull'indice rowstore. Il columnstore viene aggiornato quando i dati nella tabella rowstore vengono modificati. In questo modo entrambi gli indici possono usare gli stessi dati.  
  
A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile avere uno o più indici rowstore non cluster su un indice columnstore ed eseguire ricerche efficienti in tabelle nel columnstore sottostante. Sono disponibili anche altre opzioni. È possibile, ad esempio, applicare un vincolo di chiave primaria tramite un vincolo UNIQUE nella tabella rowstore. Dato che non è possibile inserire un valore non univoco nella tabella rowstore, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può inserire il valore nel columnstore.  
  
## <a name="metadata"></a>Metadati  
Tutte le colonne di un indice columnstore vengono archiviate nei metadati come colonne incluse. L'indice columnstore non contiene colonne chiave.  

:::row:::
    :::column:::
        [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
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
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 

## <a name="related-tasks"></a>Attività correlate  
Tutte le tabelle relazionali, se non specificate come indice columnstore cluster, usano rowstore come formato dei dati sottostanti. `CREATE TABLE` crea una tabella rowstore, a meno che non si specifichi l'opzione `WITH CLUSTERED COLUMNSTORE INDEX`.  
  
Quando si crea una tabella con l'istruzione `CREATE TABLE` è possibile creare la tabella come columnstore specificando l'opzione `WITH CLUSTERED COLUMNSTORE INDEX`. Se si ha già una tabella rowstore e si vuole convertirla in una tabella columnstore, è possibile usare l'istruzione `CREATE COLUMNSTORE INDEX`.  
  
|Attività|Argomenti di riferimento|Note|  
|----------|----------------------|-----------|  
|Creare una tabella come columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]è possibile creare la tabella come indice columnstore cluster. Non è necessario creare prima una tabella rowstore e quindi convertirla in columnstore.|  
|Creare una tabella in memoria con un indice columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile creare una tabella in memoria ottimizzata con un indice columnstore. L'indice columnstore può anche essere aggiunto dopo aver creato la tabella, usando la sintassi `ALTER TABLE ADD INDEX`.|  
|Convertire una tabella rowstore in un columnstore.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Convertire un heap o un albero binario esistente o in un columnstore. Gli esempi illustrano come gestire gli indici esistenti e il nome dell'indice quando si esegue questa conversione.|  
|Convertire una tabella columnstore in un rowstore.|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) oppure [Convertire una tabella columnstore di nuovo in un heap rowstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |Di solito non è necessario eseguire questa conversione, ma talvolta potrebbe presentarsene la necessità. Gli esempi illustrano come convertire un columnstore in un heap o in un indice cluster.|  
|Creare un indice columnstore per una tabella rowstore.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Una tabella rowstore può avere un solo indice columnstore. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]l'indice columnstore può avere una condizione di filtro. Gli esempi illustrano la sintassi di base.|  
|Creare indici ad alte prestazioni per l'analisi operativa.|[Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Descrive come creare indici columnstore e BTree complementari in modo che le query OLTP usino gli indici BTree e le query di analisi usino gli indici columnstore.|  
|Creare indici columnstore efficienti per il data warehousing.|[Indici columnstore per il data warehousing](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Descrive come usare gli indici BTree con le tabelle columnstore per creare query di data warehousing ad alte prestazioni.|  
|Usare un indice BTree per imporre un vincolo di chiave primaria per un indice columnstore.|[Indici columnstore per il data warehousing](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Illustra come combinare indici BTree e columnstore per imporre vincoli di chiave primaria per l'indice columnstore.|  
|Rimuovere un indice columnstore.|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|Per rimuovere un indice columnstore si usa la sintassi `DROP INDEX` standard usata dagli indici BTree. La rimozione di un indice columnstore cluster converte la tabella columnstore in un heap.|  
|Eliminare una riga da un indice columnstore.|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Usare [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) per eliminare una riga.<br /><br /> **Riga columnstore**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contrassegna la riga come eliminata logicamente ma recupera lo spazio di archiviazione fisico della riga solo dopo che l'indice è stato ricompilato.<br /><br /> **Riga deltastore**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina la riga logicamente e fisicamente.|  
|Aggiornare una riga nell'indice columnstore.|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Usare [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) per aggiornare una ruga.<br /><br /> **Riga columnstore**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contrassegna la riga come eliminata logicamente e quindi inserisce la riga aggiornata nel deltastore.<br /><br /> **Riga deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiorna la riga nel deltastore.|  
|Caricare dati in un indice columnstore.|[Caricamento dei dati di indici columnstore](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)||  
|Forzare il passaggio di tutte le righe del deltastore nel columnstore.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) ... `REBUILD`<br /><br /> [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|`ALTER INDEX` con l'opzione `REBUILD` forza il passaggio di tutte le righe nel columnstore.|  
|Deframmentare un indice columnstore.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|`ALTER INDEX ... REORGANIZE` consente di deframmentare indici columnstore online.|  
|Unire tabelle con indici columnstore.|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)||  
  
## <a name="see-also"></a>Vedere anche  
 [Caricamento dei dati di indici columnstore](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Riepilogo delle funzionalità con versione degli indici columnstore](~/relational-databases/indexes/columnstore-indexes-what-s-new.md)   
 [Prestazioni delle query su indici columnstore](~/relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Indici columnstore per il data warehousing](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Deframmentazione degli indici columnstore](~/relational-databases/indexes/columnstore-indexes-defragmentation.md)   
 [Guida per la progettazione di indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
 [Architettura degli indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
  
  
