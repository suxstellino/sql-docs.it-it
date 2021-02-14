---
title: Piani di esecuzione | Microsoft Docs
description: Informazioni sui piani di esecuzione o sui piani di query creati da Query Optimizer per consentire al motore di database di SQL Server di eseguire le query.
ms.custom: ''
ms.date: 03/01/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- execution plan [SQL Server]
- query plan [SQL Server]
- query execution plans
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753f
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 9007a29a613f1fea6891af33b66e69d98f2058b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355260"
---
# <a name="execution-plans"></a>Piani di esecuzione
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Per poter eseguire le query, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] deve analizzare l'istruzione per determinare il modo più efficiente di accedere ai dati necessari. Questa analisi viene gestita da un componente denominato Query Optimizer. I dati di input per Query Optimizer sono costituiti dalla query, dallo schema del database (definizioni di tabella e indice) e dalle statistiche del database. L'output di Query Optimizer è un piano di esecuzione query, talvolta definito piano di query o piano di esecuzione.   

Il piano di esecuzione di una query è costituito dalla definizione degli elementi seguenti: 

- **La sequenza di accesso alle tabelle di origine.**  
  In genere il server di database può utilizzare molte sequenze diverse per accedere alle tabelle di base e quindi compilare il set di risultati. Ad esempio, se un'istruzione `SELECT` fa riferimento a tre tabelle, il server di database può accedere innanzitutto a `TableA`, usare i dati di `TableA` per estrarre le righe corrispondenti da `TableB`, quindi usare i dati di `TableB` per estrarre i dati da `TableC`. Di seguito vengono indicate le altre sequenze di accesso alle tabelle utilizzabili dal server di database:  
  `TableC`, `TableB`, `TableA`o  
  `TableB`, `TableA`, `TableC`o  
  `TableB`, `TableC`, `TableA`o  
  `TableC`, `TableA`, `TableB`  

- **I metodi usati per estrarre i dati da ogni tabella.**  
  Per accedere ai dati di ogni tabella sono in genere disponibili metodi diversi. Se sono necessarie solo alcune righe con valori di chiave specifici, il server di database può utilizzare un indice. Se sono necessarie tutte le righe della tabella, il server di database può ignorare gli indici ed eseguire un'analisi di tabella. Se sono necessarie tutte le righe di una tabella, ma l'indice contiene colonne chiave incluse in una clausola `ORDER BY`, è consigliabile eseguire l'analisi dell'indice anziché della tabella per evitare che il set di risultati venga ordinato separatamente. Se una tabella è di dimensioni molto ridotte, l'analisi della tabella può rappresentare il metodo più efficiente per quasi tutti gli accessi alla tabella.
  
- **I metodi usati per eseguire i calcoli e i modi usati per filtrare, aggregare e ordinare i dati da ogni tabella.**  
  Se si accede ai dati dalle tabelle, è possibile usare metodi diversi per eseguire i calcoli sui dati, ad esempio il calcolo dei valori scalari, per aggregare e ordinare i dati come definito nel testo della query, ad esempio quando si usa una clausola `GROUP BY` o `ORDER BY`, e per filtrare i dati, ad esempio quando si usa una clausola `WHERE` o `HAVING`.

> [!NOTE]
> [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ha tre opzioni di visualizzazione dei piani di esecuzione:        
> -  ***[Piano di esecuzione stimato](../../relational-databases/performance/display-the-estimated-execution-plan.md)***: il piano compilato generato da Query Optimizer in base alle stime. Si tratta del piano di query memorizzato nella cache dei piani.        
> -  Il ***[piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md)** _ è il piano compilato più il relativo [contesto di esecuzione](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse). Diventa disponibile _*dopo il completamento dell'esecuzione della query**. Include le informazioni di runtime effettivo, ad esempio gli avvisi relativi all'esecuzione o, nelle versioni più recenti del [!INCLUDE[ssde_md](../../includes/ssde_md.md)], il tempo trascorso e il tempo CPU usato durante l'esecuzione.         
> -  ***[Live query Statistics](../../relational-databases/performance/live-query-statistics.md)** _ è il piano compilato più il relativo contesto di esecuzione. È disponibile per le esecuzioni di query _ * in-Flight * * e viene aggiornata ogni secondo. Sono incluse informazioni di runtime, ad esempio il numero effettivo di righe che passano attraverso gli [operatori](../../relational-databases/showplan-logical-and-physical-operators-reference.md), il tempo trascorso e lo stato della query stimato.

> [!TIP]
> Per altre informazioni sull'elaborazione delle query e sui piani di esecuzione delle query, vedere le sezioni [Ottimizzazione delle istruzioni SELECT](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements) e [Memorizzazione nella cache e riutilizzo del piano di esecuzione](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse) della Guida sull'architettura di elaborazione delle query.

## <a name="in-this-section"></a>Contenuto della sezione  
[Infrastruttura di profilatura query](../../relational-databases/performance/query-profiling-infrastructure.md)     
[Visualizzare e salvare piani di esecuzione](../../relational-databases/performance/display-and-save-execution-plans.md)     
[Confrontare e analizzare i piani di esecuzione](../../relational-databases/performance/compare-and-analyze-execution-plans.md)     
[Guide di piano](../../relational-databases/performance/plan-guides.md)     

## <a name="see-also"></a>Vedere anche  
[Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
[Strumenti per il monitoraggio e l'ottimizzazione delle prestazioni](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
[Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md)    
[Statistiche sulle query dinamiche](../../relational-databases/performance/live-query-statistics.md)     
[Monitoraggio attività](../../relational-databases/performance-monitor/activity-monitor.md)     
[Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
[sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
[sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
[Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
[Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)
