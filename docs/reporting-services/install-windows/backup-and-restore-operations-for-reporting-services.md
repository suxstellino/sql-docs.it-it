---
description: Operazioni di backup e ripristino per Reporting Services
title: Operazioni di backup e ripristino per Reporting Services | Microsoft Docs
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 157bc376-ab72-4c99-8bde-7b12db70843a
ms.date: 05/08/2019
ms.openlocfilehash: 038f7bd7db0df60073ae887839eddd057b69cd58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065336"
---
# <a name="backup-and-restore-operations-for-reporting-services"></a>Operazioni di backup e ripristino per Reporting Services

  Questo articolo contiene una panoramica di tutti i file di dati usati in un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e specifica quando e come eseguire il backup dei file. Lo sviluppo di un piano di backup e ripristino per i file dei database del server di report rappresenta la parte più importante di una strategia di recupero. Una strategia di recupero più completa include tuttavia backup delle chiavi di crittografia, di estensioni o assembly personalizzati, dei file di configurazione e dei file di origine dei report.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  Modalità nativa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] | Modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.
  
 Le operazioni di backup e ripristino vengono spesso utilizzate per spostare un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o parte di essa:  
  
-   Se si stanno spostando solo i database del server di report, è possibile utilizzare il backup e il ripristino oppure eseguire il collegamento e lo scollegamento per spostare i database in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diversa. Per altre informazioni, vedere [Spostamento di database del server di report in un altro computer &#40;modalità nativa SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
-   Lo spostamento di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in un nuovo computer viene definito migrazione. In caso di migrazione di un'installazione, eseguire il programma di installazione per installare una nuova istanza del server di report e quindi copiare i dati dell'istanza in un nuovo computer. Per altre informazioni sulla migrazione di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere gli articoli seguenti:  
  
    - [Eseguire l'aggiornamento e la migrazione di Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
    - [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità nativa&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

    ::: moniker range="=sql-server-2016"
  
    - [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)  

    ::: moniker-end
  
## <a name="backing-up-the-report-server-databases"></a>Backup dei database del server di report  
 Poiché un server di report è un server senza stato, tutti i dati delle applicazioni vengono archiviati nei database **reportserver** e **reportservertempdb** in esecuzione su un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . È possibile eseguire il backup dei database **reportserver** e **reportservertempdb** usando uno dei metodi supportati per il backup dei database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di seguito sono elencati alcuni consigli specifici per i database del server di report:  
  
-   Per il backup del database **reportserver** usare il modello di recupero con registrazione completa.  
  
-   Per il backup del database **reportservertempdb** usare il modello di recupero con registrazione minima.  
  
-   Per ogni database, è possibile utilizzare diverse pianificazioni di backup. È consigliabile eseguire il backup del database **reportservertempdb** solo per non doverlo ricreare nel caso in cui si verifichi un errore hardware. Nel caso di un errore hardware, non è necessario recuperare i dati di **reportservertempdb**, ma è necessaria la struttura della tabella. Se il database **reportservertempdb** viene perso, l'unico modo per recuperarlo consiste nel ricreare il database del server di report. Se si ricrea il database **reportservertempdb**, è importante assegnare lo stesso nome del database del server di report principale.  
  
 Per altre informazioni sul backup e il ripristino dei database relazionali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  

::: moniker range="=sql-server-2016"

> [!IMPORTANT]  
>  Se il server di report è in modalità SharePoint, è necessario occuparsi di altri database, tra cui i database di configurazione di SharePoint e il database di avvisi di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. In modalità SharePoint vengono creati tre database per ciascuna applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] : **reportserver**, **reportservertempdb** e **dataalerting** . Per altre informazioni, vedere [Eseguire il backup e il ripristino di applicazioni di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md).  

::: moniker-end
  
## <a name="backing-up-the-encryption-keys"></a>Backup delle chiavi di crittografia  
 Quando si configura un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per la prima volta, è consigliabile eseguire il backup delle chiavi di crittografia. È consigliabile eseguire il backup delle chiavi anche ogni volta che si modifica l'identità degli account di servizio o si rinomina il computer. Per altre informazioni, vedere [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md). 

::: moniker range="=sql-server-2016"

Per i server di report in modalità SharePoint, vedere la sezione "Gestione chiavi" in [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  

::: moniker-end
  
## <a name="backing-up-the-configuration-files"></a>Backup dei file di configurazione  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa i file di configurazione per archiviare le impostazioni dell'applicazione. È consigliabile eseguire il backup dei file quando si configura il server per la prima volta e dopo avere distribuito eventuali estensioni personalizzate. I file di cui eseguire il backup includono:  
  
-   RSReportServer.config  
  
-   Rssvrpolicy.config  
  
-   Rsmgrpolicy.config  
  
-   Reportingservicesservice.exe.config  
  
-   Web.config per l'applicazione [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] del server di report
  
-   Machine.config per [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]  
  
## <a name="backing-up-data-files"></a>Backup dei file di dati  
 Eseguire il backup dei file creati e gestiti in Progettazione report. Questi file includono i file di definizione del report (con estensione rdl), i file origine dati condivisa (con estensione rds), i file vista dati (con estensione dv), i file origine dati (con estensione ds), i file di progetto del server di report (con estensione rptproj) e i file di soluzione di report (con estensione sln).  
  
 Ricordare di eseguire il backup di eventuali file script (con estensione rss) creati per attività di amministrazione o di distribuzione.  
  
 Verificare di avere una copia di backup per tutte le estensioni e gli assembly personalizzati utilizzati.  

## <a name="next-steps"></a>Passaggi successivi

[Database del server di report](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
[File di configurazione di Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
[Utilità rskeymgmt](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)   
[Copiare database tramite backup e ripristino](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
[Amministrare un database del server di report](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)   
[Configurare e gestire le chiavi di crittografia](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
