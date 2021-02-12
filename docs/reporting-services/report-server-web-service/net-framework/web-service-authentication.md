---
title: Autenticazione del servizio Web | Microsoft Docs
description: Se il client esegue richieste SOAP a un server di report, implementare la parte client dell'autenticazione. Informazioni su come implementare l'autenticazione per un servizio Web.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Web service [Reporting Services], authentication
- XML Web service [Reporting Services], authentication
- Report Server Web service, authentication
ms.assetid: 852b4947-a090-4e54-8555-5a503945ceab
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f3dfb2b1f3806d9e00e309b45ebf90182c83cab5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021757"
---
# <a name="web-service-authentication"></a>Autenticazione del servizio Web
  Per autenticare le chiamate effettuate al servizio Web ReportServer, è possibile utilizzare l'autenticazione di Windows o l'autenticazione di base. Qualsiasi client che effettua richieste SOAP al server di report deve implementare la parte client di uno dei protocolli di autenticazione supportati. Se si usa [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], è possibile usare le classi HTTP di codice gestito per implementare l'autenticazione. L'utilizzo di queste API semplifica l'invio delle informazioni di autenticazione insieme alle richieste SOAP.  
  
 Se non si dispone delle credenziali appropriate prima di effettuare una chiamata al servizio Web ReportServer, la chiamata ha esito negativo. In fase di esecuzione è possibile passare le credenziali al servizio Web impostando la proprietà **Credenziali** dell'oggetto sul lato client che rappresenta il servizio Web prima di chiamarne i metodi.  
  
 Nelle sezioni seguenti sono inclusi esempi di codice per l'invio delle credenziali utilizzando [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="windows-authentication"></a>Autenticazione di Windows  
 Nel codice seguente vengono passate le credenziali di Windows al servizio Web.  
  
```vb  
Dim rs As New ReportingService()  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
```  
  
```csharp  
ReportingService rs = new ReportingService();  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
```  
  
## <a name="basic-authentication"></a>Autenticazione di base  
 Nel codice seguente vengono passate le credenziali di base al servizio Web.  
  
```vb  
Dim rs As New ReportingService()  
rs.Credentials = New System.Net.NetworkCredential("username", "password", "domain")  
```  
  
```csharp  
ReportingService service = new ReportingService();  
service.Credentials = new System.Net.NetworkCredential("username", "password", "domain");  
```  
  
 Le credenziali devono essere impostate prima di chiamare i metodi del servizio Web ReportServer. Se non si impostano le credenziali, viene visualizzato il codice di errore Errore HTTP 401: Accesso negato. È necessario autenticare il servizio prima di utilizzarlo, ma dopo avere impostato le credenziali non è necessario impostarle di nuovo fino a quando si continua a utilizzare la stessa variabile del servizio (ad esempio *rs*).  
  
## <a name="custom-authentication"></a>Autenticazione personalizzata  
 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] include un'API di programmazione che consente agli sviluppatori di progettare e sviluppare estensioni di autenticazione personalizzate, note come estensioni di sicurezza. Per ulteriori informazioni, vedere [Implementing a Security Extension](../../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni tramite servizio Web e .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Servizio Web ReportServer](../../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
