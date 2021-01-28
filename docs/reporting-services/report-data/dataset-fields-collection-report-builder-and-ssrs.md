---
title: Raccolta di campi del set di dati (Generatore report) | Microsoft Docs
description: Informazioni sulla raccolta di campi di un set di dati. I campi del set di dati rappresentano i dati provenienti da una connessione dati. Un campo può presentare dati numerici o non numerici.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: b3884576-1f7e-4d40-bb7d-168312333bb3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6ad6853715963c9a29ee4db2e73199fecd50801a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597317"
---
# <a name="dataset-fields-collection-report-builder-and-ssrs"></a>Raccolta di campi del set di dati (Generatore report e SSRS)
  I campi del set di dati rappresentano i dati provenienti da una connessione dati. Un campo può presentare dati numerici o non numerici. Possono essere inclusi, ad esempio, importi delle vendite, vendite totali, nomi dei clienti, identificatori di database, URL, immagini, dati spaziali e indirizzi di posta elettronica. Nell'area di progettazione i campi vengono visualizzati come espressioni in elementi del report quali caselle di testo, tabelle e grafici.  
  
 Un report dispone di tre tipi di campi che vengono visualizzati nel riquadro dei dati del report, ovvero i campi del set di dati, i campi calcolati del set di dati e i campi predefiniti.  
  
-   **Campi del set di dati.** I metadati che rappresentano la raccolta dei campi che verranno restituiti quando viene eseguita la query del set di dati nell'origine dati.  
  
-   **Campi calcolati del set di dati.** I campi aggiuntivi creati dall'utente per il set di dati. Ogni campo calcolato viene creato valutando un'espressione definita dall'utente.  
  
-   **Campi predefiniti.** I metadati che rappresentano una raccolta dei campi forniti da Generatore report contenenti informazioni sui report quali il nome o l'ora di elaborazione del report. Per altre informazioni, vedere [Riferimenti alle raccolte predefinite Globals e Users &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
 I nomi dei campi del set di dati vengono salvati come parte della definizione del set di dati del report. Per altre informazioni, vedere [Set di dati condivisi e incorporati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="dataset-fields-and-queries"></a><a name="Fields"></a> Campi del set di dati e query  
 I campi del set di dati vengono specificati dal comando di query del set di dati e da qualsiasi campo calcolato definito dall'utente. La raccolta di campi visualizzata nel report dipende dal tipo di set di dati a disposizione:  
  
-   **Set di dati condivisi.** La raccolta di campi è costituita dall'elenco di campi per la query presente nella definizione del set di dati condiviso quando il set di dati condiviso è stato aggiunto direttamente al report oppure quando è stata aggiunta una parte di report nella quale era incluso il set di dati condiviso. La raccolta di campi locale non subisce variazioni quando viene modificata la definizione del set di dati condiviso sul server di report. Per aggiornare la raccolta di campi locale, è necessario aggiornare l'elenco per il set di dati condiviso locale.  
  
-   **Set di dati incorporato.** La raccolta di campi è costituita dall'elenco di campi restituito tramite l'esecuzione della query corrente rispetto all'origine dati.  
  
 Per altre informazioni, vedere [Aggiunta, modifica e aggiornamento di campi nel riquadro dei dati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)  
  
  
### <a name="calculated-fields"></a>Campi calcolati  
 Un campo calcolato viene specificato manualmente creando un'espressione. I campi calcolati possono essere utilizzati per creare nuovi valori che non esistono nell'origine dati. Un campo calcolato può rappresentare ad esempio un nuovo valore, un ordinamento personalizzato per un set di valori del campo o un campo esistente convertito in un tipo di dati diverso.  
  
 I campi calcolati sono locali in un report e non possono essere salvati come parte di un set di dati condiviso.  
  
 Per altre informazioni, vedere [Aggiunta, modifica e aggiornamento di campi nel riquadro dei dati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
  
### <a name="entities-and-entity-fields"></a>Entità e campi di entità  
 Se si utilizza un'origine dati del modello di report, è necessario specificare le entità e i campi delle entità come dati del report in uso. In Progettazione query per un modello di report è possibile esplorare in modo interattivo e selezionare le entità correlate, nonché scegliere i campi che si desidera includere nel set di dati del report. Dopo aver completato la progettazione della query, è possibile vedere la raccolta di identificatori di entità e campi di entità nel riquadro dei dati del report. Gli identificatori di entità vengono generati automaticamente dal modello di report e in genere non vengono visualizzati dall'utente finale.  
  
### <a name="using-extended-field-properties"></a>Utilizzo delle proprietà di campo estese  
 Le origini dati che supportano query multidimensionali, ad esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], supportano anche le proprietà di campo per i campi. Tali proprietà vengono visualizzate nel set di risultati per una query, ma non sono visibili nel riquadro **Dati report** . È possibile comunque usarle nel report. Per fare riferimento a una proprietà per un campo, trascinare il campo nel report e modificare la proprietà predefinita **Value** impostandola sul nome del campo della proprietà desiderata. In un cubo di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , ad esempio, è possibile definire formati per i valori presenti nelle celle del cubo. Il valore formattato è disponibile se si usa la proprietà di campo **FormattedValue**. Per usare direttamente il valore anziché usare un valore e impostare la proprietà del formato della casella di testo, trascinare il campo nella casella di testo e impostare l'espressione predefinita `=Fields!FieldName.Value` su `=Fields!FieldName.FormattedValue`.  
  
> [!NOTE]
>  Solo alcune proprietà **Field** possono essere usate per tutte le origini dati. Le proprietà **Value** e **IsMissing** vengono definite per tutte le origini dati. Altre proprietà predefinite, ad esempio **Key**, **UniqueName** e **ParentUniqueName** per origini dati multidimensionali, sono supportate solo se sono disponibili nell'origine dati. Le proprietà personalizzate sono supportate da alcuni provider di dati. Per altre informazioni, vedere gli argomenti specifici sulle proprietà di campo estese per il tipo di origine dati in [Set di dati condivisi e incorporati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md). Ad esempio, per un'origine dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vedere [Proprietà di campo estese per un database di Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
  
##  <a name="understanding-default-expressions-for-fields"></a><a name="Defaults"></a> Informazioni sulle espressioni predefinite per i campi  
 Una casella di testo può essere un elemento di report casella di testo nel corpo del report oppure essere presente in una cella di un'area dati Tablix. Quando si collega un campo a una casella di testo, la posizione di quest'ultima determina l'espressione predefinita per il riferimento di campo. Nel corpo del report un'espressione del valore della casella di testo deve specificare un'aggregazione e un set di dati. Se nel report è presente un unico set di dati, tale espressione predefinita viene creata automaticamente. Per un campo che rappresenta un valore numerico, la funzione di aggregazione predefinita è Sum, mentre per un campo che rappresenta un valore non numerico l'aggregazione predefinita è First.  
  
 In un'area dati Tablix l'espressione predefinita del campo dipende dalle appartenenze a una riga e a un gruppo della casella di testo aggiunta al campo. Se a una casella di testo nella riga di dettaglio di una tabella viene aggiunto il campo Sales, l'espressione relativa è `[Sales]`. Se si aggiunge lo stesso campo a una casella di testo in un'intestazione del gruppo, l'espressione predefinita è `(Sum[Sales])`, poiché nell'intestazione del gruppo vengono visualizzati i valori di riepilogo per il gruppo, non quelli dettagliati. Durante l'esecuzione del report, l'elaboratore di report valuta ogni espressione e sostituisce il risultato nel report.  
  
 Per altre informazioni sulle espressioni, vedere [Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
  
##  <a name="field-data-types"></a><a name="DataTypes"></a> Tipi di dati dei campi  
 Quando si crea un set di dati, i tipi di dati dei campi nell'origine dati potrebbero non corrispondere esattamente ai tipi di dati utilizzati in un report. Ai tipi di dati possono essere applicati uno o due livelli di mapping. L'estensione per l'elaborazione dati o il provider di dati può eseguire il mapping dei tipi di dati dall'origine dati a tipi di dati CLR (Common Language Runtime). I tipi di dati restituiti dalle estensioni per l'elaborazione dei dati vengono su cui viene eseguito il mapping a un subset di tipi di dati CLR da [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Nell'origine dati i dati vengono archiviati in tipi di dati supportati dall'origine stessa. I dati presenti in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio, devono essere di un tipo supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , come **nvarchar** o **datetime**. Quando vengono recuperati dall'origine dati, i dati vengono passati attraverso un'estensione per l'elaborazione dati o un provider di dati associato al tipo di origine dati. In base all'estensione per l'elaborazione dati, i dati possono essere convertiti dai tipi utilizzati dall'origine dati in tipi di dati supportati dall'estensione per l'elaborazione dati. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa i tipi di dati supportati da Common Language Runtime (CLR) installato con [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Il provider di dati esegue il mapping di ogni colonna nel set di risultati dal tipo di dati nativo a un tipo di dati CLR di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] :  
  
 In ogni fase i dati vengono rappresentati dai tipi di dati in base a quanto descritto nell'elenco seguente.  
  
-   **Origine dati** Tipi di dati supportati dalla versione del tipo di origine dati alla quale ci si sta connettendo.  
  
     Tra i tipi di dati utilizzati in genere per un'origine dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono inclusi ad esempio **int**, **datetime** e **varchar**. I tipi di dati introdotti da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hanno aggiunto il supporto per **date**, **time**, **datetimetz** e **datetime2**. Per altre informazioni, vedere [Tipi di dati (Transact-SQL)](/previous-versions/sql/sql-server-2008/ms187752(v=sql.100)).  
  
-   **Provider di dati o estensione per l'elaborazione dati** Tipi di dati supportati dalla versione del provider di dati dell'estensione per l'elaborazione dei dati selezionata al momento della connessione all'origine dei dati. I provider di dati basati su [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] usano tipi di dati supportati da CLR. Per altre informazioni sui tipi di dati del provider di dati [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , vedere [Mapping di tipi di dati (ADO.NET)](/dotnet/framework/data/adonet/data-type-mappings-in-ado-net) e [Utilizzo dei tipi di base](/dotnet/standard/base-types/common-type-system) su MSDN.  
  
     Tra i tipi di dati supportati in genere da [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sono inclusi ad esempio **Int32** e **String**. Le date e le ore del calendario sono supportate dalla struttura **DateTime** . In [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 2.0 Service Pack 1 è stato introdotto il supporto per la struttura **DateTimeOffset** per le date con una differenza di fuso orario.  
  
    > [!NOTE]  
    >  Nel server di report vengono utilizzati i provider di dati installati e configurati nel server di report stesso. Nei client di creazione dei report in modalità di anteprima vengono utilizzate le estensioni per l'elaborazione dati installate e configurate nel computer client. È necessario eseguire il test del report sia nell'ambiente del client che nell'ambiente del server di report.  
  
-   **Elaborazione report** Tipi di dati basati sulla versione di CLR installata al momento dell'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
     I tipi di dati usati ad esempio da Elaborazione report per i nuovi tipi date e time introdotti in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] sono riportati nella tabella seguente:  
  
    |Tipo di dati SQL|Tipo di dati CLR|Descrizione|  
    |-------------------|-------------------|-----------------|  
    |**Date**|**DateTime**|Solo data|  
    |**Time**|**TimeSpan**|Solo ora|  
    |**DateTimeTZ**|**DateTimeOffset**|Data e ora con differenza di fuso orario|  
    |**DateTime2**|**DateTime**|Data e ora con millisecondi frazionari|  
  
 Per altre informazioni sui tipi di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Tipi di dati (motore di database)](/previous-versions/sql/sql-server-2008/ms187752(v=sql.100)) e [Funzioni e tipi di dati di data e ora (Transact-SQL)](/previous-versions/sql/sql-server-2008/ms186724(v=sql.100)).  
  
 Per altre informazioni sull'inclusione di riferimenti in un campo del set di dati da un'espressione, vedere [Tipi di dati nelle espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md).  
  
  
##  <a name="detecting-missing-fields-at-run-time"></a><a name="MissingFields"></a> Rilevamento dei campi mancanti in fase di esecuzione  
 Quando il report viene elaborato, il set di risultati per un set di dati potrebbe non contenere valori per tutte le colonne specificate, in quanto tali colonne non sono più disponibili nell'origine dati. È possibile usare la proprietà del campo IsMissing per verificare se i valori di un campo sono stati restituiti in fase di esecuzione. Per altre informazioni, vedere [Riferimenti alla raccolta di campi del set di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/built-in-collections-dataset-fields-collection-references-report-builder.md).  
  
  
## <a name="see-also"></a>Vedere anche  
 [Finestra di dialogo Proprietà set di dati, Campi &#40;Generatore report&#41;]()   
 [Parti del report e set di dati in Generatore report](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Set di dati condivisi e incorporati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
