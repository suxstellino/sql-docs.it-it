---
title: 'Esercitazione: Eseguire il training di modelli predittivi in R e confrontarli'
titleSuffix: SQL machine learning
description: Nella terza parte di questa serie di esercitazioni in quattro parti si svilupperanno due modelli predittivi in R con Machine Learning in SQL e quindi si selezionerà il modello più accurato.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0ae6a293ab1e7e888a83ceb3216e8ef74aa9f95a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348075"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-sql-machine-learning"></a>Esercitazione: Creare un modello predittivo in R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nella terza parte di questa serie di esercitazioni in quattro parti si eseguirà il training di un modello predittivo in R. Nella parte successiva di questa serie questo modello verrà distribuito in un database di SQL Server con Machine Learning Services oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nella terza parte di questa serie di esercitazioni in quattro parti si eseguirà il training di un modello predittivo in R. Nella parte successiva di questa serie questo modello verrà distribuito in un database di SQL Server con Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Nella terza parte di questa serie di esercitazioni in quattro parti si eseguirà il training di un modello predittivo in R. Nella parte successiva di questa serie questo modello verrà distribuito in un database con R Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nella terza parte di questa serie di esercitazioni in quattro parti si eseguirà il training di un modello predittivo in R. Nella parte successiva di questa serie il modello verrà distribuito in un database di Istanza gestita di SQL di Azure con Machine Learning Services.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Eseguire il training di due modelli di Machine Learning
> * Eseguire stime da entrambi i modelli
> * Confrontare i risultati per scegliere il modello più accurato

Nella [prima parte](r-predictive-model-introduction.md) si è appreso come ripristinare il database di esempio.

Nella [seconda parte](r-predictive-model-prepare-data.md) si è appreso come caricare i dati da un database in un frame di dati Python e come preparare i dati in R.

Nelle [quarta parte](r-predictive-model-deploy.md) si apprenderà come archiviare il modello in un database e quindi creare le stored procedure dagli script Python sviluppati nella seconda e nella terza parte. Le stored procedure verranno quindi eseguite sul server per eseguire stime basate sui nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

Nella terza parte di questa serie di esercitazioni si presuppone che siano stati soddisfatti i prerequisiti della [**prima parte**](r-predictive-model-introduction.md) e che siano stati completati i passaggi descritti nella [**seconda parte**](r-predictive-model-prepare-data.md).

## <a name="train-two-models"></a>Eseguire il training di due modelli

Per trovare il modello migliore per i dati di noleggio di sci, creare due modelli diversi (regressione lineare e albero delle decisioni) e vedere quale esegue le stime nel modo più accurato. Si userà il frame di dati `rentaldata` creato nella prima parte di questa serie.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];


#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use lm to create a linear regression model, trained with the training data set
model_lm <- lm(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rpart to create a decision tree model, trained with the training data set
library(rpart);
model_rpart  <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Eseguire stime da entrambi i modelli

Usare una funzione di stima per stimare i conteggi dei noleggi con ogni modello con training.

```r
#Use both models to make predictions using the test data set.
predict_lm <- predict(model_lm, test_data)
predict_lm <- data.frame(RentalCount_Pred = predict_lm, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

predict_rpart  <- predict(model_rpart,  test_data)
predict_rpart <- data.frame(RentalCount_Pred = predict_rpart, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_lm);
head(predict_rpart);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Confrontare i risultati

A questo punto si desidera vedere quale dei modelli offre le stime migliori. Un modo semplice e rapido per eseguire questa operazione è usare una funzione di tracciamento di base per visualizzare la differenza tra i valori effettivi nei dati di training e i valori stimati.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(1, 1));
plot(predict_lm$RentalCount_Pred - predict_lm$RentalCount, main = "Difference between actual and predicted. lm")
plot(predict_rpart$RentalCount_Pred  - predict_rpart$RentalCount,  main = "Difference between actual and predicted. rpart")
```

![Confronto tra i due modelli](./media/compare-models.png)

Sembra che il modello di albero delle decisioni sia il più accurato dei due modelli.

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare con questa esercitazione, eliminare il database TutorialDB.

## <a name="next-steps"></a>Passaggi successivi

Nella terza parte di questa serie di esercitazioni si è appreso come:

* Eseguire il training di due modelli di Machine Learning
* Eseguire stime da entrambi i modelli
* Confrontare i risultati per scegliere il modello più accurato

Per distribuire il modello di Machine Learning creato, seguire la quarta parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Distribuire un modello predittivo in R con Machine Learning in SQL](r-predictive-model-deploy.md)
