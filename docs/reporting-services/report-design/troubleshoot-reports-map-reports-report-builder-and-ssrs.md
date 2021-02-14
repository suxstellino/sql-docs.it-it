---
title: 'Risoluzione dei problemi dei report: Report mappa (Generatore report) | Microsoft Docs'
description: Idee per la risoluzione dei problemi in situazioni che possono verificarsi quando si esegue la personalizzazione o la pubblicazione di un report con una mappa o si aggiunge un livello a tale report in Generatore report.
ms.date: 07/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a690aec2-056b-40bc-8cab-c694bd2d6d62
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a4580559356869ffb52d75241219e42c8511ba3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081162"
---
# <a name="troubleshoot-reports-map-reports-report-builder-and-ssrs"></a>Risoluzione dei problemi dei report: Report mappa (Generatore report e SSRS)
  È possibile che si verifichino problemi con le mappe in un report impaginato di [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] quando si aggiunge una mappa o un livello mappa al report, quando si personalizza una mappa o un livello mappa esistente nel report, quando si visualizza in anteprima una mappa di un report o quando si pubblica un report con una mappa. Utilizzare le informazioni presenti in questo argomento per risolvere questi problemi.  
    
   ## <a name="need-more-help"></a>Per ulteriore assistenza  
   
  Soluzione:  
 * [SQL Server Reporting Services](https://stackoverflow.com/questions/tagged/reporting-services) su Stack Overflow  
 * Inserire un problema o un suggerimento in [Microsoft SQL Server UserVoice](https://feedback.azure.com/forums/908035-sql-server).  

  
##  <a name="report-definition-size-issues"></a><a name="Embedded"></a> Problemi relativi alle dimensioni della definizione del report  
 Usare questa sezione per risolvere i problemi relativi alle dimensioni della definizione del report.  
  
## <a name="how-do-i-reduce-the-report-definition-size"></a>Informazioni sulla riduzione delle dimensioni della definizione del report.  
 Un livello mappa contiene elementi della mappa creati dai dati spaziali. In alcuni casi, gli elementi della mappa vengono incorporati nella definizione del report. Questa operazione si verifica nei modi seguenti:  
  
-   Se l'origine dei dati spaziali proviene da una mappa della raccolta mappe o da un file di forma ESRI del computer locale, gli elementi della mappa vengono incorporati automaticamente nella definizione del report.  
  
     Se si pubblica un report sul server di report ed è presente un riferimento dell'origine dati spaziali a un file locale, i dati spaziali non possono essere recuperati durante l'elaborazione del report. Per evitare questo problema, i dati della mappa vengono incorporati nella definizione del report.  
  
-   Nella Creazione guidata mappa o nella Creazione guidata livello mappa, se si seleziona l'opzione per incorporare i dati spaziali, gli elementi della mappa basati sui dati spaziali vengono incorporati nel livello mappa della definizione del report.  
  
-   Nel riquadro Mappa, se si fa clic con il pulsante destro del mouse sul livello e successivamente si sceglie una delle opzioni **Incorpora dati spaziali** , gli elementi della mappa basati sui dati spaziali vengono incorporati nel livello mappa della definizione del report.  
  
-   Per rimuovere i dati incorporati basati su un file di forma ESRI da una definizione del report, è necessario effettuare le operazioni seguenti:  
  
1.  Caricare o pubblicare i file ESRI con estensione shp e dbf nel server di report.  
  
2.  Nel report, nel riquadro Mappa della visualizzazione della struttura, selezionare il livello che dispone dei dati incorporati e aprire le proprietà **Dati livello** . In **Usa dati spaziali da** selezionare **Collegamento a file di forma ESRI**, quindi passare alla cartella sul server di report che contiene i file di forma ESRI, selezionarla e fare clic su OK.  
  
3.  Salvare il report. I dati incorporati per il livello modificato sono stati rimossi dalla definizione del report.  
  
 Gli elementi della mappa di un report della raccolta mappe saranno sempre incorporati in un livello mappa.  
  
##  <a name="spatial-data-issues"></a><a name="Spatial"></a> Problemi relativi ai dati spaziali  
 Usare questa sezione per risolvere i problemi relativi ai dati spaziali.  
  
## <a name="on-the-design-surface-i-see-sample-spatial-data"></a>Nell'area di progettazione, vengono visualizzati dati spaziali di esempio  
 In fase di progettazione, l'area apposita potrebbe mostrare il messaggio sui dati spaziali di esempio per le ragioni seguenti:  
  
-   I dati spaziali provengono da un file ESRI con estensione shp, ma non è disponibile il file con estensione dbf corrispondente. I file di forma ESRI includono in genere sia un file con estensione shp con dati spaziali, sia un file di supporto con estensione dbf. Verificare che il file con estensione dbf si trovi nella stessa directory del file con estensione shp.  
  
-   I dati spaziali provengono da un set di dati e la connessione dati per la query o le credenziali correnti non sono valide.  
  
-   Il livello mappa contiene una proprietà con un'espressione. Le espressioni non sono valutate finché il report non è in esecuzione. Per vedere la mappa, è necessario visualizzare l'anteprima del report.  
  
-   I dati spaziali provengono da un set di dati che dispone di un ambito specifico. Ad esempio se una mappa è nidificata in un'area dati Tablix o nella mappa viene usato lo stesso ambito del set di dati per i dati analitici e quelli spaziali, l'ambito dei dati non viene calcolato finché il report non è in esecuzione.  
  
## <a name="when-i-set-an-offset-for-an-individual-map-element-a-cluster-of-map-elements-move"></a>Se si imposta un offset per un singolo elemento della mappa, si verifica uno spostamento di un gruppo di elementi della mappa  
 I dati spaziali definiscono gli elementi della mappa visualizzati su ogni livello mappa. Un elemento della mappa può essere basato su dati spaziali costituiti da un singolo punto, un set di punti, una singola riga, un set di righe, un singolo poligono o un set di poligoni. Ogni elemento della mappa rappresenta un'unità. Se un elemento della mappa contenente più punti viene spostato, verranno spostati tutti i punti di quell'elemento della mappa.  
  
 I dati per ogni elemento della mappa vengono determinati in base al formato dei dati spaziali provenienti dall'origine esterna. Ad esempio, se una query specifica i dati spaziali di un database di SQL Server, ogni riga del set di risultati può contenere più set di coordinate di punti, linee o poligoni. Tutti gli elementi della mappa definiti da una sola riga nel set di risultati vengono trattati come un'unità. Per variare la visualizzazione di set di coordinate specifici, è necessario effettuare una delle operazioni seguenti:  
  
-   Modificare la query per restituire i set di coordinate come righe separate nel set di risultati.  
  
-   Selezionare gli elementi della mappa da modificare e impostare le proprietà dei punti, delle linee o dei poligoni incorporati corrispondenti eseguendo l'override delle proprietà di visualizzazione predefinite per il tipo di livello corrispondente.  
  
## <a name="my-layer-that-uses-spatial-data-from-an-esri-shapefile-always-has-embedded-data"></a>Il livello dell'utente che usa i dati spaziali di un file di forma ESRI dispone sempre di dati incorporati.  
 Per assicurarsi che i report con le mappe possano essere eseguiti su un server di report, i file di forma ESRI devono essere disponibili come risorse sul server di report. Se si aggiunge un livello a una mappa e si specifica un file di forma presente nel file system locale, i dati spaziali vengono incorporati automaticamente nel report.  
  
 Per sostituire i dati incorporati con un collegamento al file di forma ESRI, è necessario caricare il file con estensione shp e il corrispondente file con estensione dbf sul server di report e modificare quindi l'origine dei dati spaziali per il livello.  
  
## <a name="i-renamed-a-data-source-or-dataset-to-a-friendly-name-and-now-no-data-appears-in-my-map"></a>Dopo l'assegnazione di un nome descrittivo a un'origine dati o a un set di dati, sulla mappa non viene più visualizzato alcun dato.  
 La definizione del report non viene aggiornata automaticamente quando si modifica manualmente il nome di qualsiasi elemento del report.  
  
 Quando si modifica il nome di un set di dati, è necessario aggiornare manualmente qualsiasi area dati o livello mappa che si riferisce a quel set di dati. Per riassociare una Tablix, un grafico o un misuratore a un set di dati, selezionare l'elemento nell'area di progettazione, aprire le proprietà dell'area dati e scegliere il nome del set di dati appropriato. Per riassociare un livello mappa a un set di dati, selezionare il livello, aprire le proprietà del livello e scegliere il nome del set di dati appropriato.  
  
## <a name="my-spatial-data-contains-nulls-and-empty-strings"></a>I dati spaziali dell'utente contengono valori Null e stringhe vuote.  
 Nei dati spaziali per l'elemento del report con mappe, i valori Null vengono impostati su zero (0) e per le stringhe vuote viene impostato un valore vuoto ("").  
  
 Per i dati spaziali che provengono da un database di SQL Server, per cambiare questo comportamento è necessario modificare la query che restituisce i dati spaziali.  
  
## <a name="my-map-exceeds-the-maximum-number-of-spatial-elements"></a>La mappa dell'utente supera il numero massimo di elementi spaziali  
 Per impostazione predefinita, una mappa può disporre di 20.000 elementi o di 1.000.000 punti. Se la mappa supera questi limiti, è possibile usare uno degli approcci seguenti:  
  
-   Rimuovere un livello.  
  
-   Ridurre la risoluzione mappa.  
  
-   Ridurre le coordinate del viewport mappa per visualizzare un'area più piccola.  
  
-   Se i dati spaziali provengono da un set di dati del report, impostare un filtro per limitare i dati del set di dati. Il filtro deve essere impostato su un campo che non sia di tipo dati spaziali.  
  
-   Se i dati spaziali provengono da un database di SQL Server, modificare la query per usare le funzioni spaziali e limitare così i dati a un'area più piccola.  
  
##  <a name="viewport-center-and-view-issues"></a><a name="Viewport"></a> Problemi relativi all'allineamento al centro e alla vista  
 Usare questa sezione per risolvere i problemi relativi alle opzioni del viewport.  
  
## <a name="i-cannot-set-the-center-and-view-on-an-embedded-map-element"></a>Impossibile impostare le opzioni di allineamento al centro e di visualizzazione per un elemento della mappa incorporato.  
 Per allineare un viewport al centro di un elemento della mappa specifico, è necessario aver associato i dati spaziali di un livello ai dati analitici.  
  
## <a name="i-set-the-map-center-and-view-in-my-report-when-i-reopen-the-report-why-isnt-the-map-view-the-same"></a>Nel report sono state impostate le opzioni di allineamento al centro e di visualizzazione della mappa e tuttavia, alla riapertura del report, la vista mappa è cambiata.  
 Se le credenziali utente necessarie per leggere i dati spaziali non sono disponibili nel report all'apertura, vengono usati dati spaziali segnaposto. A seconda delle opzioni di allineamento al centro e di zoom impostate per il viewport mappa, la vista mappa potrebbe essere allineata al centro su un livello differente.  
  
 Per ricaricare i dati spaziali e usare il centro della vista mappa salvato nel report, fare clic con il pulsante destro del mouse sul viewport mappa e quindi scegliere **Ricarica**. Dopo aver immesso le credenziali per l'origine dati spaziali, il livello carica i dati spaziali e viene ripristinata la vista mappa.  
  
## <a name="the-center-and-view-for-a-map-layer-option-does-not-work"></a>Le opzioni di allineamento al centro e di visualizzazione per un livello mappa non funzionano.  
 Quando il viewport viene impostato in modo da essere allineato al centro sui dati spaziali per un livello specifico e il centro della vista sembra non coincidere con il centro del livello, sono presenti probabilmente piccole isole o aree incluse nei dati spaziali che a causa delle dimensioni ridotte non è possibile visualizzare nel viewport. Ad esempio i dati spaziali per un paese potrebbero includere piccole isole o altri piccoli territori all'interno del territorio più grande. Il viewport usa tutti i dati spaziali per calcolare il centro per il livello.  
  
 Per ignorare i calcoli per il livello, è possibile eseguire una delle operazioni seguenti:  
  
-   Specificare un centro personalizzato per il viewport.  
  
-   Modificare il livello di zoom per il viewport al fine di eliminare le posizioni che non si desidera includere.  
  
-   Incorporare i dati spaziali nel report ed eliminare le posizioni che non si desidera includere.  
  
##  <a name="layer-issues"></a><a name="Layers"></a> Problemi relativi ai livelli  
 Usare questa sezione per risolvere i problemi relativi alle opzioni dei livelli.  
  
## <a name="i-do-not-see-one-or-more-layers-in-my-map"></a>Uno o più livelli della mappa non vengono visualizzati.  
 La visualizzazione di un livello mappa in un report dipende dalla disponibilità dei dati spaziali, dalla relazione tra i dati spaziali e quelli analitici, dal tipo di dati spaziali e dal tipo di livello corrispondente, dalle opzioni di visibilità e di trasparenza del livello, nonché dall'ordine di disegno del livello. Se i dati di un livello non vengono visualizzati, verificare gli elementi seguenti:  
  
-   **Tipo di livello e tipo di dati spaziali.** Il tipo di livello visualizza solo i dati spaziali che corrispondono al tipo di livello. Ad esempio se il tipo di livello è Punto, ma i dati spaziali sono di tipo Linea, non verrà visualizzato alcun dato.  
  
-   **Valori dei campi delle corrispondenze.** I valori dei campi specificati per correlare i dati analitici e quelli spaziali devono identificare in modo univoco ogni elemento della mappa. I campi devono avere dati dello stesso tipo. I valori dei campi devono essere identici. Per altre informazioni, vedere [Problemi relativi alla legenda, alla scala dei colori e alla scala distanza](#Legend).  
  
-   **Ordine dei livelli.** L'ordine dei livelli nel riquadro Mappa è l'ordine in base al quale i livelli vengono disegnati nel renderer del report. I dati spaziali per i livelli disegnati per primi potrebbero essere sostituiti dai dati spaziali per i livelli disegnati in un secondo momento. I livelli visualizzati all'inizio dell'elenco sono disegnati per primi. La modifica dell'ordine dei livelli nell'elenco comporta il cambiamento dell'ordine di disegno dei livelli.  
  
-   **Trasparenza.** La trasparenza può essere specificata in modo indipendente per ogni livello mappa. I valori predefiniti per la trasparenza sono diversi in base alla modalità di aggiunta di un livello. Una trasparenza pari allo 0% significa che il livello è opaco e che nessun altro dato del livello sarà visibile attraverso di esso. Affinché gli altri dati siano visibili attraverso un livello esistente, regolare il valore su una percentuale superiore che consente di ottenere l'effetto desiderato.  
  
-   **Visibilità.** La visibilità per un livello può essere **Visible**, **Hidden** o **ZoomBased**, in base al livello di zoom del viewport mappa. È possibile specificare anche l'intervallo massimo e minimo per il livello di zoom. La visibilità può essere basata su un'espressione che restituisce uno di questi valori.  
  
    > [!TIP]  
    >  È possibile attivare o disattivare la visibilità per ogni livello nel riquadro Mappa. Quando si progetta ogni livello, nascondere tutti gli altri per determinare se il problema concerne un singolo livello oppure riguarda la trasparenza tra i livelli.  
  
## <a name="i-set-a-filter-on-the-map-layer-and-it-has-no-effect"></a>Nonostante l'impostazione di un filtro sul livello mappa, non si riscontra alcun effetto.  
 Per filtrare i dati per un livello, è necessario specificare il tipo di dati nell'espressione di filtro. Verificare che sia stato specificato il tipo di dati sottostanti corretto in modo che l'equazione di filtro valuti correttamente la condizione specificata. Per altre informazioni, vedere [Esempi di equazioni di filtro &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
##  <a name="legend-color-scale-and-rule-issues"></a><a name="Legend"></a> Problemi relativi alla legenda, alla scala dei colori e alle regole  
 Usare questa sezione per risolvere i problemi relativi alle opzioni delle regole, della legenda e della scala dei colori.  
  
## <a name="how-do-i-control-the-values-in-the-map-legend"></a>Informazioni sul controllo dei valori nella legenda della mappa.  
 I valori della legenda vengono determinati in modo automatico dalle regole del tipo di elemento della mappa specificate per ogni livello mappa e dalle regole di distribuzione specificate per la legenda.  
  
 Per impostazione predefinita, tutti gli elementi generati da tutte le regole sono visualizzati nella prima legenda. I valori per tutte le regole di poligono, linea e punto per ogni livello contribuiscono all'intervallo della legenda combinato. Per visualizzare gli elementi in legende diverse, è necessario innanzitutto creare più legende e quindi, per ogni regola, specificare in quale legenda visualizzare gli elementi correlati.  
  
 Per associare una regola a una legenda specifica, aprire le proprietà della regola e nella pagina Legenda, selezionare il nome della legenda da usare. Per rimuovere elementi da una legenda, nelle opzioni della legenda, selezionare la riga vuota per il nome della legenda. Se si rinominano gli elementi della legenda nel report, è necessario associare manualmente ogni livello all'elemento della legenda appropriato.  
  
 Per controllare il titolo e il contenuto di ogni legenda, usare le proprietà della legenda per la regola. È possibile specificare il numero di divisioni da creare, modificare i calcoli che consentono di assegnare valori a ogni divisione, impostare i valori di intervallo minimo e massimo, nonché modificare il formato del testo della legenda.  
  
 Per altre informazioni, vedere [Modificare legende della mappa, scala dei colori e regole associate &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
## <a name="the-rules-that-i-set-do-not-give-the-results-that-i-expect"></a>Le regole impostate non hanno fornito i risultati previsti.  
 Le regole si applicano ai dati analitici associati agli elementi della mappa su un livello. Usare l'elenco seguente per identificare i problemi riguardanti tutte le regole relative ai colori, alle dimensioni, allo spessore e al tipo di marcatore:  
  
-   L'ordine di applicazione dello stile a ogni elemento della mappa (poligono, linea, punto) inizia con la priorità più bassa ovvero proprietà dei livelli, proprietà degli elementi della mappa per tutti gli elementi della mappa del livello, regole specificate e successivamente, per gli elementi incorporati della mappa per cui si seleziona l'opzione di sostituzione, i valori specificati. Una volta selezionata l'opzione di sostituzione per un elemento incorporato, non è più possibile applicare le regole, anche se, in un secondo momento, i valori vengono ripristinati sull'impostazione originale.  
  
-   Problemi relativi ai campi delle corrispondenze. I campi delle corrispondenze consentono l'associazione dati tra gli elementi della mappa e i dati analitici. I campi dei dati spaziali e di quelli analitici che corrispondono ai campi delle corrispondenze devono disporre dello stesso tipo di dati e dello stesso formato. Se il campo delle corrispondenze non corrisponde esattamente ai dati spaziali e a quelli analitici, la regola non produce alcun effetto. Ad esempio se il campo delle corrispondenze per i dati spaziali dispone di spazi vuoti o di punteggiatura aggiuntivi rispetto al campo delle corrispondenze per i dati analitici, non si verificherà alcuna corrispondenza.  
  
-   Per altre informazioni, vedere [Variare la visualizzazione di poligoni, linee e punti in base a regole e dati analitici &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="what-is-the-value-nan-on-the-color-scale"></a>Informazioni sul valore NaN della scala dei colori.  
 **NaN** è l'acronimo di Not a Number (diverso da un numero). Si presuppone che i valori delle scale dei colori siano numerici. Verificare le impostazioni di distribuzione e il valore del testo della legenda per le regole associate alla scala dei colori. Se sono stati creati intervalli di distribuzione personalizzati, verificare di aver specificato il limite inferiore per il primo intervallo e il limite superiore per l'ultimo intervallo.  
  
## <a name="my-color-scale-does-not-appear-when-i-run-the-report"></a>Quando si esegue il report, la scala dei colori non viene visualizzata.  
 La scala dei colori visualizza informazioni all'utente quando un livello mappa specifica le regole colore per poligoni, linee o punti per tutto il livello o per gli elementi incorporati della mappa. Se nessun elemento della mappa specifica una regola colore o se le regole colore forniscono un'indicazione tramite una legenda anziché la mappa colori, quest'ultima non viene mostrata nel report visualizzabile.  
  
 Per visualizzare la scala dei colori, specificare regole colore per un livello o un elemento incorporato della mappa. Per altre informazioni, vedere [Modificare legende della mappa, scala dei colori e regole associate &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
##  <a name="tile-issues"></a><a name="Tile"></a> Problemi relativi alle sezioni  
 Usare questa sezione per risolvere i problemi relativi alle opzioni dello sfondo a sezioni.  
  
## <a name="i-cannot-see-the-bing-maps-tile-background"></a>Lo sfondo a tessere mappa di Bing non viene visualizzato.  
 Le impostazioni seguenti riguardano la visualizzazione di uno sfondo a tessere mappa di Bing nell'anteprima locale o in un report che viene eseguito dal server di report:  
  
-   Il livello tessera mappa deve essere presente. Nella creazioni guidata della mappa o del livello, selezionare **Aggiungi sfondo Bing Maps per la vista mappa**. In questo modo viene aggiunto un livello sezione per il livello di allineamento al centro e di zoom della vista del viewport mappa corrente. È inoltre possibile aggiungere un livello sezione dalla barra degli strumenti del riquadro Mappa.  
  
-   Il sistema di coordinate della mappa per il viewport deve essere **Geografico**, non **Planare**.  
  
-   La proiezione della mappa deve essere **Mercator**.  
  
-   Per l'anteprima locale, è necessario disporre dell'accesso a Internet. Per un report che è in esecuzione dal server di report, quest'ultimo deve essere configurato per supportare lo sfondo a sezioni. Per altre informazioni, vedere [Pianificare un report mappa](plan-a-map-report-report-builder-and-ssrs.md).  
  
 Per altre informazioni sull'aggiunta di un livello sezione, vedere [Aggiungere, modificare o eliminare una mappa o un livello mappa &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
## <a name="how-do-i-control-the-text-on-a-tile-layer"></a>Informazioni sul controllo del testo su un livello sezione.  
 Entrambe le viste **Strada** e **Ibrido** includono del testo. Il testo fa parte delle sezioni che provengono dai servizi Web di Bing Maps.  
  
 Per includere un livello sezione senza testo, selezionare la vista **Aereo** .  
  
##  <a name="tooltip-and-label-issues"></a><a name="Tooltip"></a> Problemi relativi alle descrizioni comandi e alle etichette  
 Usare questa sezione per risolvere i problemi relativi alle opzioni delle etichette e delle descrizioni comandi.  
  
## <a name="i-get-an-expression-error-about-dataset-scope-when-i-set-a-label-or-tooltip-to-an-expression"></a>Quando si imposta un'etichetta o una descrizione comando su un'espressione, viene restituito un errore di espressione relativo all'ambito del set di dati.  
 Se i dati spaziali provengono da una raccolta mappe o da un file di forma ESRI, i dati associati non fanno parte di un set di dati del report. Non è possibile utilizzare la sintassi dell'espressione per un riferimento al campo del set di dati al fine di specificare questi dati per un'etichetta o una descrizione comando.  
  
 Per specificare i dati correlati ai dati spaziali che non fanno parte di un set di dati del report, è necessario usare il simbolo # seguito da un'etichetta che specifica il nome dei dati.  
  
## <a name="see-also"></a>Vedere anche  
 [Mappe &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Risoluzione dei problemi relativi a Generatore report](/previous-versions/sql/2014/reporting-services/troubleshoot-report-builder?view=sql-server-2014&preserve-view=true)  
  
  
