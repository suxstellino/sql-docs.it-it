---
title: Panoramica delle estensioni per il recapito | Microsoft Docs
description: Panoramica delle estensioni per il recapito, che è possibile usare per recapitare i report Reporting Services in diversi modi, ad esempio tramite posta elettronica o condivisione di file.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- subscriptions [Reporting Services], delivery extensions
- delivery extensions [Reporting Services], about extensions
ms.assetid: a30600a9-bbed-4519-9426-3470ff2982e7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8eecb8ce351fd067d944daf4fdd7fc9ee811341f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076652"
---
# <a name="delivery-extensions-overview"></a>Cenni preliminari sulle estensioni per il recapito
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] consente agli utenti di creare e pubblicare report che dopo la creazione e la pubblicazione possono essere recapitati in diverse posizioni. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] include inoltre diverse estensioni per il recapito e un'API di recapito tramite cui gli sviluppatori possono creare estensioni per il recapito aggiuntive per estendere ulteriormente le funzionalità di recapito in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 Nella tabella seguente sono elencate le estensioni per il recapito incluse in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Estensione per il recapito|Descrizione|  
|------------------------|-----------------|  
|Messaggio di posta elettronica dal server di report|Utilizza un server SMTP per inviare tramite posta elettronica i report a singoli utenti o gruppi.|  
|Condivisione file server di report|Utilizzata per distribuire i report all'interno dell'organizzazione nelle condivisioni file di rete. Consente di copiare automaticamente un report in una condivisione file in base a una pianificazione definita.|  
  
 ![Architettura delle estensioni per il recapito di Reporting Services](../../../reporting-services/extensions/delivery-extension/media/bk-reportservicedelivery.gif "Architettura delle estensioni per il recapito di Reporting Services")  
Architettura delle estensioni per il recapito di Reporting Services  
  
 Le estensioni per il recapito sono abbinate alle sottoscrizioni. Al momento della creazione di una sottoscrizione, gli utenti hanno la possibilità di scegliere una delle estensioni per il recapito disponibili che determinano il metodo di recapito del report. In [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] le sottoscrizioni si trovano nel database del server di report. Quando si verifica un evento, in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] viene trovata la corrispondenza tra l'evento e le sottoscrizioni contenute nel database del server di report. Per ogni sottoscrizione collegata all'evento, il server di report crea una notifica. Per le sottoscrizioni guidate dai dati, viene creata una notifica per ogni destinatario. Dopo aver creato la notifica, il server di report richiama una particolare estensione per il recapito e passa i valori per le impostazioni dell'estensione specificate nella notifica. L'estensione per il recapito invia la notifica all'utente come specificato dall'estensione selezionata.  
  
 Le estensioni per il recapito implementano l'API di estensione per il recapito di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Grazie al supporto dell'API di estensione per il recapito di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], le estensioni per il recapito sono in grado di ricevere notifiche dal server di report e di fornire lo stato della notifica.  
  
 Il server di report non gestisce le destinazioni di recapito per le notifiche e i report. La raccolta delle informazioni sulle destinazioni viene eseguita tramite il codice scritto nell'estensione per il recapito.  
  
## <a name="subscriptions-and-delivery-extensions"></a>Sottoscrizioni ed estensioni per il recapito  
 Le applicazioni client creano sottoscrizioni che utilizzano le estensioni per il recapito tramite due metodi del servizio Web ReportServer: <xref:ReportService2010.ReportingService2010.CreateSubscription%2A> e <xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>. Per modificare le sottoscrizioni già esistenti, vengono utilizzati i metodi <xref:ReportService2010.ReportingService2010.SetSubscriptionProperties%2A> e <xref:ReportService2010.ReportingService2010.SetDataDrivenSubscriptionProperties%2A>. Quando viene creata una sottoscrizione, l'utente seleziona anche un'estensione per il recapito per la sottoscrizione e immette i valori per le impostazioni dell'estensione necessarie. Quando un utente salva una sottoscrizione, questa viene archiviata nel database del server di report. Le sottoscrizioni creano notifiche in base a una pianificazione o a un evento. All'inizio del processo di recapito, l'estensione per il recapito selezionata carica innanzitutto i dati di configurazione dal file di configurazione. Vengono quindi recuperate le impostazioni dell'estensione per la sottoscrizione e i valori vengono impostati. Infine viene chiamato il metodo <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.Deliver%2A> e la notifica viene inviata.  
  
## <a name="developer-requirements"></a>Requisiti per lo sviluppatore  
 Per lo sviluppo di un'estensione per il recapito di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è necessario disporre di quanto segue:  
  
-   Un computer di distribuzione in cui sia installato un server di report.  
  
-   Un computer di sviluppo in cui sia installato [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] Software Development Kit (SDK).  
  
-   Conoscenza approfondita delle caratteristiche e delle caratteristiche di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], in particolare degli aspetti di sottoscrizione e recapito.  
  
-   Conoscenza approfondita di [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] e dei controlli Web, se si intende implementare un'interfaccia utente di sottoscrizione personalizzata per Gestione report.  
  
-   Esperienza di sviluppo in un linguaggio [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], ad esempio [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementazione di un'estensione per il recapito](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
