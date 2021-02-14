---
title: 'SUSE: Configurare un gruppo di disponibilità per SQL Server in Linux'
titleSuffix: SQL Server
description: Informazioni su come creare cluster di un gruppo di disponibilità per SQL Server in SUSE Linux Enterprise Server (SLES).
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 85180155-6726-4f42-ba57-200bf1e15f4d
ms.openlocfilehash: 232235e1e7b8718b83c4826b4a019cb6c52e437c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338409"
---
# <a name="configure-sles-cluster-for-sql-server-availability-group"></a>Configurare un cluster SLES per un gruppo di disponibilità di SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo documento include le istruzioni per la creazione di un cluster a tre nodi per SQL Server in SUSE Linux Enterprise Server (SLES) 12 SP2. Per la disponibilità elevata, un gruppo di disponibilità in Linux richiede tre nodi. Vedere [Disponibilità elevata e protezione dei dati per le configurazioni del gruppo di disponibilità](sql-server-linux-availability-group-ha.md). Il livello di clustering si basa su SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability), a sua volta basato su [Pacemaker](https://clusterlabs.org/). 

Per altre informazioni su configurazione dei cluster, opzioni degli agenti delle risorse, gestione, procedure consigliate e suggerimenti, vedere [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

>[!NOTE]
>A questo punto, l'integrazione di SQL Server con Pacemaker in Linux non è associata come con il cluster WSFC in Windows. Il servizio SQL Server in Linux non è compatibile con i cluster. Pacemaker controlla tutta l'orchestrazione delle risorse cluster, inclusa la risorsa del gruppo di disponibilità. In Linux non è consigliabile fare affidamento sulle DMV dei gruppi di disponibilità Always On che forniscono informazioni sul cluster come sys.dm_hadr_cluster. Inoltre, il nome della rete virtuale è specifico di WSFC e non esiste un equivalente in Pacemaker. È comunque possibile creare un listener per la riconnessione trasparente dopo il failover, ma sarà necessario registrare manualmente il nome del listener nel server DNS con l'indirizzo IP usato per creare la risorsa IP virtuale, come illustrato nelle sezioni seguenti.

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

## <a name="roadmap"></a>Roadmap

La procedura per la creazione di un gruppo di disponibilità per la disponibilità elevata è diversa tra i server Linux e un cluster di failover di Windows Server. Nell'elenco seguente sono descritti i passaggi principali: 

1. [Configurare SQL Server nei nodi del cluster](sql-server-linux-setup.md).

2. [Creare il gruppo di disponibilità](sql-server-linux-availability-group-failover-ha.md). 

3. Configurare un modulo per la gestione di risorse cluster, ad esempio Pacemaker. Le istruzioni sono riportate in questo documento.
   
   La modalità di configurazione di un modulo per la gestione di risorse cluster dipende dalla specifica distribuzione Linux. 

   >[!IMPORTANT]
   >Per la disponibilità elevata, negli ambienti di produzione è necessario un agente di isolamento, ad esempio STONITH. Gli esempi in questo articolo non usano agenti di isolamento. Sono illustrati solo a scopo di test e convalida. 
   
   >Un cluster Pacemaker usa l'isolamento per ripristinare uno stato noto del cluster. La modalità di configurazione dell'isolamento dipende dalla distribuzione e dall'ambiente. Attualmente l'isolamento non è disponibile in alcuni ambienti cloud. Vedere [SUSE Linux Enterprise High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. [Aggiungere il gruppo di disponibilità come risorsa nel cluster](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server). 

## <a name="prerequisites"></a>Prerequisiti

Per completare lo scenario end-to-end seguente, sono necessari tre computer per distribuire il cluster a tre nodi. I passaggi seguenti illustrano come configurare questi server.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installare e configurare il sistema operativo in ogni nodo del cluster 

Il primo passaggio consiste nel configurare il sistema operativo nei nodi del cluster. Per questa procedura dettagliata, usare SLES 12 SP2 con una sottoscrizione valida per il componente aggiuntivo a disponibilità elevata.

### <a name="install-and-configure-sql-server-service-on-each-cluster-node"></a>Installare e configurare il servizio SQL Server in ogni nodo del cluster

1. Installare e configurare il servizio SQL Server in tutti i nodi. Per istruzioni dettagliate, vedere [Installare SQL Server in Linux](sql-server-linux-setup.md).

1. Designare un nodo come primario e gli altri nodi come secondari. Usare questi termini per tutte le istruzioni riportate in questo documento.

1. Assicurarsi che i nodi da includere nel cluster possano comunicare tra loro.

   L'esempio seguente mostra `/etc/hosts` con gli indirizzi aggiuntivi per i tre nodi denominati SLES1, SLES2 e SLES3.

   ```
   127.0.0.1   localhost
   10.128.16.33 SLES1
   10.128.16.77 SLES2
   10.128.16.22 SLES3
   ```

   Tutti i nodi del cluster devono poter accedere l'uno all'altro tramite SSH. Strumenti come `hb_report` o `crm_report` (per la risoluzione dei problemi) e History Explorer di Hawk richiedono l'accesso a SSH senza password da un nodo all'altro, altrimenti possono raccogliere dati solo dal nodo corrente. Se si usa una porta SSH non standard, usare l'opzione -X (vedere la pagina relativa a `man`). Se, ad esempio, la porta SSH è 3479, richiamare un oggetto `crm_report` con:

   ```bash
   sudo crm_report -X "-p 3479" [...]
   ```

   Per altre informazioni, vedere [SLES Administration Guide - Miscellaneous](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc) (Guida all'amministrazione di SLES - Varie).


## <a name="create-a-sql-server-login-for-pacemaker"></a>Creare un account di accesso di SQL Server per Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="configure-an-always-on-availability-group"></a>Configurare un gruppo di disponibilità Always On

Nei server Linux configurare il gruppo di disponibilità e quindi le risorse cluster. Per configurare il gruppo di disponibilità, vedere [Configurare un gruppo di disponibilità Always On di SQL Server per la disponibilità elevata in Linux](sql-server-linux-availability-group-configure-ha.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installare e configurare Pacemaker in ogni nodo del cluster

1. Installare l'estensione per la disponibilità elevata

   Per informazioni di riferimento, vedere [Installing SUSE Linux Enterprise Server and High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.installation) (Installazione di SUSE Linux Enterprise Server e dell'estensione per la disponibilità elevata)

1. Installare il pacchetto dell'agente delle risorse SQL Server in entrambi i nodi.

   ```bash
   sudo zypper install mssql-server-ha
   ```

## <a name="set-up-the-first-node"></a>Configurare il primo nodo

   Vedere le [istruzioni per l'installazione di SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node)

1. Accedere come `root` alla macchina virtuale o fisica che si vuole usare come nodo del cluster.
2. Avviare lo script di bootstrap con il comando seguente:
   ```bash
   sudo ha-cluster-init
   ```

   Se NTP non è stato configurato in modo da essere eseguito in fase di avvio, viene visualizzato un messaggio. 

   Se si decide di continuare comunque, lo script genera automaticamente le chiavi per l'accesso SSH e per lo strumento di sincronizzazione Csync2 e avvia i servizi necessari per entrambi. 

3. Per configurare il livello di comunicazione del cluster (Corosync): 

   a. Immettere un indirizzo di rete con cui stabilire l'associazione. Per impostazione predefinita, lo script propone l'indirizzo di rete eth0. In alternativa, immettere un indirizzo di rete diverso, ad esempio bond0. 

   b. Immettere un indirizzo multicast. Lo script propone un indirizzo casuale che è possibile usare come predefinito. 

   c. Immettere una porta multicast. Lo script propone 5405 come numero di porta predefinito. 

   d. Per configurare `SBD ()`, immettere un percorso permanente per la partizione del dispositivo a blocchi che si vuole usare per SBD. Il percorso deve essere coerente in tutti i nodi del cluster. 
   Lo script avvierà infine il servizio Pacemaker per portare online il cluster a un nodo e abilitare l'interfaccia di gestione Web Hawk2. L'URL da usare per Hawk2 viene visualizzato sullo schermo. 

4. Per informazioni dettagliate sul processo di installazione, vedere `/var/log/sleha-bootstrap.log`. A questo punto è disponibile un cluster a un nodo in esecuzione. Verificare lo stato del cluster con crm status:

   ```bash
   sudo crm status
   ```

   È anche possibile vedere la configurazione del cluster con `crm configure show xml` o `crm configure show`.

5. La routine di bootstrap crea un utente Linux denominato hacluster con password linux. Sostituire la password predefinita con una password sicura, non appena possibile: 

   ```bash
   sudo passwd hacluster
   ```

## <a name="add-nodes-to-the-existing-cluster"></a>Aggiungere nodi al cluster esistente

Se è in esecuzione un cluster con uno o più nodi, aggiungere altri nodi del cluster con lo script di bootstrap ha-cluster-join. Lo script deve semplicemente disporre dell'accesso a un nodo del cluster esistente e completerà automaticamente la configurazione di base nel computer corrente. Eseguire la procedura descritta di seguito:

Se i nodi del cluster esistenti sono stati configurati con il modulo per cluster `YaST`, prima di eseguire `ha-cluster-join` assicurarsi che i prerequisiti seguenti siano soddisfatti:
- L'utente ROOT nei nodi esistenti dispone di chiavi SSH per l'accesso senza password. 
- `Csync2` è configurato nei nodi esistenti. Per altre informazioni, vedere Configurazione di Csync2 con YaST. 

1. Accedere come utente ROOT alla macchina virtuale o fisica che dovrebbe essere aggiunta al cluster. 
2. Avviare lo script di bootstrap con il comando seguente: 

   ```bash
   sudo ha-cluster-join
   ```

   Se NTP non è stato configurato in modo da essere eseguito in fase di avvio, viene visualizzato un messaggio. 

3. Se si decide di continuare comunque, verrà chiesto di specificare l'indirizzo IP di un nodo esistente. Immettere l'indirizzo IP. 

4. Se non è già stato configurato un accesso SSH senza password tra entrambi i computer, verrà chiesto di specificare anche la password radice del nodo esistente. 

   Dopo aver eseguito l'accesso al nodo specificato, lo script copia la configurazione di Corosync, configura SSH e `Csync2`, quindi porta online il computer corrente come nuovo nodo del cluster. Oltre a questo, avvia il servizio necessario per Hawk. Se lo spazio di archiviazione condiviso è stato configurato con `OCFS2`, viene creata automaticamente anche la directory del punto di montaggio per il file system `OCFS2`. 

5. Ripetere i passaggi precedenti per tutti i computer da aggiungere al cluster. 

6. Per informazioni dettagliate sulla procedura, vedere `/var/log/ha-cluster-bootstrap.log`. 

1. Verificare lo stato del cluster con `sudo crm status`. Se il secondo nodo è stato aggiunto correttamente, l'output sarà simile al seguente:

   ```bash
   sudo crm status
   
   3 nodes configured
   1 resource configured
   Online: [ SLES1 SLES2 SLES3]
   Full list of resources:   
   admin_addr     (ocf::heartbeat:IPaddr2):       Started node1
   ```

   >[!NOTE]
   >`admin_addr` è la risorsa cluster IP virtuale definita durante la configurazione iniziale del cluster a un nodo.

Dopo aver aggiunto tutti i nodi, controllare se è necessario modificare no-quorum-policy nelle opzioni globali del cluster. Questa operazione è particolarmente importante per i cluster a due nodi. Per altre informazioni, vedere la sezione 4.1.2 relativa all'opzione no-quorum-policy. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Impostare la proprietà del cluster cluster-recheck-interval

`cluster-recheck-interval` indica l'intervallo di polling in base al quale il cluster verifica se sono state apportate modifiche ai parametri delle risorse, ai vincoli o ad altre opzioni del cluster. Se una replica diventa inattiva, il cluster prova a riavviare la replica in base a un intervallo associato ai valori di `failure-timeout` e `cluster-recheck-interval`. Se, ad esempio, `failure-timeout` è impostato su 60 secondi e `cluster-recheck-interval` su 120, viene eseguito un tentativo di riavvio in base a un intervallo superiore a 60 secondi ma inferiore a 120. È consigliabile impostare failure-timeout su 60 secondi e cluster-recheck-interval su un valore superiore a 60 secondi. Evitare di impostare cluster-recheck-interval su un valore inferiore.

Per aggiornare il valore della proprietà su `2 minutes`, eseguire il comando seguente:

```bash
crm configure property cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Se è già presente una risorsa del gruppo di disponibilità gestita da un cluster Pacemaker, si noti che tutte le distribuzioni che usano il pacchetto Pacemaker 1.1.18-11.el7 più recente introducono una modifica del comportamento per l'impostazione del cluster start-failure-is-fatal quando il relativo valore è false. Questa modifica influisce sul flusso di lavoro del failover. Se si verifica un'interruzione in una replica primaria, di norma dovrebbe essere eseguito il failover del cluster in una delle repliche secondarie disponibili. Al contrario, gli utenti noteranno che il cluster continuerà a provare ad avviare la replica primaria che ha subito l'interruzione. Se la replica primaria non torna mai online, a causa di un'interruzione permanente, il cluster non esegue mai il failover in un'altra replica secondaria disponibile. A causa di questa modifica, una configurazione precedentemente consigliata per impostare start-failure-is-fatal non è più valida e l'impostazione deve essere ripristinata sul valore `true` predefinito. La risorsa del gruppo di disponibilità deve inoltre essere aggiornata in modo da includere la proprietà `failover-timeout`. 
>
>Per aggiornare il valore della proprietà su `true`, eseguire il comando seguente:
>
>```bash
>crm configure property start-failure-is-fatal=true
>```
>
>Per aggiornare la proprietà `failure-timeout` della risorsa del gruppo di disponibilità esistente su `60s`, eseguire il comando seguente (sostituendo `ag1` con il nome della risorsa): 
>
>```bash
>crm configure edit ag1
># In the text editor, add `meta failure-timeout=60s` after any `param`s and before any `op`s
>```

Per altre informazioni sulle proprietà del cluster Pacemaker, vedere [Configurazione delle risorse cluster](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_config_crm_resources.html).

## <a name="configure-fencing-stonith"></a>Configurare l'isolamento (STONITH)
I fornitori di cluster Pacemaker richiedono l'abilitazione di STONITH e un dispositivo di isolamento impostato per una configurazione di cluster supportata. Quando il modulo per la gestione di risorse cluster non riesce a determinare lo stato di un nodo o di una risorsa in un nodo, per ripristinare uno stato noto del cluster viene usato l'isolamento.

L'isolamento a livello di risorsa garantisce soprattutto che i dati non subiscano danni durante un'interruzione tramite la configurazione di una risorsa. È ad esempio possibile usare l'isolamento a livello di risorsa con DRBD (Distributed Replicated Block Device) per contrassegnare come obsoleto il disco su un nodo quando il collegamento di comunicazione diventa inattivo.

L'isolamento a livello di nodo consente di assicurarsi che un nodo non esegua alcuna risorsa. Questa operazione viene eseguita tramite il reset del nodo e la relativa implementazione in Pacemaker è detta STONITH, che è l'acronimo di "Shoot The Other Node In The Head", un'espressione inglese usata per indicare che l'altro nodo deve essere arrestato. Pacemaker supporta un'ampia gamma di dispositivi di isolamento, ad esempio un alimentatore di continuità o schede di interfaccia di gestione per i server.

Per altre informazioni, vedere:

- [Pacemaker Clusters from Scratch](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/) (Cluster Pacemaker da zero)
- [Fencing and Stonith](https://clusterlabs.org/doc/crm_fencing.html) (Isolamento e Stonith)
- [Documentazione di SUSE a disponibilità elevata: Fencing and STONITH](https://www.suse.com/documentation/sle_ha/book_sleha/data/cha_ha_fencing.html) (Isolamento e STONITH)

In fase di inizializzazione del cluster, se non viene rilevata alcuna configurazione, STONITH è disabilitato. È possibile abilitarlo in un secondo momento tramite il comando seguente:

```bash
sudo crm configure property stonith-enabled=true
```
  
>[!IMPORTANT]
>La disabilitazione di STONITH viene eseguita solo a scopo di test. Se si prevede di usare Pacemaker in un ambiente di produzione, è consigliabile pianificare un'implementazione di STONITH in base all'ambiente e mantenerla abilitata. SUSE non fornisce agenti di isolamento per ambienti cloud (incluso Azure) o Hyper-V. Di conseguenza, il fornitore del cluster non offre supporto per l'esecuzione di cluster di produzione in questi ambienti. Questo problema è attualmente in fase di studio e una soluzione sarà disponibile nelle versioni future.

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configurare le risorse cluster per SQL Server

Vedere [SLES Administration Guide](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config) (Guida all'amministrazione di SLES)

## <a name="enable-pacemaker"></a>Abilitare Pacemaker

Abilitare Pacemaker per consentirne l'avvio automatico.

In ogni nodo del cluster eseguire il comando seguente.

```bash
systemctl enable pacemaker
```

### <a name="create-availability-group-resource"></a>Creare una risorsa del gruppo di disponibilità

Il comando seguente crea e configura la risorsa per tre repliche del gruppo di disponibilità [ag1]. Le operazioni di monitoraggio e i timeout devono essere definiti in modo esplicito in SLES tenendo conto del fatto che i timeout sono fortemente dipendenti dai carichi di lavoro e devono essere definiti con precisione per ogni distribuzione.
Eseguire il comando seguente in uno dei nodi del cluster:

1. Eseguire `crm configure` per aprire il prompt di crm:

   ```bash
   sudo crm configure 
   ```

1. Nel prompt di crm eseguire il comando seguente per configurare le proprietà della risorsa.

   ```bash
   primitive ag_cluster \
      ocf:mssql:ag \
      params ag_name="ag1" \
      meta failure-timeout=60s \
      op start timeout=60s \
      op stop timeout=60s \
      op promote timeout=60s \
      op demote timeout=10s \
      op monitor timeout=60s interval=10s \
      op monitor timeout=60s interval=11s role="Master" \
      op monitor timeout=60s interval=12s role="Slave" \
      op notify timeout=60s
   ms ms-ag_cluster ag_cluster \
      meta master-max="1" master-node-max="1" clone-max="3" \
     clone-node-max="1" notify="true" \
   commit
      ```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

### <a name="create-virtual-ip-resource"></a>Creare una risorsa IP virtuale

Se la risorsa IP virtuale non è stata creata al momento dell'esecuzione di `ha-cluster-init`, è possibile crearla adesso. Il comando seguente crea una risorsa IP virtuale. Sostituire `<**0.0.0.0**>` con un indirizzo disponibile della rete e `<**24**>` con il numero di bit nella subnet mask CIDR. Eseguire il comando in un nodo.

```bash
crm configure \
primitive admin_addr \
   ocf:heartbeat:IPaddr2 \
   params ip=<**0.0.0.0**> \
      cidr_netmask=<**24**>
```

### <a name="add-colocation-constraint"></a>Aggiungere un vincolo di condivisione percorso
Quasi tutte le decisioni di un cluster Pacemaker, come la scelta della posizione in cui deve essere eseguita una risorsa, vengono eseguite tramite il confronto di punteggi. Il calcolo dei punteggi viene eseguito per singola risorsa e il modulo per la gestione di risorse cluster sceglie il nodo con il punteggio più alto per una determinata risorsa. Se un nodo ha un punteggio negativo per una risorsa, questa non può essere eseguita su tale nodo. Le decisioni del cluster possono essere modificate applicando vincoli. I vincoli hanno un punteggio. Se un vincolo ha un punteggio inferiore a INFINITY, è solo un suggerimento. Un punteggio INFINITY indica invece che il vincolo è obbligatorio. Per assicurarsi che la replica primaria del gruppo di disponibilità e la risorsa IP virtuale vengano eseguite nello stesso host, definire un vincolo di condivisione percorso con punteggio INFINITY. 

Per impostare un vincolo di condivisione percorso per fare in modo che la risorsa IP virtuale venga eseguita nello stesso nodo della risorsa master, eseguire il comando seguente in un nodo:

```bash
crm configure
colocation vip_on_master inf: \
    admin_addr ms-ag_cluster:Master
commit
```

### <a name="add-ordering-constraint"></a>Aggiungere un vincolo di ordinamento
Il vincolo di condivisione percorso ha un vincolo di ordinamento implicito. Sposta la risorsa IP virtuale prima di spostare la risorsa del gruppo di disponibilità. Di seguito è indicata la sequenza di eventi predefinita: 

1. L'utente invia un comando di migrazione della risorsa alla replica master del gruppo di disponibilità, dal nodo 1 al nodo 2.
2. La risorsa IP virtuale viene arrestata sul nodo 1.
3. La risorsa IP virtuale viene avviata sul nodo 2. A questo punto, l'indirizzo IP punta temporaneamente al nodo 2, mentre il nodo 2 è ancora una replica secondaria preliminare al failover. 
4. La replica master del gruppo di disponibilità nel nodo 1 viene abbassata di livello.
5. Il gruppo di disponibilità nel nodo 2 viene alzato di livello a master. 

Per evitare che l'indirizzo IP punti temporaneamente al nodo con la replica secondaria preliminare al failover, aggiungere un vincolo di ordinamento. Per aggiungere un vincolo di ordinamento, eseguire il comando seguente in un nodo: 

```bash
sudo crm configure \
   order ag_first inf: ms-ag_cluster:promote admin_addr:start
```


>[!IMPORTANT]
>Dopo aver configurato il cluster e aggiunto il gruppo di disponibilità come risorsa cluster, non è possibile usare Transact-SQL per eseguire il failover delle risorse del gruppo di disponibilità. Le risorse cluster di SQL Server in Linux non sono strettamente associate al sistema operativo come in un cluster WSFC (Windows Server Failover Cluster). Il servizio SQL Server non è a conoscenza della presenza del cluster. Tutta l'orchestrazione viene eseguita tramite gli strumenti per la gestione di cluster. In SLES usare `crm`. 

Per eseguire manualmente il failover per il gruppo di disponibilità, usare `crm`. Non avviare il failover con Transact-SQL. Per altre informazioni, vedere [Failover](sql-server-linux-availability-group-failover-ha.md#failover).


Per altre informazioni, vedere:
- [Managing Cluster Resources](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm) (Gestione di risorse cluster)   
- [HA Concepts](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.concepts) (Concetti relativi alla disponibilità elevata)
- [Pacemaker Quick Reference](https://github.com/ClusterLabs/pacemaker/blob/master/doc/pcs-crmsh-quick-ref.md) (Riferimento rapido per Pacemaker) 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Passaggi successivi

[Gestire gruppi di disponibilità per la disponibilità elevata](sql-server-linux-availability-group-failover-ha.md)
