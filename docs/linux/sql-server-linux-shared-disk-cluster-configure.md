---
title: Configurare un'istanza del cluster di failover - SQL Server in Linux (RHEL)
description: Informazioni su come configurare un'istanza del cluster di failover in Red Hat Enterprise Linux (RHEL) per SQL Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: b2b97fa250b5af1a0030b51b0ea29a89324db9c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346300"
---
# <a name="configure-failover-cluster-instance---sql-server-on-linux-rhel"></a>Configurare un'istanza del cluster di failover - SQL Server in Linux (RHEL)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Un'istanza del cluster di failover su disco condiviso a due nodi di SQL Server fornisce ridondanza a livello di server per la disponibilità elevata. In questa esercitazione si apprenderà come creare un'istanza del cluster di failover a due nodi di SQL Server in Linux. I passaggi specifici che si completeranno includono:

> [!div class="checklist"]
> * Installare e configurare Linux
> * Installare e configurare SQL Server
> * Configurare il file hosts
> * Configurare la risorsa di archiviazione condivisa e spostare i file di database
> * Installare e configurare Pacemaker in ogni nodo del cluster
> * Configurare l'istanza del cluster di failover

Questo articolo illustra come creare un'istanza del cluster di failover su disco condiviso a due nodi per SQL Server. L'articolo include istruzioni ed esempi di script per Red Hat Enterprise Linux (RHEL). Le distribuzioni di Ubuntu sono simili a quelle di RHEL, quindi gli esempi di script funzionano anche in Ubuntu. 

Per informazioni di carattere generale, vedere [Istanza del cluster di failover di SQL Server in Linux](sql-server-linux-shared-disk-cluster-concepts.md).

## <a name="prerequisites"></a>Prerequisiti

Per completare lo scenario end-to-end seguente, sono necessari due computer per distribuire il cluster a due nodi e un altro server per la risorsa di archiviazione. I passaggi seguenti illustrano come verranno configurati questi server.

## <a name="set-up-and-configure-linux"></a>Installare e configurare Linux

Il primo passaggio consiste nel configurare il sistema operativo nei nodi del cluster. In ogni nodo del cluster configurare una distribuzione di Linux. Usare la stessa distribuzione e la stessa versione in entrambi i nodi. Usare una o l'altra delle distribuzioni seguenti:
    
* RHEL con una sottoscrizione valida per il componente aggiuntivo per la disponibilità elevata

## <a name="install-and-configure-sql-server"></a>Installare e configurare SQL Server

1. Installare e configurare SQL Server in entrambi i nodi.  Per istruzioni dettagliate, vedere [Installare SQL Server in Linux](sql-server-linux-setup.md).
1. Designare un nodo come primario e l'altro come secondario, ai fini della configurazione. Usare questi termini per la parte seguente di questa guida.  
1. Nel nodo secondario arrestare e disabilitare SQL Server.
    L'esempio seguente arresta e disabilita SQL Server: 
    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE] 
    > In fase di configurazione, viene generata una chiave master del server per l'istanza di SQL Server e viene inserita in `var/opt/mssql/secrets/machine-key`. In Linux, SQL Server viene sempre eseguito come account locale denominato mssql. Poiché si tratta di un account locale, l'identità non è condivisa tra i nodi. È quindi necessario copiare la chiave di crittografia dal nodo primario a ogni nodo secondario, in modo che ogni account mssql locale possa accedervi per decrittografare la chiave master del server. 

1.  Nel nodo primario creare un account di accesso di SQL Server per Pacemaker e concedere l'autorizzazione di accesso per eseguire `sp_server_diagnostics`. Pacemaker usa questo account per verificare quale nodo sta eseguendo SQL Server. 

    ```bash
    sudo systemctl start mssql-server
    ```
   
   Connettersi al database `master` di SQL Server con l'account sa ed eseguire i comandi seguenti:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```

   In alternativa, è possibile impostare le autorizzazioni a un livello più granulare. L'account di accesso di Pacemaker richiede `VIEW SERVER STATE` per effettuare una query sullo stato di integrità con sp_server_diagnostics, `setupadmin` e `ALTER ANY LINKED SERVER` per aggiornare il nome dell'istanza del cluster di failover con il nome della risorsa eseguendo sp_dropserver e sp_addserver. 

1. Nel nodo primario arrestare e disabilitare SQL Server. 

## <a name="configure-the-hosts-file"></a>Configurare il file hosts

In ogni nodo del cluster configurare il file hosts. Il file hosts deve includere l'indirizzo IP e il nome di ogni nodo del cluster.

1. Controllare l'indirizzo IP per ogni nodo. Lo script seguente mostra l'indirizzo IP del nodo corrente. 

    ```bash
    sudo ip addr show
    ```

1. Impostare il nome computer in ogni nodo. Assegnare a ogni nodo un nome univoco di 15 caratteri o meno. Per impostare il nome computer, aggiungerlo a `/etc/hosts`. Lo script seguente consente di modificare `/etc/hosts` con `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```
   L'esempio seguente mostra `/etc/hosts` con l'aggiunta di due nodi denominati `sqlfcivm1` e `sqlfcivm2`.

   ```bash
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1       localhost localhost6 localhost6.localdomain6
   10.128.18.128 sqlfcivm1
   10.128.16.77 sqlfcivm2
   ```

## <a name="configure-storage--move-database-files"></a>Configurare la risorsa di archiviazione e spostare i file di database  

È necessario fornire una risorsa di archiviazione a cui possano accedere entrambi i nodi. È possibile usare iSCSI, NFS o SMB. Configurare la risorsa di archiviazione, presentarla ai nodi del cluster e quindi spostare i file di database nella nuova risorsa di archiviazione. Gli articoli seguenti illustrano i passaggi per ogni tipo di archiviazione:

- [Configurare un'istanza del cluster di failover - iSCSI - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configurare un'istanza del cluster di failover - NFS - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configurare un'istanza del cluster di failover - SMB - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure-smb.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installare e configurare Pacemaker in ogni nodo del cluster

1. In entrambi i nodi del cluster creare un file per archiviare nome utente e password di SQL Server per l'accesso a Pacemaker. 

    Il comando seguente crea e popola questo file:

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd    
    ```

1. In entrambi i nodi del cluster aprire le porte del firewall di Pacemaker. Per aprire queste porte con `firewalld`, eseguire il comando seguente:

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > Se si sta usando un altro firewall che non ha una configurazione a disponibilità elevata predefinita, è necessario aprire le porte seguenti per consentire a Pacemaker di comunicare con altri nodi del cluster
   >
   > * TCP: porte 2224, 3121, 21064
   > * UDP: porta 5405

1. Installare i pacchetti Pacemaker in ogni nodo.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
1. Impostare la password per l'utente predefinito creato durante l'installazione dei pacchetti Pacemaker e Corosync. Usare la stessa password in entrambi i nodi. 

   ```bash
   sudo passwd hacluster
   ```
1. Abilitare e avviare il servizio `pcsd` e Pacemaker. In questo modo, i nodi potranno unirsi nuovamente in join con il cluster dopo il riavvio. Eseguire il comando seguente in entrambi i nodi.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

1. Installare l'agente delle risorse FCI per SQL Server. Eseguire i comandi seguenti in entrambi i nodi. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-the-failover-cluster-instance"></a>Configurare l'istanza del cluster di failover

L'istanza del cluster di failover verrà creata in un gruppo di risorse. Si tratta di un approccio un po' più semplice perché il gruppo di risorse riduce la necessità di vincoli. Aggiungere tuttavia le risorse nel gruppo di risorse nell'ordine in cui devono essere avviate. L'ordine di avvio è il seguente: 

1. Risorsa di archiviazione
2. Risorsa di rete
3. Risorsa dell'applicazione

Questo esempio creerà un'istanza del cluster di failover nel gruppo NewLinFCIGrp. Il nome del gruppo di risorse deve essere univoco rispetto a qualsiasi risorsa creata in Pacemaker.

1.  Creare la risorsa disco. Se non si verificano problemi, non verrà restituita alcuna risposta. Il modo in cui creare la risorsa disco dipende dal tipo di archiviazione. Di seguito è riportato un esempio per ogni tipo di archiviazione. Usare l'esempio che si applica al tipo dell'archiviazione in cluster.

    **iSCSI**

    ```bash
    sudo pcs resource create <iSCSIDiskResourceName> Filesystem device="/dev/<VolumeGroupName>/<LogicalVolumeName>" directory="<FolderToMountiSCSIDisk>" fstype="<FileSystemType>" --group RGName
    ```

    \<iSCSIDIskResourceName> è il nome della risorsa associata al disco iSCSI

    \<VolumeGroupName> è il nome del gruppo di volumi  

    \<LogicalVolumeName> è il nome del volume logico creato  

    \<FolderToMountiSCSIDIsk> è la cartella in cui montare il disco (per i database di sistema e il percorso predefinito, sarà /var/opt/mssql/data)

    \<FileSystemType> sarà EXT4 o XFS a seconda della formattazione scelta e degli elementi supportati dalla distribuzione. 

    **NFS**

    ```bash
    sudo pcs resource create <NFSDiskResourceName> Filesystem device="<IPAddressOfNFSServer>:<FolderOnNFSServer>" directory="<FolderToMountNFSShare>" fstype=nfs4 options=" nfsvers=4.2,timeo=14,intr" --group RGName
    mount -t nfs4 IPAddressOfNFSServer:FolderOnNFSServer /var/opt/mssql/data -o 
    ```

    \<NFSDIskResourceName> è il nome della risorsa associata alla condivisione NFS

    \<IPAddressOfNFSServer> è l'indirizzo IP del server NFS da usare

    \<FolderOnNFSServer> è il nome della condivisione NFS

    \<FolderToMountNFSShare> è la cartella in cui montare il disco (per i database di sistema e il percorso predefinito, sarà /var/opt/mssql/data)

    Di seguito è riportato un esempio:

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    **SMB**

    ```bash
    sudo pcs resource create SMBDiskResourceName Filesystem device="//<ServerName>/<ShareName>" directory="<FolderName>" fstype=cifs options="vers=3.0,username=<UserName>,password=<Password>,domain=<ADDomain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777" --group <RGName>
    ```

    \<ServerName> è il nome del server con la condivisione SMB

    \<ShareName> è il nome della condivisione

    \<FolderName> è il nome della cartella creata nell'ultimo passaggio
    
    \<UserName> è il nome dell'utente con cui accedere alla condivisione

    \<Password> è la password dell'utente

    \<ADDomain> è il dominio Active Directory Domain Services (se applicabile quando si usa una condivisione SMB basata su Windows Server)

    \<mssqlUID> è l'UID dell'utente mssql

    \<mssqlGID> è il GID dell'utente mssql

    \<RGName> è il nome del gruppo di risorse
 
2.  Creare l'indirizzo IP che verrà usato dall'istanza del cluster di failover. Se non si verificano problemi, non verrà restituita alcuna risposta.

    ```bash
    sudo pcs resource create <IPResourceName> ocf:heartbeat:IPaddr2 ip=<IPAddress> nic=<NetworkCard> cidr_netmask=<NetMask> --group <RGName>
    ```

    \<IPResourceName> è il nome della risorsa associata all'indirizzo IP

    \<IPAddress> è l'indirizzo IP per l'istanza del cluster di failover

    \<NetworkCard> è la scheda di rete associata alla subnet (ad esempio, eth0)

    \<NetMask> è la netmask della subnet (ad esempio, 24)

    \<RGName> è il nome del gruppo di risorse
 
3.  Creare la risorsa istanza del cluster di failover. Se non si verificano problemi, non verrà restituita alcuna risposta.

    ```bash
    sudo pcs resource create FCIResourceName ocf:mssql:fci op defaults timeout=60s --group RGName
    ```

    \<FCIResourceName> non è solo il nome della risorsa, ma anche il nome descrittivo associato all'istanza del cluster di failover che gli utenti e le applicazioni useranno per connettersi. 

    \<RGName> è il nome del gruppo di risorse.
 
4.  Eseguire il comando `sudo pcs resource`. L'istanza del cluster di failover deve essere online.
 
5.  Connettersi all'istanza del cluster di failover con SSMS o sqlcmd usando il nome DNS o della risorsa dell'istanza del cluster di failover.

6.  Eseguire l'istruzione `SELECT @@SERVERNAME`, che deve restituire il nome dell'istanza del cluster di failover.

7.  Eseguire l'istruzione `SELECT SERVERPROPERTY('ComputerNamePhysicalNetBIOS')`, che deve restituire il nome del nodo in cui è in esecuzione l'istanza del cluster di failover.

8.  Eseguire il failover manuale dell'istanza del cluster di failover negli altri nodi. Vedere le istruzioni in [Gestire un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-operate.md).

9.  Eseguire infine il failback dell'istanza del cluster di failover nel nodo originale e rimuovere il vincolo di condivisione del percorso.

<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

-->
## <a name="summary"></a>Summary

In questa esercitazione sono state completate le attività seguenti.

> [!div class="checklist"]
> * Installare e configurare Linux
> * Installare e configurare SQL Server
> * Configurare il file hosts
> * Configurare la risorsa di archiviazione condivisa e spostare i file di database
> * Installare e configurare Pacemaker in ogni nodo del cluster
> * Configurare l'istanza del cluster di failover

## <a name="next-steps"></a>Passaggi successivi

- [Gestire un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-operate.md)

<!--Image references-->
