---
title: 'Inviare processi Spark: riga di comando'
titleSuffix: SQL Server big data clusters
description: Inviare processi Spark in SQL Server cluster Big Data usando gli strumenti da riga di comando.
author: dacoelho
ms.author: dacoelho
ms.reviewer: MikeRayMSFT
ms.date: 04/01/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3983f6354c84d11809b37d784d64c2f8c5b28f10
ms.sourcegitcommit: 14b97028da137f872a0a35cfe9d5a639a2d116a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107222584"
---
# <a name="submit-spark-jobs-using-command-line-tools"></a>Inviare processi Spark usando gli strumenti da riga di comando

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo fornisce indicazioni su come usare gli strumenti da riga di comando per eseguire processi Spark in SQL Server cluster di Big Data.

## <a name="prerequisites"></a>Prerequisiti

* [SQL Server 2019 strumenti Big Data](deploy-big-data-tools.md) configurati e registrati nel cluster:
  * **azdata** 
  * applicazione **curl** per eseguire chiamate API REST a Livio

## <a name="spark-jobs-using-__azdata__-or-livy"></a>Processi Spark con __azdata__ o Livio

Questo articolo fornisce esempi di utilizzo di modelli di riga di comando per inviare applicazioni Spark a SQL Server cluster di Big Data.

I [ `azdata bdc spark` comandi](../azdata/reference/reference-azdata-bdc-spark.md) dell'interfaccia della riga di comando di Azure presentano tutte le funzionalità di SQL Server cluster di Big Data Spark nella riga di comando. Mentre questa guida è incentrata sull'invio di processi, `azdata bdc spark` supporta anche le modalità interattive per Python, scala, SQL e R tramite il `azdata bdc spark session` comando.

Se si desidera l'integrazione diretta con un'API REST, usare le chiamate standard di Livio per inviare i processi. Lo `curl` strumento da riga di comando verrà usato negli esempi di Livio per eseguire la chiamata all'API REST. Per un esempio dettagliato su come interagire con l'endpoint di Spark Livio usando il codice Python, vedere l'argomento relativo all' [uso di Spark from Livio end point](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/restful-api-access/accessing_spark_via_livy.ipynb) in GitHub.

## <a name="simple-etl-using-sql-server-bdc-spark"></a>ETL semplice con SQL Server la Spark BDC

Questa applicazione esemplifica un modello di Data Engineering comune, caricando i dati tabulari da un percorso della zona di destinazione HDFS e quindi scrivendo usando un formato di tabella in un percorso di zona HDFS elaborato. Il set di dati usato in questa applicazione di esempio può essere scaricato [qui](https://ailab.criteo.com/download-criteo-1tb-click-logs-dataset/). È possibile creare applicazioni PySpark usando PySpark, Spark scala o Spark SQL e gli esempi seguenti forniscono esercizi di esempio in ogni soluzione. Usare le schede seguenti per selezionare la piattaforma scelta e successivamente per eseguire l'applicazione con `azdata` o `curl` .

### <a name="pyspark"></a>[PySpark](#tab/pyspark)

In questo esempio si userà l'applicazione PySpark seguente salvata come file Python denominato ```parquet_etl_sample.py``` nel computer locale.

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Read clickstream_data from Storage Pool HDFS into a Spark data frame. Applies column renames.
df = spark.read.option("inferSchema", "true").csv('/securelake/landing/criteo/test.txt', sep='\t', 
    header=False).toDF("feat1","feat2","feat3","feat4","feat5","feat6","feat7","feat8",
    "feat9","feat10","feat11","feat12","feat13","catfeat1","catfeat2","catfeat3","catfeat4",
    "catfeat5","catfeat6","catfeat7","catfeat8","catfeat9","catfeat10","catfeat11","catfeat12",
    "catfeat13","catfeat14","catfeat15","catfeat16","catfeat17","catfeat18","catfeat19",
    "catfeat20","catfeat21","catfeat22","catfeat23","catfeat24","catfeat25","catfeat26")

# Prints the data frame inferred schema:
df.printSchema()

tot_rows = df.count()
print("Number of rows:", tot_rows)

# Drop the managed table
spark.sql("DROP TABLE dl_clickstream")

# Write data frame to HDFS managed table using optimized Delta Lake table format
df.write.format("parquet").mode("overwrite").saveAsTable("dl_clickstream")

print("Sample ETL pipeline completed")
```

#### <a name="copy-the-pyspark-application-to-hdfs"></a>Copiare l'applicazione PySpark in HDFS

L'applicazione deve essere archiviata in HDFS, in modo che il cluster possa accedervi per l'esecuzione. Per semplificare l'amministrazione, è consigliabile standardizzare e regolare i percorsi delle applicazioni all'interno del cluster. In questo caso di utilizzo di esempio, tutte le applicazioni della pipeline ETL verranno archiviate nel `hdfs:/apps/ETL-Pipelines` percorso. L'applicazione di esempio verrà archiviata in `hdfs:/apps/ETL-Pipelines/parquet_etl_sample.py` .

Eseguire il comando seguente per caricare __`parquet_etl_sample.py`__ dal computer di sviluppo o di staging locale al cluster HDFS. 

```bash
azdata bdc hdfs cp --from-path parquet_etl_sample.py  --to-path "hdfs:/apps/ETL-Pipelines/parquet_etl_sample.py"
```

### <a name="spark-scala"></a>[Spark scala](#tab/scala)

In questo esempio verrà usata la seguente applicazione Spark scritta in scala Spark.

```scala
import org.apache.spark.sql.SparkSession

object ParquetETLSample {
    def main(args: Array[String]) {
        val spark = SparkSession.builder.getOrCreate()
        
        val df = spark.read.
            option("inferSchema", "true").
            option("header", "false").
            option("delimiter", "\t").
            csv("/securelake/landing/criteo/test.txt").
            toDF("feat1","feat2","feat3","feat4","feat5","feat6","feat7","feat8","feat9","feat10","feat11","feat12","feat13","catfeat1","catfeat2","catfeat3","catfeat4","catfeat5","catfeat6","catfeat7","catfeat8","catfeat9","catfeat10","catfeat11","catfeat12","catfeat13","catfeat14","catfeat15","catfeat16","catfeat17","catfeat18","catfeat19","catfeat20","catfeat21","catfeat22","catfeat23","catfeat24","catfeat25","catfeat26")
        
        val tot_rows = df.count()
        println(s"Number of rows: $tot_rows")

        spark.sql("DROP TABLE dl_clickstream")

        df.write.format("parquet").mode("overwrite").saveAsTable("dl_clickstream")

        println("Sample ETL pipeline completed")
        
        spark.stop()
    }
}
```

#### <a name="bundle-and-copy-the-spark-application-to-hdfs"></a>Aggregare e copiare l'applicazione Spark in HDFS

La documentazione di Spark consiglia di creare un file JAR (o bundle) di __assembly__ contenente l'applicazione e tutte le dipendenze. Si tratta di un passaggio obbligatorio per inviare il bundle dell'applicazione al cluster per l'esecuzione. La configurazione di un ambiente di sviluppo Spark completo non rientra nell'ambito di questa guida. per altre informazioni, vedere la documentazione ufficiale di [Spark sulla creazione di applicazioni autosufficienti](https://spark.apache.org/docs/latest/quick-start.html#self-contained-applications) .

Questo esempio presuppone che un bundle jar dell'applicazione denominato `parquet-etl-sample.jar` sia compilato e disponibile. Eseguire il comando seguente per caricare il bundle dal computer di sviluppo o di staging locale al cluster HDFS.

```bash
azdata bdc hdfs cp --from-path parquet-etl-sample.jar  --to-path "hdfs:/apps/ETL-Pipelines/parquet-etl-sample.jar"
```

### <a name="spark-sql"></a>[Spark SQL](#tab/sql)

Questo esempio USA Spark SQL per eseguire la logica di inserimento usando tabelle e viste per offrire un approccio incentrato su SQL a ETL.

```sql
DROP VIEW IF EXISTS etl_clickstream;

CREATE TEMPORARY VIEW etl_clickstream
USING CSV
OPTIONS (path "/securelake/landing/criteo/test.txt", header "false", delimiter "\t", mode "FAILFAST");

DROP TABLE IF EXISTS dl_clickstream;

CREATE TABLE dl_clickstream (
    feat1 integer,
    feat2 integer,
    feat3 integer,
    feat4 integer,
    feat5 integer,
    feat6 integer,
    feat7 integer,
    feat8 integer,
    feat9 integer,
    feat10 integer,
    feat11 integer,
    feat12 integer,
    feat13 integer,
    catfeat1 string,
    catfeat2 string,
    catfeat3 string,
    catfeat4 string,
    catfeat5 string,
    catfeat6 string,
    catfeat7 string,
    catfeat8 string,
    catfeat9 string,
    catfeat10 string,
    catfeat11 string,
    catfeat12 string,
    catfeat13 string,
    catfeat14 string,
    catfeat15 string,
    catfeat16 string,
    catfeat17 string,
    catfeat18 string,
    catfeat19 string,
    catfeat20 string,
    catfeat21 string,
    catfeat22 string,
    catfeat23 string,
    catfeat24 string,
    catfeat25 string,
    catfeat26 string
) 
USING PARQUET
AS SELECT * FROM etl_clickstream;
```

#### <a name="copy-the-spark-sql-application-to-hdfs"></a>Copiare l'applicazione Spark SQL in HDFS

Eseguire il comando seguente per caricare il __```parquet-etl-sample.sql```__ dal computer di sviluppo o di staging locale al cluster HDFS.

```bash
azdata bdc hdfs cp --from-path parquet-etl-sample.sql --to-path "hdfs:/apps/ETL-Pipelines/parquet-etl-sample.sql"
```

---

#### <a name="execute-the-spark-application"></a>Eseguire l'applicazione Spark

Usare il comando seguente per inviare l'applicazione a SQL Server Spark di integrazione applicativa dei dati per l'esecuzione.


# <a name="pyspark-and-azdata"></a>[PySpark e azdata](#tab/azdata/pyspark)

Si tratta del __`azdata`__ comando che esegue l'applicazione utilizzando parametri comunemente specificati. Per le opzioni di parametro complete per `azdata bdc spark batch create` , vedere [`azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md) .

Questa applicazione richiede il `spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation` parametro di configurazione di Spark, quindi il comando usa l' `--config` opzione. Questo è stato intenzionale a esemplificare il passaggio delle configurazioni nella sessione di Spark. È possibile utilizzare l' `--config` opzione per specificare più parametri di configurazione. Questa operazione può essere eseguita anche all'interno della sessione dell'applicazione impostando la configurazione nell'oggetto SparkSession.

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/parquet_etl_sample.py \
--config '{"spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyETLPipelinePySpark --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

# <a name="pyspark-and-curl-using-livy"></a>[PySpark e curl con Livio](#tab/curl/pyspark)

Questo è il __`curl`__ comando che esegue l'applicazione con Livio. Assicurarsi di sostituire USER, PASSWORD e LIVY_ENDPOINT per riflettere l'ambiente.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "file": "/apps/ETL-Pipelines/parquet_etl_sample.py",
    "name": "MyETLPipelinePySpark",
    "numExecutors": 2,
    "executorCores": 2,
    "executorMemory": "1664m",
    "conf": {
        "spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"
    }
}
EOF
```

# <a name="scala-and-azdata"></a>[Scala e azdata](#tab/azdata/scala)

Si tratta del __`azdata`__ comando che esegue l'applicazione utilizzando parametri comunemente specificati. Per le opzioni di parametro complete per `azdata bdc spark batch create` , vedere [`azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md) .

Questa applicazione richiede il `spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation` parametro di configurazione di Spark, quindi il comando usa l' `--config` opzione. Questo è stato intenzionale a esemplificare il passaggio delle configurazioni nella sessione di Spark. È possibile utilizzare l' `--config` opzione per specificare più parametri di configurazione. Questa operazione può essere eseguita anche all'interno della sessione dell'applicazione impostando la configurazione nell'oggetto SparkSession.

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/parquet-etl-sample.jar \
--class "ParquetETLSample" \
--config '{"spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyETLPipeline --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

# <a name="scala-and-curl-using-livy"></a>[Scala e curl con Livio](#tab/curl/scala)

Questo è il __`curl`__ comando che esegue l'applicazione con Livio. Assicurarsi di sostituire USER, PASSWORD e LIVY_ENDPOINT per riflettere l'ambiente.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "file": "/apps/ETL-Pipelines/parquet-etl-sample.jar",
    "class": "ParquetETLSample",
    "name": "MyETLPipeline",
    "numExecutors": 2,
    "executorCores": 2,
    "executorMemory": "1664m",
    "conf": {
        "spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"
    }
}
EOF
```

# <a name="sql-and-azdata"></a>[SQL e azdata](#tab/azdata/sql)

Si tratta del __`azdata`__ comando che esegue l'applicazione utilizzando parametri comunemente specificati. Per le opzioni di parametro complete per `azdata bdc spark batch create` , vedere [`azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md) .

Come per l'esempio PySpark, questa applicazione richiede anche il `spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation` parametro di configurazione di Spark, quindi il comando usa l' `--config` opzione. Questo è stato intenzionale a esemplificare il passaggio delle configurazioni nella sessione di Spark. È possibile utilizzare l' `--config` opzione per specificare più parametri di configurazione. Questa operazione può essere eseguita anche all'interno della sessione dell'applicazione impostando la configurazione nell'oggetto SparkSession.

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/parquet_etl_sample.sql \
--config '{"spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyETLPipelineSQL --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

# <a name="sql-and-curl-using-livy"></a>[SQL e curl con Livio](#tab/curl/sql)

Questo è il __`curl`__ comando che esegue l'applicazione con Livio. Assicurarsi di sostituire USER, PASSWORD e LIVY_ENDPOINT per riflettere l'ambiente.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "file": "/apps/ETL-Pipelines/parquet_etl_sample.sql",
    "name": "MyETLPipelineSQL",
    "numExecutors": 2,
    "executorCores": 2,
    "executorMemory": "1664m",
    "conf": {
        "spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"
    }
}
EOF
```

---

## <a name="monitoring-spark-jobs"></a>Monitoraggio dei processi Spark

I [ `azdata bdc spark batch` comandi](../azdata/reference/reference-azdata-bdc-spark.md) contengono azioni di gestione per i processi batch Spark.

Per __elencare tutti i processi in esecuzione__, eseguire il comando seguente.

Questo è il `azdata` comando:

```bash
azdata bdc spark batch list -o table
```

Si tratta del `curl` comando che usa Livio.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches
```

Per __ottenere informazioni__ per un batch Spark con l'ID specificato, eseguire il comando seguente. `batch id`Viene restituito da "Spark batch create".

Questo è il `azdata` comando:

```bash
azdata bdc spark batch info --batch-id 0
```

Si tratta del `curl` comando che usa Livio.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches/<BATCH_ID>
```

Per __ottenere informazioni sullo stato__ per un batch Spark con l'ID specificato, eseguire il comando seguente.

Questo è il `azdata` comando:

```bash
azdata bdc spark batch state --batch-id 0
```

Si tratta del `curl` comando che usa Livio.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches/<BATCH_ID>/state
```

Per __ottenere i log__ per un batch Spark con l'ID specificato, eseguire il comando seguente.

Questo è il `azdata` comando:

```bash
azdata bdc spark batch log --batch-id 0
```

Si tratta del `curl` comando che usa Livio.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches/<BATCH_ID>/log
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla risoluzione dei problemi relativi al codice Spark, vedere [risolvere i problemi del notebook di pyspark](troubleshoot-pyspark-notebook.md).

Un set completo di codice di esempio Spark è disponibile nei [cluster SQL Server Big Data esempi Spark](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark) su GitHub.

Per ulteriori informazioni sul cluster SQL Server Big Data e sugli scenari correlati, vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md) .
