---
title: Introduzione
titleSuffix: SQL Server Big Data Clusters
description: Informazioni sulle risorse e sui passaggi necessari per la distribuzione di cluster Big Data di SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1a6a064ad4a6077bffcd1642569d63ee0e086a7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038834"
---
# <a name="get-started-with-big-data-clusters-2019-deployment"></a>Introduzione alla distribuzione di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo offre una panoramica della distribuzione di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]. L'articolo offre un'introduzione ai concetti e presenta un framework utile alla comprensione degli scenari di distribuzione. I passaggi di distribuzione specifici variano in base alla piattaforma scelta per il client e il server. Per informazioni introduttive su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)

Per altri scenari di distribuzione di SQL Server, vedere:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Contenitori Docker](../linux/sql-server-linux-docker-container-deployment.md)

## <a name="quick-introduction"></a>Breve introduzione 

Guardare questo video di 9 minuti per una panoramica della distribuzione di cluster Big Data:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Clusters-deployment-overview/player?WT.mc_id=dataexposed-c9-niner]


> [!TIP]
> Per ottenere rapidamente un ambiente in cui siano già distribuiti Kubernetes e un cluster Big Data e rendere più semplice l'apprendimento delle relative funzionalità, usare uno degli script di esempio indicati nella [sezione degli script](#scripts). Dopo la distribuzione, per gestire il cluster usare gli [strumenti client](#tools) nella sezione seguente.


## <a name="client-tools"></a><a id="tools"></a> Strumenti client

I cluster Big data richiedono un set specifico di strumenti client. Prima di distribuire un cluster Big Data in Kubernetes, è necessario installare gli strumenti richiesti per la distribuzione. Per scenari diversi sono necessari strumenti specifici. Ogni articolo descriverà gli strumenti necessari come prerequisiti per l'esecuzione di un'attività specifica. Per un elenco completo degli strumenti e dei collegamenti di installazione, vedere [Installare strumenti per Big Data di SQL Server 2019](deploy-big-data-tools.md).

## <a name="kubernetes"></a>Kubernetes

I cluster Big Data vengono distribuiti come serie di contenitori intercorrelati gestiti in [Kubernetes](https://kubernetes.io/docs/home). È possibile ospitare Kubernetes in diversi modi. Anche se esiste già un ambiente Kubernetes, è necessario esaminare i requisiti correlati per i cluster Big Data.

- **Servizio Azure Kubernetes**: il servizio Azure Kubernetes permette di distribuire un cluster Kubernetes gestito in Azure. È possibile gestire solo i nodi agente. Con il servizio Azure Kubernetes non è necessario effettuare il provisioning del proprio hardware per il cluster. È facile usare anche uno [script Python](quickstart-big-data-cluster-deploy.md) o un [notebook di distribuzione](notebooks-deploy.md) per creare il cluster del servizio Azure Kubernetes e distribuire il cluster Big Data in un unico passaggio. Per altre informazioni sulla configurazione del servizio Azure Kubernetes per la distribuzione di un cluster Big Data, vedere [Configurare il servizio Azure Kubernetes per distribuzioni di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](deploy-on-aks.md).

- **Azure Red Hat OpenShift (ARO)** : consente di distribuire un cluster Red Hat OpenShift gestito in Azure. È possibile gestire solo i nodi agente. Con Azure Red Hat OpenShift non è necessario effettuare il provisioning del proprio hardware per il cluster. È facile usare anche uno [script Python](quickstart-big-data-cluster-deploy-aro.md) per creare il cluster Azure Red Hat OpenShift e distribuire il cluster Big Data in un unico passaggio. Questo modello di distribuzione è stato introdotto in SQL Server 2019 CU5. 

- **Più computer**: è possibile distribuire Kubernetes anche in più computer Linux, che possono essere server fisici o macchine virtuali. È possibile usare lo strumento [kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) per creare il cluster Kubernetes. È possibile usare uno [script Bash](deployment-script-single-node-kubeadm.md) per automatizzare questo tipo di distribuzione. Questo metodo funziona correttamente se è già presente un'infrastruttura che si vuole usare per il cluster Big Data. Per altre informazioni sull'uso di distribuzioni **kubeadm** con cluster Big Data, vedere [Configurare Kubernetes in più computer per distribuzioni di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](deploy-with-kubeadm.md).

- **Red Hat OpenShift**: è possibile eseguire la distribuzione nel proprio cluster Red Hat OpenShift. Per informazioni, vedere [Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in OpenShift in locale e in Azure Red Hat OpenShift](deploy-openshift.md). Questo modello di distribuzione è stato introdotto in SQL Server 2019 CU5.

## <a name="deploy-a-big-data-cluster"></a>Distribuire un cluster Big Data

Dopo aver configurato Kubernetes, è possibile distribuire un cluster Big Data con il comando `azdata bdc create`. Per la distribuzione è possibile adottare diversi approcci.

- Se si esegue la distribuzione in un ambiente di sviluppo e test, è possibile scegliere di usare una delle [configurazioni predefinite](deployment-guidance.md#deploy) fornite da **azdata**.

- Per personalizzare la distribuzione, è possibile creare e usare [file di configurazione della distribuzione](deployment-guidance.md#configfile) propri.

- Per un'installazione completamente automatica, è possibile passare tutte le altre impostazioni in variabili di ambiente. Per altre informazioni, vedere [Distribuzioni automatiche](deployment-guidance.md#unattended).


## <a name="deployment-scripts"></a><a id="scripts"></a> Script di distribuzione

Gli script di distribuzione permettono di distribuire cluster Kubernetes e Big Data in un unico passaggio. Spesso forniscono anche valori predefiniti per le impostazioni dei cluster Big Data. È possibile personalizzare qualsiasi script di distribuzione creando la propria versione, per configurare la distribuzione del cluster Big Data in modo diverso.

Attualmente sono disponibili gli script di distribuzione seguenti:

- [Script Python: distribuire un cluster Big Data nel servizio Azure Kubernetes](quickstart-big-data-cluster-deploy.md)
- [Script Bash: distribuire un cluster Big Data in un cluster kubeadm a nodo singolo](deployment-script-single-node-kubeadm.md)

## <a name="deployment-notebooks"></a>Notebook di distribuzione

È possibile distribuire un cluster Big Data anche eseguendo un notebook di Azure Data Studio. Per altre informazioni su come usare un notebook per la distribuzione nel servizio Azure Kubernetes, vedere l'articolo seguente:

- [Distribuire un cluster Big Data con notebook di Azure Data Studio](notebooks-deploy.md).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver distribuito correttamente un cluster Big Data, [connettersi al cluster](connect-to-big-data-cluster.md) e valutare se [caricare dati di esempio](tutorial-load-sample-data.md) da usare con diverse procedure dettagliate.