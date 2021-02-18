---
title: 'Esercitazione su R: Eseguire le previsioni nelle stored procedure SQL'
titleSuffix: SQL machine learning
description: Nella quinta parte di questa serie di esercitazioni in cinque parti si renderà operativo lo script R incorporato nelle stored procedure SQL con funzioni T-SQL con il Machine Learning di SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: a3c46e719f4889ee53c98e671bd9569193b86e2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349061"
---
# <a name="r-tutorial-run-predictions-in-sql-stored-procedures"></a>Esercitazione su R: Eseguire le previsioni nelle stored procedure SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Nella quinta parte di questa serie di esercitazioni in cinque parti si apprenderà come *rendere operativo* il modello di cui è stato eseguito il training e il salvataggio nella parte precedente usando il modello per prevedere i possibili risultati. È possibile eseguire il wrapping del modello in una stored procedure che può essere chiamata direttamente da altre applicazioni.

Questo articolo illustra due metodi di assegnazione dei punteggi:

+ **Modalità di assegnazione dei punteggi batch**: usare una query SELECT come input per la stored procedure. La stored procedure restituisce una tabella di osservazioni corrispondenti ai casi di input.

+ **Modalità di valutazione singola**: passare come input un set di valori di parametro singoli.  La stored procedure restituisce una singola riga o un singolo valore.

Contenuto dell'articolo:

> [!div class="checklist"]
> + Creare e usare le stored procedure per l'assegnazione dei punteggi batch
> + Creare e usare le stored procedure per l'assegnazione dei punteggi a una singola riga

Nella [prima parte](r-taxi-classification-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](r-taxi-classification-explore-data.md) sono stati esaminati i dati di esempio e sono stati generati alcuni tracciati.

Nella [terza parte](r-taxi-classification-create-features.md) si è appreso come creare funzionalità dai dati non elaborati tramite una funzione Transact-SQL. La funzione è stata quindi chiamata da una stored procedure per creare una tabella contenente i valori della funzionalità.

Nella [quarta parte](r-taxi-classification-train-model.md) sono stati caricati i moduli e sono state chiamate le funzioni necessarie per la creazione e il training del modello usando una stored procedure di SQL Server.

## <a name="basic-scoring"></a>Assegnazione dei punteggi di base

La stored procedure **PredictTip** illustra la sintassi di base per il wrapping di una chiamata `PREDICT` in una stored procedure.

```sql
CREATE PROCEDURE [dbo].[RPredict] (@model varchar(250), @inquery nvarchar(max))
AS 
BEGIN 

DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);  
EXEC sp_execute_external_script @language = N'R',
  @script = N' 
    mod <- unserialize(as.raw(model));
    print(summary(mod))
    OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
    str(OutputDataSet)
    print(OutputDataSet)
    ',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max)',
  @model = @lmodel2 
  WITH RESULT SETS (("Score" float));
END
GO
```

+ L'istruzione SELECT ottiene il modello serializzato dal database e lo archivia nella variabile R `mod` per l'ulteriore elaborazione con R.

+ I nuovi casi per l'assegnazione dei punteggi vengono ottenuti dalla query [!INCLUDE[tsql](../../includes/tsql-md.md)] specificata in `@inquery`, il primo parametro della stored procedure. Man mano che vengono letti i dati della query, le righe vengono salvate nel frame di dati predefinito `InputDataSet`. Questo frame di dati viene passato alla funzione [PREDICT](../../t-sql/queries/predict-transact-sql.md), che genera i punteggi.
  
  `OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));`
  
  Poiché un data.frame può contenere una riga singola, è possibile usare lo stesso codice per la valutazione batch e la valutazione singola.
  
+ Il valore restituito dalla funzione `PREDICT` è un tipo **float** che rappresenta la probabilità che l'autista riceva una mancia di qualsiasi importo.

## <a name="batch-scoring-a-list-of-predictions"></a>Assegnazione dei punteggi batch (elenco di stime)

Uno scenario più comune consiste nel generare stime per più osservazioni in modalità batch. In questo passaggio si esaminerà il funzionamento dell'assegnazione dei punteggi batch.

1. Iniziare ottenendo un set di dati di input più piccolo sul quale lavorare. Questa query crea un elenco "top 10" delle corse, con il numero di passeggeri e altre funzionalità necessarie per effettuare una stima.
  
   ```sql
   SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance
   
   FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a

   LEFT OUTER JOIN

   (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052)    )b

   ON a.medallion=b.medallion AND a.hack_license=b.hack_license 
   AND a.pickup_datetime=b.pickup_datetime
   WHERE b.medallion IS NULL
   ```

   **Risultati dell'esempio**

   ```text
   passenger_count   trip_time_in_secs    trip_distance  dropoff_datetime          direct_distance
   1                 283                  0.7            2013-03-27 14:54:50.000   0.5427964547
   1                 289                  0.7            2013-02-24 12:55:29.000   0.3797099614
   1                 214                  0.7            2013-06-26 13:28:10.000   0.6970098661
   ```

2. Creare una stored procedure denominata **RPredictBatchOutput** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

   ```sql
   CREATE PROCEDURE [dbo].[RPredictBatchOutput] (@model varchar(250), @inquery nvarchar(max))
   AS
   BEGIN
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script 
     @language = N'R',
     @script = N'
       mod <- unserialize(as.raw(model));
       print(summary(mod))
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet)
       print(OutputDataSet)
     ',
     @input_data_1 = @inquery,
     @params = N'@model varbinary(max)',
     @model = @lmodel2
     WITH RESULT SETS ((Score float));
   END
   ```

3. Specificare il testo della query in una variabile da passare come parametro alla stored procedure:

   ```sql
   -- Define the input data
   DECLARE @query_string nvarchar(max)
   SET @query_string='SELECT TOP 10 a.passenger_count as passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance FROM  (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample  )a   LEFT OUTER JOIN (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052))b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'
   
   -- Call the stored procedure for scoring and pass the input data
   EXEC [dbo].[RPredictBatchOutput] @model = 'RTrainLogit_model', @inquery = @query_string;
   ```
  
La stored procedure restituisce una serie di valori che rappresentano la stima per ognuna delle corse "top 10". Tuttavia, queste corse sono anche corse con un singolo passeggero e su una distanza relativamente breve, per cui è poco probabile che l'autista ottenga una mancia.

> [!TIP]
> Invece di restituire solo risultati che indicano se la mancia viene corrisposta o no, è anche possibile restituire un punteggio di probabilità per la stima, quindi applicare una clausola WHERE ai valori della colonna _Score_ per classificare il punteggio per indicare una mancia probabile o una mancia poco probabile, usando un valore di soglia, ad esempio 0,5 o 0,7. Questo passaggio non è incluso nella stored procedure, ma la sua implementazione non sarebbe difficile.

## <a name="single-row-scoring-of-multiple-inputs"></a>Assegnazione dei punteggi alle singole righe per più input

A volte si vuole passare più valori di input e ottenere una singola stima basata su tali valori. È ad esempio possibile configurare un foglio di lavoro di Excel, un'applicazione Web o un report di Reporting Services per chiamare la stored procedure e fornire input immessi o selezionati dagli utenti dalle applicazioni.

In questa sezione viene illustrato come creare stime singole usando una stored procedure che accetta più input, come il numero di passeggeri, la distanza della corsa e così via. La stored procedure crea un punteggio in base al modello R archiviato in precedenza.
  
Se si chiama la stored procedure da un'applicazione esterna, assicurarsi che i dati soddisfino i requisiti del modello R. Ciò può includere la verifica che i dati di input siano sottoponibili a cast o convertibili in un tipo di dati di R o la convalida del tipo di dati e della lunghezza dei dati.

1. Creare una stored procedure **RPredictSingleRow**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPredictSingleRow] @model varchar(50), @passenger_count int = 0, @trip_distance float = 0, @trip_time_in_secs int = 0, @pickup_latitude float = 0, @pickup_longitude float = 0, @dropoff_latitude float = 0, @dropoff_longitude float = 0
   AS
   BEGIN
   DECLARE @inquery nvarchar(max) = N'SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs,  @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)';
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script  
     @language = N'R',
     @script = N'  
       mod <- unserialize(as.raw(model));  
       print(summary(mod));  
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet);
       print(OutputDataSet); 
       ',  
     @input_data_1 = @inquery,  
     @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,@trip_time_in_secs int ,  @pickup_latitude float ,@pickup_longitude float ,@dropoff_latitude float ,@dropoff_longitude float', @model = @lmodel2, @passenger_count =@passenger_count, @trip_distance=@trip_distance, @trip_time_in_secs=@trip_time_in_secs, @pickup_latitude=@pickup_latitude, @pickup_longitude=@pickup_longitude, @dropoff_latitude=@dropoff_latitude, @dropoff_longitude=@dropoff_longitude  
     WITH RESULT SETS ((Score float));  
   END
   ```

2. Provare la stored procedure inserendo manualmente i valori.
  
   Aprire una nuova finestra **Query** e chiamare la stored procedure specificando i valori per ognuno dei parametri. I parametri rappresentano le colonne delle caratteristiche usate dal modello e sono obbligatori.

   ```sql
   EXEC [dbo].[RPredictSingleRow] @model = 'RTrainLogit_model',
   @passenger_count = 1,
   @trip_distance = 2.5,
   @trip_time_in_secs = 631,
   @pickup_latitude = 40.763958,
   @pickup_longitude = -73.973373,
   @dropoff_latitude =  40.782139,
   @dropoff_longitude = -73.977303
   ```

   In alternativa, usare questa forma più breve supportata per i [parametri di una stored procedure](../../relational-databases/stored-procedures/specify-parameters.md):
  
   ```sql
   EXEC [dbo].[RPredictSingleRow] 'RTrainLogit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

3. I risultati indicano che la probabilità di ricevere una mancia è bassa (zero) in queste corse "top 10", perché sono tutte corse con un solo passeggero e su una distanza relativamente breve.

## <a name="conclusions"></a>Conclusioni

Ora che si è appreso come incorporare il codice R nelle stored procedure, è possibile estendere queste procedure per creare modelli personalizzati. L'integrazione con [!INCLUDE[tsql](../../includes/tsql-md.md)] semplifica notevolmente la distribuzione di modelli R per le stime e l'inclusione delle ripetizioni del training dei modelli nel flusso di lavoro dei dati enterprise.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Creare e usare le stored procedure per l'assegnazione dei punteggi batch
> + Creare e usare le stored procedure per l'assegnazione dei punteggi a una singola riga

Per altre informazioni su R, vedere [Estensione R in SQL Server](../concepts/extension-r.md).