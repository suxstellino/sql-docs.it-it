---
title: Esportazione in Microsoft Excel (Generatore report) | Microsoft Docs
description: In Generatore report l'estensione per il rendering di Excel esegue il rendering di un report impaginato nel formato Office Open XML da usare con Microsoft Excel.
ms.date: 12/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: 74f726fc-2167-47af-9093-1644e03ef01f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4e8b43d3e9e772b06961602bf2d8bd69aa51216e
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878854"
---
# <a name="exporting-to-microsoft-excel-report-builder-and-ssrs"></a>Esportazione in Microsoft Excel (Generatore report e SSRS)
  L'estensione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per il rendering di Excel consente di eseguire il rendering di un report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] impaginato nel formato [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (con estensione xlsx). Con l'estensione per il rendering di Excel, la larghezza delle colonne in Excel si riflette con più accuratezza nella larghezza delle colonne nei report.  
  
 Il formato è Office Open XML. Il tipo di contenuto dei file generati da questo renderer è **application/vnd.openxmlformats-officedocument.spreadsheetml.sheet** e l'estensione dei file è xlsx.  
  
 È possibile modificare alcune impostazioni predefinite per questo renderer modificando le impostazioni relative alle informazioni sul dispositivo. Per altre informazioni, vedere [Excel Device Information Settings](../../reporting-services/excel-device-information-settings.md).  
  
 Per informazioni dettagliate su come esportare in Excel, vedere [Esportazione di report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
> [!IMPORTANT]  
>  Quando si definisce un parametro di tipo **String**, viene visualizzata una casella di testo che può accettare qualsiasi valore. Se un parametro di report non è correlato a un parametro di query e i valori del parametro sono inclusi nel report, un utente potrebbe digitare nel valore del parametro un URL, uno script o la sintassi di un'espressione ed eseguire il rendering del report in formato Excel. Se il report viene in seguito visualizzato da un altro utente che fa clic sul contenuto dei parametri di cui è stato eseguito il rendering, è possibile che venga inavvertitamente eseguito il collegamento o lo script dannoso.  
>   
>  Per ridurre il rischio di eseguire inavvertitamente script dannosi, aprire i report visualizzabili solo da origini attendibili. Per altre informazioni sulla sicurezza dei report, vedere [Garantire la sicurezza di report e risorse](../../reporting-services/security/secure-reports-and-resources.md).  
  
##  <a name="excel-limitations"></a><a name="ExcelLimitations"></a> Limitazioni di Excel  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] sono previste limitazioni in relazione ai report esportati che dipendono dalle funzionalità di Excel e dai relativi formati file. Di seguito vengono elencate le limitazioni più importanti:  
  
-   La larghezza massima delle colonne è limitata a 255 caratteri o 1726,5 punti. Nel renderer non viene verificato se la larghezza della colonna sia inferiore a tale limite.  
  
-   Il numero massimo di caratteri in una cella è limitato a 32.767. Se tale limite viene superato, nel renderer viene visualizzato un messaggio di errore.  
  
-   L'altezza massima della riga è pari a 409 punti. Se a causa del contenuto della riga l'altezza della riga supera i 409 punti, la cella di Excel visualizza una parte del testo fino a un massimo di 409 punti. La parte rimanente del contenuto della cella rimane all'interno della cella (fino al numero di caratteri massimo di Excel di 32.767).

-  Poiché l'altezza massima della riga è pari a 409 punti, se l'altezza della cella definita nel report è superiore a 409 punti, Excel suddivide il contenuto della cella in più righe.
  
-   Sebbene in Excel non sia definito un numero massimo di fogli di lavoro, alcuni fattori esterni, quali la memoria e lo spazio su disco, possono comportare l'applicazione di limitazioni.  
  
-   Nelle strutture sono consentiti solo fino a sette livelli annidati.  
  
-   Se l'elemento del report con cui si controlla l'attivazione o la disattivazione di un altro elemento non è disponibile nella riga o nella colonna precedente o successiva dell'elemento che viene attivato/disattivato, anche la struttura viene disabilitata.  
  
 Per altre informazioni sulle limitazioni di Excel, vedere [Specifiche e limiti di Excel](https://support.microsoft.com/en-us/office/excel-specifications-and-limits-1672b34d-7043-467e-8e27-269d656771c3).  
  
### <a name="sizes-of-excel-2003-xls-files"></a>Dimensioni dei file di Excel 2003 (con estensione xls)  
  
> [!IMPORTANT]  
>  L'estensione per il rendering di [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 è deprecata. Per altre informazioni, vedere [Funzionalità deprecate di SQL Server Reporting Services in SQL Server 2016](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 Quando i report vengono prima esportati e poi salvati in Excel 2003, non si ottengono i vantaggi derivanti dall'ottimizzazione dei file applicata automaticamente da Excel ai file delle cartelle di lavoro *.xls. Dimensioni dei file maggiori possono provocare problemi alle sottoscrizioni tramite posta elettronica e agli allegati di posta elettronica. Per ridurre le dimensioni dei file \*.xls per i report esportati, aprire i file \*.xls e salvare di nuovo le cartelle di lavoro. L'ulteriore salvataggio delle cartelle di lavoro determina in genere una riduzione del 40-50 percento delle dimensioni dei relativi file.  
  
> [!NOTE]  
>  In Excel 2003 vengono visualizzati circa 1000 caratteri in una cella del foglio di lavoro. Nella barra della formula è tuttavia possibile modificare tale valore fino a raggiungere il numero massimo di caratteri. Questa limitazione non si applica ai file Excel (con estensione xlsx) correnti.  
  
### <a name="text-boxes-and-text"></a>Caselle di testo e testo  
 Alle caselle di testo e al testo si applicano le limitazioni seguenti:  
  
-   I valori della casella di testo costituiti da espressioni non vengono convertiti in formule di Excel. Il valore di ogni casella di testo viene valutato durante l'elaborazione del report. L'espressione valutata viene esportata come contenuto di ogni cella di Excel.  
  
-   Il rendering delle caselle di testo viene eseguito all'interno di una cella di Excel. Le dimensioni e il tipo di carattere, gli effetti e lo stile del carattere costituiscono la sola formattazione supportata nel singolo testo all'interno di una cella di Excel.  
  
-   L'effetto di testo "Linea sopra" non è supportato in Excel.  
  
-   In Excel viene aggiunto un riempimento predefinito di circa 3,75 punti a sinistra e a destra delle celle. Se l'impostazione relativa al riempimento di una casella di testo è inferiore a 3,75 punti e lo spazio disponibile è appena sufficiente per il testo, è possibile che in Excel il testo venga mandato a capo in modo automatico.  
  
    > [!NOTE]  
    >  Per risolvere questo problema, aumentare la larghezza della casella di testo nel report.  
  
### <a name="images"></a>Immagini  
 Alle immagini si applicano le limitazioni seguenti:  
  
-   Le immagini di sfondo per gli elementi del report vengono ignorate, poiché in Excel non sono supportate immagini di sfondo per le singole celle.  
  
-   L'estensione per il rendering Excel supporta solo l'immagine di sfondo del corpo del report. Se nel report viene visualizzata un'immagine di sfondo del corpo di report, il rendering dell'immagine verrà eseguito come immagine di sfondo del foglio di lavoro.  
  
### <a name="rectangles"></a>Rettangoli  
 Ai rettangoli si applicano le limitazioni seguenti:  
  
-   I rettangoli nei piè di pagina del report non vengono esportati in Excel. Il rendering dei rettangoli nel corpo del report, delle celle della Tablix e così via viene tuttavia eseguito come intervallo di celle di Excel.  
  
### <a name="report-headers-and-footers"></a>Intestazioni e piè di pagina del report  
 Alle intestazioni e ai piè di pagina del report si applicano le limitazioni seguenti:  
  
-   Nelle intestazioni e nei piè di pagina di Excel sono supportati al massimo 256 caratteri, incluso il markup. L'estensione per il rendering tronca la stringa a 256 caratteri.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non supporta margini nelle intestazioni e nei piè di pagina del report. Durante l'esportazione in Excel, questi valori dei margini vengono impostati su zero e qualsiasi intestazione o piè di pagina contenente più righe di dati potrebbe non essere stampata su più righe, a seconda delle impostazioni della stampante.  
  
-   Quando vengono esportate in Excel, le caselle di testo in un'intestazione o in un piè di pagina mantengono la relativa formattazione, ma non l'allineamento corrispondente. Questa situazione si verifica in quanto gli spazi iniziali e finali vengono rimossi durante il rendering del report.  
  
### <a name="merging-cells"></a>Unione di celle  
 All'unione di celle si applica la limitazione seguente:  
  
-   Se le celle sono unite, il ritorno a capo automatico non verrà eseguito correttamente. Se sono presenti celle unite in una riga in cui il rendering di una casella di testo viene eseguito con la proprietà AutoSize, il ridimensionamento automatico non funzionerà.  
  
 Il renderer di Excel è principalmente un renderer di layout, il cui obiettivo è replicare il layout del report visualizzabile nel modo più simile possibile in un foglio di lavoro di Excel e di conseguenza le celle potrebbero venire unite nel foglio di lavoro per mantenere inalterato il layout del report. La presenza di celle unite può causare problemi in quanto, affinché l'ordinamento venga eseguito correttamente, la funzionalità di ordinamento di Excel richiede che le celle vengano unite in un modo specifico. Per eseguirne l'ordinamento in Excel, gli intervalli di celle unite devono ad esempio avere le stesse dimensioni.  
  
 Se la possibilità di ordinare i report esportati in fogli di lavoro di Excel è un aspetto importante, le informazioni riportate di seguito possono contribuire a ridurre il numero di celle unite nei fogli di lavoro di Excel, eliminando una delle cause più comuni che rendono difficile l'utilizzo della funzionalità di ordinamento di Excel.  
  
-   Il mancato allineamento di elementi a sinistra e destra è la causa più comune di celle unite. Verificare che i bordi sinistro e destro di tutti gli elementi del report siano allineati l'uno all'altro. Nella maggior parte dei casi, il problema verrà risolto allineando gli elementi e assegnando loro la stessa larghezza.  
  
-   Anche dopo aver allineato con precisione tutti gli elementi, è possibile che alcune colonne vengano comunque unite. Questo problema potrebbe essere causato dalla conversione delle unità interne e dal conseguente arrotondamento durante il rendering del foglio di lavoro di Excel. Nel linguaggio RDL (Report Definition Language) è possibile specificare posizione e dimensione in unità di misura diverse, ad esempio pollici, pixel, centimetri e punti. Excel usa internamente l'unità di misura in punti. Per ridurre al minimo i possibili errori di conversione e arrotondamento durante la conversione di pollici e centimetri in punti e ottenere i risultati più precisi, specificare tutte le misure in punti. Un pollice corrisponde a 72 punti.  
  
### <a name="report-row-groups-and-column-groups"></a>Gruppi di righe e di colonne del report  
 I report che includono gruppi di righe o di colonne contengono celle vuote quando vengono esportati in Excel. Si immagini un report che raggruppa righe in distanza dal lavoro. Ogni distanza dal lavoro può contenere più di un cliente. Nell'immagine seguente viene illustrato il report.  
  
 ![Portale Web di Reporting Services](../../reporting-services/report-builder/media/ssrb-excelexportssrs.png "Portale Web di Reporting Services")  
  
 Quando il report viene esportato in Excel,la distanza dal lavoro viene visualizzata solo in una cella della colonna Distanza dal lavoro. A seconda dell'allineamento del testo nel report (in alto, al centro o in basso), il valore si trova nella prima cella, in quella centrale o nell'ultima. Le altre celle sono vuote. La colonna Nome, contenente i nomi dei clienti, non presenta celle vuote. Nell'immagine seguente viene mostrato il report dopo la relativa esportazione in Excel. I bordi rossi della cella sono stati aggiunti per metterla in risalto. Le caselle di colore grigio sono le celle vuote. Le righe rosse e le caselle di colore grigio non fanno parte del report esportato.  
  
 ![Report esportato in Excel, con righe](../../reporting-services/report-builder/media/ssrb-exportedexcellines.png "Report esportato in Excel, con righe")  
  
 Pertanto i report con gruppi di righe o di colonne devono essere modificati dopo la relativa esportazione in Excel e prima di poter visualizzare i dati esportati in una tabella pivot. Il valore del gruppo deve essere aggiunto alle celle in cui non è disponibile per rendere il foglio di lavoro una tabella bidimensionale con valori in tutte le celle. L'immagine seguente illustra il foglio di lavoro di aggiornamento.  
  
 ![Report esportato in Excel, bidimensionale](../../reporting-services/report-builder/media/ssrb-excelexportnomatrix.png "Report esportato in Excel, bidimensionale")  
  
 Se quindi viene creato un report con lo scopo specifico di esportalo in Excel per un'ulteriore analisi dei dati, è necessario considerare di non includervi gruppi di righe o di colonne.  
  
## <a name="excel-renderer"></a>Renderer di Excel  
  
### <a name="current-xlsx-excel-file-renderer"></a>Renderer del file Excel (con estensione xlsx) corrente  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]il renderer di Excel predefinito è la versione compatibile con i file [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (con estensione xlsx) corrente. Si tratta dell'opzione **Excel** elencata nei menu **Esportazione in corso** nel portale Web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e nell'elenco di SharePoint.  
  
 Quando si usa il renderer di Excel predefinito, anziché il renderer precedente di Excel 2003 (con estensione xls), è possibile installare Microsoft Office Compatibility Pack per i formati di file Word, Excel e PowerPoint per consentire alle versioni precedenti di Excel di aprire i file esportati.  
  
### <a name="excel-2003-xls-renderer"></a>Renderer di Excel 2003 (con estensione xls)  
  
> [!IMPORTANT]  
>  L'estensione per il rendering di [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 è deprecata. Per altre informazioni, vedere [Funzionalità deprecate di SQL Server Reporting Services in SQL Server 2016](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 La versione precedente del renderer di Excel, compatibile con Excel 2003, è ora denominata Excel 2003 ed è elencata nei menu con tale nome. Il tipo di contenuto dei file generati da questo renderer è **application/vnd.ms-excel** e l'estensione dei file è xls.  
  
 Per impostazione predefinita, l'opzione di menu **Excel 2003** non è visibile. Un amministratore può renderla visibile in determinate circostanze aggiornando il file di configurazione RSReportServer. Per esportare i report da [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] usando il renderer di Excel 2003, è possibile aggiornare il file di configurazione RSReportDesigner.  
  
 L'estensione dell'opzione di menu **Excel 2003** non è mai visibile negli scenari seguenti:  
  
-   Generatore report in modalità senza connessione e anteprima di un report in Generatore report. Poiché il file di configurazione RSReportServer si trova nel server di report, gli strumenti o prodotti da cui si esportano i report devono essere connessi a un server di report per la lettura del file di configurazione.  
  
-   Web part di Visualizzatore report in modalità locale e farm di SharePoint non integrata con un server di report [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni, vedere [Report in modalità locale e con connessione nel visualizzatore di report &#40;Reporting Services in modalità SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 Se il renderer dell'opzione di menu **Excel 2003** è configurato per essere visibile, sia le opzioni di Excel sia di Excel 2003 sono disponibili negli scenari seguenti:  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] - Portale Web modalità nativa.  
  
-   Sito di SharePoint quando Reporting Services è installato in modalità integrata SharePoint.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] e anteprima dei report.  
  
-   Generatore report connesso a un server di report.  
  
-   Web part di Visualizzatore report in modalità remota.  
  
 Nel codice XML seguente sono mostrati gli elementi delle due estensioni per il rendering di Excel nei file di configurazione RSReportServer e RSReportDesigner:  
  
 `<Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>`  
  
 `<Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>`  
  
 L'estensione EXCELOPENXML consente di definire il renderer di Excel per i file Excel (con estensione xlsx) correnti. L'estensione EXCEL consente di definire la versione Excel 2003. `Visible = "false"` indica che il renderer di Excel 2003 è nascosto. Per altre informazioni, vedere [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) e [File di configurazione RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).  
  
### <a name="differences-between-the-current-xlsx-excel-and-excel-2003-renderers"></a>Differenze tra i renderer di Excel (con estensione xlsx) correnti ed Excel 2003  
 I report, di cui è stato eseguito il rendering tramite i renderer di Excel (con estensione xlsx) correnti o Excel 2003, sono in genere identici e solo in rare circostanze si noteranno differenze tra i due formati. Nella tabella seguente vengono confrontati i renderer di Excel ed Excel 2003.  
  
|Proprietà|Excel 2003|Excel corrente|  
|--------------|----------------|-------------------|  
|Numero massimo di colonne per foglio di lavoro|256|16.384|  
|Numero massimo di righe per foglio di lavoro|65,536|1,048,576|  
|Numero di colori consentito in un foglio di lavoro|56 (tavolozza)<br /><br /> Se nel report vengono usati più di 56 colori, l'estensione per il rendering abbina il colore desiderato a uno dei 56 colori già disponibili nella tavolozza personalizzata.|Circa 16 milioni (colore a 24 bit)|  
|File compressi ZIP|nessuno|Compressione ZIP|  
|Famiglia di caratteri predefinita|Arial|Calibri|  
|Dimensioni del carattere predefinite|10pt|11pt|  
|Altezza della riga predefinita|12,75 pt|15 pt|  
  
 Poiché nel report viene impostata in modo esplicito l'altezza della riga, l'altezza della riga predefinita influisce solo su righe ridimensionate automaticamente durante l'esportazione in Excel.  
  
##  <a name="report-items-in-excel"></a><a name="ReportItemsExcel"></a> Elementi del report in Excel  
 Il rendering di rettangoli, sottoreport, del corpo del report e delle aree dati viene eseguito come intervallo di celle di Excel. Il rendering di caselle di testo, immagini, grafici, barre dei dati, grafici sparkline, mappe, misuratori e indicatori deve essere eseguito all'interno di una cella di Excel che potrebbe risultare unita a seconda del layout della parte restante del report.  
  
 Immagini, grafici, barre dei dati, grafici sparkline, mappe, misuratori, indicatori e linee vengono posizionati all'interno di una cella di Excel, ma vengono inseriti sopra la griglia di celle. Il rendering delle linee viene eseguito come bordi della cella.  
  
 I grafici, le barre dei dati, i grafici sparkline, le mappe, i misuratori e gli indicatori vengono esportati come immagini. I dati rappresentati, ad esempio le etichette del valore e del membro per un grafico, non vengono esportati con gli oggetti elencati in precedenza e non sono disponibili nella cartella di lavoro di Excel a meno che non siano inclusi in una colonna o riga di un'area dati all'interno di un report.  
  
 Se si desidera usare i dati di grafici, grafici sparkline, barre dei dati, mappe, misuratori e indicatori esportare il report in un file con estensione csv o generare feed di dati conformi ad Atom dal report. Per altre informazioni, vedere [Esportazione in un file CSV &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md) e [Generazione di feed di dati dai report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  
  
## <a name="page-sizing"></a>Ridimensionamento della pagina  
 Nell'estensione per il rendering di Excel vengono usate le impostazioni relative all'altezza e alla larghezza della pagina per determinare l'impostazione del foglio da definire nel foglio di lavoro di Excel. Excel tenta di abbinare le impostazioni delle proprietà PageHeight e PageWidth a uno dei formati carta più comuni.  
  
 Se non viene trovata alcuna corrispondenza, vengono usate le dimensioni di pagina predefinite per la stampante. Se la larghezza della pagina è inferiore all'altezza, l'orientamento verrà impostato su Verticale. In caso contrario, verrà impostato su Orizzontale.  
  
##  <a name="worksheet-tab-names"></a><a name="WorksheetTabNames"></a> Nomi delle schede dei fogli di lavoro  
 Quando si esporta un report in Excel, le pagine del report create dalle interruzioni di pagina vengono esportate in fogli di lavoro differenti. Se è stato specificato un nome della pagina iniziale per il report, ogni foglio di lavoro della cartella di lavoro di Excel avrà questo nome per impostazione predefinita. Il nome viene visualizzato sulla scheda del foglio di lavoro. Tuttavia, poiché ogni foglio di lavoro in una cartella di lavoro deve avere un nome univoco, al nome della pagina iniziale di ogni foglio di lavoro aggiuntivo viene aggiunto un numero intero a partire da 1 e aumentato di 1. Se ad esempio il nome della pagina iniziale è **Report di vendite per anno fiscale**, il secondo foglio di lavoro verrebbe denominato **Report di vendite per anno fiscale 1** mentre il terzo **Report di vendite per anno fiscale 2** e così via.  
  
 Se per tutte le pagine del report create dalle interruzioni di pagina vengono specificati nomi di pagina nuovi, ogni foglio di lavoro avrà il nome della pagina associato. Tuttavia, questi nomi di pagina non sarebbero univoci. In tal caso, i fogli di lavoro sono denominati con la stessa modalità dei nomi di pagina iniziali. Se ad esempio il nome della pagina di due gruppi è **Vendite di NW**, una scheda del foglio di lavoro avrà il nome **Vendite di NW** mentre l'altra avrà **Vendite di NW 1**.  
  
 Se per il report non viene specificato né un nome di pagina iniziale, né nomi di pagina correlati alle interruzioni di pagina, le schede del foglio di lavoro avranno i nomi predefiniti **Foglio1**, **Foglio2** e così via.  
  
 Reporting Services fornisce proprietà per impostare report, aree dati, gruppi e rettangoli per facilitare la creazione di report esportabili in Excel in una modalità desiderata. Per altre informazioni, vedere [Paginazione in Reporting Services &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> Proprietà del documento  
 Il renderer di Excel scrive i metadati seguenti nel file di Excel.  
  
|Proprietà degli elementi del report|Descrizione|  
|-------------------------------|-----------------|  
|Data di creazione|Data e ora di esecuzione del report espresse come valore data/ora ISO.|  
|Autore|Report.Author|  
|Descrizione|Report.Description|  
|LastSaved|Data e ora di esecuzione del report espresse come valore data/ora ISO.|  
  
##  <a name="page-headers-and-footers"></a><a name="PageHeadersFooters"></a> Intestazioni di pagina e piè di pagina  
 Il rendering dell'intestazione di pagina può essere eseguito in due diversi modi a seconda dell'impostazione SimplePageHeaders delle informazioni sul dispositivo, ovvero sopra la griglia di celle di ogni foglio di lavoro oppure nell'effettiva sezione di intestazione del foglio di lavoro di Excel. Per impostazione predefinita, il rendering dell'intestazione viene eseguito nella griglia di celle del foglio di lavoro di Excel.  
  
 Il rendering del piè di pagina viene sempre eseguito nella sezione effettiva del piè di pagina di foglio di lavoro di Excel, indipendentemente dal valore dell'impostazione SimplePageHeaders.  
  
 Nelle sezioni di intestazione e piè di pagina di Excel sono supportati al massimo 256 caratteri, incluso il markup. Se questo limite viene superato, il renderer di Excel rimuove i caratteri di markup a partire dalla fine della stringa dell'intestazione e/o del piè di pagina per ridurre il numero di caratteri totali. Se la lunghezza supera il limite massimo anche dopo l'eliminazione di tutti i caratteri di markup, la stringa verrà troncata a partire da destra.  
  
### <a name="simplepageheader-settings"></a>Impostazioni SimplePageHeader  
 Per impostazione predefinita, il valore dell'impostazione SimplePageHeaders delle informazioni sul dispositivo è **False**. Il rendering delle intestazioni di pagina viene pertanto eseguito come righe del report sulla superficie del foglio di lavoro di Excel. Le righe del foglio di lavoro che contengono le intestazioni vengono bloccate. È possibile bloccare o sbloccare il riquadro in Excel. Se l'opzione **Stampa titoli** è selezionata, le intestazioni vengono automaticamente impostate per la stampa su ogni pagina del foglio di lavoro.  
  
 Se l'opzione **Stampa titoli** è selezionata nella scheda Layout di pagina di Excel, l'intestazione di pagina viene ripetuta all'inizio di ogni foglio di lavoro della cartella di lavoro, ad eccezione della copertina della mappa documento. Se l'opzione **Stampa sulla prima pagina** o **Stampa sull'ultima pagina** non è selezionata nelle finestre di dialogo Proprietà intestazione report o Proprietà piè di pagina report, l'intestazione non verrà aggiunta alla prima o all'ultima pagina rispettivamente.  
  
 Il rendering dei piè di pagina viene eseguito nella sezione del piè di pagina di Excel.  
  
 A causa delle limitazioni di Excel, le caselle di testo sono l'unico elemento del report di cui è possibile eseguire il rendering nella sezione dell'intestazione o del piè di pagina di Excel.  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> Interattività  
 Alcuni elementi interattivi sono supportati in Excel. Di seguito è riportata una descrizione di comportamenti specifici.  
  
### <a name="show-and-hide"></a>Elementi visualizzati e nascosti  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] sono previste limitazioni in relazione alla modalità di gestione degli elementi del report visualizzati e nascosti in seguito all'esportazione. Il rendering di gruppi, righe e colonne contenenti elementi del report attivabili e disattivabili viene eseguito come strutture di Excel. In Excel vengono create strutture per espandere e comprimere righe e colonne nell'intera riga o colonna. È pertanto possibile che vengano compressi anche elementi del report che non devono esserlo. Si possono inoltre verificare problemi con i simboli di struttura di Excel che possono determinare la sovrapposizione di strutture. Per risolvere questi problemi, quando si usa l'estensione per il rendering Excel vengono applicate le seguenti regole per la creazione di strutture:  
  
-   L'elemento di report presente nell'angolo superiore sinistro che può essere attivato/disattivato continuerà a essere attivabile/disattivabile in Excel. Gli elementi del report che possono essere attivati/disattivati e che condividono spazio verticale o orizzontale con l'elemento di report attivabile/disattivabile nell'angolo superiore sinistro non possono essere attivati/disattivati in Excel.  
  
-   Per stabilire se un'area dati sarà comprimibile in base alle righe o alle colonne, vengono determinate la posizione dell'elemento del report che controlla l'attivazione o la disattivazione e la posizione dell'elemento del report che viene attivato/disattivato. Se l'elemento che controlla l'attivazione o la disattivazione è presente prima dell'elemento da attivare/disattivare, l'elemento sarà comprimibile in base alle righe. In caso contrario, l'elemento sarà comprimibile in base alle colonne. Se l'elemento che controlla l'attivazione o la disattivazione viene visualizzato accanto e sopra l'area da attivare/disattivare in modo uniforme, il rendering dell'elemento viene eseguito con la riga comprimibile in base alle righe.  
  
-   Per determinare la posizione dei subtotali nel report visualizzabile, l'estensione per il rendering esamina la prima istanza di un membro dinamico. Se sopra tale membro è presente un membro statico di pari livello, si presuppone che il membro dinamico corrisponda ai subtotali. Vengono impostate strutture per indicare che si tratta di dati di riepilogo. In assenza di elementi di pari livello statici di un membro dinamico, la prima istanza dell'istanza corrisponde al subtotale.  
  
-   A causa di una limitazione di Excel, le strutture possono presentare solo fino a sette livelli di nidificazione.  
  
### <a name="document-map"></a>Mappa documento  
 Se nel report sono presenti eventuali etichette della mappa documento, viene eseguito il rendering di una mappa documento. Il rendering della mappa documento viene eseguito come foglio di lavoro di copertina di Excel inserito come prima scheda della cartella di lavoro. Al foglio di lavoro viene assegnato il nome **Mappa documento**.  
  
 Il testo visualizzato nella mappa documento viene determinato dalla proprietà DocumentMapLabel dell'elemento del report o del gruppo. Le etichette della mappa documento sono elencate nell'ordine in cui sono inserite nel report, a partire dalla prima riga della prima colonna. In ogni cella dell'etichetta della mappa documento sono presenti rientri il cui numero corrisponde a quello dei livelli di profondità con cui viene visualizzata nel report. Ogni livello di rientro viene rappresentato posizionando l'etichetta in una colonna successiva. Excel supporta fino a 256 livelli di nidificazione della struttura.  
  
 Il rendering della struttura della mappa documento viene eseguito come struttura di Excel comprimibile. Tale struttura corrisponde alla struttura annidata della mappa documento. Lo stato di espansione o compressione della struttura inizia a partire dal secondo livello.  
  
 Il nodo radice della mappa corrisponde al nome del report, ovvero \<*reportname*>.rdl, e non è interattivo. Il tipo di carattere dei collegamenti alla mappa documento è Arial, 10pt.  
  
### <a name="drillthrough-links"></a>Collegamenti drill-through  
 Il rendering dei collegamenti drill-through visualizzati nelle caselle di testo viene eseguito come collegamenti ipertestuali di Excel nella cella in cui viene eseguito il rendering del testo. Il rendering dei collegamenti drill-through per immagini e grafici viene eseguito come collegamenti ipertestuali di Excel nell'immagine durante il rendering stesso. Quando si fa clic su un collegamento drill-through, viene aperto il browser predefinito del client e si passa alla visualizzazione HTML della destinazione.  
  
### <a name="hyperlinks"></a>Collegamenti ipertestuali  
 Il rendering dei collegamenti ipertestuali visualizzati nelle caselle di testo viene eseguito come collegamenti ipertestuali di Excel nella cella in cui viene eseguito il rendering del testo. Il rendering dei collegamenti ipertestuali per immagini e grafici viene eseguito come collegamenti ipertestuali di Excel nell'immagine durante il rendering stesso. Quando si fa clic su un collegamento ipertestuale, viene aperto il browser predefinito del client e si passa all'URL di destinazione.  
  
### <a name="interactive-sorting"></a>Ordinamento interattivo  
 L'ordinamento interattivo non è supportato in Excel.  
  
### <a name="bookmarks"></a>Segnalibri  
 Il rendering dei collegamenti a segnalibro visualizzati nelle caselle di testo viene eseguito come collegamenti ipertestuali di Excel nella cella in cui viene eseguito il rendering del testo. Il rendering dei collegamenti a segnalibro per immagini e grafici viene eseguito come collegamenti ipertestuali di Excel nell'immagine durante il rendering stesso. Quando si fa clic su un segnalibro, si passa alla cella di Excel in cui viene eseguito il rendering dell'elemento di report con segnalibro.  
  
##  <a name="changing-reports-at-run-time"></a><a name="ConditionalFormat"></a> Modifica dei report in fase di esecuzione  
 Se per un report è necessario eseguire il rendering in più formati e non è possibile creare un layout del report che consenta di eseguire il rendering nel modo desiderato in tutti i formati necessari, considerare la possibilità di usare il valore incluso nell'elemento globale predefinito RenderFormat per modificare in modo condizionale l'aspetto del report in fase di esecuzione. In questo modo è possibile nascondere o rendere visibili gli elementi del report a seconda del renderer usato per ottenere i migliori risultati in ogni formato. Per altre informazioni, vedere [Riferimenti alle raccolte predefinite Globals e Users &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Paginazione in Reporting Services &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Tipi di rendering &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Funzionalità interattiva per estensioni per il rendering di report differenti &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendering degli elementi del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tabelle, matrici ed elenchi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
