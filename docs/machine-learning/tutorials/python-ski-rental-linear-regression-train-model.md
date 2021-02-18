---
title: 'Esercitazione su Python: Eseguire il training del modello'
titleSuffix: SQL machine learning
description: Nella terza parte di questa serie di esercitazioni in quattro parti si eseguirà il training di un modello di regressione lineare in Python per stimare il noleggio di sci con Machine Learning in SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: cb31c5486668daf64b86df50ecb493a778a9b61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351459"
---
# <a name="python-tutorial-train-a-linear-regression-model-with-sql-machine-learning"></a>Esercitazione su Python: Eseguire il training di un modello di regressione lineare con Machine Learning in SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nella terza parte di questa serie di esercitazioni in quattro parti verrà eseguito il training di un modello di regressione lineare in Python. Nella parte successiva della serie questo modello verrà distribuito in un database SQL Server con Machine Learning Services o in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nella terza parte di questa serie di esercitazioni in quattro parti verrà eseguito il training di un modello di regressione lineare in Python. Nella parte successiva della serie questo modello verrà distribuito in un database di SQL Server con Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nella terza parte di questa serie di esercitazioni in quattro parti verrà eseguito il training di un modello di regressione lineare in Python. Nella parte successiva di questa serie questo modello verrà distribuito in un database di Istanza gestita di SQL di Azure con Machine Learning Services.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Eseguire il training di un modello di regressione lineare
> * Eseguire stime tramite un modello di regressione lineare

Nella [prima parte](python-ski-rental-linear-regression.md) si è appreso come ripristinare il database di esempio.

Nella [seconda parte](python-ski-rental-linear-regression-prepare-data.md) si è appreso come caricare i dati da un database in un frame di dati Python e come preparare i dati in Python.

Nelle [quarta parte](python-ski-rental-linear-regression-deploy-model.md) si apprenderà come archiviare il modello in un database e quindi creare le stored procedure dagli script Python sviluppati nella seconda e nella terza parte. Le stored procedure verranno quindi eseguite sul server per eseguire stime basate sui nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

* Per poter eseguire la terza parte di questa esercitazione si presuppone che sia stata completata la [prima parte](python-ski-rental-linear-regression.md) e i rispettivi prerequisiti.

## <a name="train-the-model"></a>Eseguire il training del modello

Per eseguire una stima, è necessario trovare una funzione (modello) che descriva al meglio la dipendenza tra le variabili del set di dati. In questo modo viene eseguito il training del modello. Il set di dati di training sarà costituito da un subset dell'intero set di dati del frame di dati pandas **df** creato nella seconda parte di questa serie.

Verrà eseguito il training del modello **lin_model** usando un algoritmo di regressione lineare.

```python
# Store the variable we'll be predicting on.
target = "Rentalcount"

# Generate the training set.  Set random_state to be able to replicate results.
train = df.sample(frac=0.8, random_state=1)

# Select anything not in the training set and put it in the testing set.
test = df.loc[~df.index.isin(train.index)]

# Print the shapes of both sets.
print("Training set shape:", train.shape)
print("Testing set shape:", test.shape)

# Initialize the model class.
lin_model = LinearRegression()

# Fit the model to the training data.
lin_model.fit(train[columns], train[target])
```

I risultati visualizzati saranno simili ai seguenti:

```results
Training set shape: (362, 7)
Testing set shape: (91, 7)
```

## <a name="make-predictions"></a>Effettuare stime

Usare una funzione Predict per stimare il numero di noleggi usando il modello **lin_model**.

```python
# Generate our predictions for the test set.
lin_predictions = lin_model.predict(test[columns])
print("Predictions:", lin_predictions)
# Compute error between our test predictions and the actual values.
lin_mse = mean_squared_error(lin_predictions, test[target])
print("Computed error:", lin_mse)
```

I risultati visualizzati saranno simili ai seguenti:

```results
Predictions: [ 40.  38. 240.  39. 514.  48. 297.  25. 507.  24.  30.  54.  40.  26.
  30.  34.  42. 390. 336.  37.  22.  35.  55. 350. 252. 370. 499.  48.
  37. 494.  46.  25. 312. 390.  35.  35. 421.  39. 176.  21.  33. 452.
  34.  28.  37. 260.  49. 577. 312.  24.  24. 390.  34.  64.  26.  32.
  33. 358. 348.  25.  35.  48.  39.  44.  58.  24. 350. 651.  38. 468.
  26.  42. 310. 709. 155.  26. 648. 617.  26. 846. 729.  44. 432.  25.
  39.  28. 325.  46.  36.  50.  63.]
Computed error: 2.9960763804270902e-27
```

## <a name="next-steps"></a>Passaggi successivi

Nella terza parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Eseguire il training di un modello di regressione lineare
* Eseguire stime tramite un modello di regressione lineare

Per distribuire il modello di Machine Learning creato, seguire la quarta parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione su Python: Distribuire un modello di Machine Learning](python-ski-rental-linear-regression-deploy-model.md)
