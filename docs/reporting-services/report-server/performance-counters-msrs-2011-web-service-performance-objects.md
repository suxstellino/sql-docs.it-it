---
title: Contatori delle prestazioni MSRS 2016 modalità nativa, oggetti prestazioni | Microsoft Docs
description: Informazioni sui contatori delle prestazioni per gli oggetti prestazioni MSRS 2016 Web Service e MSRS 2016 Windows Service.
ms.date: 02/17/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- Report Server Web service, performance counters
- Reporting Services performance object
- RS Web Service performance object
- counters [Reporting Services]
- performance [Reporting Services]
ms.assetid: c642fc4f-8734-4626-a194-42ac9cd8e2ef
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 1a6704706a152073cdfa1367aed5c0d194834bec
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646333"
---
# <a name="performance-counters-msrs-2016-native-mode-performance-objects"></a>Contatori delle prestazioni MSRS 2016 modalità nativa, oggetti prestazioni
  Questo argomento descrive i contatori delle prestazioni per gli oggetti prestazioni **MSRS 2016 Web Service** e **MSRS 2016 Windows Service** . Questi oggetti fanno parte di un SQL Server 2016 Reporting Services la distribuzione in modalità nativa.  
  
> [!NOTE]  
>  Tramite questi oggetti prestazioni vengono monitorati gli eventi nel server di report locale. Se si esegue un server di report in una distribuzione con scalabilità orizzontale, i conteggi si applicano al server corrente e non alla distribuzione con scalabilità orizzontale.  
  
 Gli oggetti prestazioni sono disponibili in Monitoraggio prestazioni di Windows (**Perfmon.exe**). Per altre informazioni, vedere la documentazione di Windows, [Profilatura di runtime](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 Per informazioni correlate ai contatori delle prestazioni in modalità SharePoint, vedere [contatori delle prestazioni per gli oggetti prestazioni MSRS 2016 Web Service SharePoint Mode e MSRS 2016 Windows Service SharePoint mode &#40;modalità sharepoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md).  
  
 In questo argomento  
  
-   [Contatori delle prestazioni del servizio Web MSRS 2016](#bkmk_webservice)  
  
-   [Contatori delle prestazioni del servizio Windows MSRS 2016](#bkmk_windowsservice)  
  
-   [Utilizzare i cmdlet di PowerShell per restituire gli elenchi](#bkmk_powershell)  
  
##  <a name="msrs-2016-web-service-performance-counters"></a><a name="bkmk_webservice"></a> Contatori delle prestazioni del servizio Web MSRS 2016  
 L'oggetto prestazione del **servizio Web MSRS 2016** monitora le prestazioni del server di report. Questo oggetto prestazione include una raccolta di contatori che consentono di tenere traccia delle elaborazioni nel server di report avviate in genere da operazioni di visualizzazione dei report interattive. Quando si configura questo contatore, è possibile applicarlo a tutte le istanze di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] oppure è possibile selezionare istanze specifiche. I contatori vengono reimpostati ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .  
  
 La tabella seguente elenca i contatori inclusi nell'oggetto prestazioni **MSRS 2016 Web Service** .  
  
|Contatore|Descrizione|  
|-------------|-----------------|  
|**Sessioni attive**|Numero di sessioni attive. Tramite questo contatore viene restituito un conteggio cumulativo di tutte le sessioni del browser generate dalle esecuzioni dei report, indipendentemente dal fatto che siano ancora attive o meno.<br /><br /> Quando i record di sessione vengono rimossi, il contatore viene diminuito. Per impostazione predefinita, le sessioni vengono rimosse dopo dieci minuti di inattività.|  
|**Riscontri nella cache/sec**|Numero di richieste al secondo di report memorizzati nella cache. Si tratta di richieste relative a report di nuovo visualizzabili, non delle richieste relative a report elaborati direttamente dalla cache. Vedere **Totale riscontri nella cache** più avanti in questo argomento.|  
|**Riscontri nella cache/sec (modelli semantici)**|Numero di richieste al secondo per il modello memorizzato nella cache. Si tratta di richieste relative a report di nuovo visualizzabili, non delle richieste relative a report elaborati direttamente dalla cache.|  
|**Mancati riscontri nella cache/sec**|Numero di richieste al secondo che non hanno restituito un report dalla cache. Consente di stabilire se le risorse utilizzate per la memorizzazione nella cache, su disco o in memoria, siano sufficienti.|  
|**Mancati riscontri nella cache/sec (modelli semantici)**|Numero di richieste al secondo tramite cui non è stato restituito alcun modello dalla cache. Consente di stabilire se le risorse utilizzate per la memorizzazione nella cache, su disco o in memoria, siano sufficienti.|  
|**Richieste prima sessione/sec**|Numero di nuove sessioni utente avviate al secondo dalla cache del server di report.|  
|**Riscontri nella cache in memoria/sec**|Numero di volte al secondo in cui è stato possibile recuperare i report dalla cache in memoria. La *cache in memoria* è una parte della cache che archivia i report nella memoria della CPU. Quando viene usata la cache in memoria, il server di report non esegue query su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per individuare il contenuto memorizzato nella cache.|  
|**Mancati riscontri nella cache in memoria/sec**|Numero di volte al secondo in cui non è stato possibile recuperare i report dalla cache in memoria.|  
|**Richieste sessione successiva/sec**|Numero di richieste al secondo di report aperti in una sessione esistente, ovvero i report di cui è stato eseguito il rendering da una sessione di snapshot.|  
|**Richieste di report**|Numero di report attualmente attivi e gestiti dal server di report.|  
|**Report eseguiti/sec**|Numero di report eseguiti al secondo. Fornisce informazioni statistiche sul volume dei report. Usare questo contatore con **Richieste/sec** per confrontare il numero di report eseguiti con il numero di richieste di report restituibili dalla cache.|  
|**Richieste/sec**|Numero di richieste al secondo inviate al server di report. Tramite questo contatore si tiene traccia di tutti i tipi di richieste gestite dal server di report.|  
|**Totale riscontri nella cache**|Numero totale di richieste di report alla cache dall'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Totale riscontri nella cache (modelli semantici)**|Numero totale di richieste per il modello dalla cache dopo l'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da ASP.NET.|  
|**Totale mancati riscontri nella cache**|Numero totale di volte in cui non è stato possibile recuperare un report dalla cache dall'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Utilizzare questo contatore per stabilire se lo spazio su disco e la memoria siano sufficienti.|  
|**Totale mancati riscontri nella cache (modelli semantici)**|Numero totale di volte in cui non è stato possibile restituire un modello dalla cache dopo l'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da ASP.NET. Utilizzare questo contatore per stabilire se lo spazio su disco e la memoria siano sufficienti.|  
|**Totale riscontri nella cache in memoria**|Numero totale di report memorizzati nella cache restituiti dalla cache in memoria dall'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . La *cache in memoria* è una parte della cache che archivia i report nella memoria della CPU. Quando viene usata la cache in memoria, il server di report non esegue query su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per individuare il contenuto memorizzato nella cache.|  
|**Totale mancati riscontri nella cache in memoria**|Numero totale di accessi alla cache in memoria non riusciti dall'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Totale elaborazioni non riuscite**|Numero di errori di elaborazione delle richieste per il servizio Web ReportServer.|  
|**Totale thread rifiutati**|Numero totale di thread rifiutati per l'elaborazione asincrona e gestiti in un secondo momento come processi sincroni nello stesso thread. Ogni origine dei dati viene elaborata in un thread. Se il volume dei thread supera la capacità, i thread non verranno sottoposti all'elaborazione asincrona e verranno elaborati in modo seriale.|  
|**Totale report eseguiti**|Numero totale di report eseguiti dall'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Richieste totali**|Numero totale di richieste inviate al server di report dall'avvio del servizio. Il contatore viene reimpostato ogni volta che il servizio Web ReportServer viene arrestato da [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
  
##  <a name="msrs-2016-windows-service-performance-counters"></a><a name="bkmk_windowsservice"></a> Contatori delle prestazioni del servizio Windows MSRS 2016  
 L'oggetto prestazioni **MSRS 2016 Windows Service** esegue il monitoraggio del servizio Windows ReportServer. Questo oggetto prestazione include una raccolta di contatori che consentono di tenere traccia delle elaborazioni di report avviate tramite operazioni pianificate, quali sottoscrizioni e recapiti, snapshot delle esecuzioni dei report e cronologie dei report. Quando si configura questo contatore, è possibile applicarlo a tutte le istanze di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] oppure è possibile selezionare istanze specifiche.  
  
 Nella tabella seguente sono elencati i contatori inclusi nell'oggetto prestazioni **MSRS 2016 Windows Service** .  
  
|Contatore|Descrizione|  
|-------------|-----------------|  
|**Sessioni attive**|Numero di sessioni attive archiviate nel database del server di report. Questo contatore restituisce un conteggio cumulativo di tutte le sessioni utilizzabili del browser generate dalle sottoscrizioni del report, indipendentemente dal fatto che siano attive o meno.|  
|**Scaricamenti cache/sec**|Numero di scaricamenti della cache al secondo.|  
|**Riscontri nella cache/sec**|Numero di richieste al secondo di report memorizzati nella cache. Si tratta di richieste relative a report di nuovo visualizzabili, non delle richieste relative a report elaborati direttamente dalla cache. Vedere **Totale riscontri nella cache** più avanti in questo argomento.|  
|**Riscontri nella cache/sec (modelli semantici)**|Numero di richieste al secondo per i modelli memorizzati nella cache.|  
|**Mancati riscontri nella cache/sec**|Numero di richieste al secondo che non hanno restituito un report dalla cache. Consente di stabilire se le risorse utilizzate per la memorizzazione nella cache, su disco o in memoria, siano sufficienti.|  
|**Mancati riscontri nella cache/sec (modelli semantici)**|Numero di richieste al secondo tramite cui non è stato restituito alcun modello dalla cache. Consente di stabilire se le risorse utilizzate per la memorizzazione nella cache, su disco o in memoria, siano sufficienti.|  
|**Recapiti/sec**|Numero di recapiti di report al secondo, da parte di tutte le estensioni per il recapito.|  
|**Eventi/sec**|Numero di eventi elaborati al secondo. Tra gli eventi monitorati sono inclusi **SnapshotUpdated** e **TimedSubscription**.|  
|**Richieste prima sessione/sec**|Numero di nuove sessioni di esecuzione del report create al secondo.|  
|**Riscontri nella cache in memoria/sec**|Numero di volte al secondo in cui è stato possibile recuperare i report dalla cache in memoria. La *cache in memoria* è una parte della cache che archivia i report nella memoria della CPU. Quando viene usata la cache in memoria, il server di report non esegue query su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per individuare il contenuto memorizzato nella cache.|  
|**Mancati riscontri nella cache in memoria/sec**|Numero di volte al secondo in cui non è stato possibile recuperare i report dalla cache in memoria.|  
|**Richieste sessione successiva/sec**|Numero di richieste al secondo di report aperti in una sessione esistente, ovvero i report di cui è stato eseguito il rendering da una sessione di snapshot.|  
|**Richieste di report**|Numero di report attualmente attivi e gestiti dal server di report. Utilizzare questo contatore per valutare la strategia utilizzata per la memorizzazione nella cache. Potrebbero essere presenti più richieste rispetto ai report generati.|  
|**Report eseguiti/sec**|Numero di report generati al secondo.|  
|**Richieste/sec**|Numero totale di richieste elaborate correttamente al secondo da parte del servizio ReportServer.|  
|**Aggiornamenti snapshot/sec**|Numero totale di aggiornamenti di snapshot dell'esecuzione dei report al secondo.|  
|**Totale ricicli dominio applicazione**|Numero totale di cicli del dominio applicazione dopo l'avvio del servizio Windows ReportServer.|  
|**Totale scaricamenti cache**|Numero totale di aggiornamenti della cache del server di report dall'avvio del servizio. Il contatore viene reimpostato dopo il riciclo del dominio applicazione. Vedere **Scaricamenti cache/sec**.|  
|**Totale riscontri nella cache**|Numero totale di richieste di report elaborate direttamente dalla cache dall'avvio del servizio Windows ReportServer. Il contatore viene reimpostato dopo il riciclo del dominio applicazione. Vedere **Riscontri nella cache/sec**.|  
|**Totale riscontri nella cache (modelli semantici)**|Numero totale di richieste per i modelli elaborate direttamente dalla cache dopo l'avvio del servizio Windows ReportServer. Il contatore viene reimpostato dopo il riciclo del dominio applicazione. Vedere **Riscontri nella cache/sec**.|  
|**Totale mancati riscontri nella cache**|Numero totale di volte in cui non è stato possibile recuperare un report dalla cache dopo l'avvio del servizio Windows ReportServer. Il contatore viene reimpostato dopo il riciclo del dominio applicazione. Vedere **Mancati riscontri nella cache/sec**.|  
|**Totale mancati riscontri nella cache (modelli semantici)**|Numero totale di volte in cui non è stato possibile restituire un modello dalla cache dopo l'avvio del servizio Windows ReportServer. Il contatore viene reimpostato dopo il riciclo del dominio applicazione.|  
|**Totale recapiti**|Numero totale di report recapitati tramite Elaborazione pianificazione e recapito, per tutte le estensioni per il recapito. Il contatore viene reimpostato dopo il riciclo del dominio applicazione.|  
|**Totale eventi**|Numero totale di eventi dopo l'avvio del servizio Windows ReportServer. Il contatore viene reimpostato dopo il riciclo del dominio applicazione.|  
|**Totale riscontri nella cache in memoria**|Numero totale di report memorizzati nella cache restituiti dalla cache in memoria dopo l'avvio del servizio Windows ReportServer. Il contatore viene reimpostato dopo il riciclo del dominio applicazione.|  
|**Totale mancati riscontri nella cache in memoria**|Numero totale di accessi alla cache in memoria non riusciti dall'avvio del servizio. Il contatore viene reimpostato dopo il riciclo del dominio applicazione.|  
|**Totale elaborazioni non riuscite**|Numero di errori di elaborazione delle richieste nel servizio Windows ReportServer.|  
|**Totale thread rifiutati**|Numero totale di thread rifiutati per l'elaborazione asincrona e gestiti in un secondo momento come processo sincrono nello stesso thread. In presenza di un carico moderato o elevato, questo contatore viene incrementato costantemente.|  
|**Totale report eseguiti**|Numero totale di report eseguiti.|  
|**Richieste totali**|Numero totale di report eseguiti dall'avvio del servizio. Il contatore viene reimpostato dopo il riciclo del dominio applicazione.|  
|**Totale aggiornamenti snapshot**|Numero totale di aggiornamenti per gli snapshot dell'esecuzione dei report.|  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Utilizzare i cmdlet di PowerShell per restituire gli elenchi  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell")Tramite il seguente script di Windows PowerShell vengono restituiti i set di contatori in cui CounterSetName inizia con "msr":  
  
```  
get-counter -listset msr*  
```  
  
 Tramite il seguente script di Windows PowerShell viene restituito l'elenco di contatori delle prestazioni per CounterSetName.  
  
```  
(get-counter -listset "MSRS 2016 Windows Service").paths  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio delle prestazioni del server di report](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Contatori delle prestazioni per gli oggetti prestazioni MSRS 2016 Web Service SharePoint Mode e MSRS 2016 Windows Service SharePoint Mode &#40;modalità SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)   
 [Contatori delle prestazioni per gli oggetti prestazioni ReportServer:Service e ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
  
