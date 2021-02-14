---
title: Configurare il cluster di dischi condivisi SLES per SQL Server
description: Implementare la disponibilità elevata configurando il cluster di dischi condivisi SLES (SUSE Linux Enterprise Server) per SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e5ad1bdd-c054-4999-a5aa-00e74770b481
ms.openlocfilehash: 87a4fd9edf52c3590fb19781fc5bf76c9bb0168b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346260"
---
# <a name="configure-sles-shared-disk-cluster-for-sql-server"></a>Configurare il cluster di dischi condivisi SLES per SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questa guida fornisce istruzioni per la creazione di un cluster di dischi condivisi a due nodi per SQL Server in SLES (SUSE Linux Enterprise Server). Il livello di clustering si basa su SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability), a sua volta basato su [Pacemaker](https://clusterlabs.org/). 

Per altre informazioni su configurazione dei cluster, opzioni degli agenti delle risorse, gestione, procedure consigliate e suggerimenti, vedere [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

## <a name="prerequisites"></a>Prerequisiti

Per completare lo scenario end-to-end seguente, sono necessari due computer per distribuire il cluster a due nodi e un altro server per configurare la condivisione NFS. I passaggi seguenti illustrano come verranno configurati questi server.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installare e configurare il sistema operativo in ogni nodo del cluster

Il primo passaggio consiste nel configurare il sistema operativo nei nodi del cluster. Per questa procedura dettagliata, usare SLES con una sottoscrizione valida per il componente aggiuntivo a disponibilità elevata.

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Installare e configurare SQL Server in ogni nodo del cluster

1. Installare e configurare SQL Server in entrambi i nodi. Per istruzioni dettagliate, vedere [Installare SQL Server in Linux](sql-server-linux-setup.md).
2. Designare un nodo come primario e l'altro come secondario, ai fini della configurazione. Usare questi termini per la parte seguente di questa guida. 
3. Nel nodo secondario arrestare e disabilitare SQL Server. L'esempio seguente arresta e disabilita SQL Server:

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE]
    > In fase di configurazione, viene generata una chiave master del server per l'istanza di SQL Server e viene inserita in `/var/opt/mssql/secrets/machine-key`. In Linux, SQL Server viene sempre eseguito come account locale denominato mssql. Poiché si tratta di un account locale, l'identità non è condivisa tra i nodi. È quindi necessario copiare la chiave di crittografia dal nodo primario a ogni nodo secondario, in modo che ogni account mssql locale possa accedervi per decrittografare la chiave master del server.
4. Nel nodo primario creare un account di accesso di SQL Server per Pacemaker e concedere l'autorizzazione di accesso per eseguire `sp_server_diagnostics`. Pacemaker usa questo account per verificare quale nodo sta eseguendo SQL Server.

    ```bash
    sudo systemctl start mssql-server
    ```
    Connettersi al database master di SQL Server con l'account "sa" ed eseguire i comandi seguenti:

    ```sql
    USE [master]
    GO
    CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
    GRANT VIEW SERVER STATE TO <loginName>
    ```
5. Nel nodo primario arrestare e disabilitare SQL Server.
6. Seguire le istruzioni [della documentazione SUSE](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) per configurare e aggiornare il file hosts per ogni nodo del cluster. Il file "hosts" deve includere l'indirizzo IP e il nome di ogni nodo del cluster.

    Per controllare l'indirizzo IP del nodo corrente, eseguire:

    ```bash
    sudo ip addr show
    ```

    Impostare il nome computer in ogni nodo. Assegnare a ogni nodo un nome univoco di 15 caratteri o meno. Per impostare il nome computer, aggiungerlo a `/etc/hostname` usando [yast](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) o [manualmente](https://www.suse.com/documentation/sled11/book_sle_admin/data/sec_basicnet_manconf.html).

    L'esempio seguente mostra `/etc/hosts` con l'aggiunta di due nodi denominati `SLES1` e `SLES2`.

    ```
    127.0.0.1   localhost
    10.128.18.128 SLES1
    10.128.16.77 SLES2
    ```

    > [!NOTE]
    > Tutti i nodi del cluster devono poter accedere l'uno all'altro tramite SSH. Strumenti come hb_report o crm_report (per la risoluzione dei problemi) e History Explorer di Hawk richiedono l'accesso a SSH tra i nodi senza password, in caso contrario possono raccogliere dati solo dal nodo corrente. Nel caso in cui si usi una porta SSH non standard, usare l'opzione -X. [Vedere la pagina di manuale](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html). Se, ad esempio, la porta SSH è 3479, richiamare un oggetto crm_report con:
    >
    >```bash
    >crm_report -X "-p 3479" [...]
    >```
    >Per altre informazioni, vedere la [guida all'amministrazione] (https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc) ).

Nella sezione successiva si configurerà la risorsa di archiviazione condivisa, in cui verranno spostati i file di database.  

## <a name="configure-shared-storage-and-move-database-files"></a>Configurare la risorsa di archiviazione condivisa e spostare i file di database

Sono disponibili diverse soluzioni per fornire spazio di archiviazione condiviso. Questa procedura dettagliata illustra la configurazione dello spazio di archiviazione condiviso con NFS. È opportuno seguire le procedure consigliate e usare Kerberos per proteggere NFS: 

- [Condivisione dei file system con NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.nfs)

Se non si segue questa guida, chiunque possa accedere alla rete ed effettuare lo spoofing dell'indirizzo IP di un nodo SQL, riuscirà ad accedere ai file di dati. Come sempre, assicurarsi di creare un modello di rischio per il sistema prima di usarlo nell'ambiente di produzione. 

Un'altra opzione di archiviazione consiste nell'usare la condivisione file SMB:

- [Sezione della documentazione SUSE relativa a Samba](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.samba)

### <a name="configure-an-nfs-server"></a>Configurare un server NFS

Per configurare un server NFS, vedere i passaggi seguenti nella documentazione di SUSE: [Configuring NFS Server](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-server) (Configurazione del server NFS).

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Configurare tutti i nodi del cluster per la connessione alla risorsa di archiviazione condivisa NFS

Prima di configurare il client NFS per montare il percorso dei file di database di SQL Server in modo da puntare alla posizione di archiviazione condivisa, assicurarsi di salvare i file di database in un percorso temporaneo per poterli copiare in un secondo momento nella condivisione:

1. **Solo nel nodo primario** salvare i file di database in una posizione temporanea. Lo script seguente crea una nuova directory temporanea, copia i file di database nella nuova directory e rimuove i file di database obsoleti. Poiché SQL Server viene eseguito come utente locale mssql, è necessario assicurarsi che dopo il trasferimento dei dati alla condivisione montata, l'utente locale abbia accesso in lettura/scrittura alla condivisione. 

    ```bash
    su mssql
    mkdir /var/opt/mssql/tmp
    cp /var/opt/mssql/data/* /var/opt/mssql/tmp
    rm /var/opt/mssql/data/*
    exit
    ```

    Configurare il client NFS in tutti i nodi del cluster:

    - [Configurazione dei client](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-clients)

    > [!NOTE]
    > È opportuno seguire le procedure consigliate e le raccomandazioni di SUSE relative alla risorsa di archiviazione NFS a disponibilità elevata: [Highly Available NFS Storage with DRBD and Pacemaker](https://www.suse.com/documentation/sle-ha-12/book_sleha_techguides/data/art_ha_quick_nfs.html) (Risorsa di archiviazione NFS a disponibilità elevata con DRBD e Pacemaker).

2. Verificare che SQL Server venga avviato correttamente con il nuovo percorso del file. Eseguire questa operazione in ogni nodo. A questo punto, un solo nodo alla volta eseguirà SQL Server. Non possono essere eseguiti contemporaneamente perché entrambi proveranno ad accedere ai file di dati simultaneamente. Per evitare l'avvio accidentale di SQL Server in entrambi i nodi, usare una risorsa cluster del file system per assicurarsi che la condivisione non venga montata due volte da nodi diversi. I comandi seguenti avviano SQL Server, controllano lo stato e quindi arrestano SQL Server.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    sudo systemctl stop mssql-server
    ```

A questo punto, entrambe le istanze di SQL Server sono configurate per l'esecuzione con i file di database nello spazio di archiviazione condiviso. Il passaggio successivo consiste nel configurare SQL Server per Pacemaker. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installare e configurare Pacemaker in ogni nodo del cluster

1. **In entrambi i nodi del cluster creare un file per archiviare nome utente e password di SQL Server per l'accesso a Pacemaker**. Il comando seguente crea e popola questo file:

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd
    ```
2. **Tutti i nodi del cluster devono essere in grado di accedere uno all'altro tramite SSH**. Strumenti come hb_report o crm_report (per la risoluzione dei problemi) e History Explorer di Hawk richiedono l'accesso a SSH tra i nodi senza password, in caso contrario possono raccogliere dati solo dal nodo corrente. Nel caso in cui si usi una porta SSH non standard, usare l'opzione -X (vedere la pagina relativa a man). Se, ad esempio, la porta SSH è 3479, richiamare un oggetto hb_report con:

    ```bash
    crm_report -X "-p 3479" [...]
    ```

    Per altre informazioni, vedere i [requisiti di sistema e consigli nella documentazione di SUSE](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html).

3. **Installare l'estensione per la disponibilità elevata**. Per installare l'estensione, seguire la procedura nell'argomento SUSE seguente:
    
    [Installation and Setup Quick Start](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html) (Guida introduttiva all'installazione e alla configurazione)

4. **Installare l'agente delle risorse FCI per SQL Server**. Eseguire i comandi seguenti in entrambi i nodi:

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
    sudo zypper --gpg-auto-import-keys refresh
    sudo zypper install mssql-server-ha
    ```

5. **Configurare automaticamente il primo nodo**. Il passaggio successivo consiste nel configurare un cluster a un nodo in esecuzione configurando il primo nodo, SLES1. Seguire le istruzioni nell'argomento SUSE [Setting Up the First Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node) (Configurazione del primo nodo).

    Al termine, verificare lo stato del cluster con `crm status`:
    ```bash
    crm status
    ```

    Dovrebbe indicare che il nodo, SLES1, è configurato.

6. **Aggiungere nodi a un cluster esistente**. Aggiungere quindi il nodo SLES2 al cluster. Seguire le istruzioni nell'argomento SUSE [Adding the Second Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.2nd-node) (Aggiunta del secondo nodo).
    
    Al termine, verificare lo stato del cluster con **crm status**. Se il secondo nodo è stato aggiunto correttamente, l'output sarà simile al seguente:
        
    ```
    2 nodes configured
    1 resource configured
    Online: [ SLES1 SLES2 ]
    Full list of resources:
    admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
    ```

    > [!NOTE]
    > **admin_addr** è la risorsa cluster IP virtuale configurata durante l'installazione iniziale del cluster a un nodo.

7.  **Procedure di rimozione**. Se si vuole rimuovere un nodo dal cluster, usare lo script di bootstrap **ha-cluster-remove**. Per altre informazioni, vedere [Overview of the Bootstrap Scripts](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.bootstrap) (Panoramica degli script di bootstrap).  

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configurare le risorse cluster per SQL Server

I passaggi seguenti illustrano come configurare la risorsa cluster per SQL Server. Sono presenti due impostazioni che è necessario personalizzare.

- **SQL Server Resource Name** (Nome della risorsa di SQL Server): nome della risorsa di SQL Server in cluster. 
- **Valore timeout**: il valore di timeout indica per quanto tempo il cluster attende mentre una risorsa viene portata online. Per SQL Server, corrisponde al tempo previsto perché SQL Server porti online il database `master`. 

Aggiornare i valori dello script seguente per il proprio ambiente. Eseguire su un nodo per configurare e avviare il servizio in cluster.

```bash
sudo crm configure
primitive <sqlServerResourceName> ocf:mssql:fci op start timeout=<timeout_in_seconds>
colocation <constraintName> inf: <virtualIPResourceName> <sqlServerResourceName>
show
commit
exit
```

Lo script seguente, ad esempio, crea una risorsa in cluster di SQL Server denominata mssqlha. 

```bash
sudo crm configure
primitive mssqlha ocf:mssql:fci op start timeout=60s
colocation admin_addr_mssqlha inf: admin_addr mssqlha
show
commit
exit
```

Una volta eseguito il commit della configurazione, SQL Server verrà avviato nello stesso nodo della risorsa IP virtuale. 

Per altre informazioni, vedere [Configuring and Managing Cluster Resources (Command Line)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config) (Configurazione e gestione delle risorse cluster - Riga di comando). 

### <a name="verify-that-sql-server-is-started"></a>Verificare che SQL Server sia avviato. 

Per verificare che SQL Server sia avviato, eseguire il comando **crm status**:

```bash
crm status
```

Gli esempi seguenti illustrano i risultati quando Pacemaker è stato avviato correttamente come risorsa cluster. 
```
2 nodes configured
2 resources configured

Online: [ SLES1 SLES2 ]

Full list of resources:

 admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
 mssqlha        (ocf::mssql:fci):       Started SLES1
```

## <a name="managing-cluster-resources"></a>Gestione di risorse cluster

Per gestire le risorse cluster, vedere l'argomento SUSE seguente: [Managing Cluster Resources](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm ) (Gestione di risorse cluster)

### <a name="manual-failover"></a>Failover manuale

Nonostante le risorse siano configurate per effettuare automaticamente il failover (o la migrazione) ad altri nodi del cluster in caso di errore hardware o software, è anche possibile spostare manualmente una risorsa in un altro nodo del cluster usando l'interfaccia utente grafica di Pacemaker o la riga di comando. 

Per questa attività usare il comando migrate. Ad esempio, per eseguire la migrazione della risorsa SQL a un nodo del cluster denominato SLES2, eseguire: 

```bash
crm resource
migrate mssqlha SLES2
```

## <a name="additional-resources"></a>Risorse aggiuntive

[SUSE Linux Enterprise High Availability Extension - Administration Guide](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html) (SUSE Linux Enterprise High Availability Extension - Guida all'amministrazione) 
