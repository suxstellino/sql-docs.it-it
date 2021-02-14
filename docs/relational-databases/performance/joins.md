---
title: Join (SQL Server) | Microsoft Docs
description: Informazioni sui tipi di operazioni di join usate da SQL Server. SQL Server supporta il partizionamento verticale delle tabelle o l'archiviazione a colonne, tramite le operazioni di join.
ms.custom: ''
ms.date: 07/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- HASH join
- NESTED LOOPS join
- MERGE join
- ADAPTIVE join
- joins [SQL Server], about joins
- join hints [SQL Server]
ms.assetid: bfc97632-c14c-4768-9dc5-a9c512f4b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0716c686698745e5dae851f17844e49b5cab0872
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348022"
---
# <a name="joins-sql-server"></a>Join (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue operazioni di ordinamento, intersezione, unione e differenza tramite le tecnologie di ordinamento in memoria e di hash join. Usando questo tipo di piano di query, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta il partizionamento verticale delle tabelle.   

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa operazioni di join logiche, come determinato dalla sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)]:
-   Inner join
-   Left outer join
-   Right outer join
-   Full outer join
-   Cross join

> [!NOTE]
> Per altre informazioni sulla sintassi di join, vedere [Clausola FROM con JOIN, APPLY, PIVOT (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa quattro tipi di operazioni di join fisico per eseguire le operazioni di join logiche:    
-   Join a cicli annidati     
-   Merge join   
-   Hash join   
-   Join adattivi (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

## <a name="join-fundamentals"></a><a name="fundamentals"></a> Nozioni di base sui join
I join consentono di recuperare dati da due o più tabelle in base alle relazioni logiche esistenti tra le tabelle stesse. I join indicano la modalità d'uso dei dati di una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la selezione di righe in un'altra tabella.    

Una condizione di join definisce il modo in cui due tabelle sono correlate in una query in base agli elementi seguenti:    
-   L'impostazione della colonna di ogni tabella da utilizzare per il join. In una condizione di join tipica viene specificata una chiave esterna di una tabella e la chiave associata nell'altra tabella.    
-   L'impostazione dell'operatore logico (ad esempio = o <>,) da usare per il confronto dei valori delle colonne.   

I join vengono espressi logicamente usando la sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:
-   INNER JOIN
-   LEFT [ OUTER ] JOIN
-   RIGHT [ OUTER ] JOIN
-   FULL [ OUTER ] JOIN
-   CROSS JOIN

Gli **inner join** possono essere specificati nelle clausole `FROM` o `WHERE`. Gli **outer join** e i **cross join** possono essere specificati solo nella clausola `FROM`. Le condizioni di join vengono usate insieme alle condizioni di ricerca delle clausole `WHERE` e `HAVING` per definire le righe da selezionare nelle tabelle di base a cui viene fatto riferimento nella clausola `FROM`.    

L'impostazione delle condizioni di join nella clausola `FROM` consente di separare queste condizioni da altre condizioni di ricerca specificate nella clausola `WHERE`. Corrisponde anche al metodo consigliato per l'impostazione dei join. La sintassi ISO semplificata per la definizione di un join nella clausola `FROM` è la seguente:

```
FROM first_table < join_type > second_table [ ON ( join_condition ) ]
```

*join_type* specifica il tipo di join da eseguire: inner, outer o cross join. *join_condition* definisce il predicato da valutare per ogni coppia di righe unite in join. Di seguito è riportato un esempio di definizione di join nella clausola `FROM`:

```sql
FROM Purchasing.ProductVendor INNER JOIN Purchasing.Vendor
     ON ( ProductVendor.BusinessEntityID = Vendor.BusinessEntityID )
```

L'istruzione seguente è un'istruzione `SELECT` semplice che usa tale join:

```sql
SELECT ProductID, Purchasing.Vendor.BusinessEntityID, Name
FROM Purchasing.ProductVendor INNER JOIN Purchasing.Vendor
    ON (Purchasing.ProductVendor.BusinessEntityID = Purchasing.Vendor.BusinessEntityID)
WHERE StandardPrice > $10
  AND Name LIKE N'F%'
GO
```

L'istruzione `SELECT` restituisce informazioni sul prodotto e sul fornitore per ogni combinazione di prodotti con prezzo maggiore di $ 10 fornito da una società il cui nome inizia con la lettera F.   

Se in una singola query viene fatto riferimento a più tabelle, nessuno dei riferimenti alle colonne deve presentare ambiguità. Nell'esempio precedente entrambe le tabelle `ProductVendor` e `Vendor` includono una colonna denominata `BusinessEntityID`. I nomi di colonna duplicati in due o più tabelle a cui viene fatto riferimento nella query devono essere qualificati con il nome della tabella. Nell'esempio, tutti i riferimenti alle colonne `Vendor` sono qualificati.   

Se il nome di una colonna non è duplicato in due o più tabelle utilizzate nella query, non è necessario qualificare i riferimenti con il nome della tabella, come illustrato nell'esempio precedente. Una clausola `SELECT` di questo tipo a volte è di difficile comprensione in quanto non indica la tabella a cui appartiene ogni colonna. La query risulta più leggibile se tutte le colonne sono qualificate con i nomi delle rispettive tabelle. Il grado di leggibilità aumenta ulteriormente se si utilizzano gli alias di tabella, soprattutto quando è necessario qualificare anche i nomi delle tabelle con il nome del database e del proprietario. L'esempio seguente equivale all'esempio precedente. Per rendere la query più leggibile, sono stati però assegnati alias alle tabelle e i nomi di colonna sono stati qualificati con gli alias di tabella:

```sql
SELECT pv.ProductID, v.BusinessEntityID, v.Name
FROM Purchasing.ProductVendor AS pv 
INNER JOIN Purchasing.Vendor AS v
    ON (pv.BusinessEntityID = v.BusinessEntityID)
WHERE StandardPrice > $10
    AND Name LIKE N'F%';
```

Negli esempi precedenti le condizioni di join sono specificate nella clausola `FROM` (metodo consigliato). La query seguente contiene la stessa condizione di join specificata nella clausola `WHERE`:

```sql
SELECT pv.ProductID, v.BusinessEntityID, v.Name
FROM Purchasing.ProductVendor AS pv, Purchasing.Vendor AS v
WHERE pv.BusinessEntityID=v.BusinessEntityID
    AND StandardPrice > $10
    AND Name LIKE N'F%';
```

L'elenco `SELECT` per un join può fare riferimento a tutte le colonne delle tabelle unite in join o a qualsiasi subset delle colonne. Nell'elenco `SELECT` non è necessario specificare le colonne di ogni tabella del join. Ad esempio, in un join tra tre tabelle è possibile utilizzare una sola tabella come collegamento tra le altre due e le colonne di tale tabella non devono essere necessariamente incluse nell'elenco di selezione. Questa operazione è detta anche **anti semi join**.  

Sebbene le condizioni di join includano in genere confronti di uguaglianza (=), è possibile specificare altri operatori di confronto o relazionali e altri predicati. Per altre informazioni, vedere [Operatori di confronto &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md) e [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md).  

Questo significa che durante l'elaborazione di join in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Query Optimizer sceglie il metodo di elaborazione del join più efficiente tra quelli possibili. Ciò include la scelta del tipo di join fisico più efficiente, l'ordine in cui le tabelle verranno unite in join e anche l'uso di tipi di operazioni di join logiche che non possono essere espresse direttamente con la sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio **semi join** e **anti semi join**. L'esecuzione fisica di vari join può essere realizzata con molte ottimizzazioni diverse e pertanto non può essere stimata in maniera affidabile. Per altre informazioni su semi join e anti semi join, vedere [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  

Non è necessario che alle colonne di una condizione di join sia associato lo stesso nome o lo stesso tipo di dati. Se tuttavia i tipi di dati sono diversi, devono essere compatibili o supportare la conversione implicita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non è possibile eseguire la conversione implicita dei tipi di dati, è necessario impostare nella condizione di join la conversione esplicita tramite la funzione `CAST`. Per altre informazioni sulla conversione implicita ed esplicita dei dati, vedere [Conversione di tipi di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).    

La maggior parte delle query che includono un join possono essere riformulate specificando una subquery, ovvero una query nidificata in un'altra query. La maggior parte delle subquery possono a loro volta essere riformulate come join. Per altre informazioni sulle sottoquery, vedere [Subqueries](../../relational-databases/performance/subqueries.md) (Sottoquery).   

> [!NOTE]
> Non è possibile unire le tabelle in join direttamente in base alle colonne di tipo ntext, text o image. Tuttavia, è possibile unire le tabelle in join indirettamente in base alle colonne di tipo ntext, text o image usando `SUBSTRING`.    
> Ad esempio, `SELECT * FROM t1 JOIN t2 ON SUBSTRING(t1.textcolumn, 1, 20) = SUBSTRING(t2.textcolumn, 1, 20)` esegue un inner join tra due tabelle sui primi 20 caratteri di ogni colonna di tipo text delle tabelle t1 e t2.   
> È possibile anche confrontare colonne di tipo ntext e text di due tabelle confrontando la lunghezza delle colonne con una clausola `WHERE`, come illustrato nell'esempio seguente: `WHERE DATALENGTH(p1.pr_info) = DATALENGTH(p2.pr_info)`

## <a name="understanding-nested-loops-joins"></a><a name="nested_loops"></a> Informazioni sui join a cicli annidati
Se l'input di un join è ridotto (inferiore a 10 righe) e l'input dell'altro join è molto esteso e indicizzato in base alle rispettive colonne di join, l'operazione di join più rapida è rappresentata dai nested loop join indicizzati poiché questi richiedono la minore quantità di I/O e il minor numero di operazioni di confronto. 

Il join a cicli annidati, detto anche *iterazione nidificata*, usa un input di join come tabella di input esterna, visualizzata come input superiore nel piano di esecuzione grafico, e un input di join come tabella di input interna (inferiore). Il ciclo esterno elabora la tabella di input esterna, una riga alla volta. Il ciclo interno, eseguito per ogni riga esterna, cerca le righe corrispondenti nella tabella di input interna.   

Nel caso più semplice, ovvero un *join a cicli annidati di tipo naive*, la ricerca comporta l'analisi di un'intera tabella o indice. Nel caso di un *join a cicli annidati indicizzato*, per la ricerca viene usato un indice. In un *join a cicli annidati temporaneo* l'indice viene creato nell'ambito del piano di query ed eliminato dopo il completamento della query. Tutte queste varianti vengono elaborate da Query Optimizer.   

Un nested loop join è particolarmente efficace se l'input esterno è di dimensioni ridotte mentre l'input interno è preindicizzato e di dimensioni notevoli. In molte transazioni di dimensioni ridotte, ad esempio quelle relative a set di righe limitati, i nested loop join indicizzati offrono prestazioni superiori rispetto ai merge join e agli hash join. Per le query di notevoli dimensioni, tuttavia, i nested loop join rappresentano raramente la scelta ottimale.    

Quando l'attributo OPTIMIZED dell'operatore di join a cicli annidati viene impostato su **True**, significa che vengono usati cicli annidati ottimizzati (o l'ordinamento in batch) per ridurre al minimo le operazioni di I/O quando la tabella interna è di grandi dimensioni, indipendentemente dal fatto che sia parallelizzata o meno. La presenza di questa ottimizzazione in un determinato piano potrebbe non essere ovvia durante l'analisi di un piano di esecuzione, dato che l'ordinamento stesso è un'operazione nascosta. La presenza dell'attributo OPTIMIZED nel codice XML del piano, tuttavia, indica che il join a cicli annidati potrebbe tentare di riordinare le righe di input per migliorare le prestazioni di I/O.

## <a name="understanding-merge-joins"></a><a name="merge"></a> Informazioni sui merge join
Se i due input di join non sono di dimensioni ridotte, ma sono ordinati in base alla rispettiva colonna di join (ad esempio, se sono stati ottenuti tramite l'analisi di indici ordinati), l'operazione di join più rapida è rappresentata dal merge join. Se entrambi gli input di join sono di dimensioni notevoli e analoghe, un merge join con ordinamento eseguito in precedenza e un hash join offrono prestazioni simili. Le operazioni di hash join, tuttavia, risultano spesso molto più rapide se le dimensioni dei due input differiscono in modo significativo.       

Per i merge join è necessario che entrambi gli input siano ordinati in base alle colonne di merge, definite dalle clausole di uguaglianza (ON) del predicato di join. In genere Query Optimizer esegue l'analisi di un indice, se questo esiste nel set di colonne, oppure inserisce un operatore di ordinamento sotto il merge join. In rari casi possono esistere più clausole di uguaglianza, ma le colonne di merge vengono ricavate soltanto da alcune delle clausole disponibili.    

Poiché ogni input è ordinato, l'operatore **Merge Join** recupera una riga da ogni input ed esegue il confronto tra le righe. Ad esempio, per operazioni di inner join, le righe vengono restituite se sono uguali. In caso contrario, la riga con il valore minore viene scartata e dallo stesso input viene recuperata un'altra riga. Questo processo si ripete fino al completamento dell'elaborazione di tutte le righe.    

L'operazione di merge join può essere un'operazione regolare o un'operazione molti-a-molti. Un merge join molti-a-molti utilizza una tabella temporanea per l'archiviazione delle righe. Se sono presenti valori duplicati in entrambi gli input, uno degli input deve tornare all'inizio dei duplicati durante l'elaborazione di ogni duplicato dell'altro input.    

Se è presente un predicato residuo, tutte le righe conformi al predicato di merge vengono valutate da tale predicato e vengono restituite soltanto quelle che lo soddisfano.   

Il merge join è di per sé un'operazione molto rapida, ma può essere una scelta onerosa se sono necessarie operazioni di ordinamento. Se tuttavia il volume dei dati è elevato ed è possibile ottenere i dati desiderati già ordinati da indici ad albero B esistenti, il merge join risulta spesso l'algoritmo di join più veloce.    

## <a name="understanding-hash-joins"></a><a name="hash"></a> Informazioni sugli hash join
Gli hash join consentono l'elaborazione efficiente di input di grandi dimensioni, non ordinati e non indicizzati. Tali join sono utili per ottenere risultati intermedi in query complesse, in quanto:
-   I risultati intermedi non sono indicizzati, a meno che non vengano salvati esplicitamente su disco e quindi indicizzati, e spesso non vengono ordinati in modo adeguato per l'operazione successiva nel piano di query.
-   Query Optimizer stima esclusivamente le dimensioni dei risultati intermedi. Poiché le stime relative a query complesse possono essere estremamente imprecise, è necessario non solo che gli algoritmi di elaborazione dei risultati intermedi siano efficienti, ma anche che vengano ridotti gradualmente nel caso in cui un risultato intermedio risulti molto più grande del previsto.   

Gli hash join consentono di ridurre la denormalizzazione. In genere, la denormalizzazione viene utilizzata per ottenere prestazioni migliori tramite la riduzione delle operazioni di join, nonostante rischi di ridondanza quali aggiornamenti non consistenti. Gli hash join riducono la necessità di utilizzo della denormalizzazione. Gli hash join rendono il partizionamento verticale (la rappresentazione di gruppi di colonne di un'unica tabella in file o indici separati) un'opzione adeguata per la progettazione fisica dei database.     

Gli hash join prevedono due tipi di input, ovvero l'input **di compilazione** e l'input **probe**. Query Optimizer assegna all'input più piccolo il ruolo di input di compilazione.    

Gli hash join vengono utilizzati per vari tipi di operazioni di corrispondenza tra set, ovvero inner join, outer join sinistro, destro e completo, semi-join sinistro e destro, intersezione, unione e differenza. Una variante dell'hash join può anche eseguire la rimozione dei duplicati e il raggruppamento, ad esempio `SUM(salary) GROUP BY department`. Queste modifiche utilizzano un solo input sia per il ruolo di compilazione che per il ruolo probe.   

Nelle sezioni seguenti vengono descritti i diversi tipi di hash join: hash join in memoria, grace hash join e hash join ricorsivo.    

### <a name="in-memory-hash-join"></a><a name="inmem_hash"></a> Hash join in memoria
L'hash join esegue in primo luogo l'analisi o il calcolo dell'intero input di compilazione, quindi compila una tabella hash in memoria. Ogni riga viene inserita in un hash bucket in base al valore hash calcolato per la chiave hash. Se l'intero input di compilazione è inferiore alla memoria disponibile, tutte le righe possono essere inserite nella tabella hash. A questa fase di compilazione segue la fase probe. Viene eseguita l'analisi o il calcolo dell'intero input probe, una riga alla volta. Per ogni riga probe viene calcolato il valore della chiave hash, viene eseguita l'analisi dell'hash bucket corrispondente e vengono prodotte le corrispondenze.    

### <a name="grace-hash-join"></a><a name="grace_hash"></a> Grace hash join
Se le dimensioni dell'input di compilazione sono superiori a quelle della memoria disponibile, l'hash join viene eseguito in vari passaggi. In questo caso, l'hash join viene definito grace hash join. Ogni passaggio prevede una fase di compilazione e una fase probe. L'input di compilazione e l'input probe vengono inizialmente analizzati e partizionati in più file, tramite una funzione di hashing sulle chiavi hash. L'utilizzo della funzione di hashing sulle chiavi hash garantisce che ogni coppia di record su cui si basa il join si trovi nella stessa coppia di file. In tal modo il join di due input di grandi dimensioni risulta convertito in più istanze di dimensioni ridotte della stessa attività. L'hash join viene quindi applicato a ogni coppia di file partizionati.    

### <a name="recursive-hash-join"></a><a name="recursive_hash"></a> Hash join ricorsivo
Se l'input di compilazione ha dimensioni tali per cui gli input per un'unione esterna standard richiedono più livelli di unione, saranno necessari più passaggi di partizionamento e più livelli di partizionamento. Se soltanto alcune partizioni sono di grandi dimensioni, i passaggi di partizionamento aggiuntivi vengono utilizzati soltanto per tali partizioni. Per velocizzare al massimo i passaggi di partizionamento vengono utilizzate operazioni di I/O asincrone di grandi dimensioni, per cui un unico thread può tenere occupate più unità disco.    

> [!NOTE]
> Se l'input di compilazione non ha dimensioni di molto superiori a quelle della memoria disponibile, gli elementi dell'hash join in memoria e del grace hash join vengono combinati in un unico passaggio, producendo un hash join ibrido.   

Durante l'ottimizzazione non sempre è possibile determinare il tipo di hash join che verrà utilizzato. Pertanto, SQL Server usa inizialmente un hash join in memoria, quindi passa gradualmente a grace hash join e hash join ricorsivo a seconda delle dimensioni dell'input di compilazione.    

Se Query Optimizer non prevede correttamente quale dei due input è il più piccolo, al quale quindi deve essere assegnato il ruolo di input di compilazione, i ruoli di input di compilazione e di input probe vengono invertiti dinamicamente. L'hash join utilizza comunque come input di compilazione il file di overflow più piccolo. Questa tecnica è detta inversione dei ruoli. L'inversione dei ruoli si verifica all'interno dell'hash join dopo l'esecuzione di almeno uno spill sul disco.     

> [!NOTE]
> L'inversione dei ruoli si verifica in maniera indipendente da qualsiasi struttura o hint per la query e inoltre non viene visualizzata nel piano di query. Quando si verifica, l'inversione dei ruoli è un'operazione trasparente all'utente.

### <a name="hash-bailout"></a><a name="hash_bailout"></a> hash bailout
Il termine hash bailout è talvolta usato per descrivere grace hash join o hash join ricorsivi.    

> [!NOTE]
> Gli hash join ricorsivi e gli hash bailout causano una riduzione delle prestazioni del server. Se si nota la presenza di molti eventi di avviso di hash in una traccia, aggiornare le statistiche sulle colonne che si sta unendo in join.    

Per altre informazioni sugli hash bailout, vedere [Hash Warning - classe di evento](../../relational-databases/event-classes/hash-warning-event-class.md).    

## <a name="understanding-adaptive-joins"></a><a name="adaptive"></a> Informazioni sui join adattivi
[Modalità batch](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) I join adattivi consentono di rimandare a **dopo** la scansione del primo input la scelta tra l'esecuzione di un metodo [hash join](#hash) e l'esecuzione di un metodo join a [cicli annidati](#nested_loops). L'operatore Join adattivo definisce una soglia che viene usata per stabilire quando passare a un piano Cicli annidati. Durante l'esecuzione, un piano di query può pertanto passare a una strategia di join più efficace senza dover essere ricompilato. 

> [!TIP]
> Questa funzionalità è ottimale per i carichi di lavoro con frequenti oscillazioni tra i volumi di input di join rilevati.

La decisione in fase di esecuzione è basata sui passaggi seguenti:
-  Se il conteggio delle righe dell'input del join di compilazione è così ridotto che un join a cicli annidati è preferibile a un hash join, il piano passa a un algoritmo a cicli annidati.
-  Se l'input del join di compilazione supera una determinata soglia di numero di righe, non si verifica alcun cambiamento e il piano continua con un hash join.

La query seguente illustra un esempio di join adattivo:

```sql
SELECT [fo].[Order Key], [si].[Lead Time Days], [fo].[Quantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Dimension].[Stock Item] AS [si]
       ON [fo].[Stock Item Key] = [si].[Stock Item Key]
WHERE [fo].[Quantity] = 360;
```

La query restituisce 336 righe. Se si attiva [Statistiche query dinamiche](../../relational-databases/performance/live-query-statistics.md), viene visualizzato il piano seguente:

![Risultato della query: 336 righe](../../relational-databases/performance/media/4_AQPStats336Rows.png)

Nel piano notare quanto segue:
1. Un'analisi dell'indice columnstore che specifica le righe per la fase di compilazione dell'hash join.
2. Il nuovo operatore di join adattivo. L'operatore definisce la soglia usata per il passaggio a un piano Cicli annidati. In questo esempio la soglia corrisponde a 78 righe. Se il risultato è &gt;= 78 righe, verrà usato un hash join. Se è inferiore alla soglia, verrà usato un join a cicli annidati.
3. Dato che la query restituisce 336 righe, questa soglia viene superata e pertanto il secondo ramo rappresenta la fase di probe di un'operazione hash join standard. Si noti che Statistiche query dinamiche visualizza le righe del flusso tra gli operatori, in questo caso "672 di 672".
4. L'ultimo ramo è una Ricerca indice cluster che il join a cicli annidati avrebbe usato se la soglia non fosse stata superata. Il valore visualizzato è "0 di 336" righe (il ramo non viene usato).

Confrontare ora il piano con la stessa query, ma in questo caso quando il valore *Quantità* ha una sola riga nella tabella:
 
```sql
SELECT [fo].[Order Key], [si].[Lead Time Days], [fo].[Quantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Dimension].[Stock Item] AS [si]
       ON [fo].[Stock Item Key] = [si].[Stock Item Key]
WHERE [fo].[Quantity] = 361;
```
La query restituisce una riga. Con l'abilitazione di Statistiche query dinamiche viene visualizzato il piano seguente:

![Risultato della query: una riga](../../relational-databases/performance/media/5_AQPStatsOneRow.png)

Nel piano notare quanto segue:
- Con una sola riga restituita, ora il flusso di righe attraversa Ricerca indice cluster.
- Poiché non è stata portata avanti la fase di compilazione dell'hash join, nessuna riga attraversa il secondo ramo.

### <a name="adaptive-join-remarks"></a>Osservazioni per i join adattivi
I join adattivi presentano requisiti di memoria superiori rispetto a un piano equivalente con join a cicli annidati indicizzati. La memoria aggiuntiva risulta necessaria, come se il join a cicli annidati fosse un hash join. Si registra un sovraccarico anche per la fase di compilazione come operazione stop-and-go rispetto a un join a cicli annidati equivalente a livello di flussi. A tale costo aggiuntivo corrisponde una maggior flessibilità per gli scenari in cui i conteggi delle righe possono variare nell'input di compilazione.

I join adattivi in modalità batch funzionano per l'esecuzione iniziale di un'istruzione. Dopo la compilazione, le esecuzioni consecutive restano adattive sulla base della soglia di join adattivo di compilazione e delle righe di runtime del flusso di dati della fase di compilazione dell'input esterno.

Se un join adattivo passa al funzionamento con cicli annidati usa le righe già lette dalla compilazione hash join. L'operatore **non** legge di nuovo le righe del riferimento esterno.

### <a name="tracking-adaptive-join-activity"></a>Rilevamento delle attività di join adattivo
L'operatore Join adattivo ha i seguenti attributi dell'operatore del piano:

|Attributo del piano|Descrizione|
|---|---|
|AdaptiveThresholdRows|Visualizza l'uso della soglia che determina il passaggio da un hash join a un join a cicli annidati.|
|EstimatedJoinType|Probabile tipo del join.|
|ActualJoinType|In un piano reale visualizza l'algoritmo di join scelto in base alla soglia.|

Il piano stimato visualizza la struttura del piano di join adattivo, la soglia di join adattivo definita e il tipo di join stimato.

> [!TIP]
> Query Store acquisisce e può imporre un piano di join adattivo in modalità batch.

### <a name="adaptive-join-eligible-statements"></a>Istruzioni idonee per i join adattivi
Alcune condizioni rendono un join logico idoneo per un join adattivo in modalità batch:
- Il livello di compatibilità del database è 140 o superiore.
- La query è un'istruzione `SELECT` (attualmente le istruzioni di modifica dei dati non sono idonee).
- Il join è idoneo per l'esecuzione in un algoritmo fisico di join a cicli annidati indicizzati o di hash join.
- L'hash join usa la modalità batch, abilitata dalla presenza di un indice columnstore nella query globale, una tabella Columnstore indicizzata a cui fa riferimento direttamente il join o la [modalità batch per rowstore](./intelligent-query-processing.md#batch-mode-on-rowstore).
- Il primo elemento figlio (riferimento esterno) deve essere identico per le soluzioni alternative generate dal join a cicli annidati e dall'hash join.

### <a name="adaptive-threshold-rows"></a>Righe della soglia adattiva
Il grafico seguente visualizza un esempio di intersezione tra il costo di un hash join e il costo di un join a cicli annidati alternativo. In questo punto di intersezione viene determinata la soglia, che a sua volta determina l'algoritmo usato per l'operazione di join.

![Soglia di join](../../relational-databases/performance/media/6_AQPJoinThreshold.png)

### <a name="disabling-adaptive-joins-without-changing-the-compatibility-level"></a>Disabilitazione dei join adattivi senza modificare il livello di compatibilità
È possibile disabilitare i join adattivi nell'ambito del database o dell'istruzione mantenendo comunque la compatibilità sul livello 140 o superiore.  
Per disabilitare i join adattivi per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_ADAPTIVE_JOINS = ON;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ADAPTIVE_JOINS = OFF;
```

Quando è abilitata, questa impostazione viene visualizzata come abilitata in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).
Per abilitare nuovamente i join adattivi per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_ADAPTIVE_JOINS = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ADAPTIVE_JOINS = ON;
```

È anche possibile disabilitare i join adattivi per una query specifica, definendo `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` come [hint per la query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Ad esempio:

```sql
SELECT s.CustomerID,
       s.CustomerName,
       sc.CustomerCategoryName
FROM Sales.Customers AS s
LEFT OUTER JOIN Sales.CustomerCategories AS sc
       ON s.CustomerCategoryID = sc.CustomerCategoryID
OPTION (USE HINT('DISABLE_BATCH_MODE_ADAPTIVE_JOINS')); 
```

> [!NOTE]
> L'hint per la query USE HINT ha la precedenza rispetto una configurazione con ambito database o un'impostazione del flag di traccia. 

## <a name="null-values-and-joins"></a><a name="nulls_joins"></a> Valori Null e join
Gli eventuali valori Null presenti nelle colonne delle tabelle da unire in join non possono essere associati ad altri valori. La presenza di valori Null in una colonna di una delle tabelle da unire in join viene restituita solo se si usa un outer join, a meno che la clausola `WHERE` non escluda i valori Null.     

Le due tabelle riportate di seguito includono entrambe un valore Null nella colonna interessata dal join:     

```
table1                          table2
a           b                   c            d
-------     ------              -------      ------
      1        one                 NULL         two
   NULL      three                    4        four
      4      join4
```    

Un join che confronta i valori della colonna a con quelli della colonna c non consente di ottenere corrispondenze nelle colonne che includono valori NULL:

```sql
SELECT *
FROM table1 t1 JOIN table2 t2
   ON t1.a = t2.c
ORDER BY t1.a;
GO
```  

Nelle colonne a e c viene restituita una sola riga con valore 4:

```
a           b      c           d      
----------- ------ ----------- ------ 
4           join4  4           four   

(1 row(s) affected)
```   

I valori Null restituiti da una tabella di base non sono inoltre facilmente distinguibili dai valori Null restituiti da un outer join. L'istruzione `SELECT` seguente, ad esempio, esegue un left outer join sull due tabelle seguenti:   

```sql
SELECT *
FROM table1 t1 LEFT OUTER JOIN table2 t2
   ON t1.a = t2.c
ORDER BY t1.a;
GO
```   

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]   

```
a           b      c           d      
----------- ------ ----------- ------ 
NULL        three  NULL        NULL 
1           one    NULL        NULL 
4           join4  4           four   

(3 row(s) affected)
```   

Nel set di risultati non è agevole distinguere un valore NULL dei dati da un valore NULL che rappresenta un tentativo di join non riuscito. Se i dati da unire in join includono valori Null, è in genere consigliabile omettere tali valori dai risultati utilizzando un join normale.    

## <a name="see-also"></a>Vedere anche  
[Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
[Operatori di confronto &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)    
[Conversione di tipi di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
[Subqueries](../../relational-databases/performance/subqueries.md)     (Sottoquery)  
[Join adattivi](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins)    
[Clausola FROM con JOIN, APPLY, PIVOT (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
