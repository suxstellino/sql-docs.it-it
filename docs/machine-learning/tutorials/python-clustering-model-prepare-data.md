---
title: 'Esercitazione su Python: Preparare i dati dei cluster'
titleSuffix: SQL machine learning
description: Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati SQL per eseguire il clustering in Python con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 7d73f57f771a52de065e0fdc8236e280d5d85ab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339719"
---
# <a name="python-tutorial-prepare-data-to-categorize-customers-with-sql-machine-learning"></a>Esercitazione su Python: Preparare i dati per categorizzare i clienti con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nella seconda parte di questa serie di esercitazioni in quattro parti si ripristineranno e prepareranno i dati da un database tramite Python. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di clustering in Python con Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nella seconda parte di questa serie di esercitazioni in quattro parti si ripristineranno e prepareranno i dati da un database tramite Python. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di clustering in Python con Machine Learning Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nella seconda parte di questa serie di esercitazioni in quattro parti si ripristineranno e prepareranno i dati da un database tramite Python. Più avanti nel corso della serie questi dati verranno usati per eseguire il training e la distribuzione di un modello di clustering in Python con Machine Learning Services per Istanza gestita di SQL di Azure.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Separare i clienti in dimensioni diverse tramite Python
> * Caricare i dati dal database in un frame di dati Python

Nella [prima parte](python-clustering-model.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [terza parte](python-clustering-model-build.md) si apprenderà come creare ed eseguire il training di un modello di clustering K-Means in Python.

Nella [quarta parte](python-clustering-model-deploy.md) si apprenderà come creare una stored procedure in un database in grado di eseguire il clustering in Python in base ai nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

* Per poter eseguire la seconda parte di questa esercitazione si presuppone che siano stati soddisfatti i prerequisiti della [**prima parte**](python-clustering-model.md).

## <a name="separate-customers"></a>Separare i clienti

Per preparare il clustering dei clienti, occorre prima di tutto separare i clienti in base alle dimensioni seguenti:

* **orderRatio** = rapporto ordini di reso (numero totale di ordini parzialmente o completamente resi rispetto al numero totale di ordini)
* **itemsRatio** = rapporto articoli resi (numero totale di articoli resi rispetto al numero di articoli acquistati)
* **monetaryRatio** = rapporto importi resi (importo monetario totale di articoli resi rispetto all'importo di articoli acquistati)
* **frequency** = frequenza dei resi

Aprire un nuovo notebook in Azure Data Studio e immettere lo script seguente.

Nella stringa di connessione sostituire i dettagli della connessione in base alle esigenze.

```python
# Load packages.
import pyodbc
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from scipy.spatial import distance as sci_distance
from sklearn import cluster as sk_cluster

################################################################################################

## Connect to DB and select data

################################################################################################

# Connection string to connect to SQL Server named instance.
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=tpcxbb_1gb; UID=<username>; PWD=<password>')

input_query = '''SELECT
ss_customer_sk AS customer,
ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) AS orderRatio,
ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) AS itemsRatio,
ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) AS monetaryRatio,
COALESCE(returns_count, 0) AS frequency
FROM
(
  SELECT
    ss_customer_sk,
    -- return order ratio
    COUNT(distinct(ss_ticket_number)) AS orders_count,
    -- return ss_item_sk ratio
    COUNT(ss_item_sk) AS orders_items,
    -- return monetary amount ratio
    SUM( ss_net_paid ) AS orders_money
  FROM store_sales s
  GROUP BY ss_customer_sk
) orders
LEFT OUTER JOIN
(
  SELECT
    sr_customer_sk,
    -- return order ratio
    count(distinct(sr_ticket_number)) as returns_count,
    -- return ss_item_sk ratio
    COUNT(sr_item_sk) as returns_items,
    -- return monetary amount ratio
    SUM( sr_return_amt ) AS returns_money
FROM store_returns
GROUP BY sr_customer_sk ) returned ON ss_customer_sk=sr_customer_sk'''


# Define the columns we wish to import.
column_info = {
    "customer": {"type": "integer"},
    "orderRatio": {"type": "integer"},
    "itemsRatio": {"type": "integer"},
    "frequency": {"type": "integer"}
}
```

## <a name="load-the-data-into-a-data-frame"></a>Caricare i dati in un frame di dati

I risultati della query vengono restituiti a Python usando la funzione Pandas **read_sql**. Nell'ambito del processo verranno usate le informazioni sulle colonne definite nello script precedente.

```python
customer_data = pandas.read_sql(input_query, conn_str)
```

Visualizzare ora l'inizio del frame di dati per verificare che sia corretto.

```python
print("Data frame:", customer_data.head(n=5))
```

```results
Rows Read: 37336, Total Rows Processed: 37336, Total Chunk Time: 0.172 seconds
Data frame:     customer  orderRatio  itemsRatio  monetaryRatio  frequency
0    29727.0    0.000000    0.000000       0.000000          0
1    97643.0    0.068182    0.078176       0.037034          3
2    57247.0    0.000000    0.000000       0.000000          0
3    32549.0    0.086957    0.068657       0.031281          4
4     2040.0    0.000000    0.000000       0.000000          0
```

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare con questa esercitazione, eliminare il database tpcxbb_1gb.

## <a name="next-steps"></a>Passaggi successivi

Nella seconda parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Separare i clienti in dimensioni diverse tramite Python
* Caricare i dati dal database in un frame di dati Python

Per creare un modello di Machine Learning che usa questi dati dei clienti, seguire la terza parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione su Python: Creare un modello predittivo](python-clustering-model-build.md)
