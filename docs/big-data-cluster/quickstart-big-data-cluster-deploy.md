---
title: Eseguire la distribuzione con uno script Python
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come usare uno script di distribuzione per distribuire cluster Big Data di SQL Server nel servizio Azure Kubernetes.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9c0ccb64a67d547b2027423dbc3571e44b3b21ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046172"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-kubernetes-service-aks"></a>Usare uno script Python per distribuire un cluster Big Data di SQL Server nel servizio Azure Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In questa esercitazione si usa uno script di distribuzione Python di esempio per distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] nel servizio Azure Kubernetes.

> [!TIP]
> Il servizio Azure Kubernetes è solo una delle opzioni per ospitare Kubernetes per il cluster Big Data. Per informazioni sulle altre opzioni di distribuzione e su come personalizzarle, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).

La distribuzione predefinita di cluster Big Data usata in questo articolo è costituita da un'istanza SQL master, un'istanza del pool di calcolo, due istanze del pool di dati e due istanze del pool di archiviazione. I dati vengono salvati in modo permanente usando i volumi permanenti Kubernetes che usano le classi di archiviazione predefinite del servizio Azure Kubernetes. La configurazione predefinita usata in questa esercitazione è adatta per ambienti di sviluppo e test.

## <a name="prerequisites"></a>Prerequisiti

- Una sottoscrizione di Azure.
- [Strumenti per Big Data](deploy-big-data-tools.md):
   - **azdata**
   - **kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
   - **Interfaccia della riga di comando di Azure**

## <a name="log-in-to-your-azure-account"></a>Accedere all'account Azure

Lo script usa l'interfaccia della riga di comando di Azure per automatizzare la creazione di un cluster del servizio Azure Kubernetes. Prima di eseguire lo script, è necessario accedere almeno una volta al proprio account Azure con l'interfaccia della riga di comando di Azure. Al prompt dei comandi eseguire il comando seguente.

```
az login
```

## <a name="download-the-deployment-script"></a>Scaricare lo script di distribuzione

Questa esercitazione automatizza la creazione del cluster Big Data nel servizio Azure Kubernetes usando uno script Python **deploy-sql-big-data-aks.py**. Se è già stato installato Python per **azdata**, sarà possibile eseguire correttamente lo script in questa esercitazione. 

A un prompt di Windows PowerShell o della shell Bash Linux eseguire il comando seguente per scaricare lo script di distribuzione da GitHub.

```
curl -o deploy-sql-big-data-aks.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/aks/deploy-sql-big-data-aks.py"
```

## <a name="run-the-deployment-script"></a>Eseguire lo script di distribuzione

Usare la procedura seguente per eseguire lo script di distribuzione in un prompt di PowerShell per Windows o di Bash per Linux. Questo script creerà un servizio Azure Kubernetes in Azure e quindi distribuirà un cluster Big Data di SQL Server 2019 nel servizio Azure Kubernetes. È anche possibile modificare lo script con altre [variabili di ambiente](deployment-guidance.md#configfile) per creare una distribuzione personalizzata.

1. Eseguire lo script con il comando seguente:

   ```
   python deploy-sql-big-data-aks.py
   ```

   > [!NOTE]
   > Se nel computer client e nel percorso sono presenti sia Python3 che Python2, è necessario eseguire il comando usando Python3: `python3 deploy-sql-big-data-aks.py`.

1. Quando richiesto, immettere le informazioni seguenti:

   | valore | Descrizione |
   |---|---|
   | **ID sottoscrizione di Azure** | ID della sottoscrizione di Azure da usare per il servizio Azure Kubernetes. È possibile elencare tutte le sottoscrizioni e i relativi ID eseguendo `az account list` da un'altra riga di comando. |
   | **Gruppo di risorse di Azure** | Nome del gruppo di risorse di Azure da creare per il cluster del servizio Azure Kubernetes. |
   | **Area di Azure** | Area di Azure per il nuovo cluster del servizio Azure Kubernetes (il valore predefinito è **westus**). |
   | **Dimensione computer** | [Dimensione del computer](/azure/virtual-machines/windows/sizes) da usare per i nodi nel cluster del servizio Azure Kubernetes (il valore predefinito è **Standard_D16s_v3**). |
   | **Nodi del ruolo di lavoro** | Numero di nodi del ruolo di lavoro nel cluster del servizio Azure Kubernetes (il valore predefinito è **1**). |
   | **Nome cluster** | Nome del cluster del servizio Azure Kubernetes e del cluster Big Data. Il nome del cluster Big Data deve contenere solo caratteri alfanumerici minuscoli e non spazi (il valore predefinito è **sqlbigdata**). |
   | **Password** | Password per il controller, il gateway HDFS/Spark e l'istanza master (il valore predefinito è **MySQLBigData2019**). |
   | **Nome utente** | Nome utente per l'utente del controller (il valore predefinito è **admin**). |

   > [!IMPORTANT]
   > La dimensione del computer predefinita **Standard_D16s_v3** potrebbe non essere disponibile in tutte le aree di Azure. Se si selezionano dimensioni del computer diverse, assicurarsi che il numero totale di dischi che è possibile collegare tra i nodi del cluster sia maggiore o uguale a 24. Ogni attestazione di volume permanente nel cluster richiede un disco collegato. Attualmente, il cluster Big Data richiede 24 attestazioni di volumi permanenti.
   >
   >Eseguire il comando seguente per identificare i tipi di macchine virtuali disponibili.
   >
   >```azurecli-interactive
   >az vm list-sizes --query "sort_by(@,&name)[?contains(name,'Standard_D16s')]" -l westus2 -o table
   >```

   > [!NOTE]
   > Durante la distribuzione di cluster Big Data, l'account `sa` di SQL Server è disabilitato. Viene eseguito il provisioning di un nuovo account di accesso sysadmin nell'istanza master di SQL Server usando lo stesso nome specificato per l'input **Nome utente** e la password corrispondente all'input **Password**. Gli stessi valori di **Nome utente** e **Password** vengono usati anche per il provisioning di un utente amministratore del controller. Nei cluster distribuiti prima di SQL Server 2019 CU5, l'unico utente supportato per il gateway (Knox) è l'utente **ROOT** e la password è identica a quella indicata in precedenza.
   >[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

1. Lo script si avvierà creando un cluster del servizio Azure Kubernetes con i parametri specificati. Questo passaggio richiede alcuni minuti.

## <a name="monitor-the-status"></a>Monitorare lo stato

Dopo aver creato il cluster del servizio Azure Kubernetes, lo script continua a impostare le variabili di ambiente necessarie usando le impostazioni specificate in precedenza. Chiama quindi **azdata** per distribuire il cluster Big Data nel servizio Azure Kubernetes.

Nella finestra di comando client verrà visualizzato lo stato della distribuzione. Durante il processo di distribuzione dovrebbe essere visualizzata una serie di messaggi che indicano che il processo è in attesa del pod del controller:

```output
2018-11-15 15:42:02.0209 UTC | INFO | Waiting for controller pod to be up...
```

Dopo 10 o 20 minuti, si riceverà una notifica che indica che il pod del controller è in esecuzione:

```output
2018-11-15 15:50:50.0300 UTC | INFO | Controller pod is running.
2018-11-15 15:50:50.0585 UTC | INFO | Controller Endpoint: https://111.111.111.111:30080
```

> [!IMPORTANT]
> L'intera distribuzione può richiedere molto tempo, necessario per scaricare le immagini dei contenitori per i componenti del cluster Big Data. Non dovrebbero tuttavia essere necessarie diverse ore. In caso di problemi durante la distribuzione, vedere [Monitoraggio e risoluzione dei problemi dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

## <a name="inspect-the-cluster"></a>Esaminare il cluster

In qualsiasi momento durante la distribuzione, è possibile usare **kubectl** o **azdata** per esaminare lo stato e i dettagli del cluster Big Data in esecuzione.

### <a name="use-kubectl"></a>Usare kubectl

Aprire una nuova finestra di comando per usare **kubectl** durante il processo di distribuzione.

1. Eseguire il comando seguente per ottenere un riepilogo dello stato dell'intero cluster:

   ```
   kubectl get all -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > Se il nome del cluster Big Data non è stato modificato, per impostazione predefinita lo script usa **sqlbigdata**.

1. Esaminare i servizi Kubernetes e i relativi endpoint interni ed esterni con il comando **kubectl** seguente:

   ```
   kubectl get svc -n <your-big-data-cluster-name>
   ```

1. È anche possibile esaminare lo stato dei pod Kubernetes con il comando seguente:

   ```
   kubectl get pods -n <your-big-data-cluster-name>
   ```

1. Trovare altre informazioni su un pod specifico con il comando seguente:

   ```
   kubectl describe pod <pod name> -n <your-big-data-cluster-name>
   ```

> [!TIP]
> Per informazioni dettagliate su come monitorare una distribuzione e risolvere i relativi problemi, vedere [Monitoraggio e risoluzione dei problemi di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

## <a name="connect-to-the-cluster"></a>Stabilire la connessione al cluster

Al termine dell'esecuzione dello script di distribuzione, l'output segnala l'esito positivo:

```output
2018-11-15 16:10:25.0583 UTC | INFO | Cluster state: Ready
2018-11-15 16:10:25.0583 UTC | INFO | Cluster deployed successfully.
```

Il cluster Big Data di SQL Server è ora distribuito nel servizio Azure Kubernetes. È ora possibile usare Azure Data Studio per connettersi al cluster. Per altre informazioni, vedere [Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Eseguire la pulizia

Se si sta testando [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Azure, al termine dell'operazione è necessario eliminare il cluster del servizio Azure Kubernetes per evitare l'addebito di costi imprevisti. Non rimuovere il cluster se si intende continuare a usarlo.

> [!WARNING]
> La procedura seguente consente di eliminare il cluster del servizio Azure Kubernetes, rimuovendo anche il cluster Big Data di SQL Server. Se si hanno un database o dati HDFS da conservare, eseguire il backup dei dati prima di eliminare il cluster.

Eseguire il comando dell'interfaccia della riga di comando di Azure seguente per rimuovere il cluster Big Data e il servizio Azure Kubernetes (sostituire `<resource group name>` con il **gruppo di risorse di Azure** specificato nello script di distribuzione):

```azurecli
az group delete -n <resource group name>
```

## <a name="next-steps"></a>Passaggi successivi

Lo script di distribuzione ha consentito di configurare il servizio Azure Kubernetes e di distribuire un cluster Big Data di SQL Server 2019. È anche possibile scegliere di personalizzare le distribuzioni future tramite installazioni manuali. Per altre informazioni sulle modalità di distribuzione dei cluster Big Data e su come personalizzare le distribuzioni, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).

Ora che il cluster Big Data di SQL Server è stato distribuito, è possibile caricare i dati di esempio ed esplorare le esercitazioni:

> [!div class="nextstepaction"]
> [Esercitazione: Caricare dati di esempio in un cluster Big Data di SQL Server 2019](tutorial-load-sample-data.md)
