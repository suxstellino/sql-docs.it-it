---
title: Endpoint del servizio Web ReportServer | Microsoft Docs
description: Il servizio Web ReportServer fornisce tre endpoint per la gestione di un server di report e per l'esecuzione e la navigazione dei report.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- management endpoints [Reporting Services]
- Web service [Reporting Services], endpoints
- endpoints [Reporting Services]
- execution endpoints [Reporting Services]
- Report Server Web service, endpoints
ms.assetid: f3f5d85f-9359-4508-bc5a-7f78a3cf7421
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1d9a0bfe8aa4a469899a558c1cbe1d3b38b337b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063746"
---
# <a name="report-server-web-service-endpoints"></a>Endpoint del servizio Web ReportServer
  Il servizio Web ReportServer fornisce diversi endpoint per la gestione di un server di report e per l'esecuzione e la navigazione dei report.  
  
## <a name="the-management-endpoints"></a>Endpoint di gestione  
 Per la gestione degli oggetti in un server di report sono disponibili tre endpoint, <xref:ReportService2005>, <xref:ReportService2006> e <xref:ReportService2010>. L'endpoint <xref:ReportService2005> viene utilizzato per la gestione degli oggetti in un server di report configurato per la modalità nativa. L'endpoint <xref:ReportService2006> viene utilizzato per la gestione degli oggetti in un server di report configurato per la modalità integrata SharePoint. L'endpoint <xref:ReportService2010> unisce le funzionalità di <xref:ReportService2005> e <xref:ReportService2006> e può gestire gli oggetti in un server di report configurati per la modalità nativa o per la modalità integrata SharePoint.  
  
> [!IMPORTANT]  
>  Quando un server di report è configurato per la modalità integrata SharePoint, le API di <xref:ReportService2005> restituiscono un errore **rsOperationNotSupportedSharePointMode**. Se il server di report è configurato per la modalità nativa, le API di <xref:ReportService2006> restituiscono un errore **rsOperationNotSupportedNativeMode**. In modo analogo, se le API specifiche della modalità in <xref:ReportService2010> vengono utilizzate in modalità non previste, restituiranno gli errori corrispondenti.  
  
> [!NOTE]  
>  Gli endpoint <xref:ReportService2005> e <xref:ReportService2006> sono deprecati in [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]. L'endpoint <xref:ReportService2010> include le funzionalità di entrambi gli endpoint e contiene caratteristiche di gestione aggiuntive.  
  
 Se il server di report è configurato per la modalità nativa o la modalità integrata SharePoint, è possibile accedere al codice WSDL per l'endpoint di gestione utilizzando uno dei seguenti URL:  
  
```  
https://<Server Name>/ReportServer/ReportService2010.asmx?wsdl  
```  
  
 Per altre informazioni, vedere [Accesso all'API SOAP](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md).  
  
## <a name="the-execution-endpoint"></a>Endpoint di esecuzione  
 L'endpoint <xref:ReportExecution2005> consente agli sviluppatori di personalizzare in modo semplice le fasi di elaborazione e rendering dei report da un server di report sia in modalità nativa che in modalità integrata SharePoint. L'endpoint include le classi e i metodi disponibili nelle versioni precedenti del servizio Web ReportServer. Al servizio Web ReportServer sono inoltre stati aggiunti numerosi nuovi metodi e classi esposti tramite l'endpoint di esecuzione.  
  
 È possibile accedere al codice WSDL per l'endpoint di gestione utilizzando l'URL seguente:  
  
```  
https://<Server Name>/ReportServer/ReportExecution2005.asmx?wsdl  
```  
  
 Se il server di report è configurato per la modalità integrata SharePoint, è possibile accedere al codice WSDL utilizzando l'URL seguente:  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportExecution2005.asmx?wsdl  
```  
  
 Per altre informazioni, vedere [Accesso all'API SOAP](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md).  
  
## <a name="sharepoint-proxy-endpoints"></a>Endpoint proxy di SharePoint  
 Quando un server di report è configurato per la modalità integrata SharePoint ed è stato installato il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], nel server SharePoint viene installato un set di endpoint proxy. Gli endpoint proxy rappresentano l'API principale per lo sviluppo di soluzioni di report quando un server di report è configurato per la modalità integrata SharePoint. Quando lo sviluppo viene eseguito negli endpoint proxy, il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] gestisce lo scambio di credenziali tra il server SharePoint e il server di report nella modalità di autenticazione Account attendibile. Quando lo sviluppo viene eseguito negli endpoint del server di report, l'applicazione chiamante deve gestire lo scambio di credenziali nella modalità di autenticazione Account attendibile. Nella tabella seguente sono elencati gli endpoint installati con il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Endpoint proxy|Descrizione|  
|--------------------|-----------------|  
|<xref:ReportService2006>|Fornisce le API per la gestione di un server di report configurato per la modalità integrata SharePoint.<br /><br /> Nota: questo endpoint è deprecato in [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)].|  
|<xref:ReportService2010>|Fornisce le API per la gestione di un server di report configurato per la modalità nativa o la modalità integrata SharePoint.|  
|<xref:ReportExecution2005>|Fornisce le API per l'esecuzione e la navigazione dei report.|  
|<xref:ReportServiceAuthentication>|Fornisce le API per l'autenticazione degli utenti rispetto a un server di report quando l'applicazione Web SharePoint è configurata per l'autenticazione basata su form.|  
  
 Di seguito sono riportati alcuni URL di esempio che consentono di fare riferimento agli endpoint proxy in un sito di SharePoint.  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportService2010.asmx  
```  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportExecution2005.asmx  
```  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportServiceAuthentication.asmx  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di applicazioni mediante il servizio Web e .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
  
  
