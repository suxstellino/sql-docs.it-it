---
title: Infrastruttura di profilatura delle query | Microsoft Docs
description: Informazioni su come il motore di database di SQL Server accede alle informazioni di runtime sui piani di esecuzione delle query per conoscere il carico di lavoro e come viene gestito l'utilizzo delle risorse.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- query profiling
- lightweight query profiling
- lightweight profiling
- lwp
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 57372929f190ff2fe32e7688d16acc75fafc9700
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171553"
---
# <a name="query-profiling-infrastructure"></a>Infrastruttura di profilatura delle query
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] offre la possibilità di accedere alle informazioni di runtime nei piani di esecuzione delle query. Una delle azioni più importanti quando si verifica un problema di prestazioni consiste nell'individuare in modo preciso il carico di lavoro in esecuzione e la modalità di gestione delle risorse. Per questa ragione, è necessario accedere al [piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md).

Sebbene il completamento della query sia un prerequisito per la disponibilità di un piano di query effettivo, le [statistiche di query dinamiche](../../relational-databases/performance/live-query-statistics.md) possono offrire informazioni in tempo reale sul processo di esecuzione della query durante il passaggio dei dati da un [operatore del piano di query](../../relational-databases/showplan-logical-and-physical-operators-reference.md) all'altro. Il piano dinamico delle query visualizza lo stato complessivo delle query e le statistiche di esecuzione a livello di operatore, ad esempio il numero di righe prodotte, il tempo trascorso, lo stato di avanzamento dell'operatore e così via. Poiché questi dati sono disponibili in tempo reale senza dover attendere il completamento della query, queste statistiche di esecuzione sono estremamente utili per il debug di problemi relativi alle prestazioni delle query, ad esempio query a lunga esecuzione e query a esecuzione infinita che non terminano mai.

## <a name="the-standard-query-execution-statistics-profiling-infrastructure"></a>Infrastruttura di profilatura delle statistiche di esecuzione query standard

L'*infrastruttura del profilo delle statistiche di esecuzione query* o profilatura standard deve essere abilitata per raccogliere informazioni sui piani di esecuzione, ovvero il totale delle righe e l'utilizzo della CPU e dell'I/O. I seguenti metodi di raccolta delle informazioni sui piani di esecuzione per una **sessione di destinazione** usano l'infrastruttura di profilatura standard:

- [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) 
- [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)
- [Statistiche sulle query dinamiche](../../relational-databases/performance/live-query-statistics.md)

> [!NOTE]
> Fare clic sul pulsante *Includi statistiche query dinamiche* in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per usare l'infrastruttura di profilatura standard.    
> Nelle versioni successive di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se l'[infrastruttura di profilatura lightweight](#lwp) è abilitata, verrà usata da Statistiche query dinamiche al posto della profilatura standard quando viene visualizzata tramite [Monitoraggio attività](../../relational-databases/performance-monitor/activity-monitor.md) o eseguendo direttamente una query sul DMV di [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md). 

I seguenti metodi di raccolta delle informazioni sui piani di esecuzione per **tutte le sessioni** usano l'infrastruttura di profilatura standard:

-  Evento esteso **_query_post_execution_showplan_* _. Per abilitare gli eventi estesi, vedere [Monitor System Activity Using Extended Events](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
- Evento traccia _ *Showplan XML** in [Traccia SQL](../../relational-databases/sql-trace/sql-trace.md) e [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md). Per altre informazioni sull'evento traccia, vedere [Classe di evento Showplan XML](../../relational-databases/event-classes/showplan-xml-event-class.md).

Durante l'esecuzione di una sessione di eventi estesi che usa l'evento *query_post_execution_showplan*, viene popolata anche la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) che abilita le statistiche di query dinamiche per tutte le sessioni usando [Monitoraggio attività](../../relational-databases/performance-monitor/activity-monitor.md) o eseguendo la query direttamente nella DMV. Per altre informazioni, vedere [Live Query Statistics](../../relational-databases/performance/live-query-statistics.md).

## <a name="the-lightweight-query-execution-statistics-profiling-infrastructure"></a><a name="lwp"></a> Infrastruttura di profilatura delle statistiche di esecuzione query lightweight

A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 e [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] è stata introdotta una nuova *infrastruttura di profilatura delle statistiche di esecuzione query lightweight* o una **profilatura lightweight**. 

> [!NOTE]
> Le stored procedure compilate in modo nativo non sono supportate con la profilatura lightweight.  

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v1"></a>Infrastruttura di profilatura delle statistiche di esecuzione query lightweight v1

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 a [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). 
  
A partire [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 e [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], con l'introduzione della profilatura lightweight è stato ridotto l'overhead delle prestazioni per raccogliere informazioni sui piani di esecuzione. A differenza della profilatura standard, la profilatura lightweight non raccoglie informazioni di runtime della CPU. La profilatura lightweight continua comunque a raccogliere il totale di righe conteggio delle righe e le informazioni sull'utilizzo dell'I/O.

È stato anche introdotto un nuovo evento esteso **_query_thread_profile_* _ che usa la profilatura lightweight. Questo evento esteso espone le statistiche di esecuzione di ogni operatore offrendo informazioni più approfondite sulle prestazioni di ogni nodo e thread. È possibile configurare una sessione di esempio che usa questo evento esteso come illustrato nell'esempio che segue:

```sql
CREATE EVENT SESSION [NodePerfStats] ON SERVER
ADD EVENT sqlserver.query_thread_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

> [!NOTE]
> Per altre informazioni sull'overhead delle prestazioni della profilatura di query, vedere il post di blog [Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere) (Scelta degli sviluppatori: Avanzamento delle query, sempre e dovunque). 

Durante l'esecuzione di una sessione di eventi estesi che usa l'evento _query_thread_profile*, viene popolata anche la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) tramite la profilatura lightweight e vengono abilitate le statistiche di query dinamiche per tutte le sessioni usando [Monitoraggio attività](../../relational-databases/performance-monitor/activity-monitor.md) o eseguendo la query direttamente nella DMV.

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v2"></a>Infrastruttura di profilatura delle statistiche di esecuzione query lightweight v2

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). 

[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 include una versione rivista della profilatura lightweight con un overhead minimo. La profilatura lightweight può essere anche abilitata a livello globale tramite il [flag di traccia 7412](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) per le versioni indicate nella sezione precedente *Si applica a*. Una nuova DMF [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) viene introdotta per restituire il piano di esecuzione query per le richieste in elaborazione.

A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU3 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11, se la profilatura lightweight non è abilitata globalmente, è possibile usare il nuovo argomento **QUERY_PLAN_PROFILE** dell'[hint per la query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) per abilitare la profilatura a livello di query per qualsiasi sessione. Al termine dell'esecuzione di una query contenente questo nuovo hint, viene generato anche un nuovo evento esteso **_query_plan_profile_* che fornisce l'XML di un piano di esecuzione effettivo simile all'evento esteso _query_post_execution_showplan*. 

> [!NOTE]
> L'evento esteso *query_plan_profile* si avvale anche della profilatura leggera benché non venga usato l'hint per la query. 

Una sessione di esempio che usa l'evento esteso *query_plan_profile* può essere configurata come illustrato nell'esempio seguente:

```sql
CREATE EVENT SESSION [PerfStats_LWP_Plan] ON SERVER
ADD EVENT sqlserver.query_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v3"></a>Infrastruttura di profilatura delle statistiche di esecuzione query lightweight v3

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] includono una nuova versione rivista della profilatura leggera che raccoglie informazioni sul numero di righe per tutte le esecuzioni. La profilatura leggera è abilitata per impostazione predefinita in [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], il flag di traccia 7412 non ha alcun effetto. È possibile disabilitare la profilatura lightweight a livello di database usando la [configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LIGHTWEIGHT_QUERY_PROFILING: `ALTER DATABASE SCOPED CONFIGURATION SET LIGHTWEIGHT_QUERY_PROFILING = OFF;`.

È stata introdotta una nuova DMF [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md) per restituire l'equivalente dell'ultimo piano di esecuzione effettivo noto per la maggior parte delle query. Tale DMF è denominata *statistiche dell'ultimo piano di query*. È possibile abilitare le statistiche dell'ultimo piano di query a livello di database usando la [configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LAST_QUERY_PLAN_STATS: `ALTER DATABASE SCOPED CONFIGURATION SET LAST_QUERY_PLAN_STATS = ON;`.

Un nuovo evento esteso *query_post_execution_plan_profile* raccoglie l'equivalente di un piano di esecuzione effettivo in base alla profilatura leggera, a differenza di *query_post_execution_showplan* che usa la profilatura standard. Anche [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] offre questo evento a partire dalla versione CU14. È possibile configurare una sessione di esempio che usa l'evento esteso *query_post_execution_plan_profile* come illustrato nell'esempio seguente:

```sql
CREATE EVENT SESSION [PerfStats_LWP_All_Plans] ON SERVER
ADD EVENT sqlserver.query_post_execution_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

#### <a name="example-1---extended-event-session-using-standard-profiling"></a>Esempio 1 - Sessione Evento esteso con profilatura standard

```sql
CREATE EVENT SESSION [QueryPlanOld] ON SERVER 
ADD EVENT sqlserver.query_post_execution_showplan(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename = N'C:\Temp\QueryPlanStd.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

#### <a name="example-2---extended-event-session-using-lightweight-profiling"></a>Esempio 2 - Sessione Evento esteso con profilatura leggera

```sql
CREATE EVENT SESSION [QueryPlanLWP] ON SERVER 
ADD EVENT sqlserver.query_post_execution_plan_profile(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename=N'C:\Temp\QueryPlanLWP.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

## <a name="query-profiling-infrastruture-usage-guidance"></a>Linee guida per l'utilizzo dell'infrastruttura di profilatura di query
La tabella seguente riepiloga le azioni per abilitare la profilatura standard o la profilatura leggera, sia a livello globale (a livello di server) che in una singola sessione. Include anche la versione minima per cui è disponibile l'azione. 

|Scope|Profilatura standard|Profilatura leggera|
|---------------|---------------|---------------|
|Global|Sessione xEvent con `query_post_execution_showplan` XE; a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|Flag di traccia 7412; a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1|
|Global|Traccia SQL e SQL Server Profiler con l'evento di traccia `Showplan XML`; a partire da SQL Server 2000|Sessione xEvent con `query_thread_profile` XE; a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2|
|Global|-|Sessione xEvent con `query_post_execution_plan_profile` XE; a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU14 e [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|
|sessione|Usare `SET STATISTICS XML ON`; a partire da SQL Server 2000|Usare l'hint per la query `QUERY_PLAN_PROFILE` insieme a una sessione xEvent con `query_plan_profile` XE; a partire [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU3 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11|
|sessione|Usare `SET STATISTICS PROFILE ON`; a partire da SQL Server 2000|-|
|sessione|Fare clic sul pulsante [Statistiche query dinamiche](../../relational-databases/performance/live-query-statistics.md) in SSMS; a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2|-|

## <a name="remarks"></a>Osservazioni

> [!IMPORTANT]
> A causa di un possibile AV casuale, durante l'esecuzione di una stored procedure di monitoraggio che fa riferimento a [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md), assicurarsi che sia installato [KB 4078596](https://support.microsoft.com/help/4078596) in [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

A partire dalla profilatura lightweight v2 con overhead ridotto, qualsiasi server che non è già basato su CPU può eseguire la profilatura lightweight **in modo continuo** e consentire ai professionisti di database di inserirsi in qualsiasi esecuzione in corso in qualsiasi momento, ad esempio usando Monitoraggio attività o eseguendo direttamente una query in `sys.dm_exec_query_profiles` e ottenere il piano di query con le statistiche di runtime.

Per altre informazioni sull'overhead delle prestazioni della profilatura di query, vedere il post di blog [Developers Choice: Query progress - anytime, anywhere](https://techcommunity.microsoft.com/t5/SQL-Server/Developers-Choice-Query-progress-anytime-anywhere/ba-p/385004) (Scelta degli sviluppatori: Avanzamento delle query, sempre e dovunque). 

> [!NOTE]
> Se l'infrastruttura di profilatura standard è già abilitata, gli eventi estesi che sfruttano la profilatura leggera useranno le informazioni disponibili nella profilatura standard. Si supponga ad esempio che sia in esecuzione una sessione di evento esteso che usa `query_post_execution_showplan` e che venga avviata un'altra sessione che usa `query_post_execution_plan_profile`. La seconda sessione userà le informazioni provenienti dalla profilatura standard.

> [!NOTE]
> In [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] la profilatura leggera è disattivata per impostazione predefinita, ma viene attivata quando viene avviata una traccia XEvent che si basa su `query_post_execution_plan_profile` e viene quindi disattivata di nuovo quando la traccia viene arrestata. Di conseguenza, se le tracce XEvent basate su `query_post_execution_plan_profile` vengono spesso avviate e arrestate in un'istanza di [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], è consigliabile attivare la profilatura leggera a livello globale con il flag di traccia 7412 per evitare il sovraccarico ripetuto di attivazione/disattivazione. 

## <a name="see-also"></a>Vedere anche  
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Strumenti per il monitoraggio e l'ottimizzazione delle prestazioni](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Aprire Monitoraggio attività &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Monitoraggio attività](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Monitorare l'attività del sistema mediante gli eventi estesi](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)      
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
 [piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md)    
 [Statistiche sulle query dinamiche](../../relational-databases/performance/live-query-statistics.md)