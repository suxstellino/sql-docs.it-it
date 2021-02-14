---
title: 'Creare ed esportare modelli di ML Spark: MLeap'
titleSuffix: SQL Server Big Data Clusters
description: Usare PySpark per eseguire il training e creare modelli di Machine Learning con Spark in cluster Big Data di SQL Server. Esportare con MLeap e quindi assegnare punteggi al modello con Java in SQL Server.
author: RogPodge
ms.author: roliu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 4b403644de65f866d4bafb191356fd08212c706f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043601"
---
# <a name="create-export-and-score-spark-machine-learning-models-on-big-data-clusters-2019"></a>Creare, esportare e assegnare punteggi ai modelli di Machine Learning Spark in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

Nell'esempio seguente viene illustrato come creare un modello con [Machine Learning Spark](https://spark.apache.org/docs/latest/ml-guide.html), esportare il modello in [MLeap](http://mleap-docs.combust.ml/) e quindi assegnare punteggi al modello in SQL Server con l'[estensione del linguaggio Java](../language-extensions/language-extensions-overview.md). Questa operazione viene eseguita nel contesto di un cluster Big Data di SQL Server 2019.

Nella figura seguente vengono illustrate le operazioni eseguite in questo esempio:

![Eseguire il training, assegnare punteggi ed esportare con Spark](./media/spark-create-machine-learning-model/train-score-export-with-spark.png)

## <a name="prerequisites"></a>Prerequisiti

Tutti i file necessari per questo esempio si trovano in [https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml).

Per poter eseguire l'esempio, è necessario anche soddisfare i prerequisiti seguenti:

- Un [cluster Big Data di SQL Server](deploy-get-started.md)

- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **curl**
   - **Azure Data Studio**

## <a name="model-training-with-spark-ml"></a>Training del modello con Machine Learning Spark

In questo esempio, per creare un modello di pipeline per Machine Learning Spark vengono usati dati di Census (**AdultCensusIncome.csv**).

1. Usare il file [mleap_sql_test/setup.sh](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/setup.sh) per scaricare il set di dati da Internet e inserirlo nel sistema HDFS all'interno del cluster Big Data di SQL Server, in modo che sia accessibile da Spark.

1. Scaricare quindi il notebook di esempio [train_score_export_ml_models_with_spark.ipynb](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb). Da una riga di comando di PowerShell o della shell Bash, eseguire il comando seguente per scaricare il notebook:

   ```PowerShell
   curl -o mssql_spark_connector.ipynb "https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb"
   ```

   Il notebook contiene le celle con i comandi necessari per questa sezione dell'esempio.

1. Aprire il notebook in Azure Data Studio ed eseguire ogni blocco di codice. Per altre informazioni sull'uso dei notebook, vedere [Come usare i notebook in SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).

Per prima cosa, i dati vengono letti in Spark e suddivisi in set di dati di training e di testing. Il codice esegue quindi il training di un modello di pipeline con i dati di training ed esporta infine il modello in un bundle MLeap.

> [!TIP]
> È possibile rivedere o eseguire il codice Python associato a questi passaggi anche all'esterno del notebook, nel file [mleap_sql_test/mleap_pyspark. py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_pyspark.py).

## <a name="model-scoring-with-sql-server"></a>Assegnazione di punteggi al modello con SQL Server

Ora che il modello di pipeline per Machine Learning Spark si trova nel formato di serializzazione comune [bundle MLeap](http://mleap-docs.combust.ml/core-concepts/mleap-bundles.html), è possibile assegnare punteggi al modello in Java anche senza la presenza di Spark.

Questo esempio usa l'[estensione del linguaggio Java](../language-extensions/language-extensions-overview.md) in SQL Server. Per poter assegnare i punteggi al modello in SQL Server, è prima necessario sviluppare un'applicazione Java in grado di caricare il modello in Java e assegnargli i punteggi. È possibile trovare il codice di esempio per questa applicazione Java nella [cartella mssql-mleap-app](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mssql-mleap-app).

Dopo aver creato l'esempio, è possibile usare Transact-SQL per chiamare l'applicazione Java e assegnare i punteggi al modello con una tabella di database, disponibile nel file di origine [mleap_sql_test/mleap_sql_tests.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_sql_tests.py).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md)