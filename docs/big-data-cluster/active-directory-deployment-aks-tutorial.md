---
title: Eseguire la distribuzione in Active Directory nei servizi Azure Kubernetes - Esercitazione
titleSuffix: SQL Server Big Data Cluster
description: Informazioni su come distribuire cluster Big Data di SQL Server in modalità AD nei servizi Azure Kubernetes (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 760fc7070f0673f61a28cd126be0c6542ac4ee93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039863"
---
# <a name="tutorial-deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Esercitazione: Distribuire cluster Big Data di SQL Server in modalità AD nei servizi Azure Kubernetes (AKS)

Questo articolo descrive come distribuire un cluster Big Data di SQL Server in modalità di autenticazione di Active Directory con un'architettura di riferimento. L'architettura di riferimento estende il servizio Active Directory Domain Services locale (AD DS) ad Azure. È possibile distribuirlo dal [Centro architetture di Azure](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain) con [blocchi predefiniti di Azure](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks).

## <a name="prerequisites"></a>Prerequisiti

Prima di distribuire un cluster Big Data di SQL Server, è necessario:

* Accedere a una macchina virtuale di Azure per la gestione. Questa macchina virtuale richiede l'accesso alla Rete virtuale di Azure (VNet) in cui verrà distribuito il cluster Big Data. Deve risiedere nella stessa VNet o in una [VNet con peering](/azure/virtual-network/virtual-network-manage-peering).
* [Installare gli strumenti per Big Data](deploy-big-data-tools.md) nella macchina virtuale di gestione.
* Preparare la distribuzione del cluster in [modalità di autenticazione di Active Directory](active-directory-prerequisites.md) nel controller AD locale.

## <a name="create-aks-subnet"></a>Creare la subnet AKS

1. Impostare le variabili di ambiente

   ```console
   export REGION_NAME=< your Azure Region >
   export RESOURCE_GROUP=<your resource group >
   export SUBNET_NAME=aks-subnet
   export VNet_NAME= adds-vnet
   export AKS_NAME= <your aks cluster name>
   ```

1. Creare la subnet AKS

   ```console
   SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNet_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
   ```

Lo screenshot seguente mostra come si pianificano le subnet incluse nella VNet nell'architettura.

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="Cluster AKS con AD e cluster Big Data di SQL Server":::

## <a name="create-an-aks-private-cluster"></a>Creare un cluster privato AKS

È possibile usare il comando seguente per distribuire il cluster privato AKS. Se non è necessario un cluster privato, rimuovere il parametro `--enable-private-cluster` nel comando. Per informazioni su altri requisiti, vedere [Come distribuire un cluster Azure Kubernetes (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster).

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.3.0.10 \
    --service-cidr 10.3.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Dopo la distribuzione di un cluster AKS, [connettersi al cluster AKS](/azure/aks/tutorial-kubernetes-deploy-cluster#connect-to-cluster-using-kubectl).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Verificare la voce DNS inversa per il controller di dominio

Prima di avviare la distribuzione del cluster Big Data in modalità AD nel cluster AKS, verificare che il controller di dominio stesso disponga sia di un **record A** che di un **record PTR** (voce DNS inverso), registrati nel server DNS.

Per verificare questa impostazione, eseguire il comando `nslookup` o eseguire lo [script di PowerShell](troubleshoot-ad-reverse-lookup-zone.md) per verificare che sia disponibile una voce DNS inverso (record PTR) configurata.

## <a name="create-bdc-deployment-profile"></a>Creare un profilo di distribuzione del cluster Big Data

Il comando seguente crea un profilo di distribuzione:

```console
azdata bdc config init --source kubeadm-prod  --target bdc-ad-aks
```

I comandi seguenti vengono usati per impostare i parametri per un profilo di distribuzione.

### <a name="controljson"></a>control.json

```console
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.logs.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].dnsName=controller.contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].dnsName=proxys.contoso.com"

# security settings 
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"192.168.0.4\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"ad1.contoso.com\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainDnsName=contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
```

### <a name="bdcjson"></a>bdc.json

```console
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=master.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=mastersec.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=gateway.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=approxy.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="initiate-deployment"></a>Avviare la distribuzione

Il comando seguente avvia una distribuzione di cluster Big Data:

```console
azdata bdc create --config-profile bdc-ad-aks --accept-eula yes
```

## <a name="next-steps"></a>Passaggi successivi

[Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)

[Esercitazione: Caricare dati di esempio in un cluster Big Data di SQL Server](tutorial-load-sample-data.md)