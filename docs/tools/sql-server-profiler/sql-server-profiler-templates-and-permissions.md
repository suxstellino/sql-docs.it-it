---
title: Modelli e autorizzazioni di SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Informazioni sul funzionamento di SQL Server Profiler, su come usarlo per tracciare gli eventi e su dove trovare altre informazioni sulle relative funzionalità.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6d00378a-5d74-463b-9ed6-a2685306a9d2
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 679b8c0b120b3cea55a2cf8621e5619bc6c282bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341999"
---
# <a name="sql-server-profiler-templates-and-permissions"></a>Modelli e autorizzazioni di SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] mostra in che modo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risolve internamente le query. In questo modo gli amministratori possono verificare quali istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o espressioni multidimensionali vengono inviate al server e in quale modo il server accede al database o al cubo per restituire i set di risultati.  
  
 Tramite [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], è possibile eseguire una delle operazioni seguenti:  
  
-   Creazione di una traccia basata su un modello riutilizzabile  
  
-   Monitoraggio dei risultati della traccia durante l'esecuzione della traccia  
  
-   Archiviazione dei risultati della traccia in una tabella  
  
-   Avvio, arresto e modifica dei risultati della traccia in base alle specifiche esigenze  
  
-   Riproduzione dei risultati della traccia  
  
 Utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per eseguire unicamente il monitoraggio degli eventi di interesse. Se le dimensioni delle tracce diventano troppo grandi, è possibile filtrarle in base ai criteri desiderati, in modo da raccogliere solo un subset dei dati degli eventi. Il monitoraggio di un numero troppo elevato di eventi comporta un overhead aggiuntivo per il server e il processo di monitoraggio e pertanto potrebbe aumentare in modo considerevole le dimensioni del file o della tabella di traccia, in particolare se il processo di monitoraggio viene prolungato nel tempo.  
  
> [!NOTE]  
>  I valori della colonna di traccia maggiori di 1 GB restituiscono un errore e vengono troncati nell'output di traccia.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Modelli di SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates.md)|Presenta informazioni sui modelli di traccia predefiniti disponibili in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Autorizzazioni necessarie per l'esecuzione di SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|Presenta informazioni sulle autorizzazioni necessarie per l'esecuzione di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Salvare tracce e modelli di traccia](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|Presenta informazioni sul salvataggio di un output di traccia e sul salvataggio delle definizioni di traccia in un modello.|  
|[Modificare modelli di traccia](../../tools/sql-server-profiler/modify-trace-templates.md)|Presenta informazioni sulla modifica dei modelli di traccia tramite [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|[Avviare una traccia](../../tools/sql-server-profiler/start-a-trace.md)|Presenta informazioni sugli effetti dell'avvio, della sospensione o dell'arresto di una traccia.|  
|[Correlare una traccia con i dati del log delle prestazioni di Windows](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|Presenta informazioni sulla correlazione dei dati del registro prestazioni di Windows con una traccia tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler.|  
|[Visualizzare e analizzare le tracce con SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|Presenta informazioni sull'utilizzo delle tracce per la risoluzione di problemi relativi ai dati, sulla visualizzazione dei nomi degli oggetti in una traccia e sulla ricerca di eventi in una traccia.|  
|[Analizzare deadlock con SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|Presenta informazioni sull'utilizzo di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per identificare la causa di un deadlock.|  
|[Analizzare query con risultati SHOWPLAN in SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|Presenta informazioni sull'utilizzo di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per raccogliere e visualizzare i risultati di Showplan e Showplan Statistics.|  
|[Filtrare le tracce tramite SQL Server Profiler](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|Presenta informazioni sull'impostazione di filtri per le colonne di dati allo scopo di filtrare l'output di traccia tramite [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Riprodurre le tracce](../../tools/sql-server-profiler/replay-traces.md)|Presenta informazioni relative alla riproduzione di una traccia e ai relativi requisiti.|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Avviare SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)  
  
  
