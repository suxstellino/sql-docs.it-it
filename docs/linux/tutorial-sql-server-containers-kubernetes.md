---
title: Distribuire un contenitore SQL Server con il servizio Azure Kubernetes
description: Questa esercitazione illustra come distribuire una soluzione SQL Server a disponibilità elevata con Kubernetes nel servizio Azure Kubernetes.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 53d63320bc72b0c5f0e1891b67fd41b81d46458e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337292"
---
# <a name="deploy-a-sql-server-container-in-kubernetes-with-azure-kubernetes-services-aks"></a>Distribuire un contenitore SQL Server in Kubernetes con il servizio Azure Kubernetes

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come configurare un'istanza di SQL Server in Kubernetes nel servizio Azure Kubernetes, con archiviazione persistente per la disponibilità elevata. La soluzione garantisce la resilienza. Se l'istanza di SQL Server ha esito negativo, Kubernetes la ricrea automaticamente in un nuovo pod. Kubernetes garantisce la resilienza anche in caso di errore di un nodo.

Questa esercitazione illustra come configurare un'istanza di SQL Server a disponibilità elevata in un contenitore nel servizio Azure Kubernetes.

> [!div class="checklist"]
> * Creare una password dell'amministratore di sistema
> * Creare una risorsa di archiviazione
> * Creare la distribuzione
> * Connettersi con SQL Server Management Studio (SSMS)
> * Verificare l'errore e il ripristino

## <a name="ha-solution-on-kubernetes-running-in-azure-kubernetes-service"></a>Soluzione a disponibilità elevata in Kubernetes in esecuzione nel servizio Azure Kubernetes

Kubernetes 1.6 e versioni successive supportano le [classi di archiviazione](https://kubernetes.io/docs/concepts/storage/storage-classes/), le [richieste di volumi persistenti](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) e il [tipo di volume del disco di Azure](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). È possibile creare e gestire le istanze di SQL Server in modo nativo in Kubernetes. L'esempio in questo articolo illustra come creare una [distribuzione](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) per ottenere una configurazione a disponibilità elevata simile a un'istanza del cluster di failover su disco condiviso. In questa configurazione Kubernetes svolge il ruolo di agente di orchestrazione del cluster. Quando si verifica un errore in un'istanza di SQL Server in un contenitore, l'agente di orchestrazione avvia un'altra istanza del contenitore che si collega alla stessa risorsa di archiviazione permanente.

![Contenitore di SQL Server nel cluster Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

Nel diagramma precedente `mssql-server` è un contenitore in un [pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Kubernetes orchestra le risorse nel cluster. Un [set di repliche](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) garantisce che il pod venga automaticamente ripristinato dopo un errore del nodo. Le applicazioni si connettono al servizio. In questo caso, il servizio rappresenta un servizio di bilanciamento del carico che ospita un indirizzo IP che rimane invariato dopo l'errore di `mssql-server`.

Nel diagramma seguente si è verificato un errore nel contenitore `mssql-server`. Come agente di orchestrazione, Kubernetes garantisce il numero corretto di istanze integre nel set di repliche e avvia un nuovo contenitore in base alla configurazione. L'agente di orchestrazione avvia un nuovo pod nello stesso nodo e `mssql-server` si riconnette alla stessa risorsa di archiviazione permanente. Il servizio si connette al contenitore `mssql-server` ricreato.

![Errore del pod di SQL Server nel cluster Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-pod-fail.png)

Nel diagramma seguente si è verificato un errore nel nodo che ospita il contenitore `mssql-server`. L'agente di orchestrazione avvia il nuovo pod in un nodo diverso e `mssql-server` si riconnette alla stessa risorsa di archiviazione permanente. Il servizio si connette al contenitore `mssql-server` ricreato.

![Ripristino del pod di SQL Server nel cluster Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-node-fail.png)

## <a name="prerequisites"></a>Prerequisiti

* **Cluster Kubernetes**
   - L'esercitazione richiede un cluster Kubernetes. La procedura usa [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) per gestire il cluster. 

   - Vedere [Distribuire un cluster del servizio Azure Kubernetes (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster) per creare e connettersi a un cluster Kubernetes a nodo singolo nel servizio Azure Kubernetes con `kubectl`. 

   >[!NOTE]
   >Per la protezione da errori del nodo, un cluster Kubernetes richiede più di un nodo.

* **Interfaccia della riga di comando di Azure 2.0.23**
   - Le istruzioni in questa esercitazione sono state convalidate con l'interfaccia della riga di comando di Azure 2.0.23.

## <a name="create-an-sa-password"></a>Creare una password dell'amministratore di sistema

Creare una password dell'amministratore di sistema nel cluster Kubernetes. Kubernetes può gestire informazioni di configurazione riservate, ad esempio le password come [segreti](https://kubernetes.io/docs/concepts/configuration/secret/).

Il comando seguente crea una password per l'account dell'amministratore di sistema:

   ```console
   kubectl create secret generic mssql --from-literal=SA_PASSWORD="MyC0m9l&xP@ssw0rd"
   ```  

   Sostituire `MyC0m9l&xP@ssw0rd` con una password complessa.

   Per creare in Kubernetes un segreto denominato `mssql` che contenga il valore `MyC0m9l&xP@ssw0rd` per `SA_PASSWORD`, eseguire il comando.


## <a name="create-storage"></a>Creare una risorsa di archiviazione

Configurare un [volume persistente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) e una [richiesta di volume persistente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-protection) nel cluster Kubernetes. Completare i passaggi seguenti: 

1. Creare un manifesto per definire la classe di archiviazione e la richiesta di volume persistente.  Il manifesto specifica lo strumento di provisioning di archiviazione, i parametri e i [criteri di recupero](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming). Il cluster Kubernetes usa questo manifesto per creare l'archivio permanente. 

   L'esempio YAML seguente definisce una classe di archiviazione e una richiesta di volume persistente. Lo strumento di provisioning della classe di archiviazione è `azure-disk`, perché questo cluster Kubernetes è in Azure. Il tipo di account di archiviazione è `Standard_LRS`. La richiesta di volume persistente è denominata `mssql-data`. I metadati della richiesta di volume persistente includono un'annotazione che la riconnette alla classe di archiviazione. 

   ```yaml
   kind: StorageClass
   apiVersion: storage.k8s.io/v1
   metadata:
        name: azure-disk
   provisioner: kubernetes.io/azure-disk
   parameters:
     storageaccounttype: Standard_LRS
     kind: Managed
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mssql-data
     annotations:
       volume.beta.kubernetes.io/storage-class: azure-disk
   spec:
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
   ```

   Salvare il file (ad esempio, **pvc.yaml**).

1. Creare la richiesta di volume persistente in Kubernetes.

   ```console
   kubectl apply -f <Path to pvc.yaml file>
   ```

   `<Path to pvc.yaml file>` è la posizione in cui si è salvato il file.

   Il volume persistente viene creato automaticamente come account di archiviazione di Azure e associato alla richiesta di volume persistente. 

    ![Screenshot del comando di richiesta di volume persistente](media/tutorial-sql-server-containers-kubernetes/02_pvc_cmd.png)

1. Verificare la richiesta di volume persistente.

   ```console
   kubectl describe pvc <PersistentVolumeClaim>
   ```

   `<PersistentVolumeClaim>` è il nome della richiesta di volume persistente.

   Nel passaggio precedente la richiesta di volume persistente è denominata `mssql-data`. Per visualizzare i metadati relativi alla richiesta di volume persistente, eseguire il comando seguente:

   ```console
   kubectl describe pvc mssql-data
   ```

   I metadati restituiti includono un valore denominato `Volume`. Questo valore esegue il mapping al nome del BLOB.

   ![Screenshot dei metadati restituiti, incluso il volume](media/tutorial-sql-server-containers-kubernetes/describe-volume.png)

   Il valore per il volume corrisponde a una parte del nome del BLOB nell'immagine seguente tratta dal portale di Azure: 

   ![Screenshot del nome del BLOB nel portale di Azure](media/tutorial-sql-server-containers-kubernetes/describe-volume-portal.png)

1. Verificare il volume persistente.

   ```console
   kubectl describe pv
   ```

   `kubectl` restituisce i metadati relativi al volume persistente creato automaticamente e associato alla richiesta di volume persistente. 

## <a name="create-the-deployment"></a>Creare la distribuzione

In questo esempio il contenitore che ospita l'istanza di SQL Server viene descritto come oggetto di distribuzione Kubernetes. La distribuzione crea un set di repliche. Il set di repliche crea il pod. 

In questo passaggio creare un manifesto per descrivere il contenitore in base all'immagine Docker [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) per SQL Server. Il manifesto fa riferimento alla richiesta di volume persistente `mssql-server` e al segreto `mssql` già applicato al cluster Kubernetes. Il manifesto descrive anche un [servizio](https://kubernetes.io/docs/concepts/services-networking/service/). Si tratta di un servizio di bilanciamento del carico. Il servizio di bilanciamento del carico garantisce che l'indirizzo IP venga mantenuto anche dopo il ripristino dell'istanza di SQL Server. 

1. Creare un manifesto (un file YAML) per descrivere la distribuzione. L'esempio seguente descrive una distribuzione, incluso un contenitore basato sull'immagine del contenitore per SQL Server.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: mssql-deployment
   spec:
     replicas: 1
     selector:
        matchLabels:
          app: mssql
     template:
       metadata:
         labels:
           app: mssql
       spec:
         terminationGracePeriodSeconds: 30
         hostname: mssqlinst
         securityContext:
           fsGroup: 10001
         containers:
         - name: mssql
           image: mcr.microsoft.com/mssql/server:2019-latest
           ports:
           - containerPort: 1433
           env:
           - name: MSSQL_PID
             value: "Developer"
           - name: ACCEPT_EULA
             value: "Y"
           - name: SA_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: mssql
                 key: SA_PASSWORD 
           volumeMounts:
           - name: mssqldb
             mountPath: /var/opt/mssql
         volumes:
         - name: mssqldb
           persistentVolumeClaim:
             claimName: mssql-data
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: mssql-deployment
   spec:
     selector:
       app: mssql
     ports:
       - protocol: TCP
         port: 1433
         targetPort: 1433
     type: LoadBalancer
   ```

   Copiare il codice precedente in un nuovo file, denominato `sqldeployment.yaml`. Aggiornare i valori seguenti: 

   * MSSQL_PID `value: "Developer"`: imposta il contenitore per l'esecuzione dell'edizione SQL Server Developer. L'edizione Developer non è concessa in licenza per i dati di produzione. Se la distribuzione è destinata all'uso in produzione, impostare l'edizione appropriata (`Enterprise`, `Standard` o `Express`). 

      >[!NOTE]
      >Per altre informazioni, vedere [Come ottenere una licenza per SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

   * `persistentVolumeClaim`: questo valore richiede una voce per `claimName:` che esegue il mapping al nome usato per la richiesta di volume persistente. In questa esercitazione viene usato `mssql-data`. 

   * `name: SA_PASSWORD`: configura l'immagine del contenitore per impostare la password dell'amministratore di sistema, come definito in questa sezione.

     ```yaml
     valueFrom:
       secretKeyRef:
         name: mssql
         key: SA_PASSWORD
     ```

    Quando Kubernetes distribuisce il contenitore, fa riferimento al segreto denominato `mssql` per ottenere il valore per la password.

   * `securityContext` : definisce le impostazioni dei privilegi e del controllo di accesso per un pod o un contenitore, in questo caso viene specificato a livello di pod, quindi tutti i contenitori (in questo caso solo uno) aderiscono a tale contesto di sicurezza. Nel contesto di sicurezza si definisce il valore fsGroup 10001, ovvero il GID per il gruppo mssql, che indica che tutti i processi del contenitore fanno parte anche del gruppo supplementare con ID 10001(mssql). Il proprietario per il volume /var/opt/mssql e tutti i file creati in tale volume avranno l'ID di gruppo 10001(mssql group).

   >[!NOTE]
   >Usando il tipo di servizio `LoadBalancer`, l'istanza di SQL Server è accessibile in modalità remota (tramite Internet) sulla porta 1433.

   Salvare il file (ad esempio, **sqldeployment.yaml**).

1. Creare la distribuzione.

   ```console
   kubectl apply -f <Path to sqldeployment.yaml file>
   ```

   `<Path to sqldeployment.yaml file>` è la posizione in cui si è salvato il file.

   ![Screenshot del comando di distribuzione](media/tutorial-sql-server-containers-kubernetes/04_deploy_cmd.png)

   Vengono creati la distribuzione e il servizio. L'istanza di SQL Server si trova in un contenitore, connesso a una risorsa di archiviazione permanente.

   Per visualizzare lo stato del pod, digitare `kubectl get pod`.

   ![Screenshot del comando get pod](media/tutorial-sql-server-containers-kubernetes/05_get_pod_cmd.png)

   Nell'immagine precedente il pod ha lo stato `Running`. Questo stato indica che il contenitore è pronto. L'operazione potrebbe richiedere alcuni minuti.

   >[!NOTE]
   >Dopo aver creato la distribuzione, la visualizzazione del pod potrebbe richiedere alcuni minuti. Il ritardo si verifica perché il cluster esegue il pull dell'immagine [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) dall'hub Docker. Dopo il primo pull dell'immagine, le distribuzioni successive potrebbero essere più veloci se la distribuzione viene eseguita in un nodo in cui l'immagine è già memorizzata. 

1. Verificare che i servizi siano in esecuzione. Eseguire il comando seguente:

   ```console
   kubectl get services 
   ```

   Questo comando restituisce i servizi in esecuzione, nonché gli indirizzi IP interni ed esterni per i servizi. Prendere nota dell'indirizzo IP esterno per il servizio `mssql-deployment`. Usare questo indirizzo IP per connettersi a SQL Server. 

   ![Screenshot del comando get service](media/tutorial-sql-server-containers-kubernetes/06_get_service_cmd.png)

   Per altre informazioni sullo stato degli oggetti nel cluster Kubernetes, eseguire:

   ```azurecli
   az aks browse --resource-group <MyResourceGroup> --name <MyKubernetesClustername>
   ```

1. È anche possibile verificare che il contenitore sia in esecuzione non come root eseguendo il comando seguente:

    ```console
    kubectl.exe exec <name of SQL POD> -it -- /bin/bash 
    ```

    e quindi eseguire 'whoami' per verificare che il nome utente è mssql. Che è un utente non root.

    ```console
    whoami
    ```

## <a name="connect-to-the-sql-server-instance"></a>Connettersi all'istanza di SQL Server

Se il contenitore è stato configurato come descritto, è possibile connettersi a un'applicazione dall'esterno della rete virtuale di Azure. Usare l'account `sa` e l'indirizzo IP esterno per il servizio. Usare la password configurata come segreto di Kubernetes. 

È possibile usare le applicazioni seguenti per connettersi all'istanza di SQL Server. 

* [SSMS](./sql-server-linux-manage-ssms.md)

* [SSDT](./sql-server-linux-develop-use-ssdt.md)

* sqlcmd

   Per connettersi con `sqlcmd`, eseguire il comando seguente:

   ```cmd
   sqlcmd -S <External IP Address> -U sa -P "MyC0m9l&xP@ssw0rd"
   ```

   Sostituire i valori seguenti:

  * `<External IP Address>` con l'indirizzo IP per il servizio `mssql-deployment` 
  * `MyC0m9l&xP@ssw0rd` con la password

## <a name="verify-failure-and-recovery"></a>Verificare l'errore e il ripristino

Per verificare l'errore e il ripristino, è possibile eliminare il pod. Eseguire i passaggi seguenti:

1. Elencare il pod che esegue SQL Server.

   ```console
   kubectl get pods
   ```

   Prendere nota del nome del pod che esegue SQL Server.

1. Eliminare il pod.

   ```console
   kubectl delete pod mssql-deployment-0
   ```

   `mssql-deployment-0` è il valore del nome del pod restituito nel passaggio precedente. 

Kubernetes ricrea automaticamente il pod per ripristinare un'istanza di SQL Server e connettersi alla risorsa di archiviazione permanente. Usare `kubectl get pods` per verificare che venga distribuito un nuovo pod. Usare `kubectl get services` per verificare che l'indirizzo IP del nuovo contenitore sia lo stesso. 

## <a name="summary"></a>Summary

In questa esercitazione si è appreso come distribuire contenitori di SQL Server in un cluster di Kubernetes per la disponibilità elevata. 

> [!div class="checklist"]
> * Creare una password dell'amministratore di sistema
> * Creare una risorsa di archiviazione
> * Creare la distribuzione
> * Connettersi con SQL Server Management Studio (SSMS)
> * Verificare l'errore e il ripristino

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
>[Introduzione a Kubernetes](/azure/aks/intro-kubernetes)
