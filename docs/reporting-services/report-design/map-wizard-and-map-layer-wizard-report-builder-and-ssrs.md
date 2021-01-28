---
title: Creazione guidata mappa e Creazione guidata livello mappa (Generatore report) | Microsoft Docs
description: Come automatizzare la creazione di una mappa, l'aggiunta di un livello mappa o la modifica delle opzioni di un livello mappa con la Creazione guidata mappa e la Creazione guidata livello mappa in Generatore report.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: reference
f1_keywords:
- sql13.rtp.rptdesigner.mapandlayerwizard.f1
- "10542"
- MICROSOFT.REPORTDESIGNER.MAPLAYER.NAME
ms.assetid: 48cbe18b-1290-4107-8a1c-ec6acd71f73b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b307966577cfd30197963263597aee7a9d06e018
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597328"
---
# <a name="map-wizard-and-map-layer-wizard-report-builder-and-ssrs"></a>Creazione guidata mappa e Creazione guidata livello mappa (Generatore report e SSRS)
 Nei report impaginati di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , la Creazione guidata mappa e la Creazione guidata livello mappa consentono di automatizzare l'attività di creazione di una mappa, di aggiunta di un livello mappa o di modifica delle opzioni di livello mappa in un livello esistente.  
  
 Prima di aggiungere una mappa a un report o un livello mappa a una mappa, è necessario disporre delle informazioni seguenti:  
  
-   **Origine dati spaziali.** Percorso o connessione a un'origine che fornisce dati spaziali, ad esempio il nome di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di un database in cui sono contenuti dati spaziali o il nome di un file di forma Environmental Systems Research Institute, Inc. (ESRI).  
  
-   **Spatial data.** Dall'origine dati spaziali, un campo che contiene set di coordinate che specificano le posizioni.  
  
-   **Dati analitici.** Dati analitici da usare per variare la visualizzazione della mappa, ad esempio le vendite annuali dei negozi.  
  
-   **Campi delle corrispondenze.** Campi delle corrispondenze che definiscono la relazione tra i dati spaziali e quelli analitici, ad esempio il nome di una regione e di una città per identificare in modo univoco ogni città.  
  
 Nelle sezioni seguenti vengono fornite informazioni sulle opzioni specificate durante l'esecuzione delle creazioni guidate relative alla mappa e al livello mappa.  
  
-   Alla prima apertura di Generatore report fare clic sull'icona della **Creazione guidata mappa** al centro dell'area di progettazione.  
  
-   Nella scheda **Inserisci** , fare clic su **Mappa**, quindi su **Creazione guidata mappa**.  
  
 Per aprire la Creazione guidata livello mappa, eseguire l'azione seguente:  
  
-   Selezionare la mappa per visualizzare il riquadro Mappa e fare clic sul pulsante **Creazione guidata nuovo livello** sulla barra degli strumenti.  
  
 Fare clic sul titolo della pagina della procedura guidata per il contenuto della guida corrispondente. Le pagine visualizzate variano a seconda delle scelte effettuate per il tipo di mappa, l'origine dei dati spaziali e l'origine dei dati analitici.  
  
1.  [Scegliere un'origine dati spaziali](#SpatialDataSource). I dati spaziali possono provenire dalla raccolta mappe, ovvero un file di forma Environmental Systems Research Institute, Inc. (ESRI) o da dati spaziali di un database relazionale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   [Informazioni sui dati spaziali](#SpatialData)  
  
    -   [Informazioni sulla raccolta mappe](#MapGallery)  
  
    -   [Informazioni su un file di forma ESRI](#Shapefile)  
  
    -   [Dove è possibile ottenere i file di forma ESRI](#GetShapefiles)  
  
    -   [Informazioni su una query spaziale di SQL Server](#SqlServerSpatial)  
  
2.  [Scegli opzioni di dati spaziali e vista mappa](#MapView). Impostare la vista e la risoluzione mappa, specificare se incorporare i dati spaziali nel report e se includere uno sfondo a tessere con le tessere mappa di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing.  
  
    -   [Informazioni sulla vista o sul viewport mappa](#Viewport)  
  
    -   [Informazioni sulla risoluzione o ottimizzazione della mappa](#Resolution)  
  
    -   [Funzione dell'incorporamento dei dati spaziali](#Embed)  
  
    -   [Informazioni su uno sfondo a sezioni di Bing Maps](#Tiles)  
  
3.  [Scegli vista mappa](#Visualization). Scegliere il tipo di mappa da creare.  
  
    -   [Differenze tra una mappa di base, una mappa a bolle e una mappa analitica](#MapType)  
  
    -   Scegli vista mappa: Poligoni  
  
    -   Scegli vista mappa: Linee  
  
    -   Scegli vista mappa: Punti  
  
4.  Scegliere una connessione a un'origine dati Scegli vista mappa: Punti. Scegliere una connessione a un'origine dei dati o crearne una a un'origine dati esterna contenente dati analitici da visualizzare sulla mappa.  
  
5.  Progettare una query. Compilare una query che specifica i dati analitici.  
  
6.  [Scegli il set di dati analitici](#AnalyticalData). Specificare un'origine dati per i dati analitici.  
  
    -   [Differenze tra dati spaziali e dati analitici](#Diff)  
  
7.  [Specifica campi delle corrispondenze per dati spaziali e analitici](#SpecifyMatchFields). Compilare una relazione tra i dati spaziali e i dati analitici in modo che l'aspetto degli elementi della mappa possa variare in base ai dati.  
  
    -   [Informazioni su un campo delle corrispondenze](#MatchFields)  
  
8.  [Scegliere combinazione di colori e visualizzazione dati](#ThemeandVisualization). Per specificare come visualizzare i dati sullo sfondo della mappa, indicare il tema della mappa, i campi da visualizzare e gli elementi da variare: colore, dimensioni e/o tipo di marcatore.  
  
    -   [Funzione del tema](#Theme)  
  
    -   [Utilizzo delle legende e delle scale in Anteprima mappe](#Legends)  
  
    -   [Informazioni sulle regole](#Rules)  
  
 Dopo aver aggiunto una mappa o un relativo livello e visualizzato in anteprima il report, è possibile modificare le opzioni della mappa e del relativo livello che sono state impostate nelle procedure guidate. Per altre informazioni, vedere [Personalizzare i dati e la visualizzazione di una mappa o di un livello mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
 Per altre informazioni sulle mappe, vedere [Mappe &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md). Per istruzioni dettagliate sull'aggiunta di una mappa a un report, vedere [Esercitazione: Report mappa &#40;Generatore report&#41;](../../reporting-services/tutorial-map-report-report-builder.md).  
  
##  <a name="choose-a-source-of-spatial-data"></a><a name="SpatialDataSource"></a> Scegliere un'origine dati spaziali  
 In questa pagina specificare l'origine dati spaziali e i dati spaziali da includere. I dati spaziali possono provenire dalla raccolta mappe, da un file di forma ESRI o da una query del set di dati che specifica i dati spaziali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclusi in un database di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o di una versione successiva.  
  
 È possibile utilizzare la stessa origine dati spaziali o una diversa per ogni livello, tuttavia è necessario specificare l'origine ogni volta che si aggiunge un livello. Se i dati spaziali provengono dalla raccolta mappe o da un file di forma ESRI, l'origine dati spaziali non è un elemento del report distinto e non viene visualizzata nel riquadro dei dati del report.  
  
###  <a name="what-is-spatial-data"></a><a name="SpatialData"></a> Informazioni sui dati spaziali  
 I dati spaziali contengono le coordinate che definiscono gli elementi geografici o geometrici. In una mappa, i dati spaziali definiscono gli *elementi della mappa*: poligoni per aree o forme, linee per route o percorsi e punti per marcatori o puntine da disegno. I dati spaziali vengono archiviati in formato binario nell'origine dati e vengono specificati come set di coordinate. Ad esempio un punto è indicato dalle coordinate X e Y (X Y), una linea da due set di coordinate [(X1 Y1), (X2 Y2)], mentre un poligono viene indicato da almeno quattro set di coordinate dove il primo e l'ultimo set sono uguali [(X1 Y1) (X3 Y3) (X2 Y2)].  
  
 Per ulteriori informazioni, vedere la documentazione relativa al tipo di dati spaziali utilizzato.  
  
###  <a name="what-is-the-map-gallery"></a><a name="MapGallery"></a> What is the map gallery?  
 La raccolta mappe contiene le mappe dei report presenti nella cartella della raccolta mappe per l'ambiente di creazione del report. Le mappe della raccolta rappresentano un modo rapido per aggiungere una mappa al report. Le mappe predefinite della raccolta vengono fornite da un provider di mappe.  
  
> [!NOTE]  
>  Questa funzionalità di mapping di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa i dati dei file di forma TIGER/Line forniti gentilmente Census Bureau degli Stati Uniti ([https://www.census.gov/](https://www.census.gov/)). I file di forma TIGER/Line sono un estratto di informazioni geografiche e cartografiche selezionate dal database Census MAF/TIGER e sono messi a disposizione gratuitamente dallo United States Census Bureau. Per altre informazioni sui file di forma TIGER/Line, vedere [TIGER/Line Shapefiles and TIGER/Line Files Technical Documentation](https://www.census.gov/programs-surveys/geography/technical-documentation/complete-technical-documentation/tiger-geo-line.html) (Documentazione tecnica dei file di forma TIGER/Line e dei file TIGER/Line). Le informazioni sui confini presenti nei file di forma TIGER/Line servono solo per la raccolta dei dati statistici e la tabulazione. La relativa rappresentazione e designazione per scopi statistici non determina un'autorità giurisdizionale oppure diritti di proprietà o titoli, né rappresentano descrizioni geografiche valide a livello legale. Census TIGER e TIGER/Line sono marchi registrati dello United States Bureau of the Census.  
  
 Per estendere la raccolta mappe, è possibile aggiungere o rimuovere report dalla directory della raccolta mappe e aggiungere cartelle per organizzare le mappe. Per altre informazioni, vedere [Mappe &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
###  <a name="what-is-an-esri-shapefile"></a><a name="Shapefile"></a> What is an ESRI shapefile?  
 Un file di forma ESRI è un set di file contenente dati conformi al formato dei dati spaziali dei file di forma Environmental Systems Research Institute, Inc. (ESRI). Il set di file include in genere il file *\<filename>* .shp che contiene i dati spaziali e un file di supporto, *\<filename>* .dbf.  
  
 Quando si specifica come origine dati spaziali un file di forma posizionato nel computer locale, i dati spaziali vengono incorporati automaticamente nel report. Per utilizzare in modo dinamico i dati spaziali di un file ESRI, è necessario eseguire le operazioni seguenti:  
  
 In Generatore report, caricare sia il file con estensione shp sia il file con estensione dbf nella stessa cartella su un server di report, quindi eseguire il collegamento al file con estensione shp come origine dati spaziali.  
  
 In Progettazione report di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aggiungere sia il file con estensione shp sia il file con estensione dbf al progetto report, quindi specificare il nome del file con estensione shp come origine dati spaziali.  
  
###  <a name="where-can-i-get-esri-shapefiles"></a><a name="GetShapefiles"></a> Dove è possibile ottenere i file di forma ESRI  
 I file di forma ESRI sono disponibili sul Web. Per altre informazioni, vedere la pagina relativa alla [ricerca di file di forma ESRI per una mappa](https://go.microsoft.com/fwlink/?linkid=178814).  
  
###  <a name="what-is-a-sql-server-spatial-query"></a><a name="SqlServerSpatial"></a> Informazioni su una query spaziale di SQL Server  
 Una query spaziale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è una query del set di dati che specifica dati che possono essere di tipo SQLGeometry o SQLGeography di un database relazionale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Se nella procedura guidata si definisce un'origine dati, nella pagina Progetta query verranno visualizzate diverse finestre Progettazione query, a seconda del tipo di origine dati a cui si è connessi. Per altre informazioni, vedere [Strumenti di progettazione query &#40;SSRS&#41;](../report-data/query-design-tools-ssrs.md).  
  
 Quando si esegue la query in Progettazione query, nel set di risultati viene visualizzata una colonna con i dati spaziali sotto forma di testo. Ad esempio una riga potrebbe contenere dati spaziali che rappresentano un singolo punto e la riga successiva dati spaziali che definiscono un set di punti. Ogni riga diventa un elemento della mappa. È possibile variare la visualizzazione di ogni elemento della mappa come un'unità indivisibile.  
  
 Per altre informazioni, vedere [Tipi di dati spaziali](../../relational-databases/spatial/spatial-data-types-overview.md).  
  
##  <a name="choose-spatial-data-and-map-view-options"></a><a name="MapView"></a> Scegli opzioni di dati spaziali e vista mappa  
 In questa pagina è possibile impostare le opzioni seguenti:  
  
-   Impostare il livello di allineamento al centro e zoom della vista per i dati spaziali selezionati nella pagina della procedura guidata precedente. La vista impostata si applica a tutta la mappa.  
  
-   Impostare la risoluzione mappa.  
  
-   Specificare se incorporare i dati spaziali nel report.  
  
-   Per i dati incorporati, specificare se includere tutti i dati o solo i dati nella vista corrente.  
  
-   Specificare se includere uno sfondo a tessere mappa di Bing di [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
###  <a name="what-is-the-map-view-or-viewport"></a><a name="Viewport"></a> Informazioni sulla vista o sul viewport mappa  
 Il viewport mappa definisce l'area della mappa da visualizzare per tutti i livelli nel report.  
  
 Per impostazione predefinita, la scala dei colori e la scala distanza vengono visualizzate all'interno del viewport, mentre la legenda della mappa all'esterno. Queste opzioni relative al viewport possono essere modificate dopo aver completato la procedura guidata.  
  
###  <a name="what-is-map-resolution-and-optimization"></a><a name="Resolution"></a> Informazioni sulla risoluzione e ottimizzazione della mappa  
 Quando si modifica la risoluzione per i dati spaziali che rappresentano linee o poligoni, si specifica il livello di dettaglio con cui si desidera che la mappa sia disegnata. Ad esempio, per le viste aeree di un'area, è necessaria una granularità fino a cento metri di superficie sulla terra oppure una risoluzione di 1,5 km è sufficiente?  
  
 Se i dati spaziali sono incorporati nel report, una risoluzione maggiore aumenta il numero di elementi necessario per disegnare i dettagli con quella risoluzione. Se i dati spaziali non sono incorporati nel report, una risoluzione maggiore aumenta il tempo richiesto dall'elaboratore di report per calcolare le linee per la mappa con quella risoluzione ogni volta che si visualizza il report.  
  
 Se si abbassa la risoluzione, l'elaboratore di report applica un algoritmo per ridurre il numero di punti necessario per disegnare gli elementi della mappa. Minore sarà la risoluzione, minore sarà il numero di dati necessario per visualizzare gli elementi della mappa; tale condizione consentirà di ottenere una migliore visualizzazione.  
  
 Quando si regola il dispositivo di scorrimento, i dati dell'anteprima nel riquadro della procedura guidata vengono aggiornati per fornire all'utente un'indicazione dell'effetto. Dopo aver aggiunto la mappa al report, è possibile regolare questo valore modificando le opzioni del viewport mappa.  
  
###  <a name="what-does-embedding-spatial-data-do"></a><a name="Embed"></a> Funzione dell'incorporamento dei dati spaziali  
 Quando si incorporano gli elementi della mappa o le tessere mappa di Bing in un report, i dati spaziali vengono archiviati nella definizione del report.  
  
 Un report con una mappa può utilizzare i dati spaziali o le tessere mappa di Bing che vengono recuperati in modo dinamico durante l'elaborazione del report o in fase di progettazione e quindi incorporati nella definizione del report. Gli elementi della mappa incorporati possono aumentare notevolmente le dimensioni della definizione del report, ma riducono il tempo necessario per visualizzare la mappa nel report. Gli elementi dinamici della mappa riducono le dimensioni della definizione del report, ma aumentano il tempo necessario per elaborare e visualizzare la mappa.  
  
 Una buona progettazione di report richiede una valutazione dei compromessi per i dati della mappa statici o dinamici e l'individuazione dell'equilibrio migliore in base alle circostanze. Generalmente, più dati indicano che la definizione del report e il report compilato richiedono uno spazio maggiore per l'archiviazione sul server di report e tempi di elaborazione più lunghi. Pertanto, è consigliabile limitare i dati spaziali, oltre agli altri dati del report, alle sole informazioni necessarie per il report.  
  
###  <a name="what-is-a-bing-map-tile-background"></a><a name="Tiles"></a> Informazioni su uno sfondo a tessere mappa di Bing  
 Per aggiungere uno sfondo con un'immagine geografica alla mappa, selezionare l'opzione relativa allo sfondo a tessere mappa di Bing. L'elaboratore di report scarica le sezioni dai servizi Web di Bing Maps per l'area della mappa e la risoluzione specificata in questa pagina della procedura guidata. È possibile specificare uno dei seguenti tipi di sezioni:  
  
-   **Strada.** Visualizza uno stile per la mappa stradale con sfondo bianco.  
  
-   **Aereo.** Visualizza solo una vista aerea. Con questa modalità non viene visualizzato alcun testo.  
  
-   **Ibrido.** Visualizza la combinazione delle viste **Strada** e **Aereo** .  
  
 Per altre informazioni sulle sezioni, vedere [Bing Maps Tiles System](/bingmaps/articles/bing-maps-tile-system). Per altre informazioni sull'utilizzo delle tessere mappa di Bing nel report, vedere [Ulteriori condizioni di utilizzo](https://go.microsoft.com/fwlink/?LinkId=151371).  
  
 Per vedere uno sfondo a sezioni in visualizzazione della struttura, è necessario poter accedere a Internet. Per vedere in anteprima lo sfondo a sezioni di un report in un server di report, quest'ultimo deve essere configurato in modo da supportare le tessere mappa di Bing. Per altre informazioni, vedere [Risoluzione dei problemi dei report: Report mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md) e [Pianificare un report mappa](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md).  
  
 Per altre informazioni su come personalizzare un livello sezione, vedere [Aggiungere, modificare o eliminare una mappa o livello mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
##  <a name="choose-map-visualization"></a><a name="Visualization"></a> Scegli vista mappa  
 In questa pagina scegliere il tipo di mappa o il livello mappa da aggiungere al report. La prima esecuzione della procedura guidata comporta l'aggiunta della mappa e del relativo primo livello al report. Una mappa può contenere più livelli. Ognuno di essi visualizza un tipo specifico di dati spaziali: poligoni, linee o punti.  
  
 Il tipo di mappa da scegliere dipende dallo scopo della mappa e dai dati a disposizione.  
  
###  <a name="what-is-the-difference-among-a-basic-map-a-bubble-map-and-an-analytical-map"></a><a name="MapType"></a> Differenza tra una mappa di base, una mappa a bolle e una mappa analitica  
 Una **Mappa di base** visualizza solo le posizioni. È possibile variare i colori delle aree sulla mappa usando le sfumature, tuttavia il colore non rappresenta i valori dei dati analitici.  
  
 Una **Mappa a bolle** fornisce il valore relativo per una singola aggregazione di dati analitici usando le dimensioni delle bolle, ad esempio per le vendite dei negozi. È possibile creare mappe a bolle per i poligoni o i punti. Per i poligoni, impostare le proprietà del punto centrale del poligono; per i punti, impostare le proprietà del marcatore.  
  
 Una **Mappa analitica** fornisce il valore relativo di una o più aggregazioni di dati analitici per ogni elemento della mappa. Ad esempio, le vendite dei negozi mediante le dimensioni del marcatore, i margini di profitto per le categorie di prodotti mediante il colore del marcatore e i prodotti più venduti mediante il tipo di marcatore.  
  
 Per altre informazioni, vedere [Pianificare un report mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md).  
  
##  <a name="choose-the-analytical-dataset"></a><a name="AnalyticalData"></a> Scegli il set di dati analitici  
 In questa pagina specificare dove acquisire i dati analitici da visualizzare su questo livello mappa.  
  
 Per visualizzare i dati del report o qualsiasi altro dato analitico sullo sfondo della mappa, è necessario specificare dove si trovano i dati e il modo in cui sono correlati ai dati spaziali. I dati possono provenire da un set di dati del report esistente o da un nuovo set di dati per cui si compila una query. I dati analitici esistenti potrebbero essere inclusi nel file di forma ESRI che contiene i dati spaziali.  
  
###  <a name="what-is-the-difference-between-spatial-data-and-analytical-data"></a><a name="Diff"></a> Differenze tra dati spaziali e dati analitici  
 I dati spaziali sono costituiti da set di coordinate che specificano punti, linee e poligoni. Gli elementi della mappa si basano sui dati spaziali.  
  
 I dati analitici sono dati numerici o categorici utilizzati per variare l'aspetto della mappa. I dati analitici possono provenire da un set di dati del report o potrebbero essere inclusi con i dati spaziali di una mappa della raccolta mappe o di un file di forma ESRI.  
  
##  <a name="specify-the-match-fields"></a><a name="SpecifyMatchFields"></a> Specifica campi delle corrispondenze  
 In questa pagina compilare una relazione tra i dati spaziali e i dati analitici.  
  
###  <a name="what-are-match-fields"></a><a name="MatchFields"></a> Informazioni sui campi delle corrispondenze  
 I campi delle corrispondenze consentono all'elaboratore di report di compilare una relazione tra i dati analitici e i dati spaziali e specificano valori univoci all'interno dei dati analitici. Il nome del negozio, ad esempio, potrebbe non essere univoco all'interno dei dati, pertanto è possibile specificare sia una città sia il nome del negozio.  
  
##  <a name="choose-color-theme-and-data-visualization"></a><a name="ThemeandVisualization"></a> Scegliere combinazione di colori e visualizzazione dati  
 In questa pagina specificare come visualizzare i dati sullo sfondo della mappa, il tema della mappa, i campi da visualizzare e gli elementi da variare, quali colore, dimensioni e/o tipo di marcatore.  
  
###  <a name="what-does-the-theme-do"></a><a name="Theme"></a> Funzione del tema  
 Il tema scelto imposta i valori predefiniti per colore, bordo e carattere. Queste opzioni possono essere modificate dopo aver completato la procedura guidata.  
  
###  <a name="what-are-the-legends-and-scales-in-map-preview-for"></a><a name="Legends"></a> Utilizzo delle legende e delle scale in Anteprima mappe  
 Le legende consentono a un utente di interpretare i dati visualizzati su una mappa. Una mappa presenta un intervallo di colori, una scala distanza e una legenda.  
  
-   **Intervallo di colori.** L'intervallo di colori visualizza una barra dei colori con una scala che fornisce un supporto per gli intervalli di dati determinati dall'elaboratore di report in base alle regole specificate per il livello.  
  
-   **Scala distanza.** La scala distanza fornisce un supporto per le unità di distanza sulla mappa. Le unità di distanza vengono determinate in modo automatico in base alla proiezione della mappa e al livello di zoom.  
  
-   **Legenda.** La legenda fornisce un supporto per l'interpretazione dei colori, delle dimensioni e dei tipi di marcatori su una mappa. Per impostazione predefinita, tutte le regole dei livelli visualizzano gli intervalli di dati nella prima legenda. È possibile personalizzare questa legenda e aggiungerne altre dopo aver aggiunto la mappa al report.  
  
###  <a name="what-are-rules"></a><a name="Rules"></a> Informazioni sulle regole  
 Le regole sono calcoli utilizzati dall'elaboratore di report per suddividere i dati analitici in intervalli. È possibile specificare regole diverse per ogni livello. Il tipo di regole che è possibile specificare dipende dal tipo di dati spaziali presenti nel livello:  
  
-   **Poligoni.** È possibile specificare regole colore.  
  
-   **Punti centrali per i poligoni.** È possibile specificare regole colore, dimensioni e tipo di marcatore.  
  
-   **Linee.** È possibile specificare regole colore e spessore.  
  
-   **Punti.** È possibile specificare regole colore, dimensioni e tipo di marcatore.  
  
 L'elaboratore di report applica le regole impostate e determina in modo automatico l'elenco di elementi da visualizzare in una legenda. Per impostazione predefinita, i risultati di tutte le regole dei livelli vengono visualizzati nella prima legenda. È possibile apportare delle modifiche dopo aver completato la procedura guidata. Per altre informazioni, vedere [Variare la visualizzazione di poligoni, linee e punti in base a regole e dati analitici &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Risolvere problemi relativi ai report: Report mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)   
 [Pianificare un report mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)   
 [Mappe &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)  
  
