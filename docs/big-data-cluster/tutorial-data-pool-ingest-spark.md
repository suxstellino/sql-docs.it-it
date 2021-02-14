---
title: Inserire dati con processi Spark
titleSuffix: SQL Server Big Data Clusters
description: Questa esercitazione illustra come inserire dati nel pool di dati di un cluster Big Data di SQL Server usando processi Spark in Azure Data Studio.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b580ef9f94ef4a677d67518b15a740fc2df75e2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045675"
---
# <a name="tutorial-ingest-data-into-a-sql-server-data-pool-with-spark-jobs"></a>Esercitazione: Inserire dati in un pool di dati di SQL Server con processi Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come usare processi Spark per caricare dati nel [pool di dati](concept-data-pool.md) di un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. 

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare una tabella esterna nel pool di dati.
> * Creare un processo Spark mediante il quale caricare i dati da HDFS.
> * Eseguire una query sui risultati nella tabella esterna.

> [!TIP]
> Se si preferisce, è possibile scaricare ed eseguire uno script per i comandi descritti in questa esercitazione. Per istruzioni, vedere i [pool di dati di esempio](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-pool) in GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
- [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md)

## <a name="create-an-external-table-in-the-data-pool"></a>Creare una tabella esterna nel pool di dati

Questa procedura consente di creare una tabella esterna nel pool di dati denominata **web_clickstreams_spark_results**. La tabella può essere quindi usata come posizione per l'inserimento di dati nel cluster Big Data.

1. In Azure Data Studio connettersi all'istanza master di SQL Server del cluster Big Data. Per altre informazioni, vedere [Connettersi all'istanza master di SQL Server](connect-to-big-data-cluster.md#master).

1. Fare doppio clic sulla connessione nella finestra **Server** per visualizzare il dashboard del server per l'istanza master di SQL Server. Selezionare **Nuova query**.

   ![Query dell'istanza master di SQL Server](./media/tutorial-data-pool-ingest-spark/sql-server-master-instance-query.png)

1. Creare autorizzazioni per il connettore MSSQL-Spark.
   ```sql
   USE Sales
   CREATE LOGIN sample_user  WITH PASSWORD ='password123!#' 
   CREATE USER sample_user FROM LOGIN sample_user

   -- To create external tables in data pools
   GRANT ALTER ANY EXTERNAL DATA SOURCE TO sample_user;

   -- To create external tables
   GRANT CREATE TABLE TO sample_user;
   GRANT ALTER ANY SCHEMA TO sample_user;

   -- To view database state for Sales
   GRANT VIEW DATABASE STATE ON DATABASE::Sales TO sample_user;

   ALTER ROLE [db_datareader] ADD MEMBER sample_user
   ALTER ROLE [db_datawriter] ADD MEMBER sample_user
   ```

1. Creare un'origine dati esterna nel pool di dati, se non esiste già.

   ```sql
   USE Sales
   GO
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
     CREATE EXTERNAL DATA SOURCE SqlDataPool
     WITH (LOCATION = 'sqldatapool://controller-svc/default');
   ```

1. Creare una tabella esterna denominata **web_clickstreams_spark_results** nel pool di dati.

   ```sql
   USE Sales
   GO
   IF NOT EXISTS(SELECT * FROM sys.external_tables WHERE name = 'web_clickstreams_spark_results')
      CREATE EXTERNAL TABLE [web_clickstreams_spark_results]
      ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
      WITH
      (
         DATA_SOURCE = SqlDataPool,
         DISTRIBUTION = ROUND_ROBIN
      );
   ```
   
1. Creare l'account di accesso per i pool di dati e fornire le autorizzazioni all'utente.
   ```sql 
   EXECUTE( ' Use Sales; CREATE LOGIN sample_user  WITH PASSWORD = ''password123!#'' ;') AT  DATA_SOURCE SqlDataPool;

   EXECUTE('Use Sales; CREATE USER sample_user; ALTER ROLE [db_datareader] ADD MEMBER sample_user;  ALTER ROLE [db_datawriter] ADD MEMBER sample_user;') AT DATA_SOURCE SqlDataPool;
   ```
   
La creazione di una tabella esterna del pool di dati è un'operazione di blocco. Il controllo viene restituito dopo aver completato la creazione della tabella specificata in tutti i nodi del pool di dati back-end. Se si verifica un errore durante l'operazione di creazione, viene restituito un messaggio di errore al chiamante.

## <a name="start-a-spark-streaming-job"></a>Avviare un processo di streaming Spark

Il passaggio successivo consiste nella creazione di un processo di streaming Spark che carica dati clickstream Web dal pool di archiviazione (HDFS) nella tabella esterna creata nel pool di dati. Questi dati sono stati aggiunti a /clickstream_data in [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md).

1. In Azure Data Studio connettersi all'istanza master del cluster Big Data. Per altre informazioni, vedere [Connettersi a un cluster Big Data](connect-to-big-data-cluster.md).

2. Creare un nuovo notebook e selezionare Spark | Scala come kernel.

3. Eseguire il processo di inserimento Spark

   1. Configurare i parametri del connettore Spark-SQL

    > [!NOTE]
    > Se il cluster Big Data viene distribuito con l'integrazione di Active Directory, sostituire il valore di **nome host** riportato di seguito in modo da includere il nome di dominio completo aggiunto al nome del servizio. ad esempio *nome host=master-p-svc.\<domainName>* .

      ```
      import org.apache.spark.sql.types._
      import org.apache.spark.sql.{SparkSession, SaveMode, Row, DataFrame}

      // Change per your installation
      val user= "username"
      val password= "****"
      val database =  "MyTestDatabase"
      val sourceDir = "/clickstream_data"
      val datapool_table = "web_clickstreams_spark_results"
      val datasource_name = "SqlDataPool"
      val schema = StructType(Seq(
      StructField("wcs_click_date_sk",LongType,true), StructField("wcs_click_time_sk",LongType,true), 
      StructField("wcs_sales_sk",LongType,true), StructField("wcs_item_sk",LongType,true),
      StructField("wcs_web_page_sk",LongType,true), StructField("wcs_user_sk",LongType,true)
      ))

      val hostname = "master-p-svc"
      val port = 1433
      val url = s"jdbc:sqlserver://${hostname}:${port};database=${database};user=${user};password=${password};"
      ```
   2. Definire ed eseguire il processo Spark
      * Ogni processo è costituito da due parti: readStream e writeStream. Di seguito viene creato un frame di dati usando lo schema definito in precedenza e quindi viene scritto nella tabella esterna nel pool di dati.
      ```
      import org.apache.spark.sql.{SparkSession, SaveMode, Row, DataFrame}
      
      val df = spark.readStream.format("csv").schema(schema).option("header", true).load(sourceDir)
      val query = df.writeStream.outputMode("append").foreachBatch{ (batchDF: DataFrame, batchId: Long) => 
                batchDF.write
                 .format("com.microsoft.sqlserver.jdbc.spark")
                 .mode("append")
                  .option("url", url)
                  .option("dbtable", datapool_table)
                  .option("user", user)
                  .option("password", password)
                  .option("dataPoolDataSource",datasource_name).save()
               }.start()

      query.awaitTermination(40000)
      query.stop()
      ```
## <a name="query-the-data"></a>Eseguire una query sui dati

Questa procedura dimostra che il processo di streaming Spark ha caricato i dati da HDFS nel pool di dati.

1. Prima di eseguire query sui dati inseriti, esaminare lo stato di esecuzione di Spark, inclusi l'ID dell'app Yarn, l'interfaccia utente di Spark e i log del driver. Queste informazioni verranno visualizzate nel notebook al primo avvio dell'applicazione Spark.

   ![Dettagli di esecuzione di Spark](./media/tutorial-data-pool-ingest-spark/Spark-Joblog-sparkui-yarn.png)

1. Tornare alla finestra di query dell'istanza master di SQL Server aperta all'inizio di questa esercitazione.

1. Eseguire la query seguente per esaminare i dati inseriti.

   ```sql
   USE Sales
   GO
   SELECT count(*) FROM [web_clickstreams_spark_results];
   SELECT TOP 10 * FROM [web_clickstreams_spark_results];
   ```
1. È anche possibile eseguire query sui dati in Spark. Il codice seguente, ad esempio, stampa il numero di record nella tabella:
   ```
   def df_read(dbtable: String,
                url: String,
                dataPoolDataSource: String=""): DataFrame = {
        spark.read
             .format("com.microsoft.sqlserver.jdbc.spark")
             .option("url", url)
             .option("dbtable", dbtable)
             .option("user", user)
             .option("password", password)
             .option("dataPoolDataSource", dataPoolDataSource)
             .load()
             }

   val new_df = df_read(datapool_table, url, dataPoolDataSource=datasource_name)
   println("Number of rows is " +  new_df.count)
   ```
## <a name="clean-up"></a>Eseguire la pulizia

Usare il comando seguente per rimuovere gli oggetti di database creati in questa esercitazione.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_spark_results];
```

## <a name="next-steps"></a>Passaggi successivi

Informazioni su come eseguire un notebook di esempio in Azure Data Studio:
> [!div class="nextstepaction"]
> [Eseguire un notebook di esempio](notebooks-tutorial-spark.md)
