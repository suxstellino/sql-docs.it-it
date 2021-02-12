---
title: Servizio Web ReportServer | Microsoft Docs
description: Reporting Services fornisce la funzionalità del server di report con il servizio Web ReportServer, un endpoint del servizio SOAP per l'esecuzione e la gestione dei report.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- SSIS, Web service
- Web service [Reporting Services]
- Reporting Services, extending
- SQL Server Reporting Services, Web service
- Reporting Services, Web service
- XML Web service [Reporting Services]
- Report Server Web service
ms.assetid: 16c21dec-6b46-4497-9a0c-1b0f2b6ab8fc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 55160717791f4d14a7f008725710f725069cd6d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022108"
---
# <a name="report-server-web-service"></a>servizio Web ReportServer
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] consente di accedere alle funzionalità complete del server di report tramite il servizio Web ReportServer. Il servizio Web ReportServer è un servizio Web XML con un'API SOAP. Il servizio usano SOAP su HTTP e funge da interfaccia di comunicazione tra i programmi client e il server di report. Il servizio Web fornisce due endpoint, uno per l'esecuzione dei report e uno per la gestione dei report, con metodi che espongono le funzionalità del server di report e consentono di creare strumenti personalizzati per qualsiasi parte del ciclo di vita del report.  
  
 Sono disponibili tre modi per sviluppare applicazioni [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] basate sul servizio Web. È possibile:  
  
-   Sviluppare applicazioni usando [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK. Per altre informazioni sull'uso di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] per la compilazione di applicazioni del servizio Web, vedere [Compilazione di applicazioni tramite servizio Web e .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md).  
  
-   Sviluppare applicazioni usando l'utilità **rs** (RS.exe), l'ambiente di script [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Gli script di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] consentono di eseguire qualsiasi operazione del servizio Web ReportServer. Per altre informazioni sull'esecuzione di script in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Eseguire lo script con l'utilità rs.exe e il servizio Web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md).  
  
-   Sviluppare applicazioni usando qualsiasi set di strumenti di sviluppo abilitato per SOAP. Per altre informazioni, vedere [Ruolo di SOAP in Reporting Services](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md).  
  
## <a name="programming-diagram"></a>Diagramma di programmazione  
 ![Opzioni di sviluppo del servizio Web ReportServer](../../reporting-services/report-server-web-service/media/reportserviceswebserviceprog-01.gif "Opzioni di sviluppo del servizio Web ReportServer")  
Opzioni di sviluppo dei servizi Web disponibili in Reporting Services  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Metodi del servizio Web di Report Server](../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)  
 Vengono descritti i metodi e le caratteristiche di ogni servizio Web ReportServer.  
  
 [Ruolo di SOAP in Reporting Services](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md)  
 Viene fornita una panoramica su SOAP e sul suo utilizzo nei servizi Web ReportServer.  
  
 [Accesso all'API SOAP](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)  
 Viene descritto il linguaggio WSDL (Web Service Description Language) e vengono forniti gli URL per l'accesso a un file WSDL di Reporting Services.  
  
 [Creazione di applicazioni mediante il servizio Web e .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
 Sono incluse informazioni sullo sviluppo di applicazioni e servizi Web che chiamano l'API SOAP di Reporting Services.  
  
 [Eseguire lo script con l'utilità rs.exe e il servizio Web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)  
 Viene fornita una panoramica sull'ambiente di scripting [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 [Riferimento tecnico &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)  
 È incluso materiale di riferimento specifico dei metodi dei servizi Web ReportServer e dei tipi complessi corrispondenti.  
  
## <a name="user-requirements-for-web-service-development"></a>Requisiti utente per lo sviluppo del servizio Web  
 Per sviluppare applicazioni usando il servizio Web ReportServer, è necessario quanto segue:  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer 5.5 o versione successiva installato in un computer con una connessione Internet e accesso al server di report.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK installato in un computer se si vuole sviluppare e distribuire le applicazioni [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usando [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
-   Conoscenza approfondita delle caratteristiche e delle funzionalità di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Buona conoscenza di SOAP e [!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)].  
  
-   Esperienza nello sviluppo in un linguaggio compatibile con [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] o [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], se si prevede di usare [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] come piattaforma di sviluppo.  
  
## <a name="see-also"></a>Vedere anche  
 [Servizio Web ReportServer](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
