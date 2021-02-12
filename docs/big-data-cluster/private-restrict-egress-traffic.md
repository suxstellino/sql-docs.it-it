---
title: Limitare il traffico in uscita dei cluster Big Data (BDC) nel cluster privato del servizio Azure Kubernetes (AKS)
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come limitare il traffico in uscita dei cluster Big Data (BDC) nel cluster privato del servizio Azure Kubernetes (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 589031e40ba0f1e53afd5b4082d132358e9cda32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046021"
---
# <a name="restrict-egress-traffic-of-big-data-clusters-bdc-clusters-in-azure-kubernetes-service-aks-private-cluster"></a>Limitare il traffico in uscita dei cluster Big Data (BDC) nel cluster privato del servizio Azure Kubernetes (AKS)

Il servizio Azure Kubernetes effettua il provisioning di un Load Balancer dello SKU standard, che verrà configurato e usato per l'uscita come opzione predefinita. La configurazione predefinita potrebbe, però, non soddisfare i requisiti di tutti gli scenari se gli indirizzi IP pubblici non sono consentiti o se per l'uscita sono necessari hop aggiuntivi. Definire una tabella di route definita dall'utente se il cluster non consente gli indirizzi IP pubblici e segue un'appliance virtuale di rete.

Per impostazione predefinita, i cluster del servizio Azure Kubernetes hanno accesso a Internet in uscita illimitato per finalità di gestione e operative. I nodi di lavoro in un cluster del servizio Azure Kubernetes devono poter accedere a porte e nomi di dominio completi (FQDN) specifici, quali:

* aggiornamenti della sicurezza del sistema operativo del nodo di lavoro; il cluster deve eseguire il pull delle immagini del contenitore di sistema di base da Microsoft Container Registry
* i nodi di lavoro del servizio Azure Kubernetes abilitati per GPU devono accedere ad alcuni endpoint da Nvidia per installare il driver
* nello scenario in cui i clienti usano il servizio Azure Kubernetes insieme ai servizi di Azure, come i criteri di Azure per la conformità di livello aziendale, monitoraggio di Azure (con informazioni dettagliate sul contenitore) 
* nello scenario abilitato per Dev Space e altri scenari di natura simile

> [!NOTE]
> Attualmente, quando si distribuisce un cluster Big Data nel cluster privato del servizio Azure Kubernetes (AKS), non sono presenti dipendenze in ingresso nello scenario, oltre a quanto indicato in questo articolo; è possibile trovare tutte le dipendenze in uscita nel [traffico in uscita di controllo per i nodi del cluster nel servizio Azure Kubernetes (AKS)](/azure/aks/limit-egress-traffic).

Questo articolo illustra come distribuire un cluster Big Data nel cluster privato AKS con la funzionalità di rete avanzata e l'UDR (route definita dall'utente), nonché come integrarlo ulteriormente con l'ambiente di rete di livello aziendale.

## <a name="use-azure-firewall-to-restrict-egress-traffic"></a>Usare il firewall di Azure per limitare il traffico in uscita

Il firewall di Azure fornisce un tag FQDN del servizio Azure Kubernetes `(AzureKubernetesService)` per semplificare questa configurazione.

Per informazioni complete su questo tag, vedere [Limitare il traffico in uscita con il firewall di Azure](/azure/aks/limit-egress-traffic#restrict-egress-traffic-using-azure-firewall).

L'immagine seguente mostra come il traffico viene limitato in un cluster privato del servizio Azure Kubernetes. 

:::image type="content" source="media/private-cluster-restrict-egress-traffic/aks-azure-firewall-egress.png" alt-text="Traffico in uscita del firewall del cluster privato AKS":::

Per configurare un'architettura di base con il firewall di Azure:

1. Creare il gruppo di risorse e VNet
2. Creare e configurare il firewall di Azure
3. Creare una tabella di route definita dall'utente
4. Configurare le regole del firewall
5. Creare un'entità servizio (SP)
6. Creare un cluster privato AKS
7. Creare un profilo di distribuzione BDC
8. Distribuire BDC

I passaggi seguenti forniscono informazioni dettagliate.

## <a name="create-the-resource-group-and-vnet"></a>Creare il gruppo di risorse e VNet

1. Definire un set di variabili di ambiente per la creazione di risorse.

   ```console
   export REGION_NAME=<region>
   export RESOURCE_GROUP=private-bdc-aksudr-rg
   export SUBNET_NAME=aks-subnet
   export VNET_NAME=bdc-vnet
   export AKS_NAME=bdcaksprivatecluster
   ```

1. Creare il gruppo di risorse

   ```azurecli
   az group create -n $RESOURCE_GROUP -l $REGION_NAME
   ```

1. Creare la rete virtuale

   ```azurecli
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
   ```

## <a name="create-and-set-up-azure-firewall"></a>Creare e configurare il firewall di Azure

1. Definire un set di variabili di ambiente per la creazione di risorse.

   ```console
   export FWNAME=bdcaksazfw
   export FWPUBIP=$FWNAME-ip
   export FWIPCONFIG_NAME=$FWNAME-config

   az extension add --name azure-firewall
   ```

1. Creare una subnet dedicata per il firewall

   > [!NOTE]
   > Non è possibile modificare il nome del firewall dopo la creazione

   ```azurecli
   az network vnet subnet create \
     --resource-group $RESOURCE_GROUP \
     --vnet-name $VNET_NAME \
     --name AzureFirewallSubnet \
     --address-prefix 10.3.0.0/24

    az network firewall create -g $RESOURCE_GROUP -n $FWNAME -l $REGION_NAME --enable-dns-proxy true

    az network public-ip create -g $RESOURCE_GROUP -n $FWPUBIP -l $REGION_NAME --sku "Standard"

    az network firewall ip-config create -g $RESOURCE_GROUP -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBIP --vnet-name $VNET_NAME
   ```

Per impostazione predefinita Azure effettua il routing automatico del traffico tra subnet di Azure, reti virtuali e reti locali. 

## <a name="create-user-defined-route-table"></a>Creare una tabella di route definita dall'utente

Creare una tabella di route definita dall'utente (UDR) con un hop al firewall di Azure.

```azurecli

export SUBID= <your Azure subscription ID>
export FWROUTE_TABLE_NAME=bdcaks-rt
export FWROUTE_NAME=bdcaksroute
export FWROUTE_NAME_INTERNET=bdcaksrouteinet

export FWPUBLIC_IP=$(az network public-ip show -g $RESOURCE_GROUP -n $FWPUBIP --query "ipAddress" -o tsv)
export FWPRIVATE_IP=$(az network firewall show -g $RESOURCE_GROUP -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)

# Create UDR and add a route for Azure Firewall

az network route-table create -g $RESOURCE_GROUP --name $FWROUTE_TABLE_NAME

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

## <a name="set-up-firewall-rules"></a>Configurare le regole del firewall 

```azurecli
# Add FW Network Rules

az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apiudp' --protocols 'UDP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 1194 --action allow --priority 100
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apitcp' --protocols 'TCP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 9000
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'time' --protocols 'UDP' --source-addresses '*' --destination-fqdns 'ntp.ubuntu.com' --destination-ports 123

# Add FW Application Rules

az network firewall application-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwar' -n 'fqdn' --source-addresses '*' --protocols 'http=80' 'https=443' --fqdn-tags "AzureKubernetesService" --action allow --priority 100
```

È possibile associare la tabella di route definita dall'utente (UDR) al cluster AKS in cui è stato distribuito in precedenza un cluster Big Data (BDC) tramite il comando seguente:

```azurecli
az network vnet subnet update -g $RESOURCE_GROUP --vnet-name $VNET_NAME --name $SUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="create--configure-service-principal-sp"></a>Creare e configurare l'entità servizio (SP)

In questo passaggio è necessario creare l'entità servizio e assegnare l'autorizzazione alla rete virtuale.

Vedere l'esempio seguente: 

```azurecli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "bdcaks-sp" --skip-assignment

APPID=<your service principal ID >
PASSWORD=< your service principal password >
VNETID=$(az network vnet show -g $RESOURCE_GROUP --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role "Network Contributor"


RTID=$(az network route-table show -g $RESOURCE_GROUP -n $FWROUTE_TABLE_NAME --query id -o tsv)
az role assignment create --assignee $APPID --scope $RTID --role "Network Contributor"
```

## <a name="create-aks-cluster"></a>Creare un cluster del servizio Azure Container

Creare quindi il cluster AKS con `userDefinedRouting` come tipo in uscita.

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --outbound-type userDefinedRouting \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --service-principal $APPID \
    --client-secret $PASSWORD \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Compilare il profilo di distribuzione del cluster Big Data (BDC)

È possibile creare cluster BDC con un profilo personalizzato: 

```console
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

## <a name="generate-and-config-bdc-custom-deployment-profile"></a>Generare e configurare il profilo di distribuzione personalizzato del cluster Big Data: 
```console
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Distribuire un cluster Big Data in un cluster privato AKS

```console
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="use-third-party-firewall-instead-of-azure-firewall"></a>Usare firewall di terze parti anziché il firewall di Azure

Usare un firewall di terze parti per limitare il traffico in uscita quando il cluster Big Data viene distribuito con cluster privato AKS. Vedere ad esempio [Firewall di Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=firewall&page=1). È possibile usare firewall di terze parti nelle soluzioni di distribuzione privata con configurazioni più conformi. Il firewall deve fornire le seguenti regole di rete:

* tutte le [regole di rete in uscita e gli FQDN per cluster AKS](/azure/aks/limit-egress-traffic#required-outbound-network-rules-and-fqdns-for-aks-clusters) e tutti gli endpoint HTTP/HTTPS con caratteri jolly e le dipendenze che possono variare con il cluster AKS in base a diversi qualificatori e ai requisiti effettivi;
* le regole di rete/l'FQDN/le regole di applicazione richieste da Azure Global sono indicate [qui](/azure/aks/limit-egress-traffic#azure-global-required-network-rules).
* L'FQDN/le regole di applicazione raccomandate facoltative per i cluster AKS sono indicate [qui](/azure/aks/limit-egress-traffic#optional-recommended-fqdn--application-rules-for-aks-clusters). 

Vedere come [gestire il cluster Big Data nel cluster privato AKS](private-manage.md). Il passaggio successivo è la [connessione al cluster Big Data](connect-to-big-data-cluster.md).

Per questo scenario, vedere gli script di automazione nel [repository di esempi SQL Server su GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Passaggi successivi

[Gestire cluster Big Data nel cluster privato AKS](private-manage.md)

[Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
