---
title: Configurare le distribuzioni
titleSuffix: SQL Server big data clusters
description: Informazioni su come personalizzare la distribuzione di un cluster Big Data con file di configurazione compilati nello strumento di gestione azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d983b4d0d7cfb02a587675984fdc42c54bf9f0ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047201"
---
# <a name="configure-deployment-settings-for-cluster-resources-and-services"></a>Configurare le impostazioni di distribuzione per risorse e servizi cluster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Partendo da un set predefinito di profili di configurazione integrati nello strumento di gestione [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], è possibile modificare facilmente le impostazioni predefinite per soddisfare al meglio i requisiti del carico di lavoro del cluster Big Data. La struttura dei file di configurazione permette di aggiornare in modo granulare le impostazioni per ogni servizio della risorsa.

Guardare questo video di 13 minuti per una panoramica della configurazione di cluster Big Data:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Cluster-Configuration/player?WT.mc_id=dataexposed-c9-niner]

> [!TIP]
> Per informazioni dettagliate su come distribuire servizi a disponibilità elevata, fare riferimento agli articoli su come configurare la **disponibilità elevata** per componenti cruciali come l'[istanza master di SQL Server](deployment-high-availability.md) o il nodo [NameNode di HDFS](deployment-high-availability-hdfs-spark.md).

È anche possibile impostare configurazioni a livello di risorsa o aggiornare le configurazioni per tutti i servizi in una risorsa. Ecco un riepilogo della struttura per `bdc.json`:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "BigDataCluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "resources": {
            "nmnode-0": {...
            },
            "sparkhead": {...
            },
            "zookeeper": {...
            },
            "gateway": {...
            },
            "appproxy": {...
            },
            "master": {...
            },
            "compute-0": {...
            },
            "data-0": {...
            },
            "storage-0": {...
        },
        "services": {
            "sql": {
                "resources": [
                    "master",
                    "compute-0",
                    "data-0",
                    "storage-0"
                ]
            },
            "hdfs": {
                "resources": [
                    "nmnode-0",
                    "zookeeper",
                    "storage-0",
                    "sparkhead"
                ],
                "settings": {...
            },
            "spark": {
                "resources": [
                    "sparkhead",
                    "storage-0"
                ],
                "settings": {...
            }
        }
    }
}
```

Per aggiornare le configurazioni a livello di risorsa, ad esempio le istanze in un pool, è necessario aggiornare la specifica della risorsa. Ad esempio, per aggiornare il numero di istanze nel pool di calcolo, è necessario modificare questa sezione nel file di configurazione `bdc.json`:

```json
"resources": {
    ...
    "compute-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Compute",
            "replicas": 4
        }
    }
    ...
}
``` 

Lo stesso vale per la modifica delle impostazioni di un singolo servizio all'interno di una risorsa specifica. Ad esempio, per modificare le impostazioni della memoria di Spark solo per il componente Spark nel pool di archiviazione, è necessario aggiornare la risorsa `storage-0` con una sezione `settings` per il servizio `spark` nel file di configurazione `bdc.json`.

```json
"resources":{
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
                    "spark-defaults-conf.spark.driver.memory": "2g",
                    "spark-defaults-conf.spark.driver.cores": "1",
                    "spark-defaults-conf.spark.executor.instances": "3",
                    "spark-defaults-conf.spark.executor.memory": "1536m",
                    "spark-defaults-conf.spark.executor.cores": "1",
                    "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                    "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                    "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                    "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                    "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
                }
            }
        }
    }
    ...
}
```

Per applicare le stesse configurazioni per un servizio associato a più risorse, è necessario aggiornare la sezione `settings` corrispondente nella sezione `services`. Ad esempio, per configurare le stesse impostazioni per Spark nel pool di archiviazione e nei pool Spark, è necessario aggiornare la sezione `settings` nella sezione del servizio `spark` nel file di configurazione `bdc.json`.

```json
"services": {
    ...
    "spark": {
        "resources": [
            "sparkhead",
            "storage-0"
        ],
        "settings": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
        }
    }
    ...
}
```

Per personalizzare i file di configurazione della distribuzione del cluster, è possibile usare qualsiasi editor in formato JSON, ad esempio VSCode. Per la creazione di script per queste modifiche a scopo di automazione, usare il comando `azdata bdc config`. Questo articolo descrive come configurare le distribuzioni di cluster Big Data modificando i file di configurazione della distribuzione. Fornisce anche alcuni esempi su come modificare la configurazione per diversi scenari. Per altre informazioni sull'uso di file di configurazione nelle distribuzioni, vedere le [linee guida per la distribuzione](deployment-guidance.md#configfile).

## <a name="prerequisites"></a>Prerequisiti

- [Installare azdata](../azdata/install/deploy-install-azdata.md).

- Ognuno degli esempi in questa sezione presuppone che sia stata creata una copia di una delle configurazioni standard. Per altre informazioni, vedere [Creare una pubblicazione](deployment-guidance.md#customconfig). Ad esempio, il comando seguente crea una directory denominata `custom-bdc` che contiene due file di configurazione della distribuzione JSON, `bdc.json` e `control.json`, in base alla configurazione `aks-dev-test` predefinita:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom-bdc
   ```

## <a name="change-default-docker-registry-repository-and-images-tag"></a><a id="docker"></a> Modificare il registro Docker predefinito, il repository e il tag images

I file di configurazione predefiniti, in particolare control.json, includono una sezione `docker` in cui vengono immessi automaticamente il registro contenitori, il repository e il tag images. Per impostazione predefinita, le immagini necessarie per i cluster Big Data si trovano nel registro contenitori di Microsoft (`mcr.microsoft.com`) nel repository `mssql/bdc`:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "Cluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "docker": {
            "registry": "mcr.microsoft.com",
            "repository": "mssql/bdc",
            "imageTag": "2019-GDR1-ubuntu-16.04",
            "imagePullPolicy": "Always"
        },
        ...
    }
}
```

Prima della distribuzione, è possibile personalizzare le impostazioni di `docker` modificando direttamente il file di configurazione `control.json` o tramite comandi `azdata bdc config`. Ad esempio, i comandi seguenti aggiornano un file di configurazione control.json di `custom-bdc` con `<registry>`, `<repository>` e `<image_tag>` diversi:

```bash
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.registry=<registry>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.repository=<repository>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.imageTag=<image_tag>"
```

> [!TIP]
> Come procedura consigliata, è necessario usare un tag di immagine specifico della versione ed evitare di usare il tag `latest`, che può causare una mancata corrispondenza della versione e conseguenti problemi di integrità del cluster.

> [!TIP]
> La distribuzione di cluster Big Data deve avere accesso al registro contenitori e al repository da cui eseguire il pull delle immagini dei contenitori. Se l'ambiente in uso non ha accesso al registro contenitori di Microsoft predefinito, è possibile eseguire un'installazione offline in cui le immagini necessarie vengono inserite prima di tutto in un repository Docker privato. Per altre informazioni sulle installazioni offline, vedere [Eseguire una distribuzione offline di un cluster Big Data di SQL Server](deploy-offline.md). È necessario impostare le [variabili di ambiente](deployment-guidance.md#env) `DOCKER_USERNAME` e `DOCKER_PASSWORD` prima di eseguire la distribuzione per assicurarsi che il flusso di lavoro di distribuzione abbia accesso al repository privato da cui eseguire il pull delle immagini.

## <a name="change-cluster-name"></a><a id="clustername"></a> Modificare il nome del cluster

Il nome del cluster è sia il nome del cluster Big Data sia lo spazio dei nomi Kubernetes che verrà creato durante la distribuzione. Viene specificato nella parte seguente del file di configurazione della distribuzione `bdc.json`:

```json
"metadata": {
    "kind": "BigDataCluster",
    "name": "mssql-cluster"
},
```

Il comando seguente invia una coppia chiave-valore al parametro `--json-values` per modificare il nome del cluster Big Data in `test-cluster`:

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "metadata.name=test-cluster"
```

> [!IMPORTANT]
> Il nome del cluster Big Data deve contenere solo caratteri alfanumerici minuscoli, senza spazi. Tutti gli elementi Kubernetes (contenitori, pod, set con stato, servizi) per il cluster verranno creati in uno spazio dei nomi con lo stesso nome del nome del cluster specificato.

## <a name="update-endpoint-ports"></a><a id="ports"></a> Aggiornare le porte degli endpoint

Gli endpoint per il controller vengono definiti in `control.json`, mentre quelli per il gateway e l'istanza master di SQL Server vengono definiti nelle sezioni corrispondenti di `bdc.json`. La parte seguente del file di configurazione `control.json` mostra le definizioni degli endpoint per il controller:

```json
{
  "endpoints": [
    {
      "name": "Controller",
      "serviceType": "LoadBalancer",
      "port": 30080
    },
    {
      "name": "ServiceProxy",
      "serviceType": "LoadBalancer",
      "port": 30777
    }
  ]
}
```

L'esempio seguente usa il formato JSON inline per modificare la porta per l'endpoint `controller`:

```bash
azdata bdc config replace --config-file custom-bdc/control.json --json-values "$.spec.endpoints[?(@.name==""Controller"")].port=30000"
```

## <a name="configure-scale"></a><a id="replicas"></a> Configurare la scalabilità

Le configurazioni di ogni risorsa, ad esempio il pool di archiviazione, vengono definite nel file di configurazione `bdc.json`. Ad esempio, la parte seguente del file `bdc.json` mostra una definizione della risorsa `storage-0`:

```json
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
                "spark-defaults-conf.spark.driver.memory": "2g",
                "spark-defaults-conf.spark.driver.cores": "1",
                "spark-defaults-conf.spark.executor.instances": "3",
                "spark-defaults-conf.spark.executor.memory": "1536m",
                "spark-defaults-conf.spark.executor.cores": "1",
                "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
            }
        }
    }
}
```

È possibile configurare il numero di istanze in un pool di archiviazione, calcolo e/o dati modificando il valore `replicas` per ogni pool. L'esempio seguente usa il formato JSON inline per modificare questi valori per i pool di archiviazione, calcolo e dati rispettivamente in `10`, `4` e `4`:

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.replicas=10"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.compute-0.spec.replicas=4"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.data-0.spec.replicas=4"
```

> [!NOTE]
> Il numero massimo di istanze convalidate per i pool di calcolo e dati è `8` ciascuno. Non è prevista l'applicazione di questo limite in fase di distribuzione, ma non è consigliabile configurare un valore più elevato nelle distribuzioni di produzione.

## <a name="configure-storage"></a><a id="storage"></a> Configurare l'archiviazione

È anche possibile modificare la classe di archiviazione e le caratteristiche usate per ogni pool. L'esempio seguente assegna una classe di archiviazione personalizzata ai pool di archiviazione e dati e aggiorna le dimensioni dell'attestazione di volume permanente per l'archiviazione dei dati a 500 GB per HDFS (pool di archiviazione) e 100 GB per il master e il pool di dati. 

> [!TIP]
> Per altre informazioni sulla configurazione dell'archiviazione, vedere [Salvataggio permanente dei dati con un cluster Big Data di SQL Server in Kubernetes](concept-data-persistence.md).

Creare prima di tutto un file patch con estensione json come riportato di seguito per modificare le impostazioni *storage*

```json
{
        "patch": [
                {
                        "op": "add",
                        "path": "spec.resources.storage-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "500Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
        {
                        "op": "add",
                        "path": "spec.resources.master.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
                {
                        "op": "add",
                        "path": "spec.resources.data-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                }
        ]
}

```

È quindi possibile usare il comando `azdata bdc config patch` per aggiornare il file di configurazione `bdc.json`.
```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch ./patch.json
```

> [!NOTE]
> Un file di configurazione basato su `kubeadm-dev-test` non include una definizione di archiviazione per ogni pool, ma è possibile usare il processo precedente per aggiungerle, se necessario.

## <a name="configure-storage-pool-without-spark"></a><a id="sparkstorage"></a> Configurare il pool di archiviazione senza Spark

È anche possibile configurare i pool di archiviazione per l'esecuzione senza Spark e creare un pool Spark separato. Questa configurazione permette di ridimensionare la potenza di calcolo di Spark indipendentemente dall'archiviazione. Per informazioni su come configurare il pool Spark, vedere la sezione [Creare un pool Spark](#sparkpool) in questo articolo.

> [!NOTE]
> La distribuzione di un cluster Big Data senza Spark non è supportata. Di conseguenza, è necessario impostare `includeSpark` su `true` oppure creare un [pool Spark](#sparkpool) separato con almeno un'istanza. È anche possibile fare in modo che Spark venga eseguito nel pool di archiviazione (`includeSpark` è `true`) e predisporre un pool Spark separato.

Poiché per impostazione predefinita `includeSpark` per il pool di archiviazione è impostato su true, è necessario modificare il campo `includeSpark` nella configurazione dell'archiviazione per poter apportare modifiche. Il comando seguente mostra come modificare questo valore tramite il formato JSON inline.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.settings.spark.includeSpark=false"
```

## <a name="create-a-spark-pool"></a><a id="sparkpool"></a> Creare un pool Spark

È possibile creare un pool Spark insieme a o al posto di istanze di Spark in esecuzione nel pool di archiviazione. L'esempio seguente mostra come creare un pool Spark con due istanze applicando patch al file di configurazione `bdc.json`. 

Prima di tutto, creare un file `spark-pool-patch.json` come mostrato di seguito:

```json
{
    "patch": [
        {
            "op": "add",
            "path": "spec.resources.spark-0",
            "value": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Spark",
                    "replicas": 2
                }
            }
        },
        {
            "op": "add",
            "path": "spec.services.spark.resources/-",
            "value": "spark-0"
        },
        {
            "op": "add",
            "path": "spec.services.hdfs.resources/-",
            "value": "spark-0"
        }
    ]
}
```

Eseguire quindi il comando `azdata bdc config patch`:

```bash
azdata bdc config patch -c custom-bdc/bdc.json -p spark-pool-patch.json
```

## <a name="configure-pod-placement-using-kubernetes-labels"></a><a id="podplacement"></a> Configurare la posizione dei pod tramite etichette Kubernetes

È possibile controllare la posizione dei pod in nodi Kubernetes che includono risorse specifiche per soddisfare i vari tipi di requisiti relativi ai carichi di lavoro. Tramite etichette Kubernetes è possibile personalizzare i nodi del cluster Kubernetes che verranno usati per la distribuzione di risorse cluster Big Data, nonché limitare i nodi usati per risorse specifiche.
Ad esempio, può essere necessario fare in modo che i pod di risorse del pool di archiviazione siano posizionati in nodi con più capacità di archiviazione o che le istanze master di SQL Server si trovino in nodi con risorse di CPU e memoria più elevate. In questo caso, si creerà prima di tutto un cluster Kubernetes eterogeneo con diversi tipi di hardware e quindi si [assegneranno le etichette dei nodi](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) di conseguenza. Al momento della distribuzione del cluster Big Data, è possibile specificare le stesse etichette a livello di cluster per indicare quali nodi vengono usati per il cluster Big Data tramite l'attributo `clusterLabel` nel file `control.json`. Verranno quindi usate etichette diverse per il posizionamento a livello di host. Queste etichette possono essere specificate nei file di configurazione della distribuzione del cluster Big Data tramite l'attributo `nodeLabel`. Kubernetes assegna i pod nei nodi che corrispondono alle etichette specificate. Le chiavi di etichetta specifiche che devono essere aggiunte ai nodi nel cluster Kubernetes sono `mssql-cluster` (per indicare i nodi usati per il cluster Big Data) e `mssql-resource` (per indicare i nodi specifici in cui vengono posizionati i pod per le varie risorse). I valori di queste etichette possono essere qualsiasi stringa scelta.

> [!NOTE]
> A causa della natura dei pod che eseguono la raccolta di metriche a livello di nodo, i pod `metricsdc` vengono distribuiti in tutti i nodi con l'etichetta `mssql-cluster`, mentre l'etichetta `mssql-resource` non viene applicata a questi pod.

L'esempio seguente mostra come modificare un file di configurazione personalizzato per includere un'etichetta del nodo `bdc` per l'intero cluster Big Data, un'etichetta `bdc-master` per inserire pod dell'istanza master di SQL Server in un nodo specifico, `bdc-storage-pool` per le risorse del pool di archiviazione, `bdc-compute-pool` per i pod del pool di calcolo e del pool di dati e `bdc-shared` per le risorse rimanenti. 

Aggiungere le etichette prima di tutto per i nodi Kubernetes:

```bash
kubectl label node <kubernetesNodeName1> mssql-cluster=bdc mssql-resource=bdc-shared --overwrite=true
kubectl label node <kubernetesNodeName2> mssql-cluster=bdc mssql-resource=bdc-master --overwrite=true
kubectl label node <kubernetesNodeName3> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName4> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName5> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName6> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName7> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName8> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
```

Aggiornare quindi i file di configurazione della distribuzione del cluster in modo da includere i valori delle etichette. Questo esempio presuppone che i file di configurazione vengano personalizzati in un profilo `custom-bdc`. Per impostazione predefinita, nelle configurazioni predefinite non sono presenti chiavi `nodeLabel` e `clusterLabel` e di conseguenza è necessario modificare un file di configurazione personalizzato manualmente oppure usare i comandi `azdata bdc config add` per apportare le modifiche necessarie.

```bash
azdata bdc config add -c custom-bdc/control.json -j "$.spec.clusterLabel=bdc"
azdata bdc config add -c custom-bdc/control.json -j "$.spec.nodeLabel=bdc-shared"

azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.master.spec.nodeLabel=bdc-master"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.compute-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.data-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.storage-0.spec.nodeLabel=bdc-storage-pool"

# below can be omitted in which case we will take the node label default from the control.json
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.nmnode-0.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.sparkhead.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.zookeeper.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.gateway.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.appproxy.spec.nodeLabel=bdc-shared"
```
>[!NOTE]
> La procedura consigliata prevede di non assegnare al nodo master Kubernetes uno dei ruoli BDC precedenti. Se si prevede comunque di assegnare uno di questi ruoli al nodo master Kubernetes, sarà necessario [rimuoverne il taint ``master:NoSchedule``.](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) Questa operazione potrebbe tuttavia sovraccaricare il nodo master e impedirne la capacità di eseguire attività di gestione Kubernetes su cluster più grandi. È normale vedere alcuni pod pianificati sul master in qualsiasi distribuzione: tollerano già il taint ``master:NoSchedule`` e vengono usati principalmente per semplificare la gestione del cluster. 

## <a name="other-customizations-using-json-patch-files"></a><a id="jsonpatch"></a> Altre personalizzazioni con file di patch JSON

I file di patch JSON configurano più impostazioni contemporaneamente. Per altre informazioni sulle patch JSON, vedere [Patch JSON in Python](https://github.com/stefankoegl/python-json-patch) e [JSONPath Online Evaluator](https://jsonpath.com/).

I file `patch.json` seguenti eseguono queste modifiche:

- Aggiornano la porta di un singolo endpoint in `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.endpoints[?(@.name=='Controller')].port",
      "value": 30000
    }
  ]
}
```

- Aggiornano tutti gli endpoint (`port` e `serviceType`) in `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.endpoints",
      "value": [
        {
          "serviceType": "LoadBalancer",
          "port": 30001,
          "name": "Controller"
        },
        {
          "serviceType": "LoadBalancer",
          "port": 30778,
          "name": "ServiceProxy"
        }
      ]
    }
  ]
}
```

- Aggiornano le impostazioni di archiviazione del controller in `control.json`. Queste impostazioni sono applicabili a tutti i componenti del cluster, a meno che non vengano sostituite a livello di pool.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage",
      "value": {
        "data": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "100Gi"
        },
        "logs": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "32Gi"
        }
      }
    }
  ]
}
```

- Aggiornano il nome della classe di archiviazione in `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage.data.className",
      "value": "managed-premium"
    }
  ]
}
```

- Aggiornano le impostazioni di archiviazione per il pool di archiviazione in `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.resources.storage-0.spec",
      "value": {
        "type": "Storage",
        "replicas": 2,
        "storage": {
          "data": {
            "size": "100Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          },
          "logs": {
            "size": "32Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          }
        }
      }
    }
  ]
}
```

- Aggiornano le impostazioni di Spark per il pool di archiviazione in `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.services.spark.settings",
      "value": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
      }
    }
  ]
}
```

> [!TIP]
> Per altre informazioni sulla struttura e sulle opzioni per modificare un file di configurazione della distribuzione, vedere [Informazioni di riferimento sul file di configurazione della distribuzione per cluster Big Data](reference-deployment-config.md).

Usare i comandi `azdata bdc config` per applicare le modifiche nel file di patch JSON. L'esempio seguente applica il file `patch.json` a un file di configurazione della distribuzione di destinazione `custom-bdc/bdc.json`.

```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch-file ./patch.json
```

## <a name="disable-elasticsearch-to-run-in-privileged-mode"></a>Disabilitare ElasticSearch per l'esecuzione in modalità privilegiata

Per impostazione predefinita, il contenitore ElasticSearch viene eseguito in modalità privilegiata nel cluster Big Data. Questa impostazione garantisce che in fase di inizializzazione del contenitore il contenitore abbia autorizzazioni sufficienti per aggiornare un'impostazione nell'host necessaria quando ElasticSearch elabora un numero maggiore di log. Altre informazioni su questo argomento sono disponibili in [questo articolo](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html). 

Per disabilitare l'esecuzione in modalità privilegiata del contenitore che esegue ElasticSearch, è necessario aggiornare la sezione `settings` nel file `control.json` e impostare il valore di `vm.max_map_count` su `-1`. Ecco un esempio del possibile aspetto di questa sezione:

```json
{
    "apiVersion": "v1",
    "metadata": {...},
    "spec": {
        "docker": {...},
        "storage": {...},
        "endpoints": [...],
        "settings": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
            }
        }
    }
}
```

È possibile modificare manualmente il file `control.json` e aggiungere la sezione precedente a `spec` oppure è possibile creare un file di patch `elasticsearch-patch.json` come quello seguente e usare [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] per applicare patch al file `control.json`:

```json
{
  "patch": [
    {
      "op": "add",
      "path": "spec.settings",
      "value": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
        }
      }
    }
  ]
}
```

Eseguire questo comando per applicare patch al file di configurazione:

```bash
azdata bdc config patch --config-file custom-bdc/control.json --patch-file elasticsearch-patch.json
```

> [!IMPORTANT]
> Come procedura consigliata, aggiornare manualmente l'impostazione di `max_map_count` in ogni host del cluster Kubernetes in base alle istruzioni contenute in [questo articolo](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).

## <a name="turn-pods-and-nodes-metrics-collection-onoff"></a>Attivare/disattivare la raccolta di metriche di pod e nodi

SQL Server 2019 CU5 ha abilitato due opzioni di funzionalità per controllare la raccolta di metriche di pod e nodi. Se si usano soluzioni diverse per il monitoraggio dell'infrastruttura Kubernetes, è possibile disattivare la raccolta predefinita di metriche per pod e nodi dell'host impostando *allowNodeMetricsCollection* e *allowPodMetricsCollection* su *false* nel file di configurazione della distribuzione *control.json*. Per gli ambienti OpenShift, queste impostazioni sono impostate su *false* per impostazione predefinita nei profili di distribuzione predefiniti, perché la raccolta di metriche di pod e nodi richiede capacità con privilegi.
Eseguire questo comando per aggiornare i valori di queste impostazioni nel file di configurazione personalizzato usando l'interfaccia della riga di comando di *azdata*:

```bash
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowNodeMetricsCollection=false"
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowPodMetricsCollection=false"
 ```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'uso di file di configurazione in distribuzioni di cluster Big Data, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md#configfile).