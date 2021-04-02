---
title: Librerie di classi dell'elemento del report personalizzato | Microsoft Docs
description: Informazioni sulle librerie di classi dell'elemento del report personalizzato e sull'uso di esempi di codice per comprendere come usare queste classi.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, RDL
- RDL [Reporting Services], custom report items
ms.assetid: f18c5d8f-1d6b-4f0b-8657-c14896c2ce0d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d5d58066e16788dcba313bba82e77504c8394a75
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981127"
---
# <a name="custom-report-item-class-libraries"></a>Librerie di classi dell'elemento del report personalizzato
  Gli elementi del report personalizzati usano le classi dello spazio dei nomi **Microsoft.ReportDesigner**. Le classi utilizzate per implementare un elemento del report personalizzato possono essere suddivise in due categorie principali: le classi univoche progettate per supportare l'infrastruttura dell'elemento del report personalizzato e le classi wrapper gestite che incapsulano la funzionalità degli elementi RDL (Report Definition Language) rilevanti. Per un esempio di codice sull'uso di queste classi, vedere [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Esempi del prodotto SQL Server Reporting Services).  
  
## <a name="custom-report-item-infrastructure-classes"></a>Classi di infrastruttura dell'elemento del report personalizzato  
 Le classi riportate di seguito vengono utilizzate per implementare un elemento del report personalizzato.  
  
> [!NOTE]  
>  Nelle tabelle seguenti non vengono forniti elenchi completi, ma solo le proprietà e i metodi utilizzati più di frequente per ciascuna classe.  
  
### <a name="microsoftreportdesignercustomreportitemdesigner"></a>Microsoft.ReportDesigner.CustomReportItemDesigner  
 È la classe principale dell'elemento del report personalizzato. La classe principale dell'implementazione dell'elemento del report personalizzato deve ereditare da questa classe.  
  
#### <a name="public-properties"></a>Proprietà pubbliche  
  
|Proprietà|Descrizione|  
|-|-|  
|**Nome**|Nome dell'elemento del report personalizzato.|  
|**Tipo**|Tipo di elemento del report personalizzato.|  
|**CustomData**|Oggetto <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> che incapsula le proprietà dei dati dell'elemento del report personalizzato specificate in fase di progettazione.|  
|**CustomProperties**|Raccolta di proprietà personalizzate per l'elemento del report personalizzato.|  
|**Altezza**|Altezza del controllo dell'elemento del report personalizzato.|  
|**Larghezza**|Larghezza del controllo dell'elemento del report personalizzato.|  
|**Report**|Contenitore per le proprietà a livello di report, ad esempio l'elenco dei set di dati nel report.|  
|**AltReportItem**|Oggetto elemento del report alternativo, da utilizzare se il controllo di runtime di un elemento del report personalizzato non è supportato.|  
|**Style**|Proprietà di stile per l'elemento del report personalizzato.|  
|**Adornment**|Finestra dell'area di controllo utilizzata per la modifica interattiva del controllo.|  
|**Sito**|Interfaccia **ISite** del componente.|  
|**DesignerVerbCollection**|Matrice di verbi personalizzati per il menu di scelta rapida del controllo.|  
  
#### <a name="public-methods"></a>Metodi pubblici  
  
|Metodo|Descrizione|  
|-|-|  
|**BeginEdit**|Attiva la modifica interattiva per il controllo.|  
|**DoDefaultAction**|Viene chiamato quando si fa doppio clic o si preme Invio sul controllo.|  
|**EndEdit**|Disattiva la modifica interattiva per il controllo.|  
|**GetService**|Restituisce un oggetto che rappresenta un servizio.|  
|**InitializeNewComponent**|Viene chiamato quando si crea un nuovo elemento del report personalizzato.|  
|**Invalidate**|Ridisegna l'intera superficie del controllo.|  
|**OnDragEnter**<br /><br /> **OnDragDrop**|Viene chiamato quando si trascina un oggetto sul controllo.|  
|**OnPaint**|Viene chiamato in risposta all'evento **Paint**.|  
  
### <a name="microsoftreportdesignercustomreportitemattribute"></a>Microsoft.ReportDesigner.CustomReportItemAttribute  
 Attributo utilizzato per identificare il tipo di elemento del report personalizzato. Il nome deve corrispondere al valore dell' \<**Name**> attributo dell'elemento **ReportItem** nel file di configurazione progettazione report.  
  
#### <a name="public-methods"></a>Metodi pubblici  
  
|Metodo|Descrizione|  
|-|-|  
|**CustomReportItemAttribute**|Crea l'oggetto CustomReportItemAttribute.|  
  
### <a name="microsoftreportdesignerlocalizednameattribute"></a>Microsoft.ReportDesigner.LocalizedNameAttribute  
 Attributo utilizzato per specificare il nome visualizzato da utilizzare per la finestra di progettazione dell'elemento del report personalizzato.  
  
#### <a name="public-methods"></a>Metodi pubblici  
  
|Metodo|Descrizione|  
|-|-|  
|**LocalizedNameAttribute**|Crea l'oggetto LocalizedNameAttribute.|  
  
### <a name="microsoftreportdesigneradornment"></a>Microsoft.ReportDesigner.Adornment  
 La classe **Adornment** viene usata dal componente della fase di progettazione dell'elemento del report personalizzato per offrire aree esterne al rettangolo principale dell'area di progettazione. Tali aree possono gestire eventi dell'interfaccia utente, quali clic del mouse e operazioni di trascinamento della selezione.  
  
#### <a name="public-methods"></a>Metodi pubblici  
  
|Metodo|Descrizione|  
|-|-|  
|**OnShow**|Viene chiamato quando **Adornment** è attivato.|  
|**OnHide**|Viene chiamato quando **Adornment** è disattivato.|  
|**Paint**|Viene chiamato in risposta all'evento **Paint**.|  
|**OnDragEnter**<br /><br /> **OnDragOver**<br /><br /> **OnDragLeave**<br /><br /> **OnDragDrop**|Viene chiamato quando un oggetto viene trascinato in **Adornment**.|  
  
### <a name="microsoftreportdesigneradornerservice"></a>Microsoft.ReportDesigner.AdornerService  
 Questa classe viene usata per fornire una raccolta di servizi di visualizzazione usata dall'elemento del report personalizzato per supportare gli oggetti **Adornment** per il componente della fase di progettazione dell'elemento del report personalizzato.  
  
#### <a name="public-properties"></a>Proprietà pubbliche  
  
|Proprietà|Descrizione|  
|-|-|  
|**AdornerWindowBounds**|Limiti della finestra Adorner.|  
|**AdornerWindowRegion**|Area della finestra Adorner.|  
|**AdornerWindowGraphics**|Contesto grafico per la finestra Adorner.|  
  
#### <a name="public-methods"></a>Metodi pubblici  
  
|Metodo|Descrizione|  
|-|-|  
|**ComponentRectInDesignerFrame**|Restituisce i limiti del componente convertito nelle coordinate della cornice della finestra di progettazione.|  
|**InvalidateAdorner**|Invalida la finestra Adorner.|  
|**PointToAdorner**|Restituisce un punto nelle coordinate dello schermo convertito nelle coordinate della finestra Adorner.|  
  
### <a name="microsoftreportdesignerexpressioneditor"></a>Microsoft.ReportDesigner.ExpressionEditor  
 Questa classe può essere utilizzata da un controllo della fase di progettazione dell'elemento del report personalizzato per richiamare l'Editor espressioni.  
  
#### <a name="public-methods"></a>Metodi pubblici  
  
|Metodo|Descrizione|  
|-|-|  
|**EditValue**|Richiama l'Editor espressioni, inizializzato con il valore dell'oggetto specificato.|  
  
### <a name="microsoftreportdesignerifieldsdataobject"></a>Microsoft.ReportDesigner.IFieldsDataObject  
 Questa classe è una raccolta di campi di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e viene utilizzata per supportare eventi di trascinamento della selezione nell'ambiente di progettazione. Eredita da **IReportItemDataObject**.  
  
#### <a name="public-properties"></a>Proprietà pubbliche  
  
|Proprietà|Descrizione|  
|-|-|  
|**DataSetName**|Nome del set di dati contenente i campi da eliminare.|  
|**Fields**|Raccolta di campi (**Microsoft.ReportDesigner.Field**) da rilasciare.|  
  
## <a name="see-also"></a>Vedere anche  
 [Report Definition Language &#40;SSRS&#41;](../../reporting-services/reports/report-definition-language-ssrs.md)   
 [Creazione di un elemento del report personalizzato Run-Time componente](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Creazione di un componente dell'elemento del report personalizzato per la fase di progettazione](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)  
  
  
