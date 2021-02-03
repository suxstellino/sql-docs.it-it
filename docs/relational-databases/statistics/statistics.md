---
title: Statistiche
description: Query Optimizer usa le statistiche per creare piani di query che consentono di migliorare le prestazioni delle query. Informazioni sui concetti e le linee guida per usare l'ottimizzazione query.
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], query optimization
- query performance [SQL Server], statistics
- query optimization statistics [SQL Server]
- statistical information [SQL Server], database options
- query optimization statistics [SQL Server], about query optimization statistics
- statistical information [SQL Server], guidelines
- statistical information [SQL Server]
- using statistics [SQL Server]
- statistical information [SQL Server], indexes
- index statistics [SQL Server]
- query optimizer [SQL Server], statistics
- statistics [SQL Server]
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 521904030d97213770d4a2310b51eaadc37d4e5d
ms.sourcegitcommit: 05fc736e6b6b3a08f503ab124c3151f615e6faab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478586"
---
# <a name="statistics"></a>Statistiche

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Query Optimizer usa le statistiche per creare piani di query che consentono di migliorare le prestazioni delle query. Per la maggior parte delle query, Query Optimizer genera già le statistiche necessarie per un piano di query di alta qualità. In alcuni casi, è necessario creare statistiche aggiuntive o modificare la progettazione delle query per ottenere risultati ottimali. In questo articolo vengono illustrati i concetti relativi alle statistiche e vengono fornite linee guida per un utilizzo efficace delle statistiche di ottimizzazione delle query.  
  
##  <a name="components-and-concepts"></a><a name="DefinitionQOStatistics"></a> Componenti e concetti  
### <a name="statistics"></a>Statistiche  
 Le statistiche di ottimizzazione delle query sono oggetti binari di grandi dimensioni (BLOB) contenenti informazioni statistiche sulla distribuzione dei valori in una o più colonne di una tabella o di una vista indicizzata. Query Optimizer usa queste statistiche per la stima della *cardinalità* o del numero di righe nel risultato della query. Queste *stime di cardinalità* consentono a Query Optimizer di creare un piano di query di alta qualità. A seconda dei predicati, ad esempio, Query Optimizer può usare le stime della cardinalità per scegliere l'operatore Index Seek anziché l'operatore Index Scan che usa una maggior quantità di risorse, se questa opzione migliora le prestazioni delle query.  
  
 Ogni oggetto statistiche viene creato in un elenco di una o più colonne di tabella e include un *istogramma* in cui è visualizzata la distribuzione dei valori nella prima colonna. Negli oggetti statistiche su più colonne sono inoltre archiviate informazioni statistiche sulla correlazione dei valori tra le colonne. Queste statistiche sulla correlazione o *densità* derivano dal numero di righe distinte di valori di colonna. 

#### <a name="histogram"></a><a name="histogram"></a> Istogramma  
Un **istogramma** misura la frequenza di occorrenza per ogni valore distinto in un set di dati. Query Optimizer calcola un istogramma sui valori della colonna nella prima colonna chiave dell'oggetto statistiche, selezionando i valori delle colonne tramite il campionamento statistico delle righe o eseguendo un'analisi completa di tutte le righe nella tabella o nella vista. Se l'istogramma viene creato da un set campionato di righe, i totali archiviati per numero di righe e numero di valori distinct sono stime e non è necessario che siano numeri interi.

> [!NOTE]
> <a name="frequency"></a> In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli istogrammi vengono creati solo per una singola colonna, ovvero la prima colonna nel set di colonne chiave dell'oggetto statistiche.
  
Per creare l'istogramma, Query Optimizer ordina i valori della colonna, calcola il numero di valori che corrispondono a ogni valore di colonna distinto, quindi aggrega i valori della colonna in un massimo di 200 passaggi dell'istogramma contigui. Ogni intervallo dell'istogramma comprende un insieme di valori di colonna seguiti da un valore di colonna pari al limite superiore. Nell'insieme sono inclusi tutti i possibili valori di colonna compresi tra i valori limite, esclusi questi ultimi. Il minore tra i valori di colonna ordinati costituisce il limite superiore per il primo intervallo dell'istogramma.

Più in dettaglio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea l'**istogramma** dal set ordinato di valori di colonna in tre passaggi:

- **Inizializzazione dell'istogramma**: nel primo passaggio viene elaborata una sequenza di valori a partire dall'inizio del set ordinato e vengono raccolti fino a 200 valori di *range_high_key*, *equal_rows*, *range_rows* e *distinct_range_rows* (in questo passaggio *range_rows* e *distinct_range_rows* sono sempre pari a zero). Il primo passaggio termina quando è stato esaurito tutto l'input o quando sono stati trovati 200 valori. 
- **Analisi con unione di bucket**: nel secondo passaggio viene elaborato ogni valore aggiuntivo della colonna iniziale della chiave delle statistiche in base all'ordinamento. Ogni valore successivo viene aggiunto all'ultimo intervallo o viene creato un nuovo intervallo alla fine (questo è possibile perché i valori di input sono ordinati). Se viene creato un nuovo intervallo, una coppia di intervalli esistenti adiacenti viene compressa in un intervallo singolo. Questa coppia di intervalli viene selezionata per ridurre al minimo la perdita di informazioni. Questo metodo usa un algoritmo per il calcolo della *differenza massima*, per ridurre al minimo il numero di intervalli nell'istogramma, aumentando contemporaneamente la differenza tra i valori limite. Durante questa fase il numero di passaggi dopo la compressione degli intervalli rimane 200.
- **Consolidamento dell'istogramma**: nel terzo passaggio, può essere eseguita la compressione di più intervalli se non viene persa una quantità significativa di informazioni. Il numero di intervalli dell'istogramma può essere minore del numero di valori distinct, anche per le colonne con un numero di punti limite inferiore a 200. Pertanto, anche se la colonna ha più di 200 valori univoci, l'istogramma può avere meno di 200 intervalli. Per una colonna costituita solo da valori univoci, quindi, l'istogramma consolidato ha un minimo di tre intervalli.

> [!NOTE]
> Se l'istogramma è stato creato tramite un campione anziché tramite l'analisi completa, i valori di *equal_rows*, *range_rows*, *distinct_range_rows* e  *average_range_rows* vengono stimati e pertanto non devono necessariamente essere valori integer.

Nel diagramma seguente viene illustrato un istogramma con sei intervalli. L'area a sinistra del primo valore limite superiore è il primo intervallo.
  
![Istogramma](../../relational-databases/system-dynamic-management-views/media/histogram-2.svg "Istogramma") 
  
Per ogni passaggio dell'istogramma sopra citato:
-   La riga in grassetto rappresenta il valore limite superiore (*range_high_key*) e il relativo numero di occorrenze (*equal_rows*).  
  
-   L'area a tinta unita a sinistra di *range_high_key* rappresenta l'intervallo dei valori di colonna e il numero medio di occorrenze di ogni valore (*average_range_rows*) di colonna. Il valore *average_range_rows* per il primo passaggio dell'istogramma è sempre 0.  
  
-   Le linee punteggiate rappresentano i valori campionati usati per stimare il numero complessivo dei valori distinti nell'intervallo (*distinct_range_rows*) e il numero complessivo dei valori nell'intervallo (*range_rows*). Query Optimizer utilizza *RANGE_ROWS* e *distinct_range_rows* per calcolare *average_range_rows* e non archivia i valori campionati.   
  
#### <a name="density-vector"></a><a name="density"></a>Vettore di densità  
La **densità** è rappresentata da informazioni sul numero di duplicati in una colonna o combinazione di colonne specifica e viene calcolata con la formula 1/(numero di valori distinti). Query Optimizer utilizza le densità per migliorare le stime della cardinalità per le query che restituiscono più colonne dalla stessa tabella o vista indicizzata. Man mano che la densità diminuisce, aumenta la selettività di un valore. Ad esempio, in una tabella che rappresenta automobili, molte automobili vengono prodotte dallo stesso costruttore, ma a ciascuna è assegnato un numero di identificazione univoco. Un indice basato sul numero di identificazione del veicolo è più selettivo rispetto all'indice basato sul produttore, perché il numero di identificazione del veicolo ha una densità minore rispetto al produttore. 

> [!NOTE]
> La frequenza è rappresentata dalle informazioni sull'occorrenza di ogni valore distinto nella prima colonna chiave dell'oggetto statistiche e viene calcolata con la formula conteggio delle righe * densità. Nelle colonne con valori univoci è possibile trovare una frequenza massima pari a 1.

Il vettore di densità contiene una densità per ogni prefisso di colonna nell'oggetto statistiche. Se in un oggetto statistiche, ad esempio, sono presenti le colonne chiave `CustomerId`, `ItemId` e `Price`, la densità viene calcolata per ognuno dei prefissi di colonna seguenti.
  
|Prefisso di colonna|Densità calcolata su|  
|---|---|
|(CustomerId)|Righe con valori corrispondenti per CustomerId|  
|(CustomerId, ItemId)|Righe con valori corrispondenti per CustomerId e ItemId|  
|(CustomerId, ItemId, Price)|Righe con valori corrispondenti per CustomerId, ItemId e Price| 

### <a name="filtered-statistics"></a>Statistiche filtrate  
 Le statistiche filtrate possono migliorare le prestazioni di esecuzione delle query che effettuano la selezione da subset ben definiti di dati. Le statistiche filtrate utilizzano un predicato del filtro per selezionare il subset di dati incluso nelle statistiche. Statistiche filtrate progettate correttamente possono migliorare il piano di esecuzione delle query rispetto alle statistiche di tabella completa. Per altre informazioni sul predicato del filtro, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md). Per altre informazioni su quando creare statistiche filtrate, vedere la sezione [Quando creare le statistiche](#CreateStatistics) in questo articolo.  
 
### <a name="statistics-options"></a>Opzioni relative alle statistiche  
 Sono disponibili tre opzioni che influiscono sui tempi e sulle modalità di creazione e aggiornamento delle statistiche. Queste opzioni sono configurabili solo a livello di database.  
  
#### <a name="auto_create_statistics-option"></a><a name="AutoUpdateStats"></a>Opzione AUTO_CREATE_STATISTICS  
 Quando l'opzione per la creazione automatica delle statistiche, [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics), è impostata su ON, Query Optimizer crea le statistiche necessarie per colonne singole nel predicato di query, per migliorare le stime della cardinalità per il piano di query. Queste statistiche di colonna singola vengono create in colonne che ancora non hanno un [istogramma](#histogram) in un oggetto statistiche esistente. L'opzione AUTO_CREATE_STATISTICS non determina se le statistiche vengono create per gli indici. Questa opzione non genera inoltre statistiche filtrate, ma si applica esclusivamente alle statistiche di colonna singola per la tabella completa.  
  
 Quando Query Optimizer crea statistiche in seguito all'uso dell'opzione AUTO_CREATE_STATISTICS, il nome delle statistiche inizia con `_WA`. Per determinare se Query Optimizer ha creato statistiche per una colonna del predicato di query, è possibile usare la query seguente.  
  
```sql  
SELECT OBJECT_NAME(s.object_id) AS object_name,  
    COL_NAME(sc.object_id, sc.column_id) AS column_name,  
    s.name AS statistics_name  
FROM sys.stats AS s 
INNER JOIN sys.stats_columns AS sc  
    ON s.stats_id = sc.stats_id AND s.object_id = sc.object_id  
WHERE s.name like '_WA%'  
ORDER BY s.name;  
```  
  
#### <a name="auto_update_statistics-option"></a>Opzione AUTO_UPDATE_STATISTICS  
 Quando l'opzione per l'aggiornamento automatico delle statistiche, [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics), è impostata su ON, Query Optimizer determina se le statistiche potrebbero non essere aggiornate, quindi ne esegue l'aggiornamento qualora vengano usate da una query. Questa azione è nota anche come ricompilazione statistica. Le statistiche diventano obsolete dopo le modifiche delle operazioni di inserimento, aggiornamento, eliminazione o unione modificano la distribuzione dei dati nella tabella o nella vista indicizzata. Query Optimizer determina se le statistiche potrebbero non essere aggiornate contando il numero di modifiche apportate alle righe dall'ultimo aggiornamento delle statistiche e confrontando il numero di modifiche di riga a una soglia. La soglia è basata sulla cardinalità della tabella, che può essere definita come il numero di righe nella tabella o nella vista indicizzata.  
  
- Fino a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , [!INCLUDE[ssde_md](../../includes/ssde_md.md)] utilizza una soglia di ricompilazione basata sul numero di righe nella tabella o nella vista indicizzata al momento della valutazione delle statistiche. La soglia è diversa se una tabella è temporanea o permanente.

  |Tipo di tabella.|Cardinalità tabella (*n*)|Soglia di ricompilazione (# modifiche)|
  |-----------|-----------|-----------|
  |Temporanea|*n* < 6|6|
  |Temporanea|6 <= *n* <= 500|500|
  |Permanente|*n* <= 500|500|
  |Temporanea o permanente|*n* > 500|500 + (0,20 * *n*)|
  
  Se, ad esempio, la tabella contiene 20000 righe, il calcolo è `500 + (0.2 * 20,000) = 4,500` e le statistiche verranno aggiornate ogni 4.500 modifiche.

- A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e con il [livello di compatibilità del database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 130, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] usa anche una soglia di ricompilazione statistica dinamica decrescente che viene modificata in base alla cardinalità della tabella al momento della valutazione delle statistiche. Con questa modifica, le statistiche sulle tabelle di grandi dimensioni vengono aggiornate più spesso. Tuttavia, se un database ha un livello di compatibilità inferiore a 130, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] vengono applicate le soglie.

  |Tipo di tabella.|Cardinalità tabella (*n*)|Soglia di ricompilazione (# modifiche)|
  |-----------|-----------|-----------|
  |Temporanea|*n* < 6|6|
  |Temporanea|6 <= *n* <= 500|500|
  |Permanente|*n* <= 500|500|
  |Temporanea o permanente|500 <= *n* <= 25.000|500 + (0,20 * *n*)|
  |Temporanea o permanente|*n* > 25.000|SQRT (1000 * *n*)|

  Se, ad esempio, la tabella contiene 2 milioni righe, il calcolo è `SQRT(1,000 * 2,000,000) = 44,721` e le statistiche verranno aggiornate ogni 44.721 modifiche.

> [!IMPORTANT]
> In [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] fino a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive con [livello di compatibilità del database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 120 e inferiore abilitare il [flag di traccia 2371](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) in modo che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usi una soglia di aggiornamento delle statistiche dinamica e decrescente.

Sebbene sia consigliabile per tutti gli scenari, l'abilitazione del flag di traccia è facoltativa. Tuttavia, è possibile usare le linee guida seguenti per abilitare il flag di traccia 2371 nel pre- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ambiente:

 - Se si è in un sistema SAP, abilitare questo flag di traccia. Per altre informazioni, vedere questo [blog](/archive/blogs/saponsqlserver/changes-to-automatic-update-statistics-in-sql-server-traceflag-2371).
 - Se è necessario fare affidamento sul processo notturno per aggiornare le statistiche perché l'aggiornamento automatico corrente non viene attivato con frequenza sufficiente, provare ad abilitare il flag di traccia 2371 per regolare la soglia per la cardinalità della tabella.
  
Query Optimizer controlla la presenza di statistiche non aggiornate prima di compilare una query e prima di eseguire un piano di query memorizzato nella cache. Prima di compilare una query, Query Optimizer usa le colonne, le tabelle e le viste indicizzate nel predicato di query per identificare le eventuali statistiche non aggiornate. Prima di eseguire un piano di query memorizzato nella cache, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica che tale piano faccia riferimento alle statistiche aggiornate.  
  
L'opzione AUTO_UPDATE_STATISTICS si applica a oggetti statistiche creati per indici, colonne singole nei predicati di query e statistiche create con l'istruzione [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) . Questa opzione si applica anche alle statistiche filtrate.  
 
È possibile usare [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) per verificare accuratamente il numero di righe modificate in una tabella e decidere se si vogliono aggiornare manualmente le statistiche.

#### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC  
L'opzione relativa all'aggiornamento asincrono delle statistiche, [AUTO_UPDATE_STATISTICS_ASYNC](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics_async), determina se Query Optimizer usa gli aggiornamenti sincroni o asincroni delle statistiche. L'opzione relativa all'aggiornamento asincrono delle statistiche è OFF per impostazione predefinita. Query Optimizer aggiorna quindi le statistiche in modo sincrono. L'opzione AUTO_UPDATE_STATISTICS_ASYNC si applica a oggetti statistiche creati per indici, colonne singole nei predicati di query e statistiche create con l'istruzione [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) .  
 
> [!NOTE]
> Per impostare l'opzione relativa all'aggiornamento asincrono delle statistiche in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], nella pagina *Opzioni* della finestra *Proprietà database* entrambe le opzioni *Aggiornamento automatico statistiche* e *Aggiornamento automatico statistiche asincrono* devono essere impostate su **True**.
  
Gli aggiornamenti delle statistiche possono essere sincroni (impostazione predefinita) o asincroni. 

* Con gli aggiornamenti sincroni delle statistiche, le query vengono sempre compilate ed eseguite con statistiche aggiornate. Quando le statistiche sono obsolete, Query Optimizer attende le statistiche aggiornate prima di compilare ed eseguire la query. 

* Con gli aggiornamenti asincroni delle statistiche, le query vengono compilate con le statistiche esistenti anche se non sono aggiornate. Query Optimizer potrebbe scegliere un piano di query non ottimale se le statistiche non sono aggiornate al momento della compilazione della query. Le statistiche vengono in genere aggiornate subito dopo. Le query che vengono compilate dopo il completamento degli aggiornamenti delle statistiche possono trarre vantaggio dall'utilizzo delle statistiche aggiornate.   

Utilizzare le statistiche sincrone quando si eseguono operazioni che modificano la distribuzione dei dati, quali il troncamento di una tabella o l'esecuzione di un inserimento bulk di una percentuale elevata di righe. Se non si aggiornano manualmente le statistiche dopo avere completato l'operazione, l'uso di statistiche sincrone garantisce che le statistiche vengano aggiornate prima di eseguire query sui dati modificati.  
  
Utilizzare le statistiche asincrone per ottenere tempi di risposta alle query più stimabili per gli scenari seguenti:  
  
* L'applicazione esegue di frequente la stessa query, query analoghe o piani di query memorizzati nella cache analoghi. È possibile che gli aggiornamenti asincroni delle statistiche consentano di ottenere tempi di risposta alle query più stimabili rispetto agli aggiornamenti sincroni delle statistiche perché Query Optimizer può eseguire le query in entrata senza attendere le statistiche aggiornate. Ciò evita che alcune query vengano eseguite in ritardo rispetto ad altre.  
  
* Sono stati riscontrati timeout nelle richieste client causati da una o più query in attesa delle statistiche aggiornate. In alcuni casi, l'attesa delle statistiche sincrone può causare errori nelle applicazioni con timeout aggressivi.  

> [!NOTE]
> Le statistiche sulle tabelle temporanee locali vengono sempre aggiornate in modo sincrono indipendentemente dall'opzione AUTO_UPDATE_STATISTICS_ASYNC. Le statistiche sulle tabelle temporanee globali vengono aggiornate in modo sincrono o asincrono in base all'opzione AUTO_UPDATE_STATISTICS_ASYNC impostata per il database utente.

L'aggiornamento asincrono delle statistiche viene eseguito da una richiesta in background. Quando è pronta per scrivere le statistiche aggiornate nel database, la richiesta tenta di acquisire un blocco di modifica dello schema sull'oggetto dei metadati delle statistiche. Se in una sessione diversa è già presente un blocco sullo stesso oggetto, l'aggiornamento asincrono delle statistiche viene bloccato fino a quando non è possibile acquisire il blocco di modifica dello schema. Analogamente, le sessioni che devono acquisire un blocco di stabilità dello schema (SCH-S) sull'oggetto metadati Statistics per compilare una query potrebbero essere bloccate dalla sessione in background dell'aggiornamento asincrono delle statistiche, che è già in attesa di acquisire il blocco di modifica dello schema. Pertanto, per i carichi di lavoro con compilazioni di query molto frequenti e aggiornamenti frequenti delle statistiche, l'uso di statistiche asincrone può aumentare la probabilità di problemi di concorrenza dovuti al blocco.

In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] è possibile evitare potenziali problemi di concorrenza con l'aggiornamento asincrono delle statistiche se si Abilita la [configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY. Con questa configurazione abilitata, la richiesta in background attenderà di acquisire il blocco di modifica dello schema (SCH-M) e di salvare in modo permanente le statistiche aggiornate in una coda separata con priorità bassa, consentendo ad altre richieste di continuare a compilare query con statistiche esistenti. Quando nessun'altra sessione mantiene un blocco sull'oggetto dei metadati delle statistiche, la richiesta in background acquisisce il blocco di modifica dello schema e aggiorna le statistiche. Nel caso improbabile in cui la richiesta in background non possa acquisire il blocco entro un periodo di timeout di diversi minuti, l'aggiornamento asincrono delle statistiche verrà interrotto e le statistiche non verranno aggiornate fino a quando non viene attivato un altro aggiornamento automatico delle statistiche o fino a quando le statistiche non vengono [aggiornate manualmente](update-statistics.md).

> [!Note]
> L'opzione di configurazione ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY database scoped è disponibile in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] ed è pianificata per essere inclusa in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 

#### <a name="incremental"></a>INCREMENTAL  
 Quando l'opzione INCREMENTAL di CREATE STATISTICS è impostata su ON, le statistiche create sono statistiche della partizione. Con l'impostazione OFF l'albero delle statistiche viene eliminato e le statistiche vengono ricalcolate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore predefinito è OFF. Questa impostazione esegue l'override della proprietà INCREMENTAL a livello di database. Per altre informazioni sulla creazione di statistiche incrementali, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md). Per altre informazioni sulla creazione automatica di statistiche della partizione, vedere [Proprietà database &#40;pagina Opzioni&#41;](../../relational-databases/databases/database-properties-options-page.md#automatic) e [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). 
  
 Quando vengono aggiunte delle nuove partizioni a una tabella di grandi dimensioni, è necessario aggiornare le statistiche per includere le nuove partizioni. Tuttavia, l'analisi dell'intera tabella (opzione FULLSCAN o SAMPLE) potrebbe richiedere diverso tempo. Inoltre, l'analisi dell'intera tabella non è necessaria in quanto occorrono solo le statistiche sulle nuove partizioni. L'opzione Incrementale crea e archivia le statistiche in base alle partizioni, e quando viene eseguito un aggiornamento, permette di aggiornare solo le statistiche di quelle partizioni che richiedono nuove statistiche.  
  
 Se le statistiche per partizione non sono supportate, l'opzione viene ignorata e viene generato un avviso. Le statistiche incrementali non sono supportate per i seguenti tipi di statistiche:  
  
* Statistiche create con indici che non hanno il partizionamento allineato con la tabella di base.  
* Statistiche create per i database secondari leggibili Always On.  
* Statistiche create per i database di sola lettura.  
* Statistiche create per gli indici filtrati.  
* Statistiche create per le viste.  
* Statistiche create per le tabelle interne.  
* Statistiche create con indici spaziali o indici XML.  
  
**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive. 
  
## <a name="when-to-create-statistics"></a><a name="CreateStatistics"></a> Quando creare le statistiche  
 Query Optimizer crea già le statistiche nelle modalità seguenti:  
  
1.  Query Optimizer crea statistiche per gli indici in tabelle o viste, al momento della creazione dell'indice stesso. Tali statistiche vengono create nelle colonne chiave dell'indice. Se l'indice è filtrato, Query Optimizer crea statistiche filtrate nello stesso subset di righe specificato per l'indice filtrato. Per altre informazioni sugli indici filtrati, vedere [Creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md) e [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  

    > [!NOTE]
    > A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], le statistiche non vengono create analizzando tutte le righe nella tabella quando viene creato o ricompilato un indice partizionato. Query Optimizer usa invece l'algoritmo di campionamento predefinito per generare statistiche. Dopo avere aggiornato un database con gli indici partizionati, è possibile notare una differenza nei dati dell'istogramma relativamente a tali indici. Tale cambiamento potrebbe non influire sulle prestazioni di query. Per ottenere statistiche sugli indici partizionati analizzando tutte le righe nella tabella, usare `CREATE STATISTICS` o `UPDATE STATISTICS` con la clausola `FULLSCAN`. 
  
2.  Quando [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) è impostata su ON, Query Optimizer crea statistiche per le singole colonne nei predicati di query.  

Per la maggior parte delle query, questi due metodi di creazione delle statistiche garantiscono la definizione di un piano di query di alta qualità. In alcuni casi, è possibile migliorare i piani di query creando statistiche aggiuntive con l'istruzione [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) . Tali statistiche aggiuntive possono acquisire correlazioni statistiche che non vengono prese in considerazione in Query Optimizer durante la creazione di statistiche per indici o singole colonne. È possibile che nell'applicazione siano disponibili correlazioni statistiche aggiuntive nei dati della tabella che, se calcolate in un oggetto statistiche, possono consentire a Query Optimizer di migliorare i piani di query. Le statistiche filtrate per un subset di righe di dati o le statistiche multicolonna per le colonne dei predicati di query possono ad esempio migliorare il piano di query.  
  
Quando si creano statistiche con l'istruzione CREATE STATISTICS, si consiglia di mantenere l'opzione AUTO_CREATE_STATISTICS impostata su ON, in modo tale che Query Optimizer continui a creare regolarmente statistiche a colonna singola per le colonne dei predicati di query. Per altre informazioni sui predicati, vedere [Condizione di ricerca&#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
Utilizzare l'istruzione CREATE STATISTICS per creare statistiche in una delle seguenti condizioni:  

* In Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene indicato di creare statistiche. 
* Il predicato di query contiene più colonne correlate che non si trovano ancora nello stesso indice.  
* La query effettua la selezione da un subset di dati.  
* La query presenta statistiche mancanti.  
  
### <a name="query-predicate-contains-multiple-correlated-columns"></a>Il predicato di query contiene più colonne correlate  
Quando il predicato di una query contiene più colonne correlate e dipendenti tra loro, è possibile che la creazione di statistiche per più colonne consenta di migliorare il piano di query. Le statistiche su più colonne contengono le statistiche sulla correlazione tra colonne, denominate *densità*, le quali non sono disponili nelle statistiche di colonna singola. Le densità possono migliorare le stime della cardinalità qualora i risultati della query dipendano da relazioni tra i dati di più colonne.  
  
Se le colonne si trovano già nello stesso indice, l'oggetto statistiche multicolonna esiste già e non è necessario crearlo manualmente. Se le colonne ancora non si trovano nello stesso indice, è possibile creare le statistiche multicolonna creando un indice nelle colonne o usando l'istruzione [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md). La gestione di un indice richiede più risorse di sistema rispetto a un oggetto statistiche. Se l'applicazione non richiede l'indice multicolonna, è possibile utilizzare una quantità ridotta di risorse di sistema creando l'oggetto statistiche senza creare l'indice.  
  
Quando si creano le statistiche multicolonna, l'ordine delle colonne nella definizione dell'oggetto statistiche influisce sull'efficacia delle densità nel calcolo delle stime della cardinalità. L'oggetto statistiche archivia le densità per ciascun prefisso delle colonne chiave nella definizione dell'oggetto statistiche. Per altre informazioni sulle densità, vedere la sezione [Densità](#density) in questa pagina.  
  
Per creare densità utili per le stime della cardinalità, è necessario che le colonne nel predicato di query corrispondano a uno dei prefissi delle colonne nella definizione dell'oggetto statistiche. L'esempio seguente crea ad esempio un oggetto statistiche multicolonna per le colonne `LastName`, `MiddleName` e `FirstName`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.stats  
    WHERE name = 'LastFirst'  
    AND object_ID = OBJECT_ID ('Person.Person'))  
DROP STATISTICS Person.Person.LastFirst;  
GO  
CREATE STATISTICS LastFirst ON Person.Person (LastName, MiddleName, FirstName);  
GO  
```  
  
In questo esempio, l'oggetto statistiche `LastFirst` dispone delle densità per i prefissi di colonna seguenti: `(LastName)`, `(LastName, MiddleName)` e `(LastName, MiddleName, FirstName)`. La densità non è disponibile per `(LastName, FirstName)`. Se la query utilizza `LastName` e `FirstName` senza utilizzare `MiddleName`, la densità non è disponibile per le stime della cardinalità.  
  
### <a name="query-selects-from-a-subset-of-data"></a>La query effettua la selezione da un subset di dati  
La creazione di statistiche per indici e colonne singole in Query Optimizer implica la creazione di statistiche per i valori in tutte le righe. Quando le query effettuano la selezione da un subset di righe che dispone di una distribuzione dei dati univoca, le statistiche filtrate possono migliorare i piani di query. È possibile creare le statistiche filtrate usando l'istruzione [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) con la clausola [WHERE](../../t-sql/queries/where-transact-sql.md) per definire l'espressione del predicato del filtro.  
  
Ad esempio, se si usa [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], ogni prodotto nella tabella `Production.Product` appartiene a una delle quattro categorie della tabella `Production.ProductCategory`: Bikes, Components, Clothing e Accessories. Ogni categoria dispone di una distribuzione dei dati diversa in relazione al peso. I pesi nella categoria Bikes sono compresi tra 13,77 e 30, quelli della categoria Components sono compresi tra 2,12 e 1.050 con alcuni valori NULL e quelli delle categorie Clothing e Accessories sono tutti NULL.  
  
Prendendo come esempio la categoria Bikes, le statistiche filtrate per tutti i pesi consentono di fornire a Query Optimizer statistiche più accurate e di migliorare la qualità del piano di query rispetto alle statistiche di tabella completa o alle statistiche inesistenti nella colonna relativa al peso (Weight). La colonna Weight della categoria Bikes rappresenta un candidato valido per le statistiche filtrate. Nel caso di un numero relativamente ridotto di ricerche correlate al peso, tale colonna non è tuttavia necessariamente un candidato valido per un indice filtrato. È possibile che i vantaggi derivanti dai miglioramenti alle prestazioni delle ricerche offerti da un indice filtrato siano inferiori rispetto agli svantaggi derivanti dai costi di manutenzione e archiviazione supplementari dovuti all'aggiunta di un indice filtrato al database.  
  
L'istruzione seguente crea le statistiche filtrate di `BikeWeights` per tutte le sottocategorie di Bikes. L'espressione del predicato filtrata definisce le biciclette (Bikes) enumerando tutte le sottocategorie di Bikes con l'elemento `Production.ProductSubcategoryID IN (1,2,3)`di confronto. Il predicato non può utilizzare il nome di categoria Bikes perché viene archiviato nella tabella Production.ProductCategory e tutte le colonne nell'espressione di filtro devono trovarsi nella stessa tabella.  
  
[!code-sql[StatisticsDDL#FilteredStats2](../../relational-databases/statistics/codesnippet/tsql/statistics_1.sql)]  
  
Query Optimizer può usare le statistiche filtrate di `BikeWeights` per migliorare il piano di query per la query seguente che seleziona tutti gli elementi della categoria Bikes con peso maggiore di `25`.  
  
```sql  
SELECT P.Weight AS Weight, S.Name AS BikeName  
FROM Production.Product AS P  
    JOIN Production.ProductSubcategory AS S   
    ON P.ProductSubcategoryID = S.ProductSubcategoryID  
WHERE P.ProductSubcategoryID IN (1,2,3) AND P.Weight > 25  
ORDER BY P.Weight;  
GO  
```  
  
### <a name="query-identifies-missing-statistics"></a>La query identifica le statistiche mancanti  
Se un errore o un altro evento impedisce la creazione di statistiche da parte di Query Optimizer, il piano di query viene creato senza usare statistiche. Query Optimizer contrassegna le statistiche come mancanti e tenta di rigenerare le statistiche alla successiva esecuzione della query.  
  
Le statistiche mancanti sono indicate come avvisi (nome della tabella in rosso) quando il piano di esecuzione di una query viene visualizzato graficamente utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Il monitoraggio della classe di eventi **Missing Column Statistics** con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] indica anche i casi in cui le statistiche risultano mancanti. Per altre informazioni, vedere [Categoria di eventi Errori e avvisi &#40;Motore di database&#41;](../../relational-databases/event-classes/errors-and-warnings-event-category-database-engine.md).  
  
 In caso di statistiche mancanti, effettuare quanto segue:  
  
* Verificare che [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) e [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) siano impostate su ON.  
* Verificare che il database non sia di sola lettura. Se il database è di sola lettura, non è possibile salvare un nuovo oggetto statistiche.  
* Creare le statistiche mancanti usando l'istruzione [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).  
  
Quando le statistiche su uno snapshot o un database di sola lettura sono mancanti o non aggiornate, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea e gestisce statistiche temporanee in **tempdb**. Quando il [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea statistiche temporanee, al nome delle statistiche viene aggiunto il suffisso *_readonly_database_statistic* per distinguere le statistiche temporanee da quelle permanenti. Il suffisso *_readonly_database_statistic* è riservato alle statistiche generate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile creare script per le statistiche temporanee e riprodurli in un database di lettura e scrittura. Quando viene creato uno script, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] modifica il suffisso del nome delle statistiche da *_readonly_database_statistic* a *_readonly_database_statistic_scripted*.  
  
Solo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile creare e aggiornare le statistiche temporanee. È tuttavia possibile eliminare le statistiche temporanee e monitorare le relative proprietà utilizzando gli stessi strumenti utilizzati per le statistiche permanenti:  
  
* Eliminare le statistiche temporanee usando l'istruzione [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md).  
* Monitorare le statistiche usando le viste del catalogo **[sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)** e **[sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)** . **sys_stats** include una colonna, **is_temporary** , che indica quali statistiche sono permanenti e quali invece temporanee.  
  
 Poiché le statistiche temporanee sono archiviate in **tempdb**, un riavvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comporta l'indisponibilità di tutte le statistiche temporanee.  
    
## <a name="when-to-update-statistics"></a><a name="UpdateStatistics"></a> Quando aggiornare le statistiche  
 Query Optimizer determina che le statistiche potrebbero non essere aggiornate, quindi le aggiorna qualora siano necessarie per un piano di query. In alcuni casi, è possibile migliorare il piano di query e le prestazioni di esecuzione delle query aggiornando le statistiche più frequentemente di quanto accada con l'impostazione ON per [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics). È possibile aggiornare le statistiche mediante l'istruzione UPDATE STATISTICS o la stored procedure sp_updatestats.  
  
 Sebbene consenta di garantire che le query vengano compilate con statistiche aggiornate, l'aggiornamento delle statistiche causa la ricompilazione delle query. Si consiglia di non aggiornare le statistiche troppo frequentemente perché è necessario mantenere un equilibrio a livello di prestazioni tra la necessità di migliorare i piani di query e il tempo necessario per la ricompilazione delle query. Tale equilibrio dipende dall'applicazione in uso.  
  
 Quando si usa UPDATE STATISTICS o sp_updatestats per aggiornare le statistiche, mantenere AUTO_UPDATE_STATISTICS impostata su ON, affinché l'aggiornamento continui a essere eseguito regolarmente in Query Optimizer. Per altre informazioni sull'aggiornamento delle statistiche per una colonna, un indice, una tabella o una vista indicizzata, vedere [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md). Per informazioni sull'aggiornamento delle statistiche per tutte le tabelle interne e definite dall'utente nel database, vedere la stored procedure [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md).  
  
 Per determinare la data dell'ultimo aggiornamento delle statistiche, usare le funzioni [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) o [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md).  
  
 Aggiornare le statistiche nei seguenti casi:  
  
* I tempi di esecuzione delle query sono particolarmente lunghi.  
* Si verificano operazioni di inserimento in colonne chiave crescenti o decrescenti.  
* In seguito a operazioni di manutenzione.  

### <a name="query-execution-times-are-slow"></a>I tempi di esecuzione delle query sono particolarmente lunghi  
 Se i tempi di risposta alle query sono troppo lunghi o imprevedibili, assicurarsi che le query dispongano di statistiche aggiornate prima di eseguire ulteriori procedure di risoluzione dei problemi.  
  
### <a name="insert-operations-occur-on-ascending-or-descending-key-columns"></a>Si verificano operazioni di inserimento in colonne chiave crescenti o decrescenti  
 È possibile che le statistiche per colonne chiave crescenti o decrescenti, ad esempio colonne IDENTITY o timestamp in tempo reale, richiedano aggiornamenti più frequenti rispetto a quelli previsti in Query Optimizer. Le operazioni di inserimento accodano nuovi valori alle colonne crescenti o decrescenti. È possibile che il numero di righe aggiunte non sia sufficiente per attivare un aggiornamento delle statistiche. Se le statistiche non sono aggiornate e le query effettuano la selezione dalle righe aggiunte più di recente, le statistiche correnti non disporranno delle stime della cardinalità per tali nuovi valori. Ciò può comportare imprecisioni nelle stime della cardinalità e prestazioni di esecuzione delle query ridotte.  
  
 Le stime della cardinalità di una query che effettua la selezione dalle date degli ordini di vendita più recenti non saranno ad esempio precise se le statistiche non sono aggiornate in modo da includere le stime della cardinalità di tali date.  
  
### <a name="after-maintenance-operations"></a>In seguito a operazioni di manutenzione  
 Aggiornare le statistiche dopo avere eseguito procedure di manutenzione che modificano la distribuzione dei dati, quali il troncamento di una tabella o l'esecuzione di un inserimento bulk di un'elevata percentuale di righe. Ciò consente di evitare ritardi futuri nell'elaborazione delle query causati dall'attesa da parte delle query stesse degli aggiornamenti automatici delle statistiche.  
  
 Operazioni quali la ricompilazione, la deframmentazione o la riorganizzazione di un indice non modificano la distribuzione dei dati. Non è quindi necessario aggiornare le statistiche in seguito all'esecuzione di operazioni [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes), [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md), [DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) o [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md#reorganizing-indexes). Query Optimizer aggiorna le statistiche in seguito alla ricompilazione di un indice in una tabella o una vista mediante ALTER INDEX REBUILD o DBCC DBREINDEX. Tale aggiornamento delle statistiche è tuttavia il risultato della ricostruzione dell'indice. L'aggiornamento delle statistiche non viene eseguito in Query Optimizer dopo operazioni DBCC INDEXDEFRAG o ALTER INDEX REORGANIZE. 
 
> [!TIP]
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4, usare l'opzione PERSIST_SAMPLE_PERCENT di [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md) o [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md), per impostare e mantenere una percentuale di campionamento specifica per gli aggiornamenti delle statistiche successivi che non specificano in modo esplicito una percentuale di campionamento.

### <a name="automatic-index-and-statistics-management"></a>Gestione automatica dell'indice e delle statistiche

Sfruttare le soluzioni, ad esempio la [deframmentazione dell'indice adattativo](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), per gestire automaticamente la deframmentazione dell'indice e gli aggiornamenti delle statistiche per uno o più database. Questa procedura sceglie automaticamente se ricompilare o riorganizzare un indice in base al relativo livello di frammentazione, tra gli altri parametri, e aggiornare le statistiche con una soglia lineare.
  
##  <a name="queries-that-use-statistics-effectively"></a><a name="DesignStatistics"></a> Query che usano le statistiche in modo efficace  
 Alcune implementazioni delle query, quali le variabili locali e le espressioni complesse nel predicato di query, possono comportare la definizione di piani di query non ottimali. Per evitare che ciò accada, attenersi alle linee guida relative alla progettazione delle query per un utilizzo efficace delle statistiche. Per altre informazioni sui predicati, vedere [Condizione di ricerca&#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 È possibile migliorare i piani di query applicando le linee guida relative alla progettazione delle query che prevedono un utilizzo efficace delle statistiche, al fine di migliorare le *stime della cardinalità* per espressioni, variabili e funzioni usate nei predicati di query. Se il valore di un'espressione, di una variabile o di una funzione non è noto, Query Optimizer non è in grado di determinare il valore da ricercare nell'istogramma e non può pertanto recuperare la stima della cardinalità ottimale dall'istogramma. In tal caso, la stima della cardinalità di Query Optimizer si basa sul numero medio di righe per valore distinct per tutte le righe campionate nell'istogramma. Ciò comporta stime della cardinalità non ottimali e può causare una riduzione delle prestazioni di esecuzione delle query. Per altre informazioni sugli istogrammi, vedere la sezione [Istogramma](#histogram) in questa pagina o [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md).
  
 Nelle linee guida seguenti viene indicato come scrivere query per migliorare i piani di query grazie all'ottimizzazione delle stime della cardinalità.  
  
### <a name="improving-cardinality-estimates-for-expressions"></a>Miglioramento delle stime della cardinalità per le espressioni  
Per migliorare le stime della cardinalità per le espressioni, attenersi alle seguenti linee guida:  
  
* Se possibile, semplificare le espressioni contenenti costanti. Query Optimizer non valuta tutte le funzioni e le espressioni che contengono costanti prima di determinare le stime della cardinalità. Semplificare ad esempio l'espressione `ABS(-100)` in `100`.  
  
* Se l'espressione utilizza più variabili, creare una colonna calcolata per l'espressione, quindi creare le statistiche o un indice nella colonna calcolata. È ad esempio possibile che la stima della cardinalità del predicato di query `WHERE PRICE + Tax > 100` sia migliore se si crea una colonna calcolata per l'espressione `Price + Tax`.  
  
### <a name="improving-cardinality-estimates-for-variables-and-functions"></a>Miglioramento delle stime della cardinalità per variabili e funzioni  
Per migliorare le stime della cardinalità per variabili e funzioni, attenersi alle seguenti linee guida:  
  
* Se nel predicato di query viene utilizzata una variabile locale, riscrivere la query in modo da utilizzare un parametro anziché una variabile locale. Il valore di una variabile locale non è noto quando Query Optimizer crea il piano di esecuzione della query. Quando una query usa un parametro, Query Optimizer usa la stima della cardinalità per il primo valore di parametro effettivo passato alla stored procedure.  
  
* Valutare la possibilità di usare una tabella standard o una tabella temporanea per archiviare i risultati di funzioni con valori di tabella con più istruzioni (mstvf). Query Optimizer non crea statistiche per le funzioni con valori di tabella con più istruzioni. In questo modo, Query Optimizer è in grado di creare statistiche per le colonne della tabella e usarle per definire un piano di query più efficace.  
  
* Utilizzare una tabella standard o una tabella temporanea in sostituzione delle variabili di tabella. Query Optimizer non crea statistiche per le variabili di tabella. In questo modo, Query Optimizer è in grado di creare statistiche per le colonne della tabella e usarle per definire un piano di query più efficace. Diversi elementi determinano l'opportunità di utilizzare una tabella temporanea o una variabile di tabella. Le variabili di tabella utilizzate nelle stored procedure causano un numero di ricompilazioni delle stored procedure inferiore rispetto alle tabelle temporanee. In base all'applicazione in uso, è possibile che l'utilizzo di una tabella temporanea al posto di una variabile di tabella non comporti un miglioramento delle prestazioni.  
  
* Se una stored procedure contiene una query che utilizza un parametro passato, evitare di modificare il valore del parametro all'interno della stored procedure prima di utilizzarlo nella query. Le stime della cardinalità per la query sono basate sul valore del parametro passato, non sul valore aggiornato. Per evitare di modificare il valore del parametro, è possibile riscrivere la query in modo da utilizzare due stored procedure.  
  
     La stored procedure seguente `Sales.GetRecentSales` modifica ad esempio il valore del parametro `@date` se `@date` è NULL.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
    AS BEGIN  
        IF @date IS NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
     Se la prima chiamata alla stored procedure `Sales.GetRecentSales` passa un valore NULL per il parametro `@date` , Query Optimizer compilerà la stored procedure con la stima della cardinalità per `@date = NULL` anche se il predicato di query non è chiamato con `@date = NULL`. È possibile che tale stima della cardinalità differisca notevolmente rispetto al numero di righe nel risultato della query effettivo. Query Optimizer potrebbe quindi scegliere un piano di query non ottimale. Per evitare che ciò accada, è possibile riscrivere la stored procedure utilizzando due procedure, come illustrato di seguito:  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNullRecentSales (@date datetime)  
    AS BEGIN  
        IF @date is NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        EXEC Sales.GetNonNullRecentSales @date;  
    END  
    GO  
    IF OBJECT_ID ( 'Sales.GetNonNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNonNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNonNullRecentSales (@date datetime)  
    AS BEGIN  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
### <a name="improving-cardinality-estimates-with-query-hints"></a>Miglioramento delle stime della cardinalità con gli hint per la query  
 Per migliorare le stime della cardinalità per le variabili locali, è possibile usare gli hint per la query `OPTIMIZE FOR <value>` o `OPTIMIZE FOR UNKNOWN` con RECOMPILE. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
 In alcune applicazioni, la ricompilazione della query ogni volta che viene eseguita può richiedere tempi troppo lunghi. L'hint per la query `OPTIMIZE FOR` può risultare utile anche se non si usa l'opzione `RECOMPILE`. È possibile ad esempio aggiungere un'opzione `OPTIMIZE FOR` alla stored procedure Sales.GetRecentSales per indicare una data specifica. Nell'esempio seguente viene aggiunta l'opzione `OPTIMIZE FOR` alla procedura Sales.GetRecentSales.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetRecentSales;  
GO  
CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
AS BEGIN  
    IF @date is NULL  
        SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
    SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
    WHERE h.SalesOrderID = d.SalesOrderID  
    AND h.OrderDate > @date  
    OPTION ( OPTIMIZE FOR ( @date = '2004-05-01 00:00:00.000'))  
END;  
GO  
```  
  
### <a name="improving-cardinality-estimates-with-plan-guides"></a>Miglioramento delle stime della cardinalità con le guide di piano  
 È possibile che le linee guida relative alla progettazione delle query non siano valide per alcune applicazioni, in quanto non è possibile modificare la query o l'hint per la query RECOMPILE potrebbe causare un numero eccessivo di ricompilazioni. È possibile utilizzare le guide di piano per specificare altri hint, ad esempio USE PLAN, in modo da controllare il comportamento della query mentre si collabora con il fornitore dell'applicazione per esaminarne le modifiche. Per altre informazioni sulle guide di piano, vedere [Guide di piano](../../relational-databases/performance/plan-guides.md).  
  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [Creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md)   
 [Controllo del comportamento Autostat (AUTO_UPDATE_STATISTICS) in SQL Server](https://support.microsoft.com/help/2754171)   
 [STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)   
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)  
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)  
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)    
 [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) (Deframmentazione dell'indice adattativo)
