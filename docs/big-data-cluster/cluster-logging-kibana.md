---
title: Estrarre i log del cluster con il dashboard Kibana
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio del cluster con il dashboard Kibana nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 75a7d58a25e7027450b53d1ce387635847bfdfe2
ms.sourcegitcommit: 554497d604e0c63c055bf6d572d92fdadb027dbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107571323"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Estrarre i log del cluster con il dashboard Kibana

Questo articolo descrive come monitorare un'applicazione all'interno di [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] .

## <a name="prerequisites"></a>Prerequisiti

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [Utilità della riga di comando azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capabilities

In [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] è possibile creare, eliminare, descrivere, inizializzare, elencare l'esecuzione e aggiornare l'applicazione. La tabella seguente descrive i comandi per la distribuzione di applicazioni che è possibile usare con **azdata**.

|Comando |Descrizione |
|:---|:---|
|`azdata bdc endpoint list` | Elenca gli endpoint per [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] . |


È possibile usare l'esempio seguente per visualizzare l'endpoint del **dashboard Kibana**:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

L'output indicherà l'endpoint che è possibile usare, il nome utente e la password del cluster per eseguire l'accesso. 

![Dashboard Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Collegamento a un dashboard Kibana:

![Dashboard Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Il browser Microsoft Edge precedente non è compatibile con Kibana, è necessario usare il browser basato su chromium edge per visualizzare correttamente il dashboard. Verrà visualizzata una pagina vuota quando si caricano i dashboard usando un browser non supportato. Vedere [browser supportati per Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).


