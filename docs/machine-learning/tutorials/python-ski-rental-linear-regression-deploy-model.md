---
title: 'Esercitazione su Python: Distribuire il modello'
titleSuffix: SQL machine learning
description: Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello Python per la stima dei noleggi di sci in un database con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4df492591109bef94802e1bf09a162eda09563c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351499"
---
# <a name="python-tutorial-deploy-a-linear-regression-model-with-sql-machine-learning"></a>Esercitazione su Python: Distribuire un modello di regressione lineare con Machine Learning in SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di regressione lineare sviluppato in Python in un database SQL Server usando Machine Learning Services o cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di regressione lineare sviluppato in Python in un database SQL Server usando Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di regressione lineare sviluppato in Python in un database di Istanza gestita di SQL di Azure usando Machine Learning Services.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Creare una stored procedure che genera il modello di Machine Learning
> * Archiviare il modello in una tabella del database
> * Creare una stored procedure che esegue stime usando il modello
> * Eseguire il modello con nuovi dati

Nella [prima parte](python-ski-rental-linear-regression.md) si è appreso come ripristinare il database di esempio.

Nella [seconda parte](python-ski-rental-linear-regression-prepare-data.md) si è appreso come caricare i dati da un database in un frame di dati Python e come preparare i dati in Python.

Nella [terza parte](python-ski-rental-linear-regression-train-model.md) si è appreso come eseguire il training di un modello di Machine Learning di regressione lineare in Python.

## <a name="prerequisites"></a>Prerequisiti

* Per poter eseguire la quarta parte di questa esercitazione si presuppone che sia stata completata la [prima parte](python-ski-rental-linear-regression.md) e i rispettivi prerequisiti.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Creare una stored procedure che genera il modello

Usando gli script Python sviluppati, creare ora una stored procedure **generate_rental_py_model** che esegue il training e genera il modello di regressione lineare usando la funzione LinearRegression da scikit-learn.

Eseguire l'istruzione T-SQL seguente in Azure Data Studio per creare la stored procedure che eseguirà il training del modello.

```sql
-- Stored procedure that trains and generates a Python model using the rental_data and a linear regression algorithm
DROP PROCEDURE IF EXISTS generate_rental_py_model;
go
CREATE PROCEDURE generate_rental_py_model (@trained_model varbinary(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
from sklearn.linear_model import LinearRegression
import pickle

df = rental_train_data

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Store the variable well be predicting on.
target = "RentalCount"

# Initialize the model class.
lin_model = LinearRegression()

# Fit the model to the training data.
lin_model.fit(df[columns], df[target])

# Before saving the model to the DB table, convert it to a binary object
trained_model = pickle.dumps(lin_model)'

, @input_data_1 = N'select "RentalCount", "Year", "Month", "Day", "WeekDay", "Snow", "Holiday" from dbo.rental_data where Year < 2015'
, @input_data_1_name = N'rental_train_data'
, @params = N'@trained_model varbinary(max) OUTPUT'
, @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Archiviare il modello in una tabella del database

Creare una tabella nel database TutorialDB e quindi salvare il modello nella tabella.

1. Eseguire l'istruzione T-SQL seguente in Azure Data Studio per creare una tabella denominata **dbo.rental_py_models** in cui verrà archiviato il modello.

   ```sql
   USE TutorialDB;
   DROP TABLE IF EXISTS dbo.rental_py_models;
   GO
   CREATE TABLE dbo.rental_py_models (
       model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY,
       model VARBINARY(MAX) NOT NULL
   );
   GO
   ```

1. Salvare il modello nella tabella come oggetto binario con il nome **linear_model**.

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXECUTE generate_rental_py_model @model OUTPUT;
   
   INSERT INTO rental_py_models (model_name, model) VALUES('linear_model', @model);
   ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Creare una stored procedure che esegue stime

1. Creare una stored procedure **py_predict_rentalcount** che esegue stime usando il modello sottoposto a training e un set di nuovi dati. Eseguire l'istruzione T-SQL seguente in Azure Data Studio.

   ```sql
   DROP PROCEDURE IF EXISTS py_predict_rentalcount;
   GO
   CREATE PROCEDURE py_predict_rentalcount (@model varchar(100))
   AS
   BEGIN
       DECLARE @py_model varbinary(max) = (select model from rental_py_models where model_name = @model);
   
       EXECUTE sp_execute_external_script
                   @language = N'Python',
                   @script = N'
   
   # Import the scikit-learn function to compute error.
   from sklearn.metrics import mean_squared_error
   import pickle
   import pandas
   
   rental_model = pickle.loads(py_model)
   
   df = rental_score_data
   
   # Get all the columns from the dataframe.
   columns = df.columns.tolist()
   
   # Variable you will be predicting on.
   target = "RentalCount"
   
   # Generate the predictions for the test set.
   lin_predictions = rental_model.predict(df[columns])
   print(lin_predictions)
   
   # Compute error between the test predictions and the actual values.
   lin_mse = mean_squared_error(lin_predictions, df[target])
   #print(lin_mse)
   
   predictions_df = pandas.DataFrame(lin_predictions)
   
   OutputDataSet = pandas.concat([predictions_df, df["RentalCount"], df["Month"], df["Day"], df["WeekDay"], df["Snow"], df["Holiday"], df["Year"]], axis=1)
   '
   , @input_data_1 = N'Select "RentalCount", "Year" ,"Month", "Day", "WeekDay", "Snow", "Holiday"  from rental_data where Year = 2015'
   , @input_data_1_name = N'rental_score_data'
   , @params = N'@py_model varbinary(max)'
   , @py_model = @py_model
   with result sets (("RentalCount_Predicted" float, "RentalCount" float, "Month" float,"Day" float,"WeekDay" float,"Snow" float,"Holiday" float, "Year" float));
   
   END;
   GO
    ```

1. Creare una tabella in cui archiviare le stime.

   ```sql
   DROP TABLE IF EXISTS [dbo].[py_rental_predictions];
   GO
   
   CREATE TABLE [dbo].[py_rental_predictions](
    [RentalCount_Predicted] [int] NULL,
    [RentalCount_Actual] [int] NULL,
    [Month] [int] NULL,
    [Day] [int] NULL,
    [WeekDay] [int] NULL,
    [Snow] [int] NULL,
    [Holiday] [int] NULL,
    [Year] [int] NULL
   ) ON [PRIMARY]
   GO
   ```

1. Eseguire la stored procedure per stimare il numero di noleggi

   ```sql
   --Insert the results of the predictions for test set into a table
   INSERT INTO py_rental_predictions
   EXEC py_predict_rentalcount 'linear_model';
   
   -- Select contents of the table
   SELECT * FROM py_rental_predictions;
   ```

   I risultati visualizzati saranno simili ai seguenti:

   :::image type="content" source="media/python-tutorial-prediction-results.png" alt-text="Risultati della stima dalla stored procedure":::

È stato creato un modello e ne è correttamente eseguito il training e la distribuzione. Il modello è stato quindi usato in una stored procedure per stimare i valori in base ai nuovi dati.

## <a name="next-steps"></a>Passaggi successivi

Nella quarta parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Creare una stored procedure che genera il modello di Machine Learning
* Archiviare il modello in una tabella del database
* Creare una stored procedure che esegue stime usando il modello
* Eseguire il modello con nuovi dati

Per altre informazioni sull'uso di Python con Machine Learning in SQL, vedere:

+ [Esercitazioni di Python](python-tutorials.md)
