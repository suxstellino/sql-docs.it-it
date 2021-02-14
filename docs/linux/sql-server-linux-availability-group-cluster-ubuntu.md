---
title: Configurare un cluster Ubuntu per un gruppo di disponibilità
titleSuffix: SQL Server on Linux
description: Informazioni su come creare un cluster a tre nodi in Ubuntu e come aggiungere al cluster una risorsa del gruppo di disponibilità creata in precedenza.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dd0d6fb9-df0a-41b9-9f22-9b558b2b2233
ms.openlocfilehash: 13b719848e9cbced670e58ae3a747967a8901ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338311"
---
# <a name="configure-ubuntu-cluster-and-availability-group-resource"></a>Configurare un cluster Ubuntu e una risorsa del gruppo di disponibilità

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo documento illustra come creare un cluster a tre nodi in Ubuntu e come aggiungere un gruppo di disponibilità creato in precedenza come risorsa nel cluster. Per la disponibilità elevata, un gruppo di disponibilità in Linux richiede tre nodi. Vedere [Disponibilità elevata e protezione dei dati per le configurazioni del gruppo di disponibilità](sql-server-linux-availability-group-ha.md).

> [!NOTE] 
> A questo punto, l'integrazione di SQL Server con Pacemaker in Linux non è associata come con il cluster WSFC in Windows. SQL Server non è a conoscenza della presenza del cluster. Tutta l'orchestrazione viene eseguita all'esterno e il servizio viene controllato come istanza autonoma da Pacemaker. Inoltre, il nome della rete virtuale è specifico di WSFC e non esiste un equivalente in Pacemaker. Le DMV Always On che eseguono query sulle informazioni del cluster restituiscono righe vuote. È comunque possibile creare un listener per la riconnessione trasparente dopo il failover, ma è necessario registrare manualmente il nome del listener nel server DNS con l'indirizzo IP usato per creare la risorsa IP virtuale, come illustrato nelle sezioni seguenti.

Le sezioni seguenti illustrano i passaggi per configurare una soluzione cluster di failover. 

## <a name="roadmap"></a>Roadmap

I passaggi da seguire per creare un gruppo di disponibilità sui server Linux per la disponibilità elevata sono diversi da quelli relativi a un cluster di failover di Windows Server. Nell'elenco seguente sono descritti i passaggi principali: 

1. [Configurare SQL Server nei nodi del cluster](sql-server-linux-setup.md).

2. [Creare il gruppo di disponibilità](sql-server-linux-availability-group-configure-ha.md). 

3. Configurare un modulo per la gestione di risorse cluster, ad esempio Pacemaker. Le istruzioni sono riportate in questo documento.
   
   La modalità di configurazione di un modulo per la gestione di risorse cluster dipende dalla specifica distribuzione Linux. 

   >[!IMPORTANT]
   >Per la disponibilità elevata, negli ambienti di produzione è necessario un agente di isolamento, ad esempio STONITH. Nelle dimostrazioni di questa documentazione non vengono usati agenti di isolamento. Le dimostrazioni sono riportate solo a scopo di test e convalida. 
   >
   >Un cluster Linux usa l'isolamento per ripristinare uno stato noto del cluster. La modalità di configurazione dell'isolamento dipende dalla distribuzione e dall'ambiente. Attualmente l'isolamento non è disponibile in alcuni ambienti cloud. Per altre informazioni, vedere [Support Policies for RHEL High Availability Clusters - Virtualization Platforms](https://access.redhat.com/articles/29440) (Criteri di supporto per il cluster RHEL a disponibilità elevata - Piattaforme di virtualizzazione).
   >
   >L'isolamento viene in genere implementato nel sistema operativo e dipende dall'ambiente. Per istruzioni sull'isolamento nel sistema operativo, vedere la documentazione relativa al server di distribuzione.

5.  [Aggiungere il gruppo di disponibilità come risorsa nel cluster](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource). 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installare e configurare Pacemaker in ogni nodo del cluster

1. Aprire le porte del firewall in tutti i nodi. Aprire la porta per il servizio Pacemaker per la disponibilità elevata, l'istanza di SQL Server e l'endpoint del gruppo di disponibilità. La porta TCP predefinita per il server che esegue SQL Server è la 1433.  

   ```bash
   sudo ufw allow 2224/tcp
   sudo ufw allow 3121/tcp
   sudo ufw allow 21064/tcp
   sudo ufw allow 5405/udp
        
   sudo ufw allow 1433/tcp # Replace with TDS endpoint
   sudo ufw allow 5022/tcp # Replace with DATA_MIRRORING endpoint
        
   sudo ufw reload
   ```
   
   In alternativa, è possibile disabilitare semplicemente il firewall:
        
   ```bash
   sudo ufw disable
   ```

1. Installare i pacchetti Pacemaker. In tutti i nodi eseguire i comandi seguenti:

   ```bash
   sudo apt-get install pacemaker pcs fence-agents resource-agents
   ```

2. Impostare la password per l'utente predefinito creato durante l'installazione dei pacchetti Pacemaker e Corosync. Usare la stessa password in tutti i nodi. 

   ```bash
   sudo passwd hacluster
   ```

## <a name="enable-and-start-pcsd-service-and-pacemaker"></a>Abilitare e avviare il servizio pcsd e Pacemaker

Il comando seguente abilita e avvia il servizio pcsd e Pacemaker. Eseguirlo in tutti i nodi. In questo modo, i nodi potranno essere aggiunti nuovamente al cluster dopo il riavvio. 

```bash
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo systemctl enable pacemaker
```
>[!NOTE]
>È possibile che al termine dell'esecuzione del comando per abilitare Pacemaker venga restituito l'errore "pacemaker Default-Start contains no runlevels, aborting" (L'impostazione Default-Start di Pacemaker non contiene livelli di esecuzione, l'operazione verrà interrotta). Si tratta di un errore innocuo. La configurazione del cluster può continuare. 

## <a name="create-the-cluster"></a>Creare il cluster

1. Rimuovere eventuali configurazioni cluster esistenti da tutti i nodi. 

   Il comando "sudo apt-get install pcs" installa contemporaneamente pacemaker, corosync e pcs e avvia l'esecuzione di tutti e tre i servizi.  L'avvio di corosync genera un file modello /etc/cluster/corosync.conf.  Per la corretta esecuzione dei passaggi seguenti tale file non deve essere presente. Per ovviare a questo problema, arrestare pacemaker / corosync ed eliminare /etc/cluster/corosync.conf. In questo modo, la procedura seguente potrà essere eseguita correttamente. "pcs cluster destroy" esegue la stessa operazione ed è possibile usarlo una sola volta come passaggio di configurazione iniziale del cluster.
   
   Il comando seguente rimuove i file di configurazione del cluster esistenti e arresta tutti i servizi del cluster. In questo modo, il cluster viene eliminato definitivamente. Eseguirlo come primo passaggio in un ambiente di pre-produzione. Si noti che "pcs cluster destroy" ha disabilitato il servizio pacemaker ed è necessario riabilitarlo. Eseguire il comando seguente in tutti i nodi.
   
   >[!WARNING]
   >Il comando elimina definitivamente tutte le risorse cluster esistenti.

   ```bash
   sudo pcs cluster destroy 
   sudo systemctl enable pacemaker
   ```

1. Creare il cluster. 

   >[!WARNING]
   >A causa di un problema noto, attualmente in fase di studio, il comando di avvio del cluster ("pcs cluster start") ha esito negativo con l'errore seguente. Questo è dovuto al fatto che il file di log configurato in /etc/corosync/corosync.conf, creato durante l'esecuzione del comando di configurazione del cluster, non è corretto. Per ovviare a questo problema, modificare il file di log in: /var/log/corosync/corosync.log. In alternativa, è possibile creare il file /var/log/cluster/corosync.log.
 
   ```Error
   Job for corosync.service failed because the control process exited with error code. 
   See "systemctl status corosync.service" and "journalctl -xe" for details.
   ```
  
Il comando seguente crea un cluster a tre nodi. Prima di eseguire lo script, sostituire i valori compresi tra `< ... >`. Eseguire il comando seguente nel nodo primario. 

   ```bash
   sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>
   sudo pcs cluster setup --name <clusterName> <node1> <node2...> <node3>
   sudo pcs cluster start --all
   sudo pcs cluster enable --all
   ```
   
   >[!NOTE]
   >Se in precedenza è stato configurato un cluster negli stessi nodi, è necessario usare l'opzione '--force' quando si esegue 'pcs cluster setup'. Si noti che ciò equivale all'esecuzione di 'pcs cluster destroy' e il servizio Pacemaker deve essere riabilitato usando 'sudo systemctl enable pacemaker'.


## <a name="configure-fencing-stonith"></a>Configurare l'isolamento (STONITH)

I fornitori di cluster Pacemaker richiedono l'abilitazione di STONITH e un dispositivo di isolamento impostato per una configurazione di cluster supportata. Quando il modulo per la gestione di risorse cluster non riesce a determinare lo stato di un nodo o di una risorsa in un nodo, per ripristinare uno stato noto del cluster viene usato l'isolamento. L'isolamento a livello di risorsa garantisce soprattutto che i dati non subiscano danni in caso di interruzione tramite la configurazione di una risorsa. È ad esempio possibile usare l'isolamento a livello di risorsa con DRBD (Distributed Replicated Block Device) per contrassegnare come obsoleto il disco su un nodo quando il collegamento di comunicazione diventa inattivo. L'isolamento a livello di nodo consente di assicurarsi che un nodo non esegua alcuna risorsa. Questa operazione viene eseguita tramite il reset del nodo e la relativa implementazione in Pacemaker è detta STONITH, che è l'acronimo di "Shoot The Other Node In The Head", un'espressione inglese usata per indicare che l'altro nodo deve essere arrestato. Pacemaker supporta un'ampia gamma di dispositivi di isolamento, ad esempio un alimentatore di continuità o schede di interfaccia di gestione per i server. Per altre informazioni, vedere [Pacemaker Clusters from Scratch](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/) (Cluster Pacemaker da zero) e [Fencing and Stonith](https://clusterlabs.org/doc/crm_fencing.html) (Isolamento e Stonith) 

Poiché l'isolamento a livello di nodo dipende principalmente dall'ambiente in uso, viene disabilitato per questa esercitazione e può essere configurato in un secondo momento. Eseguire lo script seguente nel nodo primario: 

```bash
sudo pcs property set stonith-enabled=false
```

>[!IMPORTANT]
>La disabilitazione di STONITH viene eseguita solo a scopo di test. Se si prevede di usare Pacemaker in un ambiente di produzione, è consigliabile pianificare un'implementazione di STONITH in base all'ambiente e mantenerla abilitata. Contattare il fornitore del sistema operativo per informazioni sugli agenti di isolamento per una distribuzione specifica. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Impostare la proprietà del cluster cluster-recheck-interval

`cluster-recheck-interval` indica l'intervallo di polling in base al quale il cluster verifica se sono state apportate modifiche ai parametri delle risorse, ai vincoli o ad altre opzioni del cluster. Se una replica diventa inattiva, il cluster prova a riavviare la replica in base a un intervallo associato ai valori di `failure-timeout` e `cluster-recheck-interval`. Se, ad esempio, `failure-timeout` è impostato su 60 secondi e `cluster-recheck-interval` su 120, viene eseguito un tentativo di riavvio in base a un intervallo superiore a 60 secondi ma inferiore a 120. È consigliabile impostare failure-timeout su 60 secondi e cluster-recheck-interval su un valore superiore a 60 secondi. Evitare di impostare cluster-recheck-interval su un valore inferiore.

Per aggiornare il valore della proprietà su `2 minutes`, eseguire il comando seguente:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Se è già presente una risorsa del gruppo di disponibilità gestita da un cluster Pacemaker, si noti che tutte le distribuzioni che usano il pacchetto Pacemaker 1.1.18-11.el7 più recente introducono una modifica del comportamento per l'impostazione del cluster start-failure-is-fatal quando il relativo valore è false. Questa modifica influisce sul flusso di lavoro del failover. Se si verifica un'interruzione in una replica primaria, di norma dovrebbe essere eseguito il failover del cluster in una delle repliche secondarie disponibili. Al contrario, gli utenti noteranno che il cluster continuerà a provare ad avviare la replica primaria che ha subito l'interruzione. Se la replica primaria non torna mai online, a causa di un'interruzione permanente, il cluster non esegue mai il failover in un'altra replica secondaria disponibile. A causa di questa modifica, una configurazione precedentemente consigliata per impostare start-failure-is-fatal non è più valida e l'impostazione deve essere ripristinata sul valore `true` predefinito. La risorsa del gruppo di disponibilità deve inoltre essere aggiornata in modo da includere la proprietà `failover-timeout`. 
>
>Per aggiornare il valore della proprietà su `true`, eseguire il comando seguente:
>
>```bash
>sudo pcs property set start-failure-is-fatal=true
>```
>
>Per aggiornare la proprietà `failure-timeout` della risorsa del gruppo di disponibilità esistente su `60s`, eseguire il comando seguente (sostituendo `ag1` con il nome della risorsa):
>
>```bash
>pcs resource update ag1 meta failure-timeout=60s
>```

## <a name="install-sql-server-resource-agent-for-integration-with-pacemaker"></a>Installare l'agente delle risorse SQL Server per l'integrazione con Pacemaker

Eseguire i comandi seguenti in tutti i nodi. 

```bash
sudo apt-get install mssql-server-ha
```

## <a name="create-a-sql-server-login-for-pacemaker"></a>Creare un account di accesso di SQL Server per Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Creare una risorsa del gruppo di disponibilità

Per creare la risorsa del gruppo di disponibilità, usare il comando `pcs resource create` e impostare le proprietà della risorsa. Il comando seguente crea una risorsa di tipo master/subordinato `ocf:mssql:ag` per il gruppo di disponibilità con il nome `ag1`. 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master meta notify=true

```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

## <a name="create-virtual-ip-resource"></a>Creare una risorsa IP virtuale

Per creare la risorsa indirizzo IP virtuale, eseguire il comando seguente in un nodo. Usare un indirizzo IP statico disponibile nella rete. Prima di eseguire lo script, sostituire i valori compresi tra `< ... >`con un indirizzo IP valido.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Non esiste un nome di server virtuale equivalente in Pacemaker. Per usare una stringa di connessione che punta a un nome di server in formato stringa e non usa l'indirizzo IP, registrare l'indirizzo della risorsa IP e il nome di server virtuale desiderato in DNS. Per le configurazioni di ripristino di emergenza, registrare il nome e l'indirizzo IP del server virtuale desiderato nei server DNS sia del sito primario sia del sito di ripristino di emergenza.

## <a name="add-colocation-constraint"></a>Aggiungere un vincolo di condivisione percorso

Quasi tutte le decisioni di un cluster Pacemaker, come la scelta della posizione in cui deve essere eseguita una risorsa, vengono eseguite tramite il confronto di punteggi. Il calcolo dei punteggi viene eseguito per singola risorsa e il modulo per la gestione di risorse cluster sceglie il nodo con il punteggio più alto per una determinata risorsa. Se un nodo ha un punteggio negativo per una risorsa, questa non può essere eseguita su tale nodo. Usare i vincoli per configurare le decisioni del cluster. I vincoli hanno un punteggio. Se un vincolo ha un punteggio inferiore a INFINITY, è solo un suggerimento. Un punteggio INFINITY indica invece che il vincolo è obbligatorio. Per assicurarsi che la replica primaria e la risorsa IP virtuale si trovino nello stesso host, definire un vincolo di condivisione percorso con punteggio INFINITY. Per aggiungere un vincolo di condivisione percorso, eseguire il comando seguente in un nodo. 

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Aggiungere un vincolo di ordinamento

Il vincolo di condivisione percorso ha un vincolo di ordinamento implicito. Sposta la risorsa IP virtuale prima di spostare la risorsa del gruppo di disponibilità. Di seguito è indicata la sequenza di eventi predefinita:

1. L'utente invia `pcs resource move` alla replica primaria del gruppo di disponibilità dal nodo 1 al nodo 2.
1. La risorsa IP virtuale viene arrestata sul nodo 1.
1. La risorsa IP virtuale viene avviata sul nodo 2.

   >[!NOTE]
   >A questo punto, l'indirizzo IP punta temporaneamente al nodo 2, mentre il nodo 2 è ancora una replica secondaria preliminare al failover. 
   
1. La replica primaria del gruppo di disponibilità nel nodo 1 viene abbassata di livello e diventa secondaria.
1. La replica secondaria del gruppo di disponibilità nel nodo 2 viene alzata di livello e diventa primaria. 

Per evitare che l'indirizzo IP punti temporaneamente al nodo con la replica secondaria preliminare al failover, aggiungere un vincolo di ordinamento. 

Per aggiungere un vincolo di ordinamento, eseguire il comando seguente in un nodo:

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>Dopo aver configurato il cluster e aggiunto il gruppo di disponibilità come risorsa cluster, non è possibile usare Transact-SQL per eseguire il failover delle risorse del gruppo di disponibilità. Le risorse cluster di SQL Server in Linux non sono strettamente associate al sistema operativo come in un cluster WSFC (Windows Server Failover Cluster). Il servizio SQL Server non è a conoscenza della presenza del cluster. Tutta l'orchestrazione viene eseguita tramite gli strumenti per la gestione di cluster. In RHEL o Ubuntu usare `pcs`. 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Passaggi successivi

[Gestire gruppi di disponibilità per la disponibilità elevata](sql-server-linux-availability-group-failover-ha.md)

