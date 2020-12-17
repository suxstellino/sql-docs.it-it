---
description: Risolvere i problemi di installazione di Reporting Services
title: Risolvere i problemi di installazione di Reporting Services | Microsoft Docs
ms.date: 01/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: e2536f7f-d90c-4571-9ffd-6bbfe69018d6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1cc420302bb8d1610adcc1848fda226c4c55b492
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472472"
---
# <a name="troubleshoot-a-reporting-services-installation"></a>Risolvere i problemi di installazione di Reporting Services

  Se non è possibile installare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a causa di errori restituiti durante l'installazione, utilizzare le indicazioni fornite in questo articolo per risolvere le condizioni che costituiscono la causa più probabile degli errori di installazione.  
  
 Per informazioni su altri errori e problemi riguardanti [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere la pagina relativa a [errori e risoluzione dei problemi di SSRS](https://social.technet.microsoft.com/wiki/contents/articles/ssrs-troubleshooting-issues-and-errors.aspx).  
  
 Controllare le [note sulla versione online](https://go.microsoft.com/fwlink/?linkid=236893) per verificare se vi sia descritto il problema rilevato.  
  
##  <a name="check-setup-logs"></a><a name="bkmk_setuplogs"></a> Verificare i log di installazione  
 Gli errori di installazione vengono registrati in file di log nella cartella **[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Setup Bootstrap\Log** . Ogni volta che si esegue il programma di installazione viene creata una sottocartella. Il nome della sottocartella è costituito dalla data e dall'ora di esecuzione del programma di installazione. Per istruzioni su come visualizzare i file di log del programma di installazione, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
-   I file di log includono una raccolta di file.  
  
-   Aprire il file *_summary.txt per visualizzare informazioni su prodotto, componente e istanza.  
  
-   Aprire il file *_errorlog.txt per visualizzare informazioni sull'errore generate durante l'installazione.  
  
-   Aprire il file *_RS\_\*_ComponentUpdateSetup.log per visualizzare informazioni sull'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
##  <a name="check-prerequisites"></a><a name="bkmk_prereq"></a> Verificare i prerequisiti  
 I prerequisiti vengono verificati automaticamente dal programma di installazione. In caso di problemi relativi all'installazione, tuttavia, può essere utile conoscere i requisiti verificati dal programma di installazione.  
  
-   Tra i requisiti relativi agli account per l'esecuzione del programma di installazione è inclusa l'appartenenza al gruppo Administrators locale. Il programma di installazione deve disporre delle autorizzazioni necessarie per aggiungere file e impostazioni del Registro di sistema, creare gruppi di sicurezza locali e impostare autorizzazioni. Se si installa una configurazione predefinita, il programma di installazione deve disporre dell'autorizzazione necessaria per creare un database del server di report sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si esegue l'installazione.  
  
-   Il sistema operativo deve supportare HTTP.SYS 1.1.  
  
-   Il servizio HTTP deve essere abilitato e in esecuzione.  
  
-   Distributed Transaction Coordinator (DTC) deve essere in esecuzione se si intende installare anche il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
-   Nella cartella System32 deve essere presente il file Authz.dll.  
  
 Il programma di impostazione non verifica più la presenza di Internet Information Services (IIS) o [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] richiede MDAC 2.0 e [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] versione 2.0, che verranno installati se non sono già presenti.  

::: moniker range="=sql-server-2016"
  
##  <a name="troubleshoot-problems-with-sharepoint-mode-installations"></a><a name="bkmk_tshoot_sharepoint"></a> Risoluzione dei problemi relativi alle installazioni della modalità SharePoint  
  
-   [Gestione configurazione del server di report non si avvia](#bkmk_configmanager_notstart)  
  
-   [Il servizio SQL Server Reporting Services in Amministrazione centrale SharePoint non viene visualizzato dopo l'installazione di SQL Server 2016 SSRS in modalità SharePoint](#bkmk_no_ssrs_service)  
  
-   [I cmdlet di PowerShell per Reporting Services non sono disponibili e i comandi non sono riconosciuti](#bkmk_cmdlets_not_recognized)  
  
-   [Viene visualizzato un messaggio di errore nel quale è indicato che l'URL non è configurato](#bkmk_URL_not_configured)  
  
-   [La configurazione non riesce in un computer con SharePoint installato ma non configurato](#bkmk_sharepoint_not_confiugred)  
  
-   [La pagina Amministrazione centrale SharePoint è vuota](#bkmk_central_admin_blank)  
  
-   [Viene visualizzato un messaggio di errore quando si tenta di creare un nuovo report di Generatore report](#bkmk_reportbuilder_newreport_error)  
  
-   [Viene visualizzato un messaggio di errore in cui è indicato che RS_SHP non è supportato con PREPAREIMAGE](#bkmk_RS_SHP_notsupported)  

### <a name="report-server-configuration-manager-does-not-start"></a><a name="bkmk_configmanager_notstart"></a> Gestione configurazione del server di report non si avvia

 **Descrizione:** questo è un problema che si verifica per motivi strutturali in SQL Server 2012 e nelle versioni successive. Reporting Services è progettato per l'architettura del servizio SharePoint. Gestione configurazione non è più necessario per la configurazione e l'amministrazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità SharePoint.  
  
 **Soluzione alternativa:** utilizzare Amministrazione centrale SharePoint per configurare un server di report in modalità SharePoint. Per altre informazioni, vedere [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-do-not-see-the-sql-server-reporting-services-service-in-sharepoint-central-administration-after-installing-sql-server-2016-ssrs-in-sharepoint-mode"></a><a name="bkmk_no_ssrs_service"></a> Il servizio SQL Server Reporting Services in Amministrazione centrale SharePoint non viene visualizzato dopo l'installazione di SQL Server 2016 SSRS in modalità SharePoint  
 **Descrizione:** se dopo l'installazione di SQL Server 2016 Reporting Services in modalità SharePoint e del componente aggiuntivo di SQL Server 2016 Reporting Services per SharePoint 2013/2016 non viene visualizzato "SQL Server Reporting Services" nei due menu seguenti, il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non è stato registrato:  
  
-   Amministrazione centrale SharePoint 2013/2016 -> "Gestione applicazioni" -> pagina "Gestisci servizi nel server"  
  
-   Amministrazione centrale SharePoint 2013/2016 -> "Gestione applicazioni" -> "Gestisci applicazioni di servizio" -> menu "Nuovo"  
  
 **Soluzione alternativa:** per registrare e avviare SharePoint Services per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], completare i passaggi seguenti riportati di seguito:  
  
1.  Sul computer che esegue Amministrazione centrale SharePoint 2013/2016  
  
    1.  Aprire la shell di gestione di SharePoint 2013/2016 con privilegi di amministratore. Fare clic con il pulsante destro del mouse sull'icona e scegliere **Esegui come amministratore**. Eseguire i tre cmdlet seguenti dalla shell:  
  
    2.  ```  
        Install-SPRSService  
        ```  
  
    3.  ```  
        Install-SPRSServiceProxy  
        ```  
  
    4.  ```  
        Get-SPServiceInstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
        ```  
  
2.  Verificare che il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] riporti lo stato "**Avviato**" nella pagina: Amministrazione centrale SharePoint 2013/2016 -> "**Gestione applicazioni**" -> "**Gestisci servizi nel server**"  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="reporting-services-powershell-cmdlets-are-not-available-and-commands-are-not-recognized"></a><a name="bkmk_cmdlets_not_recognized"></a> I cmdlet di PowerShell per Reporting Services non sono disponibili e i comandi non sono riconosciuti  
 **Descrizione**: quando si tenta di eseguire un cmdlet di PowerShell per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], viene visualizzato un messaggio di errore simile al seguente:  
  
-   Termine 'Install-SPRSServiceInstall-SPRSService' **non riconosciuto** come nome di cmdlet, funzione, programma eseguibile o file script. Verificare l'ortografia del nome, che il percorso sia incluso e corretto, quindi riprovare. At line:1 char:39+ Install-SPRSServiceInstall-SPRSService <<<<    + CategoryInfo          : ObjectNotFound: (Install-SPRSServiceInstall-SPRSService:String) [], CommandNotFoundExcep  
  
 **Soluzione alternativa:** completare una delle operazioni seguenti:  
  
-   Eseguire il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per prodotti SharePoint. **rssharepoint.msi**.  
  
-   Installare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità SharePoint dal supporto di installazione di SQL Server.  
  
 Se la **Shell di gestione di SharePoint 2013/2016** è aperta quando viene completata una delle soluzioni alternative, chiuderla e riaprirla.  
  
 Per altre informazioni, vedere gli articoli seguenti:  
  
-   [Posizione in cui trovare il componente aggiuntivo Reporting Services per prodotti SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  
  
-   [Installare il primo server di report in modalità SharePoint](install-the-first-report-server-in-sharepoint-mode.md)  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-see-an-error-message-indicating-the-url-is-not-configured"></a><a name="bkmk_URL_not_configured"></a> Viene visualizzato un messaggio di errore nel quale è indicato che l'URL non è configurato  
 **Descrizione**: viene visualizzato un messaggio di errore simile al seguente:  
  
 Questa funzionalità di SQL Server Reporting Services (SSRS) non è supportata. Usare Amministrazione centrale per verificare e correggere uno o più dei seguenti problemi:
 
 - L'URL di un server di report non è configurato. Usare la pagina Integrazione di SSRS per impostarlo.
 
 - Il proxy dell'applicazione del servizio SSRS non è configurato. Usare le pagine dell'applicazione del servizio SSRS per configurare il proxy.
 
 - Non è stato eseguito il mapping dell'applicazione del servizio SSRS a questa applicazione Web. Utilizzare le pagine relative all'applicazione del servizio SSRS per associare il proxy dell'applicazione del servizio Web al gruppo di proxy dell'applicazione per questa applicazione Web. 
  
 **Soluzione alternativa:** nel messaggio di errore sono contenuti tre passaggi suggeriti per correggere questo problema. Il primo suggerimento nel messaggio 'Un URL del server di report non è configurato...' è pertinente per l'integrazione con la versione del server di report anteriore a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. La configurazione di SharePoint per le versioni del server di report precedenti viene completata nella pagina **Impostazioni generali applicazione** usando **SQL Server Reporting Services (2008 e 2008 R2)**.  
  
 **Ulteriori informazioni:** questo messaggio di errore verrà visualizzato quando si tenta di utilizzare qualsiasi funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che richiede una connessione al servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ad esempio:  
  
-   Apertura di Generatore report di SQL Server da una raccolta documenti di SharePoint.  
  
-   Gestione di sottoscrizioni.  
  
-   Gestione di un'applicazione di servizio  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="setup-fails-on-a-computer-with-sharepoint-installed-but-it-is-not-configured"></a><a name="bkmk_sharepoint_not_confiugred"></a> La configurazione non riesce in un computer con SharePoint installato ma non configurato  
 **Descrizione:** se si sceglie di installare la modalità SharePoint di Reporting Services in un computer con SharePoint installato ma non configurato, verrà visualizzato un messaggio simile al seguente e l'installazione viene arrestata:  
  
 L'installazione di SQL Server ha smesso di funzionare  
  
 **Soluzione alternativa:** configurare SharePoint ed eseguire l'installazione di SQL Server.  
  
 **Ulteriori informazioni:** quando si installa [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in un'installazione di SharePoint, il programma di installazione tenta di installare e avviare il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint. L'installazione del servizio non riesce se SharePoint non è configurato.  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="sharepoint-central-administration-page-is-blank"></a><a name="bkmk_central_admin_blank"></a> La pagina Amministrazione centrale SharePoint è vuota  
 **Descrizione:** SharePoint 2013/2016 è stato installato senza errori. Tuttavia quando si passa ad Amministrazione centrale, viene visualizzata una pagina vuota.  
  
 **Soluzione alternativa:** questo non è un problema specifico di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ma è relativo alla configurazione delle autorizzazioni nell'installazione generale di SharePoint. Ecco alcuni suggerimenti per iniziare:  
  
-   Leggere l'argomento relativo a SharePoint negli ambienti di sviluppo. [Configurare un ambiente di sviluppo generale per SharePoint](/sharepoint/dev/general-development/set-up-a-general-development-environment-for-sharepoint)  
  
-   Esaminare il post del forum: [Amministrazione centrale restituisce una pagina vuota dopo l'installazione in Windows 7](https://social.technet.microsoft.com/Forums/en/sharepoint2010setup/thread/a422a3c8-39f6-4b9e-988a-4c4d1e745694)  
  
-   L'account del servizio che si sta usando per i servizi SharePoint, ad esempio il servizio Amministrazione centrale SharePoint 2013/2016, deve disporre dei privilegi amministrativi nel sistema operativo locale.  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-see-an-error-message-when-you-try-to-create-a-new-report-builder-report"></a><a name="bkmk_reportbuilder_newreport_error"></a> Viene visualizzato un messaggio di errore quando si tenta di creare un nuovo report di Generatore report  
 **Descrizione:** viene visualizzato un messaggio di errore simile al seguente quando si tenta di creare un report di Generatore report in una libreria di documenti:  
  
 Questa funzionalità non è supportata perché non esiste un'applicazione del servizio SQL Server Reporting Services o non è stato configurato un URL del server di report in Amministrazione centrale.  
  
 **Soluzione alternativa:** verificare che l'applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sia disponibile e configurata correttamente. Per altre informazioni, vedere [Installare il primo server di report in modalità SharePoint](install-the-first-report-server-in-sharepoint-mode.md).
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-see-an-error-message-that-rs_shp-is-not-supported-with-prepareimage"></a><a name="bkmk_RS_SHP_notsupported"></a> Viene visualizzato un messaggio di errore in cui è indicato che RS_SHP non è supportato con PREPAREIMAGE  
 **Descrizione:** quando si tenta di eseguire PREPAREIMAGE per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], viene visualizzato un messaggio di errore simile al seguente:  
  
 "Funzionalità specificata 'RS_SHP' non supportata durante l'esecuzione dell'azione PREPAREIMAGE perché l'utilità SysPrep non è supportata. Rimuovere le funzionalità non compatibili con SysPrep ed eseguire nuovamente l'installazione".  
  
 **Soluzione alternativa:** Non esistono soluzioni alternative. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non supporta SYSPREP (PREPAREIMAGE). [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non supporta SYSPREP.  
  
 ![Icona a forma di freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Risoluzione dei problemi relativi alle installazioni della modalità SharePoint](#bkmk_tshoot_sharepoint)  

::: moniker-end
  
##  <a name="troubleshoot-problems-with-the-native-mode-installations"></a><a name="bkmk_tshoot_native"></a> Risolvere i problemi relativi alle installazioni della modalità nativa  
  
###  <a name="performance-counters-are-not-visible-after-upgrading-to-windows-vista-or-windows-server-2008"></a><a name="PerfCounters"></a> I contatori delle prestazioni non sono visibili al termine dell'aggiornamento a Windows Vista o Windows Server 2008  
 Se si aggiorna il sistema operativo a [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] o [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] in un computer che esegue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], i contatori delle prestazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non verranno impostati dopo l'aggiornamento.  
  
#### <a name="to-reinstate-reporting-services-performance-counters"></a>Per riattivare i contatori delle prestazioni di Reporting Services  
  
1.  Eliminare le chiavi del Registro di sistema seguenti:  
  
    -   **HKLM\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service**  
  
    -   **HKLM\SYSTEM\CurrentControlSet\Services\MSRS 2016 Windows Service**  
  
2.  Aprire una finestra del prompt dei comandi e digitare il comando seguente:  
  
    -   **run \<** *.NET 4.0 Framework directory* **>\InstallUtil.exe \<** *Report Server Bin directory* **>\ReportingServicesLibrary.dll**  
  
        > [!NOTE]  
        >  Sostituire \<*.NET 4.0 Framework directory*> con il percorso fisico dei file di .NET Framework 4.0 e sostituire \<*Report Server Bin directory*> con il percorso fisico dei file della cartella Bin del server di report.  
  
3.  Riavviare il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Per verificare che il problema sia stato risolto, aprire un Web browser e accedere all'URL del portale Web o del server di report. Aprire quindi Performance Monitor per verificare che i contatori funzionino correttamente.  
  
#### <a name="to-add-the-performance-registry-keys-again-by-using-registry-editor"></a>Per aggiungere nuovamente le chiavi del Registro di sistema delle prestazioni utilizzando l'editor del Registro di sistema  
  
1.  Aprire l'editor del Registro di sistema.  
  
    1.  Fare clic su **Start** e scegliere **Esegui**.  
  
    2.  Nella finestra di dialogo **Esegui** digitare **regedit** nella casella **Apri**.  
  
2.  Nell'editor del Registro di sistema, selezionare la seguente chiave del Registro di sistema: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance`  
  
3.  Fare clic con il pulsante destro del mouse sul nodo **Prestazioni** , scegliere **Nuovo** e fare clic su **Valore multistringa**.  
  
4.  Digitare **Counter Names** e quindi premere INVIO.  
  
5.  Ripetere il passaggio per aggiungere la chiave del Registro di sistema **Counter Types** in questo nodo.  
  
6.  Passare alla chiave del Registro di sistema seguente: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance`  
  
7.  Fare clic con il pulsante destro del mouse sul nodo **Prestazioni** , scegliere **Nuovo** e fare clic su **Valore multistringa**.  
  
8.  Digitare **Counter Names** e quindi premere INVIO.  
  
9. Ripetere il passaggio per aggiungere la chiave del Registro di sistema **Counter Types** in questo nodo.  
  
 Dopo aver ripristinato l'istanza a 64 bit o aggiunto nuovamente le chiavi del Registro di sistema manualmente, è possibile utilizzare Performance Monitor per configurare gli oggetti prestazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da monitorare.  
  
###  <a name="reportserverexternalurl-and-passthroughcookies-configuration-properties-are-not-configured-after-an-upgrade-from-sql-server-2005"></a><a name="ConfigPropsMissing"></a> Le proprietà di configurazione ReportServerExternalURL e PassThroughCookies non sono configurate dopo un aggiornamento da SQL Server 2005  
 Quando si esegue l'aggiornamento da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)], le proprietà di configurazione **ReportServerExternalURL** e **PassThroughCookies** non vengono configurate dal processo di aggiornamento. **ReportServerExternalURL** è una proprietà facoltativa e deve essere impostata solo se si usano le web part di SharePoint 2.0 e si vuole che gli utenti siano in grado di recuperare un report e di aprirlo in una nuova finestra del browser. Per altre informazioni su **ReportServerExternalURL**, vedere [URL nei file di configurazione &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md). **PassThroughCookies** è richiesto solo se si usa il metodo di autenticazione Personalizzato. Per altre informazioni su **PassThroughCookies**, vedere [Configurare il portale Web per il passaggio di cookie di autenticazione personalizzati](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md).  
  
> [!NOTE]  
>  Quando si utilizza l'autenticazione personalizzata, si consiglia di eseguire la migrazione dell'installazione anziché un aggiornamento. Per altre informazioni sulla migrazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Eseguire la migrazione di un'installazione di Reporting Services &#40;modalità nativa&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
 Per impostazione predefinita, queste proprietà non esistono nella configurazione di [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . Se queste proprietà sono state configurate in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e le funzionalità che forniscono continuano ad essere richieste, è necessario aggiungerle manualmente al file **RSReportServer.config** dopo il processo di aggiornamento. Per altre informazioni, vedere [Modificare un file di configurazione di Reporting Services&#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  

### <a name="401-unauthorized-error-when-using-windows-authentication-after-an-upgrade-from-sql-server-2005-to-sql-server-2016"></a><a name="WindowsAuthBreaksAfterUpgrade"></a> 401: autorizzazione negata quando si usa l'autenticazione di Windows dopo un aggiornamento da SQL Server 2005 a SQL Server 2016

 Se si esegue l'aggiornamento da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] e si usa l'autenticazione NTLM con un account predefinito per l'account del servizio del server di report, è possibile che si verifichi l'errore 401 di autorizzazione negata quando si accede al server di report o al portale Web dopo l'aggiornamento.  
  
 L'errore si verifica a causa di una modifica nella configurazione predefinita di [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] per l'autenticazione di Windows. La negoziazione viene configurata quando l'account del servizio del server di report è Servizio di rete o Sistema locale. L'autenticazione NTLM viene configurata quando l'account del servizio del server di report non è un account predefinito. Per risolvere questo problema dopo l'aggiornamento, modificare il file RSReportServer.config e configurare **AuthenticationType** su **RSWindowsNTLM**. Per altre informazioni, vedere [Configurare l'autenticazione di Windows in un server di report](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md).  

### <a name="uninstalling-32-bit-instance-of-sql-server-2016-reporting-services-in-side-by-side-deployment-with-a-64-bit-instance-breaks-the-64-bit-instance"></a><a name="Uninstall32BitBreaks64Bit"></a> La disinstallazione dell'istanza a 32 bit di SQL Server 2016 Reporting Services in una distribuzione side-by-side con un'istanza a 64 bit interrompe l'istanza a 64 bit

 Quando si esegue l'installazione side-by-side di un'istanza a 32 bit e di un'istanza a 64 bit di [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] in un computer e si disinstalla l'istanza a 32 bit, quattro chiavi del Registro di sistema di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vengono rimosse. Di conseguenza, l'istanza a 64 bit di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene interrotta. Le chiavi del Registro di sistema di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che vengono rimosse quando si disinstalla l'istanza a 32 bit sono:  
  
 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance:Counter Names` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Windows Service\Performance:Counter Names` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance:Counter Types` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Windows Service\Performance:Counter Types`  
  
 Per risolvere questo problema, è possibile ripristinare l'istanza a 64 bit. Sebbene il ripristino sia l'operazione consigliata, è comunque possibile aggiungere nuovamente le chiavi del Registro di sistema manualmente utilizzando l'editor del Registro di sistema.  
  
> [!CAUTION]  
>  Se il Registro di sistema viene modificato in modo non appropriato, il sistema potrebbe venire gravemente danneggiato. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.  
  
##  <a name="additional-resources"></a><a name="bkmk_additional"></a> Risorse aggiuntive  
 Di seguito sono riportate ulteriori risorse disponibili per la risoluzione dei problemi:  
  
-   Wiki di TechNet: [Risoluzione dei problemi di SQL Server Reporting Services (SSRS) in modalità integrata SharePoint 2010](https://social.technet.microsoft.com/wiki/contents/articles/troubleshoot-sql-server-reporting-services-ssrs-in-sharepoint-integrated-mode.aspx)  
  
-   [Domande e risposte Microsoft: SQL Server Reporting Services](/answers/topics/sql-server-reporting-services.html)  
  
-   Si desidera sottoporre commenti o altre domande? Visitare [Microsoft SQL Server UserVoice](https://feedback.azure.com/forums/908035-sql-server).  
  
