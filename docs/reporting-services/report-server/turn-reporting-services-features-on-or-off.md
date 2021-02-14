---
title: Abilitare o disabilitare le funzionalità di Reporting Services | Microsoft Docs
description: Informazioni su come disabilitare singole funzionalità in Reporting Services in modalità nativa. Esistono diversi modi per configurare le funzionalità.
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- security [Reporting Services], strategies
ms.assetid: b69db02a-43a7-4fdc-ad9b-438d817a7f83
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c6b42238a081adbd40f9756fb3d2fb8871e1d7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023336"
---
# <a name="turn-reporting-services-features-on-or-off"></a>Abilitare o disabilitare le funzionalità di Reporting Services
  È possibile disabilitare le funzionalità del server di report non utilizzate come parte di una strategia di blocco per ridurre la superficie di attacco di un server di report di produzione. Nella maggior parte dei casi, è consigliabile eseguire le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] simultaneamente in modo da poter utilizzare tutte le funzionalità disponibili in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Tuttavia, a seconda del modello di distribuzione, è possibile disabilitare le funzionalità che non sono necessarie. Ad esempio, è possibile abilitare solo l'elaborazione in background se tutte le operazioni di elaborazione dei report vengono configurate come operazioni pianificate. Analogamente, se si vuole che la generazione di report venga eseguita solo in modo interattivo e su richiesta, è possibile eseguire solo il servizio Web ReportServer.  
  
 Le procedure in questo articolo illustrano come disattivare le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità nativa. Le funzionalità possono essere configurate in modi diversi, ad esempio modificando direttamente il file `RsReportServer.config` o usando il facet **Configurazione superficie di attacco per Reporting Services** della gestione basata su criteri in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Utilizzare i collegamenti per trovare la procedura o le procedure che illustrano come abilitare o disabilitare una funzionalità:  
  
-   [Servizio Web ReportServer](#RSWebSvc)  
  
-   [Elaborazione ed eventi pianificati](#Sched)  
  
-   [Portale Web](#WebPortal)  
  
-   [Sicurezza integrata di Windows per le origini dei dati dei report](#WinIntSec)  
  
##  <a name="report-server-web-service"></a><a name="RSWebSvc"></a> Servizio Web ReportServer  
  
### <a name="to-turn-on-or-off-the-report-server-web-service-by-editing-configuration"></a>Per attivare o disattivare il servizio Web ReportServer mediante la modifica della configurazione  
  
1.  Aprire il file `RsReportServer.config` in un editor di testo. Per altre informazioni, vedere [Modificare un file di configurazione di Reporting Services&#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
2.  Per attivare il servizio Web ReportServer, impostare **IsWebServiceEnabled** su **true**:  
  
    ```  
    <IsWebServiceEnabled>true</IsWebServiceEnabled>  
    ```  
  
3.  Per disattivare il servizio Web ReportServer, impostare **IsWebServiceEnabled** su **false**:  
  
    ```  
    <IsWebServiceEnabled>false</IsWebServiceEnabled>  
    ```  
  
4.  Salvare le modifiche, quindi chiudere il file.  
  
#### <a name="to-turn-on-or-off-the-report-server-web-service-by-using-sql-server-management-studio"></a>Per attivare o disattivare il servizio Web ReportServer tramite SQL Server Management Studio  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e connettersi all'istanza di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che si desidera configurare.  
  
2.  In Esplora oggetti fare clic con il pulsante destro del mouse sul nodo di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , scegliere **Criteri**, quindi fare clic su **Facet**.  
  
3.  Nell'elenco **Facet** selezionare **Configurazione superficie di attacco per Reporting Services**.  
  
4.  In **Proprietà facet**:  
  
    -   Per abilitare il servizio Web ReportServer, impostare **WebServiceAndHTTPAccessEnabled** su **True**.  
  
    -   Per disabilitare il servizio Web ReportServer, impostare **WebServiceAndHTTPAccessEnabled** su **False**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="scheduled-events-and-delivery"></a><a name="Sched"></a> Recapito ed eventi pianificati  
  
#### <a name="to-turn-on-or-off-scheduled-events-and-delivery-by-editing-configuration"></a>Per abilitare o disabilitare il recapito e gli eventi pianificati mediante la modifica della configurazione  
  
1.  Aprire il file RsReportServer.config in un editor di testo. Per altre informazioni, vedere [Modificare un file di configurazione di Reporting Services&#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
2.  Per abilitare l'elaborazione e il recapito pianificati dei report, impostare **IsSchedulingService**, **IsNotificationService** e **IsEventService** su **true**:  
  
    ```  
    <IsSchedulingService>true</IsSchedulingService>  
    <IsNotificationService>true</IsNotificationService>  
    <IsEventService>true</IsEventService>  
    ```  
  
3.  Per disabilitare l'elaborazione e il recapito pianificati dei report, impostare **IsSchedulingService**, **IsNotificationService** e **IsEventService** su **false**:  
  
    ```  
    <IsSchedulingService>false</IsSchedulingService>  
    <IsNotificationService>false</IsNotificationService>  
    <IsEventService>false</IsEventService>  
    ```  
  
4.  Salvare le modifiche, quindi chiudere il file.  
  
> [!NOTE]  
>  Non è possibile disabilitare completamente l'elaborazione in background, in quanto fornisce funzionalità di manutenzione dei database necessarie per le operazioni del server.  
  
##  <a name="web-portal"></a><a name="WebPortal"></a> Portale Web
  
A partire da SQL Server 2016 Reporting Services aggiornamento cumulativo 2, il portale Web sarà sempre abilitato.
  
##  <a name="windows-integrated-security"></a><a name="WinIntSec"></a> Sicurezza integrata di Windows  
  
### <a name="to-turn-on-or-off-windows-integrated-security-by-using-sql-server-management-studio"></a>Per attivare o disattivare la sicurezza integrata di Windows tramite SQL Server Management Studio  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e connettersi all'istanza di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che si desidera configurare.  
  
2.  In Esplora oggetti fare clic con il pulsante destro del mouse sul nodo di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e scegliere **Proprietà**.  
  
3.  In **Selezione pagina** nella finestra di dialogo **Proprietà server** selezionare **Sicurezza**.  
  
    -   Per attivare la sicurezza integrata di Windows, selezionare l'opzione **Abilita la sicurezza integrata di Windows per le origini dati dei report**.  
  
    -   Per disattivare la sicurezza integrata di Windows, deselezionare l'opzione **Abilita la sicurezza integrata di Windows per le origini dati dei report**.  
  
4.  Selezionare **OK**.  
  
## <a name="see-also"></a>Vedere anche  
[Gestione configurazione server di report (modalità nativa)](../install-windows/reporting-services-configuration-manager-native-mode.md)

 Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
  
