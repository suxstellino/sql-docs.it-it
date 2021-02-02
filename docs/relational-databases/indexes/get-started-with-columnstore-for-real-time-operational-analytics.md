---
description: Introduzione a columnstore per l'analisi operativa in tempo reale
title: Introduzione a columnstore per l'analisi operativa in tempo reale | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: quickstart
ms.assetid: e1328615-6b59-4473-8a8d-4f360f73187d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbc0867b71cadb33b1eb9174132b2031134a72f0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236943"
---
# <a name="get-started-with-columnstore-for-real-time-operational-analytics"></a>Introduzione a columnstore per l'analisi operativa in tempo reale
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] introduce l'analisi operativa in tempo reale, cioè la possibilità di eseguire contemporaneamente analisi e carichi di lavoro OLTP nelle stesse tabelle di database. Oltre a eseguire analisi in tempo reale, è possibile eliminare la necessità di ETL e di un data warehouse.  
  
## <a name="real-time-operational-analytics-explained"></a>Descrizione dell'analisi operativa in tempo reale  
 In passato le aziende usavano sistemi separati per i carichi di lavoro operativi, ad esempio OLTP, e per quelli di analisi. Per questi sistemi, i processi di estrazione, trasformazione e caricamento (ETL) spostano regolarmente i dati dall'archivio operativo in un archivio di analisi. I dati di analisi sono vengono in genere archiviati in un data warehouse o data mart dedicato all'esecuzione di query di analisi. Anche se questa soluzione ha rappresentato lo standard, presentava tre problemi principali:  
  
-   **Complessità.** L'implementazione di ETL può richiedere una notevole quantità di codifica, soprattutto per caricare solo le righe modificate. L'identificazione delle righe che sono state modificate può risultare difficile.  
  
-   **Costo.** L'implementazione di ETL richiede il costo di acquisto di licenze software e hardware aggiuntive.  
  
-   **Latenza dei dati.** L'implementazione di ETL aggiunge un ritardo per l'esecuzione delle analisi. Se, ad esempio, il processo ETL viene eseguito al termine di ogni giornata lavorativa, le query di analisi verranno eseguite sui dati che risalgono ad almeno un giorno prima. Per molte aziende questo ritardo è inaccettabile, perché le loro attività dipendono dall'analisi dei dati in tempo reale. Ad esempio, il rilevamento di frodi richiede l'analisi in tempo reale sui dati operativi.  
  
 ![Panoramica dell'analisi operativa in tempo reale](../../relational-databases/indexes/media/real-time-operational-analytics-overview.png "Panoramica dell'analisi operativa in tempo reale")  
  
L'analisi operativa in tempo reale offre una soluzione a questi problemi.   
Non si verifica alcun ritardo durante l'esecuzione di analisi e carichi di lavoro OLTP nella stessa tabella sottostante.   Per gli scenari in cui è possibile usare l'analisi in tempo reale, i costi e la complessità vengono notevolmente ridotti eliminando la necessità di ETL e di acquistare e gestire un data warehouse separato.  
  
> [!NOTE]  
>  L'analisi operativa in tempo reale è destinata allo scenario di una singola origine dati, ad esempio un'applicazione ERP (Enterprise Resource Planning) in cui è possibile eseguire sia i carichi di lavoro operativi che i carichi di lavoro di analisi. Ciò non sostituisce la necessità di un data warehouse separato quando è necessario integrare dati da più origini prima di eseguire il carico di lavoro di analisi o quando sono necessarie prestazioni delle analisi estremamente elevate usando dati preaggregati, ad esempio cubi.  
  
 L'analisi in tempo reale usa un indice columnstore aggiornabile in una tabella rowstore. L'indice columnstore gestisce una copia dei dati, quindi i carichi di lavoro OLTP e di analisi vengono eseguiti su copie separate dei dati. In questo modo si riduce al minimo l'impatto sulle prestazioni causato dall'esecuzione contemporanea di entrambi i carichi di lavoro. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestisce automaticamente le modifiche di indice in modo che le modifiche OLTP siano sempre aggiornate per l'analisi. Con questa progettazione, è possibile e utile eseguire l'analisi in tempo reale su dati aggiornati. Ciò funziona sia per le tabelle basate su disco che per le tabelle ottimizzate per la memoria.  
  
## <a name="get-started-example"></a>Esempio introduttivo  
 Per iniziare a usare l'analisi in tempo reale:  
  
1.  Nello schema operativo identificare le tabelle che contengono i dati necessari per l'analisi.  
  
2.  Per ogni tabella eliminare tutti gli indici Btree progettati principalmente per velocizzare l'analisi esistente sul carico di lavoro OLTP. Sostituirli con un indice columnstore singolo.  Questo può migliorare le prestazioni complessive del carico di lavoro OLTP perché saranno presenti meno indici da gestire.  
  
    ```sql  
    --This example creates a nonclustered columnstore index on an existing OLTP table.  
    --Create the table  
    CREATE TABLE t_account (  
        accountkey int PRIMARY KEY,  
        accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int  
    );  
  
    --Create the columnstore index with a filtered condition  
    CREATE NONCLUSTERED COLUMNSTORE INDEX account_NCCI   
    ON t_account (accountkey, accountdescription, unitsold)   
    ;  
    ```  
  
     L'indice columnstore in una tabella in memoria consente l'analisi operativa grazie all'integrazione di tecnologie OLTP in memoria e columnstore in memoria per fornire prestazioni elevate per carichi di lavoro OLTP e di analisi. L'indice columnstore in una tabella in memoria deve includere tutte le colonne.  
  
    ```sql  
    -- This example creates a memory-optimized table with a columnstore index.  
    CREATE TABLE t_account (  
        accountkey int NOT NULL PRIMARY KEY NONCLUSTERED,  
        Accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int,  
        INDEX t_account_cci CLUSTERED COLUMNSTORE  
        )  
        WITH (MEMORY_OPTIMIZED = ON );  
    GO  
  
    ```  
  
3.  Non è necessario eseguire altre operazioni.  

 A questo punto si è pronti per eseguire l'analisi operativa in tempo reale senza apportare modifiche all'applicazione.  Le query di analisi verranno eseguite sull'indice columnstore e le operazioni OLTP continueranno a essere eseguite su indici Btree OLTP. I carichi di lavoro OLTP continueranno a essere eseguiti, ma la gestione dell'indice columnstore determinerà un sovraccarico aggiuntivo. Vedere le ottimizzazioni delle prestazioni nella sezione successiva.  
  
## <a name="blog-posts"></a>Post di blog  
 Leggere i post di blog seguenti per altre informazioni sull'analisi operativa in tempo reale. La lettura preliminare del blog potrebbe semplificare la comprensione delle sezioni relative ai suggerimenti sulle prestazioni.  
  
-   [Business case per l'analisi operativa in tempo reale](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)  
  
-   [Uso di un indice columnstore non cluster per l'analisi operativa in tempo reale](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index)  
  
-   [Un semplice esempio di uso di indice columnstore non cluster](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)  
  
-   [In che modo SQL Server gestisce un indice columnstore non cluster in un carico di lavoro transazionale](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)  
  
-   [Uso di un indice filtrato per ridurre al minimo l'impatto della manutenzione di un indice columnstore non cluster](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
-   [Uso di un ritardo di compressione per ridurre al minimo l'impatto della manutenzione di un indice columnstore non cluster](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)  
  
-   [Uso di un ritardo di compressione/dei numeri relativi alle prestazioni per ridurre al minimo l'impatto della manutenzione di un indice columnstore non cluster](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)  
  
-   [Analisi operativa in tempo reale con tabelle ottimizzate per la memoria](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)  
  
-   [L'indice columnstore e criteri di unione per rowgroup](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)  
  
## <a name="performance-tip-1-use-filtered-indexes-to-improve-query-performance"></a>Suggerimento per le prestazioni n. 1: usare indici filtrati per migliorare le prestazioni delle query  
 L'esecuzione dell'analisi operativa in tempo reale può compromettere le prestazioni del carico di lavoro OLTP. Tale impatto dovrebbe essere minimo. L'esempio mostra come usare gli indici filtrati per ridurre al minimo l'impatto dell'indice columnstore non cluster sul carico di lavoro transazionale offrendo comunque analisi in tempo reale.  
  
 Per ridurre al minimo l'overhead di gestione di un indice columnstore non cluster in un carico di lavoro operativo, è possibile usare una condizione filtrata per creare un indice columnstore non cluster solo per i dati *meno attivi* o a modifica lenta. Ad esempio, in un'applicazione di gestione degli ordini è possibile creare un indice columnstore non cluster negli ordini che sono già stati spediti. Dopo la spedizione, raramente l'ordine viene modificato e quindi i dati possono essere considerati meno attivi. Con l'indice filtrato i dati nell'indice columnstore non cluster richiedono meno aggiornamenti, riducendo in tal modo l'impatto sul carico di lavoro transazionale.  
  
 Le query di analisi accedono in modo trasparente sia ai dati meno attivi che ai dati attivi in base alle esigenze per fornire analisi in tempo reale. Se una parte importante del carico di lavoro operativo riguarda la gestione dei dati ad accesso frequente, le operazioni non richiederanno ulteriore manutenzione dell'indice columnstore. Una procedura consigliata prevede la creazione di un indice cluster rowstore per le colonne usate nella definizione dell'indice filtrato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa l'indice cluster per analizzare rapidamente le righe che non soddisfano la condizione filtrata. Senza questo indice cluster, sarà necessario eseguire una scansione completa della tabella rowstore per trovare le righe che possono esercitare un elevato impatto negativo sulle prestazioni di una query di analisi. In assenza di un indice cluster si potrebbe creare un indice Btree non cluster filtrato complementare per identificare le righe, ma questa scelta non è consigliabile perché l'accesso a un ampio intervallo di righe usando indici Btree non cluster comporta costi elevati.  
  
> [!NOTE]  
>  Un indice columnstore non cluster filtrato è supportato solo nelle tabelle basate su disco. Non è supportato nelle tabelle ottimizzate per la memoria.  
  
### <a name="example-a-access-hot-data-from-btree-index-warm-data-from-columnstore-index"></a>Esempio A: accesso a dati attivi da un indice Btree, dati meno attivi dall'indice columnstore  
 Questo esempio usa una condizione filtrata (accountkey > 0) per stabilire quali righe saranno presenti nell'indice columnstore. L'obiettivo è di progettare la condizione filtrata e le query successive per accedere a dati attivi, caratterizzati da modifiche frequenti, dall'indice Btree e di accedere ai dati meno attivi, che sono più stabili, dall'indice columnstore.  
  
 ![Indici combinati per dati ad accesso frequente e molto frequente](../../relational-databases/indexes/media/de-columnstore-warmhotdata.png "Indici combinati per dati ad accesso frequente e molto frequente")  
  
> [!NOTE]  
>  Query Optimizer prenderà in considerazione, ma non sempre sceglierà, l'indice columnstore per il piano di query. Quando Query Optimizer sceglie l'indice columnstore filtrato, combina in modo trasparente le righe dall'indice columnstore nonché le righe che non soddisfano la condizione filtrata per consentire l'analisi in tempo reale. Si tratta di un indice diverso da un normale indice filtrato non cluster che può essere usato solo nelle query limitate alle righe presenti nell'indice.  
  
```sql  
--Use a filtered condition to separate hot data in a rowstore table  
-- from "warm" data in a columnstore index.  
  
-- create the table  
CREATE TABLE  orders (  
         AccountKey         int not null,  
         Customername       nvarchar (50),  
        OrderNumber         bigint,  
        PurchasePrice       decimal (9,2),  
        OrderStatus         smallint not null,  
        OrderStatusDesc     nvarchar (50))  
  
-- OrderStatusDesc  
-- 0 => 'Order Started'  
-- 1 => 'Order Closed'  
-- 2 => 'Order Paid'  
-- 3 => 'Order Fullfillment Wait'  
-- 4 => 'Order Shipped'  
-- 5 => 'Order Received'  
  
CREATE CLUSTERED INDEX  orders_ci ON orders(OrderStatus)  
  
--Create the columnstore index with a filtered condition  
CREATE NONCLUSTERED COLUMNSTORE INDEX orders_ncci ON orders  (accountkey, customername, purchaseprice, orderstatus)  
where orderstatus = 5  
;  
  
-- The following query returns the total purchase done by customers for items > $100 .00  
-- This query will pick  rows both from NCCI and from 'hot' rows that are not part of NCCI  
SELECT top 5 customername, sum (PurchasePrice)  
FROM orders  
WHERE purchaseprice > 100.0   
Group By customername  
```  
  
 La query di analisi verrà eseguita con il piano di query seguente. Come si può notare, è possibile accedere alle righe che non soddisfano la condizione filtrata solo usando l'indice Btree cluster.  
  
 ![Piano di query](../../relational-databases/indexes/media/query-plan-columnstore.png "Piano di query")  
  
 Consultare il blog per informazioni dettagliate sull' [indice columnstore non cluster filtrato.](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
## <a name="performance-tip-2-offload-analytics-to-always-on-readable-secondary"></a>Suggerimento per le prestazioni n. 2: offload dell'analisi a una replica secondaria leggibile AlwaysOn  
 Anche se è possibile ridurre al minimo la manutenzione degli indici columnstore usando un indice columnstore filtrato, le query di analisi possono richiedere comunque risorse di calcolo elevate (CPU, I/O, memoria) che influiscono sulle prestazioni del carico di lavoro operativo. Per i carichi di lavoro maggiormente mission-critical, si consiglia di usare la configurazione AlwaysOn. In questa configurazione è possibile eliminare l'impatto dell'esecuzione dell'analisi eseguendone l'offload a una replica secondaria leggibile.  
  
## <a name="performance-tip-3-reducing-index-fragmentation-by-keeping-hot-data-in-delta-rowgroups"></a>Suggerimento per le prestazioni n. 3: riduzione della frammentazione dell'indice, mantenendo i dati attivi nei rowgroup delta  
 Le tabelle con indice columnstore possono subire una frammentazione elevata (righe eliminate) se il carico di lavoro aggiorna o elimina le righe che sono state compresse. Un indice columnstore frammentato determina un utilizzo inefficiente della memoria o dell'archiviazione. Oltre all'uso inefficiente delle risorse, influisce negativamente sulle prestazioni delle query di analisi a causa dell'I/O aggiuntivo e della necessità di filtrare le righe eliminate dal set di risultati.  
  
 Le righe eliminate non vengono fisicamente rimosse fino a quando non si esegue la deframmentazione degli indici con il comando `REORGANIZE` o si ricompila l'indice columnstore nell'intera tabella o nelle partizioni interessate. Sia `REORGANIZE` che `REBUILD` sono operazioni di indice dispendiose in termini di risorse che diversamente potrebbero essere usate per il carico di lavoro. Inoltre, se le righe vengono compresse troppo presto, potrebbe essere necessario ricomprimerle più volte a causa di aggiornamenti che determinano un overhead di compressione inutilizzata.  
È possibile ridurre al minimo la frammentazione dell'indice usando l'opzione `COMPRESSION_DELAY`.  
  
```sql  
-- Create a sample table  
CREATE TABLE t_colstor (  
               accountkey                      int not null,  
               accountdescription              nvarchar (50) not null,  
               accounttype                     nvarchar(50),  
               accountCodeAlternatekey         int)  
  
-- Creating nonclustered columnstore index with COMPRESSION_DELAY. The columnstore index will keep the rows in closed delta rowgroup for 100 minutes   
-- after it has been marked closed  
CREATE NONCLUSTERED COLUMNSTORE index t_colstor_cci on t_colstor (accountkey, accountdescription, accounttype)   
                       WITH (DATA_COMPRESSION= COLUMNSTORE, COMPRESSION_DELAY = 100);  
  
;  
```  
  
 Consultare il blog per informazioni dettagliate sul [ritardo di compressione](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci).  
  
 Le procedure consigliate sono le seguenti:  
  
-   **Carico di lavoro costituito da inserimento/query:** se il carico di lavoro è costituito principalmente dall'inserimento di dati e dall'esecuzione di query su tali dati, il valore predefinito 0 per COMPRESSION_DELAY è l'opzione consigliata. Le nuove righe inserite verranno compresse dopo l'inserimento di 1 milione di righe in un singolo rowgroup delta.  
    Alcuni esempi di tale carico di lavoro sono (a) carico di lavoro di data warehouse tradizionale (b) analisi del flusso di clic quando è necessario analizzare lo schema dei clic in un'applicazione Web.  
  
-   **Carico di lavoro OLTP:** se il carico di lavoro comporta un numero elevato di istruzioni DML, ovvero una combinazione di numerose istruzioni Update, Delete e Insert, è possibile osservare la frammentazione dell'indice columnstore esaminando DMV sys. dm_db_column_store_row_group_physical_stats. Se si nota che una percentuale inferiore al 10% delle righe è contrassegnata come eliminata in rowgroup compressi di recente, è possibile usare l'opzione COMPRESSION_DELAY per aggiungere un ritardo quando le righe diventano idonee per la compressione. Se, ad esempio, il carico di lavoro appena inserito rimane attivo (ovvero viene aggiornato più volte) per 60 minuti, è consigliabile scegliere 60 come COMPRESSION_DELAY.  
  
 È probabile che nella maggior parte dei casi i clienti non debbano effettuare alcuna azione. Il valore predefinito dell'opzione COMPRESSION_DELAY dovrebbe essere sufficiente.  
Agli utenti avanzati si consiglia di eseguire la query seguente e di raccogliere la percentuale delle righe eliminate negli ultimi 7 giorni.  
  
```sql  
SELECT row_group_id,cast(deleted_rows as float)/cast(total_rows as float)*100 as [% fragmented], created_time  
FROM sys. dm_db_column_store_row_group_physical_stats  
WHERE object_id = object_id('FactOnlineSales2')   
             AND  state_desc='COMPRESSED'   
             AND deleted_rows>0   
             AND created_time > GETDATE() - 7  
ORDER BY created_time DESC;  
```  
  
 Se il numero di righe eliminate in rowgroup compressi è maggiore del 20%, per stabilizzare i rowgroup precedenti con variante inferiore al 5% (indicati come rowgroup inattivi) impostare COMPRESSION_DELAY = (youngest_rowgroup_created_time - current_time). Si noti che questo approccio funziona in modo ottimale con un carico di lavoro stabile e relativamente omogeneo.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida agli indici columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)   
 [Caricamento dati di indici columnstore](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Prestazioni delle query per gli indici columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Indici columnstore per il data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
