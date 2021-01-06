---
title: Istanze del cluster di failover Always On
description: Informazioni sulle istanze del cluster di failover Always On che forniscono disponibilità elevata locale tramite ridondanza a livello di istanza di server in SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- high availability [SQL Server], failover clustering
- virtual servers [SQL Server], about virtual servers
- clusters [SQL Server]
- servers [SQL Server], failover clustering
- resource groups [SQL Server]
- availability [SQL Server]
- failover clustering [SQL Server]
- AlwaysOn [SQL Server], see failover clustering [SQL Server]
ms.assetid: 86a15b33-4d03-4549-8ea2-b45e4f1baad7
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea1413b30c4706125b711493f64210c2a875ec0f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642813"
---
# <a name="always-on-failover-cluster-instances-sql-server"></a>Istanze del cluster di failover Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Nell'offerta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On le istanze del cluster di failover Always On usano la funzionalità clustering di failover di Windows Server (WSFC, Windows Server Failover Clustering) per fornire la disponibilità elevata in locale tramite la ridondanza a livello di istanza del server, ovvero un'*istanza del cluster di failover*. Un'istanza del cluster di failover è una sola istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installata nei nodi del clustering di failover di Windows Server (WSFC) e, possibilmente, in più subnet. In rete, un'istanza del cluster di failover appare come un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in esecuzione in un singolo computer, le cui funzionalità forniscono il failover da un nodo WSFC a un altro, quando il nodo corrente non è più disponibile.  
  
 Un'istanza del cluster di failover può sfruttare i [gruppi di disponibilità](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) per fornire ripristino di emergenza remoto a livello di database. Per altre informazioni, vedere [Clustering di failover e gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
 
 > [!NOTE]  
 > In Windows Server 2016 Datacenter Edition è stato introdotto il supporto per Spazi di archiviazione diretta (S2D). Le istanze del cluster di failover di SQL Server supportano S2D per le risorse di archiviazione cluster. Per altre informazioni, vedere [Spazi di archiviazione diretta in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).
 > 
 >Le istanze del cluster di failover supportano anche i volumi condivisi cluster. Per altre informazioni, vedere [Informazioni sui volumi condivisi del cluster in un cluster di failover](https://technet.microsoft.com/library/dd759255.aspx). 
   
 **Contenuto dell'argomento**  
  
-   [Vantaggi](#Benefits)  
  
-   [Indicazioni](#Recommendations)  
  
-   [Panoramica dell'istanza del cluster di failover](#Overview)  
  
-   [Elementi di un'istanza del cluster di failover](#FCIelements)  
  
-   [Concetti e attività di failover di SQL Server](#ConceptsAndTasks)  
  
-   [Argomenti correlati](#RelatedTopics)  
  
##  <a name="benefits-of-a-failover-cluster-instance"></a><a name="Benefits"></a> Vantaggi di un'istanza del cluster di failover  
 Quando si verifica un errore dell'hardware o del software di un server, le applicazioni o i client che si connettono al server subiranno tempi di inattività. Quando un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è configurata per essere un'istanza del cluster di failover (anziché un'istanza autonoma), la disponibilità elevata di quell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è protetta dalla presenza di nodi ridondanti nell'istanza del cluster di failover. Solo un nodo per volta nell'istanza del cluster di failover possiede il gruppo di risorse WSFC. Nel caso di errore (hardware, del sistema operativo, di applicazione o del servizio) o di aggiornamento pianificato, la proprietà del gruppo di risorse viene spostata a un altro nodo WSFC. Questo processo non è visibile al client o all'applicazione che si connette a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , riducendo di conseguenza i tempi d'inattività dell'applicazione o dei client durante un errore. Di seguito vengono elencati i vantaggi principali offerti dalle istanze del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   Protezione a livello di istanza tramite ridondanza  
  
-   Failover automatico in caso di errore (errore hardware, del sistema operativo, dell'applicazione o del servizio)  
  
    > [!IMPORTANT]  
    >  In un gruppo di disponibilità non è supportato il failover automatico da un'istanza del cluster di failover ad altri nodi all'interno del gruppo di disponibilità. Questo vuole dire che le istanze del cluster di failover e i nodi autonomi non devono essere accoppiati all'interno di un gruppo di disponibilità se il failover automatico è un importante componente della soluzione di disponibilità elevata. Tuttavia, è possibile creare questo accoppiamento per il *ripristino di emergenza* .  
  
-   Il supporto per una matrice estesa di soluzioni di archiviazione, tra cui i dischi WSFC (iSCSI, Fiber Channel e così via) e le condivisioni di file Server Message Block (SMB).  
  
-   Soluzione di ripristino di emergenza che usa un'istanza del cluster di failover su più subnet o che esegue un database ospitato dall'istanza del cluster di failover in un gruppo di disponibilità. Con il nuovo supporto di più subnet in [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], un'istanza del cluster di failover su più subnet non richiede più una LAN virtuale, aumentando la facilità di gestione e la sicurezza di un'istanza del cluster di failover su più subnet.  
  
-   Nessuna riconfigurazione di applicazioni e client durante i failover  
  
-   Criteri di failover flessibili per gli eventi del trigger granulari per failover automatici  
  
-   Failover affidabili tramite il rilevamento di integrità periodico e dettagliato utilizzando connessioni dedicate e persistenti  
  
-   Configurabilità e prevedibilità della durata del failover tramite checkpoint di background indiretti  
  
-   Utilizzo rallentato delle risorse durante i failover  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Raccomandazioni  
 In un ambiente di produzione è consigliabile usare indirizzi IP statici in combinazione con l'indirizzo IP virtuale di un'istanza del cluster di failover.  È consigliabile evitare l'utilizzo di DHCP in un ambiente di produzione. In caso di inattività, se il lease IP DHCP scade, per la registrazione del nuovo indirizzo IP DHCP associato al nome DNS viene richiesto tempo aggiuntivo.  
  
##  <a name="failover-cluster-instance-overview"></a><a name="Overview"></a> Panoramica dell'istanza del cluster di failover  
 Un'istanza del cluster di failover viene eseguita in un gruppo di risorse WSFC con uno o più nodi WSFC. Quando l'istanza del cluster di failover viene avviata, uno dei nodi presume la proprietà del gruppo di risorse e attiva la modalità online dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Le risorse di proprietà di questo nodo includono:  
  
-   Nome della rete  
  
-   Indirizzo IP  
  
-   Dischi condivisi  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Servizio del motore di database  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Analysis Services, se installato  
  
-   Una risorsa di condivisione file, se la funzionalità FILESTREAM è installata  
  
 In qualsiasi momento, solo il proprietario del gruppo di risorse, e nessun altro nodo nell'istanza del cluster di failover, esegue i rispettivi servizi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel gruppo di risorse. Quando si verifica un failover, ad esempio un failover automatico o un failover pianificato, si verifica la seguente sequenza di eventi:  
  
1.  A meno che non si verifichi un errore hardware o di sistema, tutte le pagine dirty della cache del buffer vengono scritte su disco.  
  
2.  Tutti i rispettivi servizi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel gruppo di risorse vengono arrestati sul nodo attivo.  
  
3.  La proprietà del gruppo di risorse viene trasferita a un altro nodo nell'istanza del cluster di failover.  
  
4.  I servizi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono avviati dal nuovo proprietario del gruppo di risorse.  
  
5.  Le richieste di connessione dell'applicazione client vengono dirette automaticamente al nuovo nodo attivo utilizzando lo stesso nome della rete virtuale (VNN, Virtual Network Name).  
  
 L'istanza del cluster di failover è online finché l'integrità del quorum del cluster WSFC sottostante è buona (la maggioranza dei nodi WSFC del quorum è disponibile come destinazione del failover automatico). Quando il cluster WSFC perde il quorum per un errore hardware, software, di rete o per la configurazione impropria del quorum, l'intero cluster WSFC, insieme all'istanza del cluster di failover viene portato offline. Quindi, in questo scenario di failover non pianificato è richiesto l'intervento manuale per ristabilire il quorum nei nodi disponibili restanti in modo da riportare il cluster WSFC e l'istanza del cluster di failover online. Per altre informazioni, vedere [Modalità quorum WSFC e configurazione del voto &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
### <a name="predictable-failover-time"></a>Tempo stimabile di failover  
 A seconda di quando l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ha eseguito l'ultima operazione di checkpoint, è possibile che vi sia una quantità sostanziale di pagine dirty nella cache del buffer. Di conseguenza, i failover durano per tutto il tempo che occorre per scrivere le pagine dirty restanti sul disco che può comportare un tempo di failover lungo e imprevedibile. A partire da [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]l'istanza del cluster di failover può usare checkpoint indiretti per limitare la quantità di pagine dirty mantenute nella cache del buffer. Sebbene vengano utilizzate risorse aggiuntive sotto un carico di lavoro normale, la durata del failover è più stimabile e configurabile. Questa operazione si rivela utile quando il contratto di servizio dell'organizzazione specifica l'obiettivo del tempo di recupero (RTO, Recovery Time Objective) per la soluzione di disponibilità elevata. Per ulteriori informazioni sui checkpoint indiretti, vedere [Indirect Checkpoints](../../../relational-databases/logs/database-checkpoints-sql-server.md#IndirectChkpt).  
  
### <a name="reliable-health-monitoring-and-flexible-failover-policy"></a>Criteri di failover flessibili e monitoraggio dell'integrità affidabile  
 Una volta avviata l'istanza del cluster di failover, il servizio WSFC esegue il monitoraggio dell'integrità del cluster del servizio WSFC sottostante e l'integrità dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . A partire da [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]il servizio WSFC usa una connessione dedicata per eseguire il polling dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] attiva per la diagnostica dettagliata dei componenti tramite una stored procedure di sistema. L'implicazione di ciò è costituita da tre riduzioni:  
  
-   La connessione dedicata all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] consente di eseguire il polling costantemente in modo affidabile per la diagnostica dei componenti, anche quando il carico di lavoro dell'istanza del cluster di failover è elevato. In tal modo è possibile distinguere tra un sistema in carico di lavoro elevato e un sistema che effettivamente è in condizione di errore, evitando pertanto problemi quali failover falsi.  
  
-   La diagnostica dettagliata dei componenti consente di configurare più criteri di failover flessibili per permettere di scegliere le condizioni di errore che attivano i failover e quelle che non li attivano.  
  
-   La diagnostica dettagliata dei componenti abilita anche una migliore risoluzione dei problemi dei failover automatici in modo retroattivo. Le informazioni di diagnostica vengono archiviate in file di log che vengono collocati insieme ai log degli errori di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . È possibile caricare i file nel visualizzatore file di log per controllare gli stati dei componenti precedenti all'occorrenza del failover per determinare la causa del failover.  
  
 Per altre informazioni, vedere [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
##  <a name="elements-of-a-failover-cluster-instance"></a><a name="FCIelements"></a> Elementi di un'istanza del cluster di failover  
 Un'istanza del cluster di failover è costituita da un set di server fisici (nodi) con configurazioni hardware simili e configurazioni software identiche, tra cui la versione del sistema operativo e il livello della patch e la versione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , il livello della patch, i componenti e il nome dell'istanza. La configurazione del software identica è necessaria assicurarsi che l'istanza del cluster di failover possa essere totalmente funzionale quando si verifica un errore tra i nodi.  
  
 Gruppo di risorse WSFC  
 Un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene eseguita in un gruppo di risorse WSFC. Ogni nodo nel gruppo di risorse gestisce una copia sincronizzata delle impostazioni di configurazione e delle chiavi del Registro di sistema del checkpoint per assicurare la funzionalità completa dell'istanza del cluster di failover dopo un failover e solo un nodo per volta (il nodo attivo) possiede nel cluster il gruppo di risorse. Il servizio WSFC gestisce il cluster del server, la configurazione del quorum, i criteri del failover e le operazioni del failover, nonché il nome di rete virtuale e gli indirizzi IP virtuali per l'istanza del cluster di failover. In caso di errore (hardware, del sistema operativo, dell'applicazione o del servizio) o di un aggiornamento pianificato, la proprietà del gruppo di risorse viene spostata a un altro nodo dell'istanza del cluster di failover. Il numero di nodi supportati in un gruppo di risorse WSFC dipende dall'edizione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Inoltre, lo stesso cluster WSFC può eseguire più istanze del cluster di failover (più gruppi di risorse), a seconda della capacità hardware, ad esempio CPU, memoria e numero di dischi.  
  
 Binari di SQL Server  
 I file binari del prodotto sono installati in locale in ogni nodo dell'istanza del cluster di failover, come avviene per le installazioni autonome di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Tuttavia, durante l'avvio, i servizi non vengono avviati automaticamente, ma gestiti dal servizio WSFC.  
  
 Archiviazione  
 Diversamente dal gruppo di disponibilità, un'istanza del cluster di failover deve usare l'archiviazione condivisa tra tutti i nodi dell'istanza del cluster di failover per l'archiviazione di log e database. L'archiviazione condivisa può avvenire sotto forma di dischi di cluster WSFC, dischi su una rete SAN, Spazi di archiviazione diretta (S2D) o condivisioni di file su un protocollo SMB. In tal modo, tutti i nodi nell'istanza del cluster di failover dispongono della stessa vista di dati dell'istanza quando si verifica un failover. Tuttavia, questo vuole dire che l'archiviazione condivisa potenzialmente potrebbe essere l'unico punto di errore e l'istanza del cluster di failover dipende dalla soluzione di archiviazione sottostante per assicurare la protezione dei dati.  
  
 Nome rete  
 Il nome della rete virtuale (VNN, Virtual Network Name) fornisce un punto di connessione unificato per l'istanza del cluster di failover. In tal modo le applicazioni possono connettersi al nome VNN senza il bisogno di conoscere il nodo al momento attivo. Quando si verifica un failover, il VNN viene registrato nel nuovo nodo attivo dopo l'avvio. Questo processo non è visibile al client o all'applicazione che si connette a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , riducendo di conseguenza i tempi d'inattività dell'applicazione o dei client durante un errore.  
  
 IP virtuali  
 Nel caso di un'istanza del cluster di failover su più subnet, un indirizzo IP virtuale viene assegnato a ogni subnet nell'istanza del cluster di failover. Durante un failover, il VNN sul server DNS viene aggiornato per puntare all'indirizzo IP virtuale per la rispettiva subnet. Le applicazioni e i client si possono connettere quindi all'istanza del cluster di failover utilizzando lo stesso VNN dopo un failover su più subnet.  
  
##  <a name="sql-server-failover-concepts-and-tasks"></a><a name="ConceptsAndTasks"></a> Concetti e attività di failover di SQL Server  
  
|Concetti e attività|Argomento|  
|------------------------|-----------|  
|Descrive il meccanismo di rilevamento dell'errore e i criteri di failover flessibili.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Descrive i concetti dell'amministrazione e della manutenzione dell'istanza del cluster di failover.|[Gestione e manutenzione dell'istanza del cluster di failover](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Descrive la configurazione di più subnet e concetti|[Clustering su più subnet di SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)|  
  
##  <a name="related-topics"></a><a name="RelatedTopics"></a> Argomenti correlati  
  
|**Descrizioni argomento**|**Argomento**|  
|----------------------------|---------------|  
|Descrive come installare e configurare una nuova istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Descrive la modalità di aggiornamento a un cluster di failover di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] .|[Aggiornare un'istanza del cluster di failover di SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|Descrive i concetti relativi al clustering di failover di Windows e fornisce collegamenti ad attività correlate al clustering di failover di Windows|[!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]: [Panoramica dei cluster di failover](https://go.microsoft.com/fwlink/?LinkId=177878)<br /><br /> [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)] R2: [Panoramica dei cluster di failover](https://go.microsoft.com/fwlink/?LinkId=177879)|  
|Descrive le diversità tra il concetto di nodo in un'istanza del cluster di failover e di replica in un gruppo di disponibilità e le considerazioni per l'utilizzo di un'istanza del cluster di failover per ospitare una replica per un gruppo di disponibilità.|[Clustering di failover e gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)|  
  
  
