---
title: Programmabilità della web part Visualizzatore di report nell'integrazione con SharePoint | Microsoft Docs
description: Informazioni su come creare web part personalizzate che forniscono parametri e percorsi di report alla web part Visualizzatore report tramite connessioni web part.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: reference
ms.assetid: 714017b7-1bd6-4950-a3c6-d0df8450a877
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d7054e2af081bd76505f3adebbae4cc7541f8a44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023275"
---
# <a name="report-viewer-web-part-programmability-in-sharepoint-integration"></a>Programmabilità della web part Visualizzatore report nell'integrazione con SharePoint
  La web part Visualizzatore di report è un controllo server contenente un set di API pubbliche che consentono agli sviluppatori di creare applicazioni di SharePoint personalizzate. È possibile creare web part personalizzate che forniscono parametri e percorsi di report a web part Visualizzatore report tramite connessioni web part. È inoltre possibile incorporare la web part in una pagina web part di SharePoint personalizzata e personalizzarla usando l'API pubblica.  
  
## <a name="connecting-to-report-viewer-web-part-with-custom-web-parts"></a>Connessione a web part Visualizzatore report con web part personalizzate  
 La web part Visualizzatore di report è una connessione dell'utente a web part di SharePoint che implementano <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> o T:Microsoft.SharePoint.WebPartPages.IFilterValues. Una web part <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>, ad esempio la web part **Documenti**, può fornire il percorso di un report a una web part Visualizzatore di report quando viene inserita nella stessa pagina web part della web part Visualizzatore di report. In modo analogo, una web part T:Microsoft.SharePoint.WebPartPages.IFilterValues, ad esempio **Filtro testo** o **Filtro scelte**, può fornire un parametro del report a una web part Visualizzatore di report quando viene inserita nella stessa pagina web part della web part Visualizzatore di report.  
  
### <a name="implementing-a-report-path-provider-with-iwebpartrow"></a>Implementazione di un provider del percorso report con IWebPartRow  
 Per fornire il percorso di un report a una web part Visualizzatore report tramite connessioni web part, eseguire le operazioni seguenti:  
  
1.  Creare una web part che implementa l'interfaccia <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>.  
  
2.  Aggiungere la web part alla stessa pagina web part della web part Visualizzatore report.  
  
3.  Connettere la web part alla web part Visualizzatore report nell'interfaccia utente di progettazione delle web part basata sul Web.  
  
    > [!NOTE]  
    >  Alla web part Visualizzatore di report è possibile connettere una sola web part <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> alla volta e non è possibile connettervi contemporaneamente una web part <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> e una web part T:Microsoft.SharePoint.WebPartPages.IFilterValues.  
  
 Per consentire il funzionamento corretto della web part <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> con la web part T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewer, è necessario eseguire le operazioni seguenti nel metodo <xref:System.Web.UI.WebControls.WebParts.IWebPartRow.GetRowData%2A>:  
  
-   Richiamare il metodo di callback con un oggetto <xref:System.Data.DataRowView> come parametro di input.  
  
-   Verificare che l'oggetto <xref:System.Data.DataRowView> contenga una colonna denominata "DocUrl" contenente il percorso del report.  
  
    > [!NOTE]  
    >  La web part Visualizzatore report nel componente aggiuntivo per [!INCLUDE[offSPServ](../includes/offspserv-md.md)] supporta inoltre la ricezione del percorso del report tramite la colonna "FileRef".  
  
### <a name="implementing-a-report-parameter-provider-with-ifiltervalues"></a>Implementazione del provider di un parametro di report con IFilterValues  
 Una web part che implementa T:Microsoft.SharePoint.WebPartPages.IFilterValues può fornire un unico parametro di report alla web part Visualizzatore di report. Il valore del parametro inviato alla web part Visualizzatore report è soggetto alle stesse restrizioni esistenti per il parametro del report come specificato nella definizione del report, ad esempio tipo di dati, valori validi e così via.  
  
 Per fornire un parametro di report a una web part Visualizzatore report, eseguire le operazioni seguenti:  
  
1.  Creare una web part che implementa l'interfaccia T:Microsoft.SharePoint.WebPartPages.IFilterValues.  
  
2.  Aggiungere la web part alla stessa pagina della web part T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewer.  
  
3.  Connettere la web part T:Microsoft.SharePoint.WebPartPages.IFilterValues alla web part Visualizzatore di report nell'interfaccia utente di progettazione delle web part basata sul Web.  
  
    > [!NOTE]  
    >  È possibile connettere più web part T:Microsoft.SharePoint.WebPartPages.IFilterValues alla web part Visualizzatore di report. Tuttavia, non è possibile connettere contemporaneamente una web part <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> e una web part T:Microsoft.SharePoint.WebPartPages.IFilterValues alla web part Visualizzatore di report.  
  
  
