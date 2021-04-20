---
title: Monitorare il cluster con il dashboard Grafana
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio del cluster con il dashboard Grafana nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/16/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: cb1a8acdf83f324dc4295869172b74bcdb737537
ms.sourcegitcommit: 708b3131db2e542b1d461d17dec30d673fd5f0fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107729081"
---
# <a name="monitor-cluster-with-azdata-and-grafana-dashboard"></a>Monitorare il cluster con azdata e il dashboard Grafana

Questo articolo descrive come monitorare un'applicazione all'interno di [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] . SQL Server cluster Big Data espone il dashboard di Grafana per il monitoraggio, tali metriche vengono archiviate in influxDB. Queste metriche sono classificate come una delle seguenti: 
- Metriche correlate all'host Kubernetes raccolte da Telegraf, un agente per la raccolta, l'elaborazione, l'aggregazione e la scrittura di metriche.
- Metriche correlate al carico di lavoro: le metriche correlate a SQL Server, Spark e HDFS vengono raccolte da CollectD, tra cui metriche DMV SQL Server ed eventi estesi di [SQL Server (XEvent).](../relational-databases/extended-events/extended-events.md) 

## <a name="available-metrics"></a>Metriche disponibili 

Le metriche seguenti sono disponibili in [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] :

|Categorie |Descrizione | Metriche |
|---|---|---|
|Metriche dei nodi ospitati|Metriche correlate all'host Kubernetes | CPU, utilizzo della RAM, operazioni di I/O al secondo del disco, medie di carico e così via.   |
|Metriche di pod e contenitori|Metriche correlate a pod e contenitori Kubernetes, Grafana consente di filtrare tali metriche in base ai pod o persino a un contenitore specifico. | Utilizzo di CPU, RAM, disco e rete.   |
|SQL Server metriche|Metriche correlate ai SQL Server | Transazioni/sec, Richieste batch/sec, Attività database, attività SQL Server e così via. In particolare, quando ContainerAG è abilitato, è anche possibile monitorare alwaysOn da qui.   |
|Metriche di Spark |Metriche correlate alle app Spark. | Scritture hdfs dell'executor, tempo GC di JVM, utilizzo dell'heap JVM e così via.   |
|Metriche delle app|Le metriche correlate alle app [distribuite in](concept-application-deployment.md) , Grafana consentono di filtrare tali metriche in base a app e [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] versione dell'app specifiche. | Stato delle richieste CPU, RAM e HTTP.   |

## <a name="prerequisites"></a>Prerequisiti

- [SQL Server 2019 Big Data Cluster](deployment-guidance.md)
- [Utilità della riga di comando azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capabilities

In SQL Server 2019 è possibile creare, eliminare, descrivere, inizializzare, elencare, eseguire e aggiornare l'applicazione. La tabella seguente descrive i comandi per la distribuzione di applicazioni che è possibile usare con **azdata**.

|Comando |Descrizione |
|:---|:---|
|`azdata bdc endpoint list` | Elenca gli endpoint per il cluster Big Data. |


È possibile usare l'esempio seguente per elencare l'endpoint del **dashboard Grafana**:

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

L'output indicherà l'endpoint che è possibile usare, il nome utente e la password del cluster per eseguire l'accesso. 

![Dashboard Grafana](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)

I valori `nodeMetricsUrl` e `sqlMetricsUrl` forniscono un collegamento a un dashboard Grafana per il monitoraggio delle metriche dei nodi Kubernetes e delle metriche dei servizi dei cluster Big Data:

![Dashboard Grafana](./media/view-cluster-status/grafana-dashboard.png)

![SQL](./media/view-cluster-status/grafana-sql-status.png)



## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).