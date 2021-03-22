---
title: Risoluzione dei problemi dei servizi Report di Reporting
description: In questo articolo vengono risolti problemi relativi alla progettazione, alla creazione di un anteprima e all'esportazione di un report, oltre alla sua pubblicazione in un server di report in modalità nativa o SharePoint.
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: a705d103-85b1-49b5-b27f-332b1040d029
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8812cfc8ac70da47da0414534ff380dd07686037
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104792948"
---
# <a name="troubleshoot--reporting-services-report-issues"></a>Risoluzione dei problemi dei servizi Report di Reporting
Questo argomento contiene informazioni utili per risolvere i problemi relativi alla progettazione di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] , alla visualizzazione in anteprima di un report, alla pubblicazione di un report in un server di report in modalità nativa o in modalità SharePoint, alla visualizzazione di un report nel server di report o all'esportazione di un report in un formato di file diverso.  
## <a name="monitor-report-servers"></a>Monitorare i server di report  
Per monitorare l'attività del server di report, è possibile utilizzare gli strumenti del sistema e del database. È inoltre possibile visualizzare file di log di traccia oppure eseguire query sul log di esecuzione del server di report per ottenere informazioni dettagliate su report specifici. Se si utilizza Performance Monitor, è possibile aggiungere contatori delle prestazioni per il servizio Web ReportServer e il servizio Windows ReportServer per identificare colli di bottiglia nell'elaborazione su richiesta o pianificata.  
Per altre informazioni, vedere [Monitoraggio delle prestazioni del server di report](../report-server/monitoring-report-server-performance.md).  
  
  
## <a name="view-the-report-server-logs"></a>Visualizzare i log del server di report  
[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] registra molti eventi interni ed esterni nei file di log che contengono i dati su report specifici, informazioni di debug, richieste e risposte HTTP ed eventi del server di report. È possibile creare i registri di prestazioni e scegliere i contatori delle prestazioni che specificano i dati da raccogliere. La directory predefinita per i file di log per un'installazione predefinita è `<drive>\Program Files\Microsoft SQL Server\MSRS130.MSSQLSERVER\Reporting Services\LogFiles`.   
  
Per altre informazioni, vedere [File di log e origini di Reporting Services](../report-server/reporting-services-log-files-and-sources.md).  
  
Per stabilire in particolare se le attese del report sono dovute al recupero dei dati, all'elaborazione del report o al rendering del report, usare il log di esecuzione. Per ulteriori informazioni, vedere [ExecutionLog del server di report e la vista ExecutionLog3](../report-server/report-server-executionlog-and-the-executionlog3-view.md).   
  
## <a name="view-the-call-stack-for-report-processing-error-messages-on-the-report-server"></a>Visualizzare lo stack di chiamate per i messaggi di errore di elaborazione del report nel server di report  
Quando si visualizza un report pubblicato in Gestione report, è possibile che venga visualizzato un messaggio di errore che rappresenta un errore generico sull'elaborazione o sul rendering. Per ulteriori informazioni, è possibile visualizzare lo stack di chiamate.   
  
Per visualizzare lo stack di chiamate, accedere al server di report usando le credenziali di amministratore locale, fare clic con il pulsante destro del mouse sulla pagina Gestione report e scegliere **Visualizza origine**. Nello stack di chiamate vengono fornite le informazioni dettagliate di contesto per il messaggio di errore.  
  
## <a name="use-ssmanstudiofull-to-verify-queries-and-credentials"></a>Usare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] per verificare le query e le credenziali  
È possibile usare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] per convalidare query complesse prima di includerle nel report.   
  
Per ulteriori informazioni, vedere [motore di database editor di query](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md) e [gestire oggetti tramite Esplora oggetti](../../ssms/object/manage-objects-by-using-object-explorer.md).  
  
## <a name="analyze-problem-reports-with-report-data-cached-on-the-client"></a>Analizzare le segnalazioni di problemi con i dati dei report memorizzati nella cache nel client  
Quando viene creato un report in Business Intelligence Development Studio, il client di creazione memorizza i dati nella cache dati in un file rdl.data che viene usato durante la visualizzazione in anteprima del report. Ogni volta che la query viene modificata, la cache viene aggiornata. Per eseguire il debug dei problemi dei report, talvolta può risultare utile impedire l'aggiornamento dei dati dei report in modo che i dati non vengono modificati mentre si esegue il debug.   
  
Per definire se [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull.md)] può usare solo dati memorizzati nella cache, aggiungere la sezione seguente al file devenv.exe.config in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio.md)]. Il percorso della directory predefinita è: `<drive>:Program Files\Microsoft Visual Studio 10.0\Common7\IDE`.   
  
```  
<system.diagnostics>  
      <switches>  
         <add name="Microsoft.ReportDesigner.ReportPreviewStore.ForceCache" value="1" />  
      </switches>  
   </system.diagnostics>  
```  
Se il valore è impostato su 1, vengono utilizzati solo i dati dei report memorizzati nella cache. Assicurarsi di rimuovere questa sezione quando è terminato il debug del report.  
  
## <a name="see-also"></a>Vedere anche  
[Errori ed eventi (Reporting Services)](errors-and-events-reference-reporting-services.md)  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]
