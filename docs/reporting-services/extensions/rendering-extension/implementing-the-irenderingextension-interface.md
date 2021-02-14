---
title: Implementazione dell'interfaccia IRenderingExtension | Microsoft Docs
description: Informazioni su come implementare l'interfaccia IRenderingExtension, che trasforma i dati del report in formati di output utilizzati da visualizzatori, stampanti e altre destinazioni.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- IRenderingExtension interface
- rendering extensions [Reporting Services], IRenderingExtension interface
ms.assetid: 74b2f2b7-6796-42da-ab7d-b05891ad4001
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 984775286d4f32d0c1e844bc6ba0ca62bcf198ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042181"
---
# <a name="implementing-the-irenderingextension-interface"></a>Implementazione dell'interfaccia IRenderingExtension
  L'estensione per il rendering prende i risultati da una definizione del report combinata con i dati effettivi ed eseguire il rendering dei dati risultanti in un formato che sia possibile utilizzare. La trasformazione dei dati combinati e della formattazione viene eseguita tramite una classe CLR (Common Language Runtime) che implementa <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension>. Ciò consente di trasformare il modello a oggetti in un formato di output che può essere utilizzato da un visualizzatore, una stampante o un'altra destinazione di output.  
  
 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension> dispone di tre metodi che devono essere codificati:  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A>: consente di eseguire il rendering del report.  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.RenderStream%2A>: consente di eseguire il rendering di un flusso specifico dal report.  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A>: consente di ottenere informazioni aggiuntive, come icone, necessarie per il report.  
  
 Nelle sezioni seguenti questi metodi vengono descritti in modo più dettagliato.  
  
## <a name="render-method"></a>Metodo Render  
 Il metodo <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> contiene argomenti che rappresentano gli oggetti seguenti:  
  
-   Il *report* di cui si desidera eseguire il rendering. Questo oggetto contiene proprietà, dati e informazioni sul layout per il report. Il report costituisce la radice dell'albero del modello a oggetti del report.  
  
-   L'oggetto *ServerParameters* contenente l'oggetto dizionario di stringhe con i parametri per il server di report, se disponibili.  
  
-   Il parametro *deviceInfo* contenente le impostazioni dei dispositivi. Per altre informazioni, vedere [Passaggio delle impostazioni relative alle informazioni sul dispositivo alle estensioni per il rendering](../../../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md).  
  
-   Il parametro *clientCapabilities* contenente un oggetto dizionario <xref:System.Collections.Specialized.NameValueCollection> con informazioni sul client in cui viene eseguito il rendering.  
  
-   L'oggetto *RenderProperties* contenente le informazioni sul risultato del rendering.  
  
-   *createAndRegisterStream* è una funzione di delegato che deve essere chiamata per ottenere un flusso in cui eseguire il rendering.  
  
### <a name="deviceinfo-parameter"></a>Parametro deviceInfo  
 Il parametro *deviceInfo* contiene i parametri di rendering, non i parametri del report. Questi parametri vengono passati all'estensione per il rendering. I valori di *deviceInfo* vengono convertiti in un oggetto <xref:System.Collections.Specialized.NameValueCollection> dal server di report. Gli elementi inclusi nel parametro *deviceInfo* vengono trattati come valori senza distinzione tra maiuscole e minuscole. Se la richiesta di rendering deriva da un accesso con URL, i parametri URL nel formato `rc:key=value` vengono convertiti in coppie chiave/valore nell'oggetto dizionario *deviceInfo*. Il codice di rilevamento del browser fornisce anche gli elementi seguenti nel dizionario *clientCapabilities*: EcmaScriptVersion, JavaScript, MajorVersion, MinorVersion, Win32, Type e AcceptLanguage. Le coppie nome/valore incluse nel parametro *deviceInfo* che non vengono comprese dall'estensione per il rendering verranno ignorate. Nell'esempio di codice seguente viene illustrato un metodo <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> di esempio per il recupero delle icone:  
  
```csharp  
public void GetRenderingResource (CreateStream createStreamCallback, NameValueCollection deviceInfo)  
{  
    string[] iconTagValues = deviceInfo.GetValues("Icon");  
    if ((iconTagValues != null) && (iconTagValues.Length > 0) )  
    {  
        // Create a stream to output to.  
        Stream outputStream = createStreamCallback(m_iconResourceName, "gif", null, "image/gif", false);  
        // Get the GIF image for one of the buttons on the toolbar  
        Image requiredImage = (Image) m_resourcemanager.GetObject(m_iconResourceName  
        // Write the image to the output stream  
        requiredImage.Save(outputStream, requiredImage.RawFormat);  
    }  
    return;  
}  
```  
  
## <a name="renderstream-method"></a>Metodo RenderStream  
 Il metodo <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.RenderStream%2A> consente di eseguire il rendering di un flusso specifico dal report. Tutti i flussi vengono creati durante la chiamata a <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> iniziale, ma i flussi non vengono inizialmente restituiti al client. Questo metodo viene utilizzato per flussi secondari, ad esempio immagini nel rendering HTML o pagine aggiuntive di un'estensione per il rendering di più pagine, come Image/EMF.  
  
## <a name="getrenderingresource-method"></a>Metodo GetRenderingResource  
 Il metodo <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> consente di recuperare le informazioni senza eseguire l'intero rendering del report. In alcuni casi, il report necessita di informazioni che non richiedono il rendering del report stesso. Se, ad esempio, è necessaria l'icona associata all'estensione per il rendering, usare il parametro *deviceInfo* contenente il tag singolo **\<Icon>** . In questi casi, è possibile utilizzare il metodo <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A>.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementazione di un'estensione per il rendering](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Panoramica delle estensioni per il rendering](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)  
  
  
