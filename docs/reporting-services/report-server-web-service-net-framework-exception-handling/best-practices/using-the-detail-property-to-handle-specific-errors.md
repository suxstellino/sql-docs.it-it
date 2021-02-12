---
title: Using the Detail Property to Handle Specific Errors | (Uso della proprietà Detail per la gestione di errori specifici) | Microsoft Docs
description: Informazioni su come accedere al testo interno dell'elemento figlio Message usando la proprietà Detail per gestire errori specifici.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- exceptions [Reporting Services], Detail property
- Detail property
- InnerText property
ms.assetid: 4392633d-b46b-41e6-bc12-efb64e166704
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d22bfdf0659b17e7730c25aef7369d4fa323bce6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023457"
---
# <a name="using-the-detail-property-to-handle-specific-errors"></a>Utilizzo della proprietà Detail per la gestione di errori specifici
  Per classificare ulteriormente le eccezioni, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] restituisce informazioni aggiuntive sull'errore nella proprietà **InnerText** degli elementi figlio nella proprietà **Detail** dell'eccezione SOAP. Poiché la proprietà **Detail** è un oggetto **XmlNode**, è possibile accedere al testo interno dell'elemento figlio **Message** usando il codice seguente.  
  
 Per un elenco di tutti gli elementi figlio disponibili inclusi nella proprietà **Detail**, vedere [Detail Property](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/detail-property.md). Per altre informazioni, vedere l'argomento "Proprietà Detail" nella documentazione di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK.  
  
```vb  
Try  
' Code for accessing the report server  
Catch ex As SoapException  
   ' The exception is a SOAP exception, so use  
   ' the Detail property's Message element.  
   Console.WriteLine(ex.Detail("Message").InnerXml)  
End Try  
```  
  
```csharp  
try  
{  
   // Code for accessing the report server  
}  
catch (SoapException ex)  
{  
   // The exception is a SOAP exception, so use  
   // the Detail property's Message element.  
   Console.WriteLine(ex.Detail["Message"].InnerXml);  
}  
```  
  
```vb  
Try  
' Code for accessing the report server  
Catch ex As SoapException  
   If ex.Detail("ErrorCode").InnerXml = "rsInvalidItemName" Then  
   End If ' Perform an action based on the specific error code  
End Try  
```  
  
```csharp  
try  
{  
   // Code for accessing the report server  
}  
catch (SoapException ex)  
{  
   if (ex.Detail["ErrorCode"].InnerXml == "rsInvalidItemName")  
   {  
      // Perform an action based on the specific error code  
   }  
}  
```  
  
 Tramite la riga di codice seguente viene scritto il codice di errore specifico restituito nell'eccezione SOAP alla console. È anche possibile valutare il codice di errore ed eseguire azioni specifiche.  
  
```vb  
Console.WriteLine(ex.Detail("ErrorCode").InnerXml)  
```  
  
```csharp  
Console.WriteLine(ex.Detail["ErrorCode"].InnerXml);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione alla gestione delle eccezioni in Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Classe SoapException di Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)   
 [Tabella degli errori SoapException](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/soapexception-errors-table.md)  
  
  
