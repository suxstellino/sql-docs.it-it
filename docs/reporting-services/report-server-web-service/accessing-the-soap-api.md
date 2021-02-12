---
title: Accesso all'API SOAP | Microsoft Docs
description: Il servizio Web ReportServer usa SOAP su HTTP e funge da interfaccia di comunicazione tra i client e il server di report. Usare WSDL per chiamare il servizio.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- XML Web service [Reporting Services], WSDL
- Web service [Reporting Services], SOAP
- calling Web service
- SOAP [Reporting Services], accessing
- Report Server Web service, SOAP
- Web service [Reporting Services], WSDL
- WSDL [Reporting Services]
- XML Web service [Reporting Services], SOAP
- Report Server Web service, WSDL
- referencing WSDL
ms.assetid: 63bb870a-4dbf-46bd-8921-78f8ebe5fd75
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5fcb5dd895f6997ed72c91e9c66d9bd7be1c5aa4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018654"
---
# <a name="accessing-the-soap-api"></a>Accesso all'API SOAP
  Il servizio Web ReportServer utilizza SOAP (Simple Object Access Protocol) tramite HTTP e funge da interfaccia di comunicazione tra i programmi client e il server di report. Il servizio Web fornisce due endpoint, uno per l'esecuzione dei report e uno per la gestione dei report ed è costituito da metodi e un set di oggetti di tipo complesso che è possibile utilizzare per accedere alle funzionalità complete di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Per chiamare il servizio, è necessario fare riferimento al linguaggio WSDL (Web Services Description Language) di Reporting Services.  
  
## <a name="referencing-the-reporting-services-wsdl"></a>Riferimento al linguaggio WSDL di Reporting Services  
 Per chiamare correttamente un servizio Web, è necessario sapere come accedere al servizio, quali operazioni sono supportate dal servizio, quali parametri sono previsti dal servizio e cosa viene restituito dal servizio. WSDL fornisce queste informazioni in un documento XML che può essere letto o elaborato da un computer.  
  
 I servizi Web ReportServer sono esposti in tre endpoint diversi. Il nome del file WSDL è diverso per ogni endpoint. L'endpoint <xref:ReportService2010> contiene metodi per la gestione di oggetti in un server di report in modalità nativa o in modalità integrata SharePoint. È possibile accedere al linguaggio WSDL per questo endpoint tramite `ReportService2010.asmx?wsdl.`  
  
> [!NOTE]  
>  Gli endpoint <xref:ReportService2005> e <xref:ReportService2006> sono deprecati in [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. L'endpoint <xref:ReportService2010> include le funzionalità di entrambi gli endpoint e contiene caratteristiche di gestione aggiuntive.  
  
-   L'endpoint <xref:ReportExecution2005> consente agli sviluppatori di elaborare a livello di programmazione i report e di eseguirne il rendering in un server di report. È possibile accedere al linguaggio WSDL per questo endpoint tramite `ReportExecution2005.asmx?wsdl`.  
  
 WSDL può essere usato dai kit di sviluppo che supportano SOAP e i servizi Web, ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  
  
 Nell'esempio seguente viene illustrato il formato dell'URL del file WSDL di gestione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
```  
https://server/reportserver/ReportService2010.asmx?wsdl  
```  
  
 Nella tabella seguente sono descritti gli elementi dell'URL.  
  
|Elemento URL|Descrizione|  
|-----------------|-----------------|  
|*server*|Nome del server in cui viene distribuito il server di report.|  
|*reportserver*|Nome della cartella contenente il servizio Web XML. Questo nome viene configurato durante l'installazione.|  
|*\<endpoint name>. asmx*|Nome dell'endpoint del servizio Web.|  
  
 Per altre informazioni sul formato WSDL, vedere la specifica WSDL nel sito Web World Wide Web Consortium (W3C) all'indirizzo http://www.w3.org/TR/wsdl.  
  
## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni tramite servizio Web e .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Servizio Web ReportServer](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
