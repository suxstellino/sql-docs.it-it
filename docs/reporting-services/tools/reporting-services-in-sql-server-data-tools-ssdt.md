---
title: Reporting Services in SQL Server Data Tools (SSDT) | Microsoft Docs
description: Informazioni su come usare l'ambiente di creazione Progettazione report di SQL Server Data Tools in Microsoft Visual Studio per creare soluzioni per Reporting Services.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- Business Intelligence Development Studio, Reporting Services in
ms.assetid: 0903c7b2-ac59-45f1-b7d0-922ecd9d76f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9ed7c7d204fe315280e379206d56f122606d1f03
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597305"
---
# <a name="reporting-services-in-sql-server-data-tools-ssdt"></a>Reporting Services in SQL Server Data Tools (SSDT)

  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] è un ambiente [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] per la creazione di soluzioni di Business Intelligence. SSDT vanta l'ambiente di creazione Progettazione report in cui è possibile aprire, modificare, visualizzare in anteprima, salvare e distribuire definizioni impaginate di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , origini dati condivise, set di dati condivisi e parti di report. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] non è incluso in SQL Server. Scaricare [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md). 
  
 Questo argomento descrive le soluzioni, i progetti, i modelli di progetto e le configurazioni di [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] usati per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], nonché le visualizzazioni, le barre degli strumenti e i collegamenti che è possibile usare in Progettazione report.  
  
 Per un'introduzione alla progettazione di report, vedere [Progettare report con Progettazione report &#40;SSRS&#41;](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md).  
  
##  <a name="solutions-and-projects"></a><a name="bkmk_SolutionsandProjects"></a> Soluzioni e progetti  
 Un progetto report costituisce il contenitore per le definizioni e le risorse del report. Tutti i file inclusi nel progetto report vengono pubblicati nel server di report al momento della distribuzione del progetto. Quando si crea un progetto per la prima volta, viene creata anche una soluzione come contenitore per il progetto. A una singola soluzione è possibile aggiungere più progetti.  
  
  
##  <a name="configurations"></a><a name="bkmk_Configurations"></a> Configurazioni  
 Per creare più set di proprietà di progetto per distribuzioni diversificate, ad esempio server di report aziendali di test e produzione, utilizzare Gestione configurazione. Per altre informazioni, vedere [Distribuzione e supporto della versione in SQL Server Data Tools &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
##  <a name="report-server-projects"></a><a name="bkmk_ReportServerProjects"></a> Progetti server di report  
 Dopo l'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]sono disponibili i modelli di progetto seguenti:  
  
-   **Progetto server di report.** Quando si seleziona un progetto server di report, verrà aperto Progettazione report. Un progetto server di report è un modello di progetti Business Intelligence installato da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e disponibile nella finestra di dialogo **Nuovo progetto** . Per altre informazioni, vedere [Aggiungere un report nuovo o esistente a un progetto report &#40;SSRS&#41;](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md). Le proprietà dei progetti server di report sono applicabili a tutti i report e a tutte le origini dati condivise di un progetto di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Queste proprietà includono l'URL del server di report e i nomi di cartella per i report e le origini dati condivise. Usare la finestra di dialogo delle **pagine delle proprietà del progetto** per visualizzare i valori correnti delle proprietà. Per aprire questa finestra di dialogo, scegliere **Proprietà** dal menu **Progetto**.  
  
-   **Creazione guidata progetto server di report.** Quando si seleziona un progetto Creazione guidata server di report, verrà creato automaticamente un progetto server di report e verrà aperta la Creazione guidata report. Nella procedura guidata è possibile creare un report seguendo le istruzioni riportate in ogni pagina per creare una stringa di connessione a un'origine dati, impostare le credenziali dell'origine dati, progettare una query, aggiungere un'area dati Tabella o Matrice, specificare i dati del report e i gruppi, selezionare uno stile di carattere e colore, pubblicare il report in un server di report e visualizzare l'anteprima del report in locale. Dopo aver creato un report con la procedura guidata, è possibile modificarne i dati e la finestra di progettazione tramite Progettazione report nel progetto server di report.  
  
 ![Nuovi modelli di progetto in SSDT](/analysis-services/analysis-services/media/ssdt-biprojects.png "Nuovi modelli di progetto in SSDT")  
  
  
##  <a name="report-designer-windows-and-panes"></a><a name="bkmk_ReportDesignerWindowsandPanes"></a> Finestre e riquadri Progettazione report  
 Progettazione report supporta due visualizzazioni: **Progettazione** per definire i dati e il layout del report e **Anteprima** per mostrare una visualizzazione del report di cui è stato eseguito il rendering. In ogni visualizzazione è possibile aprire più finestre per progettare o visualizzare un report visualizzabile.  
  
###  <a name="report-data-pane"></a><a name="bkmk_ReportDataPane"></a> Riquadro Dati report  
 Nel riquadro dei dati del report vengono visualizzati campi predefiniti, origini dati, set di dati, raccolte di campi, parametri di report e immagini.  
  
 Utilizzare il riquadro dei dati del report per visualizzare quanto segue:  
  
-   **Campi predefiniti** Informazioni predefinite sul report, ad esempio il nome o l'ora in cui è stato elaborato.  
  
-   **Origini dati** Un'origine dati rappresenta un nome e una connessione a un'origine dati.  
  
-   **Set di dati** Ogni set di dati include una query che consente di specificare i dati da recuperare dall'origine dati. Espandere il set di dati per visualizzare la raccolta di campi specificati dalla query del set di dati.  
  
     In alcune finestre Progettazione query per i set di dati multidimensionali, è possibile specificare i filtri nel riquadro Filtri e indicare se creare i parametri del report. Se si specifica l'opzione relativa ai parametri del report, viene automaticamente creato un set di dati speciale per popolare l'elenco dei valori validi del parametro.  Per impostazione predefinita, questo set di dati non viene visualizzato nel riquadro dei dati del report. Per altre informazioni, vedere [Visualizzazione di set di dati nascosti per i valori dei parametri di dati multidimensionali &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/show-hidden-datasets-for-parameter-values-multidimensional-data.md).  
  
-   **Parametri report** L'elenco dei parametri del report. Se una query del set di dati include parametri di query, è possibile creare i parametri manualmente o automaticamente.  
  
-   **Immagini** L'elenco delle immagini disponibili da includere come elemento del report Immagine in un report.  
  
 Le origini dati e i set di dati nel riquadro dei dati del report rappresentano gli elementi inclusi nella definizione del report. Il riquadro dei dati del report è una funzionalità supportata da più ambienti di creazione di report. In Generatore report, è l'unico riquadro disponibile per la gestione delle origini dati e dei set di dati. In Progettazione report il riquadro dei dati del report funziona con Esplora soluzioni in cui le origini dati condivise e i set di dati condivisi vengono elencati come file. Le origini dati condivise e i set di dati condivisi nel riquadro dei dati del report devono puntare alle origini dati condivise e ai set di dati condivisi corrispondenti di Esplora soluzioni. Gli elementi del riquadro dei dati del report contengono un riferimento ai file di dati di Esplora soluzioni. Le proprietà del progetto determinano se le origini dati condivise e i set di dati condivisi vengono distribuiti nel server di report o nel sito di SharePoint. Per altre informazioni, vedere [Conversione di origini dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/convert-data-sources-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  Se non è possibile visualizzare il riquadro dei dati del report, fare clic nell'area di progettazione e scegliere **Dati report** dal menu **Visualizza**. Se il riquadro dei dati del report è mobile, è possibile ancorarlo. Per altre informazioni, vedere [Ancorare il riquadro dei dati del report in Progettazione report &#40;SSRS&#41;](../../reporting-services/tools/dock-the-report-data-pane-in-report-designer-ssrs.md).  
  
  
###  <a name="grouping-pane"></a><a name="bkmk_GroupingPane"></a> Riquadro di raggruppamento  
 Utilizzare il riquadro di raggruppamento per definire i gruppi per un'area dati Tablix. È possibile definire gruppi di righe e gruppi di dettagli per le tabelle, nonché gruppi di righe e colonne per le matrici. Non è possibile utilizzare il riquadro di raggruppamento per definire gruppi per grafici o altre aree dati. Per altre informazioni, vedere [Informazioni sui gruppi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
 Il riquadro di raggruppamento presenta due modalità:  
  
-   **Valore predefinito.** Usare la modalità **Predefinita** per visualizzare tutti i gruppi di righe e di colonne in un formato gerarchico che consente di visualizzare la relazione esistente tra gruppi padre, gruppi figlio, gruppi adiacenti e gruppi di dettagli. Un gruppo figlio viene visualizzato al di sotto e al livello di rientro successivo rispetto al relativo gruppo padre. Un gruppo adiacente viene visualizzato allo stesso livello di rientro dei gruppi di pari livello.  
  
     Utilizzare la modalità predefinita per aggiungere, modificare o eliminare gruppi. Per i gruppi basati su un singolo campo del set di dati, trascinare il campo nel riquadro Gruppi di righe o Gruppi di colonne. È possibile inserire il gruppo sopra o sotto un gruppo esistente. Per aggiungere un gruppo adiacente, fare clic con il pulsante destro del mouse sul gruppo di pari livello, quindi utilizzare il menu di scelta rapida. Per visualizzare le celle della Tablix appartenenti a un gruppo, selezionare il gruppo nel riquadro di raggruppamento.  
  
-   **Avanzata** Usare la modalità **Avanzata** per visualizzare membri di gruppi di righe e di colonne statici e dinamici dell'area dati Tablix selezionata.  È necessario utilizzare i membri dei gruppi per impostare le proprietà che consentono di controllare la visibilità delle righe e delle colonne associate a un gruppo o a un membro di un gruppo oppure le regole utilizzate dai renderer per tentare di tenere insieme i gruppi in una pagina. I membri dei gruppi vengono visualizzati nell'area di progettazione come celle nelle aree dei gruppi di righe e di colonne.  
  
> [!NOTE]  
>  Per passare dalla modalità **Predefinita** alla modalità **Avanzata** (e viceversa), fare clic con il pulsante destro del mouse sulla freccia GIÙ a destra dell'icona **Gruppi di colonne** .  
  
 Per altre informazioni, vedere [Raggruppamento date](../../reporting-services/tools/grouping-pane.md).  
  
  
###  <a name="toolbox"></a><a name="bkmk_Toolbox"></a> Casella degli strumenti  
 La casella degli strumenti contiene elementi del report che è possibile trascinare nell'area di progettazione. Le aree dati sono elementi del report che è possibile utilizzare per organizzare i dati nel report. Tabelle, matrici, elenchi, grafici, misuratori, barre dei dati, grafici sparkline e indicatori sono aree dati. Altri elementi del report sono Mappa, Casella di testo, Rettangolo, Riga, Immagine e Sottoreport. In questo elenco possono anche essere inclusi elementi del report personalizzati, se sono stati installati e registrati dall'amministratore del sistema.  
  
###  <a name="properties-pane"></a><a name="bkmk_PropertiesPane"></a> Riquadro delle proprietà  
 Il riquadro Proprietà è una finestra standard di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] in cui vengono visualizzati i nomi e i valori delle proprietà relative all'elemento del report attualmente selezionato nell'area di progettazione. Nella maggior parte dei casi i nomi delle proprietà corrispondono agli elementi e agli attributi contenuti nel file RDL (Report Definition Language). Le proprietà utilizzate più di frequente possono essere impostate utilizzando la finestra di dialogo Proprietà relativa all'elemento selezionato. Per aprire la finestra di dialogo corrispondente, fare clic sul pulsante **Pagine delle proprietà** sulla barra degli strumenti del riquadro Proprietà. Gli utenti esperti possono impostare i valori delle proprietà direttamente nel riquadro Proprietà.  
  
 Utilizzare il riquadro Proprietà per:  
  
-   Impostare le proprietà per l'elemento attualmente selezionato nell'area di progettazione. Per alcune proprietà è disponibile un elenco a discesa di valori. È inoltre possibile digitare il valore direttamente nella cella. Alcune proprietà contengono una raccolta di valori, indicata dal valore **(Raccolta)** . La maggior parte delle proprietà accetta un'espressione. Le espressioni complesse sono indicate dal valore **\<Expression>** . Fare clic su **\<Expression>** per aprire la finestra di dialogo **Espressione**. Per altre informazioni, vedere [Finestra di dialogo Espressione](/previous-versions/sql/).  
  
-   Utilizzare i pulsanti della barra degli strumenti del riquadro Proprietà per modificare la modalità di visualizzazione della griglia passando dalla visualizzazione per categorie alla visualizzazione in ordine alfabetico. In visualizzazione categorie può essere necessario espandere una categoria per visualizzare tutte le proprietà sottostanti. Per aprire la finestra di dialogo Proprietà di un elemento, fare clic sul pulsante delle **pagine delle proprietà** sulla barra degli strumenti oppure fare clic con il pulsante destro del mouse sull'elemento e scegliere **Proprietà**.  
  
-   Impostare le proprietà per il membro del gruppo attualmente selezionato nel riquadro di raggruppamento. Le proprietà dei membri del gruppo consentono di controllare in che modo le righe dell'intestazione e del piè di pagina di un gruppo statico si ripetono per ogni istanza di un gruppo. Per altre informazioni, vedere [Visualizzare intestazioni e piè di pagina con un gruppo &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md).  
  
 Per visualizzare il riquadro Proprietà, scegliere **Finestra Proprietà** dal menu **Visualizza**. È possibile annullare l'ancoraggio a questo riquadro e spostarlo in un'altra area della finestra di [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]oppure aprirlo come visualizzazione a schede nell'area di progettazione.  
  
  
###  <a name="solution-explorer"></a><a name="bkmk_SolutionExplorer"></a> Esplora soluzioni  
 Esplora soluzioni è un componente standard di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] in cui vengono visualizzati tutti gli elementi del progetto. Per un progetto server di report, questo componente include cartelle per organizzare origini dati condivise, set di dati condivisi, report e risorse. Gli elementi della cartella vengono automaticamente disposti in ordine alfabetico quando si apre il file di soluzione. Per visualizzare le proprietà dell'elemento nel riquadro Proprietà, selezionare questo elemento.  
  
###  <a name="output"></a><a name="bkmk_Output"></a> Output  
 Nella finestra di output vengono visualizzati gli errori di elaborazione che si verificano durante la visualizzazione dell'anteprima di un report e gli errori di pubblicazione che si verificano quando si distribuisce un report o un'origine dati condivisa.  
  
 Utilizzare la finestre di output e la finestra Struttura documento per eseguire il debug degli errori contenuti nelle espressioni.  
  
  
###  <a name="document-outline"></a><a name="bkmk_DocumentOutline"></a> Struttura documento.  
 Nella finestra Struttura documento viene visualizzato un elenco gerarchico di tutti gli elementi del report inclusi nella definizione del report. Per aprire il riquadro Struttura documento, scegliere **Altre finestre** dal menu **Visualizza** e quindi fare clic sulla **finestra del documento**.  
  
 Utilizzare il riquadro Struttura documento per identificare le caselle di testo e altri elementi del report in base al nome. Quando si seleziona un elemento in Struttura documento, questo elemento viene anche selezionato nell'area di progettazione.  
  
###  <a name="task-list"></a><a name="bkmk_TaskList"></a> Elenco attività  
 Nella finestra Elenco attività vengono visualizzati gli errori di compilazione relativi alle funzionalità non supportate quando si importa un report da un'altra applicazione, ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access.  
  
  
##  <a name="report-designer-design-view"></a><a name="bkmk_ReportDesignerDesignView"></a> Visualizzazione Progettazione di Progettazione report  
 Per impostazione predefinita, quando si crea un progetto server di report, Progettazione report viene aperto in visualizzazione Progettazione e consente di visualizzare l'area di progettazione. Per impostazione predefinita, nell'area di progettazione vengono visualizzati il corpo e lo sfondo del report.  
  
 Il menu di scelta rapida sullo sfondo include le opzioni per l'aggiunta di un'intestazione e un piè di pagina, mentre dal menu Visualizza è possibile visualizzare un righello e il riquadro di raggruppamento.  
  
 Utilizzare il controllo Zoom per aumentare o ridurre l'ingrandimento del report.  
  
 Per progettare un report, trascinare gli elementi del report dalla casella degli strumenti all'area di progettazione, configurare quindi le proprietà di questi elementi e modificarne la disposizione nel report.  
  
  
##  <a name="report-designer-preview"></a><a name="bkmk_ReportDesignerPreview"></a> Anteprima di Progettazione report  
 Utilizzare Anteprima per eseguire il report e aprire il report visualizzabile nel visualizzatore di report. Con l'anteprima i dati del report vengono memorizzati nella cache in locale. È anche possibile impostare le proprietà di configurazione in modo da eseguire il report in modalità debug, tramite un browser.  
  
 Quando si visualizza l'anteprima di un report, Progettazione report si connette alle origini dati del report, esegue le query del set di dati, memorizza i dati nella cache del computer locale, elabora il report per combinare dati e layout ed esegue il rendering del report. È possibile visualizzare il report nella scheda Anteprima oppure configurare le proprietà del progetto per visualizzarlo in modalità debug e aprirlo direttamente in un browser.  
  
-   **Visualizzazione dell'anteprima dei report con parametri.** Quando si visualizza l'anteprima di un report, quest'ultimo viene automaticamente elaborato se tutti i relativi parametri includono valori predefiniti validi. Se uno o più parametri del report non includono un valore predefinito valido, è necessario scegliere un valore per ogni parametro non assegnato e quindi fare clic su **Visualizza report** sulla barra degli strumenti del report.  
  
-   **Informazioni sulla cache di dati locale** Quando si visualizza l'anteprima di un report, il componente Elaborazione report esegue tutte le query per i set di dati del report usando i valori predefiniti correnti dei parametri, quindi salva i risultati come file della cache di dati locale (con estensione rdl). È possibile continuare a progettare il report senza incorrere nell'overhead associato a un nuovo recupero dei dati, se non vengono apportate modifiche alle query del set di dati o ai parametri del report.  
  
-   **Visualizzazione dell'anteprima del report tramite Gestione configurazione e Debug.** In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]le proprietà del progetto consentono di definire come distribuire i report ed eseguirne il debug. Queste proprietà si applicano a tutti i report e a tutte le origini dati condivise del progetto. Per impostare le proprietà del progetto, scegliere **Proprietà** dal menu **Progetto**. Utilizzare queste impostazioni per testare i report e pubblicarli nel server di report.  
  
-   **Monitoraggio del riquadro di output per individuare i messaggi di errore.** Quando si visualizza l'anteprima di un report e viene rilevato un problema, il componente Elaborazione report scrive i messaggi di errore nel riquadro di output.  
  
  
##  <a name="report-designer-menus"></a><a name="bkmk_ReportDesignerMenus"></a> Menu di Progettazione report  
 Quando in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]è attivo un progetto di Progettazione report, alla barra degli strumenti principale vengono aggiunte le barre degli strumenti seguenti. I menu di Progettazione report sono visibili solo in visualizzazione Progettazione.  
  
###  <a name="format-menu"></a><a name="FormatMenu"></a> Menu Formato  
 Quando si seleziona un elemento nell'area di progettazione, il menu **Formato** contiene le opzioni seguenti:  
  
-   **Colore primo piano** Selezionare un colore per il testo. Il colore predefinito è il nero.  
  
-   **Colore di sfondo** Selezionare un colore di sfondo per le caselle di testo e le aree dati.  
  
-   **Carattere** Specificare se lo stile del testo è grassetto, corsivo o sottolineato.  
  
-   **Giustifica** Specificare se il testo è allineato a destra, centrato o allineato a sinistra.  
  
-   **Allinea** Specificare il modo in cui gli oggetti selezionati sono allineati tra loro nel report.  
  
-   **Rendi uguali** Consente di regolare le dimensioni degli oggetti selezionati nel report.  
  
-   **Spaziatura orizzontale** Consente di regolare la spaziatura orizzontale tra gli oggetti selezionati nel report.  
  
-   **Spaziatura verticale** Consente di regolare la spaziatura verticale tra gli oggetti selezionati nel report.  
  
-   **Centra nel form** Consente di allineare al centro orizzontalmente e verticalmente l'oggetto selezionato rispetto alla finestra di Progettazione report.  
  
-   **Ordina** Consente di spostare sullo sfondo o in primo piano gli oggetti selezionati.  
  
###  <a name="report-menu"></a><a name="ReportMenu"></a> Menu Report  
 Quando l'area di progettazione report ha lo stato attivo, il menu **Report** contiene le opzioni seguenti:  
  
-   **Proprietà report** Selezionare questa opzione per aprire la finestra di dialogo **Proprietà report** , in cui è possibile assegnare proprietà generali del report come il nome dell'autore e la spaziatura della griglia, nonché proprietà per il layout come il numero di colonne e le dimensioni pagina. È inoltre possibile includere codice personalizzato, riferimenti ad assembly e classi e i nomi di elementi di output dei dati, trasformazioni dati e schemi dati.  
  
-   **Visualizzazione** Consente di passare tra le due schede di Progettazione report: Progettazione e Anteprima.  
  
-   **Intestazione pagina** Consente di aggiungere o eliminare un'intestazione di pagina nel report. L'eliminazione di un'intestazione di pagina implica la rimozione di tutti gli elementi al suo interno.  
  
-   **Piè di pagina** Consente di aggiungere o eliminare un piè di pagina nel report. L'eliminazione di un piè di pagina implica la rimozione di tutti gli elementi al suo interno.  
  
-   **Riquadro di raggruppamento** Consente di visualizzare o nascondere il riquadro Raggruppamento.  
  
###  <a name="view-menu"></a><a name="ViewMenu"></a> Menu Visualizza  
 Usare il menu **Visualizza** per visualizzare finestre e barre degli strumenti di Progettazione report.  
  
-   **Elenco errori** Usare questa opzione per visualizzare gli errori rilevati durante la pubblicazione o l'anteprima di un report.  
  
-   **Output** Usare questa opzione per visualizzare gli errori rilevati durante la pubblicazione o l'elaborazione di un report oppure per ulteriori informazioni sugli errori delle espressioni quando in un report viene visualizzato il messaggio "#Errore".  
  
-   **Finestra Proprietà** Usare questa opzione per visualizzare i valori delle proprietà dell'elemento del report selezionato nell'area di progettazione. Per visualizzare le proprietà relative agli elementi nidificati del report, è necessario fare clic più volte su un elemento per scorrere la gerarchia di questo elemento e dei relativi membri nidificati. Controllare il nome dell'elemento visualizzato nella parte superiore del riquadro Proprietà per verificare a quale elemento del report appartengono le proprietà visualizzate.  
  
-   **Casella degli strumenti** Usare questa opzione per visualizzare la casella degli strumenti.  
  
-   **Altre finestre** Usare questa opzione per visualizzare il riquadro seguente:  
  
    -   **Struttura documento** Usare questa opzione per aprire una vista gerarchica degli elementi del report e le relative raccolte di caselle di testo in un report.  
  
-   **Barre degli strumenti** Usare questa opzione per visualizzare le barre degli strumenti che supportano le funzionalità di Progettazione report, tra cui **Bordi report** e **Formattazione report**. Per altre informazioni, vedere [Barre degli strumenti di Progettazione report](#bkmk_ReportDesignerToolbars).  
  
-   **Dati report** Usare questa opzione per visualizzare il riquadro dei dati del report, in cui è possibile aggiungere parametri, origini dati, set di dati e immagini del report.  
  
###  <a name="project-menu"></a><a name="ProjectMenu"></a> Menu Progetto  
 Usare il menu **Progetto** per gestire origini dati condivise e report in un progetto. Quando si aggiungono o rimuovono elementi dal progetto, la vista gerarchica degli elementi del progetto in Esplora soluzioni viene automaticamente aggiornata.  
  
-   **Aggiungi nuovo elemento** Consente di aggiungere una nuova origine dati condivisa o un nuovo report al progetto.  
  
-   **Aggiungi elemento esistente** Consente di aggiungere un'origine dati condivisa o un report esistente al progetto.  
  
-   **Importa report** Consente di importare report da un'altra applicazione, ad esempio Microsoft Access.  
  
-   **Escludi dal progetto** Consente di escludere elementi dal progetto. Con questa opzione l'elemento non viene eliminato dal file system.  
  
-   **Mostra tutti i file** Consente di visualizzare tutti i file di un progetto.  
  
-   **Aggiorna elementi della casella degli strumenti del progetto** Consente di aggiornare la cache della casella degli strumenti quando si installano nuovi elementi del report personalizzati nel progetto.  
  
-   **Proprietà** Consente di aprire la finestra di dialogo **Pagine delle proprietà** per il progetto. Per altre informazioni, vedere [Finestra di dialogo Pagine delle proprietà del progetto](../../reporting-services/tools/project-property-pages-dialog-box.md).  
  
  
##  <a name="report-designer-toolbars"></a><a name="bkmk_ReportDesignerToolbars"></a> Barre degli strumenti di Progettazione report  
 In Progettazione report sono disponibili le seguenti barre degli strumenti speciali da utilizzare per la progettazione di report:  
  
-   **Report** Consente di aggiungere un'intestazione o un piè di pagina, impostare le proprietà del report, attivare e disattivare il righello o il riquadro Raggruppamento oppure usare lo zoom per cambiare la visualizzazione del report.  
  
-   **Bordi report** Consente di impostare il colore, lo stile e lo spessore per tutte le linee e i bordi selezionati di tutti gli elementi del report selezionati.  
  
-   **Formattazione report** Consente di impostare il formato degli elementi del report selezionati. Per le caselle di testo, è possibile modificare i tipi di formattazione seguenti tramite la barra degli strumenti: proprietà del carattere e colore del testo, colore di sfondo e giustificazione del testo.  
  
-   **Layout** Consente di impostare l'ordine con cui vengono disegnati gli elementi del report e l'unione di celle in un'area dati.  
  
-   **Standard** Consente di aprire o salvare progetti, visualizzare finestre e selezionare la configurazione di debug.  
  
 Usare il menu **Visualizza** per controllare se visualizzare o meno queste barre degli strumenti. È possibile che altre barre degli strumenti di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] siano disabilitate se le relative funzionalità non sono applicabili alle funzionalità di Progettazione report.  
  

##  <a name="source-control"></a><a name="bkmk_SourceControl"></a> Controllo del codice sorgente  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] può essere integrato con i plug-in di origine. Usare le pagine Progetti e soluzioni della finestra di dialogo **Opzioni** per specificare il plug-in e configurare le proprietà.  
  
##  <a name="custom-report-templates"></a><a name="bkmk_CustomReportTemplates"></a> Modelli di report personalizzati  
 Per utilizzare report personalizzati come modelli per nuovi report, è sufficiente copiarli nella cartella ReportProject nel computer in cui è installato [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] . Per impostazione predefinita, questa cartella si trova nel percorso seguente: `<drive>:\Program Files\Microsoft Visual Studio 14.0\Common7\IDE\Private Assemblies\ProjectItems\ReportProject`. Quando si aggiunge un nuovo elemento al progetto report, il report personalizzato viene visualizzato nel riquadro dei Modelli.  
  
 È inoltre possibile aggiungere stili personalizzati alla procedura guidata del report.  
  
  
##  <a name="command-line-support-for-sql-server-data-tools"></a><a name="bkmk_CommandLineSupportForssdt"></a> Supporto della riga di comando per SQL Server Data Tools  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] è basato su [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e sull'applicazione devenv.exe sottostante. Prima di poter utilizzare queste opzioni, è necessario impostare i valori validi dei seguenti due elementi:  
  
-   Proprietà del progetto per OverwriteDataSources, TargetDataSourceFolder, TargetReportFolder e TargetServerURL.  
  
-   Almeno un set di proprietà di configurazione, ad esempio Debug o Release.  
  
 Per altre informazioni, vedere [Pubblicazione di origini dati e report](../../reporting-services/reports/publishing-data-sources-and-reports.md).  
  
 Per un progetto server di report, è possibile specificare le opzioni seguenti dalla riga di comando:  
  
-   **/deploy** Distribuisce i report tramite le proprietà del progetto specificate in un file di configurazione. Ad esempio, nel comando seguente vengono distribuiti i report specificati dal file di soluzione Reports.sln tramite le impostazioni di configurazione Release specificate nelle proprietà del progetto:  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /deploy "Release"  
    ```  
  
-   **/build** Compila il file di soluzione, ma non lo distribuisce. Ad esempio, nel comando seguente vengono compilati i report specificati dal file di soluzione Reports.sln tramite le impostazioni di configurazione Debug specificate nelle proprietà del progetto:  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /build "Debug"  
    ```  
  
-   **/out** Reindirizza l'output generato dalla compilazione di una soluzione nel file specificato. Ad esempio, il comando seguente consente di reindirizzare l'output dalla compilazione dell'esempio precedente in un file denominato mybuildlog.txt.  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /build "Debug" /out mybuildlog.txt  
    ```  
  
##  <a name="keyboard-shortcuts-in-reporting-services"></a><a name="bkmk_KeyboardShortcuts"></a> Tasti di scelta rapida di Reporting Services  
 Utilizzare i tasti di scelta rapida per:  
  
-   Controllare le finestre e le modalità di [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]:  
  
    |Descrizione|Combinazione di tasti|  
    |-----------------|---------------------|  
    |Consente di compilare il progetto selezionato|CTRL+SHIFT+B|  
    |Consente di visualizzare la finestra Proprietà|F4|  
    |Consente di visualizzare la finestra dei dati|CTRL+ALT+D|  
    |Consente di iniziare il debug|F5|  
    |Consente di spostarsi da una finestra aperta a quella successiva|F6|  
  
-   Consente di controllare gli elementi nell'area di progettazione del report  
  
    |Descrizione|Combinazione di tasti|  
    |-----------------|---------------------|  
    |Consente di spostare lo stato attivo da un elemento del report all'elemento successivo|TAB|  
    |Consente di spostare l'elemento del report selezionato|Tasti di direzione|  
    |Consentono di spostare l'elemento del report selezionato|CTRL+Tasti di direzione|  
    |Consente di aumentare o ridurre le dimensioni dell'elemento del report selezionato|CTRL+MAIUSC+Tasti di direzione|  
    |In una casella di testo, consente di spostare il cursore all'inizio del testo visualizzato|CTRL+HOME|  
    |In una casella di testo, consente di spostare il cursore alla fine del testo visualizzato|CTRL+FINE|  
    |In una casella di testo, seleziona il testo dalla posizione corrente del cursore all'inizio del testo visualizzato|MAIUSC+HOME|  
    |In una casella di testo, seleziona il testo dalla posizione corrente del cursore alla fine del testo visualizzato|MAIUSC+FINE|  
    |In una casella di testo, seleziona il testo dalla posizione corrente del cursore all'inizio dell'espressione|CTRL+MAIUSC+HOME|  
    |In una casella di testo, seleziona il testo dalla posizione corrente del cursore alla fine dell'espressione|CTRL+MAIUSC+FINE|  
    |Consente di aprire il menu di scelta rapida dell'elemento del report selezionato|MAIUSC+F10+tasto della proprietà nelle tastiere più recenti|
  
## <a name="next-steps"></a>Passaggi successivi

[Scaricare SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)
[Esplora soluzioni](../../ssms/solution/solution-explorer.md)   
[Report di Reporting Services](../../reporting-services/reports/reporting-services-reports-ssrs.md)   
[Report Definition Language](../../reporting-services/reports/report-definition-language-ssrs.md)   
[Deployment and Version Support in SQL Server Data Tools](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)