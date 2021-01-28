---
title: Elaborare report di grandi dimensioni | Microsoft Docs
description: Informazioni su come eseguire correttamente report di grandi dimensioni in un server di report seguendo le raccomandazioni di configurazione, rendering, implementazione e distribuzione.
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report processing [Reporting Services], large reports
- page breaks [Reporting Services]
- large reports
- size [SQL Server], reports
- distributing reports [Reporting Services], large reports
ms.assetid: c5275a9f-c95b-46d7-bc62-633879a8a291
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 022751a3ec4b47a8e7a3a5f5295306265f659512
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594979"
---
# <a name="process-large-reports"></a>Elaborare report di grandi dimensioni
  La corretta esecuzione di report di grandi dimensioni presenta alcuni aspetti complessi e richiede determinate configurazioni. Non è consigliabile eseguire report di grandi dimensioni su richiesta, a meno che siano configurati per supportare l'impaginazione.  
  
> [!NOTE]  
>  Le interruzioni di pagina sono abilitate per impostazione predefinita. Non disabilitare questa funzionalità se si ritiene che il report possa contenere una grande quantità di dati. Il formato di rendering HTML utilizzato per il rendering iniziale dei report prevede l'apertura del report in un browser. Se il report non viene impaginato, tutti i dati saranno inclusi in una sola pagina. Questo formato non è supportato dalla maggior parte dei browser. È molto probabile, ad esempio, che un report contenente 5.000 righe di dati non possa essere visualizzato in un browser in una sola pagina.  
  
 Per l'utilizzo di report di grandi dimensioni è opportuno scegliere opzioni per l'esecuzione, il rendering e il recapito adatte alle dimensioni dei documenti. Le dimensioni dei report dipendono in gran parte dal set di righe restituito dalla query e dall'estensione per il rendering utilizzata per la presentazione del report.  
  
 Le dimensioni dei report contenenti dati volatili possono variare in modo significativo da un'esecuzione del report all'altra. In questi casi è opportuno monitorare l'origine dei dati per stabilire gli effetti della volatilità dei dati sul report e l'eventuale necessità di eseguire la procedura indicata in questo argomento.  
  
 Per altre informazioni e suggerimenti sulla diagnosi di errori di timeout e di memoria insufficiente, vedere l'articolo [Come eseguire la diagnosi durante l'esecuzione di report nel server di report](/archive/blogs/lukaszp/) nel sito Web blogs.msdn.microsoft.com.  
  
## <a name="configuration-recommendations"></a>Raccomandazioni per la configurazione  
 Per l'esecuzione, il rendering e la visualizzazione dei report sono valide le indicazioni seguenti:  
  
-   Progettare il report in modo che supporti l'impaginazione. Il server di report invia infatti il report una pagina alla volta e se il report è suddiviso in più pagine sarà possibile regolare il flusso dei dati inviati al browser. Per altre informazioni, vedere [Precaricare la cache (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md).  
  
-   Configurare il report in modo che venga eseguito come snapshot del report pianificato per evitare che venga eseguito su richiesta. Non impostare un valore di timeout per l'esecuzione del report. Eseguire il report nelle fasce orarie di minore attività.  
  
-   Configurare il report per l'utilizzo di un'origine dei dati condivisa se si desidera controllare l'elaborazione del report. Uno dei vantaggi dell'utilizzo di un'origine dei dati condivisa è la possibilità di disabilitarla, in modo da evitare l'elaborazione del report.  
  
-   Disabilitare la cronologia del report se si desidera risparmiare spazio su disco. Per disabilitare la cronologia del report, deselezionare tutte le caselle di controllo nella pagina delle proprietà Cronologia.  
  
-   Limitare l'accesso al report. Configurare il report in modo che venga utilizzata la sicurezza a livello di elemento e sostituire le assegnazioni di ruolo predefinite con nuove assegnazioni che consentano l'accesso solo agli utenti specifici.  
  
     Per impostazione predefinita, gli utenti possono aprire tutti i report visualizzati nella gerarchia di cartelle. Anche se un report viene configurato per l'esecuzione come snapshot, gli utenti che possono visualizzare il report in una cartella possono anche aprirlo. Se il report è molto grande, quando viene aperto nel portale Web è possibile che il browser si blocchi.  
  
## <a name="rendering-recommendations"></a>Raccomandazioni per il rendering  
 Prima di configurare le modalità di distribuzione del report è fondamentale stabilire quali client di rendering supportano documenti di grandi dimensioni. Il formato consigliato è l'estensione per il rendering HTML predefinita con interruzioni di pagina software, tuttavia è possibile scegliere qualsiasi formato che supporti l'impaginazione.  
  
 Le prestazioni e l'utilizzo di memoria variano a seconda del formato di rendering. Il rendering dello stesso report verrà eseguito a velocità diverse e richiederà quantità di memoria diverse a seconda del formato selezionato. I formati più veloci e che utilizzano la minore quantità di memoria includono CSV, XML e HTML. I formati PDF ed Excel offrono le prestazioni più lente, anche se per motivi diversi. Il formato PDF fa un utilizzo elevato della CPU, mentre il formato Excel fa un utilizzo elevato della RAM. Il rendering delle immagini rientra si colloca fra questi due gruppi. Il formato può essere specificato in fase di definizione delle modalità di distribuzione del report.  
  
## <a name="deployment-and-distribution-recommendations"></a>Raccomandazioni per la distribuzione  
 Se si utilizzano interruzioni di pagina per controllare il rendering del report, è possibile distribuire un report di grandi dimensioni con le stesse modalità di distribuzione di qualsiasi altro report. Per consentire l'accesso al report, usare il portale Web, una web part di SharePoint oppure un URL aggiunto a un portale o a un sito Web. Tutte queste opzioni di distribuzione supportano l'accesso su richiesta, oltre allo snapshot di un report eseguito in precedenza.  
  
 Una strategia di distribuzione alternativa consiste nel distribuire report a singoli utenti. Se si configurano le opzioni di recapito in modo accurato, sarà possibile distribuire report di grandi dimensioni tramite sottoscrizioni. Per il recapito dei report è possibile utilizzare una sottoscrizione standard o una sottoscrizione guidata dai dati. Di seguito sono elencate le indicazioni per la sottoscrizione e il recapito:  
  
-   Configurare una sottoscrizione in modo che venga utilizzato il formato Archivio Web (MHTML), PDF o Excel.  
  
-   Configurare una sottoscrizione in modo che venga utilizzato il recapito tramite condivisione file se si utilizza il formato PDF o Excel. Dopo aver recapitato il report, per l'accesso al report sarà possibile utilizzare un'applicazione desktop. È necessario impostare autorizzazioni specifiche per la condivisione file in modo da stabilire a quali utenti è consentita la visualizzazione del report.  
  
     Si noti che i report memorizzati in una condivisione file non vengono più controllati o protetti da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Se si desidera ricevere un avviso quando il report viene aggiornato, creare una seconda sottoscrizione impostata per il recapito tramite posta elettronica solo della notifica.  
  
 Se si desidera utilizzare il recapito tramite posta elettronica, includere un collegamento durante la configurazione della sottoscrizione. Evitare di inviare il report come allegato.  
  
## <a name="see-also"></a>Vedere anche  
 [Sottoscrizioni e recapito &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Impostare proprietà di elaborazione dei report](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [Gestione contenuto del server di report &#40;modalità nativa SSRS&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Precaricare la cache (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md)