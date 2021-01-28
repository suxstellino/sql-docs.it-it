---
title: Progettare report con Progettazione report | Microsoft Docs
description: Informazioni su come usare Progettazione report in SQL Server Reporting Services per creare report impaginati con funzionalità complete e soluzioni per la creazione di report.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- Report Designer [Reporting Services], report creation
ms.assetid: 3a26dccc-6ad6-48f5-a882-f96c6c0dd405
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 500ecc4e4b0dae80faa06f8d152b99423bb45251
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597167"
---
# <a name="design-reporting-services-paginated-reports-with-report-designer-ssrs"></a>Progettare report impaginati di Reporting Services con progettazione Report (SSRS)

Usare Progettazione report per creare report e soluzioni di creazione di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] impaginati completi. Tale strumento offre un'interfaccia grafica in cui è possibile definire origini dati, set di dati, query, posizioni di layout del report per aree dati e campi, nonché caratteristiche interattive, quali l'interazione tra parametri e set di report.  

Progettazione report è una funzionalità di  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], un ambiente Microsoft Visual Studio per la creazione di soluzioni di Business Intelligence. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] non è incluso in SQL Server. Scaricare [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md). 
  
## <a name="benefits-of-report-projects"></a>Vantaggi dei progetti report  
I progetti report costituiscono il contenitore per le definizioni e le risorse del report. Utilizzare i progetti per:  
  
-   Organizzare i report e gli elementi correlati in un contenitore.  
  
-   Testare le soluzioni di report che includono report ed elementi correlati a livello locale.  
  
-   Distribuire insieme gli elementi correlati. Utilizzare le proprietà del progetto e la gestione della configurazione per la distribuzione in più ambienti.  
  
-   Mantenere un set di copie master per i report e gli elementi correlati. Dopo la distribuzione, i report pubblicati potrebbero venire accidentalmente modificati.  
  
 Usare le informazioni contenute in questo argomento per progettare report impaginati ed elementi correlati per un unico progetto di report in una soluzione di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Per altre informazioni sulle soluzioni e più progetti in SQL Server Data Tools, vedere [Reporting Services in SQL Server Data Tools](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).  

  
##  <a name="shared-data-sources"></a><a name="bkmk_SharedDataSources"></a> Origini dati condivise  
 Utilizzare [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] per definire e distribuire origini dati condivise per una soluzione di report. Le origini dati condivise possono essere distribuite indipendentemente dagli altri elementi di un progetto tramite le proprietà **OverwriteDataSources** e **TargetDataSourceFolder** . Per altre informazioni, vedere [Impostare le proprietà di distribuzione &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
 In Progettazione report per definire le origini dati utilizzate in un report è possibile utilizzare sia il riquadro dei dati del report che Esplora soluzioni. Per altre informazioni, vedere [Report Data Pane](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_ReportDataPane). Non è possibile utilizzare [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] per aprire origini dati pubblicate in un server di report o in un sito di SharePoint, ma non incluse nella soluzione di [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] . Per tale caratteristica, usare [Ambiente di creazione di Generatore report &#40;SSRS&#41;](../../reporting-services/tools/report-builder-authoring-environment-ssrs.md).  
  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] è uno strumento client. È possibile testare la soluzione di report localmente nel computer, distribuirla in un ambiente di testing per il test della soluzione server, quindi distribuirla in un ambiente di produzione. Dopo la distribuzione, verificare che le estensioni per l'elaborazione dell'origine dati e le credenziali dell'origine dati siano configurate per l'ambiente del server di report. È possibile utilizzare Gestione configurazione per gestire le proprietà per distribuzioni diverse. Per altre informazioni, vedere [Reporting Services in SQL Server Data Tools &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).  
  
 Per altre informazioni, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
   
##  <a name="shared-datasets"></a><a name="bkmk_SharedDatasets"></a> Set di dati condivisi  
 Utilizzare [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] per definire e distribuire set di dati condivisi per una soluzione di report. I set di dati condivisi possono essere distribuiti indipendentemente dagli altri elementi di un progetto tramite le proprietà **OverwriteDatasets** e **TargetDatasetFolder** . Per altre informazioni, vedere [Impostare le proprietà di distribuzione &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
 In Progettazione report per definire i set di dati condivisi utilizzati in un report è possibile utilizzare sia il riquadro dei dati del report che Esplora soluzioni. Per altre informazioni, vedere [Report Data Pane](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_ReportDataPane). Non è possibile utilizzare [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] per aprire i set di dati pubblicati direttamente da un server di report o da un sito di SharePoint. Per tale caratteristica, usare [Ambiente di creazione di Generatore report &#40;SSRS&#41;](../../reporting-services/tools/report-builder-authoring-environment-ssrs.md) in modalità Set di dati condiviso.  
  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] è uno strumento client. È possibile utilizzare le finestre Progettazione query per creare e testare i risultati delle query localmente in Anteprima. Dopo la distribuzione è possibile gestire i set di dati condivisi indipendentemente dalle origini dati condivise e dai report dai quali dipendono. Per altre informazioni, vedere [Set di dati condivisi e incorporati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md), [Strumenti di progettazione query &#40;SSRS&#41;](../../reporting-services/report-data/query-design-tools-ssrs.md) e [Gestire set di dati condivisi](../../reporting-services/report-data/manage-shared-datasets.md).  
  
##  <a name="paginated-reports"></a><a name="bkmk_Reports"></a> Report impaginati  
I report impaginati sono file archiviati in un progetto report. I report possono essere utilizzati come report autonomi, sottoreport o destinazioni per azioni drill-through dai report principali. I report possono essere distribuiti indipendentemente da altri elementi di un progetto tramite **TargetReportFolder** e altre proprietà. Per altre informazioni, vedere [Impostare le proprietà di distribuzione &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
> [!NOTE]  
>  In caso di pubblicazione in un server di report in modalità SharePoint, non è possibile testare alcune caratteristiche della soluzione di report nel progetto di Progettazione report. I riferimenti a report, sottoreport e report drill-through devono utilizzare URL completi che possono essere testati solo dopo la distribuzione del progetto report. Per altre informazioni, vedere [Esempi di URL per elementi di report pubblicati in un server di report in modalità SharePoint &#40;SSRS&#41;](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md).  
  
 È possibile aggiungere report a un progetto come indicato di seguito:  
  
-   **Aggiungere un nuovo progetto report.** Per impostazione predefinita, un report vuoto viene aperto in Progettazione report. Per altre informazioni, vedere [Aggiungere un report nuovo o esistente a un progetto report &#40;SSRS&#41;](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md).  
  
-   **Aggiungere un progetto Creazione guidata report.** Il report viene creato mediante una serie di passaggi dettagliati. La Creazione guidata report consente di semplificare la procedura di definizione dei dati e di progettazione dei report mediante una serie di passaggi che consentono di creare un report completo. È possibile aggiungere stili per personalizzare la procedura guidata in base alla propria organizzazione. Per altre informazioni, vedere [Aggiungere un report nuovo o esistente a un progetto report &#40;SSRS&#41;](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md).  
  
-   **Aggiungere un nuovo elemento di tipo Report.** In Progettazione report verrà aperto un report vuoto.  
  
-   **Aggiungere un elemento esistente.** In Progettazione report verrà visualizzata una definizione di report esistente (con estensione rdl). L'apertura di un report o di un progetto da una versione precedente di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] potrebbe determinare l'aggiornamento automatico del progetto alla versione corrente e del report allo schema corrente. Per altre informazioni, vedere [Upgrade Reports](../../reporting-services/install-windows/upgrade-reports.md).  
  
-   **Importare un [!INCLUDE[msCoName](../../includes/msconame-md.md)]report di Access.** Importare tutti i report da un file di database di Access (con estensione mdb, accdb) o da un file di progetto (con estensione adp). In Progettazione report ogni report nel file di database o di progetto viene convertito in formato RDL e salvato nel progetto report. Non tutte le funzionalità di un report di Access vengono trasferite in un file di definizione di report (con estensione rdl). Per altre informazioni, vedere [Importazione report da Microsoft Access &#40;Reporting Services&#41;](../reports/reporting-services-reports-ssrs.md) e [Caratteristiche supportate dei report di Access &#40;SSRS&#41;](../reports/reporting-services-reports-ssrs.md).  
  
    > [!NOTE]  
    >  Per utilizzare la caratteristica di importazione è necessario che Access 2002 o versione successiva sia installato nello stesso computer di Progettazione report. Per l'importazione dei report è necessario che sia disponibile l'origine dati dei report di Access.  
  
-   **Utilizzare direttamente RDL.** Al momento della scrittura in Progettazione report, il report viene salvato in formato XML come file RDL (Report Definition Language). È possibile modificare questo file in Progettazione report, in un editor di testo o con qualsiasi strumento che supporti la modifica di codice XML.  
  
     Quando si modifica l'origine della definizione di report in Progettazione report, si utilizza lo schema RDL corrente della versione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui sono stati installati gli strumenti di sviluppo. Durante la compilazione di un progetto, la versione dello schema potrebbe variare a seconda delle proprietà della distribuzione. Per altre informazioni, vedere [Distribuzione e supporto della versione in SQL Server Data Tools &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
     La modifica diretta del file RDL può determinare la mancata pubblicazione nel server di report o la mancata esecuzione del report. Come per qualsiasi file XML, assicurarsi che i caratteri specifici del linguaggio XML utilizzati negli elementi siano correttamente codificati. Quando il report viene pubblicato, il server di report utilizza lo schema per convalidare il codice XML incluso nel file RDL.  
  
     Per includere elementi che non fanno parte dello schema RDL, inserire tali elementi nell'elemento Custom. L'elemento Custom può essere letto dalle estensioni per il rendering personalizzate, ma viene ignorato dalle estensioni per il rendering incluse in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Ad esempio, è possibile utilizzare l'elemento Custom per archiviare commenti nel report.  
  
     Per altre informazioni, vedere [Report Definition Language &#40;SSRS&#41;](../../reporting-services/reports/report-definition-language-ssrs.md).  
  
##  <a name="report-parts"></a><a name="bkmk_ReportParts"></a> Parti del report  
 In Progettazione report, dopo aver creato tabelle, grafici e altri elementi impaginati del report in un progetto, è possibile pubblicarli come *parti di report* in un server di report o in un sito di SharePoint integrato con un server di report in modo da permetterne il riutilizzo in altri report. Per altre informazioni, vedere [Parti del report in Progettazione report &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md).  
  
 Le parti del report possono essere distribuite indipendentemente da altri elementi di un progetto tramite **TargetReportPartFolder** e altre proprietà. Per altre informazioni, vedere [Impostare le proprietà di distribuzione &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
##  <a name="resources"></a><a name="bkmk_Resources"></a> Risorse  
 È possibile aggiungere al progetto file correlati al report, ma non elaborati dal server di report. È ad esempio possibile aggiungere immagini per le immagini o file di forma ESRI per i dati spaziali. Per altre informazioni, vedere [Risorse](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md#bkmk_Resources).  
 
##  <a name="paginated-report-layout"></a><a name="bkmk_ReportLayout"></a> Layout del report impaginato  
 Per creare il layout del report, trascinare gli elementi e le aree dati del report dalla casella degli strumenti nell'area di progettazione e disporli. Trascinare i campi del set di dati sugli elementi dell'area di progettazione per aggiungere dati al report. Per organizzare i dati in gruppi in un'area dati Tablix, trascinare i campi del set di dati nel riquadro di raggruppamento. Poiché gli strumenti per la creazione di report costituiscono essenzialmente un modo per creare definizioni di report, l'approccio alla progettazione di report di Generatore report e Progettazione report è piuttosto simile.  
   
##  <a name="preview-a-paginated-report"></a><a name="bkmk_Preview"></a> Visualizzare in anteprima un report impaginato  
 Utilizzare **Anteprima** per verificare i dati del report e la progettazione del layout. Quando un report viene visualizzato in anteprima, il componente Elaborazione report convalida lo schema di definizione del report e la sintassi dell'espressione ed elenca i problemi nella finestra [Output](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_Output) .  
  
> [!NOTE]  
>  Quando un report viene visualizzato in anteprima, i dati per il report vengono memorizzati nella cache in un file nel computer locale. Quando lo stesso report viene visualizzato di nuovo in anteprima utilizzando la stessa query, gli stessi parametri e le stesse credenziali, in Progettazione report viene recuperata la copia memorizzata nella cache anziché rieseguire la query. Il file dei dati viene salvato come *\<reportname>* .rdl.data nella stessa directory del file di definizione del report. e non viene eliminato alla chiusura di Progettazione report.  
  
 È possibile visualizzare l'anteprima di un report nei modi seguenti:  
  
-   **Vista Anteprima.** Fare clic sulla scheda **Anteprima** . Il report viene eseguito localmente utilizzando le stesse funzionalità di elaborazione e rendering dei report offerte dal server di report. Il report visualizzato è un'immagine interattiva. È quindi possibile selezionare parametri, fare clic sui collegamenti, visualizzare la mappa documento ed espandere e comprimere le aree nascoste del report. È inoltre possibile esportare il report in uno dei formati di rendering installati.  
  
-   **Anteprima autonoma.** Consente di eseguire il report locale in un browser. Tramite una configurazione per il debug, è inoltre possibile utilizzare questa modalità per eseguire il debug degli assembly personalizzati scritti. Per eseguire un progetto in modalità debug è possibile procedere in tre modi diversi:  
  
    -   Scegliere **Avvia debug** dal menu **Debug**.  
  
    -   Nella barra degli strumenti standard di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , fare clic sul pulsante **Avvia** .  
  
    -   Premere F5.  
  
     Se si utilizza una configurazione del progetto che compila il report senza distribuirlo, il report specificato nella proprietà **StartItem** della configurazione corrente viene aperto in una finestra di anteprima separata.  
  
    > [!NOTE]  
    >  Per utilizzare la modalità debug, è necessario impostare un elemento iniziale. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto report, scegliere **Proprietà**, quindi selezionare il nome del report da visualizzare nella proprietà **StartItem**.  
  
     Se si desidera visualizzare in anteprima un report che non è l'elemento iniziale del progetto, selezionare una configurazione che compili il report senza distribuirlo, ad esempio la configurazione DebugLocal. Fare clic con il pulsante destro del mouse sul report e quindi scegliere **Esegui**. È necessario scegliere una configurazione che non preveda la distribuzione del report. In caso contrario, il report verrà pubblicato nel server di report anziché venire visualizzato in locale in una finestra di anteprima.  
  
-   **Anteprima di stampa.**  
  
     La prima volta che viene visualizzato in modalità di anteprima o nella finestra di anteprima, il report è simile a quello generato dall'estensione per il rendering HTML. L'anteprima non è in formato HTML, ma il layout e la paginazione del report sono simili a quelli dell'output HTML.  
  
     Se si passa alla modalità anteprima di stampa, è possibile visualizzare la rappresentazione del report stampato. Fare clic sul pulsante **Anteprima di stampa** sulla barra degli strumenti di anteprima. Il report verrà visualizzato come in una pagina fisica. Questa visualizzazione assomiglia all'output generato dalle estensioni per il rendering delle immagini e PDF. L'anteprima di stampa non è un'immagine né un file PDF, ma la paginazione e il layout del report sono simili a quelli dell'output in questi formati. È possibile scegliere le dimensioni dell'immagine del report, ad esempio, la larghezza della pagina.  
  
     L'anteprima di stampa consente di identificare molti problemi di rendering che si potrebbero verificare in caso di stampa del report. I comuni problemi di rendering includono:  
  
    -   Pagine vuote aggiuntive perché il report è troppo grande per adattarsi al formato della carta specificato per il report.  
  
    -   Pagine vuote aggiuntive perché il report contiene una matrice che si espande dinamicamente in modo da superare la larghezza della carta specificata.  
  
    -   Interruzioni di pagina tra i gruppi che non funzionano nel modo desiderato.  
  
    -   Intestazioni e piè di pagina non visualizzate come previsto.  
  
    -   Layout del report che deve essere modificato per essere più leggibile nel formato di stampa.  
   
##  <a name="save-and-deploy-paginated-reports"></a><a name="bkmk_SaveandDeploy"></a> Salvare e distribuire report impaginati  
 In Progettazione report è possibile salvare report e altri file di progetto in locale oppure distribuirli in un server di report o in un sito di SharePoint. È possibile distribuire origini dati condivise, set di dati condivisi, report, risorse del report e parti del report singolarmente o insieme a seconda delle proprietà di distribuzione del progetto configurate. Per altre informazioni, vedere [Configuration and Deployment Properties](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md#bkmk_ConfigurationandDeploymentProperties).  
  
 In Progettazione report è importante tenere presente che si progetta un report utilizzando lo schema di definizione del report supportato dalla versione corrente di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Quando si impostano le proprietà di distribuzione del progetto per un server di report o un sito di SharePoint specifico e si salva il report, in Progettazione report la definizione del report viene salvata nella directory di compilazione nello schema che corrisponde alla versione sul server di report di destinazione. Per creare report pubblicabili su un server di report di livello inferiore, in Progettazione report gli elementi del report non esistenti nello schema di destinazione vengono rimossi. Ciò si verifica automaticamente, senza richiesta di conferma. Quando ciò si verifica, la definizione di report originale viene mantenuta nella cartella del progetto. La definizione di report modificata che viene distribuita si trova nella cartella di compilazione.  
  
> [!NOTE]  
>  Per le espressioni di debug e gli errori di distribuzione, è necessario visualizzare la definizione di report nella cartella di compilazione. Non utilizzare **Visualizza origine**. **Visualizza origine** consente di visualizzare l'origine della definizione del report dalla cartella del progetto.  
  
 Per altre informazioni, vedere [Distribuzione e supporto della versione in SQL Server Data Tools &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
### <a name="save-a-report-locally"></a>Salvare un report in locale  
 Quando si utilizza un report o altri elementi di progetto in Progettazione report, i file vengono salvati nel computer locale o in una condivisione di un altro computer a cui si ha accesso.  
  
 Se si utilizza il software di controllo del codice sorgente, potrebbe essere necessario controllare i report nel server di controllo del codice sorgente al salvataggio del report. Per altre informazioni, vedere [Source Control](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_SourceControl).  
  
### <a name="deploy-or-publish-paginated-reports"></a>Distribuire o pubblicare report impaginati  
 In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]è possibile distribuire report o altri elementi di progetto in più versioni di server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Utilizzare le configurazioni di progetto per controllare l'aggiornamento delle definizioni di report alle versioni dello schema compatibili con i server di report di destinazione. Le proprietà controllate dalle configurazioni di progetto includono il server di report di destinazione, la cartella in cui il processo di compilazione consente di archiviare temporaneamente le definizioni di report per l'anteprima e la distribuzione, nonché i livelli di errore. Per altre informazioni, vedere [Proprietà di configurazione e distribuzione](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md#bkmk_ConfigurationandDeploymentProperties) e [Impostare le proprietà di distribuzione &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
### <a name="export-a-paginated-report-to-a-different-file-format"></a>Esportare un report impaginato in un formato di file diverso  
 I report possono essere esportati in diversi formati, dai quali dipende il funzionamento di alcune funzionalità relative all'interattività e al layout del report. Per altre informazioni sulle considerazioni relative alla progettazione per i diversi formati di output, vedere [Esportare report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
   
##  <a name="report-validation-and-error-levels"></a><a name="bkmk_ReportValidationandErrorLevels"></a> Convalida del report e livelli di errore  
 I report vengono convalidati prima dell'anteprima e durante la distribuzione. Durante la compilazione dei report si possono verificare diversi problemi. I report potrebbero ad esempio contenere stringhe quali espressioni o query incompatibili con la versione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] specificata nella configurazione del progetto.  
  
 Usare la proprietà ErrorLevel per gestire avvisi ed errori relativi alla compilazione. La proprietà ErrorLevel può contenere un valore da 0 a 4 incluso. Il valore determina quali problemi di compilazione vengono segnalati come errori e quali come avvisi. Il valore predefinito è 2. Gli avvisi e gli errori vengono scritti nella finestra [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)][Output](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_Output).  
  
 I problemi con livelli di gravità minori o uguali al valore di ErrorLevel vengono segnalati come errori; in caso contrario, vengono segnalati come avvisi.  
  
 Nella seguente tabella vengono elencati i livelli di errore.  
  
|Livello di errore|Descrizione|  
|-----------------|-----------------|  
|0|Problemi di compilazione più gravi e inevitabili che impediscono la visualizzazione in anteprima e la distribuzione di report.|  
|1|Problemi di compilazione gravi che modificano drasticamente il layout del report.|  
|2|Problemi di compilazione meno gravi che modificano sensibilmente il layout del report.|  
|3|Problemi di compilazione minori che modificano il layout del report in maniera meno significativa e quasi impercettibile.|  
|4|Utilizzato solamente per pubblicare avvisi.|  
  
 Quando si tenta di visualizzare in anteprima o di distribuire un report contenente elementi di report nuovi in [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)], tali elementi possono essere rimossi dal report. Per impostazione predefinita, la proprietà ErrorLevel della configurazione è impostata su 2. Pertanto la compilazione del report potrebbe non essere eseguita correttamente in caso di rimozione della mappa. Se tuttavia si imposta il valore della proprietà ErrorLevel su 0 o 1, la mappa viene rimossa, viene visualizzato un avviso e il processo di compilazione continua.  

## <a name="next-steps"></a>Passaggi successivi

[Scaricare SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)  
[Reporting Services in SQL Server Data Tools](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)   
[Strumenti di progettazione query](../../reporting-services/report-data/query-design-tools-ssrs.md)   
[Deployment and Version Support in SQL Server Data Tools](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)