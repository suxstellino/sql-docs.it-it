---
title: Che cos'è SQL Server Reporting Services | Microsoft Docs
description: Informazioni sugli strumenti e i servizi per i report di Reporting Services impaginati e per dispositivi mobili in locale.
ms.date: 05/06/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services]
- SSRS
- reports [Reporting Services], about reports
- Reporting Services
- SQL Server Reporting Services
ms.assetid: b8d18d3d-9db0-43e7-8286-7b46cc3a37ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 08f9271c7b42e7db3d42ffadc111e9b86dd38e68
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596187"
---
# <a name="what-is-sql-server-reporting-services-ssrs"></a>Che cos'è SQL Server Reporting Services (SSRS)?

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

Per informazioni sul server di report di Power BI, vedere [Che cos'è Server di report di Power BI?](/power-bi/report-server/get-started)

SQL Server Reporting Services (SSRS) offre un set di servizi e strumenti locali per creare, distribuire e gestire report impaginati e per dispositivi mobili.

![Insieme di SQL Server Reporting Services](../reporting-services/media/ss-reporting-services-all-together.png "Insieme di SQL Server Reporting Services")

## <a name="create-deploy-and-manage-mobile-and-paginated-reports"></a>Creare, distribuire e gestire report impaginati e per dispositivi mobili

La soluzione SSRS offre in modo flessibile le informazioni corrette agli utenti giusti. Gli utenti possono utilizzare i report tramite Web browser, su un dispositivo mobile o tramite posta elettronica.

SQL Server Reporting Services offre una famiglia di prodotti aggiornata:

* **Report impaginati "tradizionali"** aggiornati, in modo da poter creare report dall'aspetto moderno, con strumenti aggiornati e nuove funzionalità per crearli.
* **Nuovi report per dispositivi mobili** con un layout reattivo, in grado di adattarsi a diversi dispositivi e a diverse modalità di orientamento.
* **Un portale Web moderno** visualizzabile in qualsiasi browser moderno. Nel nuovo portale è possibile organizzare e visualizzare report impaginati e per dispositivi mobili e indicatori KPI di Reporting Services. È anche possibile archiviare cartelle di lavoro di Excel nel portale.

Nelle sezioni che seguono sono disponibili informazioni più dettagliate.

### <a name="whats-new-in-reporting-services"></a>Novità di Reporting Services

Le fonti seguenti consentono di rimanere aggiornati sulle nuove funzionalità di SQL Server Reporting Services:

* [Novità di Reporting Services](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)
* [Blog del team di SQL Server Reporting Services](/archive/blogs/sqlrsteamblog/)
* Il [video di Guy in a Cube sul canale YouTube](https://www.youtube.com/channel/UCFp1vaKzpfvoGai0vE5VJ0w)

## <a name="paginated-reports"></a>Report impaginati

![Immagine dei report impaginati in una schermata del desktop e in un dispositivo tablet.](../reporting-services/media/ssrs-paginated-reports.png)

Reporting Services è associato a report impaginati "tradizionali", ideali per documenti con layout fisso ottimizzati per la stampa, ad esempio file PDF e Word.

Questo carico di lavoro BI di base esiste tuttora e pertanto è stato modernizzato. Ora è possibile creare report dall'aspetto moderno con nuove funzionalità aggiornate usando Generatore report o Progettazione report in [SQL Server Data Tools (SSDT)](../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).

* Tutti gli stili e le tavolozze dei colori predefiniti sono stati aggiornati in modo da poter creare report con un nuovo stile moderno minimalista.
* Il riquadro dei parametri è stato aggiornato ed è ora possibile disporre i parametri nell'ordine desiderato.
* I report possono essere esportati in nuovi formati, ad esempio PowerPoint. Le visualizzazioni di Reporting Services in PowerPoint sono attive e modificabili, non semplici screenshot.
* È possibile creare un'esperienza ibrida di Power BI/Reporting Services. Anziché ricreare i report di Reporting Services locali in Power BI, è possibile aggiungere elementi visivi da tali report ai dashboard di Power BI, in modo da monitorare tutto da un'unica posizione nel proprio dashboard di Power BI.

## <a name="mobile-reports"></a>Report per dispositivi mobili

![Immagine dei report per dispositivi mobili in una schermata del desktop e in un dispositivo tablet.](../reporting-services/media/ssrs-mobile-reports.png)

Con il mobile computing, i dispositivi necessari per lavorare sono cambiati e pertanto le persone hanno esigenze diverse per quanto riguarda la generazione di report. I report con layout fisso non sono adatti per tablet e smartphone. Un elemento progettato per uno schermo ampio di PC non viene visualizzato in modo ottimale sul display di uno smartphone che, oltre ad avere dimensioni più piccole, consente l'orientamento orizzontale o verticale.

Con fattori di forma così diversi, è necessario un layout reattivo che si adatti alle diverse dimensioni e ai diversi orientamenti dello schermo. È stato quindi aggiunto un nuovo tipo di report, per dispositivi mobili, che è basato sulla tecnologia Datazen, acquisita da Microsoft circa un anno fa e integrata nel prodotto. È possibile eseguire la migrazione dei report Datazen esistenti in Reporting Services con [SQL Server Migration Assistant for Datazen](https://www.microsoft.com/download/details.aspx?id=53128).

I report per dispositivi mobili vengono creati con la nuova app [Mobile Report Publisher](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) . Nelle [app Power BI per dispositivi mobili](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) native per Windows 10, iOS, Android e HTML5 è quindi possibile accedere ai dati presenti in Power BI, nel cloud o in SSRS.

Mentre si creano visualizzazioni, Mobile Report Publisher genera automaticamente dati di esempio. Questa funzionalità consente di verificare l'aspetto della visualizzazione con i dati e la tipologia di dati adatta a ogni visualizzazione.

## <a name="web-portal"></a>Portale Web

![Immagine di un portatile con il portale Web visualizzato.](../reporting-services/media/ssrs-web-portal.png)

Per gli utenti finali di Reporting Services in modalità nativa, il punto di ingresso è un portale Web moderno visualizzabile nella maggior parte dei browser. Nel nuovo portale sarà possibile accedere a tutti i report impaginati e per dispositivi mobili Reporting Services, nonché agli indicatori KPI in modo da visualizzare le principali metriche aziendali immediatamente nel browser, senza dover aprire un report.

Il nuovo portale Web è una riscrittura completa di Report Manager. Attualmente è un'app HTML5 a pagina singola, basata su standard, per cui sono ottimizzati i browser moderni: Microsoft Edge, Internet Explorer 10 e 11, Chrome, Firefox, Safari e tutti i principali browser.

Il contenuto del portale Web è organizzato in base al tipo:

* report impaginati
* report per dispositivi mobili 
* KPI
* cartelle di lavoro di Excel
* set di dati condivisi
* origini dati condivise

È possibile archiviare e gestire questi elementi in modo sicuro nel portale con la gerarchia di cartelle classica. Contrassegnare i report preferiti per accedervi rapidamente. Gli utenti con le autorizzazioni appropriate possono gestire e amministrare il contenuto SSRS.

Nel nuovo portale Web è ancora possibile pianificare l'elaborazione dei report, accedere ai report su richiesta e sottoscrivere report pubblicati.

Per altre informazioni, vedere l'articolo relativo al [portale Web](../reporting-services/web-portal-ssrs-native-mode.md).

::: moniker range="=sql-server-2016"

## <a name="reporting-services-in-sharepoint-integrated-mode"></a>Reporting Services in modalità integrata SharePoint

I report vengono pubblicati in Reporting Services in modalità integrata SharePoint. È possibile pianificare l'elaborazione dei report, accedere ai report su richiesta, sottoscrivere report pubblicati ed esportare report in altre applicazioni, ad esempio Microsoft Excel. È possibile creare avvisi dati nei report pubblicati in un sito di SharePoint e ricevere messaggi di posta elettronica quando i dati del report cambiano.  

Per altre informazioni, vedere [Server di report di Reporting Services in modalità SharePoint integrata](../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md).

::: moniker-end

## <a name="ssrsnoversion-programming-features"></a>Funzionalità di programmazione di[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]

Sfruttando le funzionalità di programmazione di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] è possibile estendere e personalizzare le funzionalità di creazione di report. Usare le API di SSRS per integrare o estendere l'elaborazione di dati e report in applicazioni personalizzate.

Per altre informazioni, vedere [Guida per gli sviluppatori (Reporting Services)](../reporting-services/reporting-services-developer-documentation.md).

## <a name="next-steps"></a>Passaggi successivi

* [Installare Reporting Services](../reporting-services/install-windows/install-reporting-services.md)
* [Scaricare la versione più recente di SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)
* [Installare Generatore report](../reporting-services/install-windows/install-report-builder.md)

* Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)