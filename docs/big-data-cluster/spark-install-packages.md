---
title: Gestione libreria Spark
titleSuffix: SQL Server big data clusters
description: Gestione libreria Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 01/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 70611d3f7d4ed6825911908942d9ed707dabbd2e
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549404"
---
# <a name="spark-library-management"></a>Gestione libreria Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo fornisce indicazioni su come importare e installare i pacchetti per una sessione Spark tramite le configurazioni di sessione e notebook.

## <a name="built-in-tools"></a>Strumenti predefiniti
Pacchetti di base Spark e Hadoop Python 3,7 e Python 2,7 Pandas, Sklearn, NumPy e altri pacchetti di elaborazione dati.
Pacchetti R e riparazione Sparklyr

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Installare i pacchetti da un repository maven nel cluster Spark in fase di esecuzione
I pacchetti Maven possono essere installati nel cluster Spark usando la configurazione di celle del notebook all'inizio della sessione di Spark. Prima di avviare una sessione di Spark in Azure Data Studio, eseguire il codice seguente:

```
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-job-submission-time"></a>Installare i pacchetti Python nel processo PySpark-ora di invio
1. Specificare il percorso di un file di requirements.txt in HDFS da usare come riferimento per i pacchetti da installare.
```
%%configure -f \
{"conf": {
    "spark.pyspark.virtualenv.enabled" : "true",
    "spark.pyspark.virtualenv.type" : "conda",
    "spark.pyspark.virtualenv.requirements" : "requirements.txt",
    "spark.pyspark.virtualenv.bin.path" : "/opt/mls/python/bin/conda"
    }, 
"files": ["hdfs://nmnode-0/tmp/requirements.txt"]
}
```
2. Creare un virtualenv conda senza un file dei requisiti e aggiungere i pacchetti in modo dinamico durante la sessione di Spark.
```
%%configure -f \
{"conf": {
    'spark.pyspark.virtualenv.enabled' : 'true',
    'spark.pyspark.virtualenv.type' : 'conda',
    'spark.pyspark.virtualenv.bin.path' : '/opt/mls/python/bin/conda',
    'spark.pyspark.virtualenv.python_version': '3.6'
 }
 ```

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importa. jar da HDFS per l'uso in fase di esecuzione
Importa jar in fase di esecuzione tramite la configurazione della cella Azure Data Studio notebook.

```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Import. jar in fase di esecuzione tramite la configurazione della cella Azure Data Studio notebook
```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni sul cluster SQL Server Big Data e sugli scenari correlati, vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md) .