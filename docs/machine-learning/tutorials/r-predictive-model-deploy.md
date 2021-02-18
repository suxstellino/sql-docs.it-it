---
title: 'Esercitazione: Distribuire un modello predittivo in R'
titleSuffix: SQL machine learning
description: Nella quarta parte di questa serie di esercitazioni in quattro parti si distribuirà un modello predittivo in R con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0f264f0996a3597085c8e5103eed9ebc0ee2c27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348055"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-sql-machine-learning"></a>Esercitazione: Distribuire un modello predittivo in R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di Machine Learning, sviluppato in R, in Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà in SQL Server un modello di Machine Learning, sviluppato in R, usando Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà in SQL Server un modello di Machine Learning, sviluppato in R, usando R Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di Machine Learning, sviluppato in R, in Istanza gestita di SQL di Azure con Machine Learning Services.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Creare una stored procedure che genera il modello di Machine Learning
> * Archiviare il modello in una tabella del database
> * Creare una stored procedure che esegue stime usando il modello
> * Eseguire il modello con nuovi dati

Nella [prima parte](r-predictive-model-introduction.md) si è appreso come ripristinare il database di esempio.

Nella [seconda parte](r-predictive-model-prepare-data.md) si è appreso come importare un database di esempio e quindi preparare i dati da usare per il training di un modello predittivo in R.

Nella [terza parte](r-predictive-model-train.md) si è appreso come creare ed eseguire il training di più modelli di Machine Learning in R e quindi scegliere quello più accurato.

## <a name="prerequisites"></a>Prerequisiti

Nella quarta parte di questa esercitazione si presuppone che siano stati soddisfatti i prerequisiti della [**prima parte**](r-predictive-model-introduction.md) e che siano stati completati i passaggi descritti nella [**seconda**](r-predictive-model-prepare-data.md) e nella [**terza parte**](r-predictive-model-train.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Creare una stored procedure che genera il modello

Nella terza parte di questa serie di esercitazioni, si è scelto come modello più accurato un albero delle decisioni (dtree). Adesso, usando gli script R sviluppati, creare una stored procedure (`generate_rental_model`) per il training e la generazione del modello dtree usando rpart del pacchetto R.

Eseguire i seguenti comandi in Azure Data Studio.

```sql
USE [TutorialDB]
DROP PROCEDURE IF EXISTS generate_rental_model;
GO
CREATE PROCEDURE generate_rental_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
rental_train_data$Month   <- factor(rental_train_data$Month);
rental_train_data$Day     <- factor(rental_train_data$Day);
rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
library(rpart);
model_dtree <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Archiviare il modello in una tabella del database

Creare una tabella nel database TutorialDB e quindi salvare il modello nella tabella.

1. Creare una tabella (`rental_models`) per memorizzare il modello.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_models;
    GO
    CREATE TABLE rental_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Salvare il modello nella tabella come oggetto binario con il nome "DTree".

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_model @model OUTPUT;
    
    INSERT INTO rental_models (
          model_name
        , model
        )
    VALUES (
         'DTree'
        , @model
        );
    
    SELECT *
    FROM rental_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Creare una stored procedure che esegue stime

Creare una stored procedure (`predict_rentalcount_new`) per le stime in base al modello di cui è stato eseguito il training e un set di nuovi dati.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
USE [TutorialDB]
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @model VARBINARY(MAX) = (
            SELECT model
            FROM rental_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
#Convert types to factors
rentals$Month   <- factor(rentals$Month);
rentals$Day     <- factor(rentals$Day);
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(model);

#Call prediction function
rental_predictions <- predict(rental_model, rentals);
rental_predictions <- data.frame(rental_predictions);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@model varbinary(max)'
        , @model = @model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Eseguire il modello con nuovi dati

Ora è possibile utilizzare la stored procedure `predict_rentalcount_new` per stimare il numero di noleggio dai nuovi dati.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'DTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Il risultato visualizzato sarà simile al seguente.

```results
RentalCount_Predicted
332.571428571429
```

Sono state eseguite le operazioni di creazione, training e distribuzione di un modello in un database. Il modello è stato quindi usato in una stored procedure per stimare i valori in base ai nuovi dati.


## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver usato il database TutorialDB, eliminarlo dal server.

## <a name="next-steps"></a>Passaggi successivi

Nella quarta parte di questa serie di esercitazioni si è appreso come:

* Creare una stored procedure che genera il modello di Machine Learning
* Archiviare il modello in una tabella del database
* Creare una stored procedure che esegue stime usando il modello
* Eseguire il modello con nuovi dati

Per altre informazioni sull'uso di R in Machine Learning Services, vedere:

* [Eseguire script R semplici](quickstart-r-create-script.md)
* [Strutture di dati, tipi di dati e oggetti R](quickstart-r-data-types-and-objects.md)
* [Funzioni R](quickstart-r-functions.md)
