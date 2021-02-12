---
title: Gestione di avvisi e casi che non causano eccezioni | Microsoft Docs
description: Informazioni su come gestire gli avvisi e i casi che non causano eccezioni in modo da poter eseguire un'azione appropriata.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- exceptions [Reporting Services], warnings that don't cause
- warnings [Reporting Services]
ms.assetid: 475c0713-6265-44e7-9ebc-ebdd1b89e0af
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 41fce911539fdae4e1d0655fe488fa1e0bc7ee9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023630"
---
# <a name="handling-warnings-and-cases-that-do-not-cause-exceptions"></a>Gestione di avvisi e casi che non provocano eccezioni
  In [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] non vengono generate eccezioni per gli avvisi e per determinati errori. Quando, ad esempio, si utilizza il metodo <xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A> per pubblicare un nuovo report in un server di report, gli avvisi vengono restituiti come matrice di oggetti <xref:ReportService2010.Warning>. Questi avvisi devono essere gestiti e visualizzati in modo che sia possibile eseguire l'azione appropriata.  
  
```vb  
Try  
   rs.CreateCatalogItem(name, parentFolder, False, definition, Nothing, warnings)  
  
   If Not (warnings.Length = 0) Then  
      Dim warning As Warning  
      For Each warning In warnings  
         Console.WriteLine(warning.Message)  
      Next warning  
   Else  
      Console.WriteLine("Report {0} created successfully with no warnings", name)  
   End If  
  
Catch ex As SoapException  
   Console.WriteLine(ex.Detail("Message").InnerXml)  
End Try  
```  
  
```csharp  
try  
{  
   rs.CreateCatalogItem("Report", name, parentFolder, false, definition, null, out warnings);  
  
   if (warnings.Length != 0)  
   {  
      foreach (Warning warning in warnings)  
      {  
         Console.WriteLine(warning.Message);  
      }  
   }  
   else  
      Console.WriteLine("Report {0} created successfully with no warnings", name);  
}  
  
catch (SoapException ex)  
{  
   Console.WriteLine(ex.Detail["Message"].InnerXml);  
}  
```  
  
 Un altro modo per gestire gli errori consiste nel valutare i valori restituiti di determinati metodi. È ad esempio possibile utilizzare il metodo <xref:ReportService2010.ReportingService2010.FindItems%2A> per cercare elementi specifici nel database del server di report. Se non vengono trovati elementi corrispondenti ai criteri di ricerca, viene restituita una matrice Null di oggetti <xref:ReportService2010.CatalogItem>. È necessario valutare la matrice, verificare la presenza di **Null** e comunicare all'utente se non sono stati trovati elementi.  
  
## <a name="see-also"></a>Vedere anche  
 <xref:ReportService2010.CatalogItem>   
 [Introduzione alla gestione delle eccezioni in Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Classe SoapException di Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)  
  
  
