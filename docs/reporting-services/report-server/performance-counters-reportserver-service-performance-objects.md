---
title: Contatori delle prestazioni per oggetti prestazioni del servizio ReportServer | Microsoft Docs
description: Informazioni sui contatori delle prestazioni per gli oggetti prestazioni di ReportServer:Service e ReportServerSharePoint:Service, che fanno parte di una distribuzione di SQL Server 2012.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Report Server service, performance counters
ms.assetid: 2bcacab2-3a4f-4aae-b123-19d756b9b9ed
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 8224c126d9f23cb5f13c517400ec79e04d127ff5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466612"
---
# <a name="performance-counters---reportserver-service--performance-objects"></a>Contatori delle prestazioni per oggetti prestazioni del servizio ReportServer
  Questo argomento descrive i contatori delle prestazioni per gli oggetti prestazioni di **ReportServer:Service** e **ReportServerSharePoint:Service** che fanno parte di una distribuzione di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .  
  
> [!NOTE]  
>  Gli oggetti prestazioni vengono utilizzati per monitorare gli eventi nel server di report locale. Se si esegue un server di report in una distribuzione con scalabilità orizzontale, i conteggi si applicano al server corrente e non all'intera distribuzione con scalabilità orizzontale.  
  
 Gli oggetti prestazioni sono disponibili in Monitoraggio prestazioni di Windows (**Perfmon.exe**). Per ulteriori informazioni, vedere la documentazione di Windows. [Profilatura di runtime](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 In questo argomento  
  
-   [Contatori delle prestazioni di ReportServer:Service (server di report in modalità nativa)](#bkmk_ReportServer)  
  
-   [ReportServerSharePoint:Service (server di report in modalità SharePoint)](#bkmk_ReportServerSharePoint)  
  
-   [Utilizzare i cmdlet di PowerShell per restituire gli elenchi](#bkmk_powershell)  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
##  <a name="reportserverservice-performance-counters-native-mode-report-server"></a><a name="bkmk_ReportServer"></a> Contatori delle prestazioni di ReportServer:Service (server di report in modalità nativa)  
 L'oggetto prestazioni **ReportServer:Service** include una raccolta di contatori per tenere traccia degli eventi correlati a HTTP e degli eventi correlati alla memoria per un'istanza del server di report. Questo oggetto prestazione viene visualizzato una volta per ogni istanza di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nel computer ed è possibile aggiungere o rimuovere contatori nell'oggetto prestazione per ogni istanza. I contatori per l'istanza predefinita sono visualizzati nel formato **ReportServer:Service**. I contatori per le istanze denominate sono visualizzati nel formato **ReportServer$\<**_instance_name_*_>:Service_* .  
  
 L'oggetto prestazioni **ReportServer:Service** rappresenta una novità di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e fornisce un subset di contatori inclusi in Internet Information Services (IIS) e [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] nelle versioni precedenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Questi nuovi contatori sono specifici di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]e consentono di tenere traccia di eventi correlati a HTTP per il server di report, quali richieste, connessioni e tentativi di accesso. Questo oggetto prestazione, inoltre, include contatori per tenere traccia di eventi di gestione della memoria.  
  
 La tabella seguente elenca i contatori inclusi nell'oggetto prestazioni **ReportServer:Service** .  
  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell") Il seguente script di Windows PowerShell restituisce l'elenco di contatori delle prestazioni per CounterSetName  
  
```  
(get-counter -listset "ReportServer:Service").paths  
```  
  
|Contatore|Descrizione|  
|-------------|-----------------|  
|**Connessioni attive**|Numero di connessioni attualmente attive nel server.|  
|**Totale byte ricevuti**|Numero di byte ricevuti dal server. Questo contatore conta i byte non elaborati ricevuti complessivamente sia da Gestione report sia dal server di report.|  
|**Byte ricevuti/sec**|Numero di byte ricevuti al secondo dal server. Questo contatore è aggiornato solo al termine di un trasferimento, ovvero continua a indicare 0 e aumenta solo al termine di un trasferimento.|  
|**Totale byte inviati**|Numero di byte inviati dal server. Questo contatore conta i byte non elaborati inviati complessivamente sia da Gestione report sia dal server di report.|  
|**Byte inviati/sec**|Numero di byte inviati al secondo dal server. Questo contatore è aggiornato solo al termine di un trasferimento, ovvero continua a indicare 0 e aumenta solo al termine di un trasferimento.|  
|**Totale errori**|Numero totale di errori verificatisi durante l'elaborazione di richieste HTTP. Tali errori includono i codici di stato HTTP delle serie 400 e 500.|  
|**Errori/sec**|Numero totale di errori al secondo verificatisi durante l'elaborazione di richieste HTTP. Tali errori includono i codici di stato HTTP delle serie 400 e 500.|  
|**Totale tentativi di accesso**|Numero di tentativi di accesso eseguiti dai tipi di autenticazione RSWindows. I tipi di autenticazione RSWindows includono RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos e RSWindowsBasic. Il valore zero (0) rappresenta l'autenticazione personalizzata.|  
|**Tentativi di accesso/sec**|Frequenza di tentativi di accesso.|  
|**Totale accessi riusciti**|Numero di tentativi di accesso riusciti per i tipi di autenticazione RSWindows. I tipi di autenticazione RSWindows includono RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos e RSWindowsBasic. Il valore zero (0) rappresenta l'autenticazione personalizzata.|  
|**Accessi riusciti/sec**|Frequenza di accessi riusciti.|  
|**Stato utilizzo memoria**|Uno dei numeri seguenti, da 1 a 5, indicante lo stato corrente della memoria del server:<br /><br /> 1: nessun utilizzo<br /><br /> 2: scarso utilizzo<br /><br /> 3: utilizzo medio<br /><br /> 4: utilizzo elevato<br /><br /> 5: utilizzo eccessivo|  
|**Livello compattazione memoria**|Numero di byte richiesti dal server per compattare la memoria in uso.|  
|**Notifiche compattazione memoria/sec**|Numero di notifiche generate dal server nell'ultimo secondo per la compattazione della memoria in uso. Questo valore indica la frequenza con cui il server rileva un utilizzo eccessivo della memoria.|  
|**Richieste disconnesse**|Numero di richieste disconnesse a causa di un errore di comunicazione.|  
|**Richieste in esecuzione**|Numero di richieste attualmente in elaborazione.|  
|**Richieste non autorizzate**|Numero di richieste non riuscite con codice di stato HTTP 401.|  
|**Richieste respinte**|Numero totale di richieste non elaborate a causa di risorse server insufficienti. Questo contatore rappresenta il numero delle richieste che restituiscono un codice di stato HTTP 503, che indica che il server è troppo occupato.|  
|**Totale richieste**|Numero totale di richieste ricevute dal servizio del server di report dall'avvio. Questo contatore conta le richieste inviate a Gestione report e quelle inviate da Gestione report al server di report.|  
|**Richieste/sec**|Numero di richieste elaborate al secondo. Questo valore rappresenta la velocità di trasferimento corrente dell'applicazione.|  
|**Attività in coda**|Numero di attività in attesa che un thread diventi disponibile per l'elaborazione. Ogni richiesta inviata al server di report corrisponde a una o più attività. Questo contatore rappresenta solo il numero di attività pronte per l'elaborazione e non include il numero di attività attualmente in esecuzione.|  
  
##  <a name="reportserversharepointservice-sharepoint-mode-report-server"></a><a name="bkmk_ReportServerSharePoint"></a> ReportServerSharePoint:Service (server di report in modalità SharePoint)  
 L'oggetto prestazioni **ReportServerSharePoint:Service** è stato aggiunto in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell") Il seguente script di Windows PowerShell restituisce l'elenco di contatori delle prestazioni per CounterSetName  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
|Contatore|Descrizione|  
|-------------|-----------------|  
|**Stato utilizzo memoria**||  
|**Livello compattazione memoria**||  
|**Memory Shrink Notifications/Sec**||  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Utilizzare i cmdlet di PowerShell per restituire gli elenchi  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell") Lo script di Windows PowerShell seguente restituirà l'elenco dei contatori delle prestazioni per CounterSetName "ReportServerSharePoint:Service":  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio delle prestazioni del server di report](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Contatori delle prestazioni per gli oggetti prestazioni MSRS 2011 Web Service e MSRS 2011 Windows Service &#40;modalità nativa&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Contatori delle prestazioni per gli oggetti prestazioni MSRS 2011 Web Service SharePoint Mode e MSRS 2011 Windows Service SharePoint Mode &#40;modalità SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
  
