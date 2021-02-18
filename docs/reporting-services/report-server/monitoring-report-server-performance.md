---
title: Monitoraggio delle prestazioni del server di report | Microsoft Docs
description: Informazioni su come monitorare le prestazioni del server di report per valutare l'attività del server, osservare le tendenze, diagnosticare i colli di bottiglia e raccogliere dati sulla configurazione del sistema.
ms.date: 02/12/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- report servers [Reporting Services], performance
- counters [Reporting Services]
- monitoring performance [Reporting Services]
- SQL Server Reporting Services, performance
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: c1bc13d4-8297-4daf-bb19-4c1e5ba292a6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: deae632ce9305b58b72e24e1a57507f9f4c5ee89
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489435"
---
# <a name="monitoring-report-server-performance"></a>Monitoraggio delle prestazioni del server di report

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

  Con gli strumenti di monitoraggio delle prestazioni è possibile valutare l'attività del server di report, osservare le tendenze, diagnosticare i colli di bottiglia a livello di sistema e raccogliere dati che consentono di determinare più facilmente se la configurazione del sistema corrente è sufficiente. Per ottimizzare le prestazioni del server, è possibile specificare la frequenza di riciclo del dominio dell'applicazione del server di report. Per altre informazioni, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
## <a name="sources-of-performance-data"></a>Origini dei dati sulle prestazioni  
 Per raccogliere informazioni complete sulle prestazioni del sistema, è possibile utilizzare una combinazione di tecnologie e strumenti. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Nei sistemi operativi Windows Server le informazioni sulle prestazioni vengono fornite tramite gli strumenti seguenti:  
  
-   Gestione attività  
  
-   Visualizzatore eventi  
  
-   Monitoraggio delle prestazioni  
  
 In Gestione attività sono disponibili informazioni sui programmi e sui processi in esecuzione nel computer. È possibile utilizzare Gestione attività per monitorare i principali indicatori delle prestazioni del server di report nonché per valutare l'attività dei processi in esecuzione e visualizzare grafici e dati sull'utilizzo della CPU e della memoria. Per informazioni sull'utilizzo di Gestione attività, vedere la documentazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 È possibile utilizzare Visualizzatore eventi e performance monitor per creare registri e avvisi sull'elaborazione del report e sull'utilizzo delle risorse. Per informazioni su eventi di Windows generati da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Registro applicazioni di Windows](../../reporting-services/report-server/windows-application-log.md). Per informazioni su performance monitor, vedere "contatori delle prestazioni di Windows" più avanti in questo articolo.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le utilità, ad esempio [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md) o [gli eventi estesi](../../relational-databases/extended-events/extended-events.md), forniscono anche informazioni sul database del server di report e sui database temporanei utilizzati per la memorizzazione nella cache e la gestione delle sessioni.  
  
## <a name="windows-performance-counters"></a>Contatori delle prestazioni di Windows  
 Il monitoraggio di contatori delle prestazioni specifici consente di:  
  
-   Stimare i requisiti di sistema necessari per supportare un carico di lavoro previsto.  
  
-   Creare un riferimento per le prestazioni che consenta di misurare l'effetto delle modifiche alla configurazione o degli aggiornamenti applicativi.  
  
-   Monitorare le prestazioni dell'applicazione in presenza di determinati carichi, sia reali sia generati artificialmente.  
  
-   Verificare che gli aggiornamenti hardware abbiano l'effetto desiderato sulle prestazioni.  
  
-   Convalidare le modifiche apportate alla configurazione del sistema per verificare che abbiano l'effetto desiderato sulle prestazioni.  

  
## <a name="reporting-services-performance-objects"></a>Oggetti prestazione di Reporting Services  
SQL Server 2016 Reporting Services include gli oggetti prestazione seguenti:  
  
-   **MSRS 2016 Web Service** e **MSRS 2016 Web Service SharePoint Mode** per monitorare le prestazioni del server di report. In questi oggetti prestazione è inclusa una raccolta di contatori che consentono di tenere traccia delle elaborazioni nel server di report avviate in genere da operazioni di visualizzazione dei report interattive. Questi contatori vengono reimpostati ogni volta che il servizio Web ReportServer viene arrestato o riciclato.  
  
-   **MSRS 2016 Windows Service** e **MSRS 2016 Windows Service SharePoint Mode** per monitorare le operazioni pianificate e il recapito del report. In questi oggetti prestazione è inclusa una raccolta di contatori che consentono di tenere traccia delle elaborazioni di report avviate tramite operazioni pianificate, nelle quali sono incluse sottoscrizioni e recapiti, snapshot delle esecuzioni dei report e cronologie dei report.  
  
-   **ReportServer:Service** e **ReportServerSharePoint:Service** per il monitoraggio degli eventi correlati ad HTTP e per la gestione della memoria. Questi contatori sono specifici di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e consentono di tenere traccia degli eventi correlati ad HTTP per il server di report, ad esempio richieste, connessioni e tentativi di accesso. Questo oggetto prestazione, inoltre, include contatori correlati alla gestione della memoria.  
  
 Se sono presenti più istanze del server di report in uno stesso computer, è possibile scegliere se monitorare le istanze insieme o separatamente. Scegliere quali istanze includere quando si aggiunge un contatore. Per ulteriori informazioni sull'utilizzo di performance monitor (Perfmon. msc) e sull'aggiunta di contatori, vedere la [!INCLUDE[msCoName](../../includes/msconame-md.md)] documentazione del prodotto [Windows Performance Monitor](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749249(v=ws.11)) .  
  
## <a name="other-performance-counters"></a>Altri contatori delle prestazioni  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]I contatori delle prestazioni personalizzati sono disponibili solo per gli oggetti prestazioni Reporting Services elencati sopra. Gli oggetti prestazioni .NET Framework seguenti forniscono dati di monitoraggio delle prestazioni aggiuntivi per il server di report.
 
 > [!NOTE]
 > Server di report di Power BI e SQL Server Reporting Services 2017 e versioni successive non includono Reporting Services oggetti prestazione. Sono disponibili [.NET Framework contatori delle prestazioni](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/performance-counters) per fornire il monitoraggio delle prestazioni per il server di report. 
 
|Oggetto prestazione|Note|  
|------------------------|-----------|  
|**.NET CLR Data** e **.NET CLR Memory**|Il portale Web usa i contatori delle prestazioni di [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Per altre informazioni, scaricare [miglioramento delle prestazioni e della scalabilità delle applicazioni .NET](https://www.microsoft.com/download/details.aspx?id=11711).|  
|**Processo**|Aggiungere i contatori delle prestazioni **Elapsed Time** e **ID Process** affinché un'istanza ReportingServicesService registri il tempo di attività del processo in base all'ID processo.|  
  
## <a name="sharepoint-events"></a>Eventi di SharePoint  
 Oltre agli oggetti relativi alle prestazioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] può essere necessario configurare eventi di SharePoint se si esegue un server di report in modalità integrata SharePoint e l'ambiente di report è stato configurato per l'uso di un prodotto SharePoint. In questa sezione utilizzare Eventi per un server di report in modalità integrata SharePoint per esaminare gli eventi di diagnostica che potrebbero fornire informazioni utili se l'ambiente di report è integrato con SharePoint.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Contatori delle prestazioni per gli oggetti prestazioni MSRS 2016 Web Service e MSRS 2016 Windows Service &#40;modalità nativa&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)  
 Descrive i contatori delle prestazioni utilizzati dal servizio Web ReportServer.  
  
 [Contatori delle prestazioni per gli oggetti prestazioni MSRS 2016 Web Service SharePoint Mode e MSRS 2016 Windows Service SharePoint Mode &#40;modalità SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
 Descrive i contatori delle prestazioni utilizzati dal servizio Windows ReportServer.  
  
 [Contatori delle prestazioni per gli oggetti prestazioni ReportServer:Service e ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
 Vengono descritti i contatori delle prestazioni correlati ad HTTP e alla memoria in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Server di report di Reporting Services &#40;modalità nativa&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Strumenti di Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  
  
