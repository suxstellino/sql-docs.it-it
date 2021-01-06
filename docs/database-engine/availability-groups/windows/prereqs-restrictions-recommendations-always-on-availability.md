---
title: 'Gruppo di disponibilità: Prerequisiti, restrizioni e raccomandazioni'
description: Descrizione dei prerequisiti, delle restrizioni e dei consigli per la distribuzione di un gruppo di disponibilità Always On in SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], WSFC clusters
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], Failover Cluster Instances
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: edbab896-42bb-4d17-8d75-e92ca11f7abb
author: cawrites
ms.author: chadam
ms.openlocfilehash: ec3a84dc54dcaf373f8fd817c259602c7901410d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642534"
---
# <a name="prerequisites-restrictions-and-recommendations-for-always-on-availability-groups"></a>Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Questo articolo illustra considerazioni sulla distribuzione di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], inclusi prerequisiti, restrizioni e indicazioni su computer host, cluster WSFC (Windows Server Failover Cluster), istanze del server e gruppi di disponibilità. Per ognuno di questi componenti sono indicate le eventuali considerazioni sulla sicurezza e le autorizzazioni richieste.  
  
> [!IMPORTANT]  
>  Prima di distribuire [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], si consiglia di leggere tutte le sezioni presenti in questo argomento.  
    
##  <a name="net-hotfixes-that-support-availability-groups"></a><a name="DotNetHotfixes"></a> Hotfix di .NET che supportano i gruppi di disponibilità  
 A seconda dei componenti e delle funzionalità di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] che verranno usati con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], potrebbe essere necessario aggiungere hotfix di .NET aggiuntivi identificati nella seguente tabella. Gli hotfix possono essere installati in qualsiasi ordine.  
  
|Funzionalità dipendente|Hotfix|Collegamento|  
|-----------------------|------------|----------|  
|[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]|L'hotfix per .NET 3.5 SP1 aggiunge il supporto a SQL Client per le funzionalità AlwaysOn di Read-intent, readonly e multisubnetfailover. L'hotfix deve essere installato in ogni server di report di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .|KB 2654347: [Hotfix per .NET 3.5 SP1 per aggiungere supporto alle funzionalità Always On](https://go.microsoft.com/fwlink/?LinkId=242896)|  
  

###  <a name="checklist-requirements-windows-system"></a><a name="SystemRequirements"></a> Elenco di controllo: requisiti (sistema Windows)  
 Per supportare la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , assicurarsi che ogni computer che fa parte di uno o più gruppi di disponibilità soddisfi i requisiti essenziali seguenti:  
  
|Requisito|Collegamento|  
|-----------------|----------|  
|Assicurarsi che il sistema non sia un controller di dominio.|I gruppi di disponibilità non sono supportati nei controller di dominio.|  
|Assicurarsi che in ogni computer sia eseguita la versione di Windows Server 2012 o successive.|[Requisiti hardware e software per l'installazione di SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)|  
|Assicurarsi che ogni computer sia un nodo in un cluster WSFC.|[Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)|  
|Assicurarsi che nel cluster WSFC siano contenuti nodi sufficienti per supportare le configurazioni dei gruppi di disponibilità.|Un nodo del cluster può ospitare una sola replica per un gruppo di disponibilità. Lo stesso nodo non può ospitare due repliche dallo stesso gruppo di disponibilità. Il nodo del cluster può partecipare a più gruppi di disponibilità, con una replica da ogni gruppo. <br /><br /> Chiedere agli amministratori del database il numero di nodi del cluster necessario per supportare le repliche di disponibilità dei gruppi di disponibilità pianificati.<br /><br /> [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  

  
> [!IMPORTANT]  
>  Inoltre, assicurarsi che l'ambiente sia configurato correttamente per la connessione a un gruppo di disponibilità. Per altre informazioni, vedere [Connettività client AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="recommendations-for-computers-that-host-availability-replicas-windows-system"></a><a name="ComputerRecommendations"></a> Indicazioni per computer in cui sono ospitate repliche di disponibilità (sistema Windows)  
  
-   **Sistemi simili:**  Per un determinato gruppo di disponibilità, tutte le repliche di disponibilità devono essere in sistemi simili, in grado di gestire carichi di lavoro identici.  
  
-   **Schede di rete dedicate:**  Per ottenere prestazioni ottimali, usare una scheda di rete dedicata (scheda di interfaccia di rete) per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Spazio su disco sufficiente:**  Ogni computer in cui un'istanza del server ospita una replica di disponibilità deve disporre di spazio su disco sufficiente per tutti i database nel gruppo di disponibilità. Tenere presente che, con l'aumentare delle dimensioni dei database primari, i database secondari corrispondenti aumentano di conseguenza.  
  
###  <a name="permissions-windows-system"></a><a name="PermissionsWindows"></a> Autorizzazioni (sistema Windows)  
 Per amministrare un cluster WSFC, l'utente deve essere un amministratore di sistema in ogni nodo del cluster.  
  
 Per altre informazioni sull'account per l'amministrazione del cluster, vedere [Appendice A: Requisiti di un cluster di failover](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197454(v=ws.10)).  
  
###  <a name="related-tasks-windows-system"></a><a name="RelatedTasksWindows"></a> Attività correlate (sistema Windows)  
  
|Attività|Collegamento|  
|----------|----------|  
|Impostare il valore HostRecordTTL.|[Modificare HostRecordTTL (tramite Windows PowerShell)](#ChangeHostRecordTTLps)|  
  
####  <a name="change-the-hostrecordttl-using-windows-powershell"></a><a name="ChangeHostRecordTTLps"></a> Modificare HostRecordTTL (tramite Windows PowerShell)  
  
1.  Aprire la finestra di PowerShell con **Esegui come amministratore**.  
  
2.  Importare il modulo FailoverClusters.  
  
3.  Usare il cmdlet **Get-ClusterResource** per cercare la risorsa nome di rete, quindi usare il cmdlet **Set-ClusterParameter** per impostare il valore **HostRecordTTL** , come segue:  
  
     Get-ClusterResource " *\<NetworkResourceName>* " | Set-ClusterParameter HostRecordTTL *\<TimeInSeconds>*  
  
     Nell'esempio di PowerShell seguente impostare HostRecordTTL su 300 secondi per una risorsa nome di rete denominata `SQL Network Name (SQL35)`.  
  
    ```powershell
    Import-Module FailoverClusters  
  
    $nameResource = "SQL Network Name (SQL35)"  
    Get-ClusterResource $nameResource | Set-ClusterParameter ClusterParameter HostRecordTTL 300  
    ```  
  
    > [!TIP]  
    >  Ogni volta che viene aperta una nuova finestra di PowerShell, è necessario importare il modulo **FailoverClusters** .  
  
##### <a name="related-content-powershell"></a>Contenuto correlato (PowerShell)  
  
-   [Clustering and High-Availability](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (Failover Clustering and Network Load Balancing Team Blog) (Clustering e disponibilità elevata - Blog del team di clustering di failover e bilanciamento del carico di rete)  
  
-   [Introduzione a Windows PowerShell in un cluster di failover](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Comandi di risorse cluster e cmdlet di Windows PowerShell equivalenti](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
###  <a name="related-content-windows-system"></a><a name="RelatedContentWS"></a> Contenuto correlato (sistema Windows)  
  
-   [Pagina relativa alla configurazione delle impostazioni DNS in un cluster di failover multisito](https://technet.microsoft.com/library/dd197562\(WS.10\).aspx)  
  
-   [Post sulla registrazione DNS con la risorsa del nome di rete](https://techcommunity.microsoft.com/t5/failover-clustering/dns-registration-with-the-network-name-resource/ba-p/371482)  
  

##  <a name="sql-server-instance-prerequisites-and-restrictions"></a><a name="ServerInstance"></a> Prerequisiti e restrizioni dell'istanza di SQL Server  
 Per ogni gruppo di disponibilità è richiesto un set di partner di failover, noti come *repliche di disponibilità*, ospitati da istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Un'istanza del server specifica può essere un'*istanza autonoma* o un'*istanza del cluster di failover* (FCI) di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 **Contenuto della sezione**  
  
-   [Elenco di controllo: Prerequisiti](#PrerequisitesSI)  
  
-   [Utilizzo del thread da parte dei gruppi di disponibilità](#ThreadUsage)  
  
-   [Autorizzazioni](#PermissionsSI)  
  
-   [Attività correlate](#RelatedTasksSI)  
  
-   [Contenuto correlato](#RelatedContentSI)  
  
###  <a name="checklist-prerequisites-server-instance"></a><a name="PrerequisitesSI"></a> Elenco di controllo: prerequisiti (istanza del server)  
  
|Prerequisito|Collegamenti|  
|------------------|-----------|  
|Il computer host deve essere un nodo WSFC. Le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui sono ospitate le repliche di disponibilità di uno specifico gruppo di disponibilità risiedono in nodi diversi del cluster. Quando viene eseguita la migrazione a un cluster diverso, un gruppo di disponibilità può risiedere temporaneamente in due cluster. SQL Server 2016 introduce i gruppi di disponibilità distribuita. In un gruppo di disponibilità distribuita due gruppi di disponibilità gruppo risiedono in cluster diversi.|[Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)<br /><br /> [Clustering di failover e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)<br/> <br/> [Gruppi di disponibilità distribuiti (gruppi di disponibilità AlwaysOn)](./distributed-availability-groups.md)|  
|Se si desidera che in un gruppo di disponibilità venga utilizzato Kerberos:<br /><br /> In tutte le istanze del server in cui è ospitata una replica di disponibilità per il gruppo di disponibilità deve essere utilizzato lo stesso account del servizio SQL Server.<br /><br /> L'amministratore di dominio deve registrare manualmente un nome dell'entità servizio (SPN, Service Principal Name) con Active Directory nell'account del servizio SQL Server per il nome di rete virtuale del listener del gruppo di disponibilità. Se il nome SPN è registrato in un account diverso da quello del servizio SQL Server, l'autenticazione non verrà completata.<br /><br /> <br /><br /> <b>\*\* Importante \*\*</b> Se si modifica l'account del servizio SQL Server, l'amministratore di dominio dovrà registrare di nuovo manualmente il nome SPN.|[Registrazione di un nome dell'entità servizio per le connessioni Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)<br /><br /> **Breve spiegazione:**<br /><br /> A Kerberos e ai nomi SPN viene applicata l'autenticazione reciproca. Tramite il nome SPN viene eseguito il mapping all'account di Windows mediante il quale vengono avviati i servizi SQL Server. Se la registrazione del nome SPN non viene eseguita correttamente o presenta un errore, tramite il livello di sicurezza di Windows non è possibile determinare l'account associato al nome SPN e l'autenticazione Kerberos non può essere utilizzata.<br /><br /> <br /><br /> Nota: NTLM non presenta questo requisito.|  
|Se si intende utilizzare un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ospitare una replica di disponibilità, assicurarsi di aver compreso le restrizioni su questo tipo di istanza e che vengano soddisfatti i relativi requisiti.|[Prerequisiti e requisiti sull'uso di un'istanza del cluster di failover di SQL Server per ospitare una replica di disponibilità](#FciArLimitations), più avanti in questo articolo|  
|Per partecipare a un gruppo di disponibilità Always On, ogni istanza del server deve eseguire la stessa versione di SQL Server.|Edizioni e funzionalità supportate per [SQL 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true), [SQL 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md?view=sql-server-2016&preserve-view=true), [SQL 2017](../../../sql-server/editions-and-components-of-sql-server-2017.md?view=sql-server-2017&preserve-view=true).|  
|In tutte le istanze del server in cui sono ospitate repliche di disponibilità per un gruppo di disponibilità devono essere utilizzate le stesse regole di confronto di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Impostazione o modifica di regole di confronto del server](../../../relational-databases/collations/set-or-change-the-server-collation.md)|  
|Abilitare la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in ogni istanza del server in cui sarà ospitata una replica di disponibilità per qualsiasi gruppo di disponibilità. In un computer specificato è possibile abilitare tante istanze del server per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] quante sono quelle supportate dall'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)<br /><br /> <br /><br /> <b>\*\* Importante \*\*</b> Se si elimina e si ricrea un cluster WSFC, è necessario disabilitare e riabilitare la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in ogni istanza del server abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nel cluster originale.|  
|Per ogni istanza del server è necessario un endpoint del mirroring del database. Si noti che questo endpoint è condiviso da tutte le repliche di disponibilità, dai partner di mirroring di database, nonché dai server di controllo nell'istanza del server.<br /><br /> Se un'istanza del server selezionata come host per una replica di disponibilità è in esecuzione in un account utente di dominio e non dispone ancora di un endpoint del mirroring del database, tramite la [Creazione guidata Gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md) (o [Procedura guidata Aggiungi replica a gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)) è possibile creare l'endpoint e concedere l'autorizzazione CONNECT all'account del servizio dell'istanza del server. Se, tuttavia, il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene eseguito come account predefinito, ad esempio Sistema locale, Servizio locale o Servizio di rete, oppure come account non di dominio, è necessario usare certificati per l'autenticazione dell'endpoint e non sarà possibile creare un endpoint del mirroring del database nell'istanza del server tramite la procedura guidata. In tal caso, è consigliabile creare manualmente gli endpoint del mirroring del database prima di avviare la procedura guidata.<br /><br /> <br /><br /> <b>\*\* Nota di sicurezza \*\*</b> La sicurezza del trasporto per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] corrisponde a quella per il mirroring del database.|[Endpoint del mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)<br /><br /> [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)|  
|Se tutti i database in cui è utilizzato FILESTREAM saranno aggiunti a un gruppo di disponibilità, verificare che FILESTREAM sia abilitato in ogni istanza del server in cui sarà ospitata una replica di disponibilità per il gruppo di disponibilità.|[Abilitare e configurare FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|Se tutti i database indipendenti saranno aggiunti a un gruppo di disponibilità, verificare che l'opzione del server **contained database authentication** sia impostata su **1** in ogni istanza del server in cui sarà ospitata una replica di disponibilità per il gruppo di disponibilità.|[Opzione di configurazione del server contained database authentication](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Opzioni di configurazione del server &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
###  <a name="thread-usage-by-availability-groups"></a><a name="ThreadUsage"></a> Utilizzo del thread da parte dei gruppi di disponibilità  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] devono essere soddisfatti i requisiti seguenti:  
  
-   In caso di istanza inattiva di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] non viene utilizzato alcun thread.  
  
-   Il numero massimo di thread usato dai gruppi di disponibilità è dato dall'impostazione configurata per il numero massimo di thread del server ('**max worker threads**') meno 40.  
  
-   Nelle repliche di disponibilità ospitate in un'istanza del server specificata viene condiviso un singolo pool di thread.  
  
     I thread vengono condivisi su richiesta, come riportato di seguito:  
  
    -   In genere, sono presenti 3-10 thread condivisi, tuttavia questo numero può aumentare a seconda del carico di lavoro della replica primaria.  
  
    -   Se un thread specificato è inattivo per un certo periodo di tempo, viene rilasciato nuovamente nel pool di thread generale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . In genere, un thread inattivo viene rilasciato dopo ~15 secondi di inattività. Tuttavia, a seconda dell'ultima attività, un thread inattivo potrebbe essere mantenuto più a lungo.  

    -   Un'istanza di SQL Server usa fino a 100 thread per la fase di rollforward parallelo per le repliche secondarie. Ogni database usa fino a metà del numero totale di core CPU, ma non più di 16 thread per ogni database. Se il numero totale di thread necessari per una singola istanza supera 100, SQL Server usa un unico thread di fase di rollforward per tutti i database rimanenti. I thread di rollforward seriale vengono rilasciati dopo ~15 secondi di inattività. 
     
-   Inoltre, nei gruppi di disponibilità vengono utilizzati thread non condivisi, come riportato di seguito:  
  
    -   In ogni replica primaria viene utilizzato 1 thread di acquisizione del log per ogni database primario. Inoltre, viene utilizzato 1 thread di invio del log per ogni database secondario. I thread di invio del log vengono rilasciati dopo ~15 secondi di inattività.    
  
    -   Tramite un backup di una replica secondaria un thread viene mantenuto nella replica primaria per la durata dell'operazione di backup. 

-  SQL Server 2019 ha introdotto la fase di rollforward parallela per i database di gruppi di disponibilità ottimizzati per la memoria. In SQL Server 2016 e 2017 le tabelle basate su disco non usano la fase di rollforward parallela se un database in un gruppo di disponibilità è anche ottimizzato per la memoria. 
  
 Per altre informazioni, vedere l'articolo relativo alla [serie di informazioni su Always On - HADRON: utilizzo del pool di lavoro per database abilitati HADRON](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases) (blog degli ingegneri del supporto tecnico di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]).  
  
###  <a name="permissions-server-instance"></a><a name="PermissionsSI"></a> Autorizzazioni (istanza del server)  
  
|Attività|Autorizzazioni necessarie|  
|----------|--------------------------|  
|Creazione dell'endpoint del mirroring del database|È richiesta l'autorizzazione CREATE ENDPOINT o l'appartenenza al ruolo predefinito del server **sysadmin** .  È richiesta inoltre l'autorizzazione CONTROL ON ENDPOINT. Per altre informazioni, vedere [GRANT - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).|  
|Abilitazione di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]|È richiesta l'appartenenza al gruppo **Administrator** nel computer locale, nonché il controllo totale nel cluster WSFC.|  
  
###  <a name="related-tasks-server-instance"></a><a name="RelatedTasksSI"></a> Attività correlate (istanza del server)  
  
|Attività|Articolo|  
|----------|-----------|  
|Determinazione della presenza di un endpoint del mirroring del database|[sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)|  
|Creazione dell'endpoint del mirroring del database (se non ancora disponibile)|[Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)<br /><br /> [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)<br /><br /> [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)|  
|Abilitazione dei gruppi disponibilità|[Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)|  
  
###  <a name="related-content-server-instance"></a><a name="RelatedContentSI"></a> Contenuto correlato (istanza del server)  
  
-   [Serie di informazioni su Always On - HADRON: Uso del pool di lavoro per database abilitati HADRON](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
##  <a name="network-connectivity-recommendations"></a><a name="NetworkConnect"></a> Consigli sulla connettività di rete  
 Si consiglia di usare gli stessi collegamenti di rete per le comunicazioni tra nodi WSFC e le comunicazioni tra repliche di disponibilità.  L'utilizzo di collegamenti di rete separati può provocare comportamenti imprevisti in caso di errore di alcuni collegamenti (anche in modo intermittente).  
  
 Ad esempio, affinché un gruppo di disponibilità supporti il failover automatico, lo stato della replica secondaria che è partner di failover automatico deve essere SYNCHRONIZED. In caso di errore di collegamento di rete a questa replica secondaria (anche in modo intermittente), lo stato della replica diventa UNSYNCHRONIZED e non sarà possibile cominciare la risincronizzazione fino a quando non viene ripristinato il collegamento. Se per il cluster WSFC è richiesto un failover automatico mentre la replica secondaria non è sincronizzata, il failover automatico non si verificherà.  
  
##  <a name="client-connectivity-support"></a><a name="ClientConnSupport"></a> Supporto della connettività client  
 Per informazioni sul supporto di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] per la connettività client, vedere [Connettività client AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="prerequisites-and-restrictions-for-using-a-sql-server-failover-cluster-instance-fci-to-host-an-availability-replica"></a><a name="FciArLimitations"></a> Prerequisiti e restrizioni per l'utilizzo di un'istanza del cluster di failover di SQL Server per ospitare una replica di disponibilità  
 **Contenuto della sezione**  
  
-   [Restrizioni](#RestrictionsFCI)  
  
-   [Elenco di controllo: Prerequisiti](#PrerequisitesFCI)  
  
-   [Attività correlate](#RelatedTasksFCIs)  
  
-   [Contenuto correlato](#RelatedContentFCIs)  
  
###  <a name="restrictions-fcis"></a><a name="RestrictionsFCI"></a> Restrizioni (istanze del cluster di failover)  
  
> [!NOTE]  
> Le istanze del cluster di failover supportano i volumi condivisi cluster. Per ulteriori informazioni sui volumi condivisi cluster, vedere [Informazioni sui volumi condivisi del cluster in un cluster di failover](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11)).  
  
-   **I nodi del cluster di un'istanza del cluster di failover possono ospitare solo una replica per un gruppo di disponibilità specifico:**  Se si aggiunge una replica di disponibilità in un'istanza del cluster di failover, i nodi di WSFC che sono possibili proprietari dell'istanza del cluster di failover non possono ospitare un'altra replica per lo stesso gruppo di disponibilità.  Per evitare possibili conflitti è consigliabile configurare i proprietari possibili per l'istanza del cluster di failover. In tal modo si evita la possibilità che un singolo WSFC tenti l'hosting di due repliche di disponibilità per lo stesso gruppo di disponibilità.
  
     Inoltre tutte le altre repliche devono essere ospitate da un'istanza di SQL Server 2016 che risiede in un nodo del cluster diverso nello stesso cluster WSFC. L'unica eccezione è che quando viene eseguita la migrazione a un altro cluster, un gruppo di disponibilità può risiedere temporaneamente in due cluster. 

  >[!WARNING]
  > L'uso di Gestione cluster di failover per lo spostamento di un'*istanza di cluster di failover* che ospita un gruppo di disponibilità in un nodo che *ospita già* una replica dello stesso gruppo di disponibilità può causare la perdita della replica del gruppo di disponibilità, impedendo che venga portato online sul nodo di destinazione. Un singolo nodo di un cluster di failover non può ospitare più di una replica dello stesso gruppo di disponibilità. Per altre informazioni su come si verifica questa situazione e sulle misure da adottare, vedere il blog [Replica unexpectedly dropped in availability group](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group) (Replica rilasciata in modo inatteso nel gruppo di disponibilità). 

  
-   **Le istanze del cluster di failover non supportano il failover automatico da parte di gruppi di disponibilità:**  le istanze del cluster di failover non supportano il failover automatico da gruppi di disponibilità, quindi le repliche di disponibilità ospitate da un'istanza del cluster di failover possono essere configurate solo per il failover manuale.  
  
-   **Modifica del nome di rete di un'istanza del cluster di failover:**  se è necessario modificare il nome di rete di un'istanza del cluster di failover che ospita una replica di disponibilità, è necessario rimuovere la replica dal relativo gruppo di disponibilità e riaggiungerla successivamente a questo gruppo. Non è possibile rimuovere la replica primaria, pertanto se si rinomina un'istanza del cluster di failover in cui è ospitata la replica primaria, è consigliabile eseguire il failover in una replica secondaria e successivamente rimuovere e poi riaggiungere la prima replica primaria. Si noti che la ridenominazione di un'istanza del cluster di failover può comportare la modifica dell'URL del relativo endpoint del mirroring del database. Quando si aggiunge la replica assicurarsi di specificare l'URL dell'endpoint corrente.  
  
###  <a name="checklist-prerequisites-fcis"></a><a name="PrerequisitesFCI"></a> Elenco di controllo: prerequisiti (istanze del cluster di failover)  
  
|Prerequisito|Collegamento|  
|------------------|----------|  
|Assicurarsi che in ogni istanza del cluster di failover di SQL Server sia disponibile l'archiviazione condivisa necessaria in base all'installazione dell'istanza del cluster di failover di SQL Server standard.||  
  
###  <a name="related-tasks-fcis"></a><a name="RelatedTasksFCIs"></a> Attività correlate (istanze del cluster di failover)  
  
|Attività|Articolo|  
|----------|-----------|  
|Installazione di un cluster di failover di SQL Server|[Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Aggiornamento sul posto del cluster di failover di SQL Server esistente|[Eseguire l'aggiornamento di un'istanza del cluster di failover di SQL Server &#40;installazione&#41;](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|Gestione del cluster di failover di SQL Server esistente|[Aggiungere o rimuovere nodi in un cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)|  
  
###  <a name="related-content-fcis"></a><a name="RelatedContentFCIs"></a> Contenuto correlato (istanze del cluster di failover)  
  
-   [Clustering di failover e gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Guida all'architettura Always On: compilazione di una soluzione a disponibilità elevata e con ripristino di emergenza usando istanze del cluster di failover e gruppi di disponibilità](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
##  <a name="availability-group-prerequisites-and-restrictions"></a><a name="PrerequisitesForAGs"></a> Prerequisiti e restrizioni dei gruppi di disponibilità  
 **Contenuto della sezione**  
  
-   [Restrizioni](#RestrictionsAG)  
  
-   [Requisiti](#RequirementsAG)  
  
-   [Sicurezza](#SecurityAG)  
  
-   [Attività correlate](#RelatedTasksAGs)  
  
###  <a name="restrictions-availability-groups"></a><a name="RestrictionsAG"></a> Restrizioni (gruppi di disponibilità)  
  
-   **Le repliche di disponibilità devono essere ospitate da nodi diversi di un WSFC:**  Per un determinato gruppo di disponibilità, le repliche di disponibilità devono essere ospitate da istanze del server in esecuzione in nodi diversi dello stesso WSFC. L'unica eccezione è che quando viene eseguita la migrazione a un altro cluster, un gruppo di disponibilità può risiedere temporaneamente in due cluster.  
  
    > [!NOTE]  
    >  In ogni macchina virtuale presente nello stesso computer fisico può essere ospitata una replica di disponibilità per lo stesso gruppo di disponibilità, in quanto ogni macchina virtuale costituisce un computer distinto.  
  
-   **Nome univoco del gruppo di disponibilità:**  Ogni nome di gruppo di disponibilità deve essere univoco in WSFC. La lunghezza massima consentita per il nome del gruppo di disponibilità è 128 caratteri.  
  
-   **Repliche di disponibilità:**  Ogni gruppo di disponibilità supporta una replica primaria e fino a otto repliche secondarie. È possibile eseguire tutte le repliche in modalità con commit asincrono oppure fino a un massimo di tre in modalità con commit sincrono (una replica primaria con due repliche secondarie sincrone).  
  
-   **Numero massimo di gruppi di disponibilità e database di disponibilità per computer:** il numero effettivo di database e gruppi di disponibilità che è possibile creare in un computer (macchina virtuale o computer fisico) dipende dall'hardware e dal carico di lavoro, tuttavia non esiste un limite imposto. Microsoft ha testato un massimo di 10 gruppi di disponibilità e 100 database per computer fisico, ma non si tratta di un limite vincolante. A seconda delle specifiche hardware del server e del carico di lavoro, è possibile inserire un numero maggiore di database e di gruppi di disponibilità in un'istanza di SQL Server. I segnali di sistemi di overload possono includere, a titolo esemplificativo, l'esaurimento del thread di lavoro, tempi di risposta lenti per visualizzazioni di sistema del gruppo di disponibilità e DMV e/o dump del sistema dispatcher bloccati. Assicurarsi di testare completamente l'ambiente con un carico di lavoro simile a quello di produzione per assicurare che possa gestire capacità di carico di lavoro di picco all'interno del contratto sul livello di servizio dell'applicazione. Per i contratti sul livello di servizio occorre considerare il carico in condizioni di errore e i tempi di risposta previsti.  
  
-   **Non usare Gestione cluster di failover per modificare i gruppi di disponibilità**. Lo stato di un'istanza del cluster di failover di SQL Server viene condiviso tra SQL Server e il cluster di failover di Windows (WSFC), con SQL Server che mantiene più informazioni dettagliate sullo stato delle istanze rispetto a quelle richieste dal cluster. Il modello di gestione prevede che SQL Server guidi le transazioni e sia responsabile di mantenere la visualizzazione dello stato del cluster sincronizzata con la visualizzazione dello stato di SQL Server. Se lo stato del cluster viene modificato all'esterno di SQL Server è possibile che lo stato non sia più sincronizzato tra WSFC e SQL Server e ciò potrebbe causare un comportamento imprevedibile.
  
     Ad esempio:  
  
    -   Non modificare le proprietà del gruppo di disponibilità, ad esempio i possibili proprietari.  
  
    -   Non utilizzare Gestione cluster di failover per eseguire il failover dei gruppi di disponibilità. È necessario utilizzare [!INCLUDE[tsql](../../../includes/tsql-md.md)] o [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
###  <a name="prerequisites-availability-groups"></a><a name="RequirementsAG"></a> Prerequisiti (gruppi di disponibilità)  
 Quando si crea o riconfigura una configurazione del gruppo di disponibilità, assicurarsi che si soddisfino i requisiti seguenti.  
  
|Prerequisito|Descrizione|  
|------------------|-----------------|  
|Se si intende utilizzare un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ospitare una replica di disponibilità, assicurarsi di aver compreso le restrizioni su questo tipo di istanza e che vengano soddisfatti i relativi requisiti.|[Prerequisiti e restrizioni per l'uso di un'istanza del cluster di failover di SQL Server per ospitare una replica di disponibilità](#FciArLimitations), precedentemente in questo articolo.|  
  
###  <a name="security-availability-groups"></a><a name="SecurityAG"></a> Sicurezza (gruppi di disponibilità)  
  
-   La sicurezza viene ereditata dal cluster WSFC. Windows Server Failover Clustering fornisce due livelli di sicurezza utente a livello di granularità dell'intero cluster:  
  
    -   Accesso di sola lettura  
  
    -   Controllo completo  
  
         [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] necessitano di un controllo completo. Se si abilita [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene fornito a quest'ultima il controllo completo del cluster, tramite il SID del servizio.  
  
         Non è possibile aggiungere o rimuovere direttamente la sicurezza per un'istanza del server in Gestione cluster. Per gestire sessioni di sicurezza del cluster, usare Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o l'equivalente WMI da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve disporre delle autorizzazioni per l'accesso al Registro di sistema, al cluster e così via.  
  
-   È consigliabile utilizzare la crittografia per le connessioni tra istanze del server in cui si ospitano repliche di disponibilità di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
#### <a name="permissions-availability-groups"></a>Autorizzazioni (gruppi di disponibilità)  
  
|Attività|Autorizzazioni necessarie|  
|----------|--------------------------|  
|Creazione di un gruppo di disponibilità|Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.|  
|Modifica di un gruppo di disponibilità|È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.<br /><br /> Inoltre, per la creazione di un join di un database a un gruppo di disponibilità è richiesta l'appartenenza al ruolo predefinito del database **db_owner** .|  
|Rimozione/eliminazione di un gruppo di disponibilità|È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER. Per rimuovere un gruppo di disponibilità non ospitato nel percorso della replica locale, è necessaria l'autorizzazione CONTROL SERVER o CONTROL per questo gruppo di disponibilità.|  
  
###  <a name="related-tasks-availability-groups"></a><a name="RelatedTasksAGs"></a> Attività correlate (gruppi di disponibilità)  
  
|Attività|Articolo|  
|----------|-----------|  
|Creazione di un gruppo di disponibilità|[Utilizzare il gruppo di disponibilità (Creazione guidata Gruppo di disponibilità)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [Creare un gruppo di disponibilità (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)<br /><br /> [Creare un gruppo di disponibilità (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)<br /><br /> [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)|  
|Modifica del numero di repliche di disponibilità|[Aggiungere una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Rimuovere una replica secondaria da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
|Creazione di un listener del gruppo di disponibilità|[Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|Rimozione di un gruppo di disponibilità|[Rimuovere un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)|  
  
##  <a name="availability-database-prerequisites-and-restrictions"></a><a name="PrerequisitesForDbs"></a> Prerequisiti e restrizioni dei database di disponibilità  
 Per essere idoneo a essere aggiunto a un gruppo di disponibilità, un database deve soddisfare i prerequisiti e le restrizioni riportate di seguito.  
  
 **Contenuto della sezione**  
  
-   [Requisiti](#RequirementsDb)  
  
-   [Restrizioni](#RestrictionsDb)  
  
-   [Indicazioni per computer in cui sono ospitate repliche di disponibilità (sistema Windows)](#TDEdbs)  
  
-   [Autorizzazioni](#PermissionsDbs)  
  
-   [Attività correlate](#RelatedTasksADb)  
  
###  <a name="checklist-requirements-availability-databases"></a><a name="RequirementsDb"></a> Elenco di controllo: requisiti (database di disponibilità)  
 Per essere idoneo a essere aggiunto a un gruppo di disponibilità, un database deve soddisfare i requisiti seguenti:  
  
|Requisiti|Collegamento|  
|------------------|----------|  
|Deve trattarsi di un database utente. I database di sistema non possono appartenere a un gruppo di disponibilità.||  
|Deve trovarsi nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui si crea il gruppo di disponibilità e deve essere accessibile all'istanza del server.||  
|Deve trattarsi di un database di lettura/scrittura. I database di sola lettura non possono essere aggiunti a un gruppo di disponibilità.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_read_only** = 0)|  
|Deve trattarsi di un database multiutente.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**user_access** = 0)|  
|Non deve essere utilizzato AUTO_CLOSE.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_auto_close_on** = 0)|  
|Utilizzare il modello di recupero con registrazione completa (noto anche come modalità di recupero con registrazione completa).|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**recovery_model** = 1)|  
|Deve disporre di almeno un backup completo del database.<br /><br /> Nota: Dopo aver impostato un database sulla modalità di recupero con registrazione completa, è necessario un backup completo per iniziare la catena di log del recupero con registrazione completa.|[Creazione di un backup completo del database &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)|  
|Non deve appartenere ad alcun gruppo di disponibilità esistente.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**group_database_id** = NULL)|  
|Non deve essere configurato per il mirroring del database.|[sys.database_mirroring](../../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) Se il database non fa parte del mirroring, tutte le colonne con prefisso "mirroring_" sono NULL.|  
|Prima di aggiungere un database in cui è utilizzato FILESTREAM a un gruppo di disponibilità, verificare che FILESTREAM sia abilitato in ogni istanza del server in cui è o sarà ospitata una replica di disponibilità per il gruppo di disponibilità.|[Abilitare e configurare FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|Prima di aggiungere un database indipendente a un gruppo di disponibilità, verificare che l'opzione del server **contained database authentication** sia impostata su **1** in ogni istanza del server in cui è o sarà ospitata una replica di disponibilità per il gruppo di disponibilità.|[Opzione di configurazione del server contained database authentication](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Opzioni di configurazione del server &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
> [!NOTE]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] funziona con qualsiasi livello di compatibilità del database supportato.  
  
###  <a name="restrictions-availability-databases"></a><a name="RestrictionsDb"></a> Restrizioni (database di disponibilità)  
  
-   Se il percorso del file, inclusa la lettera di unità, di un database secondario differisce da quello del database primario corrispondente, si applicano le restrizioni seguenti:  
  
    -   **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]/[!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)]:**  l'opzione **Completo** nella pagina [Seleziona sincronizzazione dati iniziale](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md) non è supportata.  
  
    -   **RESTORE WITH MOVE:**  per creare i database secondari, i file di database devono essere sottoposti a RESTORE WITH MOVE in ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui è ospitata una replica secondaria.  
  
    -   **Impatto sulle operazioni di aggiunta di file:**  un'operazione di aggiunta file successiva nella replica primaria potrebbe non riuscire nei database secondari. Questo errore può causare la sospensione dei database secondari. Questa situazione, a sua volta, può causare l'attivazione dello stato NON IN SINCRONIZZAZIONE delle repliche secondarie.  
  
        > [!NOTE]  
        >  Per informazioni sulla risposta a un'operazione di aggiunta di file errata, vedere [Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md).  
  
-   Non è possibile eliminare un database che attualmente appartiene a un gruppo di disponibilità.  
  
###  <a name="follow-up-for-tde-protected-databases"></a><a name="TDEdbs"></a> Completamento per database protetti tramite crittografia TDE  
 Se si utilizza TDE (Transparent Data Encryption), la chiave asimmetrica o il certificato per la creazione e la decrittografia di altre chiavi deve essere identico in ogni istanza del server in cui viene ospitata una replica di disponibilità per il gruppo di disponibilità. Per altre informazioni, vedere [Spostare un database protetto da TDE in un'altra istanza di SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md).  
  
###  <a name="permissions-availability-databases"></a><a name="PermissionsDbs"></a> Autorizzazioni (database di disponibilità)  
 È richiesta l'autorizzazione ALTER per il database.  
  
###  <a name="related-tasks-availability-databases"></a><a name="RelatedTasksADb"></a> Attività correlate (database di disponibilità)  
  
|Attività|Articolo|  
|----------|-----------|  
|Preparazione di un database secondario (manuale)|[Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)|  
|Creazione di un join di un database secondario a un gruppo di disponibilità (manuale)|[Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)|  
|Modifica del numero di database di disponibilità|[Aggiungere un database a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)<br /><br /> [Rimuovere un database secondario da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)<br /><br /> [Rimuovere un database primario da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [Blog del team di SQL Server Always On: blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
-   [Serie di informazioni su Always On - HADRON: Uso del pool di lavoro per database abilitati HADRON](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Clustering di failover e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Connettività client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
    
  
--------------------------------------------------