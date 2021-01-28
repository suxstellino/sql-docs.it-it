---
description: Configurare l'autenticazione personalizzata o basata su form nel server di report
title: Configurare l'autenticazione personalizzata o basata su form nel server di report | Microsoft Docs
ms.date: 04/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Forms authentication, configuring
- custom authentication [Reporting Services]
ms.assetid: e8601a8f-e66d-4649-8e4d-a46ca20ec7d0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bca083e29d0d03a5a5470f58b4e004d81cd27a36
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596636"
---
# <a name="configure-custom-or-forms-authentication-on-the-report-server"></a>Configurare l'autenticazione personalizzata o basata su form nel server di report

Reporting Services offre un'architettura estensibile che consente di inserire moduli di autenticazione basata su form o personalizzata. È possibile implementare un'estensione di autenticazione personalizzata, se i requisiti di distribuzione non includono la sicurezza integrata di Windows o l'autenticazione di base. Lo scenario più comune per l'utilizzo dell'autenticazione personalizzata consiste nel supporto dell'accesso Internet o extranet a un'applicazione Web. La sostituzione dell'estensione di autenticazione di Windows predefinita con un'estensione di autenticazione personalizzata consente un maggiore controllo sulle modalità di concessione dell'accesso al server di report agli utenti esterni.  

La distribuzione di un'estensione di autenticazione personalizzata richiede sostanzialmente più passaggi, che includono la copia di un assembly e dei file dell'applicazione, la modifica dei file di configurazione e il test. In questo argomento vengono illustrate solo le impostazioni di autenticazione che l'utente specifica nei file di configurazione.  

> [!NOTE]
>  La creazione di un'estensione di autenticazione personalizzata richiede codice personalizzato ed esperienza in materia di sicurezza di [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Se non si desidera creare un'estensione di autenticazione personalizzata, è possibile utilizzare gruppi e account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory, ma è necessario ridurre notevolmente l'ambito di distribuzione del server di report. Per altre informazioni sull'autenticazione personalizzata, vedere [Implementazione di un'estensione di sicurezza](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).

Se si vuole usare anche l'autenticazione basata su form o un'estensione di autenticazione personalizzata in un ambiente SQL Server Reporting Services integrato con un prodotto SharePoint, è necessario configurare il sito di SharePoint per l'uso del metodo di autenticazione scelto. Per altre informazioni sulla configurazione dell'autenticazione in SharePoint, vedere [Esempi di autenticazione](/previous-versions/office/sharepoint-2007-products-and-technologies/cc262069(v=office.12)) in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Developer Network (MSDN).



### <a name="to-configure-a-report-server-to-use-custom-authentication"></a>Per configurare un server di report per l'utilizzo dell'autenticazione personalizzata

1.  Aprire RSReportServer.config in un editor di testo.

2.  Individuare \<**Authentication**>.

3.  Copiare la struttura XML seguente:

    ```
    <Authentication>
          <AuthenticationTypes>
                 <Custom />
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
    </Authentication>
    ```

4.  Incollare la struttura sulle voci esistenti per \<**Authentication**>.

     Si noti che non è possibile usare **Custom** con altri tipi di autenticazione.

5.  Salvare il file.

6.  Aprire il file Web.config per il server di report. Per impostazione predefinita, il percorso di questo file è \Programmi\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\ReportServer.

7.  Trovare **authentication mode** e impostarlo su **Forms**.

    ```
    <authentication mode = "Forms" />
    ```

8.  Trovare **identity impersonate** e impostarlo su **False**.

    ```
    <identity impersonate = "false" />  
    ```
9. Aggiungere la struttura dell'elemento **PassThroughCookies** al file di configurazione. Per altre informazioni, vedere [Configurare il portale Web per il passaggio di cookie di autenticazione personalizzati](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md)
  
10. Salvare il file.  
  
11. Se è stata configurata una distribuzione con scalabilità orizzontale, ripetere tutti i passaggi precedenti per gli altri server di report presenti nella distribuzione.  
  
12. Riavviare il server di report per cancellare qualsiasi sessione attualmente aperta.  

## <a name="see-also"></a>Vedere anche

[Implementazione di un'estensione di sicurezza](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
[Esempio di sicurezza personalizzata di Reporting Services (GitHub)](https://github.com/Microsoft/Reporting-Services/tree/master/CustomSecuritySample)  
[Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md)   
[File di configurazione RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configurare l'autenticazione di base nel server di report](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
[Configurare l'autenticazione di Windows nel server di report](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)