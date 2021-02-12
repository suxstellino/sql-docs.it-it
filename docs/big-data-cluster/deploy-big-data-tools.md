---
title: Installare strumenti Big Data
titleSuffix: SQL Server big data clusters
description: Informazioni su come installare gli strumenti usati con cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 98226c45b9bc677386870e432ee835803bde32a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047523"
---
# <a name="install-sql-server-2019-big-data-tools"></a>Installare gli strumenti per Big Data di SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive gli strumenti client che devono essere installati per la creazione, la gestione e l'uso di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. La sezione seguente contiene un elenco di strumenti e collegamenti alle istruzioni di installazione. Prima di distribuire un cluster Big Data, configurare gli strumenti indicati come obbligatori in Windows o Linux.

## <a name="big-data-cluster-tools"></a>Strumenti per cluster Big Data

La tabella seguente elenca gli strumenti comuni per i cluster Big Data e indica come installarli:

| Strumento | Obbligatoria | Descrizione | Installazione |
|---|---|---|---|
| `python` | Sì | Python è un linguaggio di programmazione di alto livello interpretato e orientato a oggetti con semantica dinamica. Molte parti dei cluster Big Data per SQL Server usano Python. | [Installare Python](#python)|
| [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] | Sì | Strumento da riga di comando per l'installazione e la gestione di un cluster Big Data. | [Installazione](../azdata/install/deploy-install-azdata.md) |
| `kubectl`<sup>1</sup> | Sì | Strumento da riga di comando per il monitoraggio del cluster Kubernetes sottostante ([altre informazioni](https://kubernetes.io/docs/tasks/tools/install-kubectl/)). | [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| **Azure Data Studio** | Sì | Strumento grafico multipiattaforma per l'esecuzione di query su SQL Server. | [Installazione](../azure-data-studio/download-azure-data-studio.md) |
| **Estensione di virtualizzazione dei dati** | Sì | Estensione per Azure Data Studio con una procedura guidata di virtualizzazione dei dati. | [Installazione](../azure-data-studio/extensions/data-virtualization-extension.md) |
| **Interfaccia della riga di comando di Azure**<sup>2</sup> | Per il servizio Azure Kubernetes | Interfaccia della riga di comando moderna per la gestione dei servizi di Azure. Viene usata con distribuzioni di cluster Big Data nel servizio Azure Kubernetes ([altre informazioni](/cli/azure/)). | [Installazione](/cli/azure/install-azure-cli) |
| **mssql-cli** | Facoltativo | Interfaccia della riga di comando moderna per l'esecuzione di query su SQL Server ([altre informazioni](../tools/mssql-cli.md)). | [Windows](https://github.com/dbcli/mssql-cli/blob/master/doc/installation/windows.md) \| [Linux](https://github.com/dbcli/mssql-cli/blob/master/doc/installation/linux.md) |
| **sqlcmd** | Per alcuni script | Strumento da riga di comando legacy per l'esecuzione di query su SQL Server ([altre informazioni](../tools/sqlcmd-utility.md)). Potrebbe essere necessario installare Microsoft ODBC Driver 11 per SQL Server prima di installare il pacchetto SQLCMD. | [Windows](https://www.microsoft.com/download/details.aspx?id=36433) \| [Linux](../linux/sql-server-linux-setup-tools.md) |
| `curl` <sup>3</sup> | Per alcuni script | Strumento da riga di comando per il trasferimento di dati con URL. | [Windows](https://curl.haxx.se/windows/) \| Linux: installare il pacchetto curl |
| `oc` | Obbligatorio per le distribuzioni di Red Hat OpenShift e Azure RedHat OpenShift. |`oc` è l'interfaccia della riga di comando di Open Shift. | [Installazione dell'interfaccia della riga di comando](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli)

<sup>1</sup> È necessario usare `kubectl` versione 1.13 o successiva. Il numero di versione di `kubectl` deve anche essere compreso tra il numero precedente e quello successivo della versione secondaria del cluster Kubernetes. Se si vuole installare una versione specifica nel client `kubectl`, vedere [Installare il file binario di `kubectl` tramite curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl). In Windows 10 usare cmd.exe e non Windows PowerShell per eseguire curl.

> [!TIP]
> Per usare `kubectl` con un cluster distribuito in precedenza nel servizio Azure Kubernetes, è necessario impostare il contesto del cluster con il comando dell'interfaccia della riga di comando di Azure seguente:
>
>    ```azurecli
>    az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>
>    ```

<sup>2</sup> È necessario usare l'interfaccia della riga di comando di Azure 2.0.4 o versioni successive. Eseguire `az --version` per trovare la versione, se necessario.

<sup>3</sup> In Windows 10, `curl` si trova già in PATH quando viene eseguito dal prompt dei comandi. Per le altre versioni di Windows, scaricare `curl` usando il collegamento e inserirlo in PATH.

## <a name="which-tools-are-required"></a>Strumenti necessari

La tabella precedente contiene tutti gli strumenti comuni usati con i cluster Big Data. Gli strumenti effettivamente necessari dipendono dallo scenario. Tuttavia, in generale gli strumenti seguenti sono più importanti per la gestione del cluster, la connessione al cluster e l'esecuzione di query sul cluster:

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- **Azure Data Studio**
- **Estensione di virtualizzazione dei dati**

Gli strumenti rimanenti sono necessari solo in determinati scenari. È possibile usare l'**interfaccia della riga di comando di Azure** per gestire i servizi di Azure associati alle distribuzioni nel servizio Azure Kubernetes. **mssql-cli** è uno strumento facoltativo ma utile che permette di connettersi all'istanza master di SQL Server nel cluster ed eseguire query dalla riga di comando. **sqlcmd** e `curl` sono obbligatori se si prevede di installare dati di esempio con lo script GitHub.

### <a name="install-python-offline"></a><a id="python"></a> Installare Python offline

1. In un computer con accesso a Internet, scaricare uno dei file compressi seguenti contenenti Python:

   | Sistema operativo | Download |
   |---|---|
   | Windows | [https://go.microsoft.com/fwlink/?linkid=2074021](https://go.microsoft.com/fwlink/?linkid=2074021) |
   | Linux   | [https://go.microsoft.com/fwlink/?linkid=2065975](https://go.microsoft.com/fwlink/?linkid=2065975) |
   | OSX     | [https://go.microsoft.com/fwlink/?linkid=2065976](https://go.microsoft.com/fwlink/?linkid=2065976) |

1. Copiare il file compresso nel computer di destinazione ed estrarlo nella cartella desiderata.

1. Solo per Windows, eseguire `installLocalPythonPackages.bat` dalla cartella di installazione e passare il percorso completo della cartella come parametro.

   ```PowerShell
   installLocalPythonPackages.bat "C:\python-3.6.6-win-x64-0.0.1-offline\0.0.1"
   ```

## <a name="download-and-install-azure-data-studio"></a>Scaricare e installare Azure Data Studio

Azure Data Studio offre caratteristiche e funzioni specifiche per i cluster Big Data di SQL Server.

[Ottenere la versione più recente di Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

Per informazioni dettagliate sulla versione più recente, vedere le [note sulla versione](./release-notes-big-data-cluster.md).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver configurato gli strumenti, distribuire un cluster Big Data di SQL Server 2019 in Kubernetes nel cloud o in locale. Per altre informazioni, vedere gli articoli sulla distribuzione seguenti:

- [Avvio rapido: Distribuire un cluster Big Data di SQL Server nel servizio Azure Kubernetes](quickstart-big-data-cluster-deploy.md)
- [Come distribuire[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md)

Per altre informazioni sui cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).