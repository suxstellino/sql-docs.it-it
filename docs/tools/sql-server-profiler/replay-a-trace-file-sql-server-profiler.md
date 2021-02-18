---
title: Riprodurre un file di traccia
titleSuffix: SQL Server Profiler
description: Risolvere i problemi riproducendo le file di traccia in SQL Server Profiler. Informazioni sulle funzionalità e sulle opzioni di riproduzione.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 9e361275-c8fd-4499-8389-242cf8e27415
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b7d6139941e7ee3b80e62596e95f0d5d492cbc1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349380"
---
# <a name="replay-a-trace-file-sql-server-profiler"></a>Riprodurre un file di traccia (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La funzionalità di riproduzione è la capacità di aprire una traccia salvata e riprodurla nuovamente. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] include un motore di riproduzione a thread multipli in grado di simulare le connessioni utente e l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La funzionalità di riproduzione risulta utile per la risoluzione dei problemi a livello di applicazione o di processo. Quando si identifica il problema e si implementano le correzioni adeguate, eseguire nell'applicazione o nel processo la traccia con cui è stato rilevato il possibile problema. Riprodurre quindi la traccia originale e confrontare i risultati.  
  
 Per consentire la riproduzione è necessario acquisire classi di evento specifiche oltre alle classi di evento che si desidera monitorare. Questi eventi vengono acquisiti per impostazione predefinita se si usa il modello di traccia **TSQL_Replay** . Per altre informazioni, vedere [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md).  
  
### <a name="to-replay-a-trace-file"></a>Per riprodurre un file di traccia  
  
1.  Scegliere **Apri** dal menu **File** e quindi **File di traccia**. Selezionare un file di traccia che contiene le classi di evento necessarie per la riproduzione.  
  
2.  Scegliere **Avvia** dal menu **Riproduci** e connettersi all'istanza del server in cui si vuole riprodurre la traccia.  
  
3.  Specificare il **Server di riproduzione** nella scheda **Opzioni di base di riproduzione** della finestra di dialogo **Configurazione riproduzione**. Fare clic su **Cambia** per modificare il server visualizzato nella casella **Server di riproduzione** .  
  
4.  È facoltativamente possibile selezionare una delle destinazioni seguenti in cui salvare la riproduzione:  
  
    -   **Salva nel file** che consente di specificare un file in cui salvare la riproduzione.  
  
    -   **Salva nella tabella** che consente di specificare una tabella di database in cui salvare la riproduzione.  
  
5.  Selezionare **Riproduci gli eventi nell'ordine in cui sono stati inseriti nella traccia** oppure **Riproduci gli eventi usando più thread**. Nella tabella seguente viene spiegata la differenza tra queste impostazioni.  
  
    |Opzione|Descrizione|  
    |------------|-----------------|  
    |**Riproduci gli eventi nell'ordine in cui sono stati inseriti nella traccia**|Gli eventi vengono riprodotti nell'ordine in cui sono stati inseriti nella traccia. Questa opzione consente il debug.|  
    |**Riproduci gli eventi usando più thread**|Vengono utilizzati più thread per riprodurre i vari eventi, indipendentemente dalla sequenza. Questa opzione consente di ottimizzare le prestazioni.|  
  
6.  Selezionare **Visualizza risultati di riproduzione** per visualizzare la riproduzione quando si verifica.  
  
7.  Se si vuole, è possibile fare clic sulla scheda **Opzioni avanzate di riproduzione** per configurare le opzioni seguenti:  
  
    -   Per riprodurre tutti gli ID dei processi server (SPID), selezionare **Riproduci SPID di sistema**.  
  
    -   Per limitare la riproduzione ai processi appartenenti a uno specifico SPID, selezionare **Riproduci un solo SPID**. Digitare lo SPID nella casella **SPID da riprodurre** .  
  
    -   Per riprodurre gli eventi che si sono verificati in un determinato periodo di tempo, selezionare **Limite di tempo per la riproduzione**. Impostare i valori di data e ora in **Ora inizio** e **Ora fine** per specificare il periodo di tempo da includere nella riproduzione.  
  
    -   Per controllare la modalità di gestione dei processi da parte di SQL Server durante la riproduzione, configurare le **Opzioni Health Monitor**.  
  
## <a name="see-also"></a>Vedere anche  
 [Autorizzazioni necessarie per l'esecuzione di SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)   
 [Riprodurre le tracce](../../tools/sql-server-profiler/replay-traces.md)   
 [Aprire un file di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
