---
title: 'Esercitazione su R: Esplorare i dati'
description: Esercitazione che illustra come visualizzare e generare riepiloghi statistici usando funzioni R per l'analisi nel database in SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 676fd125bf148a5139de1ecee7145c523ee6b4e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272942"
---
# <a name="view-and-summarize-sql-server-data-using-r-walkthrough"></a>Visualizzare e riepilogare i dati di SQL Server usando R (procedura dettagliata)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

In questa lezione vengono presentate le funzioni nel pacchetto **RevoScaleR** e vengono descritti i passaggi per completare le attività seguenti:

> [!div class="checklist"]
> * Connessione a SQL Server
> * Definire una query che contiene i dati necessari oppure specificare una tabella o una vista
> * Definire uno o più contesti di calcolo da usare durante l'esecuzione del codice R
> * Facoltativamente, definire trasformazioni da applicare all'origine dati durante la lettura dall'origine

## <a name="define-a-sql-server-compute-context"></a>Definire un contesto di calcolo di SQL Server

Eseguire le istruzioni R seguenti in un ambiente R nella workstation client. Questa sezione prevede l'utilizzo di una [workstation di data science con Microsoft R Client](../r/set-up-a-data-science-client.md), che include tutti i pacchetti RevoScaleR, oltre a un set di base e leggero di strumenti R. Ad esempio, è possibile usare Rgui.exe per eseguire lo script R in questa sezione.

1. Se il pacchetto **RevoScaleR** non è già caricato, eseguire questa riga di codice R:

    ```R
    library("RevoScaleR")
    ```

     In questo caso le virgolette sono facoltative, anche se consigliate.
     
     In caso di errore, assicurarsi che l'ambiente di sviluppo R usi una libreria che include il pacchetto RevoScaleR. Usare un comando come `.libPaths()` per visualizzare il percorso corrente della libreria.

2. Creare la stringa di connessione per SQL Server e salvarla in una variabile R, *connStr*.

   È necessario modificare il segnaposto "your_server_name" con il nome di un'istanza valida di SQL Server. Per il nome del server può essere possibile usare solo il nome dell'istanza oppure potrebbe essere necessario specificare il nome completo, a seconda della rete.
    
   Per l'autenticazione di SQL Server, la sintassi di connessione è la seguente:

    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Uid=your-sql-login;Pwd=your-login-password"
    ```

    Per l'autenticazione di Windows, la sintassi è leggermente diversa:
    
    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Trusted_Connection=True"
    ```

    In generale, è consigliabile usare l'autenticazione di Windows dove possibile, per evitare di salvare le password nel codice R.

3. Definire le variabili da usare nella creazione di un nuovo *contesto di calcolo*. Dopo aver creato l'oggetto contesto di calcolo, è possibile usarlo per eseguire codice R nell'istanza di SQL Server.

    ```R
    sqlShareDir <- paste("C:\\AllShare\\",Sys.getenv("USERNAME"),sep="")
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```

    - R usa una directory temporanea per la serializzazione degli oggetti R tra la workstation e il computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile specificare la directory locale che viene usata come *sqlShareDir* o accettare quella predefinita.
  
    - Usare *sqlWait* per indicare se si vuole che R attenda i risultati dal server.  Per una descrizione dei processi in attesa e non in attesa, vedere [Elaborazione parallela e distribuita con RevoScaleR in Microsoft R](/r-server/r/how-to-revoscaler-distributed-computing).
  
    - Usare l'argomento *sqlConsoleOutput* per indicare che non si vuole visualizzare l'output dalla console R.

4. Chiamare il costruttore [RxInSqlServer](/r-server/r-reference/revoscaler/rxinsqlserver) per creare l'oggetto contesto di calcolo con le variabili e le stringhe di connessione già definite e salvare il nuovo oggetto nella variabile R *sqlcc*.
  
    ```R
    sqlcc <- RxInSqlServer(connectionString = connStr, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```

5. Per impostazione predefinita, il contesto di calcolo è locale, quindi è necessario impostare in modo esplicito il contesto di calcolo *attivo*.

    ```R
    rxSetComputeContext(sqlcc)
    ```

    + [rxSetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) restituisce il contesto di calcolo precedentemente attivo in modo invisibile in modo che sia possibile usarlo
    + [rxGetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) restituisce il contesto di calcolo attivo
    
    L'impostazione di un contesto di calcolo ha effetto solo sulle operazioni che usano le funzioni nel pacchetto **RevoScaleR**. Il contesto di calcolo non ha alcun effetto sul modo in cui vengono eseguite le operazioni R open source.

## <a name="create-a-data-source-using-rxsqlserver"></a>Creare un'origine dati usando RxSqlServer

Quando si usano le librerie Microsoft R come RevoScaleR e MicrosoftML, un'*origine dati* è un oggetto creato usando funzioni RevoScaleR. L'oggetto origine dati specifica i set dati che si vogliono usare per un'attività, come ad esempio il training di un modello o l'estrazione di caratteristiche. È possibile ottenere dati da numerose origini, tra cui SQL Server. Per l'elenco delle origini attualmente supportate, vedere [RxDataSource](/r-server/r-reference/revoscaler/rxdatasource).

In precedenza è stata definita una stringa di connessione e le informazioni sono state salvate in una variabile R. È possibile riutilizzare queste informazioni di connessione per specificare i dati che si vogliono recuperare.

1. Salvare una query SQL come variabile stringa. La query definisce i dati per il training del modello.

    ```R
    sampleDataQuery <- "SELECT TOP 1000 tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"
    ```

    Qui è stata usata una clausola TOP per accelerare l'esecuzione, ma le righe effettive restituite dalla query possono variare a seconda dell'ordine. Di conseguenza, i risultati di riepilogo potrebbero essere diversi da quelli elencati di seguito. È possibile rimuovere la clausola TOP senza problemi.

2. Passare la definizione di query come argomento alla funzione [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata).

    ```R
    inDataSource <- RxSqlServerData(
      sqlQuery = sampleDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )
    ```
    
    + L'argomento  *colClasses* specifica i tipi di colonna da usare durante lo spostamento dei dati tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e R. Questo aspetto è importante perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa tipi di dati diversi rispetto a R e più tipi di dati. Per altre informazioni, vedere [Librerie R e tipi di dati](../r/r-libraries-and-data-types.md).
  
    + L'argomento *rowsPerRead* è importante per la gestione dell'utilizzo della memoria e per calcoli ottimizzati.  La maggior parte delle funzioni analitiche avanzate in[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] elabora i dati in blocchi e raggruppa risultati intermedi, restituendo i calcoli finali dopo che tutti i dati sono stati letti.  Aggiungendo il parametro *rowsPerRead*, è possibile controllare il numero di righe di dati lette in ogni blocco da elaborare.  Se il valore di questo parametro è troppo alto, l'accesso ai dati potrebbe risultare lento perché la memoria disponibile non è sufficiente a elaborare in modo efficiente un blocco di dati così grande.  In alcuni sistemi, anche se il parametro *rowsPerRead* viene impostato su un valore troppo basso può verificarsi una riduzione delle prestazioni.

3. A questo punto è stato creato l'oggetto *inDataSource*, che però non contiene dati. I dati non vengono estratti dalla query SQL nell'ambiente locale finché non si esegue una funzione come ad esempio [rxImport](/r-server/r-reference/revoscaler/rxdatastep) o [rxSummary](/r-server/r-reference/revoscaler/rxsummary).

    Tuttavia, una volta definiti gli oggetti dati, è possibile usarli come argomento per altre funzioni.

## <a name="use-the-sql-server-data-in-r-summaries"></a>Usare i dati di SQL Server nei riepiloghi R

In questa sezione saranno trattate alcune delle funzioni disponibili in [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] a supporto dei contesti di calcolo remoti. Applicando le funzioni R all'origine dati è possibile esplorare, riepilogare e classificare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

1. Chiamare la funzione [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) per ottenere un elenco delle variabili nell'origine dati e i relativi tipi di dati.

    **rxGetVarInfo** è una funzione utile, che si può chiamare su qualsiasi frame di dati o set di dati in un oggetto dati remoto per ottenere informazioni, ad esempio i valori minimo e massimo, il tipo di dati e il numero di livelli nelle colonne fattore.
    
    È consigliabile eseguire questa funzione dopo qualsiasi tipo di input dei dati, dopo la trasformazione o la progettazione di una funzionalità. In questo modo ci si può assicurare che tutte le caratteristiche che si vogliono usare nel modello siano del tipo di dati previsto, evitando così errori.
  
    ```R
    rxGetVarInfo(data = inDataSource)
    ```

    **Risultati**
    
    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: integer
    Var 4: trip_time_in_secs, Type: numeric, Storage: int64
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: pickup_longitude, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: dropoff_longitude, Type: numeric
    ```

2. A questo punto, chiamare la funzione RevoScaleR [rxSummary](/r-server/r-reference/revoscaler/rxsummary) per ottenere statistiche più dettagliate sulle singole variabili.

    rxSummary si basa sulla funzione R `summary`, ma presenta funzionalità e vantaggi aggiuntivi. rxSummary funziona in più contesti di calcolo e supporta la suddivisione in blocchi.  È anche possibile usare rxSummary per trasformare valori o riepilogare in base ai livelli di fattore.
    
    In questo esempio si riepiloga l'importo della tariffa in base al numero dei passeggeri.
    
    ```R
    start.time <- proc.time()
    rxSummary(~fare_amount:F(passenger_count,1,6), data = inDataSource)
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
      Elapsed Time=", round(used.time[3],2),
      " seconds to summarize the inDataSource.", sep=""))
    ```
    + Il primo argomento di rxSummary specifica la formula o il termine in base al quale eseguire il riepilogo. In questo esempio la funzione `F()` viene usata per convertire i valori di _passenger\_count_ in fattori prima di eseguire il riepilogo. È anche necessario specificare il valore minimo (1) e il valore massimo (6) per la variabile di fattore _passenger\_count_.
    + Se non si specificano le statistiche per l'output, per impostazione predefinita rxSummary genera i valori Mean, StDev, Min, Max e il numero di osservazioni valide e mancanti.
    + Questo esempio include anche una parte di codice per tenere traccia dell'ora in cui la funzione ha inizio e termina in modo da poter confrontare le prestazioni.
  
    **Risultati**

    Se la funzione rxSummary viene eseguita correttamente, verranno visualizzati risultati simili ai seguenti, seguiti da un elenco di statistiche per categoria. 

    ```R
    rxSummary(formula = ~fare_amount:F(passenger_count, 1,6), data = inDataSource)
    Data: inDataSource (RxSqlServerData Data Source)
    Number of valid observations: 1000
    ```

### <a name="bonus-exercise-on-big-data"></a>Esercizio bonus sui Big Data

Provare a definire una nuova stringa di query con tutte le righe. È consigliabile configurare un nuovo oggetto origine dati per questo esperimento. Si può anche provare a modificare il parametro *rowsToRead* per vedere come incide sulla velocità effettiva.

```R
bigDataQuery  <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"

bigDataSource <- RxSqlServerData(
      sqlQuery = bigDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )

start.time <- proc.time()
rxSummary(~fare_amount:F(passenger_count,1,6), data = bigDataSource)
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
  Elapsed Time=", round(used.time[3],2),
  " seconds to summarize the inDataSource.", sep=""))
```

> [!TIP]
> Durante l'esecuzione, è possibile usare uno strumento come [Esplora processi](/sysinternals/downloads/process-explorer) o SQL Profiler per vedere come viene eseguita la connessione e come viene eseguito il codice R usando i servizi di SQL Server.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare grafici e tracciati con R](walkthrough-create-graphs-and-plots-using-r.md)