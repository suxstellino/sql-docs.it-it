---
title: Distribuire un gruppo di disponibilità con HPE Serviceguard-- SQL Server in Linux
description: Usare HPE Serviceguard come gestione cluster per ottenere la disponibilità elevata con un gruppo di disponibilità in SQL Server in Linux
ms.date: 01/15/2021
ms.prod: sql
ms.technology: linux
ms.topic: tutorial
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.openlocfilehash: 3956c0470ac9f4b3ac2f2a35ed057015db6ea0e0
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534895"
---
# <a name="tutorial---setup-a-three-node-always-on-availability-group-with-hpe-serviceguard-for-linux"></a>Esercitazione - Configurare un gruppo di disponibilità Always On a tre nodi con HPE Serviceguard per Linux 

Questa esercitazione illustra come configurare un gruppo di disponibilità Always On di SQL Server con HPE Serviceguard per Linux in esecuzione in macchine virtuali locali.

Per una panoramica dei cluster HPE Serviceguard, vedere [HPE Serviceguard Clusters](https://h20195.www2.hpe.com/v2/GetPDF.aspx/c04154488.pdf) (Cluster di HPE Serviceguard).

> [!NOTE]
> Microsoft offre supporto per lo spostamento dei dati, il gruppo di disponibilità e i componenti di SQL Server. Contattare HPE per assistenza relativa alla documentazione per i cluster HPE Serviceguard e la gestione del quorum.

Questa esercitazione è costituita dalle attività seguenti:

> [!div class="checklist"]
> * Installare SQL Server in tutte e tre le macchine virtuali che faranno parte del gruppo di disponibilità
> * Installare HPE Serviceguard nelle macchine virtuali
> * Creare il cluster HPE Serviceguard
> * Creare il gruppo di disponibilità e aggiungere un database di esempio al gruppo di disponibilità
> * Distribuire il carico di lavoro di SQL Server nel gruppo di disponibilità tramite la gestione cluster di Serviceguard
> * Eseguire un failover automatico e aggiungere di nuovo il nodo al cluster

## <a name="prerequisites"></a>Prerequisiti

* Tre macchine virtuali in un ambiente locale che eseguono HPE Serviceguard in una distribuzione di Linux supportata.

   Per un esempio di distribuzione supportata, vedere [HPE Serviceguard for Linux](https://h20195.www2.hpe.com/v2/gethtml.aspx?docname=c04154488).

   Per informazioni sul supporto per gli ambienti cloud pubblici, rivolgersi a HPE.

   Le istruzioni in questa esercitazione sono verificate in HPE Serviceguard per Linux. È possibile scaricare una versione di valutazione da [HPE](https://www.hpe.com/us/en/resources/servers/serviceguard-linux-trial.html).

* File di database di SQL Server in una partizione LVM (Logical Volume Mount) per tutte e tre le macchine virtuali. Vedere [Quick start guide for Serviceguard Linux (HPE)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us) (Guida introduttiva per Serviceguard Linux - HPE)

* Assicurarsi che sia installato un runtime Java OpenJDK nelle macchine virtuali.

   > [!NOTE]
   > IBM java sdk non è supportato.

## <a name="install-sql-server"></a>Installare SQL Server

In tutte e tre le macchine virtuali, seguire uno dei passaggi seguenti in base alla distribuzione di Linux scelta per questa esercitazione, per installare SQL Server e gli strumenti.

### <a name="rhel"></a>RHEL

* [Installare SQL Server in Red Hat](quickstart-install-connect-red-hat.md#install)
* [Strumenti](quickstart-install-connect-red-hat.md#tools)

### <a name="sles"></a>SLES

* [Installare SQL Server in SLES](quickstart-install-connect-suse.md#install)
* [Strumenti](quickstart-install-connect-suse.md#tools)

Dopo aver completato questo passaggio, il servizio SQL Server e gli strumenti dovrebbero essere installati in tutte e tre le macchine virtuali che parteciperanno al gruppo di disponibilità.

## <a name="install-hpe-serviceguard-on-the-vms"></a>Installare HPE Serviceguard nelle macchine virtuali

In questo passaggio, installare HPE Serviceguard per Linux in tutte e tre le macchine virtuali. Nella tabella seguente viene descritto il ruolo svolto da ogni server nel cluster.

|Numero di macchine virtuali | Ruolo di HPE Serviceguard | Ruolo della replica del gruppo di disponibilità di Microsoft SQL Server|
|--------------|-----------------|------------|
|1 | Nodi del cluster HPE Serviceguard | Replica primaria |
|1 o più | Nodo del cluster HPE Serviceguard | Replica secondaria |
|1 | Server quorum HPE Serviceguard | Replica di sola configurazione |

Per installare Serviceguard, usare il metodo `cminstaller`. Sono disponibili istruzioni specifiche nei collegamenti seguenti:

Cluster Serviceguard e server quorum Serviceguard

* [Installare Serviceguard per Linux in due nodi](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_serviceguard_using_cminstaller). Vedere la sezione **Install_serviceguard_using_cminstaller**.
* [Installare il server quorum Serviceguard nel terzo nodo](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_QS_from_the_ISO). Vedere la sezione **Install_QS_from_the_ISO**.

Dopo aver completato l'installazione del cluster HPE Serviceguard, è possibile abilitare il portale di gestione del cluster sulla porta TCP 5522 nel nodo della replica primaria. La procedura seguente aggiunge una regola al firewall per consentire la porta 5522. Il comando seguente è per una distribuzione RHEL ed è necessario eseguire comandi simili per altre distribuzioni:

```console
sudo firewall-cmd --zone=public --add-port=5522/tcp --permanent

sudo firewall-cmd --reload 
```

## <a name="create-hpe-serviceguard-cluster"></a>Creare un cluster HPE Serviceguard

Seguire le istruzioni riportate di seguito per configurare e creare il cluster HPE Serviceguard. In questo passaggio verrà anche configurato il server quorum.

1. [Configurare il server quorum Serviceguard nel terzo nodo](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_QS). Vedere la sezione **Configure_QS**.
2. [Configurare e creare un cluster Serviceguard negli altri due nodi](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_and_create_cluster). Vedere la sezione **Configure_and_create_Cluster**.

## <a name="create-the-availability-group-and-add-a-sample-database"></a>Creare il gruppo di disponibilità e aggiungere un database di esempio

In questo passaggio creare un gruppo di disponibilità con due (o più) repliche sincrone e una replica di sola configurazione, che supporta la protezione dei dati e può anche offrire disponibilità elevata. Nel diagramma seguente è illustrata questa architettura:

:::image type="content" source="media/sql-server-linux-availability-group-ha/2-configuration-only.png" alt-text="La replica primaria sincronizza i dati utente e i dati di configurazione con la replica secondaria. La replica primaria sincronizza solo i dati di configurazione con la replica di sola configurazione. La replica di sola configurazione non ha repliche dei dati utente.":::

1. Replica sincrona dei dati utente nella replica secondaria. Sono inclusi anche i metadati di configurazione del gruppo di disponibilità.

2. Replica sincrona dei metadati di configurazione del gruppo di disponibilità. Non sono inclusi i dati utente.

Per altri dettagli, vedere [Due repliche sincrone e una replica di sola configurazione](sql-server-linux-availability-group-ha.md).

Per creare il gruppo di disponibilità, seguire questa procedura:

1. [Abilitare la funzionalità Gruppi di disponibilità Always On e riavviare mssql-server](#enable-always-on-availability-groups-and-restart-mssql-server) su tutte le macchine virtuali, inclusa la replica di sola configurazione.
2. [Abilitare una sessione eventi `AlwaysOn_health` - (facoltativo)](#enable-an-alwayson_health-event-session---optional)
3. [Creare un certificato nella macchina virtuale primaria](#create-a-certificate-on-the-primary-vm)
4. [Creare il certificato nei server secondari](#create-the-certificate-on-secondary-servers)
5. [Creare gli endpoint di mirroring del database per le repliche](#create-the-database-mirroring-endpoints-on-the-replicas)
6. [Creare un gruppo di disponibilità](#create-availability-group)
7. [Aggiungere le repliche secondarie](#join-the-secondary-replicas)
8. [Aggiungere un database al gruppo di disponibilità creato in precedenza](#add-a-database-to-the-availability-group-created-above)

### <a name="enable-always-on-availability-groups-and-restart-mssql-server"></a>Abilitare la funzionalità Gruppi di disponibilità Always On e riavviare mssql-server

Abilitare Always On in tutti i nodi che ospitano un'istanza di SQL Server. Riavviare quindi mssql-server. Eseguire lo script seguente in tutti e tre i nodi:

```console
sudo /opt/mssql/bin/mssql-conf
set hadr.hadrenabled 1 sudo systemctl restart mssql-serve
```

### <a name="enable-an-alwayson_health-event-session---optional"></a>Abilitare una sessione eventi `AlwaysOn_health` -(facoltativo)

Abilitare facoltativamente gli eventi estesi dei gruppi di disponibilità AlwaysOn per diagnosticare più facilmente la causa radice durante la risoluzione dei problemi che interessano un gruppo di disponibilità. Eseguire il comando seguente in ogni istanza di SQL Server:

```tsql
ALTER EVENT SESSION AlwaysOn_health ON SERVER WITH (STARTUP_STATE=ON);
GO
```

### <a name="create-a-certificate-on-the-primary-vm"></a>Creare un certificato nella macchina virtuale primaria

Lo script Transact-SQL seguente crea una chiave master e un certificato. Quindi esegue il backup del certificato e protegge il file con una chiave privata. Aggiornare lo script con password complesse. Connettersi all'istanza di SQL Server primaria ed eseguire lo script Transact-SQL seguente:

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Master_Key_Password';

CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';

BACKUP CERTIFICATE dbm_certificate TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY 
    ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
      ENCRYPTION BY PASSWORD = '<Private_Key_Password>' );

```

A questo punto la replica di SQL Server primaria ha un certificato in `/var/opt/mssql/data/dbm_certificate.cer` e una chiave privata in `var/opt/mssql/data/dbm_certificate.pvk`. Copiare questi due file nello stesso percorso in tutti i server che ospiteranno le repliche di disponibilità. Usare l'utente mssql o concedere l'autorizzazione all'utente mssql per questi file.

Nel server di origine, ad esempio, il comando seguente copia i file nel computer di destinazione. Sostituire i valori di `'node2'` con il nome dell'host che esegue l'istanza di SQL Server secondaria. Copiare il certificato anche nella replica di sola configurazione ed eseguire anche i comandi seguenti in tale nodo.

```console
cd /var/opt/mssql/data
scp dbm_certificate.* root@<node2>:/var/opt/mssql/data/
```

A questo punto, nelle macchine virtuali secondarie che eseguono l'istanza secondaria e la replica di sola configurazione di SQL Server eseguire i comandi seguenti, in modo che l'utente mssql possa diventare proprietario del certificato copiato:

```console
cd /var/opt/mssql/data

chown mssql:mssql dbm_certificate.*
```

### <a name="create-the-certificate-on-secondary-servers"></a>Creare il certificato nei server secondari

Lo script Transact-SQL seguente crea una chiave master e un certificato dal backup creato nella replica primaria di SQL Server. Aggiornare lo script con password complesse. La password di decrittografia è la stessa password utilizzata per creare il file .pvk in un passaggio precedente. Per creare il certificato, eseguire lo script seguente in tutti i server secondari ad eccezione della replica di sola configurazione:

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD =
'<Master_Key_Password>';

CREATE CERTIFICATE dbm_certificate FROM FILE =
'/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
DECRYPTION BY PASSWORD = '<Private_Key_Password>' );
```

### <a name="create-the-database-mirroring-endpoints-on-the-replicas"></a>Creare gli endpoint di mirroring del database per le repliche

Nella replica primaria e nella replica secondaria eseguire i comandi seguenti per creare gli endpoint di mirroring del database:

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING 
        (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

> [!NOTE]
> 5022 è la porta standard usata per l'endpoint di mirroring del database, ma è possibile sostituirla con qualsiasi porta disponibile.

Nella replica di sola configurazione creare l'endpoint di mirroring del database usando il comando seguente. Si noti che il valore del ruolo qui è impostato su `WITNESS`, ovvero l'impostazione necessaria per la replica di sola configurazione.

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

### <a name="create-availability-group"></a>Creare un gruppo di disponibilità

Nell'istanza della replica primaria eseguire i comandi seguenti. Questi comandi creano un gruppo di disponibilità denominato **ag1** con `cluster_type` **External** e concede l'autorizzazione per la creazione di database al gruppo di disponibilità.

Prima di eseguire gli script seguenti, sostituire i segnaposto `<node1>`, `<node2>` e `<node3>` (replica di sola configurazione) con il nome delle macchine virtuali create nei passaggi precedenti.

```tsql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR REPLICA ON
    N'<node1>' WITH (
        ENDPOINT_URL = N'tcp://<node1>:<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),

    N'<node2>' WITH (
        ENDPOINT_URL = N'tcp://<node2>:\<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),
    
    N'<node3>' WITH (
        ENDPOINT_URL = N'tcp://<node3>:<5022>',
        AVAILABILITY_MODE = CONFIGURATION_ONLY
        );
          
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-the-secondary-replicas"></a>Aggiungere le repliche secondarie

Eseguire i comandi riportati di seguito in tutte le repliche secondarie. Questi comandi aggiungono le repliche secondarie al gruppo di disponibilità "ag1" con la replica primaria e forniscono le autorizzazioni per la creazione di database al gruppo di disponibilità ag1.

```tsql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
GO
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
GO
```

### <a name="add-a-database-to-the-availability-group-created-above"></a>Aggiungere un database al gruppo di disponibilità creato in precedenza

Connettersi alla replica primaria ed eseguire i comandi T-SQL seguenti per:

1. Creare un database di esempio denominato **db1** che verrà aggiunto al gruppo di disponibilità.
2. Impostare il modello di recupero con registrazione completa per il database. Per tutti i database in un gruppo di disponibilità è richiesto il modello di recupero con registrazione completa.
3. Eseguire il backup del database. Prima di poter aggiungere un database a un gruppo di disponibilità, è necessario almeno un backup completo.

```tsql
CREATE DATABASE [db1]; -- creates a database named db1
GO
ALTER DATABASE [db1] SET RECOVERY FULL; -- set the database in full recovery mode
GO
BACKUP DATABASE [db1] -- backs up the database to disk TO DISK = N'/var/opt/mssql/data/db1.bak';
GO
ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1]; -- adds the database db1 to the AG
GO
```

Dopo aver completato i passaggi precedenti, è possibile visualizzare il gruppo di disponibilità **ag1** creato e le tre macchine virtuali vengono aggiunte come replica con una replica primaria, una replica secondaria e una replica di sola configurazione. **ag1** contiene un solo database.

## <a name="deploy-the-sql-server-availability-group-workload-hpe-cluster-manager"></a>Distribuire il carico di lavoro del gruppo di disponibilità di SQL Server (gestione cluster HPE)

In HPE Serviceguard distribuire il carico di lavoro di SQL Server in un gruppo di disponibilità tramite l'interfaccia utente di gestione cluster di Serviceguard.
   
Distribuire il carico di lavoro del gruppo di disponibilità e abilitare la disponibilità elevata e il ripristino di emergenza tramite il cluster Serviceguard usando l'[interfaccia utente grafica di gestione di Serviceguard](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Protect_your_alwayson_availability_group). Vedere la sezione **Protezione di Microsoft SQL Server in Linux per i gruppi di disponibilità Always On**.

## <a name="perform-automatic-failover-and-join-the-node-back-to-cluster"></a>Eseguire il failover automatico e aggiungere di nuovo il nodo al cluster

Per il test di failover automatico, è possibile procedere e arrestare la replica primaria (spegnimento) per replicare la mancata disponibilità improvvisa del nodo primario. Questo è il comportamento previsto:

1. La gestione cluster alza di livello una delle repliche secondarie nel gruppo di disponibilità a replica primaria.
2. La replica primaria in errore viene aggiunta automaticamente al cluster dopo il backup. La gestione cluster la alza di livello a replica secondaria.

Per HPE Serviceguard, vedere la sezione [**Testing the setup for failover readiness**](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Test_the_setup_preparedness) (Testare la configurazione per la conformità del failover)

## <a name="next-steps"></a>Passaggi successivi

Vedere altre informazioni sui [gruppi di disponibilità Always On in Linux](sql-server-linux-availability-group-overview.md).
