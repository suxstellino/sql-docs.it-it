---
title: Monitorare il cluster con Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio del cluster con Azure Data Studio in un cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d03dc9c26f33fd512e035ab3cc9e167e1d9ef7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047741"
---
# <a name="monitor-cluster-status-with-azure-data-studio"></a>Monitorare lo stato del cluster con Azure Data Studio

Questo articolo illustra come visualizzare lo stato di un cluster Big Data usando Azure Data Studio.

## <a name="use-azure-data-studio"></a><a id="datastudio"></a> Usare Azure Data Studio

Dopo aver scaricato la versione più recente della **build Insider** di [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), e possibile visualizzare gli endpoint di servizio e lo stato di un cluster Big Data con il dashboard dei cluster Big Data di SQL Server. Alcune delle funzionalità seguenti sono inizialmente disponibili solo per la build Insider di Azure Data Studio.

1. Creare prima di tutto una connessione al cluster Big Data in Azure Data Studio. Per altre informazioni, vedere [Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md).

1. Fare clic con il pulsante destro del mouse sull'endpoint del cluster Big Data e scegliere **Gestisci**.

   ![Comando Gestisci del menu di scelta rapida](media/view-cluster-status/right-click-manage.png)

1. Selezionare la scheda **SQL Server Big Data Cluster** (Cluster Big Data di SQL Server) per accedere al dashboard dei cluster Big Data.

   ![Dashboard dei cluster Big Data](media/view-cluster-status/bdc-dashboard.png)

### <a name="service-endpoints"></a>Endpoint di servizio

È importante poter accedere facilmente ai vari servizi all'interno di un cluster Big Data. Il dashboard dei cluster Big Data fornisce una tabella degli endpoint di servizio che consente di visualizzare e copiare gli endpoint.

![endpoint di servizio](media/view-cluster-status/service-endpoints.png)

Questi servizi elencano gli endpoint che è possibile copiare e incollare quando è necessario per la connessione ai servizi. È ad esempio possibile fare clic sull'icona di copia a destra dell'endpoint e quindi incollare quanto copiato in una finestra di testo che richiede tale endpoint. L'endpoint del servizio di gestione cluster è necessario per eseguire il [notebook relativo allo stato del cluster](#notebook).

### <a name="dashboards"></a>Dashboard

La tabella degli endpoint di servizio espone anche diversi dashboard per il monitoraggio:

- Metriche (Grafana)
- Log (Kibana)
- Monitoraggio di processi Spark
- Gestione di risorse di Spark

È possibile fare clic direttamente su questi collegamenti. Verrà richiesto di eseguire l'autenticazione quando si accede a questi dashboard. Per i dashboard delle metriche e dei log, fornire le credenziali di amministratore del controller impostate in fase di distribuzione usando le variabili di ambiente **AZDATA_USERNAME** e **AZDATA_PASSWORD**. I dashboard Spark useranno le credenziali del gateway (Knox), ovvero l'identità di Active Directory in un cluster integrato con AD o **AZDATA_USERNAME** e **AZDATA_PASSWORD** se si usa l'autenticazione di base nel cluster.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

### <a name="cluster-status-notebook"></a><a id="notebook"></a> Notebook relativo allo stato del cluster

1. È anche possibile visualizzare lo stato del cluster Big Data avviando il notebook relativo allo stato del cluster. Per avviare il notebook, fare clic sull'attività **Stato cluster**.

    ![avvio](media/view-cluster-status/cluster-status-launch.png)

2. Prima di iniziare, è necessario disporre di quanto segue:

    - Nome del cluster Big Data
    - Nome utente del controller
    - Password del controller
    - Endpoint controller

    Il nome predefinito del cluster Big Data è **mssql-cluster**, a meno che non sia stato personalizzato durante la distribuzione. È possibile trovare l'endpoint controller nel dashboard dei cluster Big Data nella tabella relativa agli endpoint di servizio. L'endpoint è elencato come **Cluster Management Service** (Servizio di gestione cluster). Se non si conoscono le credenziali, rivolgersi all'amministratore che ha distribuito il cluster.

3. Fare clic su **Run Cells** (Esegui celle) sulla barra degli strumenti superiore.

4. Seguire le indicazioni di richiesta delle credenziali. Premere INVIO dopo aver digitato ogni credenziale per il nome del cluster Big Data, il nome utente del controller e la password del controller.

    > [!Note]
    > Se non è disponibile un file di configurazione con i Big Data, verrà richiesto l'endpoint controller. Digitare o incollare l'endpoint e quindi premere INVIO per continuare.

5. Se la connessione viene stabilita correttamente, nel resto del notebook verrà visualizzato l'output di ogni componente del cluster Big Data. Quando si vuole eseguire di nuovo una determinata cella di codice, passare il puntatore sulla cella di codice e fare clic sull'icona **Esegui**.


## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).