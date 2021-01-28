---
title: Limiti delle dimensioni di report e snapshot | Microsoft Docs
description: Informazioni sui limiti relativi alle dimensioni dei report quando vengono pubblicati in un server di report, ne viene eseguito il rendering in fase di esecuzione e viene salvato nel file system.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- large reports
- maximum report size
- size [SQL Server], reports
- report size [Reporting Services]
- reports [Reporting Services], size
- denial of service attacks [Reporting Services]
ms.assetid: 1e3be259-d453-4802-b2f5-6b81ef607edf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5b5e8dbaee71c53f8f9491f8a1abcd15559e4800
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596838"
---
# <a name="report-and-snapshot-size-limits"></a>Limiti delle dimensioni di report e snapshot
  Le informazioni contenute in questo argomento consentono agli amministratori che gestiscono una distribuzione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] di conoscere i limiti relativi alle dimensioni dei report quando questi ultimi vengono pubblicati in un server di report, quando ne viene eseguito il rendering in fase di esecuzione e quando vengono salvati nel file system. In questo argomento vengono inoltre fornite indicazioni pratiche su come calcolare le dimensioni di un database del server di report e vengono descritti gli effetti delle dimensioni degli snapshot sulle prestazioni del server.  
  
## <a name="maximum-size-for-published-reports"></a>Dimensioni massime per i report pubblicati  
 Nel server di report le dimensioni dei report e dei modelli sono basate sulle dimensioni dei file di definizione del report, con estensione rdl, e del modello di report, con estensione smdl, pubblicati in un server di report. Il server di report non prevede limiti per le dimensioni di un report da pubblicare. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] prevede comunque una dimensione massima per gli elementi inviati al server. Per impostazione predefinita, questo limite è di 4 MB. Se si carica o si pubblica in un server di report un file le cui dimensioni superano questo limite, viene generata un'eccezione HTTP. In questo caso, è possibile modificare l'impostazione predefinita aumentando il valore dell'elemento **maxRequestLength** nel file Machine.config.  
  
 Sebbene un modello di report possa avere dimensioni molto grandi, le definizioni dei report non superano quasi mai i 4 MB. In genere, un report ha dimensioni di alcuni kilobyte (KB). Se tuttavia sono presenti immagini incorporate, la codifica di tali immagini può determinare un aumento considerevole delle dimensioni della definizione del report e, di conseguenza, il superamento del limite predefinito di 4 MB.  
  
 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] prevede un limite massimo per i file inviati al fine di ridurre i rischi di attacchi Denial of Service contro il server. Aumentando il valore, vengono ridotte le capacità di sicurezza garantite da questa limitazione. È pertanto consigliabile aumentarlo solo se i vantaggi che questa modifica comporta bilanciano i maggiori rischi a cui viene esposto il sistema di sicurezza.  
  
 Si tenga presente che il valore impostato per l'elemento **maxRequestLength** deve essere maggiore dei limiti effettivi delle dimensioni che si desidera applicare. È necessario un valore più grande per tener conto dell'inevitabile aumento delle dimensioni della richiesta HTTP che si verifica dopo che tutti i parametri vengono incapsulati in una busta SOAP e ad alcuni di essi viene applicata la codifica Base64. Con la codifica Base64 le dimensioni dei dati originali vengono aumentate di circa il 33%. Di conseguenza, il valore specificato per l'elemento **maxRequestLength** deve essere di circa il 33% maggiore delle dimensioni effettive dell'elemento utilizzabile. Ad esempio, se si specifica un valore pari a 64 MB per **maxRequestLength**, è possibile prevedere realisticamente che le dimensioni massime dei file di report inviati al server di report saranno di circa 48 MB.  
  
## <a name="report-size-in-memory"></a>Dimensioni dei report in memoria  
 Quando si esegue un report, le dimensioni di quest'ultimo corrispondono alla quantità di dati restituiti nel report più le dimensioni del flusso di output. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non è previsto un limite massimo per le dimensioni di un report visualizzabile. La memoria del sistema determina il limite superiore per le dimensioni (per impostazione predefinita, in caso di esecuzione il rendering di un report un server di report utilizza tutta la memoria configurata disponibile), ma è possibile specificare impostazioni di configurazione per impostare soglie di memoria e criteri della gestione della memoria. Per altre informazioni, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
 Le dimensioni di qualsiasi report possono variare significativamente in base alla quantità di dati restituiti e al formato di rendering utilizzato. Le dimensioni di un report con parametri possono variare in base a come i valori dei parametri influiscono sui risultati della query. Il formato di output scelto per il report influisce sulle dimensioni del report in base a quanto indicato di seguito:  
  
-   Con il formato HTML il report viene elaborato una pagina alla volta. Poiché il report viene elaborato in unità di dimensioni minori, per l'elaborazione di blocchi specifici è richiesta una quantità di memoria inferiore.  
  
-   Con i formati PDF, Excel, TIFF, XML e CSV l'intero report viene elaborato in memoria prima di essere visualizzato all'utente.  
  
 Per calcolare le dimensioni di un report visualizzabile, è possibile analizzare il log di esecuzione del report. Per altre informazioni, vedere [Vista ExecutionLog ed ExecutionLog3 del server di report](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
 Per calcolare le dimensioni su disco di un report visualizzabile, è possibile esportare e quindi salvare il report nel file system. Il file salvato includerà le informazioni di formattazione dei dati e del report.  
  
 L'unico caso in cui vi è un limite fisico per le dimensioni del report è quando si esegue il rendering in formato Excel. Nei fogli di lavoro non possono essere presenti più di 65536 righe o 256 colonne. Con altri formati di rendering non sono presenti limiti simili, pertanto le dimensioni sono limitate solo dalla quantità di risorse disponibili nel server.  
  
> [!NOTE]  
>  I processi di elaborazione e rendering dei report avvengono in memoria. Se si utilizzano report di grandi dimensioni o sono presenti numerosi utenti, eseguire una pianificazione delle capacità per garantire agli utenti un livello soddisfacente di prestazioni della distribuzione del server di report. Per altre informazioni su strumenti e linee guida, vedere le pubblicazioni seguenti su MSDN: [Pianificazione di scalabilità e prestazioni con Reporting Services](/previous-versions/sql/sql-server-2005/administrator/cc966418(v=technet.10)) e [Uso di Visual Studio 2005 per eseguire test di carico in un server di report di SQL Server 2005 Reporting Services](/previous-versions/sql/sql-server-2005/administrator/aa964139(v=sql.90)).  
  
## <a name="measuring-snapshot-storage"></a>Calcolo dello spazio di archiviazione degli snapshot  
 Le dimensioni di un determinato snapshot sono direttamente proporzionali alla quantità di dati del report. Gli snapshot hanno in genere dimensioni molto maggiori rispetto agli altri elementi archiviati in un server di report. Le dimensioni degli snapshot variano in genere da pochi MB a decine di MB. Se si dispone di report di dimensioni molto grandi, le dimensioni degli snapshot potrebbero essere anche maggiori. In base alla frequenza di utilizzo degli snapshot e alla modalità di configurazione della cronologia del report, la quantità di spazio su disco necessaria per il database del server di report può aumentare rapidamente in un intervallo di tempo breve.  
  
 Per impostazione predefinita, sia per il database **reportserver** che per il database **reportservertempdb** è impostato l'aumento automatico delle dimensioni. Le dimensioni del database possono aumentare automaticamente ma non vengono mai ridotte automaticamente. Se nel database **reportserver** è presente capacità in eccesso in quanto sono stati eliminati snapshot, per recuperare spazio su disco è necessario ridurre il database manualmente. Analogamente, se le dimensioni del database **reportservertempdb** sono aumentate per adattarsi a un volume insolitamente elevato di report interattivi, l'allocazione dello spazio su disco rimane impostata in base a quei valori fino a quando non la si riduce.  
  
 Per calcolare le dimensioni dei database del server di report, è possibile eseguire i comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] riportati di seguito. Il calcolo delle dimensioni totali del database a intervalli regolari può agevolare lo sviluppo di una stima ragionevole delle modalità di allocazione nel tempo dello spazio per il database del server di report. Le istruzioni seguenti consentono di calcolare la quantità di spazio attualmente usato e presuppongono l'uso dei nomi di database predefiniti:  
  
```  
USE ReportServer  
EXEC sp_spaceused  
```  
  
## <a name="snapshot-size-and-report-server-performance"></a>Dimensioni degli snapshot e prestazioni del server di report  
 Le dimensioni degli snapshot influiscono sulle prestazioni del server in fase di elaborazione e rendering del report. Le prestazioni del server sono influenzate soprattutto dalle operazioni di rendering, pertanto se le dimensioni dello snapshot sono elevate, è probabile che si verifichino ritardi quando gli utenti richiedono il report. A seconda del numero di utenti, i ritardi si verificano con maggiore probabilità quando le dimensioni dello snapshot superano i 100 MB.  
  
 Per ridurre al minimo i ritardi nelle prestazioni dovuti a snapshot di grandi dimensioni, è possibile eseguire le operazioni seguenti:  
  
-   Distribuire il server di report e [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in computer separati.  
  
-   Aggiungere ulteriore memoria di sistema.  
  
-   Vedere il documento relativo alla pianificazione di scalabilità e prestazioni con Reporting Services nel sito Web MSDN per informazioni sulle procedure consigliate per la configurazione di un server di report per l'organizzazione.  
  
 La quantità di snapshot archiviati in un database del server di report non è un fattore che influisce sulle prestazioni. È possibile archiviare un numero elevato di snapshot senza influenzare le prestazioni del server, nonché mantenere gli snapshot per un periodo illimitato. Tenere tuttavia presente che la cronologia del report può essere modificata. Se un amministratore del server di report riduce il limite per la cronologia del report, si potrebbero perdere snapshot presenti nella cronologia che si desiderava mantenere. Se si elimina il report, viene eliminata anche tutta la relativa cronologia.  
  
## <a name="see-also"></a>Vedere anche  
 [Impostare proprietà di elaborazione dei report](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Database del server di report &#40;modalità nativa SSRS&#41;](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
 [Elaborare report di grandi dimensioni](../../reporting-services/report-server/process-large-reports.md)  
  
