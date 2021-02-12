---
title: Usare l'API SOAP nelle applicazioni Web
description: È possibile accedere alle funzionalità del server di report tramite l'API SOAP di Reporting Services, a cui è possibile accedere per fornire funzionalità di creazione di report aziendali.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- SOAP [Reporting Services], Web applications
- impersonation [Reporting Services]
- user impersonation [Reporting Services]
- report servers [Reporting Services], SOAP
- Web applications [Reporting Services]
ms.assetid: e8ca4455-0dc3-4741-8872-3636114938ad
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8a8f6182e4579d30cfcb4a8102a439bb7b12c7a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071144"
---
# <a name="integrating-reporting-services-using-soap---web-application"></a>Integrazione di Reporting Services tramite SOAP - Applicazione Web
  È possibile accedere alle funzionalità complete del server di report tramite l'API SOAP di Reporting Services. L'API SOAP è un servizio Web e, in quanto tale, è possibile accedervi in modo semplice per fornire caratteristiche di creazione di report aziendali alle applicazioni aziendali personalizzate. È possibile accedere al servizio Web ReportServer da un'applicazione Web nello stesso modo in cui si accede all'API SOAP da un'applicazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Usando [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], è possibile generare una classe proxy che espone le proprietà e i metodi del servizio Web ReportServer e consente di usare un'infrastruttura e strumenti familiari per compilare applicazioni aziendali basate sulla tecnologia [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 È possibile accedere alla funzionalità di gestione dei report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con la stessa facilità da un'applicazione Web o da un'applicazione Windows. Da un'applicazione Web, è possibile aggiungere e rimuovere gli elementi al e dal database del server di report, impostare la sicurezza degli elementi, modificare gli elementi del database del server di report, gestire le pianificazione e il recapito e altro ancora.  
  
## <a name="enabling-impersonation"></a>Abilitazione della rappresentazione  
 Il primo passaggio per la configurazione dell'applicazione Web consiste nell'attivare la rappresentazione dal client del servizio Web. Con la rappresentazione, le applicazioni [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] possono venire eseguite con l'identità del client per il quale operano. [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] si basa su [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services (IIS) per autenticare l'utente e passare un token autenticato all'applicazione [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] o, nel caso in cui non sia possibile autenticare l'utente, passare un token non autenticato. In entrambi i casi, l'applicazione [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] rappresenta il token ricevuto, se la rappresentazione è abilitata. È possibile abilitare la rappresentazione nel client modificando il file Web.config dell'applicazione client come indicato di seguito:  
  
```asp.net  
<!-- Web.config file. -->  
<identity impersonate="true"/>  
```  
  
> [!NOTE]  
>  Per impostazione predefinita, la rappresentazione è disabilitata.  
  
 Per altre informazioni sulla rappresentazione in [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)], vedere la documentazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  
  
## <a name="managing-the-report-server-using-soap-api"></a>Gestione del server di report tramite l'API SOAP  

::: moniker range="=sql-server-2016"

 È inoltre possibile utilizzare l'applicazione Web per gestire un server di report e i relativi contenuti. Gestione report, disponibile con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], è un esempio di applicazione Web compilata completamente utilizzando [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] e l'API SOAP di Reporting Services. È possibile aggiungere le funzionalità di Gestione report alle applicazioni Web personalizzate. È ad esempio possibile fare in modo che venga restituito un elenco di report disponibili nel database del server di report e visualizzarli in un controllo [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] **Listbox** per consentire agli utenti di scegliere i report dall'elenco. Nel codice seguente viene eseguita la connessione al database del server di report e viene restituito un elenco di elementi disponibili nel database del server di report. I report disponibili vengono aggiunti quindi a un controllo ListBox, in cui viene visualizzato il percorso di ogni report.  

::: moniker-end

::: moniker range=">=sql-server-2017"

 È inoltre possibile utilizzare l'applicazione Web per gestire un server di report e i relativi contenuti. Il portale Web, incluso in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], è un esempio di applicazione Web che gestisce la maggior parte delle attività che in genere vengono eseguite usando Reporting Services. È possibile aggiungere le funzionalità di gestione report del portale Web alle applicazioni Web personalizzate. È ad esempio possibile fare in modo che venga restituito un elenco di report disponibili nel database del server di report e visualizzarli in un controllo [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] **Listbox** per consentire agli utenti di scegliere i report dall'elenco. Nel codice seguente viene eseguita la connessione al database del server di report e viene restituito un elenco di elementi disponibili nel database del server di report. I report disponibili vengono aggiunti quindi a un controllo ListBox, in cui viene visualizzato il percorso di ogni report.  

::: moniker-end
  
```vb  
Private Sub Page_Load(sender As Object, e As System.EventArgs)  
   ' Create a Web service proxy object and set credentials  
   Dim rs As New ReportingService2005()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return a list of catalog items in the report server database  
   Dim items As CatalogItem() = rs.ListChildren("/", True)  
  
   ' For each report, display the path of the report in a Listbox  
   Dim ci As CatalogItem  
   For Each ci In  items  
      If ci.Type = ItemTypeEnum.Report Then  
         catalogListBox.Items.Add(ci.Path)  
      End If  
   Next ci  
End Sub ' Page_Load   
```  
  
```csharp  
private void Page_Load(object sender, System.EventArgs e)  
{  
   // Create a Web service proxy object and set credentials  
   ReportingService2005 rs = new ReportingService2005();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return a list of catalog items in the report server database  
   CatalogItem[] items = rs.ListChildren("/", true);  
  
   // For each report, display the path of the report in a Listbox  
   foreach(CatalogItem ci in items)  
   {  
      if (ci.Type == ItemTypeEnum.Report)  
         catalogListBox.Items.Add(ci.Path);  
   }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  

 [Compilazione di applicazioni tramite servizio Web e .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Integrazione di Reporting Services nelle applicazioni](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Uso dell'API SOAP in un'applicazione Windows](../../reporting-services/application-integration/integrating-reporting-services-using-soap-windows-application.md)  
  
  
