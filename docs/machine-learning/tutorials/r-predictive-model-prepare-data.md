---
title: 'Esercitazione: Preparare i dati per eseguire il training di un modello predittivo in R'
titleSuffix: SQL machine learning
description: Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati per eseguire il training di un modello predittivo in R con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e1929f0db9fb1d70fc8c370297d2d6f1c4e74513
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348080"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-sql-machine-learning"></a>Esercitazione: Preparare i dati per eseguire il training di un modello predittivo in R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database usando R. Più avanti nella serie, questi dati verranno usati per eseguire il training e la distribuzione di un modello predittivo in R con Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nella seconda parte di questa serie di esercitazioni in quattro parti, si prepareranno i dati di un database usando R. Più avanti nella serie, questi dati verranno usati per eseguire il training e la distribuzione di un modello predittivo in R con Machine Learning Services per SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database usando R. Più avanti nella serie, questi dati verranno usati per eseguire il training e la distribuzione di un modello predittivo in R con R Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nella seconda parte di questa serie di esercitazioni in quattro parti, si prepareranno i dati di un database usando R. Più avanti nella serie, questi dati verranno usati per eseguire il training e la distribuzione di un modello predittivo in R con Machine Learning Services per Istanza gestita di SQL di Azure.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Ripristinare un database di esempio in un database
> * Caricare i dati del database in un frame di dati R
> * Preparare i dati in R identificando alcune colonne come categoriche

Nella [prima parte](r-predictive-model-introduction.md) si è appreso come ripristinare il database di esempio.

Nella [terza parte](r-predictive-model-train.md) si apprenderà come eseguire il training di un modello di Machine Learning in R.

Nelle [quarta parte](r-predictive-model-deploy.md) si apprenderà come archiviare il modello in un database e quindi creare le stored procedure dagli script R sviluppati nella seconda e nella terza parte. Le stored procedure verranno quindi eseguite sul server per eseguire stime basate sui nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

Per la seconda parte di questa esercitazione è necessario aver completato la [**prima parte**](r-predictive-model-introduction.md) e i relativi prerequisiti.

## <a name="load-the-data-into-a-data-frame"></a>Caricare i dati in un frame di dati

Per usare i dati in R, si caricheranno i dati dal database in un frame di dati (`rentaldata`).

Creare un nuovo file RScript in RStudio ed eseguire lo script seguente. Sostituire **ServerName** con le proprie informazioni di connessione.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- "Driver=SQL Server;Server=ServerName;Database=TutorialDB;uid=Username;pwd=Password"


#Get the data from the table
library(RODBC)

ch <- odbcDriverConnect(connStr)

#Import the data from the table
rentaldata <- sqlFetch(ch, "dbo.rental_data")

#Take a look at the structure of the data and the top rows
head(rentaldata)
str(rentaldata)
```

I risultati visualizzati saranno simili ai seguenti:

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Preparare i dati

In questo database di esempio, la maggior parte della preparazione è già stata completata, ma sarà necessario completare un'altra preparazione qui.
Usare lo script R seguente per identificare tre colonne come *categorie* modificando i tipi di dati in *fattore*.



```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);



#Visualize the dataset after the change
str(rentaldata);
```

I risultati visualizzati saranno simili ai seguenti:

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

I dati sono ora pronti per il training.

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare con questa esercitazione, eliminare il database TutorialDB.

## <a name="next-steps"></a>Passaggi successivi

Nella seconda parte di questa serie di esercitazioni si è appreso come:

* Caricare i dati di esempio in un frame di dati R
* Preparare i dati in R identificando alcune colonne come categoriche

Per creare un modello di Machine Learning che usa i dati del database TutorialDB, seguire la terza parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Creare un modello predittivo in R con Machine Learning in SQL](r-predictive-model-train.md)
