---
title: Eseguire la distribuzione in Azure Red Hat OpenShift tramite uno script Python
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come usare uno script per distribuire cluster Big Data di SQL Server in Azure Red Hat OpenShift (ARO).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c5889a9411c54312dd52ee1a5e9f2b7917da6838
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637471"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-red-hat-openshift-aro"></a>Usare uno script Python per distribuire cluster Big Data di SQL Server in Azure Red Hat OpenShift (ARO)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In questa esercitazione si userà uno script di distribuzione Python di esempio per distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] in [Azure Red Hat OpenShift (ARO)](/azure/virtual-machines/linux/openshift-get-started). Questa opzione di distribuzione è supportata a partire da SQL Server 2019 CU5.

> [!TIP]
> ARO è solo una delle opzioni che consente l'hosting di Kubernetes per il cluster Big Data. Per informazioni sulle altre opzioni di distribuzione e su come personalizzarle, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).


> [!WARNING]
> I volumi permanenti creati con la classe di archiviazione predefinita *managed-premium* usano il criterio di recupero *Delete*. Di conseguenza, l'eliminazione del cluster Big Data di SQL Server implica l'eliminazione delle attestazioni di volumi permanenti così come dei volumi permanenti. È consigliabile creare classi di archiviazione personalizzate usando lo strumento di provisioning azure-disk con un criterio di recupero *Retain*, come illustrato nei [concetti relativi all'archiviazione](/azure/aks/concepts-storage/#storage-classes). Lo script seguente usa la classe di archiviazione *managed-premium*. Per altre informazioni, vedere l'argomento [Salvataggio permanente dei dati](concept-data-persistence.md).

La distribuzione predefinita di cluster Big Data usata in questo articolo è costituita da un'istanza SQL master, un'istanza del pool di calcolo, due istanze del pool di dati e due istanze del pool di archiviazione. I dati vengono salvati in modo permanente tramite i volumi permanenti Kubernetes che usano le classi di archiviazione predefinite di ARO. La configurazione predefinita usata in questa esercitazione è adatta per ambienti di sviluppo e test.

## <a name="prerequisites"></a>Prerequisiti

- Una sottoscrizione di Azure.
- [oc](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html)
- [Versione minima di Python 3.0](https://www.python.org/downloads)
- [Interfaccia della riga di comando `az`](/cli/azure/install-azure-cli/)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- **Azure Data Studio**

## <a name="log-in-to-your-azure-account"></a>Accedere all'account Azure

Lo script usa l'interfaccia della riga di comando di Azure per automatizzare la creazione di un cluster di ARO. Prima di eseguire lo script, è necessario accedere almeno una volta al proprio account Azure con l'interfaccia della riga di comando di Azure. Al prompt dei comandi eseguire il comando seguente.

```terminal
az login
```

## <a name="instructions"></a>Istruzioni

1. Scaricare sia lo script Python `deploy-sql-big-data-aro.py` sia il file YAML `bdc-scc.yaml`.

   >I file si trovano in questo articolo nelle sezioni seguenti:
   - [`deploy-sql-big-data-aro.py`](#deploy-sql-big-data-aropy)
   - [`bdc-scc.yaml`](#bdc-sccyaml)

1. Eseguire lo script usando quanto segue:

```terminal
python deploy-sql-big-data-aro.py
```

Quando richiesto, specificare l'input per l'ID sottoscrizione di Azure e il gruppo di risorse di Azure in cui creare le risorse. Facoltativamente, è anche possibile specificare l'input per altre configurazioni o usare i valori predefiniti forniti. Ad esempio:

- `azure_region`
- `vm_size` per i nodi di lavoro OpenShift. Per un'esperienza ottimale durante la convalida degli scenari di base, è consigliabile disporre di almeno 8 vCPU e 64 GB di memoria in tutti i nodi di lavoro del cluster. Per impostazione predefinita, lo script usa `Standard_D8s_v3` e 3 nodi di lavoro. Una configurazione di dimensioni predefinite per i cluster Big Data usa anche circa 24 dischi per le attestazioni di volume permanenti in tutti i componenti.
- configurazione di rete per la distribuzione di cluster OpenShift: per informazioni dettagliate su ogni parametro, vedere l'[articolo relativo alla distribuzione di ARO](\azure\openshift\tutorial-create-cluster).
- `cluster_name`: questo valore viene usato sia per il cluster ARO sia per il cluster Big Data di SQL Server creato su ARO. Si noti che il nome del cluster Big data di SQL sarà uno spazio dei nomi Kubernetes.
- `username `: questo è il nome utente per gli account di cui è stato effettuato il provisioning durante la distribuzione per l'account amministratore del controller, l'account dell'istanza master di SQL Server e il gateway. Si noti che l'account di SQL Server `sa` viene disabilitato automaticamente, come procedura consigliata.
- `password`: per questo parametro verrà usato lo stesso valore per tutti gli account.

Il cluster Big Data di SQL Server è ora distribuito in ARO. È ora possibile usare Azure Data Studio per connettersi al cluster. Per altre informazioni, vedere [Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Eseguire la pulizia

Se si sta testando [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Azure, al termine dell'operazione è necessario eliminare il cluster di ARO per evitare l'addebito di costi imprevisti. Non rimuovere il cluster se si intende continuare a usarlo.

> [!WARNING]
> La procedura seguente consente di eliminare il cluster di ARO, rimuovendo anche il cluster Big Data di SQL Server. Se si hanno un database o dati HDFS da conservare, eseguire il backup dei dati prima di eliminare il cluster.

Eseguire il comando dell'interfaccia della riga di comando di Azure seguente per rimuovere il cluster Big Data e il servizio ARO in Azure (sostituire `<resource group name>` con il **gruppo di risorse di Azure** specificato nello script di distribuzione):

```terminal
az group delete -n <resource group name>
```

## `deploy-sql-big-data-aro.py` 

Lo script in questa sezione distribuisce il cluster Big Data di SQL Server in Azure Red Hat OpenShift. Copiare lo script nella workstation e salvarlo come `deploy-sql-big-data-aro.py` prima di iniziare la distribuzione.

```python
#
# Prerequisites: 
# 
# Azure CLI (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), Azure Data CLI (`azdata`) (https://docs.microsoft.com/en-us/sql/big-data-cluster/deploy-install-azdata), oc CLI (https://www.openshift.com/blog/installing-oc-tools-windows)
#
# Run `az login` at least once BEFORE running this script
#

from subprocess import check_output, CalledProcessError, STDOUT, Popen, PIPE, getoutput
from time import sleep
import os
import getpass
import json

def executeCmd (cmd):
    if os.name=="nt":
        process = Popen(cmd.split(),stdin=PIPE, shell=True)
    else:
        process = Popen(cmd.split(),stdin=PIPE)
    stdout, stderr = process.communicate()
    if (stderr is not None):
        raise Exception(stderr)

#
# MUST INPUT THESE VALUES!!!!!
#
SUBSCRIPTION_ID = input("Provide your Azure subscription ID:").strip()
GROUP_NAME = input("Provide Azure resource group name to be created:").strip()
#
# This password will be use for Controller user, Gateway user and SQL Server Master SA accounts
AZDATA_USERNAME=input("Provide username to be used for Controller, SQL Server and Gateway endpoints - Press ENTER for using  `admin`:").strip() or "admin"
AZDATA_PASSWORD = getpass.getpass("Provide password to be used for Controller user, Gateway user and SQL Server Master accounts:")
#
# Optionally change these configuration settings
#
AZURE_REGION=input("Provide Azure region - Press ENTER for using `westus2`:").strip() or "westus2"
# MASTER_VM_SIZE=input("Provide VM size for master nodes for the ARO cluster - Press ENTER for using  `Standard_D2s_v3`:").strip() or "Standard_D2s_v3"
WORKER_VM_SIZE=input("Provide VM size for the worker nodes for the ARO cluster - Press ENTER for using  `Standard_D8s_v3`:").strip() or "Standard_D8s_v3"
OC_NODE_COUNT=input("Provide number of worker nodes for ARO cluster - Press ENTER for using  `3`:").strip() or "3"
VNET_NAME=input("Provide name of Virtual Network for ARO cluster - Press ENTER for using  `aro-vnet`:").strip() or "aro-vnet"
VNET_ADDRESS_SPACE=input("Provide Virtual Network Address Space for ARO cluster - Press ENTER for using  `10.0.0.0/16`:").strip() or "10.0.0.0/16"
MASTER_SUBNET_NAME=input("Provide Master Subnet Name for ARO cluster - Press ENTER for using  `master-subnet`:").strip() or "master-subnet"
MASTER_SUBNET_IP_RANGE=input("Provide address range of Master Subnet for ARO cluster - Press ENTER for using  `10.0.0.0/23`:").strip() or "10.0.0.0/23"
WORKER_SUBNET_NAME=input("Provide Worker Subnet Name for ARO cluster - Press ENTER for using  `worker-subnet`:").strip() or "worker-subnet"
WORKER_SUBNET_IP_RANGE=input("Provide address range of Worker Subnet for ARO cluster - Press ENTER for using  `10.0.2.0/23`:").strip() or "10.0.2.0/23"
#
# This is both Kubernetes cluster name and SQL Big Data cluster name
CLUSTER_NAME=input("Provide name of OpenShift cluster and SQL big data cluster - Press ENTER for using  `sqlbigdata`:").strip() or "sqlbigdata"
#
# Deploy the ARO cluster
#  
print ("Set azure context to subscription: "+SUBSCRIPTION_ID)
command = "az account set -s "+ SUBSCRIPTION_ID
executeCmd (command)
print ("Creating azure resource group: "+GROUP_NAME)
command="az group create --name "+GROUP_NAME+" --location "+AZURE_REGION
executeCmd (command)
command = "az network vnet create --resource-group "+GROUP_NAME+" --name "+VNET_NAME+" --address-prefixes "+VNET_ADDRESS_SPACE
print("Creating Virtual Network: "+VNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+MASTER_SUBNET_NAME+" --address-prefixes "+MASTER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Master Subnet: "+MASTER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+WORKER_SUBNET_NAME+" --address-prefixes "+WORKER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Worker Subnet: "+WORKER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet update --name "+MASTER_SUBNET_NAME+" --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --disable-private-link-service-network-policies true"
print("Updating Master Subnet by disabling Private Link Policies")
executeCmd(command)
command = "az aro create --resource-group "+GROUP_NAME+" --name "+CLUSTER_NAME+" --vnet "+VNET_NAME+" --master-subnet "+MASTER_SUBNET_NAME+" --worker-subnet "+WORKER_SUBNET_NAME+" --worker-count "+OC_NODE_COUNT+" --worker-vm-size "+WORKER_VM_SIZE +" --only-show-errors"
print("Creating OpenShift cluster: "+CLUSTER_NAME)
executeCmd (command)
#
# Login to oc console
#
command = "az aro list-credentials --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
OC_CLUSTER_USERNAME = str(output['kubeadminUsername'])
OC_CLUSTER_PASSWORD = str(output['kubeadminPassword'])
command = "az aro show --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
APISERVER = str(output['apiserverProfile']['url'])
command = "oc login "+ APISERVER+ " -u " + OC_CLUSTER_USERNAME + " -p "+ OC_CLUSTER_PASSWORD
executeCmd (command)
#
# Setup pre-requisites for deploying BDC on OpenShift
#
#
#Creating new project/namespace
command = "oc new-project "+ CLUSTER_NAME
executeCmd (command)
#
# create custom SCC for BDC
command = "oc apply -f bdc-scc.yaml"
executeCmd (command)
#
#Adding the custom scc to BDC namespace
command = "oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:" + CLUSTER_NAME
executeCmd (command)
#
# Deploy big data cluster
#
print ('Set environment variables for credentials')
os.environ['AZDATA_PASSWORD'] = AZDATA_PASSWORD
os.environ['AZDATA_USERNAME'] = AZDATA_USERNAME
os.environ['ACCEPT_EULA']="Yes"
#
#Creating azdata configuration with aro-dev-test profile
command = "azdata bdc config init --source aro-dev-test --target custom --force"
executeCmd (command)
command="azdata bdc config replace -c custom/bdc.json -j ""metadata.name=" + CLUSTER_NAME + ""
executeCmd (command)
#
# Create BDC
command = "azdata bdc create --config-profile custom --accept-eula yes"
executeCmd(command)
#login into BDC cluster and list endpoints
command="azdata login -n " + CLUSTER_NAME
executeCmd (command)
print("")
print("SQL Server big data cluster endpoints: ")
command="azdata bdc endpoint list -o table"
executeCmd(command)
```

## `bdc-scc.yaml`

Il manifesto YAML seguente definisce i vincoli del contesto di sicurezza (SCC) personalizzati per la distribuzione del cluster Big Data. Copiarlo nella stessa directory di `deploy-sql-big-data-aro.py`.

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
  - SETUID
  - SETGID
  - CHOWN
  - SYS_PTRACE
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: SQL Server BDC custom scc is based on 'nonroot' scc plus additional capabilities required by BDC.
  generation: 2
  name: bdc-scc
readOnlyRootFilesystem: false
requiredDropCapabilities:
  - KILL
  - MKNOD
runAsUser:
  type: MustRunAsNonRoot
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
  - configMap
  - downwardAPI
  - emptyDir
  - persistentVolumeClaim
  - projected
  - secret
```

## <a name="next-steps"></a>Passaggi successivi

Lo script di distribuzione ha consentito di configurare il servizio Azure Kubernetes e di distribuire un cluster Big Data di SQL Server 2019. È anche possibile scegliere di personalizzare le distribuzioni future tramite installazioni manuali. Per altre informazioni sulle modalità di distribuzione dei cluster Big Data e su come personalizzare le distribuzioni, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).

Ora che il cluster Big Data di SQL Server è stato distribuito, è possibile caricare i dati di esempio ed esplorare le esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione: Caricare dati di esempio in un cluster Big Data di SQL Server 2019](tutorial-load-sample-data.md)