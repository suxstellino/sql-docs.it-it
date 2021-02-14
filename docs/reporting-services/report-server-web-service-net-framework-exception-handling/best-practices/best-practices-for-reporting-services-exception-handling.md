---
title: Procedure consigliate per la gestione delle eccezioni in Reporting Services | Microsoft Docs
description: Informazioni sulle procedure consigliate per la gestione delle eccezioni di Reporting Services, ad esempio come gestire i casi di errore che non generano eccezioni.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- exceptions [Reporting Services], best practices
ms.assetid: 72fecf28-f02e-4338-b50f-0b21f520302d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c8a14ab341e3d55cb60e13e0c4d4a2a29baeb457
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023640"
---
# <a name="best-practices-for-reporting-services-exception-handling"></a>Procedure consigliate per la gestione delle eccezioni di Reporting Services
  Quando si sviluppano applicazioni [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è possibile ricorrere a diversi metodi per eliminare o ridurre le eccezioni. Quando si verificano le eccezioni, fornire messaggi di errore chiari e concisi all'utente e aggiungere funzionalità adeguate di gestione delle eccezioni per impedire che le applicazioni vengano chiuse in modo imprevisto.  
  
 Un'applicazione che invia richieste al servizio Web ReportServer deve essere in grado di effettuare le operazioni seguenti:  
  
-   Evitare che vengano generate eccezioni impedendo il maggior numero possibile di richieste non valide.  
  
-   Rilevare le eccezioni e fornire codice specifico di gestione degli errori quando possibile.  
  
-   Gestire i casi di errore che non generano eccezioni.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Metodi per evitare le richieste non valide](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/preventing-invalid-requests.md)|Vengono descritte le tecniche per impedire l'invio delle richieste non valide al server di report.|  
|[Uso di blocchi try/catch](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/using-try-and-catch-blocks.md)|Viene descritto come migliorare l'affidabilità dell'applicazione con i blocchi try/catch.|  
|[Gestione di avvisi e casi che non causano eccezioni](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/handling-warnings-and-cases-that-do-not-cause-exceptions.md)|Viene illustrato come gestire gli errori che non comportano la generazione di un'eccezione in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
|[Uso della proprietà Detail per la gestione di errori specifici](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/using-the-detail-property-to-handle-specific-errors.md)|Spiega come gestire errori specifici a livello di codice tramite la proprietà **Detail** dell'oggetto **SoapException**.|  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Detail](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/detail-property.md)   
 [Introduzione alla gestione delle eccezioni in Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Classe SoapException di Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)  
  
  
