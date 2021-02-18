---
title: Panoramica della configurazione post-distribuzione di cluster di Big Data SQL Server
titleSuffix: SQL Server big data clusters
description: Panoramica della configurazione post-distribuzione di cluster di Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 478ecc9888bbd3c8f51ee96c6c796856472f93d5
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343910"
---
# <a name="how-to-configure-bdc-settings-post-deployment"></a>Come configurare le impostazioni di integrazione applicativa dei dati post distribuzione

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> La configurazione delle impostazioni post-distribuzione è disponibile solo nelle distribuzioni BDC CU9 e versioni successive. La configurazione delle impostazioni non include la configurazione di scalabilità, archiviazione o endpoint. Le opzioni e le istruzioni per configurare l'integrazione applicativa dei dati prima di CU9 sono disponibili [qui](configure-bdc-pre-configuration.md).

Le impostazioni relative a cluster, servizio e ambito delle risorse per i cluster di Big data possono essere configurate dopo la distribuzione tramite l'interfaccia della riga di comando di azdata. Questa funzionalità consente agli amministratori di integrazione applicativa dei dati di modificare le configurazioni per soddisfare sempre i requisiti del carico Questo articolo illustra i passaggi di esempio per configurare un catalogo dati business per soddisfare i requisiti del carico di lavoro Spark. La funzionalità di configurazione post-distribuzione segue un set, diff, Apply Flow.

## <a name="step-by-step-configure-bdc-to-meet-your-spark-workload-requirements"></a>Procedura dettagliata: configurare l'integrazione applicativa dei dati per soddisfare i requisiti del carico di lavoro Spark

### <a name="view-the-current-configurations-of-the-big-data-cluster-spark-service"></a>Visualizzare le configurazioni correnti del servizio Spark del cluster di Big Data
Nell'esempio seguente viene illustrato come visualizzare le impostazioni configurate dall'utente del servizio Spark. È possibile visualizzare tutte le impostazioni configurabili, gestite dal sistema e tutte le impostazioni configurabili e le impostazioni in sospeso tramite i parametri facoltativi. Per ulteriori informazioni, vedere l' [ `azdata bdc spark` istruzione](../azdata/reference/reference-azdata-bdc-spark-statement.md) .

```bash
azdata bdc spark settings show
```
#### <a name="sample-output"></a>Output di esempio
Servizio Spark 

|Impostazione|Valore corrente|
| --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1` |
|`spark-defaults-conf.spark.driver.memory`|`1664m` |

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-driver-across-all-resources-with-spark-ie-for-the-spark-service"></a>Modificare il numero predefinito di core e la memoria per il driver Spark in tutte le risorse con Spark, ad esempio per il servizio Spark
Aggiornare il numero predefinito di core a 2 e la memoria predefinita a 7424m per il servizio Spark.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.driver.cores=2, spark-defaults-conf.spark.driver.memory=7424m
```

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-executors-in-the-storage-pool"></a>Modificare il numero predefinito di core e la memoria per gli executor Spark nel pool di archiviazione
Aggiornare il numero predefinito di core dell'executor a 4 per il pool di archiviazione.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.executor.cores=4 --resource=storage-0
```

### <a name="view-the-pending-settings-changes-staged-in-the-bdc"></a>Visualizzare le modifiche apportate alle impostazioni in sospeso nell'integrazione applicativa dei dati
Visualizzare le modifiche delle impostazioni in sospeso solo per il servizio Spark e nell'intero cluster di integrazione applicativa dei dati.

#### <a name="pending-spark-service-settings"></a>Impostazioni del servizio Spark in sospeso
```bash
azdata bdc spark settings show --filter-option=pending --include-details
```

### <a name="spark-service"></a>Servizio Spark

|Impostazione|Valore corrente|Valore configurato|Configurabile|Configurato |Ora ultimo aggiornamento|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

#### <a name="all-pending-settings"></a>Tutte le impostazioni in sospeso
```bash
azdata bdc settings show --filter-option=pending --include-details --recursive
```

Impostazioni del servizio Spark-in sospeso

|Impostazione|Valore corrente|Valore configurato|Configurabile|Configurato|Ora ultimo aggiornamento|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

Archiviazione-0 impostazioni Spark risorsa-in sospeso

|Impostazione|Valore corrente|Valore configurato|Configurabile|Configurato|Ora ultimo aggiornamento|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.executor.cores`|`1`| `4` | `true` | `true` |

### <a name="apply-the-pending-settings-to-the-bdc"></a>Applicare le impostazioni in sospeso a BDC

```bash
azdata bdc settings apply
```

### <a name="monitor-the-status-of-the-bdc-configuration-update"></a>Monitorare lo stato dell'aggiornamento della configurazione di integrazione applicativa dei dati

```bash
azdata bdc status show -all
```

## <a name="optional-steps"></a>Passaggi facoltativi

### <a name="revert-pending-configuration-settings"></a>Ripristina impostazioni di configurazione in sospeso

Se si determina che non si desidera più modificare le impostazioni di configurazione in sospeso, è possibile annullare l'installazione di queste impostazioni. Verranno ripristinate le impostazioni in sospeso in tutti gli ambiti.

```bash
azdata bdc settings revert
```

### <a name="abort-the-configuration-upgrade"></a>Interrompi aggiornamento configurazione

Se l'aggiornamento della configurazione non riesce per uno dei componenti, è possibile annullare il processo di aggiornamento e ripristinare le relative configurazioni precedenti. Le impostazioni preparate per la modifica durante l'aggiornamento verranno nuovamente elencate come impostazioni in sospeso.

```bash
azdata bdc settings cancel-apply
```

## <a name="next-steps"></a>Passaggi successivi

[Configurare un cluster Big Data di SQL Server](configure-bdc-overview.md)