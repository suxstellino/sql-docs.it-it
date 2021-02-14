---
title: Elemento HelpLink | Microsoft Docs
description: Informazioni dettagliate sull'elemento HelpLink della proprietà Detail, inclusi gli argomenti dell'URL HelpLink.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- HelpLink element
- SoapException class
ms.assetid: a4489103-a874-44c2-8f75-95cb238928ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cfbf38148868f3f95a861e04227272db9c46692a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021821"
---
# <a name="helplink-element"></a>Elemento HelpLink
  L'elemento **HelpLink** della proprietà **Detail** è una stringa URL generata dal server di report. L'URL rimanda a una pagina Web gestita dal Supporto tecnico [!INCLUDE[msCoName](../../../includes/msconame-md.md)] e fornisce ulteriori informazioni e articoli della Knowledge Base su errori specifici che si verificano in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. La sintassi dell'URL è la seguente:  
  
 **https://** www\.microsoft.com **/** products **/** ee **/** transform.aspx **?EvtSrc**=v_alue_ **&EvtID**=_value_ **&ProdName**=_value_ **&ProdVer**=*value*  
  
 Nella tabella seguente sono elencati gli argomenti dell'URL **HelpLink**.  
  
|Argomento|valore|  
|--------------|-----------|  
|**EvtSrc**|"Microsoft.ReportingServices.Diagnostics.ErrorStrings.resources.Strings"|  
|**EvtID**|Codice di errore del server di report, ad esempio rsReservedItem.|  
|**ProdName**|"Microsoft SQL%20Server%20Reporting%20Services". Il valore del nome del prodotto è codificato nell'URL.|  
|**ProdVer**|Numero di versione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Il valore "8.00" indica [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
  
 Nell'esempio seguente viene illustrato l'URL **HelpLink** che viene restituito per il codice di errore **rsReservedItem**. Questo errore si verifica quando un utente tenta di modificare o di eliminare un elemento riservato in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]:  
  
```  
https://www.microsoft.com/products/ee/transform.aspx?  
EvtSrc=Microsoft.ReportingServices.Diagnostics.ErrorStrings.resources.Strings  
&EvtID=rsReservedItem&ProdName=Microsoft%20SQL%20Server%20Reporting%20Services&ProdVer=8.00  
```  
  
 È possibile accedere all'elemento **HelpLink** nel codice usando la classe **SoapException**.  
  
```vb  
Try  
   rs.DeleteItem("/Report1")  
  
Catch e As SoapException  
   Console.WriteLine(e.Detail("HelpLink").InnerXml)  
End Try  
```  
  
```csharp  
try  
{  
   rs.DeleteItem("/Report1");  
}  
  
catch (SoapException e)  
{  
   Console.WriteLine(e.Detail["HelpLink"].InnerXml);  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione alla gestione delle eccezioni in Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Classe SoapException di Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)   
 [Uso della proprietà Detail per la gestione di errori specifici](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/using-the-detail-property-to-handle-specific-errors.md)  
  
  
