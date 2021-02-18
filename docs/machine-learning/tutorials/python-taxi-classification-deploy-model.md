---
title: 'Esercitazione su Python: Eseguire le previsioni nelle stored procedure SQL'
titleSuffix: SQL machine learning
description: Nella quinta parte di questa serie di esercitazioni in cinque parti si renderà operativo lo script Python incorporato nelle stored procedure SQL con funzioni T-SQL con il Machine Learning di SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 39717778ca2456d77b6ceff4ba72e8de06dac75f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339701"
---
# <a name="python-tutorial-run-predictions-using-python-embedded-in-a-stored-procedure"></a>Esercitazione su Python: Eseguire stime usando Python incorporato in una stored procedure
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Nella quinta parte di questa serie di esercitazioni in cinque parti si apprenderà come *rendere operativi* i modelli di cui è stato eseguito il training e il salvataggio nella parte precedente.

In questo scenario, rendere operativo un modello significa distribuirlo nell'ambiente di produzione per l'assegnazione dei punteggi. L'integrazione con SQL Server semplifica questa operazione, perché è possibile incorporare il codice Python in una stored procedure. Per ottenere stime dal modello in base ai nuovi input, è sufficiente chiamare la stored procedure da un'applicazione e passare i nuovi dati.

Questa parte dell'esercitazione illustra due metodi per la creazione di previsioni basate su un modello Python: assegnazione dei punteggi batch e assegnazione dei punteggi riga per riga.

+ **Assegnazione dei punteggi batch:** per fornire più righe di dati di input, passare una query SELECT come argomento della stored procedure. Il risultato è una tabella di osservazioni corrispondenti ai casi di input.
+ **Assegnazione dei punteggi singoli**: passare come input un set di valori di parametro singoli.  La stored procedure restituisce una singola riga o un singolo valore.

Tutto il codice Python necessario per l'assegnazione dei punteggi viene fornito come parte delle stored procedure.

Contenuto dell'articolo:

> [!div class="checklist"]
> + Creare e usare le stored procedure per l'assegnazione dei punteggi batch
> + Creare e usare le stored procedure per l'assegnazione dei punteggi a una singola riga

Nella [prima parte](python-taxi-classification-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](python-taxi-classification-explore-data.md) sono stati esaminati i dati di esempio e sono stati generati alcuni tracciati.

Nella [terza parte](python-taxi-classification-create-features.md) si è appreso come creare funzionalità dai dati non elaborati tramite una funzione Transact-SQL. La funzione è stata quindi chiamata da una stored procedure per creare una tabella contenente i valori della funzionalità.

Nella [quarta parte](python-taxi-classification-train-model.md) sono stati caricati i moduli e sono state chiamate le funzioni necessarie per la creazione e il training del modello usando una stored procedure di SQL Server.

## <a name="batch-scoring"></a>Assegnazione dei punteggi batch

Le prime due stored procedure mostrano la sintassi di base per il wrapping di una chiamata di stima Python in una stored procedure. Entrambe le stored procedure richiedono una tabella di dati come input.

+ Il nome del modello esatto da usare viene fornito come parametro di input della stored procedure. La stored procedure carica il modello serializzato dalla tabella di database `nyc_taxi_models`, usando l'istruzione SELECT nella stored procedure.
+ Il modello serializzato viene archiviato nella variabile Python `mod` per un'ulteriore elaborazione con Python.
+ I nuovi casi a cui assegnare i punteggi vengono ottenuti dalla query [!INCLUDE[tsql](../../includes/tsql-md.md)] specificata in `@input_data_1`. Man mano che vengono letti i dati della query, le righe vengono salvate nel frame di dati predefinito `InputDataSet`.
+ Entrambe le stored procedure usano funzioni di `sklearn` per calcolare una metrica di accuratezza, ovvero l'area sotto la curva (AUC). È possibile generare metriche di accuratezza, come l'area sotto la curva, solo se si specifica anche l'etichetta di destinazione (la colonna _tipped_). Per le stime non è necessaria l'etichetta di destinazione (variabile `y`), mentre per il calcolo della metrica di accuratezza sì.

  Se quindi non si hanno etichette di destinazione per i dati a cui si vuole assegnare i punteggi, è possibile modificare la stored procedure per rimuovere i calcoli dell'area sotto la curva e restituire solo le probabilità della mancia in base alle caratteristiche (variabile `X` nella stored procedure).

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

Eseguire le istruzioni T-SQL seguenti per creare le stored procedure. Questa stored procedure richiede un modello basato sul pacchetto scikit-learn poiché usa funzioni specifiche del pacchetto.

Il frame di dati contenente gli input viene passato alla funzione `predict_proba` del modello di regressione logistica, `mod`. La funzione `predict_proba` (`probArray = mod.predict_proba(X)`) restituisce un valore **float** che rappresenta la probabilità che venga lasciata una mancia (di qualsiasi importo).

```sql
DROP PROCEDURE IF EXISTS PredictTipSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = mod.predict_proba(X)
probList = []
for i in range(len(probArray)):
  probList.append((probArray[i])[1])

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

Questa stored procedure usa gli stessi input e crea lo stesso tipo di punteggi della stored procedure precedente, ma usa le funzioni del pacchetto **revoscalepy** fornito con Machine Learning Services per SQL Server.

```sql
DROP PROCEDURE IF EXISTS PredictTipRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics
from revoscalepy.functions.RxPredict import rx_predict;

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = rx_predict(mod, X)
probList = probArray["tipped_Pred"].values 

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>Eseguire l'assegnazione dei punteggi batch usando una query SELECT

Le stored procedure **PredictTipSciKitPy** e **PredictTipRxPy** richiedono due parametri di input:

+ La query che recupera i dati per l'assegnazione dei punteggi
+ Il nome di un modello sottoposto a training

Passando questi argomenti alla stored procedure, è possibile selezionare un determinato modello o modificare i dati usati per l'assegnazione dei punteggi.

1. Per usare il modello **scikit-learn** per l'assegnazione dei punteggi, chiamare la stored procedure **PredictTipSciKitPy**, passando il nome del modello e la stringa di query come input.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
   ```

   La stored procedure restituisce le probabilità stimate per ogni corsa passata come parte della query di input. 

   Se si usa SSMS (SQL Server Management Studio) per l'esecuzione di query, le probabilità verranno visualizzate sotto forma di tabella nel riquadro **Risultati**. Il riquadro **Messaggi** restituisce la metrica di accuratezza (AUC o area sotto la curva) con un valore di circa 0,56.

2. Per usare il modello **revoscalepy** per l'assegnazione dei punteggi, chiamare la stored procedure **PredictTipRxPy**, passando il nome del modello e la stringa di query come input.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
   ```

## <a name="single-row-scoring"></a>Assegnazione dei punteggi alle singole righe

A volte, invece dell'assegnazione dei punteggi batch è preferibile passare un singolo caso, ottenere i valori da un'applicazione e restituire un singolo risultato basato su tali valori. È ad esempio possibile configurare un foglio di lavoro di Excel, un'applicazione Web o un report per chiamare la stored procedure e fornire input immessi o selezionati dagli utenti.

In questa sezione si apprenderà come creare stime singole chiamando due stored procedure:

+ [PredictTipSingleModeSciKitPy](#predicttipsinglemodescikitpy) consente l'assegnazione dei punteggi a singole righe usando il modello scikit-learn.
+ [PredictTipSingleModeRxPy](#predicttipsinglemoderxpy) consente l'assegnazione dei punteggi a singole righe usando il modello revoscalepy.
+ Se non è ancora stato eseguito il training di un modello, tornare alla [quinta parte](python-taxi-classification-train-model.md).

Entrambi i modelli accettano come input una serie di valori singoli, ad esempio il numero di passeggeri, la distanza della corsa e così via. Viene usata una funzione con valori di tabella, `fnEngineerFeatures`, per convertire i valori di latitudine e longitudine degli input in una nuova caratteristica, ovvero la distanza diretta. La [quarta parte](python-taxi-classification-create-features.md) include una descrizione di questa funzione con valori di tabella.

Entrambe le stored procedure creano un punteggio in base al modello Python.

> [!NOTE]
>
> È importante offrire tutte le funzionalità di input richieste dal modello Python quando si chiama la stored procedure da un'applicazione esterna. Per evitare errori, potrebbe essere necessario eseguire il cast dei dati di input o convertirli in un tipo di dati Python, oltre a convalidare il tipo di dati e la lunghezza dei dati.

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

Dedicare un attimo a esaminare il codice della stored procedure che esegue l'assegnazione dei punteggi usando il modello **scikit-learn**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)
probList = []
probList.append((mod.predict_proba(X)[0])[1])

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

La stored procedure seguente esegue l'assegnazione dei punteggi usando il modello **revoscalepy**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
  '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from revoscalepy.functions.RxPredict import rx_predict;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)

probArray = rx_predict(mod, X)

probList = []
probList = probArray["tipped_Pred"].values

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>Generare i punteggi dai modelli

Una volta create le stored procedure, è facile generare un punteggio in base a uno dei due modelli. È sufficiente aprire una nuova finestra **Query** e quindi inserire o incollare i parametri per ogni colonna delle caratteristiche. I sette valori necessari sono relativi alle colonne delle caratteristiche seguenti, nell'ordine indicato:

+ *passenger_count*
+ *trip_distance*
+ *trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. Per generare una stima usando il modello **revoscalepy**, eseguire l'istruzione seguente:
  
   ```sql
   EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

2. Per generare un punteggio usando il modello **scikit-learn**, eseguire l'istruzione seguente:

   ```sql
   EXEC [dbo].[PredictTipSingleModeSciKitPy] 'SciKit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

L'output di entrambe le procedure corrisponde alla probabilità che venga lasciata una mancia per la corsa in taxi con le caratteristiche o i parametri specificati.

## <a name="conclusions"></a>Conclusioni

In questa serie di esercitazioni si è appreso come usare il codice Python incorporato nelle stored procedure. L'integrazione con [!INCLUDE[tsql](../../includes/tsql-md.md)] semplifica notevolmente la distribuzione di modelli Python per le stime e l'incorporamento delle ripetizioni del training dei modelli nel flusso di lavoro dei dati dell'azienda.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Creare e usare le stored procedure per l'assegnazione dei punteggi batch
> + Creare e usare le stored procedure per l'assegnazione dei punteggi a una singola riga

Per altre informazioni su Python, vedere [Estensione Python in SQL Server](../concepts/extension-python.md).
