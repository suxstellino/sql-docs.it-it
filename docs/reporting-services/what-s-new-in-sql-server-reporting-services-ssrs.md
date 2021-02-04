---
title: Novità di Reporting Services | Microsoft Docs
description: Informazioni sulle novità delle diverse versioni di SQL Server Reporting Services, incluse le modifiche alle principali aree funzionali.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 12/05/2019
ms.openlocfilehash: fd8f612727e860f9797a5af46eb77154a19ff5aa
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237162"
---
# <a name="whats-new-in-sql-server-reporting-services-ssrs"></a>Novità di SQL Server Reporting Services (SSRS)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)]

Informazioni sulle novità delle diverse versioni di SQL Server [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Questo articolo illustra le principali aree funzionali e viene aggiornato man mano che vengono rilasciati nuovi elementi.

Per informazioni su Server di report di Power BI, vedere [Che cos'è Server di report di Power BI?](/power-bi/report-server/get-started).

::: moniker range=">=sql-server-ver15"

## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

**Scarica** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

[SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122) è disponibile per il download nell'Area download Microsoft.

### <a name="azure-sql-managed-instance-support"></a>Supporto per Istanza gestita di database SQL di Azure

È ora possibile ospitare un catalogo di database usato per SQL Server Reporting Services (SSRS) in un'istanza gestita SQL di Azure (MI) ospitata in una macchina virtuale o nel data center. Il supporto è limitato all'uso delle credenziali del database per la connessione all'istanza gestita di database SQL.

### <a name="power-bi-premium-dataset-support"></a>Supporto di set di dati di Power BI Premium

È possibile connettersi a set di dati di Power BI usando Generatore report Microsoft o SQL Server Data Tools (SSDT). È quindi possibile pubblicare i report in SSRS 2019 usando la connettività di SQL Server Analysis Services. Per abilitare lo scenario, gli utenti devono usare un nome utente e una password archiviati di Windows.

### <a name="alttext-alternative-text-support-for-report-elements"></a>Supporto di AltText (testo alternativo) per gli elementi del report

Durante la creazione di report è possibile usare descrizioni comando per specificare il testo per ogni elemento nel report. La tecnologia per la lettura dello schermo identifica queste descrizioni comando in modo corretto.

### <a name="azure-active-directory-application-proxy-support"></a>Supporto per Azure Active Directory Application Proxy

Con Azure Active Directory Application Proxy non è più necessario gestire il proxy applicazione Web per consentire l'accesso sicuro tramite le app Web o per dispositivi mobili.

### <a name="custom-headers"></a>Intestazioni personalizzate

Impostare i valori di intestazione per tutti gli URL che corrispondono al modello regex specificato. Gli utenti possono aggiornare il valore dell'intestazione personalizzata con XML valido per impostare i valori di intestazione per gli URL di richiesta selezionati. Gli amministratori possono aggiungere un numero qualsiasi di intestazioni nel codice XML. Per informazioni dettagliate, vedere [Intestazioni personalizzate](tools/server-properties-advanced-page-reporting-services.md#customheaders) nell'articolo **Pagina Avanzate delle proprietà del server**.

### <a name="transparent-database-encryption"></a>Crittografia trasparente del database

SQL Server 2019 supporta ora la crittografia TDE (Transparent Database Encryption) per il database di catalogo di SSRS per le edizioni Enterprise e Standard. 

### <a name="microsoft-report-builder-update"></a>Aggiornamento di Generatore report Microsoft

La nuova versione rilasciata di Generatore report è completamente compatibile con le versioni 2016, 2017 e 2019 di Reporting Services. È compatibile anche con tutte le versioni rilasciate e supportate di Server di report di Power BI.

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

**Scarica** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

Per scaricare SQL Server 2017 Reporting Services, accedere all' **[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=55252)** .

### <a name="comments-on-reports"></a>Commenti nei report

Per i report è ora disponibile la funzionalità dei commenti, per aggiungere un punto di vista e collaborare con altri utenti. È anche possibile includere allegati nei commenti.

![Commenti all'interno di un server di report](media/what-s-new-in-sql-server-reporting-services-ssrs/report-server-comments.png)

Per altre informazioni, vedere [Add comments to a report in a report server](https://powerbi.microsoft.com/documentation/reportserver-add-comments/) (Aggiungere commenti a un report in un server di report).

### <a name="dax-queries-in-reporting-tools"></a>Query DAX negli strumenti di report

Nelle versioni più recenti di Generatore Report e SQL Server Data Tools è possibile creare query DAX native in base ai modelli di dati tabulari di SQL Server Analysis Services. È possibile trascinare i campi nelle finestre di progettazione query. Vedere il [blog di Reporting Services](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools).

### <a name="rest-api-support"></a>Supporto delle API REST

Per consentire lo sviluppo di applicazioni e personalizzazioni moderne, SQL Server Reporting Services ora supporta completamente un'API RESTful conforme a OpenAPI. La documentazione e le specifiche complete dell'API sono ora disponibili in [SwaggerHub](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0).

### <a name="query-designer-support-for-dax-now-in-report-builder-and-sql-server-data-tools"></a>Supporto della progettazione query per DAX in Generatore Report ed SQL Server Data Tools

In Generatore Report e SQL Server Data Tools è ora possibile creare query DAX native nei modelli di dati tabulari di SQL Server Analysis Services supportati. È possibile usare progettazione query in entrambi gli strumenti per trascinare i campi desiderati. La query DAX viene quindi generata automaticamente.

Altre informazioni nel [blog di Reporting Services](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools).

* Scaricare [Generatore report di Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=734968).
* Scaricare [SQL Server Data Tools (versione finale candidata)](../ssdt/download-sql-server-data-tools-ssdt.md).

> [!NOTE]
> è possibile usare la progettazione query per DAX solo con le origini dati tabulari SSAS incorporate in SQL Server 2016 e versioni successive.
::: moniker-end

## <a name="ssrs-2016"></a>SSRS 2016

### <a name="reporting-services-ssrswebportal-non-markdown"></a>Reporting Services [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]  

È disponibile un nuovo [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]. Il portale Web aggiornato include
- KPI
- Report per dispositivi mobili
- Report impaginati
- file Excel
- File di Power BI Desktop

Il [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] sostituisce Gestione Report delle versioni precedenti.

Per creare report per dispositivi mobili, è necessario usare [!INCLUDE[SS_MobileReptPub_Short](../includes/ss-mobilereptpub-short.md)].

Per altre informazioni sul [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)], vedere [Portale Web (modalità nativa SSRS)](../reporting-services/web-portal-ssrs-native-mode.md).  

![Screenshot che mostra il portale di SQL Server Reporting Services.](../reporting-services/media/ssrsportal.png "ssRSPortal")  

#### <a name="custom-branding-for-the-ssrswebportal-non-markdown"></a>Personalizzazione del [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

È possibile personalizzare il [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] con il logo e i colori dell'organizzazione mediante un pacchetto di personalizzazione.  

Per altre informazioni sulla personalizzazione, vedere [Personalizzazione del portale Web](./branding-the-web-portal.md)

#### <a name="key-performance-indicators-kpi-in-the-ssrswebportal-non-markdown"></a>Indicatori di prestazioni chiave (KPI) nel [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

Nel [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] è possibile creare direttamente indicatori KPI contestuali alla cartella corrente. Durante la creazione di indicatori KPI, è possibile scegliere i campi del set di dati e riepilogare i relativi valori. È anche possibile selezionare il contenuto correlato per eseguire il drill-through ed esporre ulteriori dettagli.

![Screenshot che mostra gli indicatori KPI nel portale di SQL Server Reporting Services.](../reporting-services/media/ssrs-webportal-kpi.png)

Per altre informazioni, vedere [Usare gli indicatori KPI in Reporting Services](./working-with-kpis-in-reporting-services.md)

### <a name="mobile-reports"></a>Report per dispositivi mobili

I report per dispositivi mobili di Reporting Services sono report dedicati, ottimizzati per una vasta gamma di fattori di forma, e forniscono un'esperienza ottimale agli utenti che accedono ai report dai dispositivi mobili. I report per dispositivi mobili offrono un'ampia gamma di visualizzazioni, ad esempio grafici temporali, per categorie e di confronto, mappe ad albero e mappe personalizzate. È possibile connettere i report per dispositivi mobili a una gamma di origini dati, tra cui dati locali tabulari e multidimensionali di SQL Server Analysis Services. I campi dei report per dispositivi mobili possono essere posizionati su un'area di progettazione regolando le righe e le colonne della griglia. Gli elementi dei report per dispositivi mobili flessibili si adattano automaticamente in base alle dimensioni di qualsiasi schermo. È quindi possibile salvare i report per dispositivi mobili in un server di Reporting Services, visualizzarli e interagire con essi in un browser o nell'app Power BI per dispositivi mobili. I dispositivi supportati includono:

- iPad
- iPhone
- Telefoni Android
- e qualsiasi dispositivo Windows 10

#### <a name="mobile-report-publisher"></a>Mobile Report Publisher  

[!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)] consente di creare e pubblicare report per dispositivi mobili di SQL Server in [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)].  

![SS_MRP_LayoutTabSmall](../reporting-services/media/ss-mrp-layouttabsm.png "SS_MRP_LayoutTabSmall")  

Per altre informazioni, vedere [Creare report per dispositivi mobili con SQL Server Mobile Report Publisher](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md).  

#### <a name="sql-server-mobile-reports-hosted-in-reporting-services-available-in-power-bi-mobile-app"></a>Report per dispositivi mobili di SQL Server ospitati in Reporting Services disponibili nell'app per dispositivi mobili di Power BI  

L'app Power BI per dispositivi mobili per iOS su iPad e iPhone può ora visualizzare i report per dispositivi mobili di SQL Server ospitati nel server di report locale.  

![SS_MRP_iPad_HomeSm](../reporting-services/media/ss-mrp-ipad-homesm.png "SS_MRP_iPad_HomeSm")  

Non è possibile connettersi per impostazione predefinita senza modificare la configurazione. Per altre informazioni su come consentire all'app per dispositivi mobili di Power BI di connettersi al server di report, vedere [Abilitare un server di report per l'accesso a Power BI per dispositivi mobili](../reporting-services/report-server/enable-a-report-server-for-power-bi-mobile-access.md).

### <a name="support-of-sharepoint-mode-and-sharepoint-2016"></a>Supporto per la modalità SharePoint e SharePoint 2016  

[!INCLUDE[sssql15-md](../includes/sssql16-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] supporta l'integrazione con SharePoint 2013 e SharePoint 2016.

Per altre informazioni, vedere:  

- [Combinazioni supportate di SharePoint, server Reporting Services e componente aggiuntivo &#40;SQL Server 2016&#41;](../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md)  

- [Posizione in cui trovare il componente aggiuntivo Reporting Services per prodotti SharePoint](../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  

- [Installare la modalità SharePoint di Reporting Services](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  

### <a name="microsoft-net-framework-4-support"></a>Supporto per Microsoft .NET Framework 4  

[!INCLUDE[ssRSCurrent-md](../includes/ssrscurrent-md.md)] supporta le versioni correnti di Microsoft .NET Framework 4, incluse le versioni 4.0 e 4.5.1. Se non è già installata una versione di .NET Framework 4.x, il programma di installazione di [!INCLUDE[ssNoVersion-md](../includes/ssnoversion-md.md)] installa .NET 4.0 durante il passaggio di installazione delle funzionalità.  

### <a name="report-improvements"></a>Miglioramenti dei report

**Motore di rendering HTML5:** è disponibile un nuovo motore di rendering HTML5 ottimale per la modalità con standard "completi" per il Web e i browser moderni.  Il nuovo motore di rendering non si basa più su modalità particolari usate da alcuni browser meno recenti.

Per altre informazioni sul supporto dei browser, vedere [Supporto browser per Reporting Services e Power View](../reporting-services/browser-support-for-reporting-services-and-power-view.md).  

**Report moderni impaginati:** è possibile progettare report impaginati moderni accattivanti con nuovi stili attuali per grafici, misuratori, mappe e altre visualizzazioni dei dati.

**Mappa ad albero e grafici radiali:** ottimizzare i report con mappe ad albero ![ssrs_treemap_icon](../reporting-services/media/ssrs-treemap-icon.png "ssrs_treemap_icon") e grafici radiali ![ssrs_sunburst_icon](../reporting-services/media/ssrs-sunburst-icon.png "ssrs_sunburst_icon"), strumenti eccezionali per la visualizzazione di dati gerarchici. Per altre informazioni, vedere [Tree Map and Sunburst Charts in Reporting Services](../reporting-services/report-design/tree-map-and-sunburst-charts-in-reporting-services.md).  

**Incorporamento di report:** è ora possibile incorporare report per dispositivi mobili e report impaginati in altre pagine Web e applicazioni usando un iframe e i parametri URL.  

**Aggiunta di elementi di un report a un dashboard di Power BI:** quando si visualizza un report nel [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], è possibile selezionare elementi del report e aggiungerli a un dashboard di [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .   Gli elementi che è possibile aggiungere sono i grafici, i pannelli dei misuratori, le mappe e le immagini. È possibile:

1. Selezionare il gruppo contenente il dashboard a cui si vogliono aggiungere gli elementi.
2. Selezionare il dashboard a cui si vuole aggiungere l'elemento.
3. Selezionare la frequenza di aggiornamento del riquadro nel dashboard.

![nota](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "nota") L'aggiornamento è gestito dalle sottoscrizioni di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] e, dopo l'aggiunta dell'elemento, è possibile modificare la sottoscrizione e configurare un'altra pianificazione dell'aggiornamento.

![Screenshot che mostra la finestra di dialogo Aggiungi al dashboard di Power BI.](../reporting-services/media/ssrs-pin-to-powerbi.png) 

Per altre informazioni, vedere [Integrazione del server di report di Power BI &#40;Gestione configurazione&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md) e [Aggiungere elementi di Reporting Services ai dashboard di Power BI](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md).  

**Rendering ed esportazione in PowerPoint:** il formato Microsoft PowerPoint (PPTX) è una nuova estensione per il rendering di [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]. È possibile esportare report in formato PPTX dalle applicazioni consuete, ovvero Generatore report, Progettazione report (in SSDT) e dal [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. L'immagine seguente mostra ad esempio il menu per l'esportazione dal [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. 

![Screenshot che mostra l'elenco a discesa Esporta con l'opzione PowerPoint evidenziata.](../reporting-services/media/ssrs-export-powerpoint.png) 

È anche possibile selezionare il formato PPTX per l'output della sottoscrizione e usare l'accesso URL del server di report per eseguire il rendering e l'esportazione di un report. Ad esempio, il comando URL seguente nel browser esporta un report da un'istanza denominata del server di report.  

```https
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  

Per altre informazioni, vedere [Export a Report Using URL Access](../reporting-services/export-a-report-using-url-access.md).

**PDF sostituisce ActiveX per la stampa remota:** la barra degli strumenti di Visualizzatore report ora stampa tramite PDF anziché tramite i controlli ActiveX. Il nuovo Visualizzatore report è supportato dai browser più moderni, incluso Microsoft Edge. Non è più necessario scaricare controlli ActiveX. In base al browser usato e alle applicazioni e ai servizi di visualizzazione di file PDF installati, [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] aprirà una finestra di dialogo di stampa per stampare il report o verrà richiesto all'utente di scaricare un file PDF. Un amministratore può comunque disabilitare la stampa lato client da [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].

Per altre informazioni, vedere [Abilitare e disabilitare la stampa sul lato client per Reporting Services](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).

![Screenshot della finestra di dialogo Stampa.](../reporting-services/media/ssrs-pdf-printing.png)

### <a name="subscription-improvements"></a>Miglioramenti alle sottoscrizioni  

|Funzionalità|Modalità server supportata|  
|-------------|---------------------------|  
|**Abilitare e disabilitare le sottoscrizioni**. Sono disponibili nuove opzioni dell'interfaccia utente per disabilitare e abilitare rapidamente le sottoscrizioni. Le sottoscrizioni disabilitate mantengono le rispettive proprietà di configurazione, ad esempio la pianificazione, e possono essere abilitate con facilità.<br /><br /> ![Screenshot che mostra le opzioni Abilita, Disabilita ed Elimina.](../reporting-services/media/ssrs-enable-disable-subscriptions.png)<br /><br /> Per altre informazioni, vedere [Disable or Pause Report and Subscription Processing](../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).|Modalità nativa|  
|**Descrizione della sottoscrizione**. Quando si crea una nuova sottoscrizione, è ora possibile includere una descrizione del report come parte delle proprietà della sottoscrizione. La descrizione viene inclusa nella pagina di riepilogo della sottoscrizione.|Modalità SharePoint e nativa|  
|**Cambiare il proprietario della sottoscrizione**. L'interfaccia utente migliorata consente di cambiare rapidamente il proprietario di una sottoscrizione. Le versioni precedenti di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] consentono agli amministratori di cambiare i proprietari di una sottoscrizione usando uno script. A partire dalla versione [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] , è possibile cambiare i proprietari della sottoscrizione usando l'interfaccia utente o uno script. Il cambiamento del proprietario della sottoscrizione è un'attività amministrativa comune quando gli utenti lasciano l'organizzazione o cambiano ruolo all'interno di essa.|Modalità SharePoint e nativa|  
|**Credenziali condivise per sottoscrizioni con condivisioni file**. Sono ora disponibili due flussi di lavoro con le sottoscrizioni con condivisioni file di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] :<br /><br /> A partire da questa versione, l'amministratore di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] può configurare un singolo account di condivisione file, che può essere usato per più sottoscrizioni. L'account di condivisione file viene configurato tramite l'opzione **Specificare un account di condivisione file** in Gestione configurazione modalità nativa di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Nella pagina di configurazione della sottoscrizione gli utenti selezioneranno **Usa l'account di condivisione file**.<br /><br /> È possibile configurare singole sottoscrizioni con credenziali specifiche per la condivisione file di destinazione.<br /><br /> È anche possibile combinare i due approcci e fare in modo che alcune sottoscrizioni con condivisioni file usino l'account di condivisione file centrale, mentre altre sottoscrizioni usano credenziali specifiche.|Modalità nativa|

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)

La nuova versione di SSDT include i modelli di progetto per [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]: Creazione guidata progetto server di report e Progetto server di report. Per informazioni sul download di SSDT, vedere [SQL Server Data Tools per Visual Studio 2015](../ssdt/download-sql-server-data-tools-ssdt.md).  

### <a name="report-builder-improvements"></a>Miglioramenti di Generatore report

**Nuova interfaccia utente di Generatore report:** l'interfaccia utente di base di [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] ha ora un aspetto moderno grazie a elementi semplificati per l'interfaccia utente.  

|Nuovo|Previous|  
|-|-|  
|![ssrs_rbfacelift_new](../reporting-services/media/ssrs-rbfacelift-new.png "ssrs_rbfacelift_new")|![ssrs_rbfacelift_old](../reporting-services/media/ssrs-rbfacelift-old.png "ssrs_rbfacelift_old")|  

**Personalizzazione del riquadro Parametri:** è ora possibile personalizzare il riquadro Parametri. Usando l'area di progettazione in Generatore report, è possibile trascinare un parametro in una colonna e in una riga specifiche del riquadro Parametri. È possibile aggiungere e rimuovere colonne per modificare il layout del riquadro. Per altre informazioni, vedere [Personalizzare il riquadro dei parametri in un report &#40;Generatore report&#41;](../reporting-services/report-design/customize-the-parameters-pane-in-a-report-report-builder.md).  

![Elenco di parametri nel riquadro dei dati del report e nel riquadro dei parametri](../reporting-services/media/ssrs-customizeparameter-parameterlist-reportdatapane.png "Elenco di parametri nel riquadro dei dati del report e nel riquadro dei parametri")  

**Supporto per valori DPI elevati:** [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] supporta il ridimensionamento e i dispositivi con valori DPI elevati.  Per altre informazioni sui valori DPI alti, vedere le pagine seguenti:  

- [Windows 8.1 DPI Scaling Enhancements](https://blogs.windows.com/windowsexperience/2013/07/15/windows-8-1-dpi-scaling-enhancements/)  

- [Valori DPI alti e Windows 8.1](/previous-versions/windows/it-pro/windows-8.1-and-8/dn528848(v=win.10))  

## <a name="next-steps"></a>Passaggi successivi

[Novità di Analysis Services](/analysis-services/what-s-new-in-sql-server-analysis-services?viewFallbackFrom=sql-server-ver15)  
[Compatibilità con le versioni precedenti](reporting-services-backward-compatibility.md)  
[Funzionalità di Reporting Services supportate dalle edizioni di SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  
[Eseguire l'aggiornamento e la migrazione di Reporting Services](../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
[Reporting Services](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)