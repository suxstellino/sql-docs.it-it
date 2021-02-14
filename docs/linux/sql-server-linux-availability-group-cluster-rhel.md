---
title: 'RHEL: Configurare un gruppo di disponibilità per SQL Server in Linux'
description: Informazioni su come configurare un gruppo di disponibilità durante l'esecuzione di Red Hat Enterprise Linux (RHEL) per SQL Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/23/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b7102919-878b-4c08-a8c3-8500b7b42397
ms.openlocfilehash: 2790bbe9eb648a2d599d1b0bad8f514ab72eda50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338385"
---
# <a name="configure-rhel-cluster-for-sql-server-availability-group"></a>Configurare un cluster RHEL per un gruppo di disponibilità di SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo documento illustra come creare un cluster a tre nodi per un gruppo di disponibilità di SQL Server in Red Hat Enterprise Linux. Per la disponibilità elevata, un gruppo di disponibilità in Linux richiede tre nodi. Vedere [Disponibilità elevata e protezione dei dati per le configurazioni del gruppo di disponibilità](sql-server-linux-availability-group-ha.md). Il livello di clustering si basa sul [componente aggiuntivo a disponibilità elevata](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) di Red Hat Enterprise Linux (RHEL), basato a sua volta su [Pacemaker](https://clusterlabs.org/). 

> [!NOTE] 
> Per accedere alla documentazione di Red Hat, è necessaria una sottoscrizione. 

Per altre informazioni sulla configurazione del cluster, sulle opzioni degli agenti delle risorse e sulla gestione, vedere la [documentazione di riferimento di RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

> [!NOTE] 
> SQL Server non è strettamente integrato con Pacemaker in Linux come lo è con il clustering di failover di Windows Server. Un'istanza di SQL Server non è conoscenza del cluster. Pacemaker fornisce un'orchestrazione di risorse cluster. Inoltre, il nome della rete virtuale è specifico del clustering di failover di Windows Server. Non esiste un equivalente in Pacemaker. Le DMV del gruppo di disponibilità che eseguono query sulle informazioni del cluster restituiscono righe vuote nei cluster Pacemaker. Per creare un listener per la riconnessione trasparente dopo il failover, registrare manualmente il nome del listener in DNS con l'indirizzo IP usato per creare la risorsa IP virtuale. 

Le sezioni seguenti illustrano i passaggi per configurare un cluster Pacemaker e aggiungere un gruppo di disponibilità come risorsa nel cluster per la disponibilità elevata.

## <a name="roadmap"></a>Roadmap

I passaggi da seguire per creare un gruppo di disponibilità sui server Linux per la disponibilità elevata sono diversi da quelli relativi a un cluster di failover di Windows Server. Nell'elenco seguente sono descritti i passaggi principali: 

1. [Configurare SQL Server nei nodi del cluster](sql-server-linux-setup.md).

2. [Creare il gruppo di disponibilità](sql-server-linux-availability-group-configure-ha.md). 

3. Configurare un modulo per la gestione di risorse cluster, ad esempio Pacemaker. Le istruzioni sono riportate in questo documento.
   
   La modalità di configurazione di un modulo per la gestione di risorse cluster dipende dalla specifica distribuzione Linux. 

   >[!IMPORTANT]
   >Per la disponibilità elevata, negli ambienti di produzione è necessario un agente di isolamento, ad esempio STONITH. Nelle dimostrazioni di questa documentazione non vengono usati agenti di isolamento. Le dimostrazioni sono riportate solo a scopo di test e convalida.
   
   >Un cluster Linux usa l'isolamento per ripristinare uno stato noto del cluster. La modalità di configurazione dell'isolamento dipende dalla distribuzione e dall'ambiente. Attualmente l'isolamento non è disponibile in alcuni ambienti cloud. Per altre informazioni, vedere [Support Policies for RHEL High Availability Clusters - Virtualization Platforms](https://access.redhat.com/articles/29440) (Criteri di supporto per il cluster RHEL a disponibilità elevata - Piattaforme di virtualizzazione).

4. [Aggiungere il gruppo di disponibilità come risorsa nel cluster](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).  

## <a name="configure-high-availability-for-rhel"></a>Configurare la disponibilità elevata per RHEL

Per configurare la disponibilità elevata per RHEL, abilitare la sottoscrizione a disponibilità elevata e quindi configurare Pacemaker.

### <a name="enable-the-high-availability-subscription-for-rhel"></a>Abilitare la sottoscrizione a disponibilità elevata per RHEL

Ogni nodo del cluster deve avere una sottoscrizione appropriata per RHEL e il componente aggiuntivo a disponibilità elevata. Esaminare i requisiti riportati in [How to install High Availability cluster packages in Red Hat Enterprise Linux](https://access.redhat.com/solutions/45930) (Come installare i pacchetti di cluster a disponibilità elevata in Red Hat Enterprise Linux). Seguire questa procedura per configurare la sottoscrizione e i repository:

1. Registrare il sistema.

   ```bash
   sudo subscription-manager register
   ```

   Specificare il nome utente e la password.   

1. Elencare i pool disponibili per la registrazione.

   ```bash
   sudo subscription-manager list --available
   ```

   Dall'elenco dei pool disponibili, prendere nota dell'ID del pool per la sottoscrizione a disponibilità elevata.

1. Aggiornare lo script seguente. Sostituire `<pool id>` con l'ID del pool per la disponibilità elevata nel passaggio precedente. Eseguire lo script per associare la sottoscrizione.

   ```bash
   sudo subscription-manager attach --pool=<pool id>
   ```

1. Abilitare il repository.

   **RHEL 7**

   ```bash
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   ```

   **RHEL 8**

   ```bash
   sudo subscription-manager repos --enable=rhel-8-for-x86_64-highavailability-rpms
   ```

Per altre informazioni, vedere [Pacemaker - The Open Source, High Availability Cluster](https://clusterlabs.org/pacemaker/) (Pacemaker - Il cluster open source a disponibilità elevata). 

Dopo aver configurato la sottoscrizione, completare la procedura seguente per configurare Pacemaker:

### <a name="configure-pacemaker"></a>Configurare Pacemaker

Dopo aver registrato la sottoscrizione, completare la procedura seguente per configurare Pacemaker:
   
[!INCLUDE [RHEL-Configure-Pacemaker](../includes/ss-linux-cluster-pacemaker-configure-rhel.md)]

Dopo aver configurato Pacemaker, usare `pcs` per interagire con il cluster. Eseguire tutti i comandi in unico nodo del cluster. 

## <a name="configure-fencing-stonith"></a>Configurare l'isolamento (STONITH)

I fornitori di cluster Pacemaker richiedono l'abilitazione di STONITH e un dispositivo di isolamento impostato per una configurazione di cluster supportata. STONITH è l'acronimo di "Shoot The Other Node In The Head", un'espressione inglese usata per indicare che l'altro nodo deve essere arrestato. Quando il modulo per la gestione di risorse cluster non riesce a determinare lo stato di un nodo o di una risorsa in un nodo, è possibile ripristinare uno stato noto del cluster tramite l'isolamento.

Un dispositivo STONITH fornisce un agente di isolamento. La [configurazione di Pacemaker in Red Hat Enterprise Linux in Azure](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices) offre un esempio di come creare un dispositivo STONITH per questo cluster in Azure. Modificare le istruzioni per l'ambiente.

L'isolamento a livello di risorsa garantisce che i dati non subiscano danni in caso di interruzione tramite la configurazione di una risorsa. È ad esempio possibile usare l'isolamento a livello di risorsa per contrassegnare come obsoleto il disco su un nodo quando il collegamento di comunicazione diventa inattivo. 

L'isolamento a livello di nodo consente di assicurarsi che un nodo non esegua alcuna risorsa. Questa operazione viene eseguita tramite il reset del nodo. Pacemaker supporta un'ampia gamma di dispositivi di isolamento, ad esempio un alimentatore di continuità o schede di interfaccia di gestione per i server.

Per informazioni su STONITH e sull'isolamento, vedere gli articoli seguenti:

* [Pacemaker Clusters from Scratch](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/index.html) (Cluster Pacemaker da zero)
* [Fencing and STONITH](https://clusterlabs.org/doc/crm_fencing.html) (Isolamento e STONITH)
* [Red Hat High Availability Add-On with Pacemaker: Fencing](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-fencing-HAAR.html) (Componente aggiuntivo a disponibilità elevata di Red Hat con Pacemaker: Isolamento)

>[!NOTE]
>Poiché la configurazione dell'isolamento a livello di nodo dipende principalmente dall'ambiente in uso, disabilitarlo per questa esercitazione. Potrà essere configurato in un secondo momento. Lo script seguente disabilita l'isolamento a livello di nodo:
>
>```bash
>sudo pcs property set stonith-enabled=false
>``` 
>
>La disabilitazione di STONITH viene eseguita solo a scopo di test. Se si prevede di usare Pacemaker in un ambiente di produzione, è consigliabile pianificare un'implementazione di STONITH in base all'ambiente e mantenerla abilitata.

## <a name="set-cluster-property-cluster-recheck-interval"></a>Impostare la proprietà del cluster cluster-recheck-interval

`cluster-recheck-interval` indica l'intervallo di polling in base al quale il cluster verifica se sono state apportate modifiche ai parametri delle risorse, ai vincoli o ad altre opzioni del cluster. Se una replica diventa inattiva, il cluster prova a riavviare la replica in base a un intervallo associato ai valori di `failure-timeout` e `cluster-recheck-interval`. Se, ad esempio, `failure-timeout` è impostato su 60 secondi e `cluster-recheck-interval` su 120, viene eseguito un tentativo di riavvio in base a un intervallo superiore a 60 secondi ma inferiore a 120. È consigliabile impostare failure-timeout su 60 secondi e cluster-recheck-interval su un valore superiore a 60 secondi. Evitare di impostare cluster-recheck-interval su un valore inferiore.

Per aggiornare il valore della proprietà su `2 minutes`, eseguire il comando seguente:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Tutte le distribuzioni (incluse RHEL 7.3 e 7.4) che usano il pacchetto Pacemaker 1.1.18-11.el7 più recente introducono una modifica del comportamento per l'impostazione del cluster start-failure-is-fatal quando il relativo valore è false. Questa modifica influisce sul flusso di lavoro del failover. Se si verifica un'interruzione in una replica primaria, di norma dovrebbe essere eseguito il failover del cluster in una delle repliche secondarie disponibili. Al contrario, gli utenti noteranno che il cluster continuerà a provare ad avviare la replica primaria che ha subito l'interruzione. Se la replica primaria non torna mai online, a causa di un'interruzione permanente, il cluster non esegue mai il failover in un'altra replica secondaria disponibile. A causa di questa modifica, una configurazione precedentemente consigliata per impostare start-failure-is-fatal non è più valida e l'impostazione deve essere ripristinata sul valore `true` predefinito. La risorsa del gruppo di disponibilità deve inoltre essere aggiornata in modo da includere la proprietà `failover-timeout`. 

Per aggiornare il valore della proprietà su `true`, eseguire il comando seguente:

```bash
sudo pcs property set start-failure-is-fatal=true
```

Per aggiornare la proprietà `failure-timeout` della risorsa `ag_cluster` impostandola su `60s`, eseguire il comando seguente:

```bash
pcs resource update ag_cluster meta failure-timeout=60s
```


Per informazioni sulle proprietà del cluster Pacemaker, vedere [Pacemaker Cluster Properties](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-clusteropts-HAAR.html) (Proprietà del cluster Pacemaker).

## <a name="create-a-sql-server-login-for-pacemaker"></a>Creare un account di accesso di SQL Server per Pacemaker

[!INCLUDE [SQL-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Creare una risorsa del gruppo di disponibilità

Per creare la risorsa del gruppo di disponibilità, usare il comando `pcs resource create` e impostare le proprietà della risorsa. Il comando seguente crea una risorsa di tipo master/subordinato `ocf:mssql:ag` per il gruppo di disponibilità con il nome `ag1`.

**RHEL 7**

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s master notify=true
```

**RHEL 8**

Con la disponibilità di **RHEL 8**, la sintassi CREATE è cambiata. Se si usa **RHEL 8**, il termine `master` è stato modificato in `promotable`. Usare il comando di creazione seguente invece del comando precedente: 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s promotable notify=true
```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

<a name="createIP"></a>

## <a name="create-virtual-ip-resource"></a>Creare una risorsa IP virtuale

Per creare la risorsa indirizzo IP virtuale, eseguire il comando seguente in un nodo. Usare un indirizzo IP statico disponibile nella rete. Sostituire l'indirizzo IP `<10.128.16.240>` con un indirizzo IP valido.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Non esiste un nome di server virtuale equivalente in Pacemaker. Per usare una stringa di connessione che punta a un nome di server in formato stringa anziché a un indirizzo IP, registrare l'indirizzo della risorsa IP virtuale e il nome di server virtuale desiderato in DNS. Per le configurazioni di ripristino di emergenza, registrare il nome e l'indirizzo IP del server virtuale desiderato nei server DNS sia del sito primario sia del sito di ripristino di emergenza.

## <a name="add-colocation-constraint"></a>Aggiungere un vincolo di condivisione percorso

Quasi tutte le decisioni di un cluster Pacemaker, come la scelta della posizione in cui deve essere eseguita una risorsa, vengono eseguite tramite il confronto di punteggi. I punteggi vengono calcolati per singola risorsa. Il modulo per la gestione di risorse cluster sceglie il nodo con il punteggio più alto per una determinata risorsa. Se un nodo ha un punteggio negativo per una risorsa, la risorsa non può essere eseguita su tale nodo.

In un cluster Pacemaker, è possibile modificare le decisioni del cluster applicando vincoli. I vincoli hanno un punteggio. Se un vincolo ha un punteggio inferiore a `INFINITY`, Pacemaker lo interpreta come suggerimento. Un vincolo con punteggio `INFINITY` è obbligatorio.

Per assicurarsi che la replica primaria e la risorsa IP virtuale vengano eseguite nello stesso host, definire un vincolo di condivisione percorso con punteggio INFINITY. Per aggiungere un vincolo di condivisione percorso, eseguire il comando seguente in un nodo.

### <a name="rhel-7"></a>RHEL 7

Quando si crea la risorsa `ag_cluster` in RHEL 7, viene creata come `ag_cluster-master`. Usare il comando seguente per RHEL 7:

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

### <a name="rhel-8"></a>RHEL 8

Quando si crea la risorsa `ag_cluster` in RHEL 8, viene creata come `ag_cluster-clone`. Usare il comando seguente per RHEL 8:

```bash
sudo pcs constraint colocation add virtualip with master ag_cluster-clone INFINITY with-rsc-role=Master
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

### <a name="rhel-7"></a>RHEL 7

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

### <a name="rhel-8"></a>RHEL 8

```bash
sudo pcs constraint order promote ag_cluster-clone then start virtualip
```

>[!IMPORTANT]
>Dopo aver configurato il cluster e aggiunto il gruppo di disponibilità come risorsa cluster, non è possibile usare Transact-SQL per eseguire il failover delle risorse del gruppo di disponibilità. Le risorse cluster di SQL Server in Linux non sono strettamente associate al sistema operativo come in un cluster WSFC (Windows Server Failover Cluster). Il servizio SQL Server non è a conoscenza della presenza del cluster. Tutta l'orchestrazione viene eseguita tramite gli strumenti per la gestione di cluster. In RHEL o Ubuntu usare `pcs` e in SLES usare `crm`. 

Per eseguire manualmente il failover per il gruppo di disponibilità, usare `pcs`. Non avviare il failover con Transact-SQL. Per istruzioni, vedere [Failover](sql-server-linux-availability-group-failover-ha.md#failover).

## <a name="next-steps"></a>Passaggi successivi

[Gestire gruppi di disponibilità per la disponibilità elevata](sql-server-linux-availability-group-failover-ha.md)
