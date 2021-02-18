---
title: 'Esercitazione su Python: Eseguire il training e salvare un modello'
titleSuffix: SQL machine learning
description: Nella quarta parte di questa serie di esercitazioni in cinque parti si eseguirà il training e il salvataggio di un modello in Python usando Transact-SQL in SQL Server con il Machine Learning di SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 22e857ae1cf84ae30e5965015df6dffcc148800a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338270"
---
# <a name="python-tutorial-train-and-save-a-python-model-using-t-sql"></a>Esercitazione su Python: Eseguire il training e il salvataggio di un modello Python usando T-SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Nella quarta parte di questa serie di esercitazioni in cinque parti si apprenderà come eseguire il training di un modello di Machine Learning usando i pacchetti Python **scikit-learn** e **revoscalepy**. Queste librerie Python sono già installate con il Machine Learning di SQL Server.

Verranno caricati i moduli e verranno chiamate le funzioni necessarie per la creazione e il training del modello usando una stored procedure di SQL Server. Il modello richiede le funzionalità dei dati progettate nelle parti precedenti di questa serie di esercitazioni. Infine, si salverà il modello sottoposto a training in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Contenuto dell'articolo:

> [!div class="checklist"]
> + Creare ed eseguire il training di un modello usando una stored procedure SQL
> + Salvare il modello sottoposto a training in una tabella SQL

Nella [prima parte](python-taxi-classification-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](python-taxi-classification-explore-data.md) sono stati esaminati i dati di esempio e sono stati generati alcuni tracciati.

Nella [terza parte](python-taxi-classification-create-features.md) si è appreso come creare funzionalità dai dati non elaborati tramite una funzione Transact-SQL. La funzione è stata quindi chiamata da una stored procedure per creare una tabella contenente i valori della funzionalità.

Nella [quinta parte](python-taxi-classification-deploy-model.md) si apprenderà come rendere operativi i modelli sottoposti a training e salvati nella quarta parte.

## <a name="split-the-sample-data-into-training-and-testing-sets"></a>Suddividere i dati di esempio in set di training e di testing

1. Creare una stored procedure denominata **PyTrainTestSplit** per dividere i dati della tabella nyctaxi_sample in due parti: nyctaxi_sample_training e nyctaxi_sample_testing. 

   Questa stored procedure dovrebbe essere già stata creata, ma è possibile eseguire il codice seguente per crearla:

   ```sql
   DROP PROCEDURE IF EXISTS PyTrainTestSplit;
   GO

   CREATE PROCEDURE [dbo].[PyTrainTestSplit] (@pct int)
   AS
   
   DROP TABLE IF EXISTS dbo.nyctaxi_sample_training
   SELECT * into nyctaxi_sample_training FROM nyctaxi_sample WHERE (ABS(CAST(BINARY_CHECKSUM(medallion,hack_license)  as int)) % 100) < @pct
   
   DROP TABLE IF EXISTS dbo.nyctaxi_sample_testing
   SELECT * into nyctaxi_sample_testing FROM nyctaxi_sample
   WHERE (ABS(CAST(BINARY_CHECKSUM(medallion,hack_license)  as int)) % 100) > @pct
   GO
   ```

2. Per dividere i dati in base a una suddivisione personalizzata, eseguire la stored procedure e digitare un numero intero che rappresenta la percentuale di dati allocati al set di training. L'istruzione seguente, ad esempio, consente di allocare il 60% dei dati al set di training.

   ```sql
   EXEC PyTrainTestSplit 60
   GO
   ```

## <a name="build-a-logistic-regression-model"></a>Creare un modello di regressione logistica

Dopo aver preparato i dati, è possibile usarli per eseguire il training di un modello. A tale scopo, chiamare una stored procedure che esegue codice Python e accetta come input la tabella dei dati di training. Per questa esercitazione vengono creati due modelli, entrambi di classificazione binaria:

+ La stored procedure **PyTrainScikit** crea un modello di stima delle mance usando il pacchetto **scikit-learn**.
+ La stored procedure **TrainTipPredictionModelRxPy** crea un modello di stima delle mance usando il pacchetto **revoscalepy**.

Ogni stored procedure usa i dati di input forniti per eseguire la creazione e il training di un modello di regressione logistica. Viene eseguito il wrapping di tutto il codice Python nella stored procedure di sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Per semplificare la ripetizione del training del modello sui nuovi dati, è possibile eseguire il wrapping della chiamata a sp_execute_external_script in un'altra stored procedure e passare i nuovi dati di training come parametro. Il processo viene illustrato in questa sezione.

### <a name="pytrainscikit"></a>PyTrainScikit

1. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] aprire una nuova finestra **Query** ed eseguire l'istruzione seguente per creare la stored procedure **PyTrainScikit**.  La stored procedure include una definizione dei dati di input, quindi non è necessario specificare una query di input.

   ```sql
   DROP PROCEDURE IF EXISTS PyTrainScikit;
   GO

   CREATE PROCEDURE [dbo].[PyTrainScikit] (@trained_model varbinary(max) OUTPUT)
   AS
   BEGIN
   EXEC sp_execute_external_script
     @language = N'Python',
     @script = N'
   import numpy
   import pickle
   from sklearn.linear_model import LogisticRegression
   
   ##Create SciKit-Learn logistic regression model
   X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
   y = numpy.ravel(InputDataSet[["tipped"]])
   
   SKLalgo = LogisticRegression()
   logitObj = SKLalgo.fit(X, y)
   
   ##Serialize model
   trained_model = pickle.dumps(logitObj)
   ',
   @input_data_1 = N'
   select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, 
   dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
   from nyctaxi_sample_training
   ',
   @input_data_1_name = N'InputDataSet',
   @params = N'@trained_model varbinary(max) OUTPUT',
   @trained_model = @trained_model OUTPUT;
   ;
   END;
   GO
   ```

2. Eseguire le istruzioni SQL seguenti per inserire il modello sottoposto a training nella tabella nyc\_taxi_models.

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC PyTrainScikit @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('SciKit_model', @model);
   ```

   L'elaborazione dei dati e l'adattamento del modello possono richiedere alcuni minuti. I messaggi che verrebbero inoltrati al flusso **stdout** di Python sono visualizzati nella finestra **Messaggi** di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Ad esempio:

   ```text
   STDOUT message(s) from external script:
   C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\revoscalepy
   ```

3. Aprire la tabella *nyc\_taxi_models*. È possibile notare che è stata aggiunta una riga nuova contenente il modello serializzato nella colonna _model_.

   ```text
   SciKit_model
   0x800363736B6C6561726E2E6C696E6561....
   ```

### <a name="traintippredictionmodelrxpy"></a>TrainTipPredictionModelRxPy

Questa stored procedure usa il nuovo pacchetto **revoscalepy**, che è un nuovo pacchetto per Python. Tale pacchetto contiene oggetti, trasformazioni e algoritmi simili a quelli forniti per il pacchetto **RevoScaleR** del linguaggio R. 

Usando **revoscalepy**, è possibile creare contesti di calcolo remoti, spostare i dati tra contesti di calcolo, trasformare i dati ed eseguire il training di modelli predittivi usando gli algoritmi più diffusi, ad esempio la regressione logistica e lineare, gli alberi delle decisioni e altro ancora. Per altre informazioni, vedere [Modulo revoscalepy in SQL Server](../python/ref-py-revoscalepy.md) e [Informazioni di riferimento per le funzioni di revoscalepy](/r-server/python-reference/revoscalepy/revoscalepy-package).

1. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] aprire una nuova finestra **Query** ed eseguire l'istruzione seguente per creare la stored procedure _TrainTipPredictionModelRxPy_.  Poiché la stored procedure include già una definizione dei dati di input, non è necessario specificare una query di input.

   ```sql
   DROP PROCEDURE IF EXISTS TrainTipPredictionModelRxPy;
   GO

   CREATE PROCEDURE [dbo].[TrainTipPredictionModelRxPy] (@trained_model varbinary(max) OUTPUT)
   AS
   BEGIN
   EXEC sp_execute_external_script 
     @language = N'Python',
     @script = N'
   import numpy
   import pickle
   from revoscalepy.functions.RxLogit import rx_logit
   
   ## Create a logistic regression model using rx_logit function from revoscalepy package
   logitObj = rx_logit("tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance", data = InputDataSet);
   
   ## Serialize model
   trained_model = pickle.dumps(logitObj)
   ',
   @input_data_1 = N'
   select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, 
   dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
   from nyctaxi_sample_training
   ',
   @input_data_1_name = N'InputDataSet',
   @params = N'@trained_model varbinary(max) OUTPUT',
   @trained_model = @trained_model OUTPUT;
   ;
   END;
   GO
   ```

   Questa stored procedure esegue le attività seguenti come parte del training del modello:

   + La query SELECT applica la funzione scalare personalizzata _fnCalculateDistance_ per calcolare la distanza diretta tra la posizione di salita e quella di discesa dei passeggeri. I risultati della query vengono archiviati nella variabile di input Python predefinita `InputDataset`.
   + La variabile binaria _tipped_ viene usata come colonna dei risultati o *etichetta*. Il modello viene adattato usando le colonne delle caratteristiche seguenti: _passenger_count_, _trip_distance_, _trip_time_in_secs_ e _direct_distance_.
   + Il modello sottoposto a training viene serializzato e archiviato nella variabile Python `logitObj`. Aggiungendo la parola chiave T-SQL OUTPUT, è possibile aggiungere la variabile come output della stored procedure. Nel passaggio successivo tale variabile viene usata per inserire il codice binario del modello in una tabella di database _nyc_taxi_models_. Questo meccanismo semplifica l'archiviazione e il riutilizzo dei modelli.

2. Eseguire la stored procedure come indicato di seguito per inserire il modello **revoscalepy** sottoposto a training nella tabella *nyc_taxi_models*.

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC TrainTipPredictionModelRxPy @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('revoscalepy_model', @model);
   ```

   L'elaborazione dei dati e l'adattamento del modello possono richiedere un po' di tempo. I messaggi che verrebbero inoltrati al flusso **stdout** di Python sono visualizzati nella finestra **Messaggi** di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Ad esempio:

   ```text
   STDOUT message(s) from external script:
   C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\revoscalepy
   ```

3. Aprire la tabella *nyc_taxi_models*. È possibile notare che è stata aggiunta una riga nuova contenente il modello serializzato nella colonna _model_.

   ```text
   revoscalepy_model
   0x8003637265766F7363616c....
   ```

Nella parte successiva di questa esercitazione si useranno i modelli sottoposti a training per creare previsioni.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Creare ed eseguire il training di un modello usando una stored procedure SQL
> + Salvare il modello sottoposto a training in una tabella SQL

> [!div class="nextstepaction"]
> [Esercitazione su Python: Eseguire previsioni usando Python incorporato in una stored procedure](python-taxi-classification-deploy-model.md)