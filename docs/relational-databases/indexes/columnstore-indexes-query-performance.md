---
description: Indici columnstore - Prestazioni delle query
title: Indici columnstore - Prestazioni delle query | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 83acbcc4-c51e-439e-ac48-6d4048eba189
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f2fa166f15ebe07ad476a845af35b759b0d30bde
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744281"
---
# <a name="columnstore-indexes---query-performance"></a>Indici columnstore - Prestazioni delle query

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indicazioni per ottenere prestazioni delle query elevate che è possibile raggiungere con la progettazione degli indici columnstore.    
    
 Gli indici columnstore possono raggiungere un miglioramento fino a 100 volte in termini di prestazioni per le analisi e i carichi di lavoro di data warehouse e fino a 10 volte in termini di compressione dei dati rispetto agli indici rowstore tradizionali. Queste indicazioni consentono di ottenere le elevate prestazioni delle query per le quali sono progettati gli indici columnstore. Alla fine dell'articolo vengono fornite ulteriori spiegazioni sulle prestazioni di columnstore.    
    
## <a name="recommendations-for-improving-query-performance"></a>Indicazioni per migliorare le prestazioni delle query    
 Di seguito sono elencate alcune indicazioni per ottenere prestazioni elevate con gli indici columnstore.    
    
### <a name="1-organize-data-to-eliminate-more-rowgroups-from-a-full-table-scan"></a>1. Organizzare i dati per eliminare più rowgroup da una scansione di tabella completa    
    
-   **Usare l'ordine di inserimento.** In genere in un data warehouse tradizionale i dati vengono inseriti in ordine temporale e le analisi vengono eseguite in una dimensione temporale, come nel caso delle analisi delle vendite per trimestre. Per questo tipo di carico di lavoro, l'eliminazione del rowgroup viene eseguita automaticamente. In [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] diversi rowgroup vengono ignorati durante l'elaborazione della query.    
    
-   **Usare l'indice rowstore cluster.** Se il predicato della query comune è in una colonna (ad esempio C1) non correlata all'ordine di inserimento della riga, è possibile creare un indice rowstore cluster nelle colonne C1 e quindi creare l'indice columstore cluster eliminando l'indice rowstore cluster. Se si crea l'indice columnstore cluster in modo esplicito usando `MAXDOP = 1`, l'indice risultante verrà ordinato perfettamente in base alla colonna C1. Se si specifica `MAXDOP = 8`, si avrà una sovrapposizione di valori in otto rowgroup. Un caso comune di applicazione di questa strategia riguarda l'indice columnstore creato inizialmente con un set di dati di grandi dimensioni. Per un indice columnstore non cluster (NCCI), se la tabella rowstore di base include un indice cluster le righe risultano già ordinate. In questo caso, l'indice columnstore non cluster risultante sarà ordinato automaticamente. È importante notare che l'indice columnstore non mantiene automaticamente l'ordine delle righe. Quando vengono inserite nuove righe o vengono aggiornate righe meno recenti, potrebbe essere necessario ripetere il processo a causa di un possibile deterioramento delle prestazioni delle query di analisi.    
    
-   **Usare il partizionamento delle tabelle.** È possibile partizionare l'indice columnstore e quindi usare l'eliminazione delle partizioni per ridurre il numero di rowgroup da analizzare. Se ad esempio si ha una tabella dei fatti in cui vengono archiviati gli acquisti eseguiti dai clienti e si usa un modello di query comune per cercare gli acquisti eseguiti trimestralmente da un determinato cliente, è possibile combinare l'ordine di inserimento con il partizionamento nella colonna del cliente. Ogni partizione contiene righe in ordine temporale per un determinato cliente. Se è necessario rimuovere i dati dal columnstore, valutare anche la possibilità di usare il partizionamento delle tabelle. La disattivazione e il troncamento delle partizioni non più necessarie è una strategia efficace per eliminare i dati senza generare la frammentazione introdotta con rowgroup più piccoli.    

-   **Evitare di eliminare quantità elevate di dati**. La rimozione di righe compresse da un rowgroup non è un'operazione sincrona. Sarebbe costoso decomprimere un rowgroup, eliminare la riga e quindi ricomprimerla. Se si eliminano dati da rowgroup compressi, tali rowgroup verranno comunque analizzati anche se restituiranno un minor numero di righe. Se il numero di righe eliminate per diversi rowgroup è sufficientemente grande da consentire l'unione in un minor numero di rowgroup, la riorganizzazione del columnstore aumenta la qualità dell'indice e le prestazioni delle query migliorano. Se il processo di eliminazione dei dati in genere svuota interi rowgroup, valutare la possibilità di usare il partizionamento delle tabelle, disattivare le partizioni non più necessarie e troncarle anziché eliminare righe. 

    > [!NOTE]
    > A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il motore di tuple viene aiutato da un'attività di unione in background, che comprime automaticamente i rowgroup delta OPEN più piccoli che sono esistiti per un dato periodo di tempo (come determinato da una soglia interna) oppure unisce i rowgroup COMPRESSED da cui è stato eliminato un numero elevato di righe. Ciò migliora la qualità dell'indice columnstore nel tempo.   
    > Se è necessario eliminare grandi quantità di dati dall'indice columnstore, valutare la possibilità di suddividere nel tempo tale operazione in batch di eliminazione più piccoli, per consentire all'attività di unione in background di gestire l'unione di rowgroup più piccoli e migliorare la qualità dell'indice, eliminando la necessità di pianificare le finestre di manutenzione per la riorganizzazione dell'indice dopo l'eliminazione dei dati.    
    > Per altre informazioni sui termini e sui concetti dei columnstore, vedere [Indici columnstore - Panoramica](../../relational-databases/indexes/columnstore-indexes-overview.md).
    
### <a name="2-plan-for-enough-memory-to-create-columnstore-indexes-in-parallel"></a>2. Pianificare una quantità di memoria sufficiente per creare indici columnstore in parallelo    
 Per impostazione predefinita, la creazione di un indice columnstore è un'operazione parallela, a meno che la memoria non sia vincolata. La creazione dell'indice in parallelo richiede più memoria rispetto alla creazione dell'indice in modo seriale. Se si dispone di un'ampia quantità di memoria, la creazione di un indice columnstore richiede un tempo di circa 1,5 volte superiore rispetto alla compilazione di un albero B nelle stesse colonne.    
    
 La memoria richiesta per la creazione di un indice columnstore dipende dal numero di colonne, dal numero di colonne stringa, dal grado di parallelismo e dalle caratteristiche dei dati. Se ad esempio la tabella contiene meno di un milione di righe, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa un solo thread per creare l'indice columnstore.    
    
 Se la tabella dispone di più di un milione di righe, ma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non dispone di memoria sufficiente per creare l'indice usando MAXDOP, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riduce automaticamente `MAXDOP` in base alle esigenze per adattarsi alla memoria disponibile.  In alcuni casi, è necessario ridurre il grado di parallelismo a uno per compilare l'indice nella memoria vincolata.    
    
 A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] la query viene eseguita sempre in modalità batch. Nelle versioni precedenti l'esecuzione batch viene usata solo quando DOP è maggiore di uno.    
    
## <a name="columnstore-performance-explained"></a>Spiegazione delle prestazioni columnstore    
 Gli indici columnstore ottengono prestazioni di query ottimali combinando l'elaborazione in memoria in modalità batch ad alta velocità con tecniche che riducono significativamente i requisiti I/O. Poiché le query di analisi analizzano un numero elevato di righe, in genere sono associate alle operazioni I/O, quindi la riduzione di tali operazioni durante l'esecuzione delle query è fondamentale per la progettazione di indici columnstore. Dopo la lettura dei dati in memoria, è molto importante ridurre il numero di operazioni in memoria.    
    
 Gli indici columnstore riducono le operazioni I/O e ottimizzano le operazioni in memoria grazie all'elevata compressione dei dati, all'eliminazione di columnstore, all'eliminazione di rowgroup e all'elaborazione batch.    
    
### <a name="data-compression"></a>Compressione dei dati    
 Gli indici columnstore raggiungono una compressione dei dati 10 volte superiore a quella degli indici rowstore. Ciò riduce significativamente le operazioni I/O richieste per eseguire query di analisi e di conseguenza migliora le prestazioni delle query.    
    
-   Gli indici columnstore leggono i dati compressi dal disco, quindi il numero di byte che deve essere letto nella memoria risulta ridotto.    
    
-   Gli indici columnstore archiviano i dati in memoria in formato compresso. La conseguente riduzione del numero di letture in memoria degli stessi dati riduce a sua volta le operazioni I/O. Ad esempio, con una compressione 10x, gli indici columnstore possono mantenere in memoria volumi di dati 10 volte maggiori di quelli archiviati in formato non compresso. La presenza di più dati nella memoria incrementa la probabilità che l'indice columnstore trovi i dati necessari nella memoria, senza ulteriori letture dal disco.    
    
-   Gli indici columnstore comprimono i dati per colonne anziché per righe. Questo consente di ottenere percentuali di compressione maggiori e di ridurre le dimensioni dei dati archiviati su disco. Ogni colonna viene compressa e archiviata in modo indipendente.  Tutti i dati all'interno di una colonna sono sempre dello tipo e tendono ad avere valori simili. Le tecniche di compressione dei dati sono molto efficaci per raggiungere percentuali di compressione maggiori quando i valori sono simili.    
    
-   Se ad esempio una tabella dei fatti archivia gli indirizzi dei clienti e include una colonna per il paese, il numero totale di valori possibili è inferiore a 200. Alcuni di questi valori saranno ripetuti più volte. Se la tabella dei fatti contiene 100 milioni di righe, la colonna per il paese verrà compressa facilmente e non richiederà molto spazio di archiviazione. La compressione riga per riga non riesce a sfruttare appieno la somiglianza dei valori di colonna e userà più byte per comprimere i valori nella colonna per il paese.    
    
### <a name="column-elimination"></a>Eliminazione di colonne    
 Gli indici columnstore non leggono le colonne non rilevanti ai fini della query. Questa capacità, denominata eliminazione di colonne, riduce ulteriormente le operazioni I/O per l'esecuzione delle query e pertanto migliora le prestazioni delle query.    
    
-   L'eliminazione di colonne è possibile perché i dati sono organizzati e compressi colonna per colonna. Al contrario, quando i dati sono archiviati riga per riga, i valori della colonna in ogni riga vengono archiviati fisicamente insieme e non possono essere separati facilmente. Query Processor deve leggere un'intera riga per recuperare valori di colonna specifici, aumentando le operazioni I/O con letture in memoria non necessarie di dati superflui.    
    
-   Ad esempio, se una tabella contiene 50 colonne e la query ne usa solo 5, l'indice columnstore recupera solo le 5 colonne rilevanti dal disco. Non legge i dati delle altre 45 colonne. In questo modo presupponendo che tutte le colonne abbiano dimensioni simili, le operazioni I/O vengono ridotte di un ulteriore 90%. Se gli stessi dati vengono archiviati in un rowstore, Query Processor deve leggere le altre 45 colonne.    
    
### <a name="rowgroup-elimination"></a>Eliminazione di rowgroup    
 Per le scansioni di tabelle complete, un'alta percentuale di dati spesso non corrisponde ai criteri del predicato della query. Usando i metadati, l'indice columnstore può ignorare la lettura nei rowgroup che non contengono dati necessari per il risultato della query e quindi evitare di eseguire le operazioni I/O corrispondenti. Questa capacità, denominata eliminazione di rowgroup, riduce le operazioni I/O per le scansioni di tabella complete e di conseguenza migliora le prestazioni delle query.    
    
 **Quando è necessario che un indice columnstore esegua una scansione di tabella completa?**    
    
 A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile creare uno o più indici albero B non cluster normali in un indice columnstore cluster con una procedura analoga a quella per un heap rowstore. Gli indici albero B non cluster possono velocizzare una query con un predicato di uguaglianza o un predicato con un intervallo di valori limitato. Per i predicati più complessi, Query Optimizer potrebbe optare per una scansione di tabella completa. Senza la possibilità di ignorare i rowgroup, una scansione di tabella completa richiederebbe molto tempo, in particolare per tabelle di grandi dimensioni.    
    
 **In che occasioni una query di analisi trae vantaggio dall'eliminazione di rowgroup per una scansione di tabella completa?**    
    
 Ad esempio, un'azienda di vendita al dettaglio ha modellato i dati di vendita usando una tabella dei fatti con un indice columnstore cluster. Per ogni nuova vendita vengono archiviati diversi attributi della transazione inclusa la data della vendita di un articolo. È interessante notare che anche se gli indici columnstore non garantiscono l'ordinamento, le righe in questa tabella vengono caricate ordinate per data. Questa tabella si espande nel tempo. Anche se l'azienda di vendita al dettaglio conserva i dati di vendita degli ultimi 10 anni, per una query di analisi potrebbe essere necessario solo il calcolo di un'aggregazione dell'ultimo trimestre. Gli indici columnstore possono eliminare l'accesso ai dati dei 39 trimestri precedenti semplicemente esaminando i metadati per la colonna della data. Si tratta di un'ulteriore riduzione del 97% della quantità di dati letti in memoria ed elaborati.    
    
 **Quali rowgroup vengono ignorati in una scansione di tabella completa?**    
    
 Per determinare quali rowgroup eliminare, l'indice columnstore usa i metadati per archiviare i valori minimi e massimi di ogni segmento di colonna per ogni rowgroup. Se nessuno degli intervalli dei segmenti di colonna soddisfa i criteri del predicato della query, l'intero rowgroup viene ignorato senza eseguire alcuna operazione I/O. Questo procedimento funziona perché in genere i dati vengono caricati con un ordinamento e, anche se l'ordinamento delle righe non è sempre garantito, i valori dei dati simili spesso si trovano all'interno dello stesso rowgroup o in un rowgroup adiacente.    
    
 Per altri dettagli sui rowgroup, vedere [Linee guida per la progettazione di indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index).    
    
### <a name="batch-mode-execution"></a>Esecuzione in modalità batch    
 Il termine [esecuzione in modalità batch](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) indica l'elaborazione congiunta di un set di righe, generalmente non più di 900, per migliorare l'efficienza di esecuzione. Ad esempio, la query `SELECT SUM (Sales) FROM SalesData` aggrega le vendite totali della tabella SalesData. Nell'esecuzione in modalità batch, il motore di esecuzione delle query calcola l'aggregato in gruppi di 900 valori. In questo modo, invece di pagare il costo delle singole righe, i metadati, i costi di accesso e altri tipi di costi generali vengono suddivisi su tutte le righe in un batch, riducendo notevolmente il percorso del codice. L'elaborazione in modalità batch funziona sui dati compressi, quando disponibili, ed elimina alcuni degli operatori di scambio usati dall'elaborazione in modalità riga. Questo velocizza l'esecuzione delle query di analisi per ordini di grandezza.    
    
 Non tutti gli operatori di esecuzione delle query possono essere eseguiti in modalità batch. Ad esempio, le operazioni DML di inserimento, eliminazione o aggiornamento vengono eseguite una riga alla volta. Gli operatori in modalità batch fanno riferimento agli operatori per velocizzare le prestazioni delle query in operazioni di analisi, join, aggregazione, ordinamento e così via. Dall'introduzione dell'indice columnstore in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] si sta lavorando costantemente all'incremento degli operatori eseguibili in modalità batch. La tabella seguente visualizza gli operatori eseguibili in modalità batch in base alla versione del prodotto.    
    
|Operatori in modalità batch|Quando si usa?|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<sup>1</sup>|Commenti|    
|---------------------------|------------------------|---------------------|---------------------|---------------------------------------|--------------|    
|Operazioni DML (insert, delete, update, merge)||no|no|no|DML non è un'operazione in modalità batch perché non è parallela. Anche quando si abilita l'elaborazione batch in modalità seriale e si consente l'elaborazione in modalità batch di DML, non si rilevano vantaggi significativi.|    
|Index Scan columnstore|SCAN|ND|sì|sì|Per gli indici columnstore è possibile eseguire il push del predicato nel nodo SCAN.|    
|Index Scan (nonclustered) columnstore|SCAN|sì|sì|sì|sì|    
|Index Seek||ND|ND|no|Si esegue un'operazione di ricerca con un indice albero B non cluster in modalità riga.|    
|Compute Scalar|Espressione che restituisce un valore scalare.|sì|sì|sì|Esistono alcune restrizioni sul tipo di dati. Questo vale per tutti gli operatori in modalità batch.|    
|Concatenation|UNION e UNION ALL|no|sì|sì||    
|filter|Applicare i predicati|sì|sì|sì||    
|Hash Match|Funzioni di aggregazione basate su hash, outer hash join, right hash join, left hash join, right inner join, left inner join|sì|sì|sì|Restrizioni per l'aggregazione: nessun valore min e max per le stringhe. Le funzioni di aggregazione disponibili sono sum/count/avg/min/max.<br />Restrizioni per il join: nessun join con tipo non corrispondente per i tipi non integer.|    
|Merge Join||no|no|no||    
|Query multithreading||sì|sì|sì||    
|Nested Loops||no|no|no||    
|Query a thread singolo in esecuzione in MAXDOP 1||no|no|sì||    
|Query a thread singolo con un piano di query seriale||no|no|sì||    
|sort|Ordinare per clausola in SCAN con l'indice columnstore.|no|no|sì||    
|Top Sort||no|no|sì||    
|Window Aggregates||ND|ND|sì|Nuovo operatore in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].|    
    
<sup>1</sup> Si applica a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], ai livelli Standard e Premium di [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (S3 e successive), a tutti i livelli vCore e a [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]    

Per altre informazioni, vedere [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution).
    
### <a name="aggregate-pushdown"></a>Distribuzione dell'aggregazione    
 Un percorso di esecuzione normale per il calcolo di aggregazione che consente di recuperare le righe idonee dal nodo SCAN e aggregare i valori in modalità batch. Questo metodo offre buone prestazioni, ma con [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile eseguire il push dell'operazione di aggregazione nel nodo SCAN per migliorare le prestazioni di calcolo di aggregazione per ordini di grandezza durante l'esecuzione in modalità batch, purché vengano soddisfatte le condizioni seguenti: 
 
-    Le funzioni di aggregazione sono `MIN`, `MAX`, `SUM`, `COUNT` e `COUNT(*)`. 
-  L'operatore di aggregazione deve essere sopra il nodo SCAN o il nodo SCAN con `GROUP BY`.
-  L'aggregazione non è un'aggregazione distinta.
-  La colonna di aggregazione non è una colonna stringa.
-  La colonna di aggregazione non è una colonna virtuale. 
-  Il tipo di dati di input e di output deve essere uno dei seguenti e deve rientrare nei 64 bit:
    -  `tinyint`, `int`, `bigint`, `smallint`, `bit`
    -  `smallmoney`, `money`, `decimal` e `numeric` con precisione <= 18
    -  `smalldate`, `date`, `datetime`, `datetime2`, `time`
    
 La distribuzione dell'aggregazione viene ulteriormente accelerata da un'aggregazione efficiente dei dati compressi/codificati durante un'esecuzione di facile integrazione con la cache e sfruttando SIMD    
    
 ![distribuzione dell'aggregazione](../../relational-databases/indexes/media/aggregate-pushdown.jpg "Distribuzione dell'aggregazione")    
    
Ad esempio, la distribuzione dell'aggregazione viene eseguita in entrambe le query seguenti:    
    
```sql     
SELECT  productkey, SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI    
GROUP BY productkey;    
    
SELECT  SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI;    
```    
    
### <a name="string-predicate-pushdown"></a>Distribuzione del predicato stringa    
Quando si progetta uno schema del data warehouse, la modellazione consigliata è l'uso di uno schema star o snowflake costituito da una o più tabelle dei fatti e da molte tabelle delle dimensioni. La [tabella dei fatti](https://wikipedia.org/wiki/Fact_table) archivia le misure o le transazioni aziendali e la [tabella delle dimensioni](https://wikipedia.org/wiki/Dimension_table) archivia le dimensioni di cui analizzare i fatti.    
    
Ad esempio, un fatto può essere un record che rappresenta la vendita di un certo prodotto in un'area specifica, mentre la dimensione rappresenta un set di regioni, prodotti e così via. Le tabelle dei fatti e delle dimensioni sono connesse da una relazione di tipo chiave primaria/chiave esterna. Le query di analisi più diffuse creano un join di una o più tabelle delle dimensioni con la tabella dei fatti.    
    
Ad esempio considerare il caso di una tabella delle dimensioni `Products`. Una chiave primaria tipica è `ProductCode`, generalmente rappresentata con il tipo di dati stringa. Una procedura consigliata per il miglioramento delle prestazioni delle query è la creazione di una chiave surrogata, in genere una colonna di tipo integer, per fare riferimento alla riga della tabella delle dimensioni dalla tabella dei fatti. 
    
L'indice columnstore esegue in modo efficace le query di analisi con join/predicati che includono chiavi di tipo numerico o integer. In molti carichi di lavoro del cliente vengono tuttavia usate colonne basate su stringhe per il collegamento delle tabelle dei fatti e delle dimensioni e le prestazioni delle query con l'indice columnstore non risultano altrettanto efficaci. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] migliora in modo significativo le prestazioni delle query di analisi con le colonne basate su stringhe grazie alla distribuzione dei predicati con colonne di tipo stringa nel nodo SCAN.    
    
La distribuzione del predicato stringa si basa sul dizionario primario o secondario creato per le colonne per migliorare le prestazioni delle query. Ad esempio, prendere in considerazione il segmento di colonna stringa all'interno di un rowgroup costituito da 100 valori stringa distinti. Ipotizzando la presenza di un milione di righe, il valore medio dei riferimenti a ogni singolo valore stringa è pari a 10.000.    
    
Con la distribuzione del predicato stringa, l'esecuzione della query calcola il predicato in base ai valori nel dizionario e, se è qualificato, tutte le righe che fanno riferimento al valore del dizionario risultano automaticamente qualificate. Questo migliora le prestazioni in due modi:
1.  Vengono restituite solo le righe qualificate, riducendo il numero di righe da trasmettere dal nodo SCAN. 
2.  Il numero dei confronti di stringhe viene notevolmente ridotto. In questo esempio sono necessari solo 100 confronti di stringhe invece di un milione. Esistono alcune limitazioni, come descritto di seguito:    

    -   Non è consentita la distribuzione del predicato stringa per i rowgroup delta. Non esiste un dizionario per le colonne nei rowgroup delta.    
    -   Non è consentita la distribuzione del predicato stringa se le voci del dizionario superano i 64 KB.    
    -   Le espressioni che restituiscono NULL non sono supportate.    
    
## <a name="see-also"></a>Vedere anche    
 [Linee guida per la progettazione di indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index) [Indici columnstore - Linee guida per il caricamento di dati](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)     
 [Indici columnstore per il data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)    
 [Architettura degli indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)     
  
