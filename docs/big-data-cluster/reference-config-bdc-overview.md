---
title: Proprietà di configurazione di SQL Server Big Data cluster
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per le proprietà di configurazione per i cluster di Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ecaba704d9c08619f42c5cdf8d726917ccc61b9c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343547"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>Proprietà di configurazione di SQL Server Big Data cluster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

È possibile definire le impostazioni di configurazione dei cluster di Big data negli ambiti seguenti: `cluster` , `service` e `resource` . Anche la gerarchia delle impostazioni segue in questo ordine, dal più alto al più basso. I componenti BDC accetteranno il valore dell'impostazione definito nell'ambito più basso. Se l'impostazione non è definita in un ambito specificato, erediterà il valore dall'ambito padre superiore. Di seguito è riportato un elenco delle impostazioni disponibili per ogni componente di integrazione applicativa dei dati nei vari ambiti. È anche possibile visualizzare le impostazioni configurabili per l'integrazione applicativa dei dati usando azdata.

## <a name="bdc-cluster-scope-settings"></a>Impostazioni dell'ambito del cluster BDC
È possibile configurare le impostazioni seguenti nell'ambito del cluster.

|Proprietà|Opzioni|
| --- | --- |
|`mssql.telemetry`|`customerfeedback = { true | false }` |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="sql-service-scope-settings"></a>Servizio SQL-impostazioni ambito
È possibile configurare le impostazioni seguenti nell'ambito del servizio SQL.

|Proprietà|Opzioni|
| --- | --- |
|`mssql.language`|`lcid = <language_identifier>` |

## <a name="spark-service-scope-settings"></a>Servizio Spark-Impostazioni ambito
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="hdfs-service-scope-settings"></a>Servizio HDFS-impostazioni ambito
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="gateway-service-scope-settings"></a>Servizio gateway-impostazioni ambito
Non è possibile configurare le impostazioni dell'ambito del servizio gateway. Configurare le impostazioni nell'ambito della risorsa del gateway.

## <a name="app-service-scope-settings"></a>Servizio app-Impostazioni ambito
Nessuno disponibile

## <a name="master-pool-resource-scope-settings"></a>Risorse del pool master-impostazioni ambito
|Proprietà|Opzioni|
| --- | --- |
|`mssql.sqlagent`|`enabled = { true | false }` |
|`mssql.licensing`|`pid = { Enterprise | Developer }` |
<!-- |`mssql.collation`|`x = <language_identifier>` | -->

> [!NOTE]
> La modifica delle regole di confronto predefinite per un'istanza di SQL Server è un'operazione complessa. Oltre a modificare l' `mssql.collation` impostazione, potrebbe essere necessario ricreare i database utente e tutti gli oggetti in essi contenuti. Per istruzioni su come eseguire questa operazione, vedere [impostare o modificare le regole di confronto del server](../relational-databases/collations/set-or-change-the-server-collation.md#changing-the-server-collation-in-sql-server)

## <a name="storage-pool-resource-scope-settings"></a>Risorsa pool di archiviazione-impostazioni ambito
Il pool di archiviazione è costituito dai componenti SQL, Spark e HDFS.

### <a name="available-sql-configurations"></a>Configurazioni SQL disponibili
|Proprietà|Opzioni|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.storagePoolCacheSize`| |
|`mssql.storagePoolMaxCacheSize`| |
|`mssql.storagePoolCacheAutogrowth`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |


### <a name="available-apache-spark-and-hadoop-configurations"></a>Configurazioni Apache Spark e Hadoop disponibili
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="data-pool-resource-scope-settings"></a>Risorsa pool di dati-impostazioni ambito
|Proprietà|Opzioni|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="compute-pool-resource-scope-settings"></a>Risorse del pool di calcolo-impostazioni ambito
|Proprietà|Opzioni|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="spark-pool-resource-scope-settings"></a>Risorsa pool Spark-Impostazioni ambito
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="gateway-resource-scope-settings"></a>Impostazioni dell'ambito delle risorse del gateway
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="sparkhead-resource-scope-settings"></a>`Sparkhead` impostazioni dell'ambito delle risorse
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="zookeeper-resource-scope-settings"></a>Impostazioni dell'ambito della risorsa Zookeeper
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="namenode-resource-scope-settings"></a>Impostazioni dell'ambito della risorsa NameNode
Per visualizzare tutte le impostazioni supportate e non supportate, vedere l'articolo relativo alla [configurazione di Apache Spark & Apache Hadoop](reference-config-spark-hadoop.md) .

## <a name="app-proxy-resource-scope-settings"></a>Risorsa proxy app-Impostazioni ambito
Nessuno disponibile

## <a name="next-steps"></a>Passaggi successivi

[Configurare SQL Server cluster di Big Data](configure-bdc-overview.md)