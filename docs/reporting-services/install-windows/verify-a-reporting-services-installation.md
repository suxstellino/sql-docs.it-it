---
description: Verify a Reporting Services Installation
title: Verificare un'installazione di Reporting Services | Microsoft Docs
ms.date: 06/03/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- checking report server installations
- verifying report server installations
- Report Designer [Reporting Services], verifying installations
- installing Reporting Services, verifying installations
- report servers [Reporting Services], verifying installations
- Setup [Reporting Services], verifying installations
ms.assetid: 82a51a99-66f0-4b0c-b05b-07d22387adb0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3359888ec94a9893dc018782f73827f876e95037
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472402"
---
# <a name="verify-a-reporting-services-installation"></a>Verify a Reporting Services Installation
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] possono essere installati in modalità nativa o in modalità SharePoint. La procedura da seguire per verificare l'installazione dipende dalla modalità del server di report.  

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.

::: moniker range="=sql-server-2016"
  
##  <a name="verify-sharepoint-mode-installation"></a><a name="bkmk_sharepointmode"></a> Verifica dell'installazione in modalità SharePoint  
  
### <a name="to-verify-the-reporting-services-service"></a>Per verificare il servizio Reporting Services  
  
1.  Da Amministrazione centrale SharePoint fare clic su **Gestisci servizi nel server** nel gruppo **Impostazioni di sistema** .  
  
2.  Verificare che il **Servizio SQL Server Reporting Services** sia installato e **In esecuzione** .  
  
     Se non si visualizza il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nell'elenco, verificare che il servizio sia installato. Per altre informazioni, vedere [Installare il primo server di report in modalità SharePoint](install-the-first-report-server-in-sharepoint-mode.md).  
  
### <a name="to-verify-the-service-application"></a>Per verificare l'applicazione di servizio  
  
1.  Per verificare da Amministrazione centrale di disporre di almeno un'applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , fare clic su **Gestisci applicazioni di servizio** nel gruppo **Gestione dell'applicazione** .  
  
2.  Verificare che siano disponibili un'applicazione di servizio di tipo **Applicazione di servizio SQL Server Reporting Services** e un proxy dell'applicazione corrispondente.  
  
3.  Fare clic **accanto** al nome dell'applicazione di servizio, quindi fare clic su **Proprietà** nella barra degli strumenti di SharePoint.  Facendo clic sul nome dell'applicazione di servizio verranno visualizzate le relative pagine di gestione, non la pagina delle proprietà.  
  
4.  Verificare che **Associazione applicazione Web** sia configurata per l'applicazione Web desiderata.  
  
### <a name="to-verify-the-site-collection-feature"></a>Per verificare la funzionalità della raccolta siti  
  
1.  Dalle impostazioni del sito fare clic su **Funzionalità raccolta siti** nel gruppo **Amministrazione raccolta siti** .  
  
2.  Verificare che la **Funzionalità di integrazione Server report** sia attiva.  
  
### <a name="to-verify-reporting-server-content-types"></a>Per verificare i tipi di contenuto di un server di report  
  
1.  Per verificare o aggiungere tipi di contenuto di server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vedere [Aggiungere i tipi di contenuto di Reporting Services a una libreria SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
### <a name="to-verify-you-can-launch-report-builder"></a>Per verificare la possibilità di avviare Generatore report  
  
1.  Da una raccolta documenti fare clic su **Documenti** nella barra multifunzione di SharePoint.  
  
2.  Fare clic su **Nuovo documento** , quindi scegliere **Report di Generatore report**. Se questa opzione non viene visualizzata, rivedere la procedura precedente per l'aggiunta dei tipi di contenuto del server di report.  
  
### <a name="create-a-basic-report"></a>Creare un report di base  
  
1.  In una raccolta documenti di SharePoint creare un report semplice di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] contenente solo una casella di testo, ad esempio un titolo. Il report non contiene origini dati e set di dati. L'obiettivo consiste nel verificare che sia possibile aprire Generatore report. In tal caso, verrà visualizzato in anteprima un report di base.  
  
2.  Salvare il report nella raccolta documenti e quindi eseguire il report dalla raccolta. Per altre informazioni sulla creazione di report con Generatore report, vedere [Avviare Generatore report](../report-builder/start-report-builder.md).  
  
### <a name="reporting-services-samples"></a>Esempi di Reporting Services  
  
1.  Completare una delle esercitazioni di Reporting Services. Per altre informazioni, vedere [Esercitazioni su Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md).  
  
2.  Scaricare il database di esempio Adventure Works e i report di esempio di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] da GitHub. Per ulteriori informazioni, vedere [Database di esempio AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  

::: moniker-end
  
##  <a name="verify-a-native-mode-installation"></a><a name="bkmk_nativemode"></a> Verificare un'installazione in modalità nativa  
 Quando si installa un server di report in modalità nativa utilizzando la configurazione predefinita, verranno eseguite l'installazione e la distribuzione del server. Per verificare se la distribuzione del server di report viene completata, è sufficiente eseguire alcuni semplici test. A tale scopo occorre disporre dei privilegi di amministratore locale. Per consentire l'esecuzione dei test da parte di altri utenti, è necessario configurare l'accesso di tali utenti al server di report.  
  
### <a name="to-verify-that-the-report-server-is-installed-and-running"></a>Per verificare che il server di report sia installato e in esecuzione  
  
1.  Eseguire lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi all'istanza del server di report installata. Nella pagina URL servizio Web è disponibile un collegamento al servizio Web ReportServer. Fare clic sul collegamento per verificare che sia possibile accedere al server. Se il database del server di report non è configurato, eseguire questa operazione prima di fare clic sul collegamento.  
  
2.  Aprire l'applicazione console Servizi e verificare che il servizio del server di report sia in esecuzione. Per visualizzare lo stato del servizio del server di report, fare clic sul menu **Start**, scegliere **Pannello di controllo**, fare doppio clic su **Strumenti di amministrazione**, quindi su **Servizi**. Attendere che venga visualizzato l'elenco dei servizi e scorrerlo fino a **Report Server (MSSQLSERVER)**. Verificare che lo stato sia **Avviato**.  
  
3.  Aprire un browser e immettere l'URL del server di report nella barra degli indirizzi. L'indirizzo è composto dal nome del server e dal nome della directory virtuale specificati per il server di report durante l'installazione. Per impostazione predefinita, la directory virtuale del server di report è denominata **ReportServer**. Per verificare l'installazione del server di report, è possibile usare l'URL seguente: https:// *\<computer name>* /ReportServer *\<_instance name>* . L'URL sarà diverso se il server di report è stato installato come istanza denominata. Per altre informazioni sul formato dell'URL, vedere [Configurare gli URL del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md). Se si è un amministratore locale in Windows Vista o Windows Server 2008, vedere [Configurare un server di report in modalità nativa per gli amministratori locali &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
4.  Eseguire alcuni report per testare il funzionamento del server di report. Per questo passaggio è possibile creare un report di esempio da un'esercitazione. Per altre informazioni, vedere [Creare un report tabella semplice &#40;esercitazione su SSRS&#41;](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md).  
  
### <a name="to-verify-that-the-ssrswebportal-is-installed-and-running"></a>Per verificare che il [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] sia installato e in esecuzione  
  
1.  Aprire un browser e immettere l'URL del portale Web nella barra degli indirizzi. L'indirizzo è composto dal nome del server e dal nome della directory virtuale specificati per [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] durante l'installazione o nella pagina URL del portale Web dello strumento di configurazione di Reporting Services. Per impostazione predefinita, la directory virtuale di [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] è **Reports**. Per verificare l'installazione di [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] , è possibile usare l'URL seguente:  
  
     https:// *\<computer name>* /Reports *\<_instance name>* .  
  
2.  Usare [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] per creare una nuova cartella o caricare un file per testare se le definizioni vengono passate al database del server di report. L'esito positivo di queste operazioni indica il corretto funzionamento della connessione.  
  
     Per altre informazioni, vedere [Portale Web &#40;SSRS in modalità nativa&#41;](../../reporting-services/web-portal-ssrs-native-mode.md).  
  
### <a name="to-verify-that-report-designer-is-installed-and-running"></a>Per verificare che Progettazione report sia installato e in esecuzione  
  
1.  Aprire [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]e creare un nuovo progetto basato su un tipo di progetto server di report. Per altre informazioni sull'uso della Creazione guidata progetto server di report, vedere [Reporting Services in SQL Server Data Tools &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).  
  
2.  Se si sono installati esempi di report, aprire i file di progetto dei report di esempio e pubblicare i report in un server di report.  
  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di installazione di Reporting Services](../../reporting-services/install-windows/troubleshoot-a-reporting-services-installation.md)   
 [Causa e risoluzione degli errori di Reporting Services](../../reporting-services/troubleshooting/cause-and-resolution-of-reporting-services-errors.md)  
  
  
