---
title: Estrarre i log del cluster con il dashboard Kibana
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio del cluster con il dashboard Kibana nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 55dc3056b9f66f7a96b55ab750a5f74fe9bfd394
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091711"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Estrarre i log del cluster con il dashboard Kibana

Questo articolo descrive come monitorare un'applicazione in un cluster Big Data di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- [Cluster Big Data di SQL Server 2019](deployment-guidance.md)
- [Utilità della riga di comando azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capabilities

In SQL Server 2019 è possibile creare, eliminare, descrivere, inizializzare, elencare, eseguire e aggiornare l'applicazione. La tabella seguente descrive i comandi per la distribuzione di applicazioni che è possibile usare con **azdata**.

|Comando |Descrizione |
|:---|:---|
|`azdata bdc endpoint list` | Elenca gli endpoint per il cluster di Big Data. |


È possibile usare l'esempio seguente per visualizzare l'endpoint del **dashboard Kibana**:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

L'output indicherà l'endpoint che è possibile usare, il nome utente e la password del cluster per eseguire l'accesso. 

![Dashboard Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Collegamento a un dashboard Kibana:

![Dashboard Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Il browser Microsoft Edge precedente non è compatibile con Kibana. Per visualizzare il dashboard correttamente, è necessario usare un browser basato su Chromium. Quando si caricano i dashboard usando un browser non supportato, viene visualizzata una pagina vuota. Vedere qui per i browser supportati per Kibana.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).