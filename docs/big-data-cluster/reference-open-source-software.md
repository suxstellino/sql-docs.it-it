---
title: Informazioni di riferimento sul software open source
titleSuffix: SQL Server Big Data Clusters
description: Identificare il software e la versione open source usati nei cluster SQL Server Big Data.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 210787512ea3ded5aaab660c8973482bb13c2ff4
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646363"
---
# <a name="open-source-software-reference"></a>Informazioni di riferimento sul software open source

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

Un cluster SQL Server Big Data include alcuni contenitori sviluppati da progetti open source. Questo articolo identifica i progetti specifici e le versioni di questi contenitori.

## <a name="project-list"></a>Elenco progetti

La tabella seguente illustra i progetti open source in uso a partire da [!INCLUDE [sssql19-md](../includes/sssql19-md.md)] CU8 e precedenti e CU9 e versioni successive. 

| Project | CU8 e versioni precedenti | A partire da CU9 |
|--|--|--|
| [collectd](https://collectd.org/) | 5.8.1 | 5.12 |
| [InfluxDB](https://www.influxdata.com) | 1.7.6 | 1.8.3 |
| [Elasticsearch](https://www.elastic.co/) | 7.0.1 | 7.9.1 |
| [Fluent Bit](https://docs.fluentbit.io/manual/about/what-is-fluent-bit) | 1.1.1 | 1.6.3 |
| [Grafana](https://grafana.com/) | 6.3.6 | 7.3.1 |
| Hadoop <br/>[HDFS DataNode](concept-storage-pool.md)<br/>[HDFS NameNode](https://cwiki.apache.org/confluence/display/HADOOP2/NameNode) |3.1.3 +|3.3.0|
| [Hive (Metastore)](https://hive.apache.org/) |2.3.7|2.3.7<br/>3.0.0 (autonomo)<br/>3.1.2 (hive)|
| [Kibana](https://www.elastic.co/kibana) | 7.0.1 | 7.9.1 |
| [Knox](https://knox.apache.org/) |1.2.0|1.4.0|
| [Livio](https://livy.apache.org/) |0.6.0|0.7.0|
| [opendistro-elasticsearch-sicurezza](https://www.elastic.co/what-is/elastic-stack-security) | 1.0.0.1 | 1.10.1.0 |
| [Openresty (Nginx)](https://openresty.org/) | 1.15.8 | 1.17.8.2 |
| [Spark](configure-spark-hdfs.md) |2.4.6 +|2.4.10|
| [Telegraf](https://docs.influxdata.com/telegraf/) | 1.10.3 | 1.16.1 |
| [ZooKeeper](https://cwiki.apache.org/confluence/display/zookeeper) |3.5.8|3.6.2

## <a name="next-steps"></a>Passaggi successivi

Esaminare [le risorse distribuite con il cluster di Big Data](concept-architecture-pods.md).