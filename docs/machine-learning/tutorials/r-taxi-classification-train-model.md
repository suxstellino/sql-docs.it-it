---
title: 'Esercitazione su R: Eseguire il training e salvare un modello'
titleSuffix: SQL machine learning
description: Nella quarta parte di questa serie di esercitazioni in cinque parti si eseguirà il training e il salvataggio di un modello in R usando Transact-SQL in SQL Server con il Machine Learning di SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 3b5606b9f14006632bfa2782d1d547b4565e36ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350928"
---
# <a name="r-tutorial-train-and-save-model"></a>Esercitazione su R: Eseguire il training e salvare un modello
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Nella quarta parte di questa serie di esercitazioni in cinque parti si apprenderà come eseguire il training di un modello di Machine Learning usando R. Si eseguirà il training del modello usando le funzionalità dei dati create nella parte precedente e quindi si salverà il modello sottoposto a training in una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questo caso, i pacchetti R sono già installati con [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], quindi è possibile eseguire tutte le operazioni da SQL.

Contenuto dell'articolo:

> [!div class="checklist"]
> + Creare ed eseguire il training di un modello usando una stored procedure SQL
> + Salvare il modello sottoposto a training in una tabella SQL

Nella [prima parte](r-taxi-classification-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](r-taxi-classification-explore-data.md) sono stati esaminati i dati di esempio e sono stati generati alcuni tracciati.

Nella [terza parte](r-taxi-classification-create-features.md) si è appreso come creare funzionalità dai dati non elaborati tramite una funzione Transact-SQL. La funzione è stata quindi chiamata da una stored procedure per creare una tabella contenente i valori della funzionalità.

Nella [quinta parte](r-taxi-classification-deploy-model.md) si apprenderà come rendere operativi i modelli sottoposti a training e salvati nella quarta parte.

## <a name="create-the-stored-procedure"></a>Creare la stored procedure

Quando si chiama R da T-SQL, si usa la stored procedure di sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Per i processi che vengono ripetuti spesso, come la ripetizione del training di un modello, è tuttavia più semplice incapsulare la chiamata a `sp_execute_external_script` in un'altra stored procedure.

1. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] aprire una nuova finestra **Query**.

2. Eseguire l'istruzione seguente per creare la stored procedure **RTrainLogitModel**. Questa stored procedure definisce i dati di input e usa **glm** per creare un modello di regressione logistica.

   ```sql
   CREATE PROCEDURE [dbo].[RTrainLogitModel] (@trained_model varbinary(max) OUTPUT)
   
   AS
   BEGIN
     DECLARE @inquery nvarchar(max) = N'
       select tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance,
       pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
       from nyctaxi_sample
       tablesample (70 percent) repeatable (98052)
   '
   
     EXEC sp_execute_external_script @language = N'R',
                                     @script = N'
   ## Create model
   logitObj <- glm(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = InputDataSet, family = binomial)
   summary(logitObj)
   
   ## Serialize model 
   trained_model <- as.raw(serialize(logitObj, NULL));
   ',
     @input_data_1 = @inquery,
     @params = N'@trained_model varbinary(max) OUTPUT',
     @trained_model = @trained_model OUTPUT; 
   END
   GO
   ```

   + Per assicurarsi che rimangano dati disponibili per testare il modello, viene selezionato in modo casuale il 70% dei dati della tabella relativa ai dati dei taxi da usare per il training.

   + La query SELECT usa la funzione scalare personalizzata *fnCalculateDistance* per calcolare la distanza diretta tra la posizione di salita e discesa dei passeggeri. I risultati della query vengono archiviati nella variabile di input R predefinita `InputDataset`.
  
   + Lo script R chiama la funzione R **glm** per creare il modello di regressione logistica.
  
     La variabile binaria _tipped_ viene usata come *etichetta* o come colonna dei risultati. Il modello si adatta usando le colonne delle funzionalità seguenti:  _passenger_count_, _trip_distance_, _trip_time_in_secs_ e _direct_distance_.
  
   + Il modello sottoposto a training salvato nella variabile R `logitObj` viene serializzato e restituito come parametro di output.

## <a name="train-and-deploy-the-r-model-using-the-stored-procedure"></a>Eseguire il training e la distribuzione del modello R usando la stored procedure

Poiché la stored procedure include già una definizione dei dati di input, non è necessario specificare una query di input.

1. Per eseguire il training e la distribuzione del modello R, chiamare la stored procedure e inserirla nella tabella di database _nyc_taxi_models_, in modo che sia possibile farne uso per stime future:

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC RTrainLogitModel @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('RTrainLogit_model', @model);
   ```

2. Esaminare la finestra **Messaggi** di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] per visualizzare i messaggi che verrebbero inoltrati al flusso **stdout** di R, come il seguente: 

   "STDOUT message(s) from external script: Rows Read: 1193025, Total Rows Processed: 1193025, Total Chunk Time: 0.093 seconds"

3. Dopo il completamento dell'istruzione, aprire la tabella *nyc_taxi_models*. L'elaborazione dei dati e l'adattamento del modello possono richiedere un po' di tempo.

   È possibile notare che è stata aggiunta una nuova riga nuova contenente il modello serializzato nella colonna _model_ e il nome del modello **RTrainLogit_model** nella colonna _name_.

   ```text
   model                        name
   ---------------------------- ------------------
   0x580A00000002000302020....  RTrainLogit_model
   ```

Nella parte successiva di questa esercitazione si userà il modello sottoposto a training per generare previsioni.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Creare ed eseguire il training di un modello usando una stored procedure SQL
> + Salvare il modello sottoposto a training in una tabella SQL

> [!div class="nextstepaction"]
> [Esercitazione su R: Eseguire le previsioni nelle stored procedure SQL](r-taxi-classification-deploy-model.md)
