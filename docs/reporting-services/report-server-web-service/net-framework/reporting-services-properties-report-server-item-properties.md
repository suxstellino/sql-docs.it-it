---
title: Proprietà degli elementi del server di report | Microsoft Docs
description: Le proprietà degli elementi sono specifiche degli elementi del database del server di report. Tali elementi includono report, report collegati, cartelle, risorse, modelli e origini dati.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- report servers [Reporting Services], properties
- item-specific properties [Reporting Services]
- report items [Reporting Services], properties
- items [Reporting Services], properties
ms.assetid: 21edec6d-9897-48fb-8c75-182305b1dbdb
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4d4db0c4bdef91630cd2bf20b215f7a6b26d714b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063659"
---
# <a name="reporting-services-properties---report-server-item-properties"></a>Proprietà di Reporting Services - Proprietà degli elementi del server di report
  Le proprietà degli elementi sono proprietà specifiche degli elementi del database del server di report. Tali elementi includono report, report collegati, cartelle, risorse, modelli e origini dati.  
  
 I nomi di proprietà degli elementi seguenti sono riservati. Non è possibile creare proprietà definite dall'utente con lo stesso nome. È possibile leggere o modificare numerose di queste proprietà usando i metodi del servizio Web ReportServer.  
  
## <a name="item-properties"></a>Proprietà degli elementi  
 Le proprietà seguenti si applicano a tutti gli elementi nel database del server di report.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|**CreatedBy**|Nome dell'utente che ha originariamente aggiunto l'elemento al database del server di report.|  
|**CreationDate**|Data e ora di aggiunta dell'elemento al database del server di report.|  
|**Descrizione**|Descrizione dell'elemento.|  
|**Hidden**|Valore che indica se l'elemento è visibile e disponibile per gli utenti.|  
|**ID**|ID di un elemento nel database del server di report.|  
|**ModifiedBy**|Nome dell'utente che ha apportato l'ultima modifica all'elemento nel database del server di report.|  
|**ModifiedDate**|Data e ora dell'ultima modifica dell'elemento.|  
|**Nome**|Nome di un elemento nel database del server di report.|  
|**Percorso**|Percorso completo dell'elemento. Il percorso di un elemento nel database del server di report può essere composto da un massimo di 260 caratteri.|  
|**Dimensione**|Dimensione, in byte, di un elemento nel database del server di report.|  
|**Tipo**|Tipo di un elemento nel database del server di report.|  
|**VirtualPath**|Percorso virtuale di un elemento nel database del server di report. Il valore della proprietà <xref:ReportService2010.CatalogItem.VirtualPath%2A> indica il percorso in cui un utente può trovare l'elemento. Il percorso virtuale di un report denominato report1 che si trova nella cartella Report personali dell'utente è ad esempio /Report personali. Il percorso effettivo dell'elemento è /Utenti/nomeutente/Report personali.|  
  
## <a name="folder-properties"></a>Proprietà delle cartelle  
 Oltre alle proprietà degli elementi elencate in precedenza, la proprietà seguente si applica alle cartelle nel database del server di report.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|**Reserved**|Valore restituito dal metodo <xref:ReportService2010.ReportingService2010.GetProperties%2A> per le cartelle riservate dal server di report. Le cartelle riservate includono Utenti, Report personali e /. Le cartelle riservate non possono essere modificate o rimosse.|  
  
## <a name="report-properties"></a>Proprietà dei report  
 Oltre alle proprietà degli elementi elencate in precedenza, le proprietà seguenti si applicano ai report nel database del server di report.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|**Lingua**|Lingua usata in un report. Il valore è un codice della lingua definito nella specifica Internet Engineering Task Force (IETF) RFC1766. La prima parte è costituita da una designazione di due caratteri della lingua di base. La seconda parte è separata da un trattino e definisce la variazione o il sottolinguaggio della lingua. Se un valore non viene specificato nell'elemento **Style** associato all'elemento **Body** nella definizione del report, il valore predefinito è costituito dalla lingua del server di report.|  
|**ReportProcessingTimeout**|Timeout, in secondi, per un singolo report. Se questo valore è impostato, il server di report tenta di arrestare l'elaborazione di un report quando scade il tempo specificato. I valori validi sono compresi tra **-1** e **2**,**147**,**483**,**647**. Se il valore è **-1**, durante l'elaborazione non si verifica il timeout del report. Se il valore è **null**, per il timeout per l'elaborazione del report viene usato il valore della proprietà di sistema **ReportProcessingTimeout**. Il valore predefinito è **null**. Per altre informazioni, vedere [Proprietà di sistema del server di report](../../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).|  
|**ExecutionDate**|Data e ora dell'ultima creazione di uno snapshot del report.|  
|**CanRunUnattended**|Valore che indica se un report può essere eseguito automaticamente in base a una pianificazione. Se questa proprietà è impostata su **true**, vengono definiti i valori predefiniti per i parametri del report e le credenziali dell'origine dati vengono archiviate con il report oppure l'opzione per il recupero delle credenziali viene impostata su **None**. Se questa proprietà è impostata su **false**, i prerequisiti per l'esecuzione automatica di un report non sono soddisfatti. Per altre informazioni, vedere [Configurare l'account di esecuzione automatica &#40;Gestione configurazione del server di report&#41;](../../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).|  
|**HasParameterDefaultValues**|Valore che indica se per il report sono impostati valori predefiniti validi per tutti i parametri. Il valore è **true** anche se un report non dispone di parametri. Se questa proprietà è impostata su **false**, uno o più parametri del report non dispongono di un valore predefinito valido.|  
|**HasDataSourceCredentials**|Valore che indica che l'opzione per il recupero delle credenziali impostata per tutte le origini dati associate al report è **None** o **Store**. Se questa proprietà è impostata su **false**, un'opzione per il recupero delle credenziali impostata per una delle origini dati associate al report è **Integrate** o **Prompt**.|  
|**IsSnapshotExecution**|Valore che indica se il report è uno snapshot.|  
|**HasScheduleReadyDataSources**|Valore che indica se le origini dati di un report sono configurate per supportare l'esecuzione pianificata. Se questa proprietà è impostata su **false**, gli utenti non possono sottoscrivere il report.|  
  
## <a name="resource-properties"></a>Proprietà delle risorse  
 Oltre alle proprietà degli elementi elencate in precedenza, la proprietà seguente si applica alle risorse nel database del server di report.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|**MimeType**|Tipo MIME di una risorsa nel database del server di report.|  
  
## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni tramite servizio Web e .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Servizio Web ReportServer](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Riferimento tecnico &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
