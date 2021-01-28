---
title: Ottimizzazione delle prestazioni per i dati
description: Questo articolo illustra le ottimizzazioni delle prestazioni per gli script R o Python eseguiti in SQL Server. Descrive anche i metodi disponibili per aggiornare il codice R, sia per migliorare le prestazioni che per evitare problemi noti.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 71c8852efd538ef0a702c8f96435facf4882b7d2
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596662"
---
# <a name="performance-tuning-and-data-optimization-for-r"></a>Ottimizzazione delle prestazioni e ottimizzazione dei dati per R
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo illustra le ottimizzazioni delle prestazioni per gli script R o Python eseguiti in SQL Server. È possibile usare questi metodi per aggiornare il codice R, sia per migliorare le prestazioni che per evitare problemi noti.

## <a name="choosing-a-compute-context"></a>Scelta di un contesto di calcolo

In SQL Server è possibile usare il contesto di calcolo **locale** o **SQL** quando si esegue lo script R o Python.

Quando si usa il contesto di calcolo **locale**, l'analisi viene eseguita nel computer e non nel server. Se quindi si ricevono dati da SQL Server da usare nel codice, i dati devono essere recuperati in rete. Il calo di prestazioni riscontrato per questo trasferimento in rete dipende dalle dimensioni dei dati trasferiti, dalla velocità della rete e dagli eventuali altri trasferimenti in rete che si verificano nello stesso momento.

Quando si usa il **contesto di calcolo di SQL Server**, il codice viene eseguito nel server. Se si ricevono dati da SQL Server, i dati saranno locali rispetto al server che esegue l'analisi e quindi non viene introdotto alcun sovraccarico di rete. Se è necessario importare dati da altre origini, provare a organizzare in anticipo le operazioni di estrazione, trasformazione e caricamento.

Quando si usano set di dati di grandi dimensioni, è consigliabile usare sempre il contesto di calcolo SQL.

## <a name="factors"></a>Fattori

Il linguaggio R si basa sul concetto di *fattori*, che sono una variabile speciale per i dati categorici. I data scientist usano spesso variabili di fattore nella formula, perché la gestione di variabili categoriche come fattori garantisce che i dati vengano elaborati correttamente dalle funzioni di Machine Learning.

Per impostazione predefinita, le variabili di fattore possono essere convertite da stringhe a numeri interi e viceversa per l'archiviazione o l'elaborazione. La funzione R `data.frame` gestisce tutte le stringhe come variabili di fattore, a meno che l'argomento *stringsAsFactors* sia impostato su **False**. Ciò significa che le stringhe vengono convertite automaticamente in un numero intero per l'elaborazione e quindi nuovamente mappate alla stringa originale.

Se i dati di origine per i fattori vengono archiviati come numeri interi, le prestazioni possono risentirne, perché R converte i numeri interi di fattore in stringhe in fase di esecuzione e quindi esegue la conversione interna da stringa a numero intero.

Per evitare tali conversioni in fase di esecuzione, provare ad archiviare i valori come Integer nella tabella di SQL Server e a usare l'argomento _colInfo_ per specificare i livelli per la colonna usata come fattore. La maggior parte degli oggetti di origine dati in RevoScaleR accetta il parametro _colInfo_. Questo parametro si usa per assegnare un nome alle variabili usate dall'origine dati, specificarne il tipo e definire i livelli o le trasformazioni delle variabili nei valori della colonna.

La chiamata alla funzione R seguente, ad esempio, ottiene i numeri interi 1, 2 e 3 da una tabella, ma esegue il mapping dei valori a un fattore con i livelli "apple", "orange" e "banana".

```R
c("fruit" = c(type = "factor", levels=as.character(c(1:3)), newLevels=c("apple", "orange", "banana")))
```

Quando la colonna di origine contiene stringhe, è sempre più efficiente specificare i livelli in anticipo usando il parametro _colInfo_. Il codice R seguente, ad esempio, considera le stringhe come fattori durante la lettura.

```R
c("fruit" = c(type = "factor", levels= c("apple", "orange", "banana")))
```

Se non esiste alcuna differenza semantica nella generazione del modello, il secondo approccio può comportare prestazioni migliori.

## <a name="data-transformations"></a>Trasformazioni di dati

I data scientist usano spesso le funzioni di trasformazione scritte in R nell'ambito dell'analisi. Le funzioni di trasformazione vengono applicate a ogni riga recuperata dalla tabella. In SQL Server queste trasformazioni vengono applicate a tutte le righe recuperate in un batch, il che richiede la comunicazione tra l'interprete R e il motore di analisi. Per eseguire la trasformazione, i dati vengono spostati da SQL al motore di analisi e quindi al processo dell'interprete R e viceversa.

Per questo motivo, l'uso delle trasformazioni come parte del codice R può avere un notevole effetto negativo sulle prestazioni dell'algoritmo, a seconda della quantità di dati coinvolti.

È più efficiente avere tutte le colonne necessarie nella tabella o nella vista prima di eseguire l'analisi ed evitare trasformazioni durante il calcolo. Se non è possibile aggiungere altre colonne alle tabelle esistenti, è consigliabile creare un'altra tabella o vista con le colonne trasformate e usare una query appropriata per recuperare i dati.

## <a name="batch-row-reads"></a>Letture di righe in batch

Se si usa un'origine dati SQL Server (`RxSqlServerData`) nel codice, è consigliabile provare a usare il parametro _rowsPerRead_ per specificare le dimensioni del batch. Questo parametro definisce il numero di righe sottoposte a query e quindi inviate allo script esterno per l'elaborazione. In fase di esecuzione, l'algoritmo visualizza solo il numero di righe specificato in ogni batch.

Potendo controllare la quantità di dati elaborati in una volta, è possibile risolvere o evitare problemi. Se ad esempio il set di dati di input è molto grande (ha molte colonne) o se il set di dati ha poche colonne di grandi dimensioni (come il testo libero), è possibile ridurre le dimensioni del batch per evitare il paging dei dati al di fuori della memoria.

Per impostazione predefinita, il valore di questo parametro è impostato su 50000, in modo da garantire prestazioni accettabili anche nei computer con memoria insufficiente. Se nel server è disponibile sufficiente memoria, l'aumento di questo valore a 500.000 o anche a un milione può garantire prestazioni migliori, in particolare per tabelle di grandi dimensioni.

I vantaggi derivanti dall'aumento delle dimensioni del batch diventano evidenti con un set di dati grande o con un'attività che può essere eseguita in più processi. Tuttavia, l'aumento di questo valore non garantisce sempre i risultati migliori. Si consiglia provare a usare i propri dati e algoritmo per determinare il valore ottimale.

## <a name="parallel-processing"></a>Elaborazione parallela

Per migliorare le prestazioni delle funzioni di analisi **RX**, SQL Server offre la possibilità di eseguire le attività in parallelo usando i core disponibili nel computer server.

Esistono due modi per ottenere la parallelizzazione con R in SQL Server:

+ **Usare \@parallel**. Quando si usa la stored procedure `sp_execute_external_script` per eseguire uno script R, impostare il parametro `@parallel` su `1`. Questo è il metodo migliore se lo script R **non** usa funzioni RevoScaleR, che hanno altri meccanismi di elaborazione. Se lo script usa funzioni RevoScaleR (in genere con prefisso "rx"), l'elaborazione parallela viene eseguita automaticamente e non è necessario impostare in modo esplicito `@parallel` su `1`.

  Se lo script R può essere eseguito in parallelo e se la query SQL può essere eseguita in parallelo, il motore di database crea più processi paralleli. Il numero massimo di processi che è possibile creare è uguale all'impostazione **Massimo grado di parallelismo** (MAXDOP, Maximun Degree Of Parallelism) per l'istanza. Tutti i processi eseguono quindi lo stesso script, ma ricevono solo una parte dei dati.
  
  Questo metodo non è quindi utile per gli script che devono visualizzare tutti i dati, ad esempio durante il training di un modello. Tuttavia, è utile quando si eseguono attività come la stima in batch in parallelo. Per altre informazioni sull'uso del parallelismo con `sp_execute_external_script`, vedere la sezione dei **suggerimenti per utenti esperti sull'elaborazione parallela** di [Uso del codice R in Transact-SQL](../tutorials/quickstart-r-create-script.md).

+ **Usare numTasks = 1**. Quando si usano funzioni **rx** in un contesto di calcolo di SQL Server, impostare il valore del parametro _numTasks_ sul numero di processi che si vogliono creare. Il numero di processi creati non può mai essere maggiore di **MAXDOP**. Il numero effettivo di processi creati è tuttavia determinato dal motore di database e può essere minore di quello richiesto.

  Se lo script R e la query SQL possono essere rispettivamente eseguiti in parallelo, SQL Server crea più processi paralleli durante l'esecuzione delle funzioni rx. Il numero effettivo di processi creati dipende da diversi fattori, come la governance delle risorse, l'uso corrente delle risorse, altre sessioni e il piano di esecuzione query per la query usata con lo script R.

## <a name="query-parallelization"></a>Parallelizzazione delle query

In Microsoft R è possibile usare le origini dati di SQL Server definendo i dati come oggetto di origine dati RxSqlServerData.

Crea un'origine dati basata su un'intera tabella o vista:

```R
RxSqlServerData(table= "airline", connectionString = sqlConnString)
```

Crea un'origine dati basata su una query SQL:

```R
RxSqlServerData(sqlQuery= "SELECT [ArrDelay],[CRSDepTime],[DayOfWeek] FROM  airlineWithIndex WHERE rowNum <= 100000", connectionString = sqlConnString)
```

> [!NOTE]
> Se nell'origine dati viene specificata una tabella invece di una query, R Services usa l'euristica interna per determinare le colonne necessarie da recuperare dalla tabella, ma è improbabile che questo approccio implichi l'esecuzione in parallelo.

Per garantire che i dati possano essere analizzati in parallelo, la query usata per recuperare i dati dovrebbe consentire al motore di database di creare un piano di query parallelo. Se il codice o l'algoritmo usa volumi di dati di grandi dimensioni, assicurarsi che la query assegnata a `RxSqlServerData` sia ottimizzata per l'esecuzione parallela. Una query che non implica un piano di esecuzione parallela può comportare un singolo processo per il calcolo.

Se è necessario usare set di dati di grandi dimensioni, usare Management Studio o un altro analizzatore di query SQL prima di eseguire il codice R, per analizzare il piano di esecuzione. Eseguire quindi i passaggi consigliati per migliorare le prestazioni della query. Ad esempio, un indice mancante in una tabella può influire sul tempo impiegato per eseguire una query. Per altre informazioni, vedere [Monitorare e ottimizzare le prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md).

Un altro errore comune che può influire sulle prestazioni è che una query recuperi più colonne di quelle richieste. Se ad esempio una formula è basata solo su tre colonne, ma la tabella di origine include 30 colonne, si sposteranno dati inutilmente.

+ È consigliabile non usare `SELECT *`.
+ Esaminare le colonne nel set di dati e identificare solo quelle necessarie per l'analisi
+ Rimuovere dalle query tutte le colonne che contengono tipi di dati non compatibili con il codice R, ad esempio GUID e rowguid
+ Cercare i formati di data e ora non supportati
+ Invece di caricare una tabella, creare una vista che seleziona determinati valori o esegue il cast delle colonne per evitare errori di conversione

## <a name="optimizing-the-machine-learning-algorithm"></a>Ottimizzazione dell'algoritmo di Machine Learning

Questa sezione fornisce diversi suggerimenti e risorse specifici di RevoScaleR e altre opzioni in Microsoft R.

> [!TIP]
> Una discussione generale sull'ottimizzazione di R esula dall'ambito di questo articolo. Se tuttavia è necessario velocizzare il codice, è consigliabile vedere il noto articolo [The R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf) (L'inferno di R), che, con un vivido linguaggio, illustra in dettaglio i costrutti di programmazione in R e gli inconvenienti comuni e offre molti esempi specifici di tecniche di programmazione R.

### <a name="optimizations-for-revoscaler"></a>Ottimizzazioni per RevoScaleR

Molti algoritmi di RevoScaleR supportano parametri per controllare come viene generato il modello con training. Se da una parte la precisione e la correttezza del modello sono importanti, anche le prestazioni dell'algoritmo possono avere altrettanta importanza. Per raggiungere il giusto equilibrio tra precisione e durata del training, è possibile modificare i parametri per aumentare la velocità di calcolo e, in molti casi, migliorare le prestazioni senza compromettere la precisione o la correttezza.

+ [rxDTree](/r-server/r-reference/revoscaler/rxdtree)

  `rxDTree` supporta il parametro `maxDepth`, che controlla la profondità dell'albero delle decisioni. Aumentando `maxDepth`, le prestazioni possono ridursi, quindi è importante analizzare i vantaggi correlati all'aumento della profondità rispetto all'impatto sulle prestazioni.

  È anche possibile controllare il bilanciamento tra la complessità in termini di tempo e l'accuratezza della stima regolando parametri quali `maxNumBins`, `maxDepth`, `maxComplete` e `maxSurrogate`. L'aumento della profondità oltre 10 o 15 può rendere il calcolo molto dispendioso.

+ [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)

  Provare a usare l'argomento `cube` se la prima variabile dipendente nella formula è una variabile di fattore.
  
  Quando `cube` è impostato su `TRUE`, la regressione viene eseguita usando una funzione inversa partizionata, che potrebbe essere più veloce e usare meno memoria rispetto al calcolo della regressione standard. Se la formula contiene un numero elevato di variabili, il miglioramento delle prestazioni può essere significativo.

+ [rxLogit](/r-server/r-reference/revoscaler/rxlogit)

  Usare l'argomento `cube` se la prima variabile dipendente è una variabile di fattore.
  
  Quando `cube` è impostato su `TRUE`, l'algoritmo usa una funzione inversa partizionata, che potrebbe essere più veloce e usare meno memoria. Se la formula contiene un numero elevato di variabili, il miglioramento delle prestazioni può essere significativo.

Per altre informazioni sull'ottimizzazione di RevoScaleR, vedere questi articoli:

+ Articolo del supporto tecnico: [Opzioni di ottimizzazione delle prestazioni per rxDForest e rxDTree](https://support.microsoft.com/kb/3104235)

+ Metodi per controllare il modello adatto al modello di albero con boosting: [Stima di modelli con gradient boosting stocastico](/r-server/r/how-to-revoscaler-boosting)

+ Panoramica di come RevoScaleR sposta ed elabora i dati: [Scrivere algoritmi di divisione in blocchi personalizzati in ScaleR](/r-server/r/how-to-developer-write-chunking-algorithms)

+ Modello di programmazione per RevoScaleR: [Gestione dei thread in RevoScaleR](/r-server/r/how-to-developer-manage-threads)

+ Informazioni di riferimento su [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

+ Informazioni di riferimento su [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)

### <a name="use-microsoftml"></a>Usare MicrosoftML

Si consiglia anche di esaminare il nuovo pacchetto **MicrosoftML**, che fornisce algoritmi di Machine Learning scalabili in grado di usare i contesti di calcolo e le trasformazioni fornite da RevoScaleR.

+ [Iniziare a usare MicrosoftML](/r-server/r/concept-what-is-the-microsoftml-package)

+ [Come scegliere un algoritmo di MicrosoftML](/r-server/r/how-to-choose-microsoftml-algorithms-cheatsheet)

## <a name="next-steps"></a>Passaggi successivi

+ Per le funzioni R che è possibile usare per migliorare le prestazioni del codice R, vedere [Usare le funzioni di profiling del codice R per migliorare le prestazioni](using-r-code-profiling-functions.md).

+ Per informazioni complete sull'ottimizzazione delle prestazioni in SQL Server, vedere [Centro prestazioni per il motore di database di SQL Server e il database SQL di Azure](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).