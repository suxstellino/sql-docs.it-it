---
title: Distribuire un cluster Big Data nel cluster privato del servizio Azure Kubernetes (AKS)
titleSuffix: SQL Server Big Data Cluster
description: Informazioni su come distribuire un cluster Big Data di SQL Server con il cluster privato del servizio Azure Kubernetes (AKS) tramite la gestione di rete avanzata (CNI).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d9197e936f3df8f0a98db672f69ac05fce07ebd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043881"
---
# <a name="deploy-bdc-in-azure-kubernetes-service-aks-private-cluster"></a>Distribuire un cluster Big Data nel cluster privato del servizio Azure Kubernetes (AKS)

Questo articolo illustra come distribuire cluster Big Data di SQL Server nel cluster privato del servizio Azure Kubernetes (AKS). Questa configurazione supporta l'uso limitato di indirizzi IP pubblici nell'ambiente di rete aziendale.

Una distribuzione privata offre i vantaggi seguenti:

* Uso limitato degli indirizzi IP pubblici
* Il traffico di rete tra i server applicazioni e i pool di nodi del cluster rimane solo sulla rete privata
* Con la configurazione personalizzata, il traffico in uscita deve soddisfare requisiti specifici

Questo articolo illustra come usare un cluster privato AKS per limitare l'uso di un indirizzo IP pubblico quando sono applicate le stringhe di sicurezza corrispondenti.

## <a name="deploy-private-bdc-cluster-with-aks-private-cluster"></a>Distribuire un cluster Big Data privato con un cluster privato AKS

Per iniziare, creare un [cluster privato AKS](/azure/aks/private-clusters) per assicurarsi che il traffico di rete tra il server API e i pool di nodi risieda solo sulla rete privata. In un cluster privato AKS il piano di controllo o il server API dispone di indirizzi IP interni.

Questa sezione illustra come distribuire un cluster Big Data nel cluster privato del servizio Azure Kubernetes (AKS) con gestione di rete avanzata (CNI).

## <a name="create-a-private-aks-cluster-with-advanced-networking"></a>Creare un cluster privato AKS con gestione di rete avanzata

```bash

export REGION_NAME=<your Azure region >
export RESOURCE_GROUP=< your resource group name >
export SUBNET_NAME=aks-subnet
export VNET_NAME=bdc-vnet
export AKS_NAME=< your aks private cluster name >
 
az group create -n $RESOURCE_GROUP -l $REGION_NAME
 
az network vnet create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $VNET_NAME \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name $SUBNET_NAME \
    --subnet-prefix 10.1.0.0/16
 

SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
 
echo $SUBNET_ID
## will be displayed something similar as the following: 
/subscriptions/xxxx-xxxx-xxx-xxxx-xxxxxxxx/resourceGroups/your-bdc-rg/providers/Microsoft.Network/virtualNetworks/your-aks-vnet/subnets/your-aks-subnet
```

### <a name="create-aks-private-cluster-with-advanced-networking-cni"></a>Creare un cluster privato AKS con gestione di rete avanzata (CNI)

Per eseguire il passaggio successivo, è necessario effettuare il provisioning di un cluster AKS con Load Balancer Standard, con la funzionalità cluster privato abilitata. Il comando è simile al seguente: 

```bash
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Dopo aver completato la distribuzione, è possibile passare al gruppo di risorse `<MC_yourakscluster>` e confermare che `kube-apiserver` è un endpoint privato. Vedere ad esempio la sezione seguente.

## <a name="connect-to-an-aks-cluster"></a>Connettersi a un cluster AKS

```azurecli
az aks get-credentials -n $AKS_NAME -g $RESOURCE_GROUP
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Compilare il profilo di distribuzione del cluster Big Data (BDC)

Dopo la connessione a un cluster AKS è possibile iniziare a distribuire il cluster Big Data, preparare la variabile di ambiente e avviare una distribuzione: 

```azurecli
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

Generare e configurare il profilo di distribuzione personalizzato del cluster Big Data:

```azurecli
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-private-sql-server-big-data-cluster-with-ha"></a>Distribuire un cluster Big Data SQL Server privato con disponibilità elevata

Se si [distribuisce un cluster Big Data SQL Server (SQL-BDC) con disponibilità elevata (HA)](deployment-high-availability.md), si userà il profilo di distribuzione `aks-dev-test-ha`. Dopo aver completato la distribuzione è possibile usare lo stesso comando `kubectl get svc` per creare un servizio `master-secondary-svc` aggiuntivo. È necessario configurare `ServiceType` come `NodePort`. Gli altri passaggi sono simili a quelli indicati nella sezione precedente.

Nell'esempio seguente `ServiceType` viene impostato su `NodePort`.

```azurecli
azdata bdc config replace -c private-bdc-aks /bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Distribuire un cluster Big Data in un cluster privato AKS

```azurecli
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="check-deployment-status"></a>Controllare lo stato della distribuzione

La distribuzione può richiedere alcuni minuti ed è possibile usare il comando seguente per verificarne lo stato: 

```console
kubectl get pods -n mssql-cluster -w
```

## <a name="check-the-service-status"></a>Verificare lo stato del servizio

Usare il comando seguente per verificare lo stato dei servizi. Verificare che siano tutti integri e senza IP esterni:

```console
kubectl get services -n mssql-cluster
```

Vedere come [gestire il cluster Big Data nel cluster privato AKS](private-manage.md). Il passaggio successivo è la [connessione al cluster Big Data](connect-to-big-data-cluster.md).

Per questo scenario, vedere gli script di automazione nel [repository di esempi SQL Server su GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Passaggi successivi

[Gestire un cluster privato](private-manage.md)

[Limitare il traffico in uscita del cluster privato Big Data](private-restrict-egress-traffic.md)

[Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
