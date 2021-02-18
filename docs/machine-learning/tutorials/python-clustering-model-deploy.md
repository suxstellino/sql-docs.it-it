---
title: 'Esercitazione su Python: Distribuire un modello di clustering'
titleSuffix: SQL machine learning
description: Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering in Python con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 32b009f8155000dfc44f714cfa6f67fc8333813e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349050"
---
# <a name="python-tutorial-deploy-a-model-to-categorize-customers-with-sql-machine-learning"></a>Esercitazione su Python: Distribuire un modello per categorizzare i clienti con Machine Learning in SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in Python, in un database usando Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in Python, in un database usando Machine Learning Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in Python, in un database usando Machine Learning Services per Istanza gestita di SQL di Azure.
::: moniker-end

Per eseguire regolarmente il clustering, man mano che si registrano nuovi clienti, è necessario essere in grado di chiamare lo script Python da qualsiasi app. A questo scopo è possibile distribuire lo script Python in un database inserendolo all'interno di una stored procedure SQL. Poiché il modello viene eseguito nel database, può essere facilmente sottoposto a training in base ai dati archiviati nel database.

In questa sezione il codice Python appena scritto verrà spostato sul server e verrà distribuito il clustering.

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Creare una stored procedure che genera il modello
> * Eseguire il clustering sul server
> * Usare le informazioni sul clustering

Nella [prima parte](python-clustering-model.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](python-clustering-model-prepare-data.md) si è appreso come preparare i dati di un database per il clustering.

Nella [parte tre](python-clustering-model-build.md) si è appreso come creare ed eseguire il training di un modello di clustering K-Means in Python.

## <a name="prerequisites"></a>Prerequisiti

* Nella quarta parte di questa esercitazione si presuppone che siano stati soddisfatti i prerequisiti della [**prima parte**](python-clustering-model.md) e che siano stati completati i passaggi descritti nella [**seconda parte**](python-clustering-model-prepare-data.md) e nella [**terza parte**](python-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Creare una stored procedure che genera il modello

Eseguire lo script T-SQL seguente per creare la stored procedure. La stored procedure ricrea i passaggi sviluppati nella prima e nella seconda parte di questa serie di esercitazioni:

* Classificare i clienti in base alla cronologia degli acquisti e dei resi
* Generare quattro cluster di clienti usando un algoritmo K-Means

```sql
USE [tpcxbb_1gb]
GO

CREATE procedure [dbo].[py_generate_customer_return_clusters]
AS

BEGIN
    DECLARE

-- Input query to generate the purchase history & return metrics
     @input_query NVARCHAR(MAX) = N'
SELECT
  ss_customer_sk AS customer,
  CAST( (ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) ) AS FLOAT) AS orderRatio,
  CAST( (ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) ) AS FLOAT) AS itemsRatio,
  CAST( (ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) ) AS FLOAT) AS monetaryRatio,
  CAST( (COALESCE(returns_count, 0)) AS FLOAT) AS frequency
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
    GROUP BY sr_customer_sk
  ) returned ON ss_customer_sk=sr_customer_sk
 '

EXEC sp_execute_external_script
      @language = N'Python'
    , @script = N'

import pandas as pd
from sklearn.cluster import KMeans

#get data from input query
customer_data = my_input_data

#We concluded in step 2 in the tutorial that 4 would be a good number of clusters
n_clusters = 4

#Perform clustering
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orderRatio","itemsRatio","monetaryRatio","frequency"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
    , @input_data_1 = @input_query
    , @input_data_1_name = N'my_input_data'
             with result sets (("Customer" int, "orderRatio" float,"itemsRatio" float,"monetaryRatio" float,"frequency" float,"cluster" float));
END;
GO
```

## <a name="perform-clustering"></a>Eseguire il clustering

Ora che è stata creata la stored procedure, eseguire lo script seguente per eseguire il clustering usando la stored procedure.

```sql
--Create a table to store the predictions in

DROP TABLE IF EXISTS [dbo].[py_customer_clusters];
GO

CREATE TABLE [dbo].[py_customer_clusters] (
    [Customer] [bigint] NULL
  , [OrderRatio] [float] NULL
  , [itemsRatio] [float] NULL
  , [monetaryRatio] [float] NULL
  , [frequency] [float] NULL
  , [cluster] [int] NULL
  ,
    ) ON [PRIMARY]
GO

--Execute the clustering and insert results into table
INSERT INTO py_customer_clusters
EXEC [dbo].[py_generate_customer_return_clusters];

-- Select contents of the table to verify it works
SELECT * FROM py_customer_clusters;
```

## <a name="use-the-clustering-information"></a>Usare le informazioni sul clustering

Poiché la stored procedure di clustering è stata archiviata nel database, può eseguire il clustering in modo efficiente sui dati dei clienti archiviati nello stesso database. È possibile eseguire la stored procedure ogni volta che i dati dei clienti vengono aggiornati e usare le informazioni sul clustering aggiornate.

Si supponga di voler inviare un messaggio di posta elettronica promozionale ai clienti del cluster 0, il gruppo che era inattivo (la descrizione dei quattro cluster è disponibile nella [terza parte](python-clustering-model-build.md#analyze-the-results) di questa esercitazione). Il codice seguente seleziona gli indirizzi di posta elettronica dei clienti del cluster 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[py_customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

È possibile cambiare il valore di **c.cluster** per restituire gli indirizzi di posta elettronica dei clienti di altri cluster.

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver completato questa esercitazione, è possibile eliminare il database tpcxbb_1gb.

## <a name="next-steps"></a>Passaggi successivi

Nella quarta parte di questa serie di esercitazioni sono stati completati i passaggi seguenti:

* Creare una stored procedure che genera il modello
* Eseguire il clustering sul server
* Usare le informazioni sul clustering

Per altre informazioni sull'uso di Python con Machine Learning in SQL, vedere:

* [Avvio rapido: Creare ed eseguire script Python semplici](quickstart-python-create-script.md)
* [Altre esercitazioni di Python per Machine Learning in SQL](python-tutorials.md)
* [Installare pacchetti Python con sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md)