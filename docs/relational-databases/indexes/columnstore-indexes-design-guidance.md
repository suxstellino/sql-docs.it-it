---
description: Indici columnstore - Linee guida per la progettazione
title: Indici columnstore - Linee guida per la progettazione | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: fc3e22c2-3165-4ac9-87e3-bf27219c820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8dd4d753f4195a1cf4ae6a92ea8bd617675ba4f7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237595"
---
# <a name="columnstore-indexes---design-guidance"></a>Indici columnstore - Linee guida per la progettazione
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Suggerimenti generali per la progettazione di indici columnstore. Bastano poche scelte oculate per ottenere gli alti livelli di compressione dei dati e di prestazioni delle query per cui sono progettati gli indici columnstore. 

## <a name="prerequisites"></a>Prerequisiti

In questo articolo si presuppone una certa familiarità con la terminologia e l'architettura degli indici columnstore. Per altre informazioni, vedere [Indici columnstore - Panoramica](../../relational-databases/indexes/columnstore-indexes-overview.md) e [Architettura degli indici columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index).

### <a name="know-your-data-requirements"></a>Conoscere i requisiti per i dati
Prima di progettare un indice columnstore, è importante conoscere i requisiti per i dati nel modo più approfondito possibile. Ad esempio, valutare le risposte a queste domande:

- Quanto è grande la tabella?
- Le query vengono usate principalmente per operazioni di analisi su intervalli di valori di grande estensione?  La progettazione degli indici columnstore li rende efficaci per analisi di intervalli di grandi dimensioni, piuttosto che per la ricerca di valori specifici.
- Il carico di lavoro prevede un numero elevato di aggiornamenti ed eliminazioni? Gli indici columnstore sono utili quando i dati sono stabili. Le query dovrebbero includere l'aggiornamento e l'eliminazione di meno del 10% delle righe.
- Sono disponibili tabelle dei fatti e delle dimensioni per un data warehouse?
- È necessario eseguire analisi su un carico di lavoro transazionale? In questo caso, vedere le linee guida per la progettazione degli indici columnstore per l'analisi operativa in tempo reale.

Non è detto che sia necessario un indice columnstore. Le tabelle rowstore con indici heap e cluster offrono prestazioni ottimali con le query che eseguono la ricerca di un valore specifico o all'interno di un intervallo di valori di piccole dimensioni. Usare gli indici rowstore con carichi di lavoro transazionali, perché per i carichi di lavoro di questo tipo sono in genere necessarie ricerche all'interno delle tabelle anziché analisi di intervalli estesi nelle tabelle.  

## <a name="choose-the-best-columnstore-index-for-your-needs"></a>Scegliere l'indice columnstore migliore per le proprie esigenze

Un indice columnstore può essere cluster o non cluster.  Un indice columnstore cluster può avere uno o più indici albero B non cluster. È facile provare gli indici columnstore. Se si crea una tabella come indice columnstore, è possibile riconvertire facilmente la tabella in tabella rowstore eliminando l'indice columnstore. 

Di seguito è riportato un riepilogo delle opzioni e dei suggerimenti. 

| Opzione per columnstore | Uso consigliato | Compressione |
| :----------------- | :------------------- | :---------- |
| Indice columnstore cluster | Usare per:<br></br>1) Carico di lavoro di data warehouse tradizionale con schema star o snowflake<br></br>2) Carichi di lavoro Internet delle cose (IOT) per l'inserimento di grandi volumi di dati con aggiornamenti ed eliminazioni minimi. | 10x in media |
| Indici albero B non cluster su un indice columnstore cluster | Usare per:<br></br>    1. Applicare vincoli di chiave primaria e di chiave esterna su un indice columnstore cluster.<br></br>    2. Velocizzare le query che eseguono la ricerca di valori specifici o in intervalli di valori limitati.<br></br>    3. Velocizzare gli aggiornamenti e le eliminazioni di righe specifiche.| 10x in media, con ulteriore spazio di archiviazione per gli indici non cluster.|
| Indice columnstore non cluster su un indice heap o albero B basato su disco | Usare per: <br></br>1) Un carico di lavoro OLTP con alcune query analitiche. È possibile eliminare gli indici albero B creati per l'analisi e sostituirli con un solo indice columnstore non cluster.<br></br>2) Molti carichi di lavoro OLTP tradizionali che eseguono operazioni di estrazione, trasformazione e caricamento (ETL) per spostare i dati in un data warehouse separato. È possibile evitare le operazioni ETL e la necessità di un data warehouse separato creando un indice columnstore non cluster su alcune delle tabelle OLTP. | L'indice columnstore non cluster è un indice aggiuntivo che richiede in media il 10% in più di spazio di archiviazione.|
| Indice columnstore su una tabella in memoria | Le stesse indicazioni valide per un indice columnsore non cluster su una tabella basata su disco, ma la tabella di base è una tabella in memoria. | L'indice columnstore è un indice aggiuntivo.|

## <a name="use-a-clustered-columnstore-index-for-large-data-warehouse-tables"></a>Usare un indice columnstore cluster per tabelle di data warehouse di grandi dimensioni
L'indice columnstore cluster non è semplicemente un indice, ma è lo spazio di archiviazione principale per le tabelle. Consente di ottenere alti livelli di compressione dei dati e un miglioramento significativo delle prestazioni delle query per le tabelle dei fatti e delle dimensioni di data warehouse di grandi dimensioni. Gli indici columnstore cluster sono più adatti a query di analisi piuttosto che a query transazionali, perché le query di analisi eseguono tendenzialmente operazioni su grandi intervalli di valori piuttosto che ricerche di valori specifici. 

Valutare la possibilità di usare un indice columnstore cluster nei casi seguenti:

- Ogni partizione ha almeno un milione di righe. Gli indici columnstore usano rowgroup all'interno di ogni partizione. Se la tabella è troppo piccola per riempire un rowgroup all'interno di ogni partizione, non si otterranno i vantaggi della compressione e delle prestazioni delle query offerte dagli indici columnstore.
- Le query eseguono principalmente operazioni di analisi su intervalli di valori. Ad esempio, per trovare il valore medio di una colonna, la query deve analizzare tutti i valori della colonna e poi aggregare i valori sommandoli per determinare il valore medio.
- La maggior parte degli inserimenti viene eseguita su grandi volumi di dati con aggiornamenti ed eliminazioni minimi. Molti carichi di lavoro, come i carichi Internet delle cose (IOT), eseguono l'inserimento di grandi volumi di dati con aggiornamenti ed eliminazioni minimi. Questi carichi di lavoro possono trarre vantaggio dai miglioramenti a livello di compressione e prestazioni delle query derivanti dall'uso di un indice columnstore cluster.

Non usare un indice columnstore cluster nei casi seguenti:

* La tabella richiede tipi di dati varchar(max), nvarchar(max) o varbinary(max). In questo caso, progettare l'indice columnstore in modo che non includa queste colonne.
* I dati della tabella non sono permanenti. Prendere in considerazione l'uso di un heap o una tabella temporanea quando è necessario archiviare ed eliminare i dati rapidamente.
* La tabella contiene meno di un milione di righe per partizione. 
* Più del 10% delle operazioni sulla tabella sono aggiornamenti ed eliminazioni. Un numero elevato di aggiornamenti ed eliminazioni è causa di frammentazione. La frammentazione influisce sui tassi di compressione e sulle prestazioni delle query, fino a quando non si esegue un'operazione detta riorganizzazione che forza tutti i dati nel columnstore e rimuove la frammentazione. Per altre informazioni, vedere [Riduzione al minimo della frammentazione dell'indice negli indici columnstore](/archive/blogs/sqlserverstorageengine/columnstore-index-defragmentation-using-reorganize-command).

Per altre informazioni, vedere [Indici columnstore - Data warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

## <a name="add-b-tree-nonclustered-indexes-for-efficient-table-seeks"></a>Aggiungere indici albero B non cluster per ricerche efficienti nelle tabelle

A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile creare indici albero B non cluster come indici secondari in un indice columnstore cluster. L'indice albero B non cluster viene aggiornato con le modifiche apportate all'indice columnstore. Si tratta di una funzionalità potente con numerosi vantaggi. 

L'uso di un indice albero B secondario consente di eseguire ricerche di righe specifiche in modo efficiente, senza dover analizzare tutte le righe.  Sono disponibili anche altre opzioni. È possibile ad esempio applicare un vincolo chiave primaria o chiave esterna tramite un vincolo UNIQUE sull'indice albero B. Dato che non è possibile inserire un valore non univoco nell'indice albero B, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può inserire il valore nel columnstore. 

Valutare la possibilità di usare un indice albero B su un indice columnstore nei casi seguenti:
* Per eseguire query per la ricerca di valori specifici o in intervalli di valori limitati.
* Per applicare un vincolo, ad esempio un vincolo di chiave primaria o di chiave esterna.
* Per eseguire con efficienza operazioni di aggiornamento ed eliminazione. L'indice albero B consente di trovare rapidamente le righe specifiche per gli aggiornamenti e le eliminazioni, senza analizzare l'intera tabella o un'intera partizione di una tabella.
* È disponibile spazio aggiuntivo per l'archiviazione dell'indice albero B.

## <a name="use-a-nonclustered-columnstore-index-for-real-time-analytics"></a>Usare un indice columnstore non cluster per analisi in tempo reale

A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile creare un indice columnstore non cluster su una tabella rowstore basata su disco o su una tabella OLTP in memoria. Questo rende possibile l'esecuzione di analisi in tempo reale su una tabella transazionale. In questo modo è possibile eseguire allo stesso tempo analisi sull'indice columnstore e le transazioni sulla tabella sottostante. Dato che una sola tabella gestisce entrambi gli indici, le modifiche sono disponibili in tempo reale sia per l'indice rowstore che per l'indice columnstore.

Un indice columnstore consente di ottenere livelli di compressione dei dati 10 volte migliori rispetto a un indice rowstore, quindi richiede solo una piccola quantità di spazio di archiviazione aggiuntivo. Ad esempio, se la tabella rowstore compressa richiede 20 GB, l'indice columnstore potrebbe richiedere altri 2 GB. Lo spazio aggiuntivo necessario dipende anche dal numero di colonne nell'indice columnstore non cluster. 

 Valutare la possibilità di usare un indice columnstore non cluster nei casi seguenti:

* Per eseguire analisi in tempo reale su una tabella rowstore transazionale. È possibile sostituire gli indici albero B esistenti progettati per l'analisi con un indice columnstore non cluster. 
  
*   Per evitare la necessità di un data warehouse separato. In genere, le aziende eseguono le transazioni in una tabella rowstore e quindi caricano i dati in un data warehouse separato per le operazioni di analisi. Per molti carichi di lavoro, è possibile evitare il processo di caricamento e la disponibilità di un data warehouse separato creando un indice columnstore non cluster sulle tabelle transazionali.

  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] offre diverse strategie per rendere efficiente questo scenario. È molto semplice da provare, perché è possibile abilitare un indice columnstore non cluster senza dover modificare l'applicazione OLTP. 

Per aggiungere ulteriori risorse di elaborazione, è possibile eseguire le operazioni di analisi su una replica secondaria leggibile. L'uso di una replica secondaria leggibile consente di separare l'elaborazione del carico di lavoro transazionale e del carico di lavoro di analisi. 

Per altre informazioni, vedere [Get started with columnstore indexes for real-time operational analytics](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md) (Introduzione agli indici columnstore per l'analisi operativa in tempo reale).

Per altre informazioni sulla scelta dell'indice columnstore ottimale, vedere il blog di Sunil Agarwal [Which columnstore index is right for my workload?](/archive/blogs/sql_server_team/columnstore-index-which-columnstore-index-is-right-for-my-workload) (Qual è l'indice columnstore più appropriato per un carico di lavoro?).

## <a name="use-table-partitions-for-data-management-and-query-performance"></a>Usare le partizioni di tabella per la gestione dei dati e le prestazioni delle query
Gli indici columnstore supportano il partizionamento, che rappresenta una soluzione efficace per la gestione e l'archiviazione dei dati. Il partizionamento consente anche di migliorare le prestazioni delle query, limitando le operazioni a una o più partizioni.

### <a name="use-partitions-to-make-the-data-easier-to-manage"></a>Usare le partizioni per semplificare la gestione dei dati
Per le tabelle di grandi dimensioni, l'uso delle partizioni è l'unico modo pratico per gestire gli intervalli di dati. I vantaggi delle partizioni per le tabelle rowstore si applicano anche agli indici columnstore. 

Ad esempio, sia le tabelle rowstore che le tabelle columnstore usano le partizioni per:

- Controllare le dimensioni dei backup incrementali. È possibile eseguire il backup delle partizioni in filegroup separati e quindi contrassegnarli come di sola lettura. In questo modo, i backup futuri ignoreranno i filegroup di sola lettura. 
- Ridurre i costi di archiviazione spostando una partizione meno recente in uno spazio di archiviazione meno costoso. Ad esempio, è possibile usare il cambio della partizione per spostare una partizione in una posizione di archiviazione meno costosa.
- Eseguire operazioni in modo efficiente limitando le operazioni a una partizione. Ad esempio, è possibile selezionare solo le partizioni frammentate per la manutenzione degli indici.

Con un indice columnstore è inoltre possibile usare il partizionamento per:

* Risparmiare un ulteriore 30% sui costi di archiviazione. È possibile comprimere le partizioni meno recenti con le opzioni di compressione COLUMNSTORE_ARCHIVE. Le prestazioni delle query per questi dati saranno più lente, ma ciò è accettabile se le query su questa partizione sono poco frequenti.

### <a name="use-partitions-to-improve-query-performance"></a>Usare le partizioni per migliorare le prestazioni delle query

L'uso delle partizioni consente di limitare le query all'analisi di partizioni specifiche, con conseguente contenimento del numero di righe da analizzare. Ad esempio, se l'indice viene partizionato in base agli anni e la query deve analizzare i dati dell'anno precedente, l'analisi sarà limitata a una sola partizione. 

### <a name="use-fewer-partitions-for-a-columnstore-index"></a>Usare meno partizioni per un indice columnstore

A meno che le dimensioni dei dati non siano sufficientemente grandi, un indice columnstore offre prestazioni migliori con meno partizioni, rispetto al numero di partizioni generalmente usato per un indice rowstore. Se ogni partizione non include almeno un milione di righe, la maggior parte delle righe potrebbe essere trasferita all'archivio differenziale, perdendo quindi i vantaggi a livello di prestazioni derivanti dalla compressione del columnstore. Ad esempio, se si carica un milione di righe in una tabella con 10 partizioni e ogni partizione riceve 100.000 righe, tutte le righe passeranno ai rowgroup differenziali. 

Esempio:
* Caricare 1.000.000 righe in una singola partizione o in una tabella non partizionata. È possibile ottenere un rowgroup compresso con 1.000.000 di righe. Questa è la configurazione ideale per ottenere alti livelli una compressione dei dati e buone prestazioni per le query.
* Caricare 1.000.000 righe in modo uniforme in 10 partizioni. Ogni partizione riceve 100.000 righe, ovvero un numero minore rispetto alla soglia minima per la compressione del columnstore. Di conseguenza, l'indice columnstore potrebbe avere 10 rowgroup differenziali con 100.000 righe ognuno. Esistono modi per forzare i rowgroup differenziali nel columnstore. Se tuttavia si tratta delle uniche righe dell'indice columnstore, i rowgroup compressi saranno troppo piccoli per ottenere livelli ottimali di compressione e prestazioni delle query.

Per altre informazioni sul partizionamento, vedere il post di blog di Sunil Agarwal [Should I partition my columnstore index?](/archive/blogs/sqlserverstorageengine/columnstore-index-should-i-partition-my-columnstore-index) (È consigliabile partizionare l'indice columnstore?).

## <a name="choose-the-appropriate-data-compression-method"></a>Scegliere il metodo di compressione dei dati appropriato
L'indice columnstore offre due opzioni per la compressione dei dati: compressione del columnstore e compressione dell'archivio. È possibile scegliere l'opzione di compressione quando si crea l'indice o modificarlo in un secondo momento con [ALTER INDEX ... REBUILD](../../t-sql/statements/alter-index-transact-sql.md).

### <a name="use-columnstore-compression-for-best-query-performance"></a>Usare la compressione del columnstore per ottimizzare le prestazioni di query
La compressione del columnstore consente in genere di ottenere tassi di compressione 10 volte migliori rispetto agli indici rowstore. Si tratta del metodo di compressione standard per gli indici columnstore e consente di ottenere prestazioni migliori per le query. 

### <a name="use-archive-compression-for-best-data-compression"></a>Usare la compressione degli archivi per ottenere livelli ottimali di compressione dei dati
La compressione degli archivi è progettata per ottenere la massima compressione quando le prestazioni delle query non sono così importanti e consente di ottenere tassi di compressione dei dati migliori rispetto alla compressione del columnstore, anche se questo vantaggio ha un prezzo. La compressione e la decompressione dei dati richiedono infatti più tempo, quindi non è una soluzione adatta se sono necessarie prestazioni veloci per le query. 

## <a name="use-optimizations-when-you-convert-a-rowstore-table-to-a-columnstore-index"></a>Usare le ottimizzazioni per convertire una tabella rowstore in un indice columnstore

Se i dati sono già disponibili in una tabella rowstore, è possibile usare l'istruzione [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) per convertire la tabella in un indice columnstore cluster. Le due ottimizzazioni descritte di seguito migliorano le prestazioni delle query dopo la conversione della tabella.

### <a name="use-maxdop-to-improve-rowgroup-quality"></a>Usare MAXDOP per migliorare la qualità del rowgroup
È possibile configurare il numero massimo di processori per la conversione di un indice heap o un indice albero B cluster in un indice columnstore. Per configurare i processori, usare l'opzione per il massimo grado di parallelismo (MAXDOP). 

In presenza di grandi quantità di dati, è probabile che l'opzione MAXDOP 1 sia troppo lenta.  È possibile ottenere buoni risultati aumentando MAXDOP a 4. Se si ottengono meno rowgroup senza il numero ottimale di righe, è possibile usare [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md) per unirli in background.

### <a name="keep-the-sorted-order-of-a-b-tree-index"></a>Mantenere l'ordinamento di un indice albero B
Dato che le righe vengono già archiviate con un ordinamento nell'indice albero B, il fatto di mantenere tale ordinamento quando le righe vengono compresse nell'indice columnstore può portare a un miglioramento delle prestazioni delle query.

L'indice columnstore non ordina i dati, ma usa i metadati per tenere traccia dei valori minimi e massimi di ogni segmento di colonna in ogni rowgroup.  Durante l'analisi di un intervallo di valori, può rapidamente calcolare quando ignorare il rowgroup. Quando i dati sono ordinati, possono essere ignorati più rowgroup. 

Per mantenere l'ordinamento durante la conversione:
* Usare [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) con la clausola DROP_EXISTING. Viene così mantenuto anche il nome dell'indice. Se esistono script che usano già il nome dell'indice rowstore non sarà necessario aggiornarli. 

    Questo esempio converte un indice rowstore cluster su una tabella denominata `MyFactTable` in un indice columnstore cluster. Il nome dell'indice, `ClusteredIndex_d473567f7ea04d7aafcac5364c241e09`, rimane invariato.

    ```sql
    CREATE CLUSTERED COLUMNSTORE INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    ON MyFactTable  
    WITH (DROP_EXISTING = ON);  
    ```

## <a name="related-tasks"></a>Attività correlate  
La tabella seguente riepiloga le attività per la creazione e la manutenzione degli indici columnstore. 
  
|Attività|Argomenti di riferimento|Note|  
|----------|----------------------|-----------|  
|Creare una tabella come columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]è possibile creare la tabella come indice columnstore cluster. Non è necessario creare prima una tabella rowstore e quindi convertirla in columnstore.|  
|Creare una tabella in memoria con un indice columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile creare una tabella in memoria ottimizzata con un indice columnstore. L'indice columnstore può anche essere aggiunto dopo aver creato la tabella, usando la sintassi ALTER TABLE ADD INDEX.|  
|Convertire una tabella rowstore in un columnstore.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Convertire un heap o un albero binario esistente o in un columnstore. Gli esempi illustrano come gestire gli indici esistenti e il nome dell'indice quando si esegue questa conversione.|  
|Convertire una tabella columnstore in un rowstore.|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) oppure [Convertire una tabella columnstore di nuovo in un heap rowstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |Di solito non è necessario eseguire questa conversione, ma talvolta potrebbe presentarsene la necessità. Gli esempi illustrano come convertire un columnstore in un heap o in un indice cluster.|   
|Creare un indice columnstore per una tabella rowstore.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Una tabella rowstore può avere un solo indice columnstore.  A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]l'indice columnstore può avere una condizione di filtro. Gli esempi illustrano la sintassi di base.|  
|Creare indici ad alte prestazioni per l'analisi operativa.|[Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Descrive come creare indici columnstore e indici albero B complementari in modo che le query OLTP usino gli indici albero B e le query di analisi usino gli indici columnstore.|  
|Creare indici columnstore efficienti per il data warehousing.|[Indici columnstore - Data warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Descrive come usare gli indici albero B con le tabelle columnstore per creare query di data warehousing ad alte prestazioni.|  
|Usare un indice albero B per imporre un vincolo di chiave primaria per un indice columnstore.|[Indici columnstore - Data warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Illustra come combinare indici albero B e indici columnstore per imporre vincoli di chiave primaria per l'indice columnstore.|  
|Rimuovere un indice columnstore|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|Per rimuovere un indice columnstore si usa la sintassi DROP INDEX standard usata dagli indici albero B. La rimozione di un indice columnstore cluster converte la tabella columnstore in un heap.|  
|Eliminare una riga da un indice columnstore|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Usare [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) per eliminare una riga.<br /><br /> Riga **columnstore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contrassegna la riga come eliminata logicamente ma recupera lo spazio di archiviazione fisico della riga solo dopo che l'indice è stato ricompilato.<br /><br /> Riga **deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina la riga logicamente e fisicamente.|  
|Aggiornare una riga nell'indice columnstore|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Usare [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) per aggiornare una ruga.<br /><br /> Riga **columnstore** :  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contrassegna la riga come eliminata logicamente e quindi inserisce la riga aggiornata nel deltastore.<br /><br /> Riga **deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiorna la riga nel deltastore.|  
|Forzare il passaggio di tutte le righe del deltastore nel columnstore.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) ... REBUILD<br /><br /> [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|ALTER INDEX con l'opzione REBUILD forza il passaggio di tutte le righe nel columnstore.|  
|Deframmentare un indice columnstore|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|ALTER INDEX ... REORGANIZE consente di deframmentare indici columnstore online.|  
|Unire tabelle con indici columnstore.|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)|

## <a name="next-steps"></a>Passaggi successivi
Per creare un indice columnstore vuoto per:

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vedere [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
* [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], vedere [CREATE TABLE (Azure Synapse Analytics)](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md).

Per altre informazioni su come convertire un heap rowstore o un indice albero B esistente in un indice columnstore cluster o su come creare un indice columnstore non cluster, vedere [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md).