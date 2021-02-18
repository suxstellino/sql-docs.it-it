---
title: 'Esercitazione su R: Distribuire il modello'
description: Informazioni su come distribuire modelli R in un ambiente di produzione chiamando un modello sottoposto a training da una stored procedure.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1830c441e51465f9c7cf7eab7ca503eadd677caf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272972"
---
# <a name="deploy-the-r-model-and-use-it-in-sql-server-walkthrough"></a>Distribuire il modello R e usarlo in SQL Server (procedura dettagliata)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

In questa lezione si apprenderà come distribuire modelli R in un ambiente di produzione chiamando un modello sottoposto a training da una stored procedure. Sarà quindi possibile richiamare la stored procedure da R o da qualsiasi linguaggio di programmazione di applicazioni che supporti [!INCLUDE[tsql](../../includes/tsql-md.md)] (ad esempio C#, Java, Python e così via) e usare il modello per creare stime sulle nuove osservazioni.

Questo articolo illustra i due modi più comuni per usare un modello per l'assegnazione dei punteggi:

> [!div class="checklist"]
> * La **modalità di assegnazione dei punteggi batch** genera più stime
> * La **modalità di assegnazione dei punteggi singoli** genera stime una alla volta

## <a name="batch-scoring"></a>Assegnazione dei punteggi batch

Creare una stored procedure che genera più stime, *PredictTipBatchMode*, passando una query o una tabella SQL come input. Viene restituita una tabella di risultati, che è possibile inserire direttamente in una tabella o scrivere in un file.

- Ottiene un set di dati di input come query SQL
- Chiama il modello di regressione logistica di cui è stato eseguito il training e che è stato salvato nella lezione precedente
- Stima la probabilità che il tassista riceva una mancia diversa da zero

1. In Management Studio aprire una nuova finestra di query ed eseguire lo script T-SQL seguente per creare la stored procedure PredictTipBatchMode.
  
    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipBatchMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipBatchMode] @input nvarchar(max)
    AS
    BEGIN
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model  FROM nyc_taxi_models);
      EXEC sp_execute_external_script @language = N'R',
         @script = N'
           mod <- unserialize(as.raw(model));
           print(summary(mod))
           OutputDataSet<-rxPredict(modelObject = mod,
             data = InputDataSet,
             outData = NULL,
             predVarNames = "Score", type = "response",
             writeModelVars = FALSE, overwrite = TRUE);
           str(OutputDataSet)
           print(OutputDataSet)',
      @input_data_1 = @input,
      @params = N'@model varbinary(max)',
      @model = @lmodel2
      WITH RESULT SETS ((Score float));
    END
    ```

    + Usare un'istruzione SELECT per chiamare il modello archiviato da una tabella SQL. Il modello viene recuperato dalla tabella sotto forma di dati **varbinary(max)** , archiviato nella variabile SQL _\@lmodel2_ e passato come parametro *mod* alla stored procedure di sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

    + I dati usati come input per il punteggio vengono definiti come query SQL e archiviati come stringa nella variabile SQL _\@input_. I dati recuperati dal database vengono archiviati in un frame di dati denominato *InputDataSet*, che è semplicemente il nome predefinito per i dati di input della stored procedure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Se necessario, è possibile definire un altro nome di variabile usando il parametro _\@input_data_1_name_.

    + Per generare i punteggi, la stored procedure chiama la funzione rxPredict dalla libreria **RevoScaleR**.

    + Il valore restituito, *Score*, è la probabilità, dato il modello, che il conducente ottenga una mancia. Facoltativamente, si può applicare facilmente un filtro ai valori restituiti per suddividerli in gruppi "con mancia" o "senza mancia".  Ad esempio, una probabilità minore dello 0,5 indica che probabilmente non verrà data alcuna mancia.
  
2.  Per chiamare la stored procedure in modalità batch, è necessario definire la query necessaria come input per la stored procedure. Di seguito è riportata la query SQL, che è possibile eseguire in SSMS per verificarne il funzionamento.

    ```sql
    SELECT TOP 10
      a.passenger_count AS passenger_count,
      a.trip_time_in_secs AS trip_time_in_secs,
      a.trip_distance AS trip_distance,
      a.dropoff_datetime AS dropoff_datetime,
      dbo.fnCalculateDistance( pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance
      FROM 
        (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude 
        FROM nyctaxi_sample)a 
      LEFT OUTER JOIN
      ( SELECT medallion, hack_license, pickup_datetime
      FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b
      ON a.medallion=b.medallion
      AND a.hack_license=b.hack_license
      AND a.pickup_datetime=b.pickup_datetime
      WHERE b.medallion is null
    ```

3. Usare questo codice R per creare la stringa di input dalla query SQL:

    ```R
    input <- "N'SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a LEFT OUTER JOIN ( SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND  a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'";
    q <- paste("EXEC PredictTipBatchMode @input = ", input, sep="");
    ```

4. Per eseguire la stored procedure da R, chiamare il metodo **sqlQuery** del pacchetto **RODBC** e usare la connessione SQL `conn` definita in precedenza:

    ```R
    sqlQuery (conn, q);
    ```

    Se si riceve un errore ODBC, controllare se sono presenti errori di sintassi e se è usato il numero corretto di virgolette. 
    
    Se si riceve un errore di autorizzazione, verificare che l'account di accesso possa eseguire la stored procedure.

## <a name="single-row-scoring"></a>Assegnazione dei punteggi alle singole righe

La modalità di assegnazione dei punteggi singoli genera stime una alla volta, passando un set di singoli valori alla stored procedure come input. I valori corrispondono alle caratteristiche nel modello, che il modello usa per creare una stima o per generare un altro risultato, ad esempio un valore di probabilità. È quindi possibile restituire tale valore all'applicazione o all'utente.

Quando si chiama il modello per una stima riga per riga, si passa un set di valori che rappresentano le caratteristiche per ogni singolo caso. La stored procedure restituisce quindi una singola stima o probabilità. 

La stored procedure *PredictTipSingleMode* dimostra questo approccio. Accetta come input più parametri che rappresentano i valori delle caratteristiche, ad esempio il numero di passeggeri e la distanza della corsa, assegna un punteggio alle caratteristiche usando il modello R archiviato e restituisce la probabilità della mancia.

1. Eseguire l'istruzione Transact-SQL seguente per creare la stored procedure.

    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipSingleMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipSingleMode] @passenger_count int = 0,
    @trip_distance float = 0,
    @trip_time_in_secs int = 0,
    @pickup_latitude float = 0,
    @pickup_longitude float = 0,
    @dropoff_latitude float = 0,
    @dropoff_longitude float = 0
    AS
    BEGIN
      DECLARE @inquery nvarchar(max) = N'
        SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs, @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)'
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model FROM nyc_taxi_models);

      EXEC sp_execute_external_script @language = N'R',  @script = N'
            mod <- unserialize(as.raw(model));
            print(summary(mod))
            OutputDataSet<-rxPredict(
              modelObject = mod,
              data = InputDataSet,
              outData = NULL,
              predVarNames = "Score",
              type = "response",
              writeModelVars = FALSE,
              overwrite = TRUE);
            str(OutputDataSet)
            print(OutputDataSet)
            ',
      @input_data_1 = @inquery,
      @params = N'
      -- passthrough columns
      @model varbinary(max) ,
      @passenger_count int ,
      @trip_distance float ,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      -- mapped variables
      @model = @lmodel2 ,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance ,
      @trip_time_in_secs=@trip_time_in_secs ,
      @pickup_latitude=@pickup_latitude ,
      @pickup_longitude=@pickup_longitude ,
      @dropoff_latitude=@dropoff_latitude ,
      @dropoff_longitude=@dropoff_longitude
      WITH RESULT SETS ((Score float));
    END
    ```

2. In SQL Server Management Studio è possibile usare la procedura [!INCLUDE[tsql](../../includes/tsql-md.md)] **EXEC** (o **EXECUTE**) per chiamare la stored procedure e passarle gli input necessari. Provare ad esempio a eseguire questa istruzione in Management Studio:

    ```sql
    EXEC [dbo].[PredictTipSingleMode] 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

    Vengono passati i valori, rispettivamente, per le variabili _passenger\_count_, _trip_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ e _dropoff\_longitude_.

3. Per eseguire questa stessa chiamata dal codice R, è sufficiente definire una variabile R contenente l'intera chiamata alla stored procedure, come la seguente:

    ```R
    q2 = "EXEC PredictTipSingleMode 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303 ";
    ```

    Vengono passati i valori, rispettivamente, per le variabili _passenger\_count_, _trip\_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ e _dropoff\_longitude_.

4. Chiamare `sqlQuery` dal pacchetto **RODBC** e passare la stringa di connessione e la variabile di stringa contenente la chiamata alla stored procedure.

    ```R
    # predict with stored procedure in single mode
    sqlQuery (conn, q2);
    ```

    >[!TIP]
    > R Tools per Visual Studio (RTVS) offre un'ottima integrazione con SQL Server e R. Per altri esempi sull'uso di RODBC con una connessione SQL Server, vedere questo articolo: [Usare SQL Server e R](/visualstudio/rtvs/sql-server)

## <a name="next-steps"></a>Passaggi successivi

Dopo aver appreso come usare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e aver salvato permanentemente i modelli R sottoposti a training in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], risulterà semplice creare nuovi modelli basati sul set di dati. Ad esempio, è possibile provare a creare i modelli seguenti:

+ Un modello di regressione che stima l'importo della mancia
+ Un modello di classificazione multiclasse che stima l'importo piccolo, medio o grande della mancia

Può inoltre essere utile esplorare questi esempi e risorse aggiuntivi:

+ [Scenari di analisi scientifica dei dati e modelli di soluzioni](data-science-scenarios-and-solution-templates.md)
+ [Analisi avanzata nel database](r-taxi-classification-introduction.md)
+ [Guide pratiche su Machine Learning Server](/machine-learning-server/r/how-to-introduction)
+ [Risorse aggiuntive su Machine Learning Server](/machine-learning-server/resources-more)