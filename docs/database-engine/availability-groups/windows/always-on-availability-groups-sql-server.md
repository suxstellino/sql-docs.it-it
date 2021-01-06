---
title: 'Gruppi di disponibilità: una soluzione per la disponibilità elevata e il ripristino di emergenza'
description: I gruppi di disponibilità Always On sono una soluzione per la disponibilità elevata e il recupero di emergenza di SQL Server che offre un'alternativa di livello enterprise al mirroring del database con funzionalità più estese. Informazioni sulle nozioni di base e le funzionalità di questa soluzione.
ms.custom: seodec18
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- secondary replicas, see Availability Groups [SQL Server]
- availability [SQL Server]
- AlwaysOn [SQL Server], see Availability Groups [SQL Server]
- Availability Groups [SQL Server]
ms.assetid: aa427606-8422-4656-b205-c9e665ddc8c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7f9b06bd6805e75c053037b18fc6870ef1469500
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642206"
---
# <a name="always-on-availability-groups-a-high-availability-and-disaster-recovery-solution"></a>Gruppi di disponibilità Always On: una soluzione per la disponibilità elevata e il ripristino di emergenza
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  La funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] è una soluzione di disponibilità elevata e recupero di emergenza che offre un'alternativa di livello enterprise al mirroring del database. Introdotta in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aumenta la disponibilità di un set di database utente per un'azienda. Un *gruppo di disponibilità* supporta un ambiente di failover per un set discreto di database utente, noti come *database di disponibilità*, su cui si verifica il failover. Un gruppo di disponibilità supporta un set di database primari di lettura e scrittura e da uno a otto set di database secondari corrispondenti. Facoltativamente, i database secondari possono essere resi disponibili per l'accesso di sola lettura e/o alcune operazioni di backup.  
  
 Per un gruppo di disponibilità il failover si verifica al livello di una replica di disponibilità. I failover non sono dovuti a database ritenuti sospetti in seguito a una perdita di un file di dati, all'eliminazione di un database o al danneggiamento di un log delle transazioni.  
 
 >[!NOTE]
 >Il nome formale completo di questa funzionalità di disponibilità è Gruppi di disponibilità Always On. L'abbreviazione è AG (Availability Group), non AOAG o AAG. 
  
##  <a name="benefits"></a><a name="Benefits"></a> Vantaggi  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] è incluso un ampio set di opzioni con cui è possibile migliorare la disponibilità del database e che consentono un uso ottimale delle risorse. I componenti chiave sono i seguenti:  
  
-   Supporta fino a nove repliche di disponibilità. Una *replica di disponibilità* è la creazione di un'istanza di un gruppo di disponibilità ospitata da un'istanza specifica di SQL Server e che mantiene una copia locale di ogni database di disponibilità che appartiene al gruppo di disponibilità. Ogni gruppo di disponibilità supporta una replica primaria e fino a otto repliche secondarie. Per altre informazioni, vedere [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Ogni replica di disponibilità deve risiedere su un nodo diverso di un singolo cluster WSFC (Windows Server Failover Clustering). Per altre informazioni su prerequisiti, restrizioni e consigli per i gruppi di disponibilità, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Supporta modalità di disponibilità alternative, come segue:  
  
    -   *Modalità commit asincrono*. La modalità di disponibilità è una soluzione di recupero di emergenza che offre risultati ottimali quando le repliche di disponibilità vengono distribuite a distanze considerevoli.  
  
    -   *Modalità commit sincrono*. La modalità di disponibilità privilegia la disponibilità elevata e la protezione dei dati rispetto alle prestazioni, aumentando tuttavia la latenza delle transazioni. Un determinato gruppo di disponibilità è in grado di supportare fino a tre repliche di disponibilità con commit sincrono, inclusa la replica primaria corrente.  
  
     Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). 

     [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] aumenta il numero massimo di repliche sincrone fino a 5. Il numero massimo era 3 in [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)]. È possibile configurare questo gruppo di cinque repliche per il failover automatico all'interno del gruppo. Sono presenti una replica primaria e 4 repliche secondarie sincrone.
  
-   Supporta molti formati di failover del gruppo di disponibilità: failover automatico, failover manuale pianificato (in genere definito semplicemente "failover manuale") e failover manuale forzato (in genere definito semplicemente "failover forzato"). Per altre informazioni, vedere [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Permette di configurare una determinata replica di disponibilità per supportare una o entrambe le seguenti funzionalità delle repliche secondarie attive:  
  
    -   Accesso alla connessione in sola lettura che permette connessioni in sola lettura alla replica per accedere e leggere i relativi database quando viene eseguita come replica secondaria. Per altre informazioni, vedere [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
    -   Esecuzione di operazioni di backup sui relativi database quando viene eseguita come replica secondaria. Per altre informazioni, vedere [Repliche secondarie attive: Backup in repliche secondarie &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
     L'uso delle funzionalità secondarie attive migliora l'efficienza IT e riduce i costi tramite un migliore uso delle risorse dell'hardware secondario. Inoltre, la ripartizione delle applicazioni con finalità di lettura e dei processi di backup alle repliche secondarie permette di migliorare le prestazioni sulla replica primaria.  
  
-   Supporta un listener del gruppo di disponibilità per ogni gruppo di disponibilità. Un *listener del gruppo di disponibilità* è un nome del server a cui i client possono connettersi per accedere a un database in una replica primaria o secondaria di un gruppo di disponibilità Always On. I listener del gruppo di disponibilità indirizzano le connessioni in ingresso alla replica primaria o a una replica secondaria in sola lettura. Il listener permette un failover rapido dell'applicazione dopo il failover del gruppo di disponibilità. Per altre informazioni, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
-   Supporta criteri di failover flessibili per un maggiore controllo del failover del gruppo di disponibilità. Per altre informazioni, vedere [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Supporta il ripristino automatico della pagina per la protezione da danneggiamenti di pagina. Per ulteriori informazioni, vedere [Correzione automatica della pagina &#40;Gruppi di disponibilità/Mirroring del database&#41;](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md).  
  
-   Supporta crittografia e compressione, che forniscono il trasporto sicuro a prestazioni elevate.  
  
-   Fornisce un set integrato di strumenti per semplificare la distribuzione e la gestione dei gruppi di disponibilità, tra cui:  
  
    -   [!INCLUDE[tsql](../../../includes/tsql-md.md)] per la creazione e la gestione di gruppi di disponibilità. Per altre informazioni, vedere [Panoramica delle istruzioni Transact-SQL per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , come segue:  
  
        -   La [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] permette di creare e configurare un gruppo di disponibilità. In alcuni ambienti, questa procedura guidata prepara automaticamente i database secondari e avvia la sincronizzazione dei dati per ognuno di essi. Per altre informazioni, vedere [Usare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md).  
  
        -   La [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] aggiunge uno o più database primari a un gruppo di disponibilità esistente. In alcuni ambienti, questa procedura guidata prepara automaticamente i database secondari e avvia la sincronizzazione dei dati per ognuno di essi. Per altre informazioni, vedere [Usare la procedura guidata Aggiungi database a gruppo di disponibilità (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
        -   La [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] aggiunge una o più repliche secondarie a un gruppo di disponibilità esistente. In alcuni ambienti, questa procedura guidata prepara automaticamente i database secondari e avvia la sincronizzazione dei dati per ognuno di essi. Per altre informazioni, vedere [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
        -   La [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)] avvia un failover manuale su un gruppo di disponibilità. A seconda della configurazione e dello stato della replica secondaria specificata come destinazione del failover, la procedura guidata può eseguire un failover pianificato o un failover manuale forzato. Per altre informazioni, vedere [Usare la procedura guidata Failover gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md).  
  
    -   [!INCLUDE[ssAoDash](../../../includes/ssaodash-md.md)] monitora i gruppi di disponibilità Always On, le repliche di disponibilità e i database di disponibilità e valuta i risultati per i criteri Always On. Per altre informazioni, vedere [Usare il Dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
    -   Nel riquadro Dettagli Esplora oggetti sono visualizzate informazioni di base sui gruppi di disponibilità esistenti. Per altre informazioni, vedere [Utilizzare Dettagli Esplora oggetti per monitorare Gruppi di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Cmdlet di PowerShell. Per altre informazioni, vedere [Panoramica dei cmdlet di PowerShell per Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md).  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Termini e definizioni  
 **gruppo di disponibilità**  
 Contenitore per un set di database, i *database di disponibilità*, su cui si verifica il failover.  
  
 **database di disponibilità**  
 Database che appartiene a un gruppo di disponibilità. Per ogni database di disponibilità, il gruppo di disponibilità gestisce una sola copia di lettura e scrittura (il *database primario*) e da una a otto copie di sola lettura (*database secondari*).  
  
 **database primario**  
 Copia di lettura e scrittura di un database di disponibilità.  
  
 **database secondario**  
 Copia di sola lettura di un database di disponibilità.  
  
 **replica di disponibilità**  
 Istanza di un gruppo di disponibilità ospitata da un'istanza specifica di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che mantiene una copia locale di ogni database di disponibilità che appartiene al gruppo di disponibilità. Sono disponibili due tipi di replica di disponibilità: una *replica primaria* e da una a otto *repliche secondarie*.  
  
 **replica primaria**  
 Replica di disponibilità che rende disponibili i database primari per le connessioni in lettura e scrittura dai client e invia i record del log delle transazioni per ogni database primario a ogni replica secondaria.  
  
 **replica secondaria**  
 Replica di disponibilità che mantiene una copia secondaria di ogni database di disponibilità e che rappresenta la destinazione potenziale del failover per il gruppo di disponibilità. Facoltativamente, una replica secondaria può supportare l'accesso in sola lettura ai database secondari creando backup sui database secondari.  
  
 **listener del gruppo di disponibilità**  
 Nome del server a cui i client possono connettersi per accedere a un database in una replica primaria o secondaria di un gruppo di disponibilità Always On. I listener del gruppo di disponibilità indirizzano le connessioni in ingresso alla replica primaria o a una replica secondaria in sola lettura.  
  
> [!NOTE]  
>  Per altre informazioni, vedere [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="interoperability-and-coexistence-with-other-database-engine-features"></a><a name="Interoperability"></a> Interoperabilità e coesistenza con altre funzionalità del motore di database  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] può essere usato con le funzionalità o i componenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]seguenti:  
  
-   [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
-   [Informazioni sul rilevamento delle modifiche &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
-   [Database indipendenti](../../../relational-databases/databases/contained-databases.md)  
  
-   [Crittografia del database](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
-   [Snapshot di database](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)  
  
-   [FILESTREAM](../../../relational-databases/blob/filestream-sql-server.md)  
  
-   [FileTable](../../../relational-databases/blob/filetables-sql-server.md)  
  
-   [Log shipping](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
-   [Archivio BLOB remoto (RBS)](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
-   [Replica](../../../relational-databases/replication/sql-server-replication.md)  
  
-   [Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
-   [SQL Server Agent](../../../ssms/agent/sql-server-agent.md)  
  
-   [Reporting Services](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)  
  
> [!WARNING]  
>  Per informazioni sulle restrizioni e le limitazioni per l'uso di altre funzionalità con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Gruppi di disponibilità Always On: Interoperabilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Introduzione ai gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   **Blog:**  
  
     [SQL Server AlwaysOn Team Blog: blog ufficiale del team di SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
-   **Video:**  
  
     [Pagina relativa alla prima parte riguardante l'introduzione della soluzione a disponibilità elevata di prossima generazione della serie AlwaysOn di Microsoft SQL Server nome in codice "Denali"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Pagina relativa alla seconda parte riguardante la compilazione di una soluzione a disponibilità elevata critica tramite AlwasyOn della serie AlwaysOn di Microsoft SQL Server nome in codice "Denali"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **White paper:**  
  
     [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Configurazione di un'istanza del server per i Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Amministrazione di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Monitoraggio di Gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Panoramica delle istruzioni Transact-SQL per i gruppi di disponibilità Always On &#40;SQL Server&#41;.](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Panoramica dei cmdlet di PowerShell per Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
