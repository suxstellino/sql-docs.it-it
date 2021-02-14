---
title: Esportazione in Microsoft Word (Generatore report) | Microsoft Docs
ms.date: 12/06/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
description: L'estensione per il rendering di Word consente di eseguire il rendering di report impaginati nel formato Microsoft Word (con estensione docx). Il formato è Office Open XML.
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: 0cd8ae26-4682-4473-8f15-af084951defd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1a9fd855d1b0bc8defd0e4355f490a2824fead6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015261"
---
# <a name="exporting-to-microsoft-word-report-builder-and-ssrs"></a>Esportazione in Microsoft Word (Generatore report e SSRS)

  L'estensione per il rendering di Word consente di eseguire il rendering di report impaginati nel formato Microsoft Word (con estensione docx). Il formato è Office Open XML.  
  
 Il tipo di contenuto dei file generati da questo renderer è **application/vnd.openxmlformats-officedocument.wordprocessingml.document** e l'estensione dei file è docx.  
  
 Per informazioni dettagliate su come esportare in Word, vedere [Esportazione di report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
 Dopo l'esportazione del report in un documento di Word, è possibile modificarne il contenuto e progettare report in formato documento, ad esempio etichette di indirizzi, ordini di acquisto o lettere tipo.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="report-items-in-word"></a><a name="ReportItemsWord"></a> Elementi del report in Word  
 I report esportati in Word vengono visualizzati sotto forma di una tabella nidificata che rappresenta il corpo del report. Il rendering di un'area dati Tablix viene eseguito come tabella nidificata che riflette la struttura dell'area dati nel report, mentre quello di caselle di testo e rettangoli viene eseguito come celle all'interno della tabella. Il valore della casella di testo viene visualizzato all'interno della cella.  
  
 Il rendering di immagini, grafici, barre di dati, grafici sparkline, mappe, indicatori e misuratori viene eseguito come immagini statiche all'interno delle celle della tabella. Vengono sottoposti a rendering i collegamenti ipertestuali e i collegamenti drill-through presenti in questi elementi del report. Le mappe e le aree su cui è possibile fare clic all'interno di un grafico non sono supportate.  
  
 I report a colonne in formato newsletter non vengono sottoposti a rendering in Word. Il rendering di colori e immagini di sfondo delle pagine e del corpo del report non viene eseguito.  
  
##  <a name="pagination"></a><a name="Pagination"></a> Paginazione  
 Dopo l'apertura in Word, l'intero report viene rimpaginato in base alle dimensioni della pagina. La rimpaginazione può causare l'inserimento di interruzioni di pagina in posizioni non previste e, in alcuni casi, la presenza nel report esportato di due interruzioni di pagina consecutive in una riga o l'aggiunta di pagine vuote. È possibile tentare di modificare la paginazione di Word regolando i margini della pagina.  
  
 Questo renderer supporta solo interruzioni di pagina logiche.  
  
### <a name="page-sizing"></a>Ridimensionamento della pagina  
 Per il rendering del report, l'altezza e la larghezza della pagina in Word vengono impostate in base alle seguenti proprietà RDL: altezza e larghezza delle dimensioni della pagina, margini sinistro e destro della pagina e margini superiore e inferiore della pagina.  
  
### <a name="page-width"></a>Larghezza della pagina  
 Word supporta una larghezza di pagina massima di 56 centimetri. Se la larghezza del report è maggiore di 56 centimetri, il rendering del report verrà comunque eseguito, ma il contenuto del report non verrà visualizzato in visualizzazione Layout di stampa o Layout lettura di Word. Per visualizzare i dati, passare alla visualizzazione Normale o Layout Web. Poiché in queste visualizzazioni di Word la quantità di spazi vuoti viene ridotta, è possibile visualizzare un'area maggiore del contenuto del report.  
  
 Dopo il rendering, la larghezza del report aumenta se necessario fino a un massimo di 56 centimetri, per consentire la visualizzazione del contenuto. La larghezza minima del report è basata sulla proprietà RDL Width nel riquadro Proprietà.  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> Proprietà del documento  
 Il renderer di Word consente di scrivere i metadati seguenti nel file DOCX.  
  
|Proprietà degli elementi del report|Descrizione|  
|-------------------------------|-----------------|  
|Report Title (titolo del report)|Titolo|  
|Report.Author|Autore|  
|Report.Description|Commenti|  
  
##  <a name="page-headers-and-footers"></a><a name="ReportHeadersFooters"></a> Intestazioni di pagina e piè di pagina  
 Per le intestazioni e i piè di pagina il rendering viene eseguito come aree di intestazione e piè di pagina in Word. Se nell'intestazione o nel piè di pagina è visualizzato un numero di pagina o un'espressione che indica il numero complessivo di pagine del report, questi valori vengono convertiti in un campo di Word. In questo modo, nel report visualizzabile appare il numero di pagina preciso. L'eventuale impostazione nel report dell'altezza dell'intestazione o del piè di pagina non è supportata in Word. In alcune circostanze, la proprietà PrintOnFirstPage può specificare se il testo di un'intestazione e di un piè di pagina viene stampato nella prima pagina di un report. Se il report visualizzabile è costituito da più pagine e in ognuna di esse è presente un'unica sezione, la proprietà PrintOnFirstPage può essere impostata su False in modo che il testo venga eliminato dalla prima pagina; in caso contrario, il testo viene stampato indipendentemente dal valore della proprietà PrintOnFirstPage.  
  
 Tramite il renderer di Word viene tentata l'analisi di tutte le espressioni delle intestazioni e dei piè di pagina quando i report vengono esportati in Word. Molte forme di espressioni vengono analizzate correttamente e i valori previsti vengono visualizzati nei piè di pagina e nelle intestazioni di tutte le pagine dei report.  
  
 Tuttavia, se in un'intestazione o piè di pagina è presente un'espressione complessa mediante la quale vengono restituiti valori diversi in pagine differenti di un report, in tutte le pagine del report potrebbe essere visualizzato lo stesso valore. I numeri di pagina nelle due espressioni seguenti non vengono incrementati nel report esportato. Il numero viene convertito nello stesso valore in tutte le pagine del report.  
  
-   `="Page: " + Globals!PageNumber.ToString + " of " + Globals!TotalPages.ToString`  
  
-   `=Avg(Fields!YTDPurchase.Value, "Sales") & " Page Number " & Globals!PageNumber`  
  
 Questa situazione si verifica in quanto tramite il renderer di Word nel report viene eseguita l'analisi dei campi correlati alla paginazione, ad esempio **PageNumber** e **TotalPages** , e viene gestito solo un riferimento semplice, non le chiamate a una funzione. In questo caso, tramite l'espressione viene chiamata la funzione **ToString** . Le due espressioni seguenti sono equivalenti ed entrambe consentono di eseguire correttamente il rendering quando il report viene visualizzato in anteprima in Generatore report o Progettazione report o viene eseguito il rendering del report pubblicato in un portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o in una raccolta di SharePoint. Tuttavia, il renderer di Word consente di analizzare correttamente solo la seconda espressione ed eseguire il rendering dei numeri di pagina corretti.  
  
-   **Espressione complessa:**  l'espressione è `="Average Sales " & Avg(Fields!YTDPurchase.Value, "Sales") & " Page Number " & Globals!PageNumber`  
  
-   **Espressione con sequenze di testo:** testo, **vendite medie** ed espressione, `=Avg(Fields!YTDPurchase.Value, "Sales)` e testo, **numero di pagina** ed espressione `=Globals!PageNumber`  
  
 Per evitare questo problema, usare più sequenze di testo invece di una sola espressione complessa quando si usano espressioni nei piè di pagina e nelle intestazioni. Le due espressioni seguenti sono equivalenti. La prima è un'espressione complessa, mentre nella seconda vengono utilizzate sequenze di testo. Il renderer di Word consente di analizzare correttamente solo la seconda espressione.  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> Interattività  
 Alcuni elementi interattivi sono supportati in Word. Di seguito è riportata una descrizione di comportamenti specifici.  
  
### <a name="show-and-hide"></a>Elementi visualizzati e nascosti  
 Il renderer di Word esegue il rendering degli elementi del report in base al relativo stato corrente. Se un elemento del report ha lo stato impostato come nascosto, tale elemento non verrà sottoposto a rendering nel documento di Word. Un elemento del report il cui stato è impostato come visualizzato verrà sottoposto a rendering nel documento di Word. La funzionalità di attivazione e disattivazione della visualizzazione non è supportata in Word.  
  
### <a name="document-map"></a>Mappa documento  
 Se il report contiene etichette della mappa documento, queste verranno sottoposte a rendering come etichette di sommario di Word nei rispettivi elementi e gruppi del report. L'etichetta della mappa documento viene utilizzata come testo delle etichette di sommario. Il collegamento di destinazione viene posizionato accanto all'elemento su cui è impostata l'etichetta. Anche se nel documento di Word non viene automaticamente creato un sommario, è possibile compilarne uno utilizzando le etichette della mappa documento visualizzate nel report.  
  
### <a name="hyperlink-and-drillthrough-links"></a>Collegamenti ipertestuali e collegamenti drill-through  
 Per i collegamenti ipertestuali e i collegamenti drill-through presenti negli elementi del report di tipo casella di testo e immagine, il rendering viene eseguito come collegamenti ipertestuali nel documento di Word. Quando si fa clic sul collegamento ipertestuale, il browser Web predefinito viene aperto in corrispondenza dell'URL. Quando si fa clic sul collegamento ipertestuale drill-through, si accede al server di report di origine.  
  
### <a name="interactive-sorting"></a>Ordinamento interattivo  
 Il rendering del contenuto del report viene eseguito in base all'ordinamento corrente nell'area dati del report. L'ordinamento interattivo non è supportato in Word. Dopo il rendering del report, è possibile applicare l'ordinamento della tabella all'interno di Word.  
  
### <a name="bookmarks"></a>Segnalibri  
 Per i segnalibri del report viene eseguito il rendering come segnalibri di Word. Il rendering dei collegamenti a un segnalibro viene eseguito come collegamenti ipertestuali che consentono di accedere alle etichette di segnalibro all'interno del documento. Le etichette di segnalibro devono contenere meno di 40 caratteri. L'unico carattere speciale che è possibile utilizzare in queste etichette è il carattere di sottolineatura (_). I caratteri speciali non supportati vengono rimossi dal nome dell'etichetta di segnalibro. Il nome viene inoltre troncato se costituito da più di 40 caratteri. Se il report contiene nomi di segnalibro duplicati, il rendering dei segnalibri non verrà eseguito in Word.  
  
##  <a name="word-style-rendering"></a><a name="WordStyleRendering"></a> Rendering dello stile in Word  
 Di seguito è riportata una breve descrizione della modalità di rendering degli stili in Word.  
  
### <a name="color-palette"></a>Tavolozza dei colori  
 I colori visualizzati nel report vengono sottoposti a rendering nel documento di Word.  
  
### <a name="border"></a>Bordo  
 Per i bordi degli elementi del report, ad eccezione del bordo della pagina, viene eseguito il rendering come bordi di celle di tabella di Word.  
  
##  <a name="squiggly-lines-in-exported-reports"></a><a name="SquigglyLines"></a> Righe ondulate nei report esportati  
 Quando esportati e visualizzati in Word, costanti o dati del report potrebbero essere sottolineati con righe ondulate rosse o verdi. Con le righe ondulate rosse vengono identificati gli errori di ortografia, con quelle verdi gli errori grammaticali. Questa situazione si verifica quando nel report sono incluse parole non conformi agli strumenti di correzione (controllo ortografia e grammatica) della lingua di modifica specificata in Word. Ad esempio, i titoli in inglese di colonne del report probabilmente saranno sottolineati con righe ondulate rosse se viene eseguito il rendering del report in una versione spagnola di Word. Gli errori ortografici percepiti sono più comuni nei report rispetto a quelli grammaticali in quanto nei report sono inclusi in genere solo testi brevi, non paragrafi o frasi intere.  
  
 La presenza di righe ondulate nei report implica la presenza di errori nel report che probabilmente non esistono. È possibile rimuovere le righe ondulate cambiando la lingua degli strumenti di correzione per il report. Per cambiare la lingua degli strumenti di correzione, selezionare il contenuto del report e specificare la lingua appropriata per tale contenuto. È possibile selezionare una parte o l'intero contenuto. In Word, l'opzione relativa alla lingua, **Imposta lingua di modifica** , si trova nella scheda **Rivedere** nell'area **Lingua** . Al termine dell'aggiornamento del contenuto, è necessario salvare di nuovo il documento.  
  
 A seconda della lingua della versione di Office in uso, gli strumenti di correzione (ad esempio, il dizionario) della lingua scelta sono inclusi nel programma o forniti in un Language Pack di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office acquistato.  
  
 Negli argomenti seguenti vengono fornite ulteriori informazioni sull'impostazione delle opzioni di Word e di Office.  
  
-   Cambiare la lingua di modifica nella finestra di dialogo **Preferenze di lingua di Microsoft Office** o **Opzioni di Word** in Word. Per altre informazioni, vedere [Impostazione delle preferenze di lingua per la modifica, l'interfaccia utente o la Guida](https://office.microsoft.com/word-help/enable-the-use-of-other-languages-in-your-office-programs-HA010354783.aspx?CTT=1).  
  
-   Aggiungere i Language Pack di Office e cambiare la lingua di modifica. Per altre informazioni, vedere [Impostazione delle preferenze di lingua per la modifica, l'interfaccia utente o la Guida](https://office.microsoft.com/word-help/enable-the-use-of-other-languages-in-your-office-programs-HA010354783.aspx?CTT=1) e [Opzioni disponibili per Office](https://office.microsoft.com/language/).  
  
> [!NOTE]  
>  Quando si cambia la lingua di modifica nella finestra di dialogo **Preferenze di lingua di Microsoft Office** o **Opzioni di Word** in Word, la modifica viene applicata a tutti i programmi di Office.  
  
##  <a name="word-limitations"></a><a name="WordLimitations"></a> Limitazioni di Word  
 In [!INCLUDE[ofprword](../../includes/ofprword-md.md)]vengono applicate le limitazioni seguenti:  
  
-   Le tabelle di Word supportano un massimo di 63 colonne. Se il report contiene più di 63 colonne e si tenta di eseguirne il rendering, la tabella viene divisa in Word. Le colonne aggiuntive vengono posizionate accanto alle 63 colonne visualizzate nel corpo del report. Pertanto, è possibile che le colonne del report non siano allineate come previsto.  
  
-   Word supporta pagine di dimensioni massime pari a 56 centimetri di larghezza e 56 centimetri di altezza. Se la larghezza del contenuto è maggiore di 56 centimetri, è possibile che alcuni dati non vengano visualizzati in visualizzazione Layout di stampa.  
  
-   Le impostazioni relative all'altezza dell'intestazione e del piè di pagina vengono ignorate in Word.  
  
-   Dopo l'esportazione, il report viene rimpaginato in Word. Questo può causare l'aggiunta di altre interruzioni di pagina nel report visualizzabile.  
  
-   In Word le righe di intestazione non vengono ripetute nella seconda pagina o nelle pagine successive, anche se si imposta la proprietà RepeatOnNewPage della riga di intestazione statica di una Tablix (tabella, matrice o elenco) su **True**. È possibile definire interruzioni di pagina esplicite nel report per forzare la visualizzazione delle righe di intestazione nelle nuove pagine. Poiché tuttavia al report visualizzabile esportato in Word viene applicata la paginazione specifica di Word, i risultati potrebbero variare e la riga di intestazione potrebbe non essere ripetuta in modo prevedibile. La riga di intestazione statica è la riga contenente le intestazioni di colonna.  
  
-   Le dimensioni delle caselle di testo aumentano quando contengono spazi unificatori.  
  
-   Quando viene esportato in Word, il testo con effetti carattere in alcuni tipi di carattere può generare glifi imprevisti o mancanti nel report visualizzabile.  
  
##  <a name="benefits-of-using-the-word-renderer"></a><a name="WordBenefits"></a> Vantaggi dell'utilizzo del renderer di Word  
 Oltre a rendere disponibili per i report esportati le nuove funzionalità di [!INCLUDE[ofprword](../../includes/ofprword-md.md)] , i file con estensione *docx tendono anche a essere più piccoli. I report esportati tramite il renderer di Word sono in genere notevolmente più piccoli rispetto agli stessi report esportati utilizzando il renderer di Word 2003.  
  
## <a name="backward-compatibility-of-exported-reports"></a>Compatibilità con le versioni precedenti di report esportati  
 È possibile selezionare una modalità di compatibilità di Word e impostare opzioni di compatibilità. Il renderer di Word consente di creare documenti con la modalità di compatibilità abilitata. L'ulteriore salvataggio di documenti con tale modalità disabilitata potrebbe influire sul layout del documento.  
  
 Se si disabilita la modalità di compatibilità e si salva di nuovo un report, il relativo layout potrebbe cambiare in modo imprevisto.  
  
##  <a name="the-word-2003-renderer"></a><a name="AvailabilityWord"></a> Rendering di Word 2003  
  
> [!IMPORTANT]  
>  L'estensione doc per il rendering di [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 è deprecata. Per altre informazioni, vedere [Funzionalità deprecate di SQL Server Reporting Services in SQL Server 2016](~/reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 Il rendering di Word è compatibile con [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 grazie al [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Compatibility Pack per i formati di file Word, Excel e PowerPoint. Per altre informazioni, vedere [Microsoft Office Compatibility Pack per i formati di file Word, Excel e PowerPoint](https://www.microsoft.com/download/office.aspx).
  
 La versione precedente dell'estensione per il rendering di Word, compatibile con [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003, è rinominata Word 2003. Per impostazione predefinita, è disponibile solo l'estensione per il rendering di Word. È necessario aggiornare i file di configurazione di Reporting Services per rendere disponibile l'estensione per il rendering di Word 2003. Il tipo di contenuto dei file generati dal renderer di Word 2003 è **application/vnd.ms-word** e l'estensione dei file è doc.  
  
 In SQL Server Reporting Services il renderer di Word predefinito è la versione mediante la quale viene eseguito il rendering nel formato docx di [!INCLUDE[ofprword](../../includes/ofprword-md.md)]. Si tratta dell'opzione **Word** elencata nei menu **Esporta** in un portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e in SharePoint. La versione precedente, compatibile solo con [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003, è ora denominata Word 2003 ed è elencata nei menu con tale nome. L'opzione di menu **Word 2003** non è visibile per impostazione predefinita, tuttavia un amministratore può renderla tale aggiornando il file di configurazione RSReportServer. Per esportare i report da [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] usando il renderer di Word 2003, è possibile aggiornare il file di configurazione RSReportDesigner. Tuttavia, rendere visibile il renderer di Word 2003 non lo rende disponibile in tutti gli scenari. Poiché il file di configurazione RSReportServer si trova nel server di report, gli strumenti o prodotti da cui si esportano i report devono essere connessi a un server di report per la lettura del file di configurazione. Se si usano strumenti o prodotti in modalità senza connessione o locale, rendere visibile il renderer di Word 2003 non produce alcun effetto. L'opzione di menu **Word 2003** rimane non disponibile. Se si rende visibile il renderer di Word 2003 nel file di configurazione RSReportDesigner, l'opzione di menu **Word 2003** è sempre disponibile nell'anteprima report di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] .  
  
 L'opzione di menu **Word 2003** non è mai visibile negli scenari seguenti:  
  
-   Generatore report in modalità senza connessione e anteprima di un report in Generatore report.  
  
-   Web part di Visualizzatore report in modalità locale e farm di SharePoint non integrata con un server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni, vedere [Report in modalità locale e con connessione nel visualizzatore di report &#40;Reporting Services in modalità SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 Se il renderer di **Word 2003** è configurato per essere visibile, entrambe le opzioni di menu **Word** e **Word 2003** sono disponibili negli scenari seguenti:  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Portale Web se Reporting Services è installato in modalità nativa.  
  
-   Sito di SharePoint quando Reporting Services è installato in modalità integrata SharePoint.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] e anteprima dei report.  
  
-   Generatore report connesso a un server di report.  
  
-   Web part di Visualizzatore report in modalità remota.  
  
 Nel codice XML seguente sono mostrati gli elementi delle due estensioni per il rendering di Word nei file di configurazione RSReportServer e RSReportDesigner:  
  
 `<Extension Name="WORDOPENXML" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordOpenXmlRenderer.WordOpenXmlDocumentRenderer,Microsoft.ReportingServices.WordRendering"/>`  
  
 `<Extension Name="WORD" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordDocumentRenderer,Microsoft.ReportingServices.WordRendering" Visible="false"/>`  
  
 L'estensione WORDOPENXML consente di definire il renderer di Word per i file con estensione docx di [!INCLUDE[ofprword](../../includes/ofprword-md.md)] . L'estensione WORD consente di definire la versione [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003. `Visible = "false"` indica che il renderer di Word 2003 è nascosto. Per altre informazioni, vedere [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) e [File di configurazione RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).  
  
### <a name="differences-between-the-word-and-word-2003-renderers"></a>Differenze tra i renderer di Word e Word 2003  
 I report, visualizzabili tramite i renderer di Word o Word 2003, tendono a essere non distinguibili da un punto di vista visivo. Tuttavia, è possibile riscontrare piccole differenze tra i due tipi di formati Word o Word 2003.  
  
##  <a name="device-information-settings"></a><a name="DeviceInfo"></a> Impostazioni relative alle informazioni sul dispositivo  
 Modificando le impostazioni relative alle informazioni sul dispositivo, è possibile modificare alcune impostazioni predefinite per questo renderer, ad esempio omettere collegamenti ipertestuali e collegamenti drill-through o espandere tutti gli elementi la cui visibilità può essere attivata o disattivata indipendentemente dal relativo stato originale durante il rendering. Per altre informazioni, vedere [Word Device Information Settings](../../reporting-services/word-device-information-settings.md).  

## <a name="next-steps"></a>Passaggi successivi

[Paginazione in Reporting Services](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
[Comportamenti di rendering](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
[Funzionalità interattiva per estensioni per il rendering di report differenti](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
[Rendering degli elementi del report](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
[Tabelle, matrici ed elenchi](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
