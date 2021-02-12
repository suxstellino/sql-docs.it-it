---
description: Aggiungere un ulteriore front-end Web di Reporting Services a una farm
title: Aggiungere un ulteriore front-end Web di Reporting Services a una farm | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: d7a11bda-ae26-49ac-b071-37d83cae5afe
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 50bee52344ee56d83e1dc86cf9d4f8208c4c8c68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061116"
---
# <a name="add-an-additional-reporting-services-web-front-end-to-a-farm"></a>Aggiungere un ulteriore front-end Web di Reporting Services a una farm
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] La modalità SharePoint include i componenti necessari per server applicazioni e server front-end Web (WFE). Questo argomento è incentrato sull'installazione dei componenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] obbligatori per un server WFE, incluse le pagine di applicazione utilizzate dalle funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ad esempio sottoscrizioni, avvisi dati e [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]. L'installazione primaria di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] necessaria per un server WFE consiste nell'installare il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per i prodotti SharePoint 2016.  
  
## <a name="prerequisites"></a>Prerequisiti  
  
-   Per eseguire il programma di installazione di SQL Server, è necessario essere un amministratore locale.  
  
-   Il computer deve fare parte di un dominio.  
  
-   È inoltre necessario conoscere il nome del server di database esistente che ospita i database di configurazione e del contenuto di SharePoint.  
  
-   Il server di database deve essere configurato in modo da consentire le connessioni ai database remoti.  In caso contrario, non sarà possibile creare un join del nuovo server alla farm, in quanto il nuovo server non sarà in grado di stabilire una connessione ai database di configurazione di SharePoint.  
  
-   Nel nuovo server dovrà essere installata la stessa versione di SharePoint in esecuzione sui server della farm correnti. Se nella farm è già installato, ad esempio, SharePoint 2013 Service Pack 1 (SP1), sarà necessario installare SP1 anche nel nuovo server prima di poterne creare un join alla farm.  
  
## <a name="steps"></a>Passaggi  
 Nei passaggi di questo argomento si presuppone che l'installazione e la configurazione del server vengano eseguite da un amministratore della farm di SharePoint. Nel diagramma è illustrato un tipico ambiente a tre livelli e gli elementi numerati nel diagramma sono descritti nell'elenco seguente:  
  
-   (1) Più server front-end Web (WFE). I server front-end Web richiedono il componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per SharePoint 2010. Nei passaggi seguenti viene aggiunto un secondo server applicazioni a questo livello.  
  
-   (2) Due server applicazioni che eseguono [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e siti Web, ad esempio Amministrazione centrale.  
  
-   (3) Due server di database SQL Server.  
  
-   (4) Rappresenta una soluzione software o hardware di bilanciamento del carico di rete.  
  
 ![Aggiungere SSRS e un nuovo front-end Web di SharePoint](../../reporting-services/install-windows/media/rs-sharepointscale-wfe.gif "Aggiungere SSRS e un nuovo front-end Web di SharePoint")  
  
 Nei seguenti passaggi si presuppone che l'installazione e la configurazione del server vengano eseguite da un amministratore.  
  
|Passaggio|Descrizione e collegamento|  
|----------|--------------------------|  
|Aggiungere un server di SharePoint a una farm.|Per distribuire un'altra applicazione di Reporting Services, è necessario installare SharePoint.<br/><br/>Per SharePoint 2013, vedere [Aggiungere un server SharePoint a una farm in SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Per SharePoint 2016, vedere [Aggiungere un server SharePoint a una farm in SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Installare il componente aggiuntivo SQL Server Reporting Services per i prodotti SharePoint 2016.|Esistono diversi metodi per l'installazione del componente aggiuntivo. Nei seguenti passaggi viene usata l'Installazione guidata di SQL Server. Per altre informazioni sull'installazione del componente aggiuntivo, vedere [Installare o disinstallare il componente aggiuntivo Reporting Services per SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)<br /><br /> 1) Eseguire l'installazione di SQL Server.<br /><br /> 2) Nella pagina **Impostazione ruolo** selezionare **Installazione funzionalità SQL Server**<br /><br /> 3) Nella pagina **Selezione funzionalità** selezionare **Componente aggiuntivo Reporting Services per prodotti SharePoint**<br /><br /> 4) Nelle pagine successive scegliere **Avanti** per completare le opzioni di installazione.<br /><br/>Per altre informazioni sull'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Installare il primo server di report in modalità SharePoint](install-the-first-report-server-in-sharepoint-mode.md)|  
|Verificare che il nuovo server di report sia operativo.|1) Nel gruppo **Impostazioni di sistema** di Amministrazione centrale SharePoint fare clic su **Gestisci server della farm** .<br /><br /> 2) Verificare che il nuovo server sia presente nell'elenco.|  
|Aggiornare la soluzione NLB.|Se necessario, aggiornare l'ambiente NLB hardware o software per includere il nuovo server.|  

## <a name="next-steps"></a>Passaggi successivi

[Aggiungere un server SharePoint a una farm in SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Aggiungere un server SharePoint a una farm in SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)