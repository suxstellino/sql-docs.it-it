---
title: Gestione libreria Spark
titleSuffix: SQL Server big data clusters
description: Gestione libreria Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bed687cb003bfc7748aefa3c98ae5e19089f9685
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837058"
---
# <a name="spark-library-management"></a>Gestione libreria Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo fornisce indicazioni su come importare e installare i pacchetti per una sessione Spark tramite le configurazioni di sessione e notebook.

## <a name="built-in-tools"></a>Strumenti predefiniti

Pacchetti di base Spark (scala 2,11) e Hadoop. 

PySpark (Python 3,7). Pandas, Sklearn, NumPy e altri pacchetti di elaborazione dati e machine learning.

RIPARAZIONE dei pacchetti 3.5.2. Sparklyr e Sparkr per i carichi di lavoro R Spark.

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Installare i pacchetti da un repository maven nel cluster Spark in fase di esecuzione

I pacchetti Maven possono essere installati nel cluster Spark usando la configurazione di celle del notebook all'inizio della sessione di Spark. Prima di avviare una sessione di Spark in Azure Data Studio, eseguire il codice seguente:

```python
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-at-runtime"></a>Installare i pacchetti Python in PySpark in fase di esecuzione

Gestione dei pacchetti a livello di sessione e di processo garantisce la coerenza e l'isolamento della libreria. La configurazione è una configurazione della libreria standard Spark che può essere applicata alle sessioni di Livio. __azdata Spark__ supportano queste configurazioni. Gli esempi riportati di seguito sono illustrati come __Azure Data Studio notebook__ consentono di configurare celle che devono essere eseguite dopo la connessione a un cluster con il kernel PySpark.

Se la configurazione __"Spark. pyspark. virtualenv. Enabled": "true"__ non è impostata, la sessione utilizzerà le librerie predefinite del cluster Python e le librerie installate.

### <a name="sessionjob-configuration-with-requirementstxt"></a>Configurazione di sessione/processo con requirements.txt

Se specificare il percorso di un file di requirements.txt in HDFS da usare come riferimento per i pacchetti da installare.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.7",
        "spark.pyspark.virtualenv.requirements" : "hdfs://user/project-A/requirements.txt"
    }
}
```

### <a name="sessionjob-configuration-with-different-python-versions"></a>Configurazione di sessioni/processi con diverse versioni di Python

Creare un virtualenv conda senza un file dei requisiti e aggiungere i pacchetti in modo dinamico durante la sessione di Spark.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.6"
    }
}
```

### <a name="library-installation"></a>Installazione di una libreria

Eseguire l' __SC.install_packages__ per installare le librerie in modo dinamico nella sessione. Le librerie verranno installate nel driver e in tutti i nodi dell'executor.

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

È anche possibile installare più librerie nello stesso comando usando una matrice.

 ```python
sc.install_packages(["numpy==1.11.0", "xgboost"])
import numpy as np
import xgboost as xgb
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importa. jar da HDFS per l'uso in fase di esecuzione
Importa jar in fase di esecuzione tramite la configurazione della cella Azure Data Studio notebook.

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Import. jar in fase di esecuzione tramite la configurazione della cella Azure Data Studio notebook

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni sul cluster SQL Server Big Data e sugli scenari correlati, vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md) .