---
title: Apache Spark e Apache Hadoop
titleSuffix: Configure Apache Spark and Apache Hadoop in Big Data Clusters
description: I cluster Big Data di SQL Server consentono soluzioni Spark e HDFS. Di seguito sono disponibili informazioni sulla configurazione.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2fa2b5a3e9b7f996866ae1fca16c32ff1e1ba39e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039157"
---
# <a name="configure-apache-spark-and-apache-hadoop-in-big-data-clusters"></a>Configurare Apache Spark e Apache Hadoop nei cluster Big Data

Per configurare Apache Spark e Apache Hadoop nei cluster Big Data, è necessario modificare il profilo del cluster al momento della distribuzione.

Un cluster Big Data ha quattro categorie di configurazione: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark`, `sql` sono servizi. Ogni servizio è mappato alla categoria di configurazione con lo stesso nome. Tutte le configurazioni del gateway sono associate alla categoria `gateway`. 

Ad esempio, tutte le configurazioni nel servizio `hdfs` appartengono alla categoria `hdfs`. Si noti che tutte le configurazioni di Hadoop (core-site), HDFS e Zookeeper appartengono alla categoria `hdfs`. Tutte le configurazioni di Livy, Spark, Yarn, Hive e Metastore appartengono alla categoria `spark`. 

[Configurazioni supportate](reference-config-spark-hadoop.md#supported-configurations) elenca le proprietà di Apache Spark e Hadoop che è possibile configurare quando si distribuisce un cluster Big Data di SQL Server.

Le sezioni seguenti elencano le proprietà **non è possibile** modificare in un cluster:

- [Configurazioni `spark` non supportate](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Configurazioni `hdfs` non supportate](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Configurazioni `gateway` non supportate](reference-config-spark-hadoop.md#unsupported-gateway-configurations)


## <a name="configurations-via-cluster-profile"></a>Configurazioni tramite il profilo cluster

Nel profilo cluster sono disponibili risorse e servizi. In fase di distribuzione, è possibile specificare le configurazioni in uno dei due modi seguenti: 

* Prima di tutto, a livello di risorsa: 

   Gli esempi seguenti sono i file di patch per il profilo: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.zookeeper.spec.settings", 
         "value": { 
           "hdfs": { 
             "zoo-cfg.syncLimit": "6" 
           } 
         } 
   }
   ```

   Oppure: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.gateway.spec.settings", 
         "value": { 
           "gateway": { 
             "gateway-site.gateway.httpclient.socketTimeout": "95s" 
           } 
         } 
   } 
   ```

* In secondo luogo, a livello di servizio. Assegnare più risorse a un servizio e specificare le configurazioni per il servizio.

Di seguito è riportato un esempio del file di patch per il profilo per l'impostazione delle dimensioni del blocco HDFS: 

   ```json
   { 
         "op": "add", 
         "path": "spec.services.hdfs.settings", 
         "value": { 
           "hdfs-site.dfs.block.size": "268435456" 
        } 
   } 
   ```

Il servizio `hdfs` è definito come segue:

```json
{ 
  "spec": { 
   "services": { 
     "hdfs": { 
        "resources": [ 
          "nmnode-0", 
          "zookeeper", 
          "storage-0", 
          "sparkhead" 
        ], 
        "settings":{ 
          "hdfs-site.dfs.block.size": "268435456" 
        } 
      } 
    } 
  } 
} 
```
 
> [!NOTE]
> Le configurazioni a livello di risorsa sostituiscono le configurazioni a livello di servizio. Una risorsa può essere assegnata a più servizi.

## <a name="enable-spark-in-the-storage-pool"></a>Abilitare Spark nel pool di archiviazione
Oltre alle configurazioni Apache supportate, viene anche offerta la possibilità di specificare se i processi Spark possono essere eseguiti o meno nel pool di archiviazione. Questo valore booleano, `includeSpark`, si trova nel file di configurazione `bdc.json` in `spec.resources.storage-0.spec.settings.spark`.

Un esempio di definizione del pool di archiviazione in bdc.json può essere simile al seguente:
```json
...
"storage-0": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Storage",
                    "replicas": 2,
                    "settings": {
                        "spark": {
                            "includeSpark": "true"
                        }
                    }
                }
            }
```


## <a name="limitations"></a>Limitazioni

Le configurazioni possono essere specificate solo a livello di categoria. Per specificare più configurazioni con la stessa sottocategoria, non è possibile estrarre il prefisso comune nel profilo cluster. 

```json
{ 
      "op": "add", 
      "path": "spec.services.hdfs.settings.core-site.hadoop", 
      "value": { 
        “proxyuser.xyz.users”: “*”, 
        “proxyuser.abc.users”: “*” 
     } 
} 
```

## <a name="next-steps"></a>Passaggi successivi

- [Proprietà di configurazione di Apache Spark e Apache Hadoop (HDFS).](reference-config-spark-hadoop.md)
- [Informazioni di riferimento su [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)
- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)