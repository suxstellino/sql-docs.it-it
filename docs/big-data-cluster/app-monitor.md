---
title: Monitorare le applicazioni con azdata e il dashboard Grafana
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio di applicazioni con azdata e Grafana nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 380a6c8ab8812ee6a8aae132cbc12125777e2960
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047791"
---
# <a name="monitor-applications-with-azdata-and-grafana-dashboard"></a>Monitorare le applicazioni con azdata e il dashboard Grafana

Grafana è uno dei migliori strumenti di virtualizzazione nativa del cloud e può essere usato per offrire diverse metriche di monitoraggio dell'applicazione in esecuzione in Kubernetes.  

Questo articolo descrive come monitorare un'applicazione in un cluster Big Data di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- [Cluster Big Data di SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capabilities

In SQL Server 2019 è possibile creare, eliminare, descrivere, inizializzare, elencare, eseguire e aggiornare l'applicazione. La tabella seguente descrive i comandi per la distribuzione di applicazioni che è possibile usare con **azdata**.

|Comando |Descrizione |
|:---|:---|
|`azdata bdc endpoint list` | Elenca gli endpoint per il cluster di Big Data. |


È possibile usare l'esempio seguente per elencare l'endpoint del **dashboard Grafana**:

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

L'output indicherà l'endpoint che è possibile usare, il nome utente e la password del cluster per eseguire l'accesso. 

![Dashboard Grafana](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)


Quando si apre il dashboard, passare a  **Host Apps Metrics** (Metriche app host) in cui è possibile ottenere informazioni approfondite sull'applicazione e tenerla monitorata.  

![Metriche dell app host](media/big-data-cluster-monitor-apps/host-apps-metrics.png)


Per ottenere altre informazioni su un singolo pod dell'applicazione (in alcuni casi si hanno più copie dell'applicazione), passare a  **Host Apps Metrics**  (Metriche app host) e scegliere il pod. Si otterrà una visualizzazione delle metriche come di seguito riportata:  

![Metriche dei pod host](media/big-data-cluster-monitor-apps/host-pods-metrics.png) 


## <a name="next-steps"></a>Passaggi successivi

È possibile fare riferimento ad altri esempi in [Esempi di distribuzione di app](https://aka.ms/sql-app-deploy).

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).