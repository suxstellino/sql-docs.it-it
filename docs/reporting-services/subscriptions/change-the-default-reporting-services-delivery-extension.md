---
title: Modificare l'estensione per il recapito predefinita di Reporting Services | Microsoft Docs
description: Informazioni su come configurare le impostazioni di Reporting Services per riordinare le estensioni per il recapito mostrate nell'elenco "Recapito" e per impostare l'estensione per il recapito predefinita.
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- Report Manager [Reporting Services], default delivery extension
ms.assetid: 5f6fee72-01bf-4f6c-85d2-7863c46c136b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e5a382e27c1c0a03de7ee388df4e23b263916377
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076502"
---
# <a name="change-the-default-reporting-services-delivery-extension"></a>Modificare l'estensione per il recapito predefinita di Reporting Services
  È possibile modificare le impostazioni di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per modificare l'estensione per il recapito predefinita visualizzata nell'elenco **Recapito** di una pagina di definizione della sottoscrizione. Ad esempio, è possibile modificare la configurazione in modo che, quando viene creata una nuova sottoscrizione, il recapito della condivisione file venga selezionato per impostazione predefinita al posto del recapito tramite posta elettronica. È inoltre possibile modificare l'ordine con cui sono elencate le estensioni per il recapito nell'interfaccia utente.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  Modalità nativa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] | Modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] include le estensioni per il recapito tramite posta elettronica e condivisione file di Windows. Nel server di report potrebbero essere disponibili ulteriori estensioni per il recapito, se sono state distribuite estensioni personalizzate o di terze parti per supportare funzionalità di recapito particolari. Un'estensione per il recapito è disponibile se è distribuita in un server di report.  
  
## <a name="default-native-mode-report-server-configuration"></a>Configurazione dei server di report con modalità nativa predefinita  
 L'ordine con cui un'estensione per il recapito viene visualizzata nell'elenco **Recapito** di Gestione report dipende dall'ordine delle voci dell'estensione presenti nel file **RSReportServer.config** . Ad esempio, nell'immagine seguente Posta elettronica è visualizzato per primo ed è selezionato per impostazione predefinita.  
  
 ![elenco predefinito di estensioni per il recapito](../../reporting-services/subscriptions/media/ssrs-default-delivery.png "elenco predefinito di estensioni per il recapito")  
  
 Di seguito è riportata la sezione predefinita **RSReportServer.config** che controlla l'estensione per il recapito predefinita e l'ordine di visualizzazione in Gestione Report. Si noti che Posta elettronica viene visualizzato per primo nel file ed è impostato come predefinito.  
  
```  
<DeliveryUI>  
     <Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailDeliveryProviderControl,ReportingServicesEmailDeliveryProvider">  
          <DefaultDeliveryExtension>True</DefaultDeliveryExtension>  
               <Configuration>  
               <RSEmailDPConfiguration>  
                    <DefaultRenderingExtension>MHTML</DefaultRenderingExtension>  
               </RSEmailDPConfiguration>  
               </Configuration>  
     </Extension>  
     <Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareUIControl,ReportingServicesFileShareDeliveryProvider"/>  
</DeliveryUI>  
```  
  
#### <a name="configure-file-share-delivery-as-the-default-delivery-extension-in-report-manager"></a>Configurare il recapito della condivisione file come estensione per il recapito predefinita in Gestione report  
  
1.  I passaggi descritti in questa procedura consentono di modificare la configurazione in modo che il recapito tramite condivisione file venga elencato come prima opzione nell'interfaccia utente e sia la selezione predefinita.  
  
     Aprire il file RSReportServer.config in un editor di testo. Per altre informazioni sul file di configurazione, vedere [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md). Dopo la modifica della configurazione, l'interfaccia utente sarà simile all'immagine seguente:  
  
     ![elenco modificato di estensioni per il recapito](../../reporting-services/subscriptions/media/ssrs-modified-delivery.png "elenco modificato di estensioni per il recapito")  
  
2.  Modificare la sezione DeliveryUI in modo che somigli all'esempio seguente e prendere nota delle principali modifiche:  
  
    1.  L'estensione FileShare precede l'estensione per la posta elettronica. Verrà modificato l'ordine di visualizzazione delle estensioni in Gestione Report.  
  
    2.  L'estensione per la condivisione file contiene il tag DefaultExtension `<DefaultDeliveryExtension>True</DefaultDeliveryExtension>` ed è stato aggiunto il tag di fine estensione `</Extension>`.  
  
    3.  L'estensione per la posta elettronica non è più l'impostazione predefinita. `<DefaultDeliveryExtension>False</DefaultDeliveryExtension>`  
  
    ```  
    <DeliveryUI>  
         <Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareUIControl,ReportingServicesFileShareDeliveryProvider">  
              <DefaultDeliveryExtension>True</DefaultDeliveryExtension>  
         </Extension>  
         <Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailDeliveryProviderControl,ReportingServicesEmailDeliveryProvider">  
         <DefaultDeliveryExtension>False</DefaultDeliveryExtension>  
         <Configuration>  
              <RSEmailDPConfiguration>  
                   <DefaultRenderingExtension>MHTML</DefaultRenderingExtension>  
              </RSEmailDPConfiguration>  
         </Configuration>  
         </Extension>  
    </DeliveryUI>  
    ```  
  
3.  Salvare il file di configurazione.  
  
4.  Entro pochi minuti il server di report ricarica il file di configurazione e le nuove impostazioni diventano effettive. È possibile riavviare il servizio del server di report per forzare il caricamento del file di configurazione.  
  
     Il seguente evento viene scritto nel registro eventi di Windows durante la lettura della configurazione.  
  
     **ID evento:** 109  
  
     **Origine:** Servizio del server di report di Windows (nome istanza)  
  
     Il file RSReportServer.config è stato modificato  
  
## <a name="sharepoint-mode-report-servers"></a>Server di report in modalità SharePoint  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] La modalità SharePoint archivia le informazioni delle estensioni nei database dell'applicazione del servizio SharePoint e non nel file RsrReportServer.config. In modalità SharePoint, la configurazione delle estensioni per il recapito viene modificata con PowerShell.  
  
#### <a name="configure-the-default-delivery-extension"></a>Configurare l'estensione per il recapito predefinita  
  
1.  Aprire la **shell di gestione di SharePoint**.  
  
2.  È possibile ignorare questo passaggio se si conosce già il nome dell'applicazione del servizio di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Usare le seguenti funzioni PowerShell per elencare le applicazioni del servizio di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nella farm di SharePoint.  
  
    ```  
    get-sprsserviceapplication | format-list *  
    ```  
  
3.  Eseguire la funzione PowerShell seguente per verificare l'estensione per il recapito predefinita corrente per l'applicazione di servizio "ssrsapp" di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
    ```  
    $app=get-sprsserviceapplication | where {$_.name -like "ssrsapp*"};Get-SPRSExtension -identity $app | where{$_.ServerDirectivesXML -like "<DefaultDelivery*"} | format-list *  
  
    ```
  
## <a name="see-also"></a>Vedere anche  
 [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Recapito tramite condivisione file in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Recapito tramite posta elettronica in Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md)   

