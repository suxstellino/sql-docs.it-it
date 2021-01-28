---
title: Prestazioni, snapshot, memorizzazione nella cache (Reporting Services) | Microsoft Docs
description: Informazioni su come ottenere dati di base ed eseguire test per comprendere i fattori relativi alle prestazioni specifici dell'installazione in uso e produrre i risultati desiderati.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: 85afd00f-e8d7-4ef7-9174-2ff84d82f960
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f7585621cae251f639fbce4f0a219338ca01ffdc
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595095"
---
# <a name="performance-snapshots-caching-reporting-services"></a>Prestazioni, snapshot, memorizzazione nella cache (Reporting Services)
  Le prestazioni del server di report sono influenzate da una combinazione di fattori che includono hardware, numero di utenti simultanei che accedono ai report, quantità di dati in un report e formato di output. Per comprendere i fattori relativi alle prestazioni specifici dell'installazione e quali rimedi produrranno i risultati desiderati, sarà necessario ottenere dati di base ed eseguire test. Per ulteriori informazioni su strumenti e linee guida, vedere le pubblicazioni seguenti su MSDN relative all' [ottimizzazione delle prestazioni di Reporting Services](/archive/blogs/sqlcat/reporting-services-performance-and-optimization) e all' [utilizzo di Visual Studio 2005 per eseguire test di carico in un server di report di SQL Server 2005 Reporting Services](/previous-versions/sql/sql-server-2005/administrator/aa964139(v=sql.90)).  
  
 I principi generali da considerare includono gli aspetti seguenti:  
  
-   L'elaborazione e il rendering del report sono operazioni che utilizzano una quantità elevata di memoria. Quando possibile, scegliere un computer che dispone di molta memoria.  
  
-   Se il server di report e il relativo database vengono ospitati in computer separati, le prestazioni ottenute sono migliori rispetto alla situazioni in cui entrambi sono ospitati in un solo computer di fascia alta.  
  
-   Se tutti i report vengono elaborati lentamente, considerare una distribuzione con scalabilità orizzontale in cui più istanze del server di report supportano un solo database del server di report. Per ottenere risultati migliori, utilizzare software di bilanciamento carico per distribuire uniformemente le richieste nella distribuzione.  
  
-   Se l'elaborazione di un singolo report è lenta, ottimizzare le query del set di dati del report se il report viene eseguito su richiesta. Si consideri inoltre la possibilità di utilizzare set di dati condivisi che è possibile memorizzare nella cache, di memorizzare il report nella cache o di eseguire il report come snapshot.  
  
-   Se tutti i report vengono elaborati lentamente per ottenere un formato specifico (ad esempio durante il rendering in formato PDF), prendere in considerazione l'utilizzo del recapito tramite la condivisione file, l'aggiunta di ulteriore memoria o la scelta di un formato diverso.  
  
-   Per individuare la quantità di tempo necessario per elaborare un report e altre misure relative all'utilizzo, esaminare il log di esecuzione del server di report. Per altre informazioni, vedere [Vista ExecutionLog ed ExecutionLog3 del server di report](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
-   Per altre informazioni su come limitare i problemi di prestazioni ottimizzando le impostazioni di configurazione di Gestione memoria, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Monitoraggio delle prestazioni del server di report](../../reporting-services/report-server/monitoring-report-server-performance.md)  
 Descrive gli oggetti relativi alle prestazioni che è possibile utilizzare per tenere traccia del carico di elaborazione nel server.  
  
 [Impostare le proprietà di elaborazione dei report](../../reporting-services/report-server/set-report-processing-properties.md)  
 Descrive come configurare un report per l'esecuzione su richiesta, dalla cache o in base a una pianificazione come snapshot del report.  
  
 [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)  
 Viene descritto come è possibile ignorare il comportamento predefinito per la gestione della memoria.  
  
 [Memorizzazione dei report nella cache &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md)  
 Descrive il comportamento di memorizzazione del report nella cache in un server di report.  
  
 [Memorizzare nella cache set di dati condivisi &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md)  
 Descrive il comportamento di memorizzazione del set di dati condiviso nella cache in un server di report.  
  
 [Elaborare report di grandi dimensioni](../../reporting-services/report-server/process-large-reports.md)  
 Offre indicazioni su come configurare e distribuire un report di grandi dimensioni.  
  
 [Impostazione dei valori di timeout per l'elaborazione di report e di set di dati condivisi &#40;SSRS&#41;](../../reporting-services/report-server/setting-time-out-values-for-report-and-shared-dataset-processing-ssrs.md)  
 Illustra come impostare i timeout relativi all'esecuzione del report e di query.  
  
## <a name="see-also"></a>Vedere anche  
 [Manage a Running Process](../../reporting-services/subscriptions/manage-a-running-process.md)   
 [Verifica dell'esecuzione di un report](../../reporting-services/report-server/verifying-a-report-run.md)  
  
