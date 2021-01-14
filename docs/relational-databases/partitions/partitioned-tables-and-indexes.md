---
description: Partitioned Tables and Indexes
title: Tabelle e indici partizionati | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- partitioned tables [SQL Server], about partitioned tables
- partitioned indexes [SQL Server], architecture
- partitioned tables [SQL Server], architecture
- partitioned indexes [SQL Server], about partitioned indexes
- index partitions
ms.assetid: cc5bf181-18a0-44d5-8bd7-8060d227c927
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dcc5d8e3602261c975f5517ea859e19fc7902936
ms.sourcegitcommit: 629229a7c33a3ed99db63b89127bb016449f7d3d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952056"
---
# <a name="partitioned-tables-and-indexes"></a>Partitioned Tables and Indexes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta il partizionamento di tabelle e indici. I dati di tabelle e indici partizionati vengono divisi in unità che possono essere distribuite facoltativamente in più filegroup in un database. I dati sono partizionati in senso orizzontale, in modo che per gruppi di righe venga eseguito il mapping in singole partizioni. Tutte le partizioni di un singolo indice o di una singola tabella devono trovarsi nello stesso database. La tabella o indice viene gestito come singola entità logica quando si eseguono query o aggiornamenti sui dati. Nelle versioni precedenti a [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] SP1, le tabelle e gli indici partizionati sono disponibili solo in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
> [!IMPORTANT]  
> Per impostazione predefinita, in[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] viene supportato un massimo di 15.000 partizioni. Nelle versioni precedenti a[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], il numero di partizioni è limitato a 1.000 per impostazione predefinita. Nei sistemi basati su architettura x86, la creazione di una tabella o di un indice con più di 1,000 partizioni è possibile ma non supportata.  
  
## <a name="benefits-of-partitioning"></a>Vantaggi del partizionamento  
 Il partizionamento di tabelle o indici di grandi dimensioni può offrire i vantaggi in termini di gestibilità e prestazioni descritti di seguito.  
  
-   È possibile trasferire o accedere a subset di dati in modo rapido ed efficiente, salvaguardando al contempo l'integrità della raccolta di dati. Operazioni quali il caricamento di dati da un sistema OLTP a un sistema OLAP richiedono ad esempio solo pochi secondi invece che minuti o addirittura ore necessari invece quando i dati non sono partizionati.  
  
-   È possibile eseguire più rapidamente operazioni di manutenzione su una o più partizioni. Le operazioni risultano più efficienti perché vengono applicate solo a subset di dati e non all'intera tabella. È ad esempio possibile scegliere di comprimere i dati in una o più partizioni oppure ricompilare una o più partizioni di un indice.  
  
-   È possibile ottenere migliori prestazioni con le query in base alle tipologie eseguite con maggiore frequenza e alla configurazione hardware in uso. Ad esempio, Query Optimizer può elaborare più velocemente le query di tipo equijoin tra due o più tabelle partizionate quando le colonne di partizionamento corrispondono alle colonne in cui le tabelle sono unite in join. Per altre informazioni, vedere [Query](#queries) più avanti.
  
Quando in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito l'ordinamento dei dati per le operazioni di I/O, i dati vengono innanzitutto ordinati in base alla partizione. Per migliorare le prestazioni di ordinamento dei dati, eseguire lo striping dei file di dati delle partizioni tra più dischi configurando un sistema RAID. In questo modo, benché tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i dati vengano comunque ordinati in base alla partizione, è possibile accedere a tutte le unità di ogni partizione simultaneamente.  
  
È inoltre possibile migliorare le prestazioni abilitando l'escalation blocchi a livello di partizione invece che di intera tabella. Ciò consente di ridurre gli effetti di contesa dei blocchi per la tabella. Per ridurre la contesa tra blocchi consentendo l'escalation blocchi alla partizione, impostare l'opzione `LOCK_ESCALATION` dell'istruzione `ALTER TABLE` su AUTO. 

> [!TIP]
> Le partizioni di una tabella o di un indice possono essere inserite in un unico filegroup, ad esempio il filegroup `PRIMARY` o in più filegroup. Quando si usa l'archiviazione a livelli, l'uso di più filegroup consente di assegnare partizioni specifiche a livelli di archiviazione specifici. Tutti gli altri vantaggi del partizionamento si applicano indipendentemente dal numero di filegroup usati o dal posizionamento delle partizioni in filegroup specifici.
  
## <a name="components-and-concepts"></a>Componenti e concetti  
I termini seguenti sono applicabili al partizionamento di tabelle e indici.  
  
### <a name="partition-function"></a>Funzione di partizione  
Oggetto di database che definisce la modalità con cui viene eseguito il mapping delle righe di una tabella o di un indice a un set di partizioni in base ai valori di una determinata colonna, denominata colonna di partizionamento. Ogni valore nella colonna di partizionamento è un input per la funzione di partizionamento, che restituisce un valore di partizione. La funzione di partizione definisce il numero di partizioni e i limiti delle partizioni che la tabella avrà. Ad esempio, data una tabella che contiene i dati degli ordini di vendita, si può decidere di partizionare la tabella in dodici partizioni (mensili) basate su una colonna di tipo **datetime** , come ad esempio una data di vendita.  
  
### <a name="partition-scheme"></a>Schema di partizione 
Oggetto di database che mappa le partizioni di una funzione di partizione a un set di filegroup. Il principale motivo per cui inserire le partizioni in filegroup separati consiste nel fatto che in tal modo è possibile eseguire operazioni di backup nelle partizioni in modo indipendente, in quanto è possibile eseguire backup in filegroup singoli.  

> [!NOTE]
> In Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] sono supportati solo i filegroup primari.  
  
### <a name="partitioning-column"></a>Colonna di partizionamento  
Colonna di una tabella o di un indice utilizzata da una funzione di partizione per partizionare la tabella o l'indice. Le colonne calcolate che partecipano a una funzione di partizione devono essere contrassegnate in modo esplicito come PERSISTED. È possibile utilizzare come colonna di partizionamento tutti i tipi di dati che possono essere utilizzati come colonne di indice, eccetto il tipo di dati **timestamp**. Non è possibile specificare i tipi di dati **ntext**, **text**, **image**, **xml**, **varchar(max)** , **nvarchar(max)** o **varbinary(max)** . Inoltre, non è possibile specificare colonne di tipo definito dall'utente Common Language Runtime (CLR) di Microsoft .NET Framework né colonne di tipo di dati alias.  
  
### <a name="aligned-index"></a>Indice allineato  
Indice basato sullo stesso schema di partizione della relativa tabella corrispondente. Se una tabella e i relativi indici sono allineati, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile cambiare le partizioni in modo rapido ed efficiente, mantenendo inalterata la struttura delle partizioni della tabella e degli indici. Per poter essere allineato alla relativa tabella di base, non è necessario che un indice sia inserito nella stessa funzione di partizione denominata. Tuttavia, la funzione di partizione dell'indice e della tabella di base deve essere essenzialmente uguale, in quanto:
 1. Gli argomenti delle funzioni di partizione devono avere lo stesso tipo di dati.
 2. Definiscono lo stesso numero di partizioni.
 3. Definiscono gli stessi valori limite per le partizioni.  

#### <a name="partitioning-clustered-indexes"></a>Partizionamento di indici cluster
Per il partizionamento di un indice cluster, è necessario che la chiave di clustering includa la colonna di partizionamento. Per il partizionamento di un indice cluster non univoco, se la colonna di partizionamento non è specificata in modo esplicito nella chiave di clustering, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiunge per impostazione predefinita la colonna di partizionamento all'elenco delle chiavi dell'indice cluster. Se l'indice cluster è univoco, è necessario specificare in modo esplicito che la chiave dell'indice cluster include la colonna di partizionamento.        

#### <a name="partitioning-nonclustered-indexes"></a>Partizionamento di indici non cluster
Per il partizionamento di un indice non cluster univoco, è necessario che la chiave dell'indice includa la colonna di partizionamento. Per il partizionamento di un indice non cluster e non univoco, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiunge per impostazione predefinita la colonna di partizionamento come una colonna non chiave (inclusa) dell'indice allo scopo di assicurarsi che l'indice sia allineato con la tabella di base. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non aggiunge la colonna di partizionamento all'indice se è già presente. 

### <a name="non-aligned-index"></a>Indice non allineato  
Indice partizionato in modo indipendente rispetto alla relativa tabella corrispondente. Ciò significa che l'indice presenta uno schema di partizione diverso oppure che si trova in un filegroup separato rispetto alla tabella di base. La progettazione di un indice partizionato non allineato può risultare utile nei casi seguenti:  
-   La tabella di base non è stata partizionata.  
-   La chiave dell'indice è univoca e non contiene la colonna di partizionamento della tabella.  
-   Si desidera che la tabella di base sia inserita in join collocati con più tabelle che utilizzano colonne di join diverse.  

### <a name="partition-elimination"></a>Eliminazione di partizioni
Processo mediante il quale Query Optimizer accede solo alle partizioni rilevanti per soddisfare i criteri di filtro della query.  

## <a name="performance-guidelines"></a>Linee guida relative alle prestazioni  
 Il nuovo limite massimo di 15.000 partizioni influisce sulla memoria, sulle operazioni degli indici partizionati, sui comandi DBCC e sulle query. In questa sezione vengono descritte le implicazioni relative alle prestazioni determinate dall'aumento del numero di partizioni al di sopra di 1000 e vengono illustrate soluzioni alternative a seconda delle necessità. Con l'aumento del limite al numero massimo di partizioni fino a 15.000, è possibile archiviare dati per periodi prolungati. Tuttavia, è consigliabile mantenere i dati solo per il tempo strettamente necessario e preservare l'equilibrio tra livello di prestazioni e numero di partizioni.  
  
### <a name="processor-cores-and-number-of-partitions-guidelines"></a>Linee guida sui core del processore e sul numero di partizioni  
 Per ottimizzare le prestazioni con operazioni parallele, è consigliabile usare lo stesso numero di partizioni come core del processore, fino a un massimo di 64 (che è il numero massimo di processori paralleli utilizzabili da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
### <a name="memory-usage-and-guidelines"></a>Utilizzo della memoria e linee guida  
 È consigliabile disporre di almeno 16 GB di RAM se si utilizza un numero elevato di partizioni. Se il sistema non dispone di memoria sufficiente, le istruzioni DML (Data Manipulation Language) e DDL (Data Definition Language) nonché altri tipi di operazioni potrebbero avere esito negativo. Nei sistemi provvisti di 16 GB di RAM in cui vengono eseguiti numerosi processi che richiedono un'elevata quantità di memoria, quest'ultima potrebbe esaurirsi in caso di operazioni su un numero elevato di partizioni. Pertanto, la probabilità che si verifichino problemi di prestazioni o di memoria si riduce in funzione della quantità di memoria disponibile oltre i 16 GB.  
  
 I limiti di memoria possono influire sulle prestazioni o sulla capacità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di compilare un indice partizionato, specialmente se l'indice non è allineato alla relativa tabella di base o al relativo indice cluster eventualmente applicato. In questo caso può essere utile aumentare il valore dell'opzione di configurazione del server index create memory. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server index create memory](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md). 
  
### <a name="partitioned-index-operations"></a>Operazioni relative agli indici partizionati  
La creazione e la ricompilazione di indici non allineati per una tabella con oltre 1.000 partizioni sono possibili, ma non supportate. Questo tipo di operazioni può causare riduzioni delle prestazioni e un eccessivo consumo della memoria.  
  
La creazione e la ricompilazione di indici allineati possono richiedere più tempo a seconda del numero di partizioni. È consigliabile non eseguire più comandi di creazione e ricompilazione degli indici contemporaneamente poiché potrebbero verificarsi problemi di prestazioni e memoria.  
  
 Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue l'ordinamento per compilare indici partizionati, compila innanzitutto una tabella di ordinamento per ogni partizione. In seguito compila le tabelle di ordinamento nel filegroup di ogni partizione o in **tempdb** se è stata specificata l'opzione SORT_IN_TEMPDB per gli indici. Per poter compilare una tabella di ordinamento, è necessaria una quantità di memoria minima che varia in base alla tabella. Quando si compila un indice partizionato allineato alla relativa tabella di base, le tabelle di ordinamento vengono compilate una alla volta e la quantità di memoria necessaria è minore. Quando invece si compila un indice partizionato non allineato, le tabelle di ordinamento vengono compilate simultaneamente. È pertanto necessaria una quantità di memoria sufficiente a gestire simultaneamente tali ordinamenti. Maggiore è il numero di partizioni e maggiore sarà la quantità di memoria necessaria. La dimensione minima di ogni tabella di ordinamento per ogni partizione è di 40 pagine, ognuna delle quali contiene 8 kilobyte. Ad esempio, per un indice partizionato non allineato con 100 partizioni è necessaria una quantità di memoria sufficiente per ordinare simultaneamente in modo seriale 4.000 (40 * 100) pagine. Se la memoria è disponibile, l'indice verrà compilato anche se è possibile che l'operazione influisca negativamente sulle prestazioni. Se la memoria non è disponibile, l'indice non verrà compilato. Per un indice partizionato allineato con 100 partizioni è invece necessaria solo la memoria per l'ordinamento di 40 pagine, perché gli ordinamenti non vengono eseguiti simultaneamente.  
  
 È possibile che i requisiti di memoria per gli indici allineati e non allineati siamo maggiori se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applica i gradi di parallelismo all'operazione di compilazione in un computer multiprocessore. Ciò dipende dal fatto che maggiori sono i gradi di parallelismo, maggiore sarà la quantità di memoria richiesta. Ad esempio, se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] imposta i gradi di parallelismo su 4, per un indice partizionato non allineato con 100 partizioni sarà necessaria una quantità di memoria che consenta a quattro processori di ordinare simultaneamente 4.000 pagine, ovvero 16.000 pagine. Se l'indice partizionato è allineato, la quantità di memoria si riduce alla quantità necessaria a quattro processori per ordinare 40 pagine, ovvero 160 (4 * 40) pagine. Per ridurre manualmente i gradi di parallelismo, è possibile utilizzare l'opzione di indice MAXDOP.  

### <a name="dbcc-commands"></a>Comandi DBCC  
In presenza di un numero elevato di partizioni, l'esecuzione di comandi DBCC può richiedere un tempo proporzionalmente maggiore.  
  
### <a name="queries"></a>Query  
Le query per le quali si utilizza l'eliminazione di partizioni possono offrire prestazioni analoghe o migliorate con un numero elevato di partizioni. L'esecuzione di query che non utilizzano l'eliminazione di partizioni può richiedere più tempo a seconda del numero di partizioni.  
  
Si supponga ad esempio che una tabella contenga 100 milioni di righe e le colonne `A`, `B`e `C`. 
-  Nello scenario 1 la tabella è divisa in 1000 partizioni per la colonna `A`. 
-  Nello scenario 2, la tabella è divisa in 10.000 partizioni per la colonna `A`. Una query eseguita su tale tabella con clausola `WHERE` per filtrare la colonna `A` eseguirà l'eliminazione di partizioni e analizzerà una sola partizione. Se eseguita nello scenario 2, la stessa query risulta più rapida in quanto è presente un numero minore di righe da analizzare in una partizione. Una query con clausola `WHERE` per filtrare la colonna B analizzerà tutte le partizioni. Nello scenario 1, tale query viene eseguita in meno tempo rispetto allo scenario 2 in quanto sono presenti meno partizioni da analizzare.  

Le query che utilizzano operatori quali TOP o MAX/MIN su colonne diverse dalla colonna di partizionamento possono comportare prestazioni ridotte con il partizionamento perché tutte le partizioni devono essere valutate.  

Se si eseguono spesso query che comportano un equijoin tra due o più tabelle partizionate, è consigliabile che le relative colonne di partizionamento coincidano con le colonne in cui vengono unite le tabelle. È inoltre consigliabile collocare le tabelle o i relativi indici. Ciò significa che verrà utilizzata la stessa funzione di partizione denominata oppure funzioni diverse ma essenzialmente identiche, in quanto:
-  Hanno lo stesso numero di parametri utilizzati per il partizionamento e i parametri corrispondenti hanno gli stessi tipi di dati.
-  Definiscono lo stesso numero di partizioni.
-  Definiscono gli stessi valori limite per le partizioni.
In questo modo, Query Optimizer può elaborare più rapidamente il join, in quanto è possibile unire in join le partizioni stesse. Se una query unisce in join due tabelle che non sono collocate o partizionate nel campo di join, la presenza delle partizioni potrebbe in realtà rallentare l'elaborazione delle query anziché accelerarla.

Per altre informazioni sulla gestione delle partizioni nell'elaborazione delle query, vedere [Miglioramenti apportati all'elaborazione di query su tabelle e indici partizionati](../../relational-databases/query-processing-architecture-guide.md#query-processing-enhancements-on-partitioned-tables-and-indexes).

## <a name="behavior-changes-in-statistics-computation-during-partitioned-index-operations"></a>Modifiche di comportamento nel calcolo delle statistiche durante operazioni su indici partizionati  
 A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le statistiche non vengono create analizzando tutte le righe nella tabella quando viene creato o ricompilato un indice partizionato. Query Optimizer utilizza invece l'algoritmo di campionamento predefinito per generare statistiche. Dopo avere aggiornato un database con gli indici partizionati, è possibile notare una differenza nei dati dell'istogramma relativamente a tali indici. Tale cambiamento potrebbe non influire sulle prestazioni di query. Per ottenere statistiche sugli indici partizionati analizzando tutte le righe nella tabella, usare `CREATE STATISTICS` o `UPDATE STATISTICS` con la clausola `FULLSCAN`.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Attività|Argomento|  
|-|-|   
|Viene illustrato come creare funzioni e schemi di partizione e quindi applicarli a una tabella e a un indice.|[Creare tabelle e indici partizionati](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)|  
|||  
  
## <a name="related-content"></a>Contenuto correlato  
 I seguenti white paper sulle strategie e le implementazioni relative a tabelle e indici partizionati possono risultare particolarmente utili (le informazioni potrebbero essere in lingua inglese).  
-   [Strategie relative a tabelle e indici partizionati in SQL Server 2008](https://msdn.microsoft.com/library/dd578580\(SQL.100\).aspx)    
-   [Come implementare una finestra temporale scorrevole automatica](https://msdn.microsoft.com/library/aa964122\(SQL.90\).aspx)    
-   [Caricamento bulk in una tabella partizionata](/previous-versions/sql/sql-server-2005/administrator/cc966380(v=technet.10))    
-   [Progetto REAL: ciclo di vita dei dati - Partizionamento](/previous-versions/sql/sql-server-2005/administrator/cc966424(v=technet.10))    
-   [Miglioramenti apportati all'elaborazione di query su tabelle e indici partizionati](/previous-versions/sql/sql-server-2008-r2/ms345599(v=sql.105))    
-   [Le 10 migliori procedure consigliate per la compilazione di un data warehouse relazionale su vasta scala](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20Relational%20Engine.pdf) in _SQLCAT's Guide to: Relational Engineering_ (Guida di SQLCAT: informazioni tecniche sul motore relazionale)
  
