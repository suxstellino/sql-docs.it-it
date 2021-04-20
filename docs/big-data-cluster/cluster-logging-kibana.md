---
title: Estrarre i log del cluster con il dashboard Kibana
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio del cluster con il dashboard Kibana nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/16/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 80fe3abcb2656dd2202b42ca7d67362a813d8044
ms.sourcegitcommit: 708b3131db2e542b1d461d17dec30d673fd5f0fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107729091"
---
# <a name="check-out-cluster-logs-with-kibana-dashboard"></a>Estrarre i log del cluster con il dashboard Kibana

Questo articolo descrive come visualizzare i log di un'applicazione all'interno di [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] . [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] usare Fluent Bit, un processore di log e un server d'inoltro open source. Fluent Bit recupera i log dai componenti del cluster Big Data nel cluster e li archivia in [Elastic Stack Elasticsearch.](https://azure.microsoft.com/overview/linux-on-azure/elastic/) Da Kibana Dashboard è possibile visualizzare ed eseguire ricerche nel log di interesse.

## <a name="logs-stored-in-elasticsearch"></a>Log archiviati in Elasticsearch

I log correlati al cluster Big Data archiviati in Elasticsearch includono l'output standard e i log degli errori di tutti i servizi, inclusi i servizi SQL Server, Spark, HDFS e della piattaforma. 

Tali log possono essere cercati in base ai componenti di Kibana Dashboard. È possibile usare filtri come "kubernetes_container_name", "kubernetes_pod_name", "log_filename" e "service_name" per visualizzare rapidamente tutti i log, ad esempio i log dal controller di cluster Big Data, da SQL Server o da qualsiasi log di pod, servizi e altro ancora. 

In particolare, il log del controller registra lo stato e il processo delle distribuzioni del cluster e degli eventi del cluster filtrando "service_name: controller". Per chi cerca in modalità AD, il log di supporto per la sicurezza può risultare molto utile, registra gli eventi durante il processo in cui il cluster Big Data ottiene i token di Ad dal controller di dominio Active Directory locale(AD), è possibile accedervi filtrando [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] "service_name: secsupp" nel log del controller.


## <a name="prerequisites"></a>Prerequisiti

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [Utilità della riga di comando azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capabilities

In [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] è possibile creare, eliminare, descrivere, inizializzare, elencare l'esecuzione e aggiornare l'applicazione. La tabella seguente descrive i comandi per la distribuzione di applicazioni che è possibile usare con **azdata**.

|Comando |Descrizione |
|:---|:---|
|`azdata bdc endpoint list` | Elenca gli endpoint per [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] . |


È possibile usare l'esempio seguente per elencare l'endpoint **del dashboard Kibana:**

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

L'output indicherà l'endpoint che è possibile usare, il nome utente e la password del cluster per eseguire l'accesso. 

![Dashboard Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Collegamento a un dashboard Kibana:

![Dashboard Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Il browser Microsoft Edge precedente non è compatibile con Kibana, è necessario usare il browser edge basato su chromium per visualizzare correttamente il dashboard. Quando si caricano i dashboard usando un browser non supportato, verrà visualizzata una pagina vuota. Vedere [browser supportati per Kibana.](https://www.elastic.co/support/matrix#matrix_browsers)



## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
