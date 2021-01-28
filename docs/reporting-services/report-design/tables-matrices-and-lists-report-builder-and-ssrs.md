---
title: Tabelle, matrici ed elenchi (Generatore report) | Microsoft Docs
description: Informazioni su tabelle, matrici ed elenchi e sulle aree dati Tablix che visualizzano i dati dei report impaginati organizzati in righe e colonne in Generatore report.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.tablixgroup.f1
- "10045"
- sql13.rtp.rptdesigner.tablix.visibility.f1
- "10039"
- sql13.rtp.rptdesigner.groupproperties.visibility.f1
- "10104"
- "10047"
- sql13.rtp.rptdesigner.groupproperties.advanced.f1
- "10044"
- sql13.rtp.rptdesigner.groupproperties.filters.f1
- sql13.rtp.rptdesigner.tablix.sort.f1
- sql13.rtp.rptdesigner.tablix.general.f1
- sql13.rtp.rptdesigner.groupproperties.general.f1
- sql13.rtp.rptdesigner.groupproperties.variables.f1
- "10046"
- "10101"
- sql13.rtp.rptdesigner.tablix.filter.f1
- sql13.rtp.rptdesigner.groupproperties.sort.f1
- "10042"
- "10041"
- "10102"
- "10103"
- "10043"
- sql13.rtp.rptdesigner.groupproperties.pagebreaks.f1
ms.assetid: 9dcf3fc8-bf9c-4a14-a03d-e78254aa4098
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f9276557c2c2d5d3f733d6f37d243eaee0eec68a
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534298"
---
# <a name="tables-matrices-and-lists-report-builder-and-ssrs"></a>Tabelle, matrici ed elenchi (Generatore report e SSRS)
 In [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)], tabelle, matrici ed elenchi sono *aree dati* in cui i dati del report impaginati vengono visualizzati in celle suddivise in righe e colonne. Nelle celle sono contenuti in genere dati di testo, ad esempio testo, date e numeri ma possono essere contenuti anche misuratori, grafici o elementi del report come le immagini. Di frequente, tabelle, matrici ed elenchi sono definiti collettivamente aree dati *Tablix* .  
  
 I modelli di tabelle, matrici ed elenchi vengono compilati nell'area dati Tablix che è una griglia flessibile in cui è possibile visualizzare i dati in celle. Nei modelli di tabella e matrice, le celle sono organizzate in righe e colonne. Poiché i modelli sono varianti dell'area dati Tablix generica sottostante, è possibile visualizzare i dati combinando i formati di modelli e modificando la tabella, la matrice o l'elenco per includere le caratteristiche di un'altra area dati quando si sviluppa il report. Ad esempio, se si aggiunge una tabella e si scopre che non serve, è possibile aggiungere gruppi di colonne per rendere la tabella una matrice.  
  
 Le aree dati della tabella e della matrice consentono di visualizzare le relazioni dei dati complesse includendo tabelle, matrici, elenchi, grafici e misuratori nidificati. Le tabelle e le matrici dispongono di un layout tabulare e i relativi dati provengono da un solo set di dati, creato in base a una singola origine dati. La differenza principale tra le tabelle e le matrici è che le tabelle possono includere solo gruppi di righe, mentre le matrici dispongono di gruppi di righe e di gruppi di colonne.  
  
 Gli elenchi sono un po' diversi. Supportano un layout in formato libero che può includere più tabelle peer o matrici, ognuna con dati provenienti da un set di dati diverso. Gli elenchi possono essere usati anche per form quali le fatture.  
  
 Nelle immagini seguenti vengono mostrati report semplici con una tabella, una matrice o un elenco.  
  
 ![RS_TableMatrixList](../../reporting-services/report-design/media/rs-tablematrixlist.gif "RS_TableMatrixList")  
  
 Per una rapida introduzione a tabelle, matrici ed elenchi, vedere [Esercitazione: creazione di un report tabella semplice &#40;Generatore report&#41;](../../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md), [Esercitazione: creazione di un report matrice &#40;Generatore report&#41;](../../reporting-services/tutorial-creating-a-matrix-report-report-builder.md), e [Esercitazione: creazione di un report in formato libero &#40;Generatore report&#41;](../../reporting-services/tutorial-creating-a-free-form-report-report-builder.md).  
  
> [!NOTE]  
>  È possibile pubblicare tabelle matrici ed elenchi separatamente da un report come parte del report. Altre informazioni su [Parti del report](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
##  <a name="table"></a><a name="Table"></a> Tabella  
 Usare una tabella per visualizzare i dati dettaglio, organizzare i dati in gruppi di righe o per eseguire entrambe le operazioni. Il modello Tabella contiene tre colonne con una riga di intestazione di tabella e una riga di dettaglio per i dati. Nella figura seguente viene illustrato il modello di tabella iniziale selezionato nell'area di progettazione:  
  
 ![Modello di tabella selezionato nell'area di progettazione](../../reporting-services/report-design/media/rs-tabletemplatenewselected.gif "Modello di tabella selezionato nell'area di progettazione")  
  
 È possibile raggruppare i dati per un solo campo, per più campi o scrivendo un'espressione personalizzata. Si possono creare gruppi nidificati o gruppi indipendenti, adiacenti e visualizzare valori aggregati per i dati raggruppati o aggiungere totali ai gruppi. Ad esempio, se la tabella dispone di un gruppo di righe chiamato [Category], è possibile aggiungere un subtotale per ogni gruppo nonché un totale complessivo per il report. Per migliorare l'aspetto della tabella ed evidenziare i dati desiderati, è possibile unire celle e applicare la formattazione ai dati e alle intestazioni di tabella.  
  
 È possibile nascondere inizialmente i dati di dettaglio o raggruppati e includere elementi Toggle di drill-down per consentire agli utenti di scegliere in modo interattivo la quantità di dati da visualizzare.  
  
 Per altre informazioni, vedere [Tabelle &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md).  
  
##  <a name="matrix"></a><a name="Matrix"></a> Matrice  
 Usare una matrice per visualizzare i riepiloghi dei dati aggregati raggruppati in righe e colonne, analogamente a una tabella pivot o a un report a campi incrociati. Il numero di righe e colonne per i gruppi è determinato dal numero di valori univoci per ogni gruppo di righe e colonne. Nella figura seguente viene illustrato il modello di matrice iniziale selezionato nell'area di progettazione:  
  
 ![Nuova matrice aggiunta dalla casella degli strumenti e selezionata](../../reporting-services/report-design/media/rs-matrixtemplatenewselected.gif "Nuova matrice aggiunta dalla casella degli strumenti e selezionata")  
  
 È possibile raggruppare i dati per più campi o espressioni in gruppi di righe e di colonne. In fase di esecuzione, quando si combinano i dati del report e le aree dati, le dimensioni di una matrice aumentano orizzontalmente e verticalmente nella pagina quando si aggiungono colonne per i gruppi di colonne e righe per i gruppi di righe. I valori contenuti nelle celle della matrice rappresentano valori aggregati che hanno come ambito l'intersezione dei gruppi di righe e di colonne ai quali appartiene la cella. Ad esempio, se la matrice dispone di un gruppo di righe (Category) e di due gruppi di colonne (Territory e Year) che consentono di visualizzare la somma di vendite, nel report vengono visualizzate due celle con somme di vendite per ogni valore nel gruppo Category. L'ambito delle celle sono le due intersezioni: Category e Territory e Category e Year. La matrice può includere gruppi nidificati e adiacenti. I gruppi nidificati presentano una relazione padre-figlio mentre i gruppi adiacenti una relazione di tipo peer. È possibile aggiungere subtotali per alcuni o tutti i livelli di gruppi di righe e colonne nidificati all'interno della matrice.  
  
 Per migliorare la lettura dei dati della matrice ed evidenziare i dati desiderati, è possibile unire celle o dividere orizzontalmente e verticalmente e applicare la formattazione ai dati e alle intestazioni di gruppo.  
  
 È inoltre possibile includere elementi Toggle di drill-down per nascondere inizialmente i dati dettaglio. Successivamente, l'utente potrà fare clic su tali elementi per visualizzare un numero maggiore o minore di dettagli in base alle necessità.  
  
 Per altre informazioni, vedere [Creare una matrice](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md).  
  
##  <a name="list"></a><a name="List"></a> Elenco  
 Utilizzare un elenco per creare un layout in formato libero. Non è obbligatorio usare un layout griglia ma è possibile posizionare liberamente i campi all'interno dell'elenco. È possibile usare un elenco per progettare un form per la visualizzazione di molti campi di set di dati o come contenitore per la visualizzazione di più aree dati affiancate per i dati raggruppati. Si può ad esempio definire un gruppo per un elenco, aggiungere una tabella, un grafico e un'immagine, nonché visualizzare i valori in formato tabella e grafico per ogni valore di gruppo, come si farebbe per un record di un dipendente o di un paziente.  
  
 ![Nuovo elenco aggiunto dalla casella degli strumenti e selezionato](../../reporting-services/report-design/media/rs-listtemplatenewselected.gif "Nuovo elenco aggiunto dalla casella degli strumenti e selezionato")  
  
 Per altre informazioni, vedere [Creare fatture e moduli con elenchi](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md).  
  
##  <a name="preparing-data"></a><a name="PreparingData"></a> Preparazione dei dati  
 Nelle aree dati di tabella, matrice ed elenco vengono visualizzati i dati di un set di dati. È possibile preparare i dati nella query che recupera i dati per il set di dati o impostando proprietà nella tabella, matrice o elenco.  
  
 I linguaggi di query, ad esempio [!INCLUDE[tsql](../../includes/tsql-md.md)], usati per recuperare i dati per i set di dati del report consentono di preparare i dati applicando filtri per includere solo un subset dei dati, sostituendo valori Null o spazi vuoti con costanti che rendono più leggibile il report e ordinando e raggruppando dati.  
  
 Se si sceglie di preparare i dati nell'area dati di tabella, matrice o elenco di un report, le proprietà vengono impostate per l'area dati o per le celle all'interno dell'area dati. Se si desidera filtrare od ordinare i dati, impostare le proprietà per l'area dati. Ad esempio, per ordinare i dati è necessario specificare le colonne da ordinare e la direzione dell'ordinamento. Se si desidera fornire un valore alternativo per un campo, impostare i valori del testo della cella in cui viene visualizzato il campo. Ad esempio, per visualizzare Blank quando un campo è vuoto o Null, si usa un'espressione per impostare il valore.  
  
 Per altre informazioni, vedere [Preparare i dati per la visualizzazione in un'area dati Tablix &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/preparing-data-for-display-in-a-tablix-data-region-report-builder-and-ssrs.md).  
  
##  <a name="building-and-configuring-a-table-matrix-or-list"></a><a name="BuildingConfiguringTableMatrixList"></a> Compilazione e configurazione di una tabella, una matrice o un elenco  
 Quando si aggiungono tabelle o matrici al report, è possibile usare la Creazione guidata tabella e la Creazione guidata matrice o compilarle manualmente dai modelli forniti da Generatore report e Progettazione report. Gli elenchi sono compilati manualmente dal modello di elenco.  
  
 Nella procedura guidata vengono descritti i passaggi per compilare rapidamente e configurare una tabella o una matrice. Dopo avere completato la procedura guidata o se si compilano le aree dati Tablix da zero, è possibile configurare e ridefinire ulteriormente tali aree. Le finestre di dialogo, disponibili dai menu di scelta rapida sulle aree dati, facilitano l'impostazione delle proprietà più usate per interruzioni di pagina, ripetibilità e visibilità di intestazioni e piè di pagina, opzioni di visualizzazione, filtri e ordinamento. Tuttavia nell'area dati Tablix vengono fornite numerose proprietà aggiuntive che è possibile impostare solo nel riquadro Proprietà di Generatore report. Ad esempio, se si desidera visualizzare un messaggio quando il set di dati per una tabella, matrice o elenco è vuoto, è possibile specificare il testo del messaggio nella proprietà della Tablix NoRowsMessage nel riquadro Proprietà.  
  
##  <a name="changing-between-tablix-templates"></a><a name="ChangingBetweenTablixTemplates"></a> Modifica tra modelli Tablix  
 La scelta iniziale del modello della Tablix non è vincolante. Nell'aggiungere gruppi, totali ed etichette, si potrebbe voler modificare la progettazione Tablix. Si potrebbe, ad esempio, iniziare con una tabella, quindi eliminare la riga di dettaglio e aggiungere gruppi di colonne. Per altre informazioni, vedere [Esplorazione della flessibilità di un'area dati Tablix &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/exploring-the-flexibility-of-a-tablix-data-region-report-builder-and-ssrs.md).  
  
 È possibile continuare a sviluppare una tabella, una matrice o un elenco aggiungendo le caratteristiche Tablix desiderate. Nelle caratteristiche Tablix è inclusa la visualizzazione dei dati dettaglio o di aggregazioni per i dati raggruppati in righe e colonne. È inoltre possibile creare gruppi nidificati, gruppi indipendenti o adiacenti o gruppi ricorsivi. I dati raggruppati possono essere filtrati e ordinati e si possono inoltre combinare con semplici operazioni i gruppi includendo più espressioni di raggruppamento in una definizione di gruppo.  
  
 È possibile aggiungere i totali per un gruppo o i totali complessivi per l'area dati. Si possono nascondere le righe o le colonne per semplificare un report e consentire all'utente di attivare la visualizzazione dei dati nascosti, come in un report drill-down. Per altre informazioni, vedere [Controllo della visualizzazione dell'area dati Tablix in una pagina del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md).  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Procedure  
 In questa sezione vengono elencate le procedure in cui viene illustrato dettagliatamente come usare tabelle, matrici ed elenchi nei report; come visualizzare i dati in righe e colonne, aggiungere ed eliminare colonne, unire celle e includere subtotali per i gruppi di righe e di colonne.  
  
-   [Aggiungere un gruppo dettagli &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-a-details-group-report-builder-and-ssrs.md)  
  
-   [Aggiungere un totale a un gruppo o a un'area dati Tablix &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-a-total-to-a-group-or-tablix-data-region-report-builder-and-ssrs.md)  
  
-   [Modificare un elemento in una cella &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/change-an-item-within-a-cell-report-builder-and-ssrs.md)  
  
-   [Modificare l'altezza di riga o la larghezza di colonna &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/change-row-height-or-column-width-report-builder-and-ssrs.md)  
  
-   [Inserire o eliminare una colonna &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md)  
  
-   [Inserire o eliminare una riga &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-row-report-builder-and-ssrs.md)  
  
-   [Unire le celle in un'area dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/merge-cells-in-a-data-region-report-builder-and-ssrs.md)  
  
-   [Creare un gruppo di gerarchie ricorsive &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/create-a-recursive-hierarchy-group-report-builder-and-ssrs.md)  
  
-   [Aggiunta o eliminazione di un gruppo in un'area dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md)  
  
-   [Visualizzare intestazioni e piè di pagina con un gruppo &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md)  
  
-   [Creazione di un report con rientri &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/create-a-stepped-report-report-builder-and-ssrs.md)  
  
-   [Aggiunta, spostamento o eliminazione di una tabella, una matrice o un elenco &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-move-or-delete-a-table-matrix-or-list-report-builder-and-ssrs.md)  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> Contenuto della sezione  
 Negli argomenti seguenti sono disponibili ulteriori informazioni sull'utilizzo dell'area dati Tablix.  
  
 [Area dati Tablix &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)  
 Vengono illustrati i concetti chiave correlati all'area dati Tablix, ad esempio le aree della Tablix, i dati di dettaglio e raggruppati, i gruppi di colonne e di righe, e le righe e colonne statiche e dinamiche.  
  
 [Aggiunta di dati a un'area dati Tablix &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/adding-data-to-a-tablix-data-region-report-builder-and-ssrs.md)  
 Vengono fornite informazioni dettagliate sull'aggiunta dei dati di dettaglio e raggruppati, dei subtotali e totali e delle etichette a un'area dati Tablix.  
  
 [Controllo della visualizzazione dell'area dati Tablix in una pagina del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md)  
 Vengono descritte le proprietà di un'area dati Tablix che è possibile modificare per cambiarne l'aspetto quando viene visualizzata in un report.  
  
 [Controllo delle intestazioni di riga e di colonna &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/controlling-row-and-column-headings-report-builder-and-ssrs.md)  
 Viene descritto come controllare le intestazioni di riga e di colonna quando le analisi di un'area dati di tabella, matrice o elenco si estendono orizzontalmente o verticalmente in più pagine.  
  
 [Creazione di gruppi di gerarchie ricorsive &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md)  
 Viene descritto come visualizzare i dati ricorsivi dove la relazione tra padre e figlio viene rappresentata dai campi nel set di dati.  
  
 [Informazioni sui gruppi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md)  
 Vengono illustrati i gruppi e quando vengono usati; vengono inoltre descritti i gruppi disponibili per le differenti aree dati Tablix.  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiungere filtri per set di dati, aree dati e gruppi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Aree dati annidate &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md)   
 [Collegamento di più aree dati allo stesso set di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)   
 [Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Filtro, raggruppamento e ordinamento di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Grafici &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Misuratori &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
  
