---
title: Metodi di invio in batch | Microsoft Docs
description: Informazioni su come usare le intestazioni SOAP in Reporting Services per includere più metodi di servizio Web in un'unica operazione.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- methods [Reporting Services], batches
- BatchHeader SOAP header
- Web service [Reporting Services], methods
- Report Server Web service, batching
- batches [Reporting Services]
- XML Web service [Reporting Services], methods
- locking [Reporting Services]
- multiple Web service methods
ms.assetid: 86435534-c9fe-4b49-b88c-7fb6d21976b0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 145a7ad079827ca3392eb0bde0761f9f8f70a9f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081682"
---
# <a name="batching-methods"></a>Metodi di invio in batch
  L'utilizzo di intestazioni SOAP in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] consente di includere più metodi del servizio Web in una singola operazione. I metodi vengono eseguiti nell'ambito di un'unica transazione di database, nell'ordine con cui vengono chiamati.  
  
 La possibilità di eseguire il rollback rappresenta un vantaggio dell'utilizzo di operazioni batch con più metodi. Se si verifica un errore in alcune delle chiamate ai metodi mentre un batch è in esecuzione, il server di report arresta l'esecuzione del batch ed esegue il rollback di qualsiasi operazione precedente. Questo è utile quando una chiamata al metodo dipende dal corretto completamento delle altre chiamate ai metodi nel batch.  
  
 Il servizio Web non fornisce una semantica di blocco per le operazioni batch con più metodi. Le righe nel database del server di report non vengono bloccate per l'aggiornamento fino a quando il messaggio non viene inviato al server e non viene chiamato il comando Execute.  
  
 Non vengono inoltre eseguiti controlli della concorrenza per garantire che il database non sia stato modificato dopo l'ultima lettura dei dati. Se due client modificano lo stesso elemento, l'ultimo aggiornamento ha esito positivo se i parametri sono ancora validi (ad esempio se l'elemento non è stato rinominato).  
  
 Nell'esempio seguente il metodo <xref:ReportService2005.ReportingService2005.CreateFolder%2A> viene chiamato tre volte e queste chiamate vengono eseguite come singolo batch. Se una delle chiamate a <xref:ReportService2005.ReportingService2005.CreateFolder%2A> ha esito negativo, l'intero batch viene annullato.  
  
```vb  
Imports System  
Imports System.Web.Services.Protocols  
Imports myNamespace.MyReferenceName  
  
Class Sample  
    Sub Main(args() As String)  
        Dim rs As New ReportingService2005()  
        rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
      ' Set the base Web service URL of the source server  
      rs.Url = "https://<Server Name>/reportserver/ReportService2005.asmx"  
  
        Dim bh As New BatchHeader()  
  
        bh.BatchId = service.CreateBatch()  
        rs.BatchHeaderValue = bh  
        rs.CreateFolder("New Folder1", "/", Nothing)  
        rs.CreateFolder("New Folder2", "/", Nothing)  
        rs.CreateFolder("New Folder3", "/", Nothing)  
  
        Console.WriteLine("Creating folders...")  
        rs.BatchHeaderValue = bh  
        rs.ExecuteBatch()  
        Console.WriteLine("Folders created successfully.")  
  
        rs.BatchHeaderValue = Nothing  
    End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.Web.Services.Protocols;   
using myNamespace.MyReferenceName;  
  
class Sample  
{  
    static void Main(string[] args)  
    {  
        ReportingService2005 rs = new ReportingService2005();  
        rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
      // Set the base Web service URL of the source server  
      rs.Url = "https://<Server Name>/reportserver/ReportService2005.asmx"  
  
        BatchHeader bh = new BatchHeader();  
  
        bh1.BatchID = service.CreateBatch();  
        rs.BatchHeaderValue = bh;  
        rs.CreateFolder("New Folder1", "/", null);  
        rs.CreateFolder("New Folder2", "/", null);  
        rs.CreateFolder("New Folder3", "/", null);  
  
        Console.WriteLine("Creating folders...");  
        rs.BatchHeaderValue = bh1;  
        rs.ExecuteBatch();  
        Console.WriteLine("Folders created successfully.");  
  
        rs.BatchHeaderValue = null;  
    }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 <xref:ReportService2005.ReportingService2005.CancelBatch%2A>   
 <xref:ReportService2005.ReportingService2005.CreateBatch%2A>   
 [Riferimento tecnico &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Uso di intestazioni SOAP di Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
