---
title: Usare script per l'esecuzione di attività di distribuzione e di amministrazione | Microsoft Docs
description: Informazioni sugli strumenti, le tecnologie e le strategie per automatizzare l'installazione di routine, la distribuzione e le attività amministrative di un server di report.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- scripts [Reporting Services]
- moving reports
- report servers [Reporting Services], duplicating settings
- deploying [Reporting Services], scripts
- copying report server settings
- administrative tasks [Reporting Services]
- duplicating report server environment
- migrating reports [Reporting Services]
- scripts [Reporting Services], deployments
- transferrng reports
- reports [Reporting Services], migrating
ms.assetid: d0416c9e-e3f9-456d-9870-2cfd2c49039b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 883acc6af22c69b6deda8606c2361e33e55aeb1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077922"
---
# <a name="script-deployment-and-administrative-tasks"></a>Utilizzare script per l'esecuzione di attività di distribuzione e di amministrazione

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] supporta l'uso di script per automatizzare installazioni di routine, distribuzioni e attività amministrative. La distribuzione di un server di report è un processo costituito da più passaggi. Per configurare una distribuzione è necessario utilizzare diversi strumenti e processi, in quanto non è disponibile un unico programma o approccio che consenta di automatizzare tutte le attività.  
  
 Non è consigliabile automatizzare tutti i passaggi. In alcuni casi, l'esecuzione di un passaggio in modo manuale oppure tramite uno strumento grafico costituisce l'approccio migliore e più efficace. Se, ad esempio, si desidera distribuire un numero elevato di report e modelli, è consigliabile copiare i database del server di report anziché scrivere codice per ricreare l'ambiente del server di report.  
  
 Per alcuni passaggi è necessario utilizzare codice personalizzato. È ad esempio possibile configurare gli URL del servizio Web e Gestione report in modo automatico, ma solo scrivendo codice personalizzato per l'esecuzione di chiamate nel provider WMI (Windows Management Instrumentation, Strumentazione gestione Windows) di Server report. Se non si desidera scrivere codice, per eseguire il passaggio è necessario usare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Per eseguire script per la configurazione di un server di report, è necessario essere un amministratore locale nel computer che si sta configurando. Per altre informazioni, vedere [Configurare un server di report per l'amministrazione remota](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).  
  
 In questo argomento vengono descritti gli approcci consigliati per l'automatizzazione di passaggi specifici. Sono menzionati vari programmi e interfacce programmatiche, per ognuno dei quali è disponibile una descrizione di seguito in questo argomento.  
  
## <a name="deployment-tasks-and-how-to-automate-them"></a>Attività di distribuzione e modalità di automatizzazione di tali attività  
 Nella tabella seguente sono riepilogate le attività di installazione e configurazione necessarie per la distribuzione di un server di report. È possibile utilizzare la tabella per individuare, per un'attività specifica, un approccio che consenta di eseguire l'attività in modo automatico.  
  
|Attività|Approccio|  
|----------|--------------|  
|Installare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Per eseguire un'installazione automatica, è possibile avviare il programma di installazione dalla riga di comando.<br /><br /> È possibile utilizzare il programma di installazione sia per installare che per configurare un server di report, ma solo se si specifica l'opzione di configurazione predefinita e se il sistema soddisfa tutti i requisiti per il tipo di installazione. Se non è possibile installare la configurazione predefinita, è necessario eseguire un'installazione di tipo "solo file".|  
|Configurazione dell'account di servizio.|L'account di servizio viene configurato inizialmente tramite il programma di installazione. Per automatizzare le modifiche all'account di servizio come attività della post-installazione, è necessario scrivere codice personalizzato che effettua chiamate al provider WMI di Server report. Non sono disponibili utilità della riga di comando o modelli di script per la configurazione a livello di programmazione dell'account del servizio.<br /><br /> Se i requisiti relativi al codice non consentono di automatizzare questo passaggio, è possibile configurare manualmente in modo semplice l'account eseguendo lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni, vedere [Configurare un account del servizio &#40;Gestione configurazione del server di report&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).|  
|Configurazione del servizio Web ReportServer e degli URL di Gestione Report.|È necessario scrivere codice personalizzato per l'esecuzione di chiamate nel provider WMI di Server report. Non sono disponibili utilità della riga di comando o modelli di script per la configurazione degli URL.<br /><br /> Se si desidera evitare la scrittura di codice, è possibile configurare manualmente gli URL eseguendo lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni, vedere [Configurare un URL &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).|  
|Creazione di un database del server di report.|È necessario scrivere codice personalizzato per l'esecuzione di chiamate nel provider WMI di Server report. Non sono disponibili utilità della riga di comando o modelli di script per la creazione di database del server di report e di RSExecRole.<br /><br /> Se si desidera evitare la scrittura di codice, è possibile creare il database manualmente eseguendo lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni, vedere [Creare un database del server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).|  
|Configurazione della connessione del database del server di report.|Se si modifica la stringa di connessione, l'account o la password oppure il tipo di autenticazione, eseguire l'utilità **rsconfig** per configurare la connessione. Per altre informazioni, vedere [Configurare una connessione del database del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) e [Utilità rsconfig &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md).<br /><br /> Non è possibile utilizzare rsconfig.exe per creare o aggiornare il database. Il database e RSExecRole devono esistere.|  
|Configurazione di una distribuzione con scalabilità orizzontale.|Per eseguire in modo automatico una distribuzione con scalabilità orizzontale, scegliere uno degli approcci seguenti:<br /><br /> -   Eseguire l'utilità rskeymgmt.exe per unire in join le istanze del server di report e un'installazione esistente. Per altre informazioni, vedere [Aggiungere e rimuovere le chiavi di crittografia per una distribuzione con scalabilità orizzontale &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md).<br />-   Scrivere il codice personalizzato che viene eseguito nel provider WMI per Server report.|  
|Backup delle chiavi di crittografia.|Per eseguire in modo automatico il backup delle chiavi di crittografia, scegliere uno degli approcci seguenti:<br /><br /> -   Eseguire l'utilità rskeymgmt.exe per il backup delle chiavi. Per altre informazioni, vedere [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).<br />-   Scrivere il codice personalizzato che viene eseguito nel provider WMI per Server report.|  
|Configurazione della posta elettronica del server di report.|Scrivere codice personalizzato che viene eseguito nel provider WMI per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Il provider supporta un subset delle impostazioni di configurazione della posta elettronica.<br /><br /> Sebbene il file RSReportServer.config includa tutte le impostazioni, non utilizzare questo file in modo automatizzato. In particolare, non utilizzare un file batch per copiare il file in un altro server di report. Ogni file di configurazione include valori specifici per l'istanza corrente. Tali valori non saranno validi in altre istanze del server di report.<br /><br /> Per altre informazioni sulle impostazioni, vedere [Impostazioni posta elettronica - Modalità nativa di Reporting Services (Gestione configurazione)](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md).|  
|Configurazione dell'account di esecuzione automatica.|Per configurare l'account di esecuzione automatica in modo automatico, scegliere uno degli approcci seguenti:<br /><br /> -   Eseguire l'utilità rsconfig.exe per configurare l'account. Per altre informazioni, vedere [Configurare l'account di esecuzione automatica &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).<br />-   Scrivere codice personalizzato per l'esecuzione di chiamate nel provider WMI per Server report.|  
|Distribuzione di contenuti esistenti in un altro server di report, includendo gerarchia di cartelle, assegnazioni di ruolo, report, sottoscrizioni, pianificazioni, origini dei dati e risorse.|Il modo migliore per ricreare un ambiente del server di report esistente consiste nel copiare il database del server di report in una nuova istanza del server di report.<br /><br /> Un approccio alternativo consiste nello scrivere codice personalizzato per ricreare il contenuto del server di report esistente a livello di programmazione. Tenere presente, tuttavia, che le sottoscrizioni, gli snapshot dei report e la cronologia dei report non possono essere ricreati a livello di programmazione.<br /><br /> Per alcune distribuzioni può essere vantaggioso utilizzare entrambe le tecniche, ovvero ripristinare un database del server di report e quindi eseguire codice personalizzato per modificare il database per un'installazione specifica.<br /><br /> Per un esempio dettagliato, vedere [Script di esempio rs.exe di Reporting Services per la copia di contenuto tra server di report](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).<br /><br /> Per altre informazioni sullo spostamento di un database del server di report, vedere [Spostamento di database del server di report in un altro computer &#40;modalità nativa SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md). Per ulteriori informazioni sulla creazione di un ambiente del server di report a livello di programmazione, vedere la sezione "Utilizzo di script per eseguire la migrazione del contenuto e delle cartelle del server di report" di questo argomento.|  
  
## <a name="tools-and-technologies-for-automating-server-deployment"></a>Strumenti e tecnologie per l'automatizzazione della distribuzione del server  
 Nell'elenco seguente vengono riepilogati i programmi e le interfacce che è possibile utilizzare per automatizzare la distribuzione e le attività di manutenzione:  
  
-   È possibile eseguire il programma di installazione in modalità automatica per installare e, in alcuni casi, configurare i componenti del server di report. Per configurare un'istanza del server di report con il programma di installazione, è necessario utilizzare un'installazione di tipo "solo file".  
  
-   È possibile usare il provider WMI [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e le utilità della riga di comando di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per la configurazione del server locale e remoto.  
  
     Il provider WMI [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] espone classi, proprietà e metodi che consentono di configurare tutti gli aspetti di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , tra cui la definizione dell'account di servizio, la configurazione degli URL, la creazione e la configurazione del database del server di report o la configurazione di un server di report per il recapito tramite posta elettronica. Per utilizzare il provider WMI è necessario scrivere codice personalizzato o script. Per altre informazioni, vedere [Accedere al provider WMI per Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
     Se non si desidera scrivere codice, è possibile utilizzare le utilità della riga di comando rsconfig.exe e rskeymgmt.exe. È possibile scrivere file batch che consentano di eseguire le utilità. Queste utilità consentono di automatizzare alcune attività di configurazione, ma non tutte.  
  
-   Lo strumento host di scripting del server di report (rs.exe) consente di eseguire codice [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] scritto per ricreare contenuti esistenti o spostarli da un server di report a un altro. Per usare questo approccio, scrivere lo script in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], salvarlo come file con estensione rss e quindi usare l'utilità rs.exe per eseguire lo script nel server di report di destinazione. Lo script creato può chiamare l'interfaccia SOAP per il servizio Web ReportServer. Gli script di distribuzione vengono creati utilizzando questo approccio poiché consente di ricreare i contenuti e lo spazio dei nomi della cartella del server di report, nonché la sicurezza basata sui ruoli.  
  
-   Nella versione SQL Server 2012 sono stati introdotti i cmdlet di PowerShell per la modalità integrata SharePoint. È possibile utilizzare PowerShell per configurare e amministrare l'integrazione con SharePoint.  Per altre informazioni, vedere [PowerShell cmdlets for Reporting Services SharePoint Mode](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md)(Cmdlet di PowerShell per la modalità SharePoint di Reporting Services).  
  
## <a name="use-scripts-to-migrate-report-server-content-and-folders"></a>Utilizzare gli script per eseguire la migrazione del contenuto e delle cartelle del server di report  
 È possibile creare script per duplicare un ambiente del server di report in un'altra istanza del server di report. Gli script di distribuzione vengono in genere scritti in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] e successivamente elaborati utilizzando l'utilità host di scripting del server di report.  
  
 Per un esempio dettagliato, vedere [Script di esempio rs.exe di Reporting Services per la copia di contenuto tra server di report](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
 Utilizzare gli script per copiare cartelle, origini dei dati condivise, risorse, report, assegnazioni di ruoli e impostazioni da un server all'altro. È possibile scrivere uno script per un'istanza del server di report, quindi eseguirlo in un altro server per ricreare lo spazio dei nomi del server di report. In presenza di più server di report nella distribuzione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , è possibile eseguire lo script in ogni singolo server per configurare tutti i server allo stesso modo.  
  
 Nell'elenco seguente viene descritta la procedura per migrare report da un server in un altro.  
  
1.  Impostare la variabile script sull'URL del server di report di origine.  
  
2.  Usare i metodi <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A> e <xref:ReportService2010.ReportingService2010.GetProperties%2A> per recuperare la definizione e le proprietà del report.  
  
3.  Impostare l'URL in modo che punti al server di destinazione.  
  
4.  Usare il metodo <xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A> passando le proprietà restituite da <xref:ReportService2010.ReportingService2010.GetProperties%2A> e la definizione del report restituite da <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>.  
  
 Utilizzando una combinazione di metodi Get e Create, è possibile eseguire una procedura simile per migrare impostazioni, cartelle, origini dei dati condivise e risorse. Per altre informazioni sui metodi disponibili, vedere [Riferimento tecnico &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md).  
  
> [!NOTE]  
>  Gli script vengono eseguiti con le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows dell'utente che esegue lo script, a meno che non si decida di impostare le credenziali esplicitamente.  
  
 Per altre informazioni sulla formattazione ed esecuzione di un file di script, vedere [Eseguire lo script con l'utilità rs.exe e il servizio Web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md).  
  
## <a name="using-scripts-to-set-server-properties"></a>Utilizzo degli script per impostare le proprietà del server  
 È possibile scrivere script che consentono di impostare le proprietà di sistema nel server di report. Nello script [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET riportato di seguito viene illustrato un sistema per impostare le proprietà: Nell'esempio il controllo ActiveX RSClientPrint viene disabilitato, tuttavia è possibile sostituire **EnableClientPrinting** e **False** con qualsiasi nome e valore di proprietà valido. Per visualizzare un elenco completo di proprietà del server, vedere [Proprietà di sistema del server di report](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
 Per utilizzare lo script, salvarlo in un file con estensione rss e quindi utilizzare l'utilità della riga di comando rs.exe per eseguire il file nel server di report. Lo script non è compilato, pertanto non è necessario disporre di un'installazione di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Nell'esempio si presuppone che l'utente disponga delle autorizzazioni per il computer locale che ospita il server di report. Se non è stato eseguito l'accesso con un account che dispone delle autorizzazioni, sarà necessario specificare le informazioni sull'account tramite ulteriori argomenti della riga di comando. Per altre informazioni, vedere [Utilità RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md).  
  
> [!TIP]  
>  Per un esempio dettagliato, vedere [Script di esempio rs.exe di Reporting Services per la copia di contenuto tra server di report](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
```  
Public Sub Main()  
        Dim props(0) As [Property]  
        Dim setProp As New [Property]  
        setProp.Name = "EnableClientPrinting"  
        setProp.Value = "False"   
        props(0) = setProp  
        Try  
            rs.SetSystemProperties(props)  
        Catch ex As System.Web.Services.Protocols.SoapException  
            Console.Write(ex.Detail.InnerXml)  
        Catch e as Exception  
            Console.Write(e.Message)  
        End Try  
End Sub  
```

## <a name="next-steps"></a>Passaggi successivi

[Metodo GenerateDatabaseCreationScript &#40;MSReportServer_ConfigurationSetting WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabasecreationscript.md)   
[Metodo GenerateDatabaseRightsScript &#40;MSReportServer_ConfigurationSetting WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaserightsscript.md)   
[Metodo GenerateDatabaseUpgradeScript &#40;MSReportServer_ConfigurationSetting WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)   
[Installare SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
[Installare un server di report in modalità nativa di Reporting Services](~/reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)   
[Server di report di Reporting Services &#40;modalità nativa&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
[Utilità della riga di comando del server di report &#40;SSRS&#41;](../../reporting-services/tools/report-server-command-prompt-utilities-ssrs.md)   
[Supporto browser per Reporting Services e Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)   
[Strumenti di Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
