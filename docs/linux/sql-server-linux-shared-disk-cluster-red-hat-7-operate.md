---
title: Usare un'istanza del cluster di failover RHEL per SQL Server in Linux
description: Informazioni su come gestire un'istanza del cluster di failover di dischi condivisi di Red Hat Enterprise Linux (RHEL) per SQL Server per disponibilità elevata, ad esempio failover manuale dell'istanza del cluster di failover, aggiunta o rimozione di nodi nel cluster.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 075ab7d8-8b68-43f3-9303-bbdf00b54db1
ms.openlocfilehash: 272bc33012e37c58f6a3f8ea0ed51f1c216d716f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346275"
---
# <a name="operate-rhel-failover-cluster-instance-fci-for-sql-server"></a>Gestire un'istanza del cluster di failover RHEL per SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo documento descrive come eseguire le attività seguenti per SQL Server in un cluster di failover di dischi condivisi con Red Hat Enterprise Linux.

- Effettuare il failover manuale del cluster
- Monitorare un servizio SQL Server del cluster di failover
- Aggiungere un nodo del cluster
- Rimuovere un nodo del cluster
- Modificare la frequenza di monitoraggio delle risorse di SQL Server

## <a name="architecture-description"></a>Descrizione dell'architettura

Il livello di clustering si basa sul [componente aggiuntivo a disponibilità elevata](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) di Red Hat Enterprise Linux (RHEL), basato a sua volta su [Pacemaker](https://clusterlabs.org/). Corosync e Pacemaker coordinano le comunicazioni del cluster e la gestione delle risorse. L'istanza di SQL Server è attiva in un nodo o nell'altro.

Il diagramma seguente illustra i componenti in un cluster Linux con SQL Server. 

![Cluster SQL del disco condiviso Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Per altre informazioni sulla configurazione del cluster, sulle opzioni degli agenti delle risorse e sulla gestione, vedere la [documentazione di riferimento di RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

## <a name="failover-cluster-manually"></a><a name = "failManual"></a> Effettuare il failover manuale del cluster

Il comando `resource move` crea un vincolo che forza l'avvio della risorsa nel nodo di destinazione.    Dopo l'esecuzione del comando `move`, l'esecuzione di `clear` per la risorsa rimuoverà il vincolo e sarà possibile spostare nuovamente la risorsa o effettuare il failover automatico della risorsa. 

```bash
sudo pcs resource move <sqlResourceName> <targetNodeName>  
sudo pcs resource clear <sqlResourceName> 
```

L'esempio seguente sposta la risorsa **mssqlha** in un nodo denominato **sqlfcivm2** e quindi rimuove il vincolo in modo che la risorsa possa passare in un nodo diverso in un secondo momento.  

```bash
sudo pcs resource move mssqlha sqlfcivm2 
sudo pcs resource clear mssqlha 
```

## <a name="monitor-a-failover-cluster-sql-server-service"></a>Monitorare un servizio SQL Server del cluster di failover

Visualizzare lo stato corrente del cluster:

```bash
sudo pcs status  
```

Visualizzare lo stato in tempo reale del cluster e delle risorse:

```bash
sudo crm_mon 
```

Visualizzare i log degli agenti delle risorse in `/var/log/cluster/corosync.log`

## <a name="add-a-node-to-a-cluster"></a>Aggiungere un nodo a un cluster

1. Controllare l'indirizzo IP per ogni nodo. Lo script seguente mostra l'indirizzo IP del nodo corrente. 

   ```bash
   ip addr show
   ```

3. Il nuovo nodo deve avere un nome univoco di 15 caratteri o meno. Per impostazione predefinita, in Red Hat Linux il nome computer è `localhost.localdomain`. Questo nome predefinito potrebbe non essere univoco ed è troppo lungo. Impostare il nome computer per il nuovo nodo. Per impostare il nome computer, aggiungerlo a `/etc/hosts`. Lo script seguente consente di modificare `/etc/hosts` con `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```

   L'esempio seguente mostra `/etc/hosts` con l'aggiunta di tre nodi denominati `sqlfcivm1`, `sqlfcivm2` e `sqlfcivm3`.

   ```
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1         localhost localhost6 localhost6.localdomain6
   10.128.18.128 fcivm1
   10.128.16.77 fcivm2
   10.128.14.26 fcivm3
    ```
    
   Il file deve essere lo stesso in ogni nodo. 

1. Arrestare il servizio SQL Server nel nuovo nodo.

1. Seguire le istruzioni per montare la directory del file di database nel percorso condiviso:

   Dal server NFS installare `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils 
   ``` 

   Aprire il firewall nei client e nel server NFS 

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

   Modificare il file /etc/fstab per includere il comando mount: 

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr
   ```

   Eseguire `mount -a` per rendere effettive le modifiche.
   
1. Nel nuovo nodo creare un file per archiviare nome utente e password di SQL Server per l'accesso a Pacemaker. Il comando seguente crea e popola questo file:

   ```bash
   sudo touch /var/opt/mssql/passwd
   sudo echo "<loginName>" >> /var/opt/mssql/secrets/passwd
   sudo echo "<loginPassword>" >> /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/passwd
   sudo chmod 600 /var/opt/mssql/passwd
   ```

3. Nel nuovo nodo aprire le porte del firewall di Pacemaker. Per aprire queste porte con `firewalld`, eseguire il comando seguente:

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > [!NOTE]
   > Se si sta usando un altro firewall che non ha una configurazione a disponibilità elevata predefinita, è necessario aprire le porte seguenti per consentire a Pacemaker di comunicare con altri nodi del cluster
   >
   > * TCP: porte 2224, 3121, 21064
   > * UDP: porta 5405

1. Installare i pacchetti Pacemaker nel nuovo nodo.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
 
2. Impostare la password per l'utente predefinito creato durante l'installazione dei pacchetti Pacemaker e Corosync. Usare la stessa password dei nodi esistenti. 

   ```bash
   sudo passwd hacluster
   ```
 
3. Abilitare e avviare il servizio `pcsd` e Pacemaker. In questo modo, il nuovo nodo potrà unirsi nuovamente in join con il cluster dopo il riavvio. Eseguire il comando seguente nel nuovo nodo.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Installare l'agente delle risorse FCI per SQL Server. Eseguire i comandi seguenti nel nuovo nodo. 

   ```bash
   sudo yum install mssql-server-ha
   ```

1. In un nodo esistente del cluster autenticare il nuovo nodo e aggiungerlo al cluster:

    ```bash
    sudo pcs    cluster auth <nodeName3> -u hacluster 
    sudo pcs    cluster node add <nodeName3> 
    ```

    L'esempio seguente aggiunge un nodo denominato **vm3** al cluster.

    ```bash
    sudo pcs    cluster auth  
    sudo pcs    cluster start 
    ```

## <a name="remove-nodes-from-a-cluster"></a>Rimuovere i nodi da un cluster

Per rimuovere un nodo da un cluster, eseguire il comando seguente:

```bash
sudo pcs    cluster node remove <nodeName>  
```

## <a name="change-the-frequency-of-sqlservr-resource-monitoring-interval"></a>Modificare la frequenza dell'intervallo di monitoraggio della risorsa sqlservr

```bash
sudo pcs    resource op monitor interval=<interval>s <sqlResourceName> 
```

L'esempio seguente imposta l'intervallo di monitoraggio su 2 secondi per la risorsa mssql:

```bash
sudo pcs    resource op monitor interval=2s mssqlha 
```
## <a name="troubleshoot-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Risolvere i problemi del cluster di dischi condivisi Red Hat Enterprise Linux per SQL Server

Per la risoluzione dei problemi del cluster, può essere utile conoscere il modo in cui i tre daemon interagiscono per gestire le risorse cluster. 

| Daemon | Descrizione 
| ----- | -----
| Corosync | Fornisce l'appartenenza al quorum e la messaggistica tra i nodi del cluster.
| Pacemaker | Si basa su Corosync e fornisce le macchine a stati per le risorse. 
| PCSD | Gestisce sia Pacemaker che Corosync tramite gli strumenti `pcs`

Per usare gli strumenti `pcs`, è necessario che PCSD sia in esecuzione. 

### <a name="current-cluster-status"></a>Stato corrente del cluster 

`sudo pcs status` restituisce le informazioni di base sullo stato di cluster, quorum, nodi, risorse e daemon per ogni nodo. 

Un esempio di output del quorum di Pacemaker integro è:

```
Cluster name: MyAppSQL 
Last updated: Wed Oct 31 12:00:00 2016  Last change: Wed Oct 31 11:00:00 2016 by root via crm_resource on sqlvmnode1 
Stack: corosync 
Current DC: sqlvmnode1  (version 1.1.13-10.el7_2.4-44eb2dd) - partition with quorum 
3 nodes and 1 resource configured 

Online: [ sqlvmnode1 sqlvmnode2 sqlvmnode3] 

Full list of resources: 

mssqlha (ocf::sql:fci): Started sqlvmnode1 

PCSD Status: 
sqlvmnode1: Online 
sqlvmnode2: Online 
sqlvmnode3: Online 

Daemon Status: 
corosync: active/disabled 
pacemaker: active/enabled 
```

Nell'esempio, `partition with quorum` indica che un quorum di maggioranza dei nodi è online. Se il cluster perde un quorum di maggioranza dei nodi, `pcs status` restituirà `partition WITHOUT quorum` e tutte le risorse verranno arrestate. 

`online: [sqlvmnode1 sqlvmnode2 sqlvmnode3]` restituisce il nome di tutti i nodi che attualmente partecipano al cluster. Se un nodo non partecipa, `pcs status` restituisce `OFFLINE: [<nodename>]`.

`PCSD Status` mostra lo stato del cluster per ogni nodo.

### <a name="reasons-why-a-node-may-be-offline"></a>Motivi per cui un nodo può essere offline

Quando un nodo è offline, controllare gli elementi seguenti.

- **Firewall**

    Le porte seguenti devono essere aperte in tutti i nodi perché Pacemaker riesca a comunicare.
    
    - **TCP: 2224, 3121, 21064

- **Servizi Pacemaker o Corosync in esecuzione**

- **Comunicazione tra nodi**

- **Mapping dei nomi dei nodi**

## <a name="additional-resources"></a>Risorse aggiuntive

* Guida di Pacemaker sui [nuovi cluster](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf)

## <a name="next-steps"></a>Passaggi successivi

[Configurare il cluster di dischi condivisi Red Hat Enterprise Linux per SQL Server](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)

