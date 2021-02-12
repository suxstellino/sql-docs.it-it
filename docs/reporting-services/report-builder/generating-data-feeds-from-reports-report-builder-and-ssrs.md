---
title: Generazione di feed di dati dai report (Generatore report) | Microsoft Docs
description: L'estensione per il rendering Atom di Reporting Services genera un documento di servizio per i feed di dati in un report impaginato e i feed dalle aree dati in esso contenute.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: 4e00789f-6967-42e5-b2b4-03181fdb1e2c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e0b1b313671b93fd62fb61d6a244590cf6a62086
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014841"
---
# <a name="generating-data-feeds-from-reports-report-builder-and-ssrs"></a>Generazione di feed di dati dai report (Generatore report e SSRS)

  L'estensione per il rendering Atom di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] consente di generare un documento di servizio Atom in cui sono elencati i feed di dati disponibili in un report impaginato e i feed di dati delle aree dati di un report. Questa estensione viene usata per generare feed di dati conformi ad Atom, leggibili e scambiabili con applicazioni che possono usare i feed di dati generati dai report. Ad esempio è possibile usare l'estensione per il rendering Atom per generare feed di dati che, in seguito, possono essere usati in Power Pivot o Power BI.  
  
 Il documento di servizio Atom elenca almeno un feed di dati per ogni area dati in un report. A seconda del tipo di area dati e dei dati in essa visualizzati, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] potrebbe generare più feed di dati da un'area dati. Ad esempio una matrice o un grafico può fornire più feed di dati. Quando l'estensione per il rendering Atom crea il documento di servizio Atom, viene generato un identificatore univoco per ogni feed di dati che può essere usato nell'URL per accedere al contenuto del feed di dati.  
  
 Il modo in cui l'estensione per il rendering Atom genera i dati per un relativo feed è simile al modo in cui l'estensione per il rendering CSV (Comma-Separated Value, file delimitato da virgole) esegue il rendering dei dati in un file CSV. Come per quest'ultimo tipo di file, un feed di dati è una rappresentazione bidimensionale dei dati del report. Ad esempio una tabella con un gruppo di righe che somma le vendite all'interno di un gruppo, ripete la somma in ogni riga di dati e non vi è alcuna riga separata che contiene solo la somma.  
  
 È possibile generare documenti di servizio Atom e feed di dati usando il portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , Server di report o un sito di SharePoint integrato con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Atom può essere applicato ad alcuni standard correlati. Il documento di servizio Atom è conforme alla specifica del protocollo di pubblicazione Atom RFC 5023 mentre i feed di dati sono conformi alla specifica del protocollo del formato di diffusione Atom RFC 4287.  
  
 Nelle sezioni seguenti vengono fornite ulteriori informazioni sull'utilizzo dell'estensione per il rendering Atom:  
  
 [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="reports-as-data-feeds"></a><a name="ReportDataAsDataFeeds"></a> Report come feed di dati  
 È possibile esportare un report di produzione come un feed di dati oppure creare un report il cui scopo principale è quello di fornire dati alle applicazioni, sotto forma di feed di dati. L'utilizzo dei report come feed di dati rappresenta un ulteriore modo per fornire dati alle applicazioni quando i dati non sono facilmente accessibili tramite i provider di dati client o quando si preferisce nascondere la complessità dell'origine dati e rendere più semplice l'utilizzo dei dati. Se si usano i dati del report come feed di dati è inoltre possibile avvalersi delle caratteristiche di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ad esempio sicurezza, pianificazione e snapshot del report per gestire i report che forniscono i feed di dati.  
  
 Per ottenere il massimo dall'estensione per il rendering Atom, è necessario capire il modo in cui viene eseguito il rendering del report in feed di dati. Se si usano report esistenti, risulta utile essere in grado di stimare i feed di dati che saranno generati dai report, mentre se i report vengono scritti per essere specificatamente usati come feed di dati, la possibilità di includere i dati e ottimizzare il layout del report per aumentare l'utilità dei feed di dati risulta vantaggiosa.  
  
 Per altre informazioni, vedere [Generare i feed di dati da un report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
  
##  <a name="atom-service-document-atomsvc-file"></a><a name="AtomServiceDocument"></a> Documento di servizio Atom (file con estensione atomsvc)  
 Un documento di servizio Atom specifica una connessione a uno o più feed di dati. La connessione è almeno un semplice URL del servizio dati che produce il feed.  
  
 Quando si esegue il rendering dei dati del report tramite l'estensione per il rendering Atom, il documento di servizio Atom elenca i feed di dati disponibili per un report. Nel documento è elencato almeno un feed di dati per ogni area dati nel report. Tabelle e misuratori generano solo un feed di dati ognuno, mentre matrici, elenchi e grafici potrebbero generarne di più a seconda dei dati che visualizzano.  
  
 Nel diagramma seguente viene mostrato un report che usa due tabelle e un grafico.  
  
 ![RS_Atom_TableAndChartDataFeeds](../../reporting-services/report-builder/media/rs-atom-tableandchartdatafeeds.gif "RS_Atom_TableAndChartDataFeeds")  
  
 Il documento di servizio Atom generato da questo report include tre feed di dati, uno per ogni tabella e uno per il grafico.  
  
 Le aree dati della matrice potrebbero disporre di più di un feed di dati, a seconda della struttura della matrice. Nel diagramma seguente viene mostrato un report che usa una matrice che genera due feed di dati.  
  
 ![RS_Atom_PeerDynamicColumns](../../reporting-services/report-builder/media/rs-atom-peerdynamiccolumns.gif "RS_Atom_PeerDynamicColumns")  
  
 Il documento di servizio Atom generato da questo report include due feed di dati, uno per ogni colonna peer dinamica: territorio e anno. Nel diagramma seguente viene mostrato il contenuto di ogni feed di dati.  
  
 ![RS_Atom_PeerDynamicDataFeeds](../../reporting-services/report-builder/media/rs-atom-peerdynamicdatafeeds.gif "RS_Atom_PeerDynamicDataFeeds")  
  
  
##  <a name="data-feeds"></a><a name="DataFeeds"></a> Feed di dati  
 Il feed di dati è un file XML che dispone di un formato tabulare coerente che non cambia nel tempo e di dati variabili che possono essere diversi ogni volta che viene eseguito il report. Il formato dei feed di dati generati da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è uguale a quello dei dati generati da ADO.NET Data Services.  
  
 Un feed di dati contiene due sezioni: intestazione e dati. La specifica Atom definisce gli elementi di ogni sezione. L'intestazione include informazioni quali lo schema di codifica dei caratteri da usare con i feed di dati.  
  
### <a name="header-section"></a>Sezione di intestazione  
 Nel seguente codice XML viene mostrata la sezione di intestazione di un feed di dati.  
  
 `<?xml version="1.0" encoding="utf-8" standalone="yes"?><feed xmlns:d="https://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="https://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">`  
  
 `<title type="text"></title>`  
  
 `<id>uuid:1795992c-a6f3-40ec-9243-fbfd0b1a5be3;id=166321</id>`  
  
 `<updated>2009-05-08T23:09:58Z</updated>`  
  
### <a name="data-section"></a>Sezione di dati  
 La sezione Data dei feed di dati contiene un \<**entry**> elemento per ogni riga del set di righe bidimensionale generato dall'estensione per il rendering Atom.  
  
 Nel diagramma seguente viene mostrato un report che usa gruppi e totali.  
  
 ![RS_Atom_ProductSalesSummaryCircledValues](../../reporting-services/report-builder/media/rs-atom-productsalessummarycircledvalues.gif "RS_Atom_ProductSalesSummaryCircledValues")  
  
 Nel codice XML seguente viene illustrato un \<**entry**> elemento del report in un feed di dati. Si noti che l' \<**entry**> elemento include i totali delle vendite e degli ordini per il gruppo e i totali delle vendite e degli ordini per tutti i gruppi. L' \<**entry**> elemento include tutti i valori nel report.  
  
 `<entry><id>uuid:1795992c-a6f3-40ec-9243-fbfd0b1a5be3;id=166322</id><title type="text"></title><updated>2009-05-08T23:09:58Z</updated><author /><content type="application/xml"><m:properties>`  
  
 `<d:ProductCategory_Value>Accessories</d:ProductCategory_Value>`  
  
 `<d:OrderYear_Value m:type="Edm.Int32">2001</d:OrderYear_Value>`  
  
 `<d:SumLineTotal_Value m:type="Edm.Decimal">20235.364608</d:SumLineTotal_Value>`  
  
 `<d:SumOrderQty_Value m:type="Edm.Int32">1003</d:SumOrderQty_Value>`  
  
 `<d:SumLineTotal_Total_2_1 m:type="Edm.Decimal">1272072.883926</d:SumLineTotal_Total_2_1>`  
  
 `<d:SumOrderQty_Total_2_1 m:type="Edm.Double">61932</d:SumOrderQty_Total_2_1>`  
  
 `<d:SumLineTotal_Total_2_2 m:type="Edm.Decimal">109846381.399888</d:SumLineTotal_Total_2_2>`  
  
 `<d:SumOrderQty_Total_2_2 m:type="Edm.Double">274914</d:SumOrderQty_Total_2_2></m:properties></content>`  
  
 `</entry>`  
  
### <a name="working-with-data-feeds"></a>Utilizzo dei feed di dati  
 Tutti i feed di dati generati dal report includono gli elementi del report che si trovano nell'ambito dell'elemento padre dell'area dati che genera i feed di dati. . Immaginare un report che dispone di diverse tabelle e di un grafico. Le caselle di testo nel corpo del report forniscono il testo descrittivo di ogni area dati. Ogni voce di ciascun feed di dati generato dal report include il valore della casella di testo. Ad esempio se il testo è "Il grafico visualizza le medie delle vendite mensili per area di vendita", tutti e tre i feed di dati includono questo testo in ogni riga.  
  
 Se il layout del report include le relazioni di dati gerarchiche, ad esempio le aree dati nidificate, tali relazioni sono incluse nel set di righe bidimensionale dei dati del report.  
  
 Le righe di dati per le aree dati nidificate sono generalmente ampie, specialmente se le tabelle e le matrici nidificate includono gruppi e totali. Potrebbe essere utile esportare il report in un feed di dati e visualizzare quest'ultimo per verificare che i dati generati siano quelli previsti.  
  
 Quando l'estensione per il rendering Atom crea il documento di servizio Atom, viene generato un identificatore univoco per il feed di dati che può essere usato nell'URL per visualizzare il contenuto del feed di dati. Nel documento di servizio Atom di esempio illustrato in precedenza è incluso l'URL `https://ServerName/ReportServer?%2fProduct+Sales+Summary&rs%3aCommand=Render&rs%3aFormat=ATOM&rc%3aDataFeed=xAx0x1`. L'URL identifica il report (Product Sales Summary), il formato di rendering Atom (ATOM) e il nome del feed di dati (xAx0x1).  
  
 I nomi degli elementi del report vengono impostati in modo predefinito sui nomi degli elementi del linguaggio RDL degli elementi del report e, spesso, non sono intuitivi o facili da ricordare. Ad esempio, il nome predefinito della prima matrice presente in un report è Tablix 1. I feed di dati usano questi nomi.  
  
 Per rendere più semplice l'uso del feed di dati, è possibile sfruttare la proprietà DataElementName dell'area dati per fornire nomi descrittivi. Se si specifica un valore per DataElementName, il sottoelemento feed di dati utilizzerà il valore \<**d**> anziché il nome dell'area dati predefinita. Se, ad esempio, il nome predefinito di un'area dati è Tablix1 e DataElementName è impostato su SalesByTerritoryYear, \<**d**> nel feed di dati viene utilizzato SalesByTerritoryYear. Se le aree dati dispongono di due feed di dati come il report matrice descritto in precedenza, i nomi usati nei feed di dati sono SalesByTerritoryYear _Territory e SalesByTerritoryYear _Year.  
  
 Se si confrontano i dati mostrati sul report e i dati del feed di dati, è possibile notare alcune differenze. I report spesso mostrano dati numerici formattati e relativi all'ora/data, mentre il feed di dati contiene dati non formattati.  
  
 Un feed di dati viene salvato con l'estensione di file atom. Per visualizzare la struttura e il contenuto del file, è possibile usare un editor di testo o XML, ad esempio il Blocco note o l'editor XML.  
  
  
##  <a name="flattening-report-data"></a><a name="FlatteningReportData"></a> Rendere bidimensionali i dati del report  
 Il renderer Atom fornisce dati del report come i set di righe bidimensionali in un formato XML. Le regole per rendere bidimensionali le tabelle di dati sono uguali a quelle del renderer CSV con alcune eccezioni:  
  
-   Gli elementi dell'ambito sono resi bidimensionali a livello di dettaglio. A differenza del renderer CSV, le caselle di testo del livello principale vengono visualizzate in ogni voce scritta nel feed di dati.  
  
-   I valori dei parametri del report vengono sottoposti a rendering in ogni riga dell'output.  
  
 Per poter essere rappresentati nel formato conforme ad Atom, i dati gerarchici e raggruppati devono essere bidimensionali. L'estensione per il rendering rende bidimensionale il report in una struttura ad albero che rappresenta i gruppi nidificati all'interno dell'area dati. Per rendere bidimensionale il report:  
  
-   Una gerarchia di righe viene resa bidimensionale prima di una gerarchia di colonne.  
  
-   Il rendering dei membri della gerarchia di righe nel feed di dati viene eseguito prima di quello dei membri della gerarchia di colonne.  
  
-   Le colonne vengono ordinate nel modo seguente: caselle di testo presenti nel corpo da sinistra verso destra e quindi dall'alto verso il basso seguite dalle aree dati da sinistra verso destra e quindi dall'alto verso il basso.  
  
-   All'interno di un'area dati le colonne vengono ordinate nel modo seguente: membri di angolo, membri della gerarchia delle righe, membri della gerarchia delle colonne e quindi le celle.  
  
-   Le aree dati di pari livello sono aree dati o gruppi dinamici che condividono un'area dati o un predecessore dinamico comune. I dati di pari livello sono identificabili dalle diramazioni dell'albero bidimensionale.  
  
 Per altre informazioni, vedere [Tabelle, matrici ed elenchi &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
  
##  <a name="atom-rendering-rules"></a><a name="AtomRendering"></a> Regole di rendering Atom  
 L'estensione per il rendering Atom ignora le informazioni seguenti quando si esegue il rendering di un feed di dati:  
  
-   Formattazione e layout  
  
-   Intestazione di pagina  
  
-   Piè di pagina  
  
-   Elementi personalizzati del report  
  
-   Rettangoli  
  
-   Linee  
  
-   Immagini  
  
-   Subtotali automatici  
  
 Gli elementi rimanenti del report vengono ordinati dall'alto verso il basso, quindi da sinistra a destra. Viene quindi eseguito il rendering di ogni elemento in una colonna. Se il report include elementi di dati nidificati, ad esempio elenchi o tabelle, gli elementi padre vengono ripetuti in ogni riga.  
  
 Nella seguente tabella è indicato l'aspetto degli elementi del report di cui è stato eseguito il rendering:  
  
|Elemento|Tipo di rendering|  
|----------|------------------------|  
|Tabella|Il rendering viene eseguito mediante l'espansione della tabella e la creazione di una riga e una colonna per ogni riga e colonna al livello di dettaglio inferiore. Per le righe e le colonne di subtotali non sono disponibili intestazioni. I report drill-through non sono supportati.|  
|Matrice|Il rendering viene eseguito mediante l'espansione della matrice e la creazione di una riga e una colonna per ogni riga e colonna al livello di dettaglio inferiore. Per le righe e le colonne di subtotali non sono disponibili intestazioni.|  
|Elenco|Viene eseguito il rendering di un record per ogni riga di dettagli o istanza nell'elenco.|  
|Sottoreport|L'elemento padre viene ripetuto per ogni istanza del contenuto.|  
|Grafico|Viene eseguito il rendering di un record con tutte le etichette del grafico per ogni valore del grafico. Le etichette delle serie e delle categorie nelle gerarchie sono rese bidimensionali e incluse nella riga per un valore del grafico.|  
|Barra dei dati|Viene eseguito il rendering come grafico. In genere, in una barra dei dati non sono incluse gerarchie o etichette.|  
|Grafico sparkline|Viene eseguito il rendering come grafico. In genere, in un grafico sparkline non sono incluse gerarchie o etichette.|  
|Misuratore|Viene eseguito il rendering come un record singolo con i valori minimo e massimo della scala lineare, i valori iniziale e finale dell'intervallo e il valore dell'indicatore di misura.|  
|Indicatore|Viene eseguito il rendering come un singolo record con il nome di stato attivo, gli stati disponibili e il valore dei dati.|  
|Mappa|Viene generato un feed di dati per ogni area dati mappa. Se più livelli mappa usano la stessa area dati, vengono inclusi tutti nel feed di dati. Nel feed di dati è incluso un record con le etichette e i valori per ogni membro della mappa del livello mappa.|  
  
  
##  <a name="device-information-settings"></a><a name="DeviceInfo"></a> Impostazioni relative alle informazioni sul dispositivo  
 È possibile modificare alcune impostazioni predefinite per questo renderer, incluso lo schema di codifica da usare. Per altre informazioni, vedere [ATOM Device Information Settings](../../reporting-services/atom-device-information-settings.md).  

## <a name="next-steps"></a>Passaggi successivi

[Esportazione in un file CSV](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)   
[Esportare report](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
