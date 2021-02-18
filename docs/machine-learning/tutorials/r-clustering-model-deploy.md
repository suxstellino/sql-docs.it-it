---
title: 'Esercitazione: Distribuire un modello di clustering in R'
titleSuffix: SQL machine learning
description: Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering in R con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1dc019023546a4d1912480bfe452b3067f36bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339623"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-sql-machine-learning"></a>Esercitazione: Distribuire un modello di clustering in R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in R, in un database usando Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in R, in un database usando Machine Learning Services per SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in R, in un database usando R Services per SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nell'ultima parte di questa serie di esercitazioni in quattro parti si distribuirà un modello di clustering, sviluppato in R, in un database usando Machine Learning Services per Istanza gestita di SQL di Azure.
::: moniker-end

Per eseguire regolarmente il clustering, man mano che si registrano nuovi clienti, è necessario essere in grado di chiamare lo script R da qualsiasi app. A questo scopo è possibile distribuire lo script R in un database inserendolo all'interno di una stored procedure SQL. Poiché il modello viene eseguito nel database, può essere facilmente sottoposto a training in base ai dati archiviati nel database.

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Creare una stored procedure che genera il modello
> * Eseguire il clustering
> * Usare le informazioni sul clustering

Nella [prima parte](r-clustering-model-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](r-clustering-model-prepare-data.md) si è appreso come preparare i dati di un database per il clustering.

Nella [terza parte](r-clustering-model-build.md) si è appreso come creare ed eseguire il training di un modello di clustering K-Means in R.

## <a name="prerequisites"></a>Prerequisiti

* Nella quarta parte di questa serie di esercitazioni si presuppone che siano stati soddisfatti i prerequisiti della [**prima parte**](r-clustering-model-introduction.md) e che siano stati completati i passaggi descritti nella [**seconda**](r-clustering-model-build.md) e nella [**terza parte**](r-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Creare una stored procedure che genera il modello

Eseguire lo script T-SQL seguente per creare la stored procedure. La stored procedure ricrea i passaggi sviluppati nella seconda e nella terza parte di questa serie di esercitazioni:

* Classificare i clienti in base alla cronologia degli acquisti e dei resi
* Generare quattro cluster di clienti usando un algoritmo K-Means

La stored procedure archivia i mapping dei cluster di clienti risultanti nella tabella di database **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
            WHEN (returns_count IS NULL)
                THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string

connStr <- paste("Driver=SQL Server; Server=", instance_name,
                 "; Database=", database_name,
                 "; uid=Username;pwd=Password; ",
                 sep="" )

# Input customer data that needs to be classified.
# This is the result we get from the query.
library(RODBC)

ch <- odbcDriverConnect(connStr);

customer_data <- sqlQuery(ch, input_query)

sqlDrop(ch, "customer_return_clusters")

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering output for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
            itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "customer_return_clusters")

## clean up
odbcClose(ch)
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering"></a>Eseguire il clustering

A questo punto, dopo aver creato la stored procedure, eseguire lo script seguente per il clustering.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Verificare che funzioni e che restituisca realmente l'elenco di clienti e i relativi mapping ai cluster.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Usare le informazioni sul clustering

Poiché la stored procedure di clustering è stata archiviata nel database, può eseguire il clustering in modo efficiente sui dati dei clienti archiviati nello stesso database. È possibile eseguire la stored procedure ogni volta che i dati dei clienti vengono aggiornati e usare le informazioni sul clustering aggiornate.

Si supponga di voler inviare un messaggio di posta elettronica promozionale ai clienti del cluster 0, il gruppo che era inattivo (la descrizione dei quattro cluster è disponibile nella [terza parte](r-clustering-model-build.md#analyze-the-results) di questa esercitazione). Il codice seguente seleziona gli indirizzi di posta elettronica dei clienti del cluster 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

È possibile cambiare il valore di **c.cluster** per restituire gli indirizzi di posta elettronica dei clienti di altri cluster.

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver completato questa esercitazione, è possibile eliminare il database tpcxbb_1gb.

## <a name="next-steps"></a>Passaggi successivi

Nella quarta parte di questa serie di esercitazioni si è appreso come:

* Creare una stored procedure che genera il modello
* Eseguire il clustering con Machine Learning in SQL
* Usare le informazioni sul clustering

Per altre informazioni sull'uso di R in Machine Learning Services, vedere:

* [Eseguire script R semplici](quickstart-r-create-script.md)
* [Strutture di dati, tipi di dati e oggetti R](quickstart-r-data-types-and-objects.md)
* [Funzioni R](quickstart-r-functions.md)
