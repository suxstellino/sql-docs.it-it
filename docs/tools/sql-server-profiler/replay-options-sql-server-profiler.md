---
title: Opzioni di riproduzione
titleSuffix: SQL Server Profiler
description: Esplorare le impostazioni usate da SQL Server Profiler per la riproduzione di una traccia acquisita. Di seguito viene descritto come usare la finestra di dialogo Configurazione riproduzione per modificare le impostazioni.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 58761a25-a84f-4a90-9c61-97700bc5ad9c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: af342941ca797d9054492e59a5004096f901bf69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353369"
---
# <a name="replay-options-sql-server-profiler"></a>Opzioni di riproduzione (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Prima di riprodurre una traccia acquisita con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], è necessario specificare le opzioni di riproduzione nella finestra di dialogo **Configurazione riproduzione** . Per visualizzare questa finestra di dialogo, aprire il file o la tabella di traccia per la riproduzione in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]e scegliere **Avvia** dal menu **Riproduci**. Per informazioni sulle autorizzazioni necessarie per riprodurre una traccia, vedere [Autorizzazioni necessarie per l'esecuzione di SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
 In questo argomento vengono descritte le opzioni specificate con la finestra di dialogo **Configurazione riproduzione** .  
  
> [!NOTE]  
>  È consigliabile utilizzare Distributed Replay Utility per riprodurre un'applicazione OLTP intensiva (con molte connessioni simultanee attive o una velocità effettiva elevata). Distributed Replay Utility può riprodurre dati di traccia da più computer, per simulare in modo migliore un carico di lavoro di importanza critica. Per altre informazioni, vedere [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md).  
  
## <a name="basic-replay-options"></a>Opzioni di riproduzione di base  
 **Server di riproduzione**  
 Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si desidera riprodurre la traccia. Il server deve soddisfare i requisiti per la riproduzione descritti in [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md)."  
  
 **Salva nel file**  
 File di output nel quale vengono memorizzati i risultati della riproduzione della traccia per analisi successive. Per impostazione predefinita, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza sullo schermo solo i risultati della riproduzione della traccia.  
  
 **Salva nella tabella**  
 Tabella del database nella quale vengono memorizzati i risultati della riproduzione della traccia per analisi successive.  
  
 **Numero di thread di riproduzione**  
 Consente di specificare il numero di thread di riproduzione da utilizzare simultaneamente. Un numero più alto richiede un numero di risorse maggiore durante la riproduzione, ma il processo è più rapido. Se si utilizzano più thread, l'ordine degli eventi non viene mantenuto completamente.  
  
 **Riproduci gli eventi nell'ordine in cui sono stati inseriti nella traccia**  
 Consente di utilizzare metodi di debug quali l'esecuzione istruzione per istruzione di una traccia. Se l'opzione non è selezionata, non è garantito che l'ordine nel quale vengono riprodotti gli eventi sia consistente con l'ordine in cui sono stati acquisiti in origine.  
  
 **Riproduci gli eventi usando più thread**  
 Ottimizza le prestazioni e disabilita il debug. Gli eventi vengono riprodotti nell'ordine in cui sono stati registrati per un ID di processo server (SPID), ma non viene garantito l'ordinamento degli SPID.  
  
 **Visualizza risultati di riproduzione**  
 Visualizza i risultati della riproduzione. Questa è l'opzione predefinita. Se la traccia che si desidera riprodurre è molto estesa, è possibile disabilitare questa opzione per risparmiare spazio su disco.  
  
> [!NOTE]  
>  Per ottenere le migliori prestazioni di riproduzione, è consigliabile selezionare l'opzione per la riproduzione con più thread e deselezionare l'opzione per la visualizzazione dei risultati.  
  
## <a name="advanced-replay-options"></a>Opzioni avanzate di riproduzione  
 **Riproduci SPID di sistema**  
 Riproduce tutti gli SPID. Questa è l'opzione predefinita.  
  
 **Riproduci un solo SPID**  
 Riproduce il numero di SPID selezionato nell'elenco.  
  
 **Limite di tempo per la riproduzione**  
 Riproduce la traccia in base ai valori **Ora di inizio** e **Ora di fine** specificati.  
  
 **Intervallo di attesa Health Monitor**  
 Imposta il periodo di tempo per il quale è consentita l'esecuzione di un processo prima che venga terminato da Health Monitor.  
  
 **Intervallo di polling Health Monitor**  
 Imposta la frequenza con la quale Health Monitor esegue il polling sui candidati per la chiusura.  
  
 **Abilita monitoraggio processi bloccati di SQL Server**  
 Imposta la frequenza con la quale il monitoraggio dei processi bloccati esegue la ricerca dei processi bloccati o in fase di blocco.  
  
## <a name="about-the-health-monitor"></a>Informazioni su Health Monitor  
 Health Monitor è un thread dell'applicazione che esegue il monitoraggio dei processi simulati coinvolti nella riproduzione di una traccia e che termina i processi bloccati nella fase di riproduzione. Nella scheda **Opzioni avanzate di riproduzione** della finestra di dialogo **Configurazione riproduzione** è possibile specificare l'intervallo espresso in secondi per il quale Health Monitor dovrà attendere prima di terminare un processo bloccato (**Intervallo di attesa Health Monitor**). Se si imposta l'intervallo su 0, Health Monitor non terminerà mai i processi bloccati nella traccia di riproduzione.  
  
## <a name="see-also"></a>Vedere anche  
 [Riprodurre le tracce](../../tools/sql-server-profiler/replay-traces.md)   
 [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md)   
 [Considerazioni relative alla riproduzione di tracce &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/considerations-for-replaying-traces-sql-server-profiler.md)  
  
  
