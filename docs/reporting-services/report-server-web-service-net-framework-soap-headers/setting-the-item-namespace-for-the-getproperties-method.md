---
description: Impostazione dello spazio dei nomi degli elementi per il metodo GetProperties
title: Impostazione dello spazio dei nomi degli elementi per il metodo GetProperties | Microsoft Docs
Description: Informazioni su come recuperare le proprietà in base al percorso o all'ID di un elemento usando il metodo GetProperties e l'intestazione SOAP ItemNamespaceHeader.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- item properties [Reporting Services]
- ItemNamespaceHeader SOAP header
- GetProperties method
ms.assetid: b0a08639-3101-40a2-abe2-3a41753826d1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 45edea2b8f4b489f7681ded859a4f81a06047b5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044971"
---
# <a name="setting-the-item-namespace-for-the-getproperties-method"></a>Impostazione dello spazio dei nomi degli elementi per il metodo GetProperties
  È possibile utilizzare l'intestazione SOAP <xref:ReportService2010.ItemNamespaceHeader> in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per recuperare le proprietà degli elementi in base a due identificatori diversi, ovvero il percorso completo dell'elemento o l'ID dell'elemento.  
  
 Quando si effettua una chiamata al metodo <xref:ReportService2010.ReportingService2010.GetProperties%2A>, normalmente si passa come argomento il percorso completo dell'elemento per il quale si desidera recuperare le proprietà. Tramite <xref:ReportService2010.ItemNamespaceHeader>, è possibile impostare l'intestazione SOAP per la chiamata al metodo per utilizzare <xref:ReportService2010.ReportingService2010.GetProperties%2A> passando l'ID dell'elemento come identificatore.  
  
 Nell'esempio di codice seguente vengono recuperati i valori per le proprietà dell'elemento in base all'ID dell'elemento.  
  
> [!NOTE]  
>  Per impostazione predefinita, non è necessario impostare un valore per <xref:ReportService2010.ItemNamespaceHeader> se si passa al metodo <xref:ReportService2010.ReportingService2010.GetProperties%2A> il nome di un percorso completo come identificatore dell'elemento.  
  
```vb  
Imports System  
Imports System.Collections  
  
Class Sample  
   Sub Main()  
      Dim rs As New ReportingService2010()  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
      rs.Url = "https://<Server Name>/reportserver/ReportService2010.asmx"  
  
      Dim items() As CatalogItem  
  
      Try  
         ' Need the ID property of items. Normally, you would already have   
         ' this stored somewhere.  
         items = rs.ListChildren("/AdventureWorks Sample Reports", False)  
  
         ' Set the item namespace header to be GUID-based  
         rs.ItemNamespaceHeaderValue = New ItemNamespaceHeader()  
         rs.ItemNamespaceHeaderValue.ItemNamespace = ItemNamespaceEnum.GUIDBased  
  
         ' Call GetProperties with item ID.  
         If Not (items Is Nothing) Then  
            Dim item As CatalogItem  
            For Each item In  items  
               Dim properties As [Property]() = rs.GetProperties(item.ID, Nothing)  
               Dim property As [Property]  
               For Each property In  properties  
                  Console.WriteLine(([property].Name + ": " + [property].Value))  
               Next property  
               Console.WriteLine()  
            Next item  
         End If  
  
      Catch e As Exception  
         Console.WriteLine((e.Message + ": " + e.StackTrace))  
      End Try  
   End Sub 'Main  
End Class 'Sample  
```  
  
```csharp  
using System;  
using System.Collections;  
  
class Sample  
{  
   static void Main()  
   {  
   ReportingService2010 rs = new ReportingService2010();  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
      rs.Url = "https://<Server Name>/reportserver/ReportService2010.asmx";  
  
      CatalogItem[] items;  
  
      try  
      {  
         // Need the ID property of items. Normally, you would already have   
         // this stored somewhere.  
         items = rs.ListChildren("/AdventureWorks Sample Reports", false);  
  
         // Set the item namespace header to be GUID-based  
         rs.ItemNamespaceHeaderValue = new ItemNamespaceHeader();  
         rs.ItemNamespaceHeaderValue.ItemNamespace = ItemNamespaceEnum.GUIDBased;  
  
         // Call GetProperties with item ID.  
         if (items != null)  
         {  
            foreach( CatalogItem item in items)  
            {  
               Property[] properties = rs.GetProperties(item.ID, null);  
               foreach (Property property in properties)  
               {  
                  Console.WriteLine(property.Name + ": " + property.Value);  
               }  
               Console.WriteLine();  
            }  
         }  
      }  
  
      catch (Exception e)  
      {  
         Console.WriteLine(e.Message);  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento tecnico &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Utilizzo di intestazioni SOAP di Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
