---
title: Esportare report (Generatore report) | Microsoft Docs
description: È possibile esportare un report di Reporting Services in un altro formato di file, ad esempio PowerPoint o PDF, usando Generatore report, Progettazione report o il server di report.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d82c6f45d6cd8e4e35735291433d3e054afe5481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024107"
---
# <a name="export-reports-report-builder-and-ssrs"></a>Esportare report (Generatore Report e SSRS)

  Un report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] può essere esportato in un altro formato, ad esempio PowerPoint, Image, PDF, [!INCLUDE[ofprword](../../includes/ofprword-md.md)]o [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] oppure generando un documento di servizio Atom, elencando i feed di dati conformi ad Atom disponibili dal report. È possibile esportare il report da Generatore report, Progettazione report ([!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]) o il server di report.  
  
 Esportare un report per effettuare le operazioni seguenti:  
  
-   **Usare i dati del report in un'altra applicazione.** Ad esempio, è possibile esportare il report in Excel e continuare a lavorare con i dati in questa applicazione.  
  
-   **Stampare il report in un formato diverso.** Ad esempio, è possibile esportare il report nel formato di file PDF e quindi stamparlo.  
  
-   **Salvare una copia del report come un altro tipo di file.** Ad esempio, è possibile esportare un report in Word e salvarlo, creando una copia del report.  
  
-   **Usare i dati del report come feed di dati nelle applicazioni.** Ad esempio, è possibile generare feed di dati conformi ad Atom utilizzabili da Power Pivot o Power BI e quindi usare i dati in Power Pivot, or Power BI. Per altre informazioni, vedere [Generare i feed di dati da un report](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md)  
  
-   Il rendering del report nel server di report risulta utile quando si impostano sottoscrizioni o si recapitano i report tramite posta elettronica oppure se si desidera salvare un report disponibile nel server di report. Per altre informazioni, vedere [Sottoscrizioni e recapito &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sono disponibili molte estensioni per il rendering che consentono di esportare i report in formati di file comuni. Le estensioni per il rendering supportano i formati di file con interruzioni di pagina automatiche, ad esempio Word o Excel, interruzioni di pagina manuali, ad esempio PDF o TIFF, o solo dati, ad esempio CSV o XML conformi ad Atom.  
  
 È probabile che l'impaginazione del report possa subire variazioni quando si esporta un report in un formato diverso. Quando si esegue l'anteprima di un report, si visualizza il report sottoposto a rendering dall'estensione per il rendering HTML che si basa sulle regole dell'interruzione di pagina automatica. Quando si esporta un report in un formato di file diverso, ad esempio Adobe Acrobat (PDF), l'impaginazione si basa sulle dimensioni fisiche della pagina che segue le regole dell'interruzione di pagina manuale. Le pagine possono anche essere separate da interruzioni di pagina logiche aggiunte a un report, ma la lunghezza effettiva delle pagine varia in base al tipo di renderer utilizzato. Per modificare l'impaginazione del report, è necessario comprendere il comportamento dell'impaginazione dell'estensione per il rendering scelto. Potrebbe essere necessario modificare la progettazione del layout del report per questa estensione del rendering. Per altre informazioni, vedere [Layout e rendering della pagina](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a name="to-export-a-report-from-report-builder"></a><a name="bkmk_export_from_rb"></a> Per esportare un report da Generatore report

1.  Eseguire o visualizzare in anteprima il report.  
  
2.  Sulla barra multifunzione fare clic su **Esporta**.  
  
     ![Esporta in Generatore report](../../reporting-services/report-builder/media/ssrb-export.png "Esporta in Generatore report")  
  
3.  Selezionare il formato che si desidera usare.  
  
     Verrà visualizzata la finestra di dialogo **Salva con nome** . Per impostazione predefinita, il nome del file è quello del report esportato. Se lo si desidera, tale nome può essere modificato.  
  
##  <a name="to-export-a-report-from-the-ssrsnoversion-web-portal"></a><a name="bkmk_export_from_rm"></a> Per esportare un report dal portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
  
1.  Dalla pagina [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Home **del portale Web di** passare al report da esportare.  
  
2.  Fare clic sul report per eseguirne il rendering e visualizzarlo in anteprima.  
  
3.  Sulla barra degli strumenti del Visualizzatore report fare clic sulla freccia a discesa **Esporta** .  
  
     ![Esporta nel portale Web di Reporting Services](../../reporting-services/report-builder/media/ssrsportal-export.png "Esporta nel portale Web di Reporting Services")  
  
4.  Selezionare il formato che si desidera usare.  
  
5.  Fare clic su **Esporta**. Verrà visualizzata una finestra di dialogo che chiede se si vuole aprire o salvare il file.  
  
6.  Per visualizzare il report nel formato di esportazione selezionato, fare clic su **Apri**.  
  
     \- - oppure -  
  
     Per salvare immediatamente il report nel formato di esportazione selezionato, fare clic su **Salva**.  
  
     Il report verrà visualizzato o salvato con l'applicazione associata al formato scelto. Se si fa clic su **Salva**, verrà richiesto di specificare un percorso in cui salvare il report.  
  
##  <a name="to-export-a-report-from-a-sharepoint-library"></a><a name="bkmk_export_from_sharepoint"></a> Per esportare un report da una raccolta di SharePoint  
  
1.  Visualizzare l'anteprima del report.  
  
2.  Nella barra degli strumenti, fare clic su **Azioni**, scegliere **Esporta**, quindi selezionare il formato che si desidera usare.  
  
     Viene visualizzata la finestra di dialogo **Download file** .  
  
3.  Per visualizzare il report nel formato di esportazione selezionato, fare clic su **Apri**.  
  
     \- - oppure -  
  
     Per salvare immediatamente il report nel formato di esportazione selezionato, fare clic su **Salva**.  
  
     Il report verrà visualizzato o salvato con l'applicazione associata al formato scelto. Se si fa clic su **Salva**, verrà richiesto di specificare un percorso in cui salvare il report.  
  
     Se lo si desidera, modificare il nome del file del report esportato.  
  
     **Nota** Se non è possibile aprire il report nel formato selezionato perché non è stato associato alcun programma a questo tipo di file, verrà richiesto di salvare il report esportato o di individuare un programma online con cui aprirlo.  
  
##  <a name="rendering-extension-types"></a><a name="RendererTypes"></a> Tipi di estensioni per il rendering  
 Sono disponibili tre tipi di estensioni per il rendering [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   **Estensioni per il renderer di dati** Le estensioni per il rendering di dati rimuovono tutte le informazioni di formattazione e layout dal report e visualizzano solo i dati. Il file risultante può essere usato per importare i dati del report non elaborati in un altro tipo di file, ad esempio Excel, in un altro database, in un messaggio di dati XML o in un'applicazione personalizzata. I renderer di dati non supportano le interruzioni di pagina.  
  
     Sono supportate le estensioni seguenti per il rendering di dati: CSV, XML e Atom.  
  
-   **Estensioni per il renderer interruzione di pagina automatica** Le estensioni per il rendering di interruzioni di pagina automatiche mantengono il layout e la formattazione del report. Il file risultante è ottimizzato per la visualizzazione su schermo e il recapito, ad esempio in una pagina Web o nei controlli **ReportViewer** .  
  
     Sono supportate le estensioni per il rendering di interruzioni di pagina software seguenti: [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word e Archivio Web (MHTML).  
  
-   **Estensioni per il renderer interruzione di pagina manuale** Le estensioni per il rendering di interruzioni di pagina manuali mantengono il layout e la formattazione del report. Il file risultante è ottimizzato per garantire una stampa coerente o per la visualizzazione del report online in formato libro.  
  
     Sono supportate le estensioni per il rendering di interruzione di pagina manuale seguenti: TIFF e PDF.  
  
##  <a name="formats-you-can-export-while-viewing-reports"></a><a name="ExportFormats"></a> Formati che è possibile esportare durante la visualizzazione dei report  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sono disponibili estensioni per il rendering per l'esecuzione del rendering dei report in formati diversi. È consigliabile ottimizzare la progettazione del report per il formato di file scelto.  La tabella seguente elenca i formati che è possibile esportare dall'interfaccia utente.  Esistono altri formati che è possibile usare con le sottoscrizioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o se si esporta dall'accesso con URL.  Vedere la sezione [Altri modi di esportare report](#OtherWaysExportingReports)in questo argomento.  
  
|Format|Tipi di estensione per il rendering|Descrizione|  
|------------|------------------------------|-----------------|  
|File Acrobat (PDF)|Interruzione di pagina manuale|L'estensione per il rendering PDF consente di eseguire il rendering di un report in file che possono essere aperti in Adobe Acrobat e in altri visualizzatori PDF di terze parti che supportano il formato PDF 1.3. Anche se PDF 1.3 è compatibile con Adobe Acrobat 4.0 e versioni successive, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] supporta Adobe Acrobat 6 e versioni successive. Non è necessaria l'applicazione Adobe per convertire i report mediante l'estensione per il rendering. I visualizzatori PDF, ad esempio Adobe Acrobat, sono tuttavia necessari per visualizzare o stampare un report in formato PDF.<br /><br /> Per altre informazioni, vedere [Esportazione in un file PDF](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md).|  
|Atom|Data|L'estensione per il rendering Atom genera feed di dati conformi ad Atom dai report. I feed di dati sono leggibili e interscambiabili con applicazioni come Power Pivot o Power BI in grado di usare i feed di dati conformi ad Atom.<br /><br /> L'output è un documento di servizio Atom in cui sono elencati i feed di dati disponibili in un report. Per ogni area dati di un report viene creato almeno un feed di dati. A seconda del tipo di area dati e dei dati in essa contenuti, potrebbero essere generati più feed di dati.<br /><br /> Per altre informazioni, vedere [Generazione di feed di dati dai report](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).|  
|CSV|Data|L'estensione per il rendering CSV (Comma-Separated Value) consente di eseguire il rendering di report come rappresentazione bidimensionale dei dati di un report in un formato di testo normale standardizzato, facilmente leggibile e interscambiabile con numerose applicazioni.<br /><br /> Per altre informazioni, vedere [Esportazione in un file CSV](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md).|  
|EXCELOPENXML|Interruzione di pagina automatica|Visualizzato come "Excel" nel menu di esportazione durante la revisione dei report. L'estensione per il rendering di Excel consente di eseguire il rendering di un report in formato Excel (con estensione xlsx) compatibile con [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2013.  Per altre informazioni, vedere [Esportazione in Microsoft Excel](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md).|  
|PowerPoint|Interruzione di pagina manuale|L'estensione per il rendering di PowerPoint consente di eseguire il rendering di un report in formato PowerPoint (con estensione pptx) compatibile con PowerPoint 2013.|  
|File TIFF|Interruzione di pagina manuale|L'estensione per il rendering delle immagini genera bitmap o metafile dei report. Per impostazione predefinita, l'estensione per il rendering delle immagini crea un file TIFF del report, che può essere visualizzato in più pagine. Nel client l'immagine può essere visualizzata in un visualizzatore di immagini e stampata.<br /><br /> L'estensione per il rendering delle immagini permette di generare file in tutti i formati supportati da [!INCLUDE[ndptecgdiplus](../../includes/ndptecgdiplus-md.md)]: BMP, EMF, EMFPlus, GIF, JPEG, PNG e TIFF.<br /><br /> Per altre informazioni, vedere [Esportazione in un file di immagine](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md).|  
|Archivio Web|Interruzione di pagina automatica|L'estensione per il rendering HTML consente di eseguire il rendering di un report in formato HTML. Può inoltre generare pagine HTML complete o frammenti di HTML da incorporare in altre pagine HTML. Tutto il codice HTML viene generato con la codifica UTF-8.<br /><br /> L'estensione per il rendering HTML è l'estensione per il rendering predefinita per i report visualizzati in anteprima in Generatore report e visualizzati in un browser, inclusi quelli eseguiti nel portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .<br /><br /> Per altre informazioni, vedere [Rendering in formato HTML](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md).|  
|WORDOPENXML|Interruzione di pagina automatica|Visualizzato come "Word" nel menu di esportazione durante la visualizzazione dei report. L'estensione per il rendering di Word consente di eseguire il rendering di un report in formato Word (con estensione docx) compatibile con [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2013.  Per altre informazioni, vedere [Esportazione in Microsoft Word](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md).|  
|XML|Data|L'estensione per il rendering XML genera report in formato XML. Lo schema per il report XML è specifico del report e contiene solo dati. Il rendering delle informazioni di layout non viene eseguito e la paginazione non viene mantenuta dall'estensione per il rendering XML. Il codice XML generato da questa estensione può essere importato in un database, usato come messaggio di dati XML o inviato a un'applicazione personalizzata.<br/><br/> Per altre informazioni, vedere [Esportazione in XML](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md).|  
  
##  <a name="generating-data-feeds-from-a-report"></a><a name="GeneratingDataFeedsFromReport"></a> Generazione di feed di dati da un report  
 Per generare feed di dati da un report, eseguire il report nel portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e quindi fare clic sull'icona **Generazione di feed di dati** sulla barra degli strumenti del portale Web. Verrà chiesto se si desidera salvare o aprire il file. Se si sceglie **Apri**, il documento di servizio Atom verrà aperto nell'applicazione associata all'estensione di file atomsvc. Se si sceglie **Salva**, il documento verrà salvato come file con estensione atomsvc. Per impostazione predefinita, il nome del file corrisponde al nome del report. È possibile modificare il nome per renderlo più descrittivo.  
  
 Salvare il documento di servizio Atom nel computer. In un secondo momento sarà possibile caricarlo in un server di report o in un altro server per renderlo disponibile ad altri utenti. Per altre informazioni, vedere [Generazione di feed di dati dai report ](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md) e [Generare i feed di dati da un report](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
##  <a name="troubleshooting-exported-reports"></a><a name="Troubleshooting"></a> Risoluzione dei problemi relativi ai report esportati  
 Dopo averli esportati in un formato diverso, i report possono presentare un aspetto diverso o non funzionare nel modo desiderato. Questo dipende dal fatto che al renderer potrebbero applicarsi determinate regole e limitazioni. È possibile superare molte limitazioni semplicemente tenendole presenti durante la creazione del report. Potrebbe essere necessario usare un layout leggermente diverso nel report, allineare con cura gli elementi all'interno del report, limitare i piè di pagina del report a una sola riga di testo e così via.  
  
 Se il report include testo Unicode con numeri arabi o include date in arabo, il rendering di date e numeri non sarà eseguito correttamente quando si esporta il report in uno dei formati seguenti o si stampa il report.  
  
-   PDF  
  
-   Word  
  
-   Excel  
  
-   Image/TIFF  
  
 Se si esporta il report in HTML, il rendering di date e numeri sarà eseguito correttamente.  
  
 Negli argomenti relativi a renderer specifici vengono illustrate le modalità di rendering degli elementi e delle aree dati del report, nonché le limitazioni e le soluzioni per ogni renderer.  
  
-   [Esportazione in un file CSV &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)  
  
-   [Esportazione in Microsoft Excel &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md)  
  
-   [Esportazione in Microsoft Word &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md)  
  
-   [Rendering in formato HTML &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)  
  
-   [Esportazione in un file PDF &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md)  
  
-   [Esportazione in un file di immagine &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md)  
  
-   [Esportazione in XML &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md)  
  
-   [Generazione di feed di dati dai report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] rende disponibili caratteristiche aggiuntive che consentono di creare report utilizzabili in modo ottimale in altri formati. Le interruzioni di pagina nelle aree dati Tablix (tabella, matrice ed elenco), nei gruppi e nei rettangoli consentono di controllare meglio la paginazione del report. Le pagine del report, delimitate da interruzioni di pagina, possono disporre di nomi diversi ed è possibile anche reimpostarne la numerazione. Tramite le espressioni, i nomi e i numeri delle pagine possono essere aggiornati dinamicamente in fase di esecuzione del report. Per altre informazioni, vedere [Paginazione in Reporting Services](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
 È inoltre possibile utilizzare l'elemento globale predefinito RenderFormat per applicare in modo condizionale differenti layout del report per renderer diversi. Per altre informazioni, vedere [Riferimenti alle raccolte predefinite Globals e Users](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md)

##  <a name="other-ways-of-exporting-reports"></a><a name="OtherWaysExportingReports"></a> Altri modi di esportare report  
 L'esportazione di un report è un'attività su richiesta che viene eseguita con il report aperto nel portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o in Generatore report. Se si desidera automatizzare un'operazione di esportazione, ad esempio per esportare un report in una cartella condivisa con un tipo di file specifico su base periodica, sarà necessario creare una sottoscrizione per il recapito del report nella cartella condivisa. Per ulteriori informazioni, vedere [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
 Per i report visualizzati in anteprima negli strumenti per la creazione di report o aperti in un browser, ad esempio il portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , il rendering viene sempre eseguito in formato HTML. Non è possibile specificare un'estensione di rendering diversa come impostazione predefinita per la visualizzazione. È tuttavia possibile creare una sottoscrizione che generi un report nel formato di rendering desiderato per il successivo recapito nella Posta in arrivo o in una cartella condivisa. Per altre informazioni, vedere [Creare e gestire sottoscrizioni per server di report in modalità nativa](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) e [Creare, modificare ed eliminare le sottoscrizioni guidate dai dati](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md).  
  
 È inoltre possibile accedere a un report tramite un URL che specifica un'estensione per il rendering come parametro URL ed eseguire il rendering direttamente nel formato specificato senza prima eseguirlo in HTML. Nell'esempio seguente viene eseguito il rendering di un report nel formato Excel:  
  
```  
https://<Report Server Name>/reportserver?/Sales/YearlySalesSummary&rs:Format=Excel&rs:Command=Render  
```  
  
 e quanto segue esegue il rendering di un report di PowerPoint da un'istanza denominata:  
  
```  
https://<Report Server Name/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Per altre informazioni, vedere [Export a Report Using URL Access](../../reporting-services/export-a-report-using-url-access.md).  

## <a name="next-steps"></a>Passaggi successivi

[Controllo di interruzioni di pagina, intestazioni, colonne e righe &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
[Ricerca, visualizzazione e gestione dei report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
[Stampa di report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
[Salvataggio di report &#40;Generatore report&#41;](../../reporting-services/report-builder/saving-reports-report-builder.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
