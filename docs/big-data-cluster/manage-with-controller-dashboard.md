---
title: Gestire un cluster Big Data di SQL Server con il dashboard del controller
titleSuffix: Manage SQL Server big data cluster with controller dashboard
description: Usare un notebook di Azure Data Studio per gestire e risolvere problemi relativi a un cluster Big Data.
author: yualan
ms.author: alanyu
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e7b71318f55b15df6a758237c26eedb9be5646c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046281"
---
# <a name="manage-big-data-clusters-for-sql-server-controller-dashboard"></a>Gestire cluster Big Data con il dashboard del controller di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Oltre ad **azdata** e al notebook relativo allo stato del cluster, c'è un altro modo per visualizzare lo stato di un cluster Big Data di SQL Server. È ora possibile aggiungere il controller del cluster Big Data di SQL Server tramite il viewlet **Connessioni**. Si disporrà così di un dashboard per visualizzare l'integrità del cluster.

![dashboard](media/manage-with-controller-dashboard/controller-dashboard.png)
## <a name="prerequisites"></a>Prerequisites

Per avviare il notebook, sono necessari i prerequisiti seguenti:

* Versione più recente di [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
* [Estensione di [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] installata in Azure Data Studio](../azure-data-studio/data-virtualization-extension.md)

Oltre ai prerequisiti precedenti, il cluster Big Data di SQL Server 2019 richiede anche:

* **azdata**
    - [Windows Installer](../azdata/install/deploy-install-azdata-installer.md)
    - [Gestione pacchetti Linux](../azdata/install/deploy-install-azdata-linux-package.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli)

## <a name="add-sql-server-big-data-cluster-controller"></a>Aggiungere il controller del cluster Big Data di SQL Server

1. Fare clic sulla vista Connessioni nel riquadro sinistro.
2. Nella parte inferiore della vista Connessioni fare clic su **SQL Server Big Data Clusters** (Cluster Big Data di SQL Server) per espandere la voce.
3. Fare clic su **Add SQL Server big data cluster controller** (Aggiungi controller cluster Big Data di SQL Server). Verrà aperta una nuova finestra di dialogo.

## <a name="add-new-controller"></a>Aggiungere un nuovo controller

1. Aggiungere il nome del cluster.
2. Aggiungere l'URL del servizio di gestione del cluster. L'URL è indicato nella tabella degli endpoint di servizio del dashboard del cluster Big Data oppure è possibile chiederlo all'amministratore del cluster.
3. Aggiungere nome utente e password.

## <a name="launch-controller-dashboard"></a>Avviare il dashboard del controller

1. Dopo essere stato aggiunto, il controller può essere visualizzato in SQL Server Big Data Clusters (Cluster Big Data di SQL Server).
2. Per avviare il dashboard, fare clic con il pulsante destro del mouse sul controller e scegliere **Gestisci**.

## <a name="controller-dashboard"></a>Dashboard del controller

1. Nel dashboard del controller sono presenti tre componenti principali:

    - **Barra degli strumenti** nella parte superiore, che contiene le azioni per il dashboard.
    - **Riquadro di spostamento** a sinistra, che consente di passare tra le diverse viste nel dashboard.
    - **Vista**, che occupa la maggior parte della pagina.

2. Nella pagina **Big data cluster overview** (Panoramica cluster Big Data) sono presenti le informazioni seguenti:

    - **Cluster Properties** (Proprietà cluster), per visualizzare le informazioni sul cluster.
    - **Cluster Overview** (Panoramica cluster), per visualizzare una panoramica generale di tutti i componenti del cluster e di quelli non integri
    - **Endpoint servizio**, per accedere ai diversi servizi all'interno del cluster Big Data di SQL Server o per copiarli.

3. Nel **riquadro di spostamento** è possibile visualizzare l'elenco dei servizi e fare clic su uno di essi per visualizzare dettagli aggiuntivi del cluster. La vista corrisponde a quella aperta quando si fa clic su un servizio in **Cluster Overview** (Panoramica cluster).

4. Ogni vista in **Dettagli cluster** è costituita dagli stessi componenti dell'interfaccia utente:

    - **Health Status Details** (Dettagli stato integrità), che indica lo stato e l'integrità del componente.
    - **Metrics and Logs** (Metriche e log) per visualizzare metriche e log aggiuntivi tramite Grafana e Kibana.

1. Se si nota un componente non integro, fare clic su **Risoluzione dei problemi** sulla barra degli strumenti per avviare un book Jupyter contenente un notebook utile per la diagnosi del problema.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul controller, vedere la [documentazione del controller](concept-controller.md).