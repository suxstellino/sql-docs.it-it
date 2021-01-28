---
description: Migrazione di un'installazione di Reporting Services in modalità SharePoint
title: Eseguire la migrazione di un'installazione di Reporting Services (modalità SharePoint) | Microsoft Docs
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 61290949-690a-4e19-b078-57c99b6b30fa
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: 9281c38d56b85cda96b9a5f9888ff707bc4e139a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596215"
---
# <a name="migrate-a-reporting-services-installation-sharepoint-mode"></a>Migrazione di un'installazione di Reporting Services in modalità SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE [ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  In questo argomento viene fornita una panoramica dei passaggi necessari per eseguire la migrazione di una distribuzione in modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da un ambiente SharePoint a un altro. I passaggi specifici possono variare a seconda della versione dalla quale si esegue la migrazione. Per altre informazioni sugli scenari di aggiornamento e di migrazione per la modalità SharePoint, vedere [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md). Se si vuole solo copiare gli elementi del report da un server a un altro, vedere [Script di esempio rs.exe di Reporting Services per la copia di contenuto tra server di report](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
 Per informazioni sulla migrazione di una distribuzione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità nativa, vedere [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità nativa&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
 Un motivo comune per effettuare una migrazione è costituito dall'aggiornamento di una distribuzione di SharePoint 2010 a SharePoint 2013/2016. SharePoint 2013/2016 non supporta l'aggiornamento sul posto di SharePoint 2010 ed è necessario completare la procedura di **aggiornamento del collegamento di un database** o una migrazione del solo contenuto.  
  
 Per altre informazioni sull'aggiornamento a SharePoint 2013/2016, vedere gli argomenti seguenti:  

-   [Panoramica del processo di aggiornamento a SharePoint 2016](https://technet.microsoft.com/library/cc262483\(v=office.16\)).

-   [Panoramica del processo di aggiornamento a SharePoint 2013](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Pulire le preparazioni prima di un aggiornamento a SharePoint 2013](/SharePoint/upgrade-and-update/clean-up-an-environment-before-an-upgrade-to-sharepoint-2013).  
  
-   [Aggiornare i database da SharePoint 2013 a SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\)).

-   [Aggiornare i database da SharePoint 2010 a SharePoint 2013](/SharePoint/upgrade-and-update/upgrade-content-databases-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Spostare i database del contenuto in SharePoint 2016](https://technet.microsoft.com/library/cc262792\(v=office.16\).aspx).

-   [Spostare i database del contenuto in SharePoint 2013](/SharePoint/administration/move-content-databases).
  
##  <a name="migrate-from-reporting-services-sharepoint-mode-versions-prior-to-sql-server-2012"></a><a name="bkmk_prior_versions"></a> Migrazione da versioni della modalità SharePoint di Reporting Services precedenti a SQL Server 2012  
 L'architettura della modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] modificata in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], incluso lo schema del database dell'applicazione di servizio. Per eseguire la migrazione alla modalità SharePoint di [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] da una versione precedente a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], creare innanzitutto il nuovo ambiente SharePoint installando SharePoint e la modalità SharePoint di SQL Server 2016 Reporting Services. Per altre informazioni, vedere [Installare la modalità SharePoint di Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
 Quando il nuovo ambiente SharePoint è in esecuzione, è possibile scegliere tra una migrazione solo del contenuto o una migrazione completa a livello di database che include i database del contenuto.  
  
###  <a name="content-only-migration"></a><a name="bkmk_content_only_migration"></a> Migrazione solo del contenuto  
 **Migrazione solo del contenuto di Reporting Services:** se è necessario copiare il contenuto di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in una nuova farm, sarà necessario utilizzare strumenti come **rs.exe** per copiare il contenuto nella nuova installazione di SharePoint. Per altre informazioni sulle migrazioni solo del contenuto, vedere quanto riportato di seguito:  
  
-   **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Script RSS:** gli script consentono di eseguire la migrazione del contenuto e delle risorse tra i server di report in modalità SharePoint e in modalità nativa. Per altre informazioni, vedere [Script di esempio rs.exe di Reporting Services per la copia di contenuto tra server di report](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md) e [Script di esempio rs.exe di Reporting Services per la migrazione del contenuto tra server di report](https://azuresql.codeplex.com/releases/view/115207).  
  
-   **Strumento di migrazione di Reporting Services:** lo strumento consente di copiare gli elementi del report da un server in modalità nativa a un server in modalità SharePoint. Per altre informazioni, vedere la pagina relativa allo [strumento di migrazione di Reporting Services](https://www.microsoft.com/download/details.aspx?id=29560) (https://www.microsoft.com/download/details.aspx?id=29560).  
  
###  <a name="full-migration"></a><a name="bkmk_full_migration"></a> Migrazione completa  
 **Migrazione completa:** se si desidera eseguire la migrazione dei database del contenuto di SharePoint insieme ai database del catalogo di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a una nuova farm, è possibile ricorrere a una serie di opzioni di backup e ripristino riepilogate nel presente argomento. In alcuni casi per la fase di ripristino sarà necessario usare uno strumento diverso da quello usato per la fase di backup. È ad esempio possibile usare Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per eseguire il backup delle chiavi di crittografia da una versione precedente di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ma è necessario usare Amministrazione centrale SharePoint o PowerShell per ripristinare le chiavi di crittografia in un'installazione della modalità SharePoint di SQL Server 2016 Reporting Services.  
  
####  <a name="databases-you-will-see-in-the-completed-migration"></a><a name="bkmk_databases"></a> Database che saranno visibili nella migrazione completata  
 Nella tabella seguente vengono descritti i database di SQL Server correlati a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che si avranno a disposizione dopo avere eseguito correttamente la migrazione dell'installazione di SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
|Database|Nome di esempio|Note|  
|--------------|------------------|-|  
|Database del catalogo|ReportingService_[GUID applicazione del servizio] **(&#42;)**|L'utente esegue la migrazione.|  
|Database temporaneo|ReportingService_[GUID applicazione del servizio]TempDB **(&#42;)**|L'utente esegue la migrazione.|  
|Database di avviso|ReportingService_[GUID applicazione del servizio]_Alerting|Viene creato al momento della creazione di un'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|  
  
 **(&#42;)** I nomi di esempio indicati nella tabella seguono la convenzione di denominazione usata in SSRS quando si crea una nuova applicazione del servizio SSRS. Se si esegue la migrazione da un server diverso, i tempDB e il catalogo manterranno i nomi dall'installazione originale.  
  
####  <a name="backup-operations"></a><a name="bkmk_backup_operations"></a> Operazioni di backup  
 In questa sezione vengono descritti i tipi di informazioni necessari per eseguire la migrazione e gli strumenti o il processo da usare per completare il backup.  
  
 ![Diagramma di base di una migrazione SharePoint per SSRS](../../reporting-services/install-windows/media/rs-sharepoint-migration.gif "Diagramma di base di una migrazione SharePoint per SSRS")  
  
|Elemento|Oggetti|Metodo|Note|  
|-|-------------|------------|-----------|  
|**1**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|**Rskeymgmt.exe** o Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vedere [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).|Gli strumenti indicati possono essere utilizzati per il backup ma per l'operazione di ripristino saranno necessarie le pagine di gestione dell'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] oppure PowerShell.|  
|**2**|Database del contenuto di SharePoint.||Creare il database e scollegarlo.<br /><br /> Vedere la sezione "aggiornamento di connessione al database" in [determinare l'approccio di aggiornamento (SharePoint Server 2010) (/SharePoint/Upgrade-and-Update/Determine-Strategy-for-upgrade-to-SharePoint-2013)](/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013).|  
|**3**|Database di SQL Server corrispondente al database del catalogo di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Backup e ripristino del database di SQL Server<br /><br /> oppure<br /><br /> Scollegamento e collegamento di un database di SQL Server.||  
|**4**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Semplice copia di file.|È sufficiente copiare rsreportserver.config se sono state apportate personalizzazioni a tale file. Esempio di percorso predefinito dei file: C:\Programmi\File comuni\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting\\*:<br /><br /> <br /><br /> RSReportServer.config<br /><br /> Rssvrpolicy.config<br /><br /> Web.config per l'applicazione ASP.NET del server di report.<br /><br /> Machine.config per ASP.NET.|  
  
####  <a name="restore-operations"></a><a name="bkmk_restore_operations"></a> Operazioni di ripristino  
 Questa sezione descrive i tipi di informazioni necessari per eseguire la migrazione e gli strumenti o processi da usare per completare il ripristino. È possibile che gli strumenti da usare per il ripristino siano diversi da quelli che si sono usati per il backup.  
  
 Prima di completare i passaggi del ripristino, è necessario installare e configurare la nuova farm SharePoint e la modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni sull'installazione di base della modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vedere [Installare la modalità SharePoint di Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
|Elemento|Oggetti|Metodo|Note|  
|-|-------------|------------|-----------|  
|**1**|Ripristinare il database del contenuto di SharePoint nella nuova farm.|Metodo SharePoint "Aggiornamento basato sul collegamento di database".|Passaggi principali:<br /><br /> 1) Ripristinare il database nel nuovo server.<br /><br /> 2) Collegare il database del contenuto a un'applicazione Web indicando l'URL.<br /><br /> 3) Get-SPWebapplication elenca tutte le applicazioni Web e tutti gli URL.<br /><br /> <br /><br /> Vedere la sezione "aggiornamento di connessione al database" in [determinare l'approccio di aggiornamento (SharePoint Server 2010) (/SharePoint/Upgrade-and-Update/Determine-Strategy-for-upgrade-to-SharePoint-2013)](/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013)e [alleghi database e aggiornamento a SharePoint Server 2010 (/SharePoint/Upgrade-and-Update/Upgrade-content-databases)](/SharePoint/upgrade-and-update/upgrade-content-databases).|  
|**2**|Ripristinare il database SQL Server che rappresenta il database del catalogo di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (ReportServer).|Backup e ripristino del database SQL.<br /><br /> **or**<br /><br /> Collegamento e scollegamento di un database di SQL Server.|Quando il database viene usato per la prima volta, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] aggiorna lo schema del database affinché funzioni correttamente con l'ambiente SQL Server 2016.|  
|**3**|Creare una nuova applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Creare una nuova applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Quando si crea la nuova applicazione di servizio, configurarla per usare il database del server di report che si è copiato.<br /><br /> Per altre informazioni sull'uso di Amministrazione centrale SharePoint, vedere la sezione "Passaggio 3: Creare un'applicazione di servizio Reporting Services" in [Installare la modalità SharePoint di Reporting Services per SharePoint 2013](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md).<br /><br /> Per alcuni esempi sull'uso di PowerShell, vedere la sezione "Per creare un'applicazione di servizio Reporting Services usando PowerShell" in [Reporting Services SharePoint Service and Service Applications](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).|  
|**4**|Ripristinare i file di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Semplice copia di file.|Esempio di percorso predefinito dei file: C:\Programmi\File comuni\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting.|  
|**5**|Ripristinare le chiavi di crittografia di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Ripristinare il file di backup delle chiavi usando la pagina "SystemSettings" dell'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].<br /><br /> **or**<br /><br /> PowerShell.|Vedere la sezione "Gestione chiavi" di [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).|   
  
##  <a name="migrate-from-a-sql-server-2012-or-sql-server-2014-deployment"></a><a name="bkmk_migrate_from_ctp"></a> Eseguire la migrazione da una distribuzione di SQL Server 2012 o SQL Server 2014  
 In una farm multiserver, molto probabilmente gli utenti avranno i database del contenuto e del catalogo su computer diversi, nel qual caso sarà sufficiente aggiungere alla farm di SharePoint un nuovo server con il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installato, quindi rimuovere il server obsoleto. Non è necessario copiare alcun database.  
  
### <a name="backup-operations"></a>Operazioni di backup  
  
1.  Eseguire il backup delle chiavi di crittografia di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
2.  Eseguire il backup dell'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] tramite Amministrazione centrale SharePoint oppure utilizzare PowerShell. Ciò determinerà anche l'esecuzione del backup dei database delle applicazioni di servizio in SharePoint. Vedere l'argomento  [Eseguire il backup e il ripristino di applicazioni di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md)  
  
3.  Se si ha un account di esecuzione automatica (UEA) e l'autenticazione di Windows, annotare le credenziali in modo da poterle usare per il processo di ripristino.  
  
4.  Per altre informazioni, vedere [Eseguire il backup delle applicazioni di servizio in SharePoint 2013](/SharePoint/administration/back-up-a-service-application).  
  
### <a name="restore-operations"></a>Operazioni di ripristino  
  
1.  Ripristinare l'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usando Amministrazione centrale SharePoint. È anche possibile usare PowerShell.  
  
2.  Ripristinare le chiavi di crittografia di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
     Vedere la sezione "Gestione chiavi" di [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
3.  Configurare gli account di esecuzione automatica (UEA) e credenziali di Windows sull'applicazione di servizio.  
  
4.  Per altre informazioni, vedere [Ripristinare le applicazioni di servizio in SharePoint 2013](/SharePoint/administration/restore-a-service-application).  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> Risorse aggiuntive  
  
-   [Introduzione agli aggiornamenti a SharePoint 2013 (/SharePoint/Upgrade-and-Update/Get-Started-with-upgrade)](/SharePoint/upgrade-and-update/get-started-with-upgrade).  
  
-   [Panoramica del processo di aggiornamento a SharePoint 2013 (/SharePoint/Upgrade-and-Update/Overview-of-the-upgrade-process)](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process).  

## <a name="next-steps"></a>Passaggi successivi

[Eseguire l'aggiornamento e la migrazione di Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Eseguire la migrazione di un'installazione di Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)