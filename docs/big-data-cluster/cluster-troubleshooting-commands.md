---
title: Risolvere i problemi relativi a Kubernetes
titleSuffix: SQL Server big data clusters
description: Questo articolo offre utili comandi per il monitoraggio e la risoluzione dei problemi di un cluster Big Data di SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f0d8ba02c56ca625094829770771c053e31802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039641"
---
# <a name="troubleshoot-big-data-clusters-2019-kubernetes"></a>Risolvere i problemi relativi a Kubernetes in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive alcuni comandi utili di Kubernetes che è possibile usare per il monitoraggio e la risoluzione dei problemi di un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Mostra come visualizzare dettagli approfonditi su un pod o altri elementi di Kubernetes che si trovano nel cluster Big Data. Questo articolo presenta anche le attività comuni, ad esempio la copia di file in o da un contenitore che esegue uno dei cluster Big Data di SQL Server.

> [!TIP]
> Per il monitoraggio dello stato di componenti di cluster Big Data è possibile usare i comandi [**azdata bdc status**](deployment-guidance.md#status) o i [notebook per la risoluzione dei problemi](notebooks-manage-bdc.md) disponibili in Azure Data Studio.

> [!TIP]
> Eseguire i comandi **kubectl** seguenti in un computer client Windows (cmd.exe o PowerShell) o Linux (Bash). Questi comandi richiedono la precedente autenticazione nel cluster e un contesto del cluster per l'esecuzione. Ad esempio, per un cluster del servizio Azure Kubernetes creato in precedenza è possibile eseguire `az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>` per scaricare il file di configurazione del cluster Kubernetes e impostare il contesto del cluster.

## <a name="get-status-of-pods"></a>Ottenere lo stato dei pod

È possibile usare il comando `kubectl get pods` per ottenere lo stato dei pod nel cluster per tutti gli spazi dei nomi o per lo spazio dei nomi del cluster Big Data. Le sezioni seguenti mostrano esempi di entrambi i casi.

### <a name="show-status-of-all-pods-in-the-kubernetes-cluster"></a>Mostrare lo stato di tutti i pod nel cluster Kubernetes

Eseguire il comando seguente per ottenere tutti i pod e i rispettivi stati, inclusi i pod che fanno parte dello spazio dei nomi in cui vengono creati i pod del cluster Big Data di SQL Server:

```bash
kubectl get pods --all-namespaces
```

### <a name="show-status-of-all-pods-in-the-sql-server-big-data-cluster"></a>Mostrare lo stato di tutti i pod nel cluster Big Data di SQL Server

Usare il parametro `-n` per specificare un determinato spazio dei nomi. I pod del cluster Big Data di SQL Server vengono creati in un nuovo spazio dei nomi creato in fase di bootstrap del cluster in base al nome del cluster specificato nel file di configurazione della distribuzione. Qui viene usato il nome predefinito `mssql-cluster`.

```bash
kubectl get pods -n mssql-cluster
```

Per un cluster Big Data in esecuzione, l'output dovrebbe essere simile all'elenco seguente:

```output
PS C:\> kubectl get pods -n mssql-cluster
NAME              READY   STATUS    RESTARTS   AGE
appproxy-f2qqt    2/2     Running   0          110m
compute-0-0       3/3     Running   0          110m
control-zlncl     4/4     Running   0          118m
data-0-0          3/3     Running   0          110m
data-0-1          3/3     Running   0          110m
gateway-0         2/2     Running   0          109m
logsdb-0          1/1     Running   0          112m
logsui-jtdnv      1/1     Running   0          112m
master-0          7/7     Running   0          110m
metricsdb-0       1/1     Running   0          112m
metricsdc-shv2f   1/1     Running   0          112m
metricsui-9bcj7   1/1     Running   0          112m
mgmtproxy-x6gcs   2/2     Running   0          112m
nmnode-0-0        1/1     Running   0          110m
storage-0-0       7/7     Running   0          110m
storage-0-1       7/7     Running   0          110m
```

> [!NOTE]
> Durante la distribuzione, i pod con **STATUS** **ContainerCreating** sono ancora in arrivo. Se la distribuzione viene interrotta per qualsiasi motivo, questo può dare un'indicazione del problema. Osservare anche la colonna **READY**. Questa colonna indica il numero di contenitori avviati nel pod. Si noti che le distribuzioni possono richiedere 30 o più minuti, a seconda della configurazione e della rete. Gran parte di questo tempo è dedicato al download delle immagini dei contenitori per diversi componenti.

## <a name="get-pod-details"></a>Ottenere i dettagli del pod

Eseguire il comando seguente per ottenere una descrizione dettagliata di un pod specifico con output in formato JSON. Sono inclusi i dettagli, ad esempio il nodo Kubernetes corrente in cui si trova il pod, i contenitori in esecuzione all'interno del pod e l'immagine usata per il bootstrap dei contenitori. Mostra anche altri dettagli, tra cui etichette, stato e attestazioni di volumi permanenti associati al pod.

```bash
kubectl describe pod  <pod_name> -n <namespace_name>
```

L'esempio seguente mostra i dettagli per il pod `master-0` in un cluster Big Data denominato `mssql-cluster`:

```bash
kubectl describe pod  master-0 -n mssql-cluster
```

Se si verificano errori, talvolta è possibile visualizzare l'errore negli eventi recenti per il pod.

## <a name="get-pod-logs"></a>Ottenere i log del pod

È possibile recuperare i log per i contenitori in esecuzione in un pod. Il comando seguente recupera i log per tutti i contenitori in esecuzione nel pod denominato `master-0` e li restituisce in un file denominato `master-0-pod-logs.txt`:

```bash
kubectl logs master-0 --all-containers=true -n mssql-cluster > master-0-pod-logs.txt
```

## <a name="get-status-of-services"></a><a id="services"></a> Ottenere lo stato dei servizi

Eseguire il comando seguente per ottenere i dettagli per i servizi del cluster Big Data. Questi dettagli includono il tipo e gli indirizzi IP associati ai rispettivi servizi e porte. I servizi del cluster Big Data di SQL Server vengono creati in un nuovo spazio dei nomi creato in fase di bootstrap del cluster in base al nome del cluster specificato nel file di configurazione della distribuzione.

```bash
kubectl get svc -n <namespace_name>
```

L'esempio seguente mostra lo stato per i servizi in un cluster Big Data denominato `mssql-cluster`:

```bash
kubectl get svc -n mssql-cluster
```

I servizi seguenti supportano connessioni esterne al cluster Big Data:

| Service | Descrizione |
|---|---|
| **master-svc-external** | Permette l'accesso all'istanza master<br/>(**EXTERNAL-IP, 31433** e utente **SA**). |
| **controller-svc-external** | Supporta strumenti e client che gestiscono il cluster. |
| **gateway-svc-external** | Permette l'accesso al gateway ADFS/Spark<br/>(**EXTERNAL-IP** e utente `<AZDATA_USERNAME>`)<sup>1</sup>|
| **appproxy-svc-external** | Supporta scenari di distribuzione di applicazioni. |

<sup>1</sup> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

> [!TIP]
> Questo è un modo per visualizzare i servizi con **kubectl**, ma è anche possibile usare il comando `azdata bdc endpoint list` per visualizzare questi endpoint. Per altre informazioni, vedere [Ottenere gli endpoint del cluster Big Data](deployment-guidance.md#endpoints).

## <a name="get-service-details"></a>Ottenere i dettagli dei servizi

Eseguire il comando seguente per ottenere una descrizione dettagliata di un servizio con output in formato JSON. La descrizione includerà dettagli come etichette, selettore, indirizzo IP esterno (se il servizio è di tipo LoadBalancer), porta e così via.

```bash
kubectl describe service <service_name> -n <namespace_name>
```

L'esempio seguente recupera i dettagli per il servizio **master-svc-external**:

```bash
kubectl describe service master-svc-external -n mssql-cluster
```

## <a name="copy-files"></a><a id="copy"></a> Copiare file

Se è necessario copiare file dal contenitore al computer locale, usare il comando `kubectl cp` con la sintassi seguente:

```bash
kubectl cp <pod_name>:<source_file_path> -c <container_name> -n <namespace_name> <target_local_file_path>
```

Analogamente, è possibile usare `kubectl cp` per copiare file dal computer locale a un contenitore specifico.

```bash
kubectl cp <source_local_file_path> <pod_name>:<target_container_path> -c <container_name>  -n <namespace_name>
```

### <a name="copy-files-from-a-container"></a><a id="copyfrom"></a> Copiare file da un contenitore

L'esempio seguente copia i file di log di SQL Server dal contenitore al percorso `~/temp/sqlserverlogs` nel computer locale (in questo esempio il computer locale è un client Linux):

```bash
kubectl cp master-0:/var/opt/mssql/log -c mssql-server -n mssql-cluster ~/tmp/sqlserverlogs
```

### <a name="copy-files-into-container"></a><a id="copyinto"></a> Copiare file in un contenitore

L'esempio seguente copia il file **AdventureWorks2016CTP3.bak** dal computer locale al contenitore dell'istanza master di SQL Server (`mssql-server`) nel pod `master-0`. Il file viene copiato nella directory `/tmp` nel contenitore. 

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/tmp -c mssql-server -n mssql-cluster
```

## <a name="force-delete-a-pod"></a><a id="forcedelete"></a> Forzare l'eliminazione di un pod
 
Non è consigliabile forzare l'eliminazione di un pod. Tuttavia, per verificare la disponibilità, la resilienza o il salvataggio permanente dei dati, è possibile eliminare un pod per simulare un errore del pod con il comando `kubectl delete pods`.

```bash
kubectl delete pods <pod_name> -n <namespace_name> --grace-period=0 --force
```

L'esempio seguente elimina il pod del pool di archiviazione `storage-0-0`:

```bash
kubectl delete pods storage-0-0 -n mssql-cluster --grace-period=0 --force
```

## <a name="get-pod-ip"></a><a id="getip"></a> Ottenere l'indirizzo IP del pod
 
Ai fini della risoluzione dei problemi, può essere necessario ottenere l'indirizzo IP del nodo in cui viene attualmente eseguito un pod. Per ottenere l'indirizzo IP, usare il comando `kubectl get pods` con la sintassi seguente:

```bash
kubectl get pods <pod_name> -o yaml -n <namespace_name> | grep hostIP
```

L'esempio seguente ottiene l'indirizzo IP del nodo in cui viene eseguito il pod `master-0`:

```bash
kubectl get pods master-0 -o yaml -n mssql-cluster | grep hostIP
```

## <a name="kubernetes-dashboard"></a>Dashboard di Kubernetes

È possibile avviare il dashboard di Kubernetes per altre informazioni sul cluster. Le sezioni seguenti descrivono come avviare il dashboard per Kubernetes nel servizio Azure Kubernetes e per Kubernetes quando viene eseguito il bootstrap con kubeadm.
 
### <a name="start-dashboard-when-cluster-is-running-in-aks"></a>Avviare il dashboard quando il cluster è in esecuzione nel servizio Azure Kubernetes

Per avviare il dashboard di Kubernetes, eseguire:

```bash
az aks browse --resource-group <azure_resource_group> --name <aks_cluster_name>
```

> [!Note]
> Se viene restituito l'errore seguente: *Unable to listen on port 8001: All listeners failed to create with the following errors: Unable to create listener: Errore Listen tcp4 127.0.0.1:8001: > bind: Only one usage of each socket address (protocol/network address/port) is normally permitted. Unable to create listener: Error listen tcp6: address [[::1]]:8001: missing port in >address error: Unable to listen on any of the requested ports: [{8001 9090}]* , assicurarsi di non aver già avviato il dashboard da un'altra finestra.

Quando si avvia il dashboard nel browser, possono essere restituiti avvisi di autorizzazione perché il controllo degli accessi in base al ruolo è abilitato per impostazione predefinita nei cluster del servizio Azure Kubernetes e l'account del servizio usato dal dashboard non ha autorizzazioni sufficienti per accedere a tutte le risorse, ad esempio: *pod is forbidden: User "system:serviceaccount:kube-system:kubernetes-dashboard" cannot list pods in the namespace "default"* . Eseguire il comando seguente per concedere le autorizzazioni necessarie a `kubernetes-dashboard` e quindi riavviare il dashboard:

```bash
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

### <a name="start-dashboard-when-kubernetes-cluster-is-bootstrapped--using-kubeadm"></a>Avviare il dashboard quando il bootstrap del cluster Kubernetes viene eseguito con kubeadm

Per istruzioni dettagliate su come distribuire e configurare il dashboard nel cluster Kubernetes, vedere la [documentazione di Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). Per avviare il dashboard di Kubernetes, eseguire questo comando:

```bash
kubectl proxy
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md)
