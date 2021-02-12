---
title: Ruolo di SOAP in Reporting Services | Microsoft Docs
description: Il servizio Web ReportServer usa SOAP su HTTP. Gestione report fornisce un'interfaccia per il database del server di report in cui vengono archiviati i report e il relativo contenuto.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Web service [Reporting Services], SOAP
- SOAP [Reporting Services], role in Reporting Services
- Report Server Web service, SOAP
- XML Web service [Reporting Services], SOAP
ms.assetid: f229c3ef-f2ca-448f-98f1-b8df350b9992
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 616171eb0b641149cf7b27d7c45c04c20daa8c64
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021713"
---
# <a name="the-role-of-soap-in-reporting-services"></a>Ruolo di SOAP in Reporting Services
  Il servizio Web ReportServer utilizza la messaggistica SOAP (Simple Object Access Protocol) per inviare comandi basati su testo in una rete. Questi comandi sono in formato di testo XML e vengono inviati nel World Wide Web utilizzando HTTP. Grazie all'utilizzo di SOAP come protocollo di comunicazione, il servizio Web ReportServer consente alle applicazioni e ai componenti di scambiare dati con il server di report utilizzando un'infrastruttura aperta molto diffusa. Lo standard SOAP è definito nel sito www.w3.org/TR/SOAP (informazioni in lingua inglese).  
  
 Qualsiasi applicazione client può essere utilizzata come client SOAP a condizione che supporti SOAP e che sia in grado di inviare richieste SOAP. Gestione report è un client SOAP. Questo client fornisce un'interfaccia per il database del server di report in cui vengono archiviati tutti i report e il contenuto correlato ai report. Gli utenti finali possono utilizzare l'applicazione per esplorare e gestire report e cartelle nello spazio dei nomi del server di report. Gestione report è basato sull'infrastruttura del servizio Web ReportServer.  
  
 Un server di report funge da server SOAP, un servizio che supporta SOAP in grado di accettare le richieste dai client SOAP e di creare risposte appropriate. Il server gestisce le richieste e restituisce al client risposte codificate.  
  
 I messaggi SOAP in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] possono avere formati diversi, a seconda del tipo di richiesta effettuata dal client. Nell'esempio seguente viene rappresentata una semplice richiesta client SOAP per la rimozione di un elemento dal database del server di report.  
  
```  
<soap:Envelope xmlns:soap="https://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItem xmlns="https://www.microsoft.com/sql/ReportingServer">  
            <item>/Samples/Report1</item>  
        </DeleteItem>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 SOAP richiede che i messaggi siano inseriti in un elemento **Busta** e che il blocco del messaggio sia inserito in un elemento **Corpo**. In questo esempio il corpo contiene una chiamata al metodo <xref:ReportService2010.ReportingService2010.DeleteItem%2A> che accetta un parametro stringa che rappresenta il percorso dell'elemento da eliminare. È possibile creare una classe proxy client [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] che incapsula tutte le operazioni SOAP nei metodi. Il metodo [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] seguente rappresenta l'esempio SOAP riportato in precedenza.  
  
```  
public void DeleteItem(string item);  
```  
  
 La risposta dal server può essere simile alla seguente:  
  
```  
<soap:Envelope xmlns:soap="https://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItemResponse xmlns="https://www.microsoft.com/sql/ReportingServer" />  
    </soap:Body>  
</soap:Envelope>  
```  
  
 Il metodo <xref:ReportService2010.ReportingService2010.DeleteItem%2A> non restituisce alcun valore, pertanto viene restituita una risposta vuota.  
  
## <a name="see-also"></a>Vedere anche  
 [Accesso all'API SOAP](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)   
 [Gestione report &#40;modalità nativa SSRS&#41;](../web-portal-ssrs-native-mode.md)   
 [Reporting Services Report Server](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)   
 [Servizio Web ReportServer](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
