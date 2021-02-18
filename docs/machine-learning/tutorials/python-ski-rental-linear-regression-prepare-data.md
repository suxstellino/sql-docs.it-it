---
title: 'Esercitazione su Python: Preparazione dei dati'
titleSuffix: SQL machine learning
description: Nella seconda parte di questa serie di esercitazioni in quattro parti si userà Python per preparare i dati per prevedere i noleggi di sci con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1cce65d71e686876370b6a978d660d0f75a1d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351470"
---
# <a name="python-tutorial-prepare-data-to-train-a-linear-regression-model-with-sql-machine-learning"></a>Esercitazione su Python: Preparare i dati per eseguire il training di un modello di regressione lineare con Machine Learning in SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati da un database tramite Python. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di regressione lineare in Python con Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati da un database tramite Python. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di regressione lineare in Python con Machine Learning Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati da un database tramite Python. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di regressione lineare in Python con Machine Learning Services per Istanza gestita di SQL di Azure.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Caricare i dati dal database in un frame di dati **pandas**
> * Preparare i dati in Python rimuovendo alcune colonne

Nella [prima parte](python-ski-rental-linear-regression.md) si è appreso come ripristinare il database di esempio.

Nella [terza parte](python-ski-rental-linear-regression-train-model.md) si apprenderà come eseguire il training di un modello di Machine Learning di regressione lineare in Python.

Nelle [quarta parte](python-ski-rental-linear-regression-deploy-model.md) si apprenderà come archiviare il modello in un database e quindi creare le stored procedure dagli script Python sviluppati nella seconda e nella terza parte. Le stored procedure verranno quindi eseguite sul server per eseguire stime basate sui nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

* Per poter eseguire la seconda parte di questa esercitazione si presuppone che sia stata completata la [prima parte](python-ski-rental-linear-regression.md) e i rispettivi prerequisiti.

## <a name="explore-and-prepare-the-data"></a>Esplorare e preparare i dati

Per usare i dati in Python, si caricheranno i dati dal database in un frame di dati pandas.

Creare un nuovo notebook Python in Azure Data Studio ed eseguire lo script seguente. 

Lo script Python seguente consente di importare il set di dati dalla tabella **dbo.rental_data** del database in un frame di dati pandas **df**.

Nella stringa di connessione sostituire i dettagli della connessione in base alle esigenze.

```python
import pyodbc
import pandas
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# Connection string to your SQL Server instance
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=TutorialDB;UID=<username>;PWD=<password>)

query_str = 'SELECT Year, Month, Day, Rentalcount, Weekday, Holiday, Snow FROM dbo.rental_data'

df = pandas.read_sql(sql=query_str, con=conn_str)

print("Data frame:", df)

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Filter the columns to remove ones we don't want to use in the training
columns = [c for c in columns if c not in ["Year"]]
```

I risultati visualizzati saranno simili ai seguenti:

```results
Data frame:      Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
0    2014      1   20          445        2        1     0
1    2014      2   13           40        5        0     0
2    2013      3   10          456        1        0     0
3    2014      3   31           38        2        0     0
4    2014      4   24           23        5        0     0
..    ...    ...  ...          ...      ...      ...   ...
448  2013      2   19           57        3        0     1
449  2015      3   18           26        4        0     0
450  2015      3   24           29        3        0     1
451  2014      3   26           50        4        0     1
452  2015     12    6          377        1        0     1

[453 rows x 7 columns]
```

## <a name="next-steps"></a>Passaggi successivi

Nella seconda parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Caricare i dati dal database in un frame di dati **pandas**
* Preparare i dati in Python rimuovendo alcune colonne

Per eseguire il training di un modello di Machine Learning che usa i dati del database TutorialDB, seguire la terza parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione su Python: Eseguire il training di un modello di regressione lineare](python-ski-rental-linear-regression-train-model.md)