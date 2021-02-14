---
title: Configurare il servizio Azure Kubernetes
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come configurare il servizio Azure Kubernetes per le distribuzioni di cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2021cf90230bd22de775cef164336f9e31631991
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038801"
---
# <a name="configure-azure-kubernetes-service-for-sql-server-big-data-cluster-deployments"></a>Configurare il servizio Azure Kubernetes per le distribuzioni di cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive come configurare il servizio Azure Kubernetes per le distribuzioni di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

Il servizio Azure Kubernetes semplifica la creazione, la configurazione e la gestione di un cluster di macchine virtuali preconfigurate con un cluster Kubernetes per l'esecuzione di applicazioni in contenitori. In questo modo è possibile usare le competenze esistenti o fare affidamento sulle vaste esperienze della community, in continua crescita, per distribuire e gestire applicazioni basate su contenitori in Microsoft Azure.

Questo articolo descrive i passaggi per la distribuzione di Kubernetes nel servizio Azure Kubernetes tramite l'interfaccia della riga di comando di Azure. Se non si ha una sottoscrizione di Azure, creare un account gratuito prima di iniziare.

> [!TIP]
> È anche possibile usare uno script per distribuire il servizio Azure Kubernetes e un cluster Big Data in un unico passaggio. Per altre informazioni, vedere come eseguire questa operazione in uno [script Python](quickstart-big-data-cluster-deploy.md) o in un [notebook](notebooks-deploy.md) di Azure Data Studio.

## <a name="prerequisites"></a>Prerequisiti

- [Distribuire gli strumenti per Big Data di SQL Server 2019](deploy-big-data-tools.md):
   - **Kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
   - **Interfaccia della riga di comando di Azure**

- Server Kubernetes versione 1.13 o successiva. Per il servizio Azure Kubernetes, è necessario usare il parametro `--kubernetes-version` per specificare una versione diversa rispetto a quella predefinita.

- Per garantire una distribuzione corretta e un'esperienza ottimale durante la convalida degli scenari di base in AKS, è possibile usare un singolo nodo o un cluster AKS multinodo, con le seguenti risorse disponibili:
   - 8 vCPU in tutti i nodi
   - 64 GB di memoria per macchina virtuale
   - 24 o più dischi collegati in tutti i nodi

   > [!TIP]
   > L'infrastruttura di Azure offre più opzioni di dimensioni per le macchine virtuali. Vedere [qui](/azure/virtual-machines/windows/sizes) per le opzioni disponibili nell'area in cui verrà eseguita la distribuzione.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Un gruppo di risorse di Azure è un gruppo logico in cui le risorse di Azure vengono distribuite e gestite. La procedura seguente illustra come accedere ad Azure e creare un gruppo di risorse per il cluster del servizio Azure Kubernetes.

1. Al prompt dei comandi eseguire il comando seguente e attenersi alle istruzioni per accedere alla sottoscrizione di Azure:

    ```azurecli
    az login
    ```

1. Se si hanno più sottoscrizioni, è possibile visualizzare tutte le sottoscrizioni eseguendo questo comando:

   ```azurecli
   az account list
   ```

1. Se si vuole passare a una sottoscrizione diversa, è possibile eseguire questo comando:

   ```azurecli
   az account set --subscription <subscription id>
   ```

1. Identificare l'area di Azure in cui si vuole distribuire il cluster e le risorse usando questo comando:

   ```azurecli
   az account list-locations -o table
   ```

1. Creare un gruppo di risorse con il comando **az group create**. L'esempio seguente consente di creare un gruppo di risorse denominato `sqlbdcgroup` nell'area `westus2`.

   ```azurecli
   az group create --name sqlbdcgroup --location westus2
   ```

## <a name="verify-available-kubernetes-versions"></a>Verificare le versioni di Kubernetes disponibili

Usare la versione più recente disponibile di Kubernetes. La versione più recente disponibile dipende dalla posizione in cui si distribuisce il cluster. Il comando seguente restituisce le versioni di Kubernetes disponibili in una posizione specifica.

Prima di eseguire il comando, aggiornare lo script. Sostituire `<Azure data center>` con la posizione del cluster.

   **Bash**

   ```bash
   az aks get-versions \
   --location <Azure data center> \
   --query orchestrators \
   --o table
   ```

   **PowerShell**

   ```powershell
   az aks get-versions `
   --location <Azure data center> `
   --query orchestrators `
   --o table
   ```

Scegliere la versione più recente disponibile del cluster. Annotare il numero di versione. Verrà usato nel passaggio successivo.

## <a name="create-a-kubernetes-cluster"></a>Creare un cluster Kubernetes

1. Creare un cluster Kubernetes nel servizio Azure Kubernetes con il comando [az aks create](/cli/azure/aks). L'esempio seguente crea un cluster Kubernetes denominato *kubcluster* con un nodo dell'agente Linux con dimensioni **Standard_L8s**.

   Prima di eseguire lo script, sostituire `<version number>` con il numero di versione identificato nel passaggio precedente.

   Assicurarsi di creare il cluster del servizio Azure Kubernetes nello stesso gruppo di risorse usato nelle sezioni precedenti.

   **Bash:**

   ```bash
   az aks create --name kubcluster \
   --resource-group sqlbdcgroup \
   --generate-ssh-keys \
   --node-vm-size Standard_L8s \
   --node-count 1 \
   --kubernetes-version <version number>
   ```

   **PowerShell:**

   ```powershell
   az aks create --name kubcluster `
   --resource-group sqlbdcgroup `
   --generate-ssh-keys `
   --node-vm-size Standard_L8s `
   --node-count 1 `
   --kubernetes-version <version number>
   ```

   È possibile aumentare o ridurre il numero di nodi dell'agente Kubernetes modificando `--node-count <n>`, dove `<n>` indica il numero di nodi dell'agente che si vuole usare. Questo numero non include il nodo Kubernetes master, gestito in background dal servizio Azure Kubernetes. L'esempio precedente usa un solo nodo a scopo di valutazione. È anche possibile modificare `--node-vm-size` per selezionare una dimensione di macchina virtuale appropriata corrispondente ai requisiti del carico di lavoro. Usare il comando `az vm list-sizes --location westus2 -o table` per elencare le dimensioni delle macchine virtuali disponibili nella propria area.

   Dopo alcuni minuti, il comando viene completato e restituisce le informazioni in formato JSON sul cluster.

   > [!TIP]
   > Se vengono generati errori durante la creazione del cluster nel servizio Azure Kubernetes, vedere la [sezione relativa alla risoluzione dei problemi](#troubleshoot) di questo articolo.

1. Salvare l'output JSON del comando precedente per usarlo in seguito.

## <a name="connect-to-the-cluster"></a>Stabilire la connessione al cluster

1. Per configurare kubectl per la connessione al cluster Kubernetes, eseguire il comando [az aks get-credentials](/cli/azure/aks#az-aks-get-credentials). Questo passaggio consente di scaricare le credenziali e di configurare l'interfaccia della riga di comando di kubectl per usarle.

   ```azurecli
   az aks get-credentials --resource-group=sqlbdcgroup --name kubcluster
   ```

1. Per verificare la connessione al cluster, usare il comando [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) per restituire un elenco di nodi del cluster.  L'esempio seguente mostra l'output nel caso in cui siano presenti un nodo master e tre nodi agente.

   ```bash
   kubectl get nodes
   ```

## <a name="troubleshooting"></a><a id="troubleshoot"></a> Risoluzione dei problemi

Se si verificano problemi durante la creazione di un servizio Azure Kubernetes con i comandi precedenti, provare le risoluzioni seguenti:

- Assicurarsi di avere installato la [versione più recente dell'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli).
- Provare a eseguire gli stessi passaggi usando un gruppo di risorse e un nome di cluster diversi.
- Vedere la [documentazione dettagliata sulla risoluzione dei problemi per il servizio Azure Kubernetes](/azure/aks/troubleshooting).

## <a name="next-steps"></a>Passaggi successivi

Tramite i passaggi in questo articolo è stato configurato un cluster Kubernetes nel servizio Azure Kubernetes. Il passaggio successivo consiste nel distribuire un cluster Big Data di SQL Server 2019 nel cluster Kubernetes del servizio Azure Kubernetes. Per altre informazioni su come distribuire i cluster Big Data, vedere l'articolo seguente:

[Come distribuire[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md)