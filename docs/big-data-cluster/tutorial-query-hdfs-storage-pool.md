---
title: 'Eseguire query su dati HDFS: pool di archiviazione'
titleSuffix: SQL Server Big Data Clusters
description: Questa esercitazione illustra come eseguire una query su dati HDFS in un cluster Big Data di SQL Server 2019. Si creerà una tabella esterna con i dati del pool di archiviazione e si eseguirà quindi una query.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: aa376549bf3d6430ab5967e193069d35650be76b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043541"
---
# <a name="tutorial-query-hdfs-in-a-sql-server-big-data-cluster"></a>Esercitazione: Eseguire query su dati HDFS in un cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come eseguire una query su dati HDFS in un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare una tabella esterna che punta ai dati HDFS in un cluster Big Data.
> * Unire questi dati con dati di valore elevato nell'istanza master.

> [!TIP]
> Se si preferisce, è possibile scaricare ed eseguire uno script per i comandi descritti in questa esercitazione. Per istruzioni, vedere [Esempi di virtualizzazione di dati](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) in GitHub.

Questo video di 7 minuti illustra come eseguire query su dati HDFS in un cluster Big Data:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Query-HDFS-data-inside-SQL-Server-big-data-cluster/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
- [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md)

## <a name="create-an-external-table-to-hdfs"></a>Creare una tabella esterna basata su dati HDFS

Il pool di archiviazione contiene dati clickstream Web in un file CSV archiviato in HDFS. Usare questa procedura per definire una tabella esterna in grado di accedere ai dati presenti nel file.

1. In Azure Data Studio connettersi all'istanza master di SQL Server del cluster Big Data. Per altre informazioni, vedere [Connettersi all'istanza master di SQL Server](connect-to-big-data-cluster.md#master).

1. Fare doppio clic sulla connessione nella finestra **Server** per visualizzare il dashboard del server per l'istanza master di SQL Server. Selezionare **Nuova query**.

   ![Query dell'istanza master di SQL Server](./media/tutorial-query-hdfs-storage-pool/sql-server-master-instance-query.png)

1. Eseguire il comando Transact-SQL seguente per modificare il contesto nel database **Sales** dell'istanza master.

   ```sql
   USE Sales
   GO
   ```

1. Definire il formato del file CSV da leggere dal sistema HDFS. Premere F5 per eseguire l'istruzione.

   ```sql
   CREATE EXTERNAL FILE FORMAT csv_file
   WITH (
       FORMAT_TYPE = DELIMITEDTEXT,
       FORMAT_OPTIONS(
           FIELD_TERMINATOR = ',',
           STRING_DELIMITER = '"',
           FIRST_ROW = 2,
           USE_TYPE_DEFAULT = TRUE)
   );
   ```

1. Creare un'origine dati esterna nel pool di archiviazione, se non esiste già.

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   BEGIN
     CREATE EXTERNAL DATA SOURCE SqlStoragePool
     WITH (LOCATION = 'sqlhdfs://controller-svc/default');
   END
   ```

1. Creare una tabella esterna in grado di leggere `/clickstream_data` dal pool di archiviazione. **SqlStoragePool** è accessibile dall'istanza master di un cluster Big Data.

   ```sql
   CREATE EXTERNAL TABLE [web_clickstreams_hdfs]
   ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
   WITH
   (
       DATA_SOURCE = SqlStoragePool,
       LOCATION = '/clickstream_data',
       FILE_FORMAT = csv_file
   );
   GO
   ```

## <a name="query-the-data"></a>Eseguire una query sui dati

Eseguire la query seguente per creare un join tra i dati della tabella esterna `web_clickstream_hdfs` e i dati relazionali nel database `Sales` locale.

```sql
SELECT  
    wcs_user_sk,
    SUM( CASE WHEN i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
    SUM( CASE WHEN i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
    SUM( CASE WHEN i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
    SUM( CASE WHEN i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
    SUM( CASE WHEN i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
    SUM( CASE WHEN i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
    SUM( CASE WHEN i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
    SUM( CASE WHEN i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
    SUM( CASE WHEN i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
    SUM( CASE WHEN i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
  FROM [dbo].[web_clickstreams_hdfs]
  INNER JOIN item it ON (wcs_item_sk = i_item_sk
                        AND wcs_user_sk IS NOT NULL)
GROUP BY  wcs_user_sk;
GO
```

## <a name="clean-up"></a>Eseguire la pulizia

Usare il comando seguente per rimuovere la tabella esterna usata in questa esercitazione.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_hdfs];
GO
```

## <a name="next-steps"></a>Passaggi successivi

Passare all'articolo successivo per informazioni su come eseguire una query su dati Oracle da un cluster Big Data.
> [!div class="nextstepaction"]
> [Eseguire query su dati esterni in Oracle](tutorial-query-oracle.md)
