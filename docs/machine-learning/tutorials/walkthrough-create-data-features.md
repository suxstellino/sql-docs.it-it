---
title: 'Esercitazione su R: Progettazione delle funzioni'
description: Esercitazione che illustra come creare caratteristiche dei dati usando le funzioni di SQL Server per l'analisi nel database.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 502600980a3cb1f80eaeca64284f0a189cfcb405
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272982"
---
# <a name="create-data-features-using-r-and-sql-server-walkthrough"></a>Creare caratteristiche dei dati usando R e SQL Server (procedura dettagliata)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Il data engineering è un'area importante dell'apprendimento automatico. Spesso i dati devono essere trasformati per poter essere usati per la modellazione predittiva. Se i dati non dispongono delle funzionalità necessarie, è possibile crearle da valori esistenti.

Per questa attività di modellazione, anziché usare i valori raw di latitudine e longitudine dei punti di inizio e fine della corsa si vuole avere la distanza in miglia tra le due posizioni. Per creare questa caratteristica, si calcola la distanza lineare diretta tra due punti usando la [formula dell'emisenoverso](https://en.wikipedia.org/wiki/Haversine_formula).

In questo passaggio vengono illustrati due metodi diversi per la creazione di una caratteristica dai dati:

> [!div class="checklist"]
> * Uso di una funzione R personalizzata
> * Uso di una funzione T-SQL personalizzata in [!INCLUDE[tsql](../../includes/tsql-md.md)]

Lo scopo è quello di creare un nuovo set di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che include le colonne originali più la nuova caratteristica numerica *direct_distance*.

## <a name="prerequisites"></a>Prerequisiti

Questo passaggio presuppone una sessione R in corso basata sui passaggi precedenti di questa procedura dettagliata. Usa le stringhe di connessione e gli oggetti origine dati creati in tali passaggi. Per eseguire lo script vengono usati gli strumenti e i pacchetti seguenti.

+ Rgui.exe per eseguire i comandi R
+ Management Studio per eseguire T-SQL

## <a name="featurization-using-r"></a>Sviluppo di caratteristiche tramite R

Il linguaggio R è noto per le ricche e variate librerie statistiche, ma potrebbe essere ancora necessario creare trasformazioni di dati personalizzate.

In primo luogo, ci si atterrà alla procedura a cui sono abituati gli utenti di R: ottenere i dati nel portatile e quindi eseguire una funzione R personalizzata, *ComputeDist*, che calcola la distanza lineare tra due punti specificati da valori di latitudine e longitudine.

1. Tenere presente che l'oggetto origine dati creato in precedenza ottiene solo le prime 1000 righe. Definire quindi una query che ottiene tutti i dati.

    ```R
    bigQuery <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,  pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude FROM nyctaxi_sample";
    ```

2. Creare un nuovo oggetto origine dati usando la query.

    ```R
    featureDataSource <- RxSqlServerData(sqlQuery = bigQuery,colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric", dropoff_longitude = "numeric", dropoff_latitude = "numeric", passenger_count  = "numeric", trip_distance  = "numeric", trip_time_in_secs  = "numeric", direct_distance  = "numeric"), connectionString = connStr);
    ```

    - [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata) può accettare sia una query costituita da una query SELECT valida, fornita come argomento del parametro _sqlQuery_, sia il nome di un oggetto tabella, fornito come parametro _table_.
    
    - Se si vuole campionare i dati da una tabella, è necessario usare il parametro _sqlQuery_, definire i parametri di campionamento usando la clausola T-SQL TABLESAMPLE e impostare l'argomento _rowBuffering_ su FALSE.

3. Eseguire il codice seguente per creare la funzione R personalizzata. ComputeDist accetta due coppie di valori di latitudine e longitudine e calcola la distanza lineare fra di esse restituendo la distanza in miglia.

    ```R
    env <- new.env();
    env$ComputeDist <- function(pickup_long, pickup_lat, dropoff_long, dropoff_lat){
      R <- 6371/1.609344 #radius in mile
      delta_lat <- dropoff_lat - pickup_lat
      delta_long <- dropoff_long - pickup_long
      degrees_to_radians = pi/180.0
      a1 <- sin(delta_lat/2*degrees_to_radians)
      a2 <- as.numeric(a1)^2
      a3 <- cos(pickup_lat*degrees_to_radians)
      a4 <- cos(dropoff_lat*degrees_to_radians)
      a5 <- sin(delta_long/2*degrees_to_radians)
      a6 <- as.numeric(a5)^2
      a <- a2+a3*a4*a6
      c <- 2*atan2(sqrt(a),sqrt(1-a))
      d <- R*c
      return (d)
    }
    ```
  
    + La prima riga definisce un nuovo ambiente. In R un ambiente può essere usato per incapsulare gli spazi dei nomi in pacchetti e strutture simili. È possibile usare la funzione `search()` per visualizzare gli ambienti nell'area di lavoro. Per visualizzare gli oggetti in un ambiente specifico, digitare `ls(<envname>)`.
    + Le righe che iniziano con `$env.ComputeDist` contengono il codice di definizione della formula dell'emisenoverso, che calcola la *distanza ortodromica* tra due punti su una sfera.

4. Dopo aver definito la funzione la si applica ai dati per creare una nuova colonna della caratteristica, *direct_distance*. Prima di eseguire la trasformazione, tuttavia, impostare il contesto di calcolo su local.

    ```R
    rxSetComputeContext("local");
    ```

5. Chiamare la funzione [rxDataStep](/r-server/r-reference/revoscaler/rxdatastep) per ottenere i dati di progettazione della caratteristica e applicare la funzione `env$ComputeDist` ai dati in memoria.

    ```R
    start.time <- proc.time();
  
    changed_ds <- rxDataStep(inData = featureDataSource,
    transforms = list(direct_distance=ComputeDist(pickup_longitude,pickup_latitude, dropoff_longitude, dropoff_latitude),
    tipped = "tipped", fare_amount = "fare_amount", passenger_count = "passenger_count",
    trip_time_in_secs = "trip_time_in_secs",  trip_distance="trip_distance",
    pickup_datetime = "pickup_datetime",  dropoff_datetime = "dropoff_datetime"),
    transformEnvir = env,
    rowsPerRead=500,
    reportProgress = 3);
  
    used.time <- proc.time() - start.time;
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""));
    ```

    + La funzione rxDataStep supporta vari metodi per la modifica dei dati sul posto. Per altre informazioni, vedere questo articolo:  [Come trasformare i dati e creare subset in Microsoft R](/r-server/r/how-to-revoscaler-data-transform)
    
    Ci sono tuttavia un paio di aspetti da considerare relativi a rxDataStep: 
    
    In altre origini dati è possibile usare gli argomenti *varsToKeep* e *varsToDrop*, che non sono invece supportati per le origini dati di SQL Server. In questo esempio è stato quindi usato l'argomento _transforms_ per specificare sia le colonne pass-through che le colonne trasformate. In caso di esecuzione in un contesto di calcolo di SQL Server, inoltre, l'argomento _inData_ può accettare solo un'origine dati di SQL Server.

    Il codice precedente può inoltre generare un messaggio di avviso quando l'esecuzione avviene su set di dati più grandi. Quando il numero di righe per il numero di colonne da creare supera un valore impostato (il valore predefinito è 3 milioni), rxDataStep restituisce un avviso e il numero di righe nel frame di dati restituito viene troncato. Per rimuovere l'avviso, è possibile modificare l'argomento _maxRowsByCols_ nella funzione rxDataStep. Se tuttavia il valore di _maxRowsByCols_ è troppo grande, possono verificarsi problemi quando il frame di dati viene caricato in memoria.

7. Facoltativamente, è possibile chiamare [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) per esaminare lo schema dell'origine dati trasformata.

    ```R
    rxGetVarInfo(data = changed_ds);
    ```

## <a name="featurization-using-transact-sql"></a>Definizione delle funzionalità con Transact-SQL

In questo esercizio si apprenderà come eseguire la stessa attività usando le funzioni SQL invece delle funzioni R personalizzate. 

Passare a [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) o a un altro editor di query per eseguire lo script T-SQL.

1. Usare una funzione SQL denominata *fnCalculateDistance*. La funzione dovrebbe essere già presente nel database NYCTaxi_Sample. In Esplora oggetti verificare che la funzione sia presente passando a questo percorso: Database > NYCTaxi_Sample > Programmazione > Funzioni > Funzioni a valori scalari > dbo.fnCalculateDistance.

    Se la funzione non è presente, usare SQL Server Management Studio per generarla nel database NYCTaxi_Sample.

    ```sql
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
    -- User-defined function calculates the direct distance between two geographical coordinates.
    RETURNS decimal(28, 10)
    AS
    BEGIN
      DECLARE @distance decimal(28, 10)
      -- Convert to radians
      SET @Lat1 = @Lat1 / 57.2958
      SET @Long1 = @Long1 / 57.2958
      SET @Lat2 = @Lat2 / 57.2958
      SET @Long2 = @Long2 / 57.2958
      -- Calculate distance
      SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
      --Convert to miles
      IF @distance <> 0
      BEGIN
        SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
      END
      RETURN @distance
    END
    ```

2. In Management Studio, in una nuova finestra di query eseguire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente da qualsiasi applicazione che supporti [!INCLUDE[tsql](../../includes/tsql-md.md)] per vedere l'effetto della funzione.

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude 
    FROM nyctaxi_sample
    ```
3. Per inserire i valori direttamente in una nuova tabella (che è prima necessario creare), è possibile aggiungere una clausola **INTO** specificando il nome della tabella.

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude
    INTO NewFeatureTable
    FROM nyctaxi_sample
    ```

4. È anche possibile chiamare la funzione SQL dal codice R. Tornare a Rgui e archiviare la query di sviluppo di caratteristiche SQL in una variabile R.

    ```R
    featureEngineeringQuery = "SELECT tipped, fare_amount, passenger_count,
        trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
        dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
        pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
        FROM nyctaxi_sample
        tablesample (1 percent) repeatable (98052)"
    ```
  
    > [!TIP]
    > Questa query è stata modificata per ottenere un campione di dati più piccolo, al fine di velocizzare questa procedura dettagliata. Se si vuole ottenere tutti i dati, è possibile rimuovere la clausola TABLESAMPLE. Tuttavia, a seconda dell'ambiente, potrebbe non essere possibile caricare il set di dati completo in R, con la conseguente generazione di un errore.
  
5. Usare le righe di codice seguenti per chiamare la funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] dall'ambiente R e applicarla ai dati definiti in *featureEngineeringQuery*.
  
    ```R
    featureDataSource = RxSqlServerData(sqlQuery = featureEngineeringQuery,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
        dropoff_longitude = "numeric", dropoff_latitude = "numeric",
        passenger_count  = "numeric", trip_distance  = "numeric",
        trip_time_in_secs  = "numeric", direct_distance  = "numeric"),
      connectionString = connStr)
    ```
  
6.  Dopo aver creato la nuova caratteristica, chiamare **rxGetVarsInfo** per creare un riepilogo dei dati nella tabella delle caratteristiche.
  
    ```R
    rxGetVarInfo(data = featureDataSource)
    ```

    **Risultati**

    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: numeric
    Var 4: trip_time_in_secs, Type: numeric
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: direct_distance, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: pickup_longitude, Type: numeric
    Var 11: dropoff_latitude, Type: numeric
    Var 12: dropoff_longitude, Type: numeric
    ```

    > [!NOTE]
    > In alcuni casi può venire generato un errore come il seguente: *Autorizzazione EXECUTE negata per l'oggetto 'fnCalculateDistance'* . In tal caso, verificare che l'account di accesso in uso disponga delle autorizzazioni per eseguire script e creare oggetti nel database, non solo nell'istanza.
    > Controllare lo schema per l'oggetto fnCalculateDistance. Se l'oggetto è stato creato dal proprietario del database e l'account di accesso appartiene al ruolo db_datareader, è necessario concedere all'account di accesso le autorizzazioni esplicite per eseguire lo script.

## <a name="comparing-r-functions-and-sql-functions"></a>Confronto tra funzioni R e funzioni SQL

In precedenza è stato usato questo frammento di codice per calcolare il tempo di esecuzione del codice R.

```R
start.time <- proc.time()
<your code here>
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""))
```

È possibile provare a usarlo con l'esempio di funzione SQL personalizzata per calcolare il tempo necessario per la trasformazione dei dati quando si chiama una funzione SQL. Provare anche a cambiare i contesti di calcolo con rxSetComputeContext e confrontare le tempistiche.

I tempi possono variare in modo significativo, a seconda della velocità di rete e della configurazione hardware. Nelle configurazioni testate, l'uso della funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] ha consentito di ottenere tempi più rapidi rispetto all'uso di una funzione R personalizzata. Nei passaggi successivi si userà quindi la funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] per tali calcoli.

> [!TIP]
> Molto spesso, la progettazione di caratteristiche con [!INCLUDE[tsql](../../includes/tsql-md.md)] è più veloce che con R. T-SQL include ad esempio funzioni di rango e finestra rapide che è possibile applicare a calcoli di data science comuni, come le medie mobili in sequenza e la classificazione di valori in base agli intervalli (*n*-tile). Scegliere il metodo più efficace in base ai dati e all'attività eseguita.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare un modello R e salvarlo in SQL](walkthrough-build-and-save-the-model.md)