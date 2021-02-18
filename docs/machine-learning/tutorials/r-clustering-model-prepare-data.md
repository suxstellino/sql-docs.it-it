---
title: 'Esercitazione: Preparare i dati per eseguire il clustering in R'
titleSuffix: SQL machine learning
description: Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database per eseguire il clustering in R con Machine Learning in SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 358f34e281c5c5b1a750135117886247c5e504c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348067"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-sql-machine-learning"></a>Esercitazione: Preparare i dati per eseguire il clustering in R con Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database per eseguire il clustering in R con Machine Learning Services per SQL Server oppure in cluster Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database per eseguire il clustering in R con Machine Learning Services per SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database per eseguire il clustering in R con R Services per SQL Server 2016.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Nella seconda parte di questa serie di esercitazioni in quattro parti si prepareranno i dati di un database per eseguire il clustering in R con Machine Learning Services per Istanza gestita di SQL di Azure.
::: moniker-end

In questo articolo si apprenderà come:

> [!div class="checklist"]
> * Separare i clienti in base alle diverse dimensioni usando R
> * Caricare i dati del database in un frame di dati R

Nella [prima parte](r-clustering-model-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [terza parte](r-clustering-model-build.md) si apprenderà come creare ed eseguire il training di un modello di clustering K-Means in R.

Nella [quarta parte](r-clustering-model-deploy.md) si apprenderà come creare una stored procedure in un database in grado di eseguire il clustering in R in base ai nuovi dati.

## <a name="prerequisites"></a>Prerequisiti

* Nella seconda parte di questa esercitazione si presuppone che sia stata completata la [**prima parte**](r-clustering-model-introduction.md).

## <a name="separate-customers"></a>Separare i clienti

Creare un nuovo file RScript in RStudio ed eseguire lo script seguente.
Nella query SQL si separeranno i clienti in base alle dimensioni seguenti:

* **orderRatio** = rapporto ordini di reso (numero totale di ordini parzialmente o completamente resi rispetto al numero totale di ordini)
* **itemsRatio** = rapporto articoli resi (numero totale di articoli resi rispetto al numero di articoli acquistati)
* **monetaryRatio** = rapporto importi resi (importo monetario totale di articoli resi rispetto all'importo di articoli acquistati)
* **frequency** = frequenza dei resi

Nella funzione **connStr** sostituire **ServerName** con le proprie informazioni di connessione.

```r
# Define the connection string to connect to the tpcxbb_1gb database

connStr <- "Driver=SQL Server;Server=ServerName;Database=tpcxbb_1gb;uid=Username;pwd=Password"

#Define the query to select data
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
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
    ) returned ON ss_customer_sk = sr_customer_sk";
```

## <a name="load-the-data-into-a-data-frame"></a>Caricare i dati in un frame di dati

A questo punto usare lo script seguente per restituire i risultati della query in un frame di dati R.

```r
# Query using input_query and get the results back
# to data frame customer_data

library(RODBC)

ch <- odbcDriverConnect(connStr)

customer_data <- sqlQuery(ch, input_query)

# Take a look at the data just loaded
head(customer_data, n = 5);
```

I risultati visualizzati saranno simili ai seguenti:

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare con questa esercitazione, eliminare il database tpcxbb_1gb.

## <a name="next-steps"></a>Passaggi successivi

Nella seconda parte di questa serie di esercitazioni si è appreso come:

* Separare i clienti in base alle diverse dimensioni usando R
* Caricare i dati del database in un frame di dati R

Per creare un modello di Machine Learning che usa questi dati dei clienti, seguire la terza parte di questa serie di esercitazioni:

> [!div class="nextstepaction"]
> [Creare un modello predittivo in R con Machine Learning in SQL](r-clustering-model-build.md)
