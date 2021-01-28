---
description: Upgrade and Migrate Reporting Services
title: Eseguire l'aggiornamento e la migrazione di Reporting Services | Microsoft Docs
ms.prod: reporting-services
ms.prod_service: reporting-services-native
helpviewer_keywords:
- SSRS, upgrading
- Reporting Services, upgrades
- SQL Server Reporting Services, upgrading
- upgrading Reporting Services
author: maggiesMSFT
ms.author: maggies
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 2515a93b025392735135cd13cbd734975f8ed221
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596834"
---
# <a name="upgrade-and-migrate-reporting-services"></a>Upgrade and Migrate Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Questo argomento offre una panoramica delle opzioni di aggiornamento e migrazione per SQL Server Reporting Services. Sono disponibili due approcci generali per l'aggiornamento di una distribuzione di SQL Server Reporting Services:  
 
- **Eseguire l'aggiornamento *a* Reporting Services 2016 e versioni precedenti *da* Reporting Services 2016 e versioni precedenti:** vengono aggiornati i componenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nei server e nelle istanze in cui sono attualmente installati. Si tratta dell'aggiornamento comunemente definito "sul posto". L'aggiornamento sul posto non è supportato da una modalità all'altra del server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Non è possibile ad esempio eseguire l'aggiornamento da un server di report in modalità nativa a un server di report in modalità SharePoint. È possibile eseguire la migrazione degli elementi del report da una modalità all'altra. Per altre informazioni, vedere la sezione [Scenari di aggiornamento e migrazione della modalità SharePoint](#bkmk_sharePoint_scenarios) più avanti in questo documento.  

- **L'aggiornamento *a* Reporting Services 2017 e versioni successive *da* Reporting Services 2016 e versioni precedenti** non è lo stesso scenario di aggiornamento delle versioni precedenti. Quando si esegue l'aggiornamento *a* Reporting Services 2016 e versioni precedenti, è possibile seguire un processo di aggiornamento sul posto usando il supporto di installazione di SQL Server. Quando si esegue l'aggiornamento *a* Reporting Services 2017 e versioni successive *da* Reporting Services 2016 e versioni precedenti, non è possibile seguire la stessa procedura perché la nuova installazione di Reporting Services è un prodotto autonomo. Non fa più parte del supporto di installazione di SQL Server. 

    Per eseguire l'aggiornamento da Reporting Services 2016 e versioni precedenti a Reporting Services 2017 e versioni successive, seguire l'articolo [Eseguire la migrazione di un'installazione di Reporting Services (modalità nativa)](migrate-a-reporting-services-installation-native-mode.md) con Reporting Services 2017 o versione successiva come istanza di destinazione. 

- **L'aggiornamento *da* Reporting Services 2017 alle versioni future** è di nuovo uno scenario di aggiornamento sul posto, perché i GUID di installazione del prodotto sono gli stessi. Eseguire il file di installazione SQLServerReportingServices.exe per avviare l'aggiornamento sul posto nel server in cui è attualmente installato Reporting Services.
  
- **Migrazione**: viene installato e configurato un nuovo ambiente SharePoint, vengono copiate risorse ed elementi di report nel nuovo ambiente che viene configurato in modo da usare il contesto esistente. Un tipo di migrazione di livello inferiore consiste nel copiare i database di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , i file di configurazione e, se si utilizza la modalità SharePoint, i database di contenuto di SharePoint.  


> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è disponibile nelle versioni successive a SQL Server 2016.
   
##  <a name="known-upgrade-issues-and-best-practices"></a><a name="bkmk_known_issues"></a> Problemi di aggiornamento noti e procedure consigliate  
 Per un elenco dettagliato delle edizioni e delle versioni supportate che è possibile aggiornare, vedere [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
> [!TIP]  
>  Per le informazioni più recenti relative a problemi riguardanti SQL Server, vedere [Note sulla versione di SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md).  
  
  
##  <a name="side-by-side-installations"></a><a name="bkmk_side_by_side"></a> Installazioni side-by-side  
 È possibile eseguire l'installazione side-by-side della modalità nativa di SQL Server Reporting Services con una distribuzione in modalità nativa di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Le distribuzioni side-by-side di SQL Server Reporting Services in modalità SharePoint e di qualsiasi versione precedente dei componenti della modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non sono supportate.  
  
  
##  <a name="in-place-upgrade"></a><a name="bkmk_inplace_upgrade"></a> Aggiornamento sul posto  
 L'aggiornamento viene completato tramite il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per aggiornare uno o tutti i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , incluso [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Durante l'installazione vengono rilevate le istanze esistenti e viene richiesto di eseguire l'aggiornamento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il programma di installazione offre diverse opzioni di aggiornamento che è possibile specificare come argomento della riga di comando o nell'Installazione guidata.  
  
 Quando si esegue il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile selezionare l'opzione che consente di eseguire l'aggiornamento da una delle versioni seguenti oppure installare una nuova istanza di SQL Server Reporting Services che esegue le installazioni side-by-side esistenti:  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 Per ulteriori informazioni su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere quanto segue:  

* [Eseguire l'aggiornamento a SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)
* [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [Installazione di SQL Server 2016 dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)
  
  
##  <a name="pre-upgrade-checklist"></a><a name="bkmk_upgrade_checklist"></a> Elenco di controllo preliminare all'aggiornamento  
 Prima dell'aggiornamento a SQL Server Reporting Services, verificare quanto segue:  
  
-   Rivedere i requisiti per determinare se l'hardware e il software in uso supportano [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]. Per altre informazioni, vedere [Requisiti hardware e software per l'installazione di SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Usare Controllo configurazione sistema (SCC) per analizzare il computer del server di report per individuare eventuali condizioni che potrebbero compromettere l'installazione di SQL Server Reporting Services. Per altre informazioni, vedere [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   Rivedere le procedure consigliate per la sicurezza e le indicazioni di guida per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   Eseguire il backup della chiave simmetrica. Per altre informazioni, vedere [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
-   Eseguire il backup dei database e dei file di configurazione del server di report. Per altre informazioni, vedere [Backup and Restore Operations for Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md).  
  
-   Eseguire il backup di eventuali personalizzazioni nelle directory virtuali di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] esistenti in IIS.  
  
-   Rimuovere i certificati TLS/SSL non validi.  In questa operazione sono inclusi i certificati scaduti e quelli che non si intende aggiornare prima dell'aggiornamento di Reporting Services.  Se i certificati non sono validi, l'aggiornamento non sarà eseguito correttamente e nel file di log di Reporting Services verrà visualizzato un messaggio di errore simile al seguente: **Microsoft.ReportingServices.WmiProvider.WMIProviderException: Nel sito Web non è configurato un certificato SSL (Secure Sockets Layer).**  
  
 Prima di aggiornare un ambiente di produzione, eseguire sempre un aggiornamento di prova in un ambiente di pre-produzione che abbia la stessa configurazione dell'ambiente di produzione.  
  
  
## <a name="overview-of-migration-scenarios"></a>Panoramica degli scenari di migrazione  
 Se si esegue l'aggiornamento da una versione supportata di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è in genere possibile eseguire l'Installazione guidata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per aggiornare i file di programma del server di report, il database e tutti i dati dell'applicazione.  
  
 È tuttavia necessario eseguire manualmente la **migrazione** di un'installazione del server di report se si verifica una delle condizioni seguenti:  
  
-   Si desidera modificare il tipo di server di report usato nella distribuzione. Non è possibile, ad esempio, eseguire l'aggiornamento o la conversione da un server di report in modalità nativa a un server di report in modalità SharePoint. Per altre informazioni, vedere [Migrazione dalla modalità nativa alla modalità SharePoint &#40;SSRS&#41;](../../reporting-services/install-windows/native-to-sharepoint-migration-ssrs.md).  
  
-   Si desidera ridurre la quantità di tempo in cui il server di report viene portato offline durante il processo di aggiornamento. L'installazione corrente rimane online durante la copia dei dati di contenuto in una nuova istanza del server di report e il test dell'installazione senza la modifica dello stato di installazione del server di report esistente.  
  
-   Si desidera eseguire la migrazione di una distribuzione di SharePoint 2010 di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a SharePoint 2013/2016. SharePoint 2013/2016 non supporta l'aggiornamento sul posto da SharePoint 2010. Per altre informazioni, vedere [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
  
##  <a name="native-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_native_scenarios"></a> Scenari di aggiornamento e migrazione della modalità nativa  
 **Aggiornamento:** l'aggiornamento sul posto per la modalità nativa prevede lo stesso processo per ognuna delle versioni supportate elencate in precedenza in questo argomento. Eseguire l'Installazione guidata di SQL Server o l'installazione da riga di comando. Al termine dell'installazione il database del server di report verrà aggiornato automaticamente al nuovo schema del database del server di report. Per altre informazioni, vedere la sezione [Aggiornamento sul posto](#bkmk_inplace_upgrade) in questo argomento.  
  
 Il processo di aggiornamento viene avviato quando si seleziona un'istanza del server di report esistente da aggiornare.  
  
1.  Se il database del server di report è installato in un computer remoto e non si dispone dell'autorizzazione necessaria per aggiornarlo, verrà richiesto di specificare le credenziali per l'aggiornamento a un database del server di report remoto. Assicurarsi di fornire credenziali che dispongono di autorizzazioni **sysadmin** o per l'aggiornamento del database.  
  
2.  Il programma di installazione verifica la presenza di eventuali condizioni o impostazioni che impediscono l'aggiornamento e legge le impostazioni di configurazione. Alcuni esempi sono le estensioni personalizzate distribuite nel server di report. Se l'aggiornamento viene bloccato, è necessario modificare l'installazione per evitare il blocco oppure eseguire la migrazione a una nuova istanza di SQL Server Reporting Services. Per altre informazioni, vedere la documentazione di Preparazione aggiornamento.  
  
3.  Se è possibile procedere, viene richiesto di continuare con il processo di aggiornamento.  
  
4.  Il programma di installazione crea nuove cartelle per i file di programma di SQL Server Reporting Services. Tra le cartelle di programma per un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è inclusa la cartella MSRS13.\<*instance name*>.  
  
5.  Vengono anche aggiunti i file di programma del server di report, gli strumenti di configurazione e le utilità della riga di comando di SQL Server Reporting Services che fanno parte della funzionalità del server di report.  
  
    1.  I file di programma della versione precedente vengono rimossi.  
  
    2.  Tra gli strumenti e le utilità di configurazione del server di report aggiornati alla nuova versione sono inclusi lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità nativa, le utilità della riga di comando, ad esempio RS.exe, e Generatore report.  
  
    3.  Altri strumenti client, ad esempio [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , sono un download separati e devono essere aggiornati separatamente. Per altre informazioni, vedere [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
  
    4.  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] è un download separato. Per altre informazioni, vedere [SQL Server Data Tools in Visual Studio 2015](/previous-versions/mt186501(v=msdn.10)).  
  
6.  Il programma di installazione riutilizza la voce di servizio in Gestione controllo servizi per il servizio del server di report di SQL Server Reporting Services. Tale voce di servizio include l'account del servizio Windows del server di report.  
  
7.  Nuovi URL vengono riservati in base alle impostazioni delle directory virtuali esistenti in IIS. Poiché le directory virtuali di IIS potrebbero non essere rimosse dal programma di installazione, assicurarsi di rimuoverle manualmente al termine dell'aggiornamento.  
  
8.  Il programma di installazione unisce le impostazioni nei file di configurazione. Usando come base i file di configurazione dell'installazione corrente, vengono aggiunte nuove voci. Le voci obsolete non vengono rimosse, ma non verranno più lette dal server di report al termine dell'aggiornamento. L'aggiornamento non comporta l'eliminazione dei file di log precedenti, del file RSWebApplication.config obsoleto o delle impostazioni delle directory virtuali in IIS. L'aggiornamento non comporta la rimozione di versioni precedenti di Progettazione report, Management Studio o altri strumenti client. Se non sono più necessari, accertarsi di rimuovere i file e gli strumenti al termine dell'aggiornamento.  
  
 **Migrazione:** la migrazione di una versione precedente di un'installazione in modalità nativa a SQL Server Reporting Services prevede gli stessi passaggi per tutte le versioni supportate elencate in precedenza in questo argomento. Per altre informazioni, vedere [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità nativa&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
  
##  <a name="upgrade-a-reporting-services-native-mode-scale-out-deployment"></a><a name="bkmk_native_scaleout"></a> Aggiornare una distribuzione con scalabilità orizzontale in modalità nativa di Reporting Services  
 Di seguito viene illustrato come aggiornare una distribuzione in modalità nativa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con scalabilità orizzontale a più di un server di report. Questo processo implica tempi di inattività della distribuzione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
1.  Eseguire il backup dei database e delle chiavi di crittografia del server di report. Per altre informazioni, vedere [Operazioni di backup e ripristino per Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md) e [Aggiungere e rimuovere le chiavi di crittografia per una distribuzione con scalabilità orizzontale &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md).  
  
2.  Utilizzare Gestione configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e rimuovere tutti i server di report dalla distribuzione con scalabilità orizzontale. Per altre informazioni, vedere [Configurare una distribuzione con scalabilità orizzontale di un server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
3.  Aggiornare uno dei server di report a SQL Server Reporting Services.  
  
4.  Utilizzare Gestione configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per aggiungere di nuovo i server di report nella distribuzione con scalabilità orizzontale. Per altre informazioni, vedere [Configurare una distribuzione con scalabilità orizzontale di un server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
     Per ogni server, ripetere i passaggi di aggiornamento e scalabilità orizzontale.  
  
##  <a name="sharepoint-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_sharePoint_scenarios"></a> Scenari di aggiornamento e migrazione della modalità SharePoint  
 Nelle sezioni seguenti vengono descritti i problemi e i passaggi di base necessari per eseguire l'aggiornamento o la migrazione da versioni specificate della modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] alla modalità SharePoint di SQL Server Reporting Services [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Esistono due componenti di installazione per aggiornare una distribuzione della modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
    > [!TIP]  
    >  Utilizzare il cmdlet di SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] di `Get-SPRSServiceApplicationServers` per determinare i server nella farm SharePoint che attualmente eseguono il servizio SharePoint Shared di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e, pertanto, richiedono un aggiornamento.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Componente aggiuntivo per prodotti SharePoint. Per altre informazioni, vedere [Installare o disinstallare il componente aggiuntivo Reporting Services per SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  
  
 Per informazioni dettagliate sulla migrazione di un'installazione in modalità SharePoint, vedere [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
> [!IMPORTANT]  
>  Alcuni degli scenari seguenti richiedono l'inattività dell'ambiente SharePoint a causa delle diverse tecnologie che devono essere aggiornate. Se non è possibile rendere inattivo l'ambiente, sarà necessario completare una migrazione anziché un aggiornamento sul posto.  
  
### <a name="sssql14-to-sql-server-reporting-services"></a>Da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a SQL Server Reporting Services  
 **Ambiente iniziale:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP1, SharePoint 2010 o SharePoint 2013.  
  
 **Ambiente finale:** SQL Server Reporting Services, SharePoint 2013 o SharePoint 2016.   
  
-   **SharePoint 2013/2016:** SharePoint 2013/2016 non supporta l'aggiornamento sul posto da SharePoint 2010. Tuttavia, la procedura di **aggiornamento del collegamento di un database**  è supportata.
  
     Se si dispone di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] integrata con SharePoint 2010, non è possibile eseguire l'aggiornamento sul posto del server SharePoint. È tuttavia possibile eseguire la migrazione dei database del contenuto e di quelli dell'applicazione di servizio dalla farm di SharePoint 2010 a una di SharePoint 2013/2016.  
  
### <a name="sssql11-to-sql-server-reporting-services"></a>Da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a SQL Server Reporting Services  
 **Ambiente iniziale:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)], SharePoint 2010.  
  
 **Ambiente finale:** SQL Server Reporting Services, SharePoint 2013 o SharePoint 2016.   
  
-   **SharePoint 2013/2016:** SharePoint 2013/2016 non supporta l'aggiornamento sul posto da SharePoint 2010. Tuttavia, la procedura di **aggiornamento del collegamento di un database**  è supportata.
  
     Se si dispone di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] integrata con SharePoint 2010, non è possibile eseguire l'aggiornamento sul posto del server SharePoint. È tuttavia possibile eseguire la migrazione dei database del contenuto e di quelli dell'applicazione di servizio dalla farm di SharePoint 2010 a una di SharePoint 2013/2016.  
  
### <a name="sskilimanjaro-to-sql-server-reporting-services"></a>Da [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] a SQL Server Reporting Services  
 **Ambiente iniziale:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], SharePoint 2010.  
  
 **Ambiente finale:** SQL Server Reporting Services, SharePoint 2013 o SharePoint 2016.  
 
-   **SharePoint 2013/2016:** SharePoint 2013/2016 non supporta l'aggiornamento sul posto da SharePoint 2010. Tuttavia, la procedura di **aggiornamento del collegamento di un database**  è supportata.

    Prima di aggiornare Reporting Services, è necessario eseguire la migrazione di SharePoint.
  
-   Installare la versione SQL Server Reporting Services del componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per SharePoint in ogni front-end Web della farm. È possibile installare il componente aggiuntivo mediante l'Installazione guidata di SQL Server Reporting Services oppure è possibile scaricarlo.  
  
-   Eseguire l'installazione di SQL Server Reporting Services per aggiornare la modalità SharePoint per ogni "server di report". L'installazione guidata di SQL Server installerà il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e creerà una nuova applicazione di servizio. 
  
  
##  <a name="considerations-for-a-migration"></a><a name="bkmk_migration_considerations"></a> Considerazioni sulla migrazione  
 Quando si spostano dati dell'applicazione, è necessario tenere presente i problemi e le restrizioni seguenti:  
  
-   La sicurezza della chiave di crittografia include un valore hash in cui è incorporata l'identità del computer.  
  
-   I nomi del database del server di report sono fissi e non possono essere modificati nel nuovo computer.  
  
### <a name="encryption-key-considerations"></a>Considerazioni sulla chiave di crittografia  
 Eseguire sempre il backup delle chiavi di crittografia prima di spostare un database del server di report in un nuovo computer.  
  
 In seguito allo spostamento di un'installazione del server di report in un altro computer, il valore hash che protegge le chiavi di crittografia usate per proteggere dati sensibili archiviati nel database del server di report non sarà più valido. Ogni istanza del server di report che usa il database dispone della propria copia della chiave di crittografia, crittografata con l'identità dell'account del servizio come definito nel computer corrente. Se si cambia computer, il servizio non sarà più in grado di accedere alla propria chiave, anche se si usa lo stesso nome dell'account nel nuovo computer.  
  
 Per ristabilire la crittografia reversibile nel nuovo computer del server di report, è necessario ripristinare la chiave di cui in precedenza è stato eseguito il backup. Il set di chiavi completo archiviato nel database del server di report è costituito da un valore della chiave simmetrica più le informazioni sull'identità del servizio usate per limitare l'accesso alla chiave in modo che possa essere usata solo dall'istanza del server di report in cui è archiviata. Durante il ripristino della chiave, nel server di report le copie esistenti della chiave vengono sostituite con nuove versioni che includono i valori relativi all'identità del computer e del servizio definiti nel computer corrente. Per altre informazioni, vedere gli argomenti seguenti:  
  
-   Modalità SharePoint: vedere la sezione "Gestione chiavi" di [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
-   Modalità nativa: vedere [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)  
  
  
### <a name="fixed-database-name"></a>Nome fisso del database  
 Non è possibile rinominare il database del server di report poiché l'identità del database viene registrata nelle stored procedure del server di report al momento della creazione del database stesso. La ridenominazione dei database primari o temporanei del server di report provocherà errori durante l'esecuzione delle procedure, rendendo non valida l'installazione del server di report.  
  
 Se il nome del database dell'installazione esistente non è appropriato per la nuova installazione, è consigliabile creare un nuovo database con il nome desiderato, quindi caricare i dati dell'applicazione esistenti usando le tecniche seguenti:  
  
-   Scrivere uno script [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] che chiama metodi SOAP del servizio Web ReportServer per copiare dati tra database. Lo script può essere eseguito mediante l'utilità RS.exe. Per altre informazioni su questo approccio, vedere [Script e PowerShell con Reporting Services](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md).  
  
-   Scrivere codice che chiama il provider WMI per copiare dati tra database. Per altre informazioni su questo approccio, vedere [Accedere al provider WMI per Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
-   Se il numero di elementi non è elevato, è possibile ripubblicare report e origini dati condivise da Progettazione report, Progettazione modelli e Generatore report nel nuovo server di report. È necessario ricreare assegnazioni di ruolo, sottoscrizioni, pianificazioni condivise, pianificazioni dello snapshot del report, proprietà personalizzate impostate nei report o in altri elementi, sicurezza degli elementi dei modelli e proprietà impostate nel server di report. La cronologia del report e i dati del log di esecuzione dei report saranno persi.  
  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> Risorse aggiuntive  
  
> [!NOTE]  
>  Per altre informazioni sull'aggiornamento del collegamento di un database di SharePoint, vedere gli argomenti seguenti:  
  
-   [Panoramica del processo di aggiornamento a SharePoint 2016](https://technet.microsoft.com/library/cc262483\(v=office.16\)).

-   [Panoramica del processo di aggiornamento a SharePoint 2013](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Pulire le preparazioni prima di un aggiornamento a SharePoint 2013](/SharePoint/upgrade-and-update/clean-up-an-environment-before-an-upgrade-to-sharepoint-2013).  
  
-   [Aggiornare i database da SharePoint 2013 a SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\)).

-   [Aggiornare i database da SharePoint 2010 a SharePoint 2013](/SharePoint/upgrade-and-update/upgrade-content-databases-from-sharepoint-2010-to-sharepoint-2013).  

## <a name="next-steps"></a>Passaggi successivi

[Aggiornare i report](../../reporting-services/install-windows/upgrade-reports.md)   
[Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)