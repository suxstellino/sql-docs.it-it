---
description: Aggiungere un ulteriore server di report a una farm (con scalabilità orizzontale SSRS)
title: Aggiungere un ulteriore server di report a una farm (con scalabilità orizzontale SSRS) | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: c1a6b683-15cf-44ae-ac60-ceee63a60aaf
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: e0f1807d8050408ca7e6153d9f7b9189b004bc8a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101973"
---
# <a name="add-an-additional-report-server-to-a-farm-ssrs-scale-out"></a>Aggiungere un ulteriore server di report a una farm (con scalabilità orizzontale SSRS)

  L'aggiunta di un secondo o altri server di report in modalità SharePoint alla farm di SharePoint può migliorare le prestazioni e il tempo di risposta dell'elaborazione del server di report. Se le prestazioni risultano rallentate dopo aver aggiunto altri utenti, report e applicazioni al server di report, l'aggiunta di ulteriori server di report può migliorare le prestazioni. È inoltre consigliabile aggiungere un secondo server di report per aumentare la disponibilità dei server di report in caso di problemi hardware o di manutenzione generale di singoli server di report nell'ambiente. A partire dalla versione [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , la procedura per la scalabilità orizzontale dell'ambiente [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità SharePoint segue la distribuzione standard della farm di SharePoint e sfrutta le funzionalità di bilanciamento del carico di SharePoint.  
  
> [!IMPORTANT]  
>  La distribuzione con scalabilità orizzontale di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non è supportata in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere la sezione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] di [Funzionalità supportate dalle edizioni di SQL Server](~/sql-server/editions-and-components-of-sql-server-2017.md#SSRS).  
  
> [!TIP]  
>  A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] non si usa Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per aggiungere server e per aumentare il numero di istanze dei server di report. Con i prodotti SharePoint è possibile gestire la distribuzione con scalabilità orizzontale di Reporting Services in quanto i server SharePoint con il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vengono aggiunti alla farm.  
  
 Per altre informazioni su come aumentare il numero di istanze dei server di report in modalità nativa, vedere [Configurare una distribuzione con scalabilità orizzontale di un server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
##  <a name="load-balancing"></a><a name="bkmk_loadbalancing"></a> Bilanciamento del carico  
 Il bilanciamento del carico di applicazioni del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene gestito automaticamente da SharePoint, a meno che nell'ambiente sia presente una soluzione di bilanciamento del carico personalizzata o di terze parti. Secondo il comportamento predefinito del bilanciamento del carico di SharePoint, ogni applicazione del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene bilanciata tra tutti i server applicazioni in cui è stato avviato il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per verificare se il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è installato e avviato, fare clic su **Gestisci servizi nel server** in Amministrazione centrale SharePoint.  
  
##  <a name="prerequisites"></a><a name="bkmk_prerequisites"></a> Prerequisiti  
  
-   Per eseguire il programma di installazione di SQL Server, è necessario essere un amministratore locale.  
  
-   Il computer deve fare parte di un dominio.  
  
-   È inoltre necessario conoscere il nome del server di database esistente che ospita i database di configurazione e del contenuto di SharePoint.  
  
-   Il server di database deve essere configurato in modo da consentire le connessioni ai database remoti.  In caso contrario, non sarà possibile creare un join del nuovo server alla farm, in quanto il nuovo server non sarà in grado di stabilire una connessione ai database di configurazione di SharePoint.  
  
-   Nel nuovo server dovrà essere installata la stessa versione di SharePoint in esecuzione sui server della farm correnti. Se nella farm è già installato, ad esempio, SharePoint 2013 Service Pack 1 (SP1), sarà necessario installare SP1 anche nel nuovo server prima di poterne creare un join alla farm.  
  
##  <a name="steps"></a><a name="bkmk_steps"></a> Passaggi  
 Nei passaggi di questo argomento si presuppone che l'installazione e la configurazione del server vengano eseguite da un amministratore della farm di SharePoint. Nel diagramma è illustrato un tipico ambiente a tre livelli e gli elementi numerati nel diagramma sono descritti nell'elenco seguente:  
  
-   (1) Più server front-end Web (WFE). I server front-end Web richiedono il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per SharePoint 2016.  
  
-   (2) Un singolo server applicazioni che esegue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e siti Web, ad esempio Amministrazione centrale. Nei passaggi seguenti viene aggiunto un secondo server applicazioni a questo livello.  
  
-   (3) Due server di database SQL Server.  
  
-   (4) Rappresenta una soluzione software o hardware di bilanciamento del carico di rete.  
  
 ![Aggiunta di un server applicazioni a Reporting Services](../../reporting-services/install-windows/media/rs-sharepointscale.gif "Aggiunta di un server applicazioni a Reporting Services")  
  
 Nei seguenti passaggi si presuppone che l'installazione e la configurazione del server vengano eseguite da un amministratore. Il server verrà configurato come nuovo server applicazioni nella farm e non verrà utilizzato come un front-end Web.  
  
|Passaggio|Descrizione e collegamento|  
|----------|--------------------------|  
|Aggiungere un server di SharePoint a una farm.|Per distribuire un'altra applicazione di Reporting Services, è necessario installare SharePoint.<br/><br/>Per SharePoint 2013, vedere [Aggiungere un server SharePoint a una farm in SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Per SharePoint 2016, vedere [Aggiungere un server SharePoint a una farm in SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Installare e configurare la modalità SharePoint di Reporting Services|Eseguire l'installazione di SQL Server. Per altre informazioni sull'installazione della modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Installare il primo server di report in modalità SharePoint](install-the-first-report-server-in-sharepoint-mode.md)<br /><br /> Se il server viene usato solo come server applicazioni e non come un front-end Web, non è necessario selezionare **Componente aggiuntivo Reporting Services per prodotti SharePoint**.<br /><br /> 1) Nella pagina **Impostazione ruolo** selezionare **Installazione funzionalità SQL Server**<br /><br /> 2) Nella pagina **Selezione funzionalità** selezionare **Reporting Services - SharePoint**<br /><br /> 3) Nella pagina **Configurazione di Reporting Services**  verificare che l'opzione **Solo installazione** sia selezionata per **Modalità SharePoint di Reporting Services**.|  
|Verificare che Reporting Services sia operativo.|1) Nel gruppo **Impostazioni di sistema** di Amministrazione centrale SharePoint fare clic su **Gestisci server della farm** .<br /><br /> 2) Verificare il **Servizio SQL Server Reporting Services**.<br /><br />Per altre informazioni, vedere [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)|  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional"></a> Configurazione aggiuntiva  
 È possibile ottimizzare singoli server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in una distribuzione con scalabilità orizzontale solo per eseguire l'elaborazione in background, in modo da non condividere le risorse con esecuzione interattiva del report. Nell'elaborazione in background sono incluse pianificazioni, sottoscrizioni e avvisi dati.  
  
 Per cambiare il comportamento di singoli server di report, impostare **\<IsWebServiceEnable>** su False nel file di configurazione **RSreportServer.config**.  
  
 Per impostazione predefinita, i server di report vengono configurati con \<IsWebServiceEnable> impostato su TRUE. Quando tutti i server sono configurati per TRUE, l'elaborazione interattiva e in background sarà con carico bilanciato in tutti i nodi nella farm.  
  
 Se si configurano tutti i server di report con \<IsWebServiceEnable> impostato su False, verrà visualizzato un messaggio di errore simile al seguente quando si tenta di usare le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
```output
The Reporting Services Web Service is not enabled. Configure at least one instance of the Reporting Services SharePoint Service to have <IsWebServiceEnable> set to true.
```
 
 Per altre informazioni, vedere [Modificare un file di configurazione di Reporting Services&#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  

## <a name="next-steps"></a>Passaggi successivi

[Aggiungere un server SharePoint a una farm in SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Aggiungere un server SharePoint a una farm in SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)