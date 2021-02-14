---
title: Compilazione di applicazioni tramite servizio Web e .NET Framework | Microsoft Docs
description: Un client del servizio Web ReportServer comunica con un server di report tramite SOAP. Usare .NET Framework per creare client di servizi Web per lavorare con qualsiasi servizio Web.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Report Server Web service, application building
- .NET Framework [Reporting Services]
- XML Web service [Reporting Services], client creation
- reports [Reporting Services], building
- Web service [Reporting Services], application building
- Report Server Web service, client creation
- client configuration [Reporting Services]
- XML Web service [Reporting Services], application building
- Web service [Reporting Services], client creation
ms.assetid: 92a9678c-bc4f-4d7a-ba44-85989bfe27ca
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d40d77660fd722e668fc5d1d080b52b37bd30822
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075156"
---
# <a name="building-applications-using-the-web-service-and-the-net-framework"></a>Compilazione di applicazioni tramite servizio Web e .NET Framework
  Con [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], è possibile usare costrutti di programmazione familiari, ad esempio metodi, tipi primitivi e tipi complessi definiti dall'utente, per usare i servizi Web. [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] include un'infrastruttura e strumenti che è possibile utilizzare per creare i client del servizio Web che possono chiamare qualsiasi servizio Web conforme agli standard del World Wide Web Consortium (W3C).  
  
 Un client del servizio Web ReportServer è qualsiasi componente o applicazione che comunica con un server di report utilizzando messaggi SOAP (Simple Object Access Protocol).  
  
 **Per creare un client del servizio Web ReportServer utilizzando .NET Framework, effettuare i passaggi di base seguenti:**  
  
1.  Creare una classe proxy per il servizio Web.  
  
     A tale scopo, aggiungere una classe proxy o un riferimento Web al progetto di sviluppo, fare riferimento alla classe proxy nel codice client e creare un'istanza del proxy. Per altre informazioni vedere [Creazione del proxy del servizio Web](../../../reporting-services/report-server-web-service/net-framework/creating-the-web-service-proxy.md).  
  
2.  Autenticare il client del servizio Web con il server di report.  
  
     A tale scopo, impostare la proprietà <xref:System.Web.Services.Protocols.WebClientProtocol.Credentials%2A> dell'oggetto servizio in modo che corrisponda alle credenziali di un utente autenticato nel server di report. Per altre informazioni, vedere [Autenticazione del servizio Web](../../../reporting-services/report-server-web-service/net-framework/web-service-authentication.md).  
  
3.  Chiamare il metodo della classe proxy che corrisponde all'operazione del servizio Web che si desidera richiamare.  
  
     A tale scopo, chiamare un metodo del servizio Web e fornire gli argomenti necessari. Per altre informazioni sui metodi del servizio Web, vedere [Metodi del servizio Web ReportServer](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md). Per altre informazioni sulla chiamata, vedere [Chiamata ai metodi del servizio Web](../../../reporting-services/report-server-web-service/net-framework/calling-web-service-methods.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Creazione del proxy del servizio Web](../../../reporting-services/report-server-web-service/net-framework/creating-the-web-service-proxy.md)|Vengono descritte le modalità di aggiunta di una classe proxy al progetto usando [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].|  
|[Autenticazione del servizio Web](../../../reporting-services/report-server-web-service/net-framework/web-service-authentication.md)|Viene descritto in che modo vengono autenticate le chiamate al servizio Web ReportServer.|  
|[Chiamata ai metodi del servizio Web](../../../reporting-services/report-server-web-service/net-framework/calling-web-service-methods.md)|Viene descritto come usare l'API SOAP per chiamare i metodi del servizio Web [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].|  
|[Impostazione della proprietà Url del servizio Web](../../../reporting-services/report-server-web-service/net-framework/setting-the-url-property-of-the-web-service.md)|Viene illustrato come indirizzare a livello di programmazione il proxy del servizio Web a un nuovo URL del server dopo avere creato il riferimento Web.|  
|[Impostazione degli argomenti dei metodi del servizio Web](../../../reporting-services/report-server-web-service/net-framework/supplying-web-service-method-arguments.md)|Viene descritto come richiamare un metodo del servizio Web e come fornire gli argomenti del metodo.|  
|[Omissione di valori per gli oggetti del servizio Web facoltativi](../../../reporting-services/report-server-web-service/net-framework/omitting-values-for-optional-web-service-objects.md)|Viene descritto come omettere i valori per gli oggetti del servizio Web facoltativi.|  
|[Uso di metodi del servizio Web protetti](../../../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md)|Viene descritta l'impostazione **SecureConnectionLevel** e in che modo influisce sull'uso dell'API SOAP di Reporting Services.|  
|[Passaggio delle impostazioni relative alle informazioni sul dispositivo alle estensioni per il rendering](../../../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md)|Vengono descritte le impostazioni relative alle informazioni sul dispositivo utilizzate per eseguire il rendering dei report in formati diversi.|  
|[Impostazioni delle estensioni per il recapito di Reporting Services](../../../reporting-services/report-server-web-service/net-framework/reporting-services-delivery-extension-settings.md)|Vengono descritte le impostazioni utilizzate per recapitare i report utilizzando il servizio di posta elettronica del server di report.|  
|[Uso di intestazioni SOAP di Reporting Services](../../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)|Viene illustrato l'utilizzo delle intestazioni SOAP in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
|[Introduzione alla gestione delle eccezioni in Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)|Vengono fornite informazioni sulla modalità di gestione degli errori in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
  
## <a name="see-also"></a>Vedere anche  
 [Servizio Web ReportServer](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Riferimento tecnico &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
