---
title: Inserire dati in un pool di dati di SQL Server
titleSuffix: SQL Server big data clusters
description: Questa esercitazione illustra come inserire dati nel pool di dati di un cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d7e24258e61412584b7364f8dae7d07a1581727b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043542"
---
# <a name="tutorial-ingest-data-into-a-sql-server-data-pool-with-transact-sql"></a>Esercitazione: Inserire dati in un pool di dati di SQL Server con Transact-SQL

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come usare Transact-SQL per caricare dati nel [pool di dati](concept-data-pool.md) di un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. I [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] consentono infatti di inserire e distribuire dati provenienti da origini diverse tra più istanze di pool di dati.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare una tabella esterna nel pool di dati.
> * Inserire dati clickstream Web di esempio nella tabella del pool di dati.
> * Unire i dati della tabella del pool di dati con tabelle locali.

> [!TIP]
> Se si preferisce, è possibile scaricare ed eseguire uno script per i comandi descritti in questa esercitazione. Per istruzioni, vedere i [pool di dati di esempio](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-pool) in GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
- [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md)

## <a name="create-an-external-table-in-the-data-pool"></a>Creare una tabella esterna nel pool di dati

La procedura seguente consente di creare una tabella esterna nel pool di dati denominata **web_clickstream_clicks_data_pool**. La tabella può essere quindi usata come posizione per l'inserimento di dati nel cluster Big Data.

1. In Azure Data Studio connettersi all'istanza master di SQL Server del cluster Big Data. Per altre informazioni, vedere [Connettersi all'istanza master di SQL Server](connect-to-big-data-cluster.md#master).

1. Fare doppio clic sulla connessione nella finestra **Server** per visualizzare il dashboard del server per l'istanza master di SQL Server. Selezionare **Nuova query**.

   ![Query dell'istanza master di SQL Server](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. Eseguire il comando Transact-SQL seguente per modificare il contesto nel database **Sales** dell'istanza master.

   ```sql
   USE Sales
   GO
   ```

1. Creare un'origine dati esterna nel pool di dati, se non esiste già.

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
     CREATE EXTERNAL DATA SOURCE SqlDataPool
     WITH (LOCATION = 'sqldatapool://controller-svc/default');
   ```

1. Creare una tabella esterna denominata **web_clickstream_clicks_data_pool** nel pool di dati.

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_tables WHERE name = 'web_clickstream_clicks_data_pool')
      CREATE EXTERNAL TABLE [web_clickstream_clicks_data_pool]
      ("wcs_user_sk" BIGINT , "i_category_id" BIGINT , "clicks" BIGINT)
      WITH
      (
         DATA_SOURCE = SqlDataPool,
         DISTRIBUTION = ROUND_ROBIN
      );
   ```

La creazione di una tabella esterna del pool di dati è un'operazione di blocco. Il controllo viene restituito dopo aver completato la creazione della tabella specificata in tutti i nodi del pool di dati back-end. Se si verifica un errore durante l'operazione di creazione, viene restituito un messaggio di errore al chiamante.

## <a name="load-data"></a>Caricare dati

La procedura seguente consente di inserire nel pool dati clickstream Web di esempio usando la tabella esterna creata in precedenza.

1. Usare un'`INSERT INTO`istruzione per inserire i risultati della query nel pool di dati (tabella esterna **web_clickstream_clicks_data_pool**).

   ```sql
   INSERT INTO web_clickstream_clicks_data_pool
   SELECT wcs_user_sk, i_category_id, COUNT_BIG(*) as clicks
     FROM sales.dbo.web_clickstreams_hdfs
   INNER JOIN sales.dbo.item it ON (wcs_item_sk = i_item_sk
                           AND wcs_user_sk IS NOT NULL)
   GROUP BY wcs_user_sk, i_category_id
   HAVING COUNT_BIG(*) > 100;
   ```

1. Esaminare i dati inseriti con due query SELECT.

   ```sql
   SELECT count(*) FROM [dbo].[web_clickstream_clicks_data_pool]
   SELECT TOP 10 * FROM [dbo].[web_clickstream_clicks_data_pool]  
   ```

## <a name="query-the-data"></a>Eseguire una query sui dati

Unire in join i risultati archiviati dalla query nel pool di dati con i dati locali della tabella **Sales**.

```sql
SELECT TOP (100)
   w.wcs_user_sk,
   SUM( CASE WHEN i.i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
   SUM( CASE WHEN w.i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
   SUM( CASE WHEN w.i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
   SUM( CASE WHEN w.i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
   SUM( CASE WHEN w.i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
   SUM( CASE WHEN w.i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
   SUM( CASE WHEN w.i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
   SUM( CASE WHEN w.i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
   SUM( CASE WHEN w.i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
   SUM( CASE WHEN w.i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
FROM [dbo].[web_clickstream_clicks_data_pool] as w
INNER JOIN (SELECT DISTINCT i_category_id, i_category FROM item) as i
   ON i.i_category_id = w.i_category_id
GROUP BY w.wcs_user_sk;
```

## <a name="clean-up"></a>Eseguire la pulizia

Usare il comando seguente per rimuovere gli oggetti di database creati in questa esercitazione.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstream_clicks_data_pool];
```

## <a name="next-steps"></a>Passaggi successivi

Informazioni su come inserire dati nel pool di dati con processi Spark:
> [!div class="nextstepaction"]
> [Inserire dati con processi Spark](tutorial-data-pool-ingest-spark.md)
