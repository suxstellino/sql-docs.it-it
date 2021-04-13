---
title: Guida di Spark streaming in cluster SQL Server Big Data
titleSuffix: SQL Server Big Data Clusters
description: Questa guida illustra i casi d'uso di streaming e come implementarla usando SQL Server le funzionalità di cluster di Big Data.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/06/2021
ms.topic: guide
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 05e35cf861a46a667ce25c88e9d6d2ea17b1183b
ms.sourcegitcommit: 14b97028da137f872a0a35cfe9d5a639a2d116a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107222585"
---
# <a name="sql-server-big-data-clusters-spark-streaming-guide"></a>Guida di Spark streaming in cluster SQL Server Big Data

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa guida illustra i casi d'uso di streaming e come implementarla usando SQL Server cluster Big Data Spark.

Questa guida illustra come eseguire queste operazioni:

> [!div class="checklist"]
> * Caricare le librerie di streaming da usare con PySpark e scala Spark.
> * Implementare tre modelli di flusso comuni usando SQL Server cluster di Big Data.

## <a name="prerequisites"></a>Prerequisiti

* Distribuzione di SQL Server cluster di Big Data
* Una delle due opzioni seguenti:
    * Apache Kafka cluster 2.0 +
    * Hub eventi di Azure-spazio dei nomi e hub eventi

Questa guida presuppone un livello di conoscenza dei concetti e delle architetture della tecnologia di streaming.
Gli articoli seguenti forniscono linee di base concettuali ottimali:

* [Guida all'architettura dei dati-elaborazione in tempo reale](/azure/architecture/data-guide/big-data/real-time-processing)
* [Usare Hub eventi di Azure da applicazioni Apache Kafka](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)
* [Guida all'architettura dei dati-scelta di una tecnologia di inserimento di messaggi in tempo reale in Azure](/azure/architecture/data-guide/technology-choices/real-time-ingestion)

### <a name="apache-kafka-and-azure-event-hub-conceptual-mapping"></a>Apache Kafka e mapping concettuale di hub eventi di Azure

|Concetto di Apache Kafka|Concetto di Hub eventi|
|--------------------|------------------|
|Cluster|Spazio dei nomi|
|Argomento|Hub eventi|
|Partition|Partition|
|Gruppo di consumer|Gruppo di consumer|
|Offset|Offset|

### <a name="reproducibility"></a>Reproducibility

Questa guida sfrutta l'applicazione Producer fornita dalla Guida [introduttiva: flusso di dati con hub eventi usando il protocollo Kafka](/azure/event-hubs/event-hubs-quickstart-kafka-enabled-event-hubs). Sono inoltre disponibili applicazioni di esempio in molti linguaggi di programmazione negli [Hub eventi di Azure per Apache Kafka](https://github.com/Azure/azure-event-hubs-for-kafka) pagina di GitHub che consentono di avviare scenari di streaming.

Ecco un modificato `producer.py` che trasmette i dati JSON dei sensori simulati nel motore di streaming usando un client compatibile con Kafka. È importante notare che hub eventi di Azure è compatibile con il protocollo Kafka. Seguire le istruzioni di installazione nel [repository GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/python) per ottenere l'esempio da usare. Il `conf` dizionario è il punto in cui si verificano tutte le informazioni di connessione e il chilometraggio può variare a seconda dell'ambiente. Assicurarsi di sostituire almeno `bootstrap.servers` e `sasl.password` come è la configurazione più pertinente nell'esempio di codice sotto.

```python
#!/usr/bin/env python
#
# Copyright (c) Microsoft Corporation. All rights reserved.
# Copyright 2016 Confluent Inc.
# Licensed under the MIT License.
# Licensed under the Apache License, Version 2.0
#
# Original Confluent sample modified for use with Azure Event Hubs for Apache Kafka Ecosystems

from confluent_kafka import Producer
import sys
import random
import time
import json

sensors = ["Sensor 1", "Sensor 2", "Sensor 3"]

if __name__ == '__main__':
    if len(sys.argv) != 2:
        sys.stderr.write('Usage: %s <topic>\n' % sys.argv[0])
        sys.exit(1)
    topic = sys.argv[1]

    # Producer configuration
    # See https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md
    # See https://github.com/edenhill/librdkafka/wiki/Using-SSL-with-librdkafka#prerequisites for SSL issues
    conf = {
        'bootstrap.servers': '',                     #replace!
        'security.protocol': 'SASL_SSL',
        'ssl.ca.location': '/usr/lib/ssl/certs/ca-certificates.crt',
        'sasl.mechanism': 'PLAIN',
        'sasl.username': '$ConnectionString',
        'sasl.password': '',                         #replace!
        'client.id': 'python-sample-producer'
    }

    # Create Producer instance
    p = Producer(**conf)

    def delivery_callback(err, msg):
        if err:
            sys.stderr.write('%% Message failed delivery: %s\n' % err)
        else:
            sys.stderr.write('%% Message delivered to %s [%d] @ %o\n' % (msg.topic(), msg.partition(), msg.offset()))

    # Simulate stream
    for i in range(0, 10000):
        try:
            payload = {
                'sensor': random.choice(sensors),
                'measure1': random.gauss(37, 7),
                'measure2': random.random(),
            }
            p.produce(topic, json.dumps(payload).encode('utf-8'), callback=delivery_callback)
            #p.produce(topic, str(i), callback=delivery_callback)
        except BufferError as e:
            sys.stderr.write('%% Local producer queue is full (%d messages awaiting delivery): try again\n' % len(p))
        p.poll(0)
        time.sleep(2)

    # Wait until all messages have been delivered
    sys.stderr.write('%% Waiting for %d deliveries\n' % len(p))
    p.flush()

```

Eseguire l'applicazione di esempio Producer usando il comando seguente, sostituendo `<my-sample-topic>` con le informazioni sull'ambiente.

```bash
python producer.py <my-sample-topic>
```

## <a name="streaming-scenarios"></a>Scenari di streaming

|Modello di flusso             |Descrizione dello scenario e implementazione         |
|------------------------------|------------------------------------------------|
|Pull da Kafka o hub eventi |Creare un processo di Spark streaming che estrae continuamente i dati dal motore di flusso, eseguendo trasformazioni e logica di analisi facoltative.|
|Sink di streaming dei dati in HDFS |In generale, questo è correlato al modello precedente; Dopo la logica di pull e trasformazione del flusso, i dati possono essere scritti in una vasta gamma di percorsi di dati per ottenere il requisito di persistenza dei dati desiderato.|
|Push da Spark a Kafka o hub eventi |Dopo l'elaborazione da parte di Spark, è possibile che i dati vengano inseriti nuovamente nel motore di streaming esterno. Ci sono molti scenari in cui si vuole, ad esempio la raccomandazione dei prodotti in tempo reale, la frode di micro batch e il rilevamento delle anomalie e così via.|

## <a name="sample-spark-streaming-application"></a>Applicazione Spark streaming di esempio

Questa applicazione di esempio implementa tutti e tre i modelli di flusso descritti nella sezione precedente. L'applicazione esegue le operazioni seguenti:

1. Configura le variabili di configurazione per la connessione al servizio di streaming
1. Crea un frame di dati di streaming Spark per il pull dei dati
1. Scrive localmente i dati aggregati in HDFS
1. Scrive i dati aggregati in un altro argomento nel servizio di streaming

Ecco il codice completo `sample-spark-streaming-python.py` :
```python
from pyspark import SparkContext, SparkConf
from pyspark.streaming import StreamingContext
from pyspark.sql import SparkSession
from pyspark.sql.types import *
from pyspark.sql.functions import *

# Sets up batch size to 15 seconds
duration_ms = 15000
# Changes Spark Session into a Structured Streaming context
sc = SparkContext.getOrCreate()
ssc = StreamingContext(sc, duration_ms)
spark = SparkSession(sc)

# Connection Information
bootstrap_servers = "" # Replace!
sasl = "" # Replace!
# Topic we will consume from
topic = "sample-topic"
# Topic we will write toa
topic_to = "sample-topic-processed"

# Define the schema to speed up processing
jsonSchema = StructType([StructField("sensor", StringType(), True), StructField("measure1", DoubleType(), True), StructField("measure2", DoubleType(), True)])

streaming_input_df = (
    spark.readStream \
    .format("kafka") \
    .option("subscribe", topic) \
    .option("kafka.bootstrap.servers", bootstrap_servers) \
    .option("kafka.sasl.mechanism", "PLAIN") \
    .option("kafka.security.protocol", "SASL_SSL") \
    .option("kafka.sasl.jaas.config", sasl) \
    .option("kafka.request.timeout.ms", "60000") \
    .option("kafka.session.timeout.ms", "30000") \
    .option("failOnDataLoss", "true") \
    .option("startingOffsets", "latest") \
    .load()
)

def foreach_batch_function(df, epoch_id):
    # Transform and write batchDF
    if df.count() <= 0:
        None
    else:
        # Create a data frame to be written to HDFS
        sensor_df = df.selectExpr('CAST(value AS STRING)').select(from_json("value", jsonSchema).alias("value")).select("value.*")
        # root
        #  |-- sensor: string (nullable = true)
        #  |-- measure1: double (nullable = true)
        #  |-- measure2: double (nullable = true)
        sensor_df.persist()
        # Write to HDFS:
        sensor_df.write.format('parquet').mode('append').saveAsTable('sensor_data')
        # Create a summarization dataframe
        sensor_stats_df = (sensor_df.groupBy('sensor').agg({'measure1':'avg', 'measure2':'avg', 'sensor':'count'}).withColumn('ts', current_timestamp()).withColumnRenamed('avg(measure1)', 'measure1_avg').withColumnRenamed('avg(measure2)', 'measure2_avg').withColumnRenamed('avg(measure1)', 'measure1_avg').withColumnRenamed('count(sensor)', 'count_sensor'))
        # root
        # |-- sensor: string (nullable = true)
        # |-- measure2_avg: double (nullable = true)
        # |-- measure1_avg: double (nullable = true)
        # |-- count_sensor: long (nullable = false)
        # |-- ts: timestamp (nullable = false)
        sensor_stats_df.write.format('parquet').mode('append').saveAsTable('sensor_data_stats')
        # Group by and send metrics to an output kafka topic:
        sensor_stats_df.writeStream
            .format("kafka")
            .option("topic", topic_to)
            .option("kafka.bootstrap.servers", bootstrap_servers)
            .option("kafka.sasl.mechanism", "PLAIN")
            .option("kafka.security.protocol", "SASL_SSL")
            .option("kafka.sasl.jaas.config", sasl)
            .save()
        # For example, you could write to SQL Server
        # df.write.format('com.microsoft.sqlserver.jdbc.spark').mode('append').option('url', url).option('dbtable', datapool_table).save()
        sensor_df.unpersist()


writer = streaming_input_df.writeStream.foreachBatch(foreach_batch_function).start().awaitTermination()

```

Di seguito sono riportate le tabelle che devono essere create con __Spark SQL__:
```sql
CREATE TABLE IF NOT EXISTS sensor_data (sensor string, measure1 double, measure2 double)
USING PARQUET;

CREATE TABLE IF NOT EXISTS sensor_data_stats (sensor string, measure2_avg double, measure1_avg double, count_sensor long, ts timestamp)
USING PARQUET;
```


### <a name="copy-the-application-to-hdfs"></a>Copiare l'applicazione in HDFS

```bash
azdata bdc hdfs cp --from-path sample-spark-streaming-python.py --to-path "hdfs:/apps/ETL-Pipelines/sample-spark-streaming-python.py"
```

### <a name="configuring-kafka-libraries"></a>Configurazione delle librerie Kafka

Il passaggio più importante consiste nell'impostare le librerie client Kafka sull'applicazione prima dell'invio.

Sono disponibili __due librerie obbligatorie__:

* [Kafka-clients](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients) -Core Library che consente il supporto del protocollo Kafka e la connettività.
* [Spark-SQL-Kafka](https://mvnrepository.com/artifact/org.apache.spark/spark-sql-kafka-0-10) -Abilita la funzionalità del frame di dati Spark SQL nei flussi Kafka.

Entrambe le librerie devono essere conformi ai requisiti seguenti:

* Destinazione scala 2,11 e Spark 2.4.7. Si tratta di un requisito SQL Server BDC come per CU9 o versione successiva.
* Essere compatibile con il server di streaming.

> [!CAUTION]
   > Come regola generale, utilizzare la libreria compatibile più recente. Il codice fornito in questa guida è stato testato con Apache Kafka per hub eventi di Azure e viene fornito così com'è, non come un'istruzione di supporto. Apache Kafka offre la compatibilità bidirezionale dei client in base alla progettazione, ma le implementazioni delle librerie variano in base ai linguaggi di programmazione. Fare sempre riferimento alla documentazione della piattaforma Kafka per eseguire correttamente il mapping della compatibilità.

#### <a name="shared-library-locations-for-jobs-on-hdfs"></a>Percorsi di librerie condivise per i processi in HDFS

Se più applicazioni si connettono allo stesso cluster Kafka o l'organizzazione ha un singolo cluster Kafka con versione, copiare i file jar della libreria appropriati in un percorso condiviso in HDFS. Tutti i processi devono quindi fare riferimento agli stessi file di libreria.

Copiare le librerie nel percorso comune:

```bash
azdata bdc hdfs cp --from-path kafka-clients-2.7.0.jar --to-path "hdfs:/apps/jars/kafka-clients-2.7.0.jar"
azdata bdc hdfs cp --from-path spark-sql-kafka-0-10_2.11-2.4.7.jar --to-path "hdfs:/apps/jars/spark-sql-kafka-0-10_2.11-2.4.7.jar"
```

#### <a name="dynamically-install-the-libraries"></a>Installare le librerie in modo dinamico

È possibile installare dinamicamente i pacchetti nell'invio di processi usando le [funzionalità di gestione dei pacchetti](spark-install-packages.md) di SQL Server cluster di Big Data. Si verifica una riduzione del tempo di avvio del processo a causa dei download ricorrenti dei file di libreria in ogni invio di processo.

### <a name="submit-the-spark-streaming-job-using-azdata"></a>Inviare il processo di Spark streaming usando `azdata`

Nel primo esempio vengono usati i file jar della libreria condivisa in HDFS:

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/sample-spark-streaming-python.py \
-j '["/apps/jars/kafka-clients-2.7.0.jar","/apps/jars/spark-sql-kafka-0-10_2.11-2.4.7.jar"]' \
--config '{"spark.streaming.concurrentJobs":"3","spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyStreamingETLPipelinePySpark --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

Questo esempio usa la gestione dinamica dei pacchetti per installare le dipendenze:

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/sample-spark-streaming-python.py \
--config '{"spark.jars.packages": "org.apache.kafka:kafka-clients:2.7.0,org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.7","spark.streaming.concurrentJobs":"3","spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyStreamingETLPipelinePySpark --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come inviare processi Spark a SQL Server cluster Big Data usando gli endpoint azdata o Livio, vedere [inviare processi Spark usando gli strumenti da riga di comando](spark-submit-job-command-line.md).

Per ulteriori informazioni sul cluster SQL Server Big Data e sugli scenari correlati, vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md) .
