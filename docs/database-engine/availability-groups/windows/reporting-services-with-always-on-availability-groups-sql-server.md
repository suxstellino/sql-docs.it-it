---
title: Reporting Services con i gruppi di disponibilità
description: Informazioni su come configurare Reporting Services con i gruppi di disponibilità Always On in SQL Server. Le funzionalità supportate sono diverse per i diversi scenari.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: edeb5c75-fb13-467e-873a-ab3aad88ab72
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: 306a200cadc535ea9da3d5d21dcd74fb15cf278c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641520"
---
# <a name="reporting-services-with-always-on-availability-groups-sql-server"></a>Reporting Services con i gruppi di disponibilità AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento sono contenute informazioni sulla configurazione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] per l'utilizzo con i [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. I database per le origini dati del report, i database del server di report e la progettazione report rappresentano i tre scenari per l'utilizzo di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . La funzionalità supportata e la configurazione richiesta sono diverse per i tre scenari.  
  
 La possibilità di usare le repliche secondarie leggibili come origine dati Reporting Services mentre le repliche secondarie forniscono allo stesso tempo un failover per un database primario è un vantaggio chiave nell'utilizzo dei [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con le origini dei dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
 Per informazioni generali sui [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Domande frequenti su AlwaysOn per SQL Server 2012 (../../../sql-server/index.yml)](../../../sql-server/index.yml).  

##  <a name="requirements-for-using-reporting-services-and-always-on-availability-groups"></a><a name="bkmk_requirements"></a> Requisiti per l'uso di Reporting Services e dei gruppi di disponibilità AlwaysOn  
 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e il Server di report di Power BI usano .NET Framework 4.0 e supportano le proprietà della stringa di connessione [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] per l'uso con le origini dati.  
  
 Per usare i [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2014 e versioni precedenti, è necessario scaricare e installare un hotfix per .Net 3.5 SP1. L'hotfix aggiunge supporto a SQL Client per le funzionalità dei gruppi di disponibilità e per le proprietà della stringa di connessione **ApplicationIntent** e **MultiSubnetFailover**. Se l'hotfix non viene installato in ogni computer in cui si trova il server di report, allora gli utenti che provano a visualizzare un'anteprima dei report visualizzeranno un messaggio di errore simile a quello di seguito riportato e questo verrà scritto nel log di traccia del server di report:  
  
> **Messaggio di errore:** "Parola chiave non supportata 'applicationintent'"  
  
 Il messaggio viene visualizzato quando si include una delle proprietà dei [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nella stringa di connessione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , ma il server non riconosce la proprietà. Il messaggio di errore annotato verrà visualizzato quando si fa clic sul pulsante "Test connessione" nelle interfacce utente [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e quando viene visualizzata l'anteprima del report nel caso in cui vengano abilitati errori remoti nei server di report.  
  
 Per altre informazioni relative all'hotfix richiesto, vedere l'articolo della Knowledge Base [KB 2654347 sull'hotfix che introduce il supporto per le funzionalità AlwaysOn di SQL Server 2012 in .NET Framework 3.5 SP1](https://go.microsoft.com/fwlink/?LinkId=242896).  
  
 Per informazioni su altri requisiti di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] file di configurazione come **RSreportserver.config** non sono supportati come parte della funzionalità dei [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Se si apportano modifiche manuali a un file di configurazione in uno dei server di report, sarà necessario aggiornare manualmente le repliche.  
  
##  <a name="report-data-sources-and-availability-groups"></a><a name="bkmk_reportdatasources"></a> Origine dati del report e gruppi di disponibilità  
 Il comportamento delle origini dati [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] basate sui [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] può variare a seconda di come l'amministratore esegue la configurazione dell'ambiente dei gruppi di disponibilità.  
  
 Per usare i [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] per le origini dati dei report, è necessario configurare la stringa di connessione delle origini dati dei report per usare il *Nome DNS listener* del gruppo di disponibilità. Vengono di seguito riportate le origini dati supportate:  
  
-   Origine dati SQL che usano SQL Native Client.  
  
-   SQL Client con l'hotfix .Net applicato al server di report.  
  
 La stringa di connessione può anche contenere nuove proprietà di connessione AlwaysOn che configurano le richieste della query del report in modo da usare la replica secondaria per il report di sola lettura. L'utilizzo della replica secondaria per le richieste di report riduce il carico nella replica primaria di lettura e scrittura. Nell'immagine seguente viene riportato un esempio di una configurazione dei gruppi di disponibilità a tre repliche in cui le stringhe di connessione dell'origine dati [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sono state configurate con ApplicationIntent=ReadOnly. In questo esempio le richiesta della query di report vengono inviate a una replica secondaria e non alla replica primaria.  
  
 
  
 Di seguito viene riportata una stringa di connessione di esempio in cui [AvailabilityGroupListenerName] è il **Nome DNS del listener** configurato al momento della creazione delle repliche:  
  
 `Data Source=[AvailabilityGroupListenerName];Initial Catalog = AdventureWorks2016; ApplicationIntent=ReadOnly`  
  
 Mediante il pulsante **Test connessione** nelle interfacce utente [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] verrà eseguita la convalida, qualora sia possibile, stabilire una connessione, ma la configurazione del gruppo di disponibilità non verrà convalidata. Ad esempio, se viene incluso ApplicationIntent in una stringa di connessione a un server che non fa parte del gruppo di disponibilità, il parametro aggiuntivo viene ignorato e mediante il pulsante **Test connessione** verrà convalidata solo una connessione a un server specifico.  
  
 In base alla modalità di creazione e pubblicazione dei report verrà determinato dove si modifica la stringa di connessione:  
  
-   **Modalità nativa:** usare [!INCLUDE[ssRSWebPortal-Non-Markdown](../../../includes/ssrswebportal-non-markdown-md.md)] per le origini dati condivise e i report già pubblicati in un server di report in modalità nativa.  
  
-   **Modalità SharePoint:** usare le pagine di configurazione di SharePoint all'interno delle raccolte di documenti per i report già pubblicati in un server SharePoint.  
  
-   **Progettazione report:** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] o [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] al momento della creazione di nuovi report. Per altre informazioni, vedere la sezione 'Progettazione report' in questo argomento.  
  
 **Risorse aggiuntive:**  
  
-   [Gestire origini dati dei report](../../../reporting-services/report-data/manage-report-data-sources.md)  
  
-   Per altre informazioni sulle proprietà della stringa di connessione disponibili, vedere [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
-   Per altre informazioni sui listener del gruppo di continuità, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Considerazioni:** le repliche secondarie subiranno ritardi nella ricezione di modifiche di dati dalla replica primaria. I seguenti fattori possono influenzare la latenza di aggiornamento tra la replica primaria e quella secondaria:  
  
-   Numero di repliche secondarie. Aumenti di ritardo per ogni replica secondaria aggiunta alla configurazione.  
  
-   Posizione geografica e distanza tra la replica primaria e quella secondaria. Ad esempio, il ritardo è in genere maggiore se le repliche secondarie si trovano in centri dati diversi piuttosto che nello stesso edificio della replica primaria.  
  
-   Configurazione della modalità di disponibilità per ogni replica. La modalità di disponibilità determina se la replica primaria dovrà attendere la scrittura su disco delle transazioni prima di eseguire il commit delle transazioni su un database. Per altre informazioni sulla sezione 'Modalità di disponibilità', vedere [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 Quando si usano una replica secondaria di sola lettura come origine dati [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , è importante assicurare che la latenza di aggiornamento soddisfi le esigenze degli utenti del report.  
  
##  <a name="report-design-and-availability-groups"></a><a name="bkmk_reportdesign"></a> Progettazione report e gruppi di disponibilità  
 Durante la progettazione di report in [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] o di un progetto report in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], un utente può configurare una stringa di connessione dell'origine dati del report in modo che contenga nuove proprietà di connessione fornite dai [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Il supporto per le nuove proprietà di connessione dipende da dove l'utente visualizza l'anteprima del report.  
  
-   **Anteprima locale:** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] e [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] usano .Net Framework 4.0 e supportano le proprietà della stringa di connessione di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Anteprima modalità server o remota:** se viene visualizzato un messaggio di errore simile a quello seguente dopo la pubblicazione dei report nel server di report o dopo l'uso dell'anteprima in [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)], questo significa che si sta visualizzando l'anteprima dei report nel server di report e che l'hotfix di .NET Framework 3.5 SP1 per i [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] non è stato installato nel server di report.  
  
> **Messaggio di errore:** "Parola chiave non supportata 'applicationintent'"  
  
##  <a name="report-server-databases-and-availability-groups"></a><a name="bkmk_reportserverdatabases"></a> Database del server di report e gruppi di disponibilità  
 Reporting Services e il Server di report di Power BI offrono supporto limitato nell'uso dei [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con i database del server di report. I database del server di report possono essere configurati nel gruppo di disponibilità in modo da far parte di una replica; tuttavia, quando si verifica un failover, in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] non verrà usata automaticamente una replica diversa per i database del server di report. L'utilizzo di MultiSubnetFailover con i database del server di report non è supportato.  
  
 Le azioni manuali o gli script di automazione personalizzati devono essere usati per completare il failover e il recupero. Fino al completamento di queste azioni, alcune funzionalità del server di report potrebbero non funzionare correttamente dopo il failover dei [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  Quando si pianifica un failover e un ripristino di emergenza per i database del server di report, si consiglia di eseguire sempre una copia di backup della chiave di crittografia del server di report.  
  
###  <a name="differences-between-sharepoint-native-mode"></a><a name="bkmk_differences_in_server_mode"></a> Differenza tra la modalità nativa e SharePoint  
 In questa sezione vengono riepilogate le differenze tra la modalità di interazione dei server di report della modalità SharePoint e della modalità Nativa con i [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Tramite un server di report SharePoint vengono creati **3** database per ciascuna applicazione di servizio [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] creata. La connessione ai database del server di report in modalità SharePoint viene configurata in Amministrazione centrale SharePoint quando si crea l'applicazione di servizio. Nei nomi predefiniti dei database è incluso un GUID associato all'applicazione di servizio. Di seguito sono riportati i nomi di database di esempio per un server di report in modalità SharePoint:  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6TempDB  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6_Alerting  
  
 Nei server di report in modalità nativa vengono usati **2** database. Di seguito sono riportati i nomi di database di esempio per un server di report in modalità nativa:  
  
-   ReportServer  
  
-   ReportServerTempDB  
  
 La modalità nativa non supporta o usano i database di avviso e le funzionalità correlate. Configurare i server di report in modalità nativa in Gestione configurazione [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . Per la modalità SharePoint, configurare il nome database dell'applicazione di servizio in modo che sia il nome del "punto di accesso client" creato come parte della configurazione di SharePoint. Per altre informazioni sulla configurazione di SharePoint con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Configurare e gestire i gruppi di disponibilità di SQL Server per SharePoint Server (/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14))](/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14)).  
  
> [!NOTE]
>  I server di report in modalità SharePoint usano un processo di sincronizzazione tra i database dell'applicazione di servizio [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e i database del contenuto SharePoint. È importante mantenere insieme i database del server di report e i database del contenuto. Prendere in considerazione l'ipotesi di configurarli negli stessi gruppi di disponibilità in modo che eseguano il failover e il recupero come un set. Si consideri lo scenario seguente:  
> 
>  -   Ripristinare un failover in una copia del database del contenuto che non ha ricevuto gli stessi aggiornamenti recenti del database del server di report.  
> -   Il processo di sincronizzazione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] rileverà le differenze tra l'elenco di elementi nel database del contenuto e i database del server di report.  
> -   Il processo di sincronizzazione eliminerà o aggiornerà gli elementi nel database del contenuto.  
  
###  <a name="prepare-report-server-databases-for-availability-groups"></a><a name="bkmk_prepare_databases"></a> Preparare i database del server di report per i gruppi di disponibilità  
 Vengono di seguito riportati i passaggi di base per la preparazione e l'aggiunta dei database del server di report ai [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
-   Creare il proprio gruppo di disponibilità e configurare un *Nome DNS del listener*.  
  
-   **Replica primaria:** configurare i database del server di report affinché diventino parte di un gruppo di disponibilità singolo e creare una replica primaria che includa tutti i database del server di report.  
  
-   **Repliche secondarie:** creare una o più repliche secondarie. L'approccio comune per copiare i database dalla replica primaria nelle repliche secondarie è di ripristinare i database in ogni replica secondaria tramite 'RESTORE WITH NORECOVERY'. Per altre informazioni sulla creazione di repliche secondarie e la verifica del funzionamento della sincronizzazione dei dati, vedere [Avviare lo spostamento dati su un database secondario AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
-   **Credenziali del server di report:** è necessario creare le credenziali del server di report appropriate nelle repliche secondarie create in quella primaria. I passaggi esatti dipendono da quale tipo di autenticazione si sta usando nell'ambiente [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]; l'account di servizio Windows [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], l'account utente Windows, o l'autenticazione SQL Server. Per altre informazioni, vedere [Configurare una connessione del database del server di report &#40;Gestione configurazione SSRS&#41;](../../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
  
-   Aggiornare la connessione al database per usare il nome DNS del listener. Per i server di report in modalità nativa, cambiare il **Nome database del server di report** in Gestione configurazione [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . Per la modalità SharePoint, cambiare il **Nome del server di database** per le applicazioni di servizio [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
###  <a name="steps-to-complete-disaster-recovery-of-report-server-databases"></a><a name="bkmk_steps_to_complete_failover"></a> Passaggi per completare il ripristino di emergenza dei database del server di report  
 È necessario completare i seguenti passaggi dopo un failover dei [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in una replica secondaria:  
  
1.  Arrestare l'istanza del servizio SQL Agent in uso da parte del motore di database primario in cui si trovano i database di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
2.  Avviare il servizio SQL Agent nel computer che rappresenta la nuova replica primaria.  
  
3.  Arrestare il servizio del server di report.  
  
     Se il server di report è in modalità nativa, arrestare il server Windows di report usando la Gestione configurazione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
     Se il server di report è configurato per la modalità SharePoint, arrestare il servizio condiviso di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] nell'Amministrazione centrale SharePoint.  
  
4.  Avviare il servizio del server di report o il servizio SharePoint di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
5.  Verificare che i report possano essere eseguiti nella nuova replica primaria.  
  
###  <a name="report-server-behavior-when-a-failover-occurs"></a><a name="bkmk_failover_behavior"></a> Comportamento del server di report quando si verifica un failover  
 Quando si verifica il failover dei database del server di report e l'ambiente del server di report è stato aggiornato per usare la nuova replica primaria, ci sono alcuni problemi operativi che risultano dal processo di failover e recupero. L'impatto di questi problemi varia in base al carico di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] al momento del failover e al tempo necessario per l'esecuzione del failover di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in una replica secondaria e per l'aggiornamento da parte dell'amministratore del server di report dell'ambiente di gestione dei report per usare la nuova replica primaria.  
  
-   L'esecuzione dell'elaborazione in background potrebbe verificarsi più di una volta a causa della logica di riesecuzione e l'incapacità del server di report di indicare il lavoro programmato come completato durante il periodo di failover.  
  
-   L'elaborazione di background che normalmente sarebbe dovuta essere attivata per l'esecuzione durante il periodo del failover non verrà eseguita poiché SQL Server Agent non sarà in grado di scrivere i dati nel database del server di report e questi dati non saranno sincronizzati per la nuova replica primaria.  
  
-   Al termine del failover del database e dopo aver riavviato il servizio del server di report, i processi di SQL Server Agent verranno ricreati in modo automatico. Fino a che i processi di SQL Agent non vengono ricreati, le esecuzioni di background associate ai processi SQL Server Agent non verranno elaborate. Ad esempio, le sottoscrizioni, le pianificazioni e le istantanee [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
 [Supporto di SQL Server Native Client per il ripristino di emergenza a disponibilità elevata](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Introduzione ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)   
 [Utilizzo delle parole chiave delle stringhe di connessione con SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)   
 [Supporto di SQL Server Native Client per il ripristino di emergenza a disponibilità elevata](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Informazioni sull'accesso alla connessione client per le repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
