---
description: Eventi registrati dal servizio Integration Services
title: Eventi registrati dal servizio Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- service [Integration Services], events
- events [Integration Services], service
- Integration Services service, events
ms.assetid: d4122dcf-f16f-47a0-93a2-ffa3d0d4f9cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 45612ea0bfdbda2b27e6cac8b4deb1e302eb9298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346556"
---
# <a name="events-logged-by-the-integration-services-service"></a>Eventi registrati dal servizio Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Quando l'esecuzione del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene avviata, arrestata o quando si verificano determinati problemi, vengono registrati vari messaggi di evento nel registro eventi applicazioni di Windows.  
  
 In questo argomento vengono fornite informazioni sui messaggi di evento comuni registrati dal servizio nel registro eventi applicazioni. Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] registra tutti i messaggi descritti in questo argomento con un'origine evento di SQLISService.  
  
 Per informazioni generali sul servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Servizio Integration Services &#40;servizio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
## <a name="service-status-messages"></a>Messaggi di stato del servizio
 Quando si seleziona [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per l'installazione, viene installato e avviato il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e l'opzione Tipo di avvio è impostata su Automatico.  
  
|ID evento|Nome simbolico|Text|Note|  
|--------------|-------------------|----------|-----------|  
|256|DTS_MSG_SERVER_STARTING|È in corso l'avvio del servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].|Il servizio sta per essere avviato.|  
|257|DTS_MSG_SERVER_STARTED|Servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] avviato.|Il servizio è stato avviato.|  
|260|DTS_MSG_SERVER_START_FAILED|Impossibile avviare il servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].%nErrore: %1|Non è stato possibile avviare il servizio. L'impossibilità di avviare il servizio potrebbe dipendere da un'installazione danneggiata o da un account del servizio non corretto.|  
|258|DTS_MSG_SERVER_STOPPING|È in corso l'arresto del servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].%n%nArresta tutti i pacchetti in esecuzione all'uscita: %1|Il servizio sta per essere arrestato e, se la configurazione lo prevede, verranno arrestati tutti i pacchetti in esecuzione. È possibile impostare un valore True o False nel file di configurazione per specificare se arrestare i pacchetti in esecuzione quando viene arrestato il servizio. Nel messaggio relativo a questo evento è incluso il valore di questa impostazione.|  
|259|DTS_MSG_SERVER_STOPPED|Servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] arrestato.%nVersione server %1|Il servizio è stato arrestato.|  
  
## <a name="settings-file-messages"></a>Messaggi dei file di impostazioni  
 Le impostazioni per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vengono archiviate in un file XML che è possibile modificare. Per altre informazioni, vedere [Servizio Integration Services &#40;servizio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
|ID evento|Nome simbolico|Text|Note|  
|--------------|-------------------|----------|-----------|  
|274|DTS_MSG_SERVER_MISSING_CONFIG_REG|Servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]: %nL'impostazione del Registro di sistema che specifica il file di configurazione non esiste. %nVerrà eseguito un tentativo di caricamento del file di configurazione predefinito.|La voce del Registro di sistema che contiene il percorso del file di configurazione non esiste o è vuota.|  
|272|DTS_MSG_SERVER_MISSING_CONFIG|Il file di configurazione del servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] non esiste.%nIl servizio verrà caricato con le impostazioni predefinite.|Il file di configurazione non esiste nel percorso specificato.|  
|273|DTS_MSG_SERVER_BAD_CONFIG|Il file di configurazione del servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] non è corretto.%nErrore durante la lettura del file di configurazione:%1%n%nIl servizio verrà caricato con le impostazioni predefinite.|Non è possibile leggere il file di configurazione o il file non è valido. Questo errore potrebbe essere determinato da un errore di sintassi di XML nel file.|  
  
## <a name="other-messages"></a>Altri messaggi  
  
|ID evento|Nome simbolico|Text|Note|  
|--------------|-------------------|----------|-----------|  
|336|DTS_MSG_SERVER_STOPPING_PACKAGE|Servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]: è in corso l'arresto del pacchetto in esecuzione.%nID istanza pacchetto: %1%nID pacchetto: %2%nNome pacchetto: %3%nDescrizione pacchetto: %4%nPacchetto|Il servizio sta tentando di arrestare l'esecuzione di un pacchetto. È possibile monitorare e arrestare i pacchetti in esecuzione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Per altre informazioni su come gestire i pacchetti in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vedere [Gestione dei pacchetti &#40;servizio SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).|  

## <a name="view-events"></a>Visualizzare eventi
  Sono disponibili due strumenti nei quali è possibile visualizzare eventi per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   La finestra di dialogo **Visualizzatore file di log** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Nella finestra di dialogo **Visualizzatore file di log** sono disponibili opzioni per l'esportazione, l'applicazione di filtri e l'esecuzione di ricerche nel log. Per altre informazioni sulle opzioni della finestra di dialogo **Visualizzatore file di log**, vedere [Guida sensibile al contesto del Visualizzatore file di log](../../relational-databases/logs/log-file-viewer-f1-help.md).  
  
-   Visualizzatore eventi di Windows.  
  
 Per descrizioni degli eventi registrati dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vedere [Eventi registrati dal servizio Integration Services](../../integration-services/service/events-logged-by-the-integration-services-service.md).  
  
### <a name="to-view-service-events-for-integration-services-in-sql-server-management-studio"></a>Per visualizzare gli eventi del servizio per Integration Services in SQL Server Management Studio  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Scegliere **Connetti Esplora oggetti** dal menu **File**.  
  
3.  Nella finestra di dialogo **Connetti al server** selezionare il tipo di server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , selezionare o individuare il server a cui connettersi e quindi fare clic su **Connetti**.  
  
4.  In Esplora oggetti fare clic con il pulsante destro del mouse su [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e quindi scegliere **Visualizza log**.  
  
5.  Per visualizzare gli eventi di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , selezionare **SQL Server Integration Services**. L'opzione **Eventi NT** , selezionata automaticamente, viene deselezionata quando si seleziona l'opzione **SQL Server Integration Services** .  
  
### <a name="to-view-service-events-for-integration-services-in-windows-event-viewer"></a>Per visualizzare gli eventi del servizio per Integration Services in Windows nel Visualizzatore eventi  
  
1.  Nel **Pannello di controllo** fare clic su **Strumenti di amministrazione** nella visualizzazione classica oppure su **Prestazioni e manutenzione** e quindi su **Strumenti di amministrazione** nella visualizzazione per categorie.  
  
2.  Fare clic su **Visualizzatore eventi**.  
  
3.  Nella finestra di dialogo **Visualizzatore eventi** fare clic su **Applicazione**.  
  
4.  Nello snap-in **Applicazione** individuare nella colonna **Origine** la voce con valore **SQLISService**, fare clic con il pulsante destro del mouse sulla voce e quindi scegliere **Proprietà**.  
  
5.  Facoltativamente, fare clic sulla freccia SU o freccia GIÙ per visualizzare l'evento precedente o successivo.  
  
6.  Facoltativamente, fare clic sul pulsante Copia negli Appunti per copiare le informazioni sugli eventi.  
  
7.  Impostare la visualizzazione dei dati degli eventi in base a byte o parole.  
  
8.  Fare clic su **OK**.  
  
9. Scegliere **Esci** dal menu **File** per chiudere la finestra di dialogo **Visualizzatore eventi** .  
 
## <a name="related-tasks"></a>Attività correlate  
 Per informazioni su come visualizzare le voci di log, vedere [Eventi registrati da un pacchetto di Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
