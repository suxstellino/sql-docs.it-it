---
title: ALTER DATABASE SCOPED CONFIGURATION
description: Abilitare diverse impostazioni di configurazione del database a livello di singolo database.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- ALTER_DATABASE_SCOPED_CONFIGURATION
- ALTER_DATABASE_SCOPED_CONFIGURATION_TSQL
- DATABASE_SCOPED_CONFIGURATION_TSQL
- SCOPED_CONFIGURATION_TSQL
- SCOPED_TSQL
- ALTER_DATABASE_SCOPED_TSQL
- DATABASE_SCOPED_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CONFIGURATION statement
- configuration [SQL Server], ALTER DATABASE SCOPED CONFIGURATION statement
ms.assetid: 63373c2f-9a0b-431b-b9d2-6fa35641571a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017 ||=azure-sqldw-latest
ms.openlocfilehash: 647e68fa167b24bc713266fd45527337bdd6b9e6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348540"
---
# <a name="alter-database-scoped-configuration-transact-sql"></a>ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Questo comando abilita diverse impostazioni di configurazione del database a livello di **singolo database**. 

Le impostazioni seguenti sono supportate [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] in [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] e in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come indicato dalla riga **si applica a** per ogni impostazione nella sezione [argomenti](#arguments) : 

- Cancellare la cache delle procedure.
- Impostare il parametro MAXDOP su un valore arbitrario (1,2,...) per il database primario in base a ciò che è più adatto e impostare un valore diverso (ad esempio 0) per tutti i database secondari usati (ad esempio per le query di report).
- Impostare il modello di stima della cardinalità di Query Optimizer su un livello di compatibilità, indipendentemente dal database.
- Abilitare o disabilitare l'analisi dei parametri a livello di database.
- Abilitare o disabilitare gli hotfix di ottimizzazione query a livello di database.
- Abilitare o disabilitare la cache Identity a livello di database.
- Abilitare o disabilitare uno stub del piano compilato da memorizzare nella cache quando un batch viene compilato per la prima volta.
- Abilita o Disabilita la raccolta di statistiche di esecuzione per i moduli compilati in modo nativo [!INCLUDE[tsql](../../includes/tsql-md.md)] .
- Abilitare o disabilitare le opzioni online per impostazione predefinita per le istruzioni DDL che supportano la sintassi `ONLINE =`.
- Abilitare o disabilitare le opzioni ripristinabile per impostazione predefinita per le istruzioni DDL che supportano la sintassi `RESUMABLE =`.
- Abilitare o disabilitare le funzionalità di [elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md).
- Abilitare o disabilitare l'uso forzato del piano accelerato.
- Abilitare o disabilitare le funzionalità di eliminazione automatica delle tabelle temporanee globali.
- Abilitare o disabilitare l'[infrastruttura leggera di profilatura query](../../relational-databases/performance/query-profiling-infrastructure.md).
- Abilitare o disabilitare il nuovo messaggio di errore `String or binary data would be truncated`.
- Abilita l'equivalente dell'ultimo piano di esecuzione effettivo in [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).
- Consente di specificare il numero di minuti di sospensione di un'operazione di indici ripristinabili sospesa prima che venga interrotto automaticamente da [!INCLUDE[ssde_md](../../includes/ssde_md.md)] .
- Abilitare o disabilitare l'attesa dei blocchi con priorità bassa per l'aggiornamento asincrono delle statistiche.

Questa impostazione è disponibile solo in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] .
- Impostare il livello di compatibilità di un database utente

![Icona di collegamento](../../database-engine/configure-windows/media/topic-link.gif "icona di collegamento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure SQL Managed Instance

ALTER DATABASE SCOPED CONFIGURATION
{
    { [ FOR SECONDARY] SET <set_options>}
}
| CLEAR PROCEDURE_CACHE [plan_handle]
| SET < set_options >
[;]

< set_options > ::=
{
    MAXDOP = { <value> | PRIMARY}
    | LEGACY_CARDINALITY_ESTIMATION = { ON | OFF | PRIMARY}
    | PARAMETER_SNIFFING = { ON | OFF | PRIMARY}
    | QUERY_OPTIMIZER_HOTFIXES = { ON | OFF | PRIMARY}
    | IDENTITY_CACHE = { ON | OFF }
    | INTERLEAVED_EXECUTION_TVF = { ON | OFF }
    | BATCH_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ADAPTIVE_JOINS = { ON | OFF }
    | TSQL_SCALAR_UDF_INLINING = { ON | OFF }
    | ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | ELEVATE_RESUMABLE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | OPTIMIZE_FOR_AD_HOC_WORKLOADS = { ON | OFF }
    | XTP_PROCEDURE_EXECUTION_STATISTICS = { ON | OFF }
    | XTP_QUERY_EXECUTION_STATISTICS = { ON | OFF }
    | ROW_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ON_ROWSTORE = { ON | OFF }
    | DEFERRED_COMPILATION_TV = { ON | OFF }
    | ACCELERATED_PLAN_FORCING = { ON | OFF }
    | GLOBAL_TEMPORARY_TABLE_AUTO_DROP = { ON | OFF }
    | LIGHTWEIGHT_QUERY_PROFILING = { ON | OFF }
    | VERBOSE_TRUNCATION_WARNINGS = { ON | OFF }
    | LAST_QUERY_PLAN_STATS = { ON | OFF }
    | PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = <time>
    | ISOLATE_SECURITY_POLICY_CARDINALITY  = { ON | OFF }
    | EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS = { ON | OFF }
    | ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY = { ON | OFF }
}
```

> [!IMPORTANT]
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] , alcuni nomi di opzioni sono stati modificati:      
> -  `DISABLE_INTERLEAVED_EXECUTION_TVF` è diventata `INTERLEAVED_EXECUTION_TVF`
> -  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` è diventata `BATCH_MODE_MEMORY_GRANT_FEEDBACK`
> -  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` è diventata `BATCH_MODE_ADAPTIVE_JOINS`

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER DATABASE SCOPED CONFIGURATION
{
    SET <set_options>
}
[;]

< set_options > ::=
{
    DW_COMPATIBILITY_LEVEL = { AUTO | 10 | 20 } 
}
```

## <a name="arguments"></a>Argomenti

FOR SECONDARY

Specifica le impostazioni per i database secondari. Tutti i database secondari devono avere valori identici.

CLEAR PROCEDURE_CACHE [plan_handle]

Cancella la cache delle procedure (piani) per il database e può essere eseguito sia nel database primario che in quelli secondari.

Specificare un handle di piano di query per cancellare un singolo piano di query dalla cache dei piani.

**Si applica a**: la specifica di un handle del piano di query è disponibile in a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] .

MAXDOP **=** {\<value> | PRIMARY } **\<value>**

Specifica l'impostazione del **massimo grado di parallelismo (MAXDOP)** predefinita da usare per le istruzioni. 0 è il valore predefinito. Indica che in alternativa sarà usata la configurazione server. Nell'ambito del database il parametro MAXDOP (a meno che non sia impostato su 0) sostituisce il **massimo grado di parallelismo** impostato a livello di server da sp_configure. Gli hint per la query possono tuttavia sostituire il parametro MAXDOP con ambito database per ottimizzare le query specifiche per cui è necessaria un'impostazione diversa. Tutte queste impostazioni sono vincolate dal parametro MAXDOP definito per il [gruppo di carico di lavoro](create-workload-group-transact-sql.md).

È possibile usare l'opzione MAXDOP per limitare il numero di processori da usare per l'esecuzione di piani paralleli. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valuta i piani di esecuzione parallela per query, operazioni DDL (Data Definition Language) sugli indici, inserimento parallelo, modifica colonna online, raccolta di statistiche parallela e popolamento dei cursori statici e gestiti da keyset.

> [!NOTE]
> Il limite del **massimo grado di parallelismo (MAXDOP)** è impostato per [attività](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Non è un limite per [richiesta](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) o per query. Ciò significa che durante l'esecuzione di query parallele una singola richiesta può generare più attività che vengono assegnate a un'[utilità di pianificazione](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Per altre informazioni, vedere [Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md).

Per impostare questa opzione a livello di istanza, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

> [!NOTE]
> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], la configurazione con ambito database MAXDOP per i nuovi pool di database elastico e singolo è impostata su 8 per impostazione predefinita. MAXDOP può essere configurato per ogni database, come descritto nell'articolo corrente. Per consigli sulla configurazione ottimale di MAXDOP, vedere la sezione [Risorse aggiuntive](#additional-resources).

> [!TIP]
> Per eseguire questa operazione a livello di query, usare l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**.    
> Per eseguire questa operazione a livello di server, usare l'[opzione di configurazione server](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) relativa al **massimo grado di parallelismo (MAXDOP)** .     
> Per eseguire questa operazione a livello di carico di lavoro, usare l'[opzione di configurazione del gruppo di carico di lavoro di Resource Governor](../../t-sql/statements/create-workload-group-transact-sql.md) **MAX_DOP**.    

PRIMARY

Può essere impostato solo per i database secondari se il database è primario. Indica che la configurazione corrisponderà a quella impostata per il database primario. Se la configurazione per il database primario viene modificata, il valore nei database secondari sarà modificato di conseguenza senza dover impostare in modo esplicito il valore nei database secondari. **PRIMARY** è l'impostazione predefinita per i database secondari.

LEGACY_CARDINALITY_ESTIMATION **=** { ON | **OFF** | PRIMARY }

Consente di impostare il modello di stima della cardinalità di Query Optimizer in SQL Server 2012 o versioni precedenti indipendentemente dal livello di compatibilità del database. Il valore predefinito è **OFF** che imposta il modello di stima della cardinalità di Query Optimizer sulla base del livello di compatibilità del database. Impostare LEGACY_CARDINALITY_ESTIMATION su **ON** equivale ad abilitare il [flag di traccia 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Per eseguire questa operazione a livello di query, aggiungere l'[hint per la query](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**.
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, per eseguire questa operazione a livello di query, aggiungere l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT** invece di usare il flag di traccia.

PRIMARY

Questo valore è valido solo nei database secondari quando il database è primario. Specifica che l'impostazione del modello di stima della cardinalità di Query Optimizer in tutti i database secondari sarà il valore impostato per il database primario. Se la configurazione per il modello di stima di cardinalità di Query Optimizer viene modificata nel database primario, il valore nei database secondari sarà modificato di conseguenza. **PRIMARY** è l'impostazione predefinita per i database secondari.

PARAMETER_SNIFFING **=** { **on** | DISATTIVATO | PRIMARIO

Abilita o disabilita l'[analisi dei parametri](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing). Il valore predefinito è ON. Impostare PARAMETER_SNIFFING su OFF equivale ad abilitare il [flag di traccia 4136](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Per eseguire questa operazione a livello di query, vedere l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md) **OPTIMIZE FOR UNKNOWN**.
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, per eseguire questa operazione a livello di query, è disponibile anche l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT**.

PRIMARY

Questo valore è valido solo nei database secondari quando il database è primario. Specifica che il valore per questa impostazione in tutti i database secondari sarà il valore impostato per il database primario. Se la configurazione per l'uso dell'[analisi dei parametri](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) viene modificata nel database primario, il valore nei database secondari sarà modificato di conseguenza senza dover impostare in modo esplicito il valore nei database secondari. PRIMARY è l'impostazione predefinita per i database secondari.

<a name="qo_hotfixes"></a> QUERY_OPTIMIZER_HOTFIXES **=** { ON | **OFF** | PRIMARY }

Abilita o disabilita gli hotfix di ottimizzazione query indipendentemente dal livello di compatibilità del database. Il valore predefinito è **OFF**, che disabilita gli hotfix di ottimizzazione query rilasciati dopo che è stato introdotto il massimo livello di compatibilità disponibile per una specifica versione (post RTM). L'impostazione di **ON** equivale ad abilitare il [flag di traccia 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

> [!TIP]
> Per eseguire questa operazione a livello di query, aggiungere l'[hint per la query](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**.
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, per eseguire questa operazione a livello di query, aggiungere l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md#use_hint) USE HINT anziché usare il flag di traccia.

PRIMARY

Questo valore è valido solo nei database secondari quando il database è primario. Specifica che il valore per questa impostazione in tutti i database secondari è il valore impostato per il database primario. Se la configurazione per il database primario viene modificata, il valore nei database secondari viene modificato di conseguenza senza dover impostare in modo esplicito il valore nei database secondari. PRIMARY è l'impostazione predefinita per i database secondari.

IDENTITY_CACHE **=** { **ON** | OFF }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Abilita o disabilita la cache Identity a livello di database. Il valore predefinito è **ON**. La memorizzazione nella cache di Identity serve a migliorare le prestazioni di INSERT nelle tabelle che contengono colonne Identity. Disabilitare l'opzione IDENTITY_CACHE per evitare scostamenti nei valori in una colonna Identity nel caso in cui un server sia riavviato in modo imprevisto o esegua un failover in un server secondario. Questa opzione è simile all'attuale [flag di traccia 272](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md), ad eccezione del fatto che può essere impostata a livello di database, anziché solo a livello di server.

> [!NOTE]
> Questa opzione può essere impostata solo per PRIMARY. Per altre informazioni, vedere [Colonne Identity](create-table-transact-sql-identity-property.md).

INTERLEAVED_EXECUTION_TVF **=** { **ON** | OFF }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare l'esecuzione interleaved per funzioni con valori di tabella a più istruzioni nell'ambito del database o dell'istruzione mantenendo comunque la compatibilità sul livello 140 o superiore. L'esecuzione interleaved è una funzionalità che fa parte dell'elaborazione di query adattive in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Per altre informazioni, vedere [Elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Per livelli di compatibilità del database pari a 130 o inferiori, questa configurazione con ambito di database non ha effetto.
>
> Solo in SQL Server 2017 (14.x), l'opzione INTERLEAVED_EXECUTION_TVF ha il nome precedente **DISABLE** _INTERLEAVED_EXECUTION_TVF.

BATCH_MODE_MEMORY_GRANT_FEEDBACK **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare il feedback delle concessioni di memoria in modalità batch nell'ambito del database mantenendo comunque un livello di compatibilità del database pari a 140 o superiore. Il feedback delle concessioni di memoria in modalità batch è una funzionalità che fa parte dell'[elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md) introdotta in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

> [!NOTE]
> Per livelli di compatibilità del database pari a 130 o inferiori, questa configurazione con ambito di database non ha effetto.

BATCH_MODE_ADAPTIVE_JOINS **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare i join adattivi in modalità batch nell'ambito del database mantenendo comunque un livello di compatibilità del database pari a 140 o superiore. I join adattivi in modalità batch sono una funzionalità che fa parte dell'[elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md) introdotta in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

> [!NOTE]
> Per livelli di compatibilità del database pari a 130 o inferiori, questa configurazione con ambito di database non ha effetto.

TSQL_SCALAR_UDF_INLINING **=** { **ON** | OFF }

**SI APPLICA A**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (la funzionalità è disponibile nell'anteprima pubblica)

Consente di abilitare o disabilitare l'inlining di funzioni definite dall'utente scalari T-SQL nell'ambito del database mantenendo comunque un livello di compatibilità del database pari a 150 o superiore. L'inlining di funzioni definite dall'utente scalari T-SQL fa parte della famiglia di funzionalità di [elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Per il livello di compatibilità del database pari a 140 o inferiore, questa configurazione con ambito di database non ha effetto.

ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di selezionare opzioni grazie alle quali il motore eleva automaticamente le operazioni supportate all'esecuzione online. Il valore predefinito è OFF. Ciò significa che le operazioni verranno elevate all'esecuzione online solo se specificato nell'istruzione. [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) riflette il valore corrente di ELEVATE_ONLINE. Queste opzioni si applicano solo alle operazioni supportate per l'esecuzione online.

FAIL_UNSUPPORTED

Questo valore eleva tutte le operazioni DDL supportate all'esecuzione ONLINE. Le operazioni che non supportano l'esecuzione online hanno esito negativo e generano un avviso.

WHEN_SUPPORTED

Questo valore eleva le operazioni che supportano l'esecuzione ONLINE. Le operazioni che non supportano l'esecuzione online verranno eseguite offline.

> [!NOTE]
> È possibile eseguire l'override dell'impostazione predefinita inviando un'istruzione con l'opzione ONLINE specificata.

ELEVATE_RESUMABLE= { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di selezionare opzioni grazie alle quali il motore eleva automaticamente le operazioni supportate all'esecuzione ripristinabile. Il valore predefinito è OFF. Ciò significa che le operazioni verranno elevate all'esecuzione ripristinabile solo se specificato nell'istruzione. [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) riflette il valore corrente di ELEVATE_RESUMABLE. Queste opzioni si applicano solo alle operazioni supportate per l'esecuzione ripristinabile.

FAIL_UNSUPPORTED

Questo valore eleva tutte le operazioni DDL supportate all'esecuzione RESUMABLE. Le operazioni che non supportano l'esecuzione ripristinabile hanno esito negativo e generano un avviso.

WHEN_SUPPORTED

Questo valore eleva le operazioni che supportano l'esecuzione RESUMABLE. Le operazioni che non supportano l'esecuzione ripristinabile vengono eseguite in modo non ripristinabile.

> [!NOTE]
> È possibile eseguire l'override dell'impostazione predefinita inviando un'istruzione con l'opzione RESUMABLE specificata.

OPTIMIZE_FOR_AD_HOC_WORKLOADS **=** { ON | **OFF** }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Abilita o disabilita uno stub del piano compilato da memorizzare nella cache quando un batch viene compilato per la prima volta. Il valore predefinito è OFF. Dopo aver abilitato la configurazione con ambito database OPTIMIZE_FOR_AD_HOC_WORKLOADS per un database, uno stub del piano compilato sarà archiviato nella cache quando un batch viene compilato per la prima volta. Il footprint di memoria degli stub del piano è ridotto rispetto alle dimensioni del piano compilato completo. Se un batch viene compilato o eseguito nuovamente, lo stub del piano compilato sarà rimosso e sostituito da un piano compilato completo.

XTP_PROCEDURE_EXECUTION_STATISTICS **=** { ON | **OFF** }

**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Abilita o disabilita la raccolta di statistiche di esecuzione a livello di modulo per i moduli T-SQL compilati in modo nativo nel database corrente. Il valore predefinito è OFF. Le statistiche di esecuzione vengono riflesse in [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md).

Le statistiche di esecuzione a livello di modulo per i moduli T-SQL compilati in modo nativo vengono raccolte se questa opzione è ON o se la raccolta di statistiche è abilitata mediante [sp_xtp_control_proc_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).

XTP_QUERY_EXECUTION_STATISTICS **=** { ON | **OFF** }

**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Abilita o disabilita la raccolta di statistiche di esecuzione a livello di istruzione per i moduli di T-SQL compilati in modo nativo nel database corrente. Il valore predefinito è OFF. Le statistiche di esecuzione vengono riflesse in [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) e in [Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

Le statistiche di esecuzione a livello di istruzione per i moduli T-SQL compilati in modo nativo vengono raccolte se questa opzione è ON o se la raccolta di statistiche è abilitata mediante [sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

Per altre informazioni sul monitoraggio delle prestazioni dei moduli [!INCLUDE[tsql](../../includes/tsql-md.md)] compilati in modo nativo, vedere [Monitoraggio delle prestazioni di stored procedure compilate in modo nativo](../../relational-databases/in-memory-oltp/monitoring-performance-of-natively-compiled-stored-procedures.md).

ROW_MODE_MEMORY_GRANT_FEEDBACK **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare il feedback delle concessioni di memoria in modalità riga nell'ambito del database mantenendo comunque un livello di compatibilità del database pari a 150 o superiore. Il feedback delle concessioni di memoria in modalità riga è una funzionalità che fa parte dell'[elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md) introdotta in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] (la modalità riga è supportata in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

> [!NOTE]
> Per il livello di compatibilità del database pari a 140 o inferiore, questa configurazione con ambito di database non ha effetto.

BATCH_MODE_ON_ROWSTORE **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare la modalità batch per i rowstore nell'ambito del database mantenendo comunque un livello di compatibilità del database pari a 150 o superiore. La modalità batch per i rowstore è una funzionalità che fa parte della famiglia di funzionalità di [elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Per il livello di compatibilità del database pari a 140 o inferiore, questa configurazione con ambito di database non ha effetto.

DEFERRED_COMPILATION_TV **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare la compilazione posticipata delle variabili di tabella nell'ambito del database mantenendo comunque un livello di compatibilità del database pari a 150 o superiore. La compilazione posticipata delle variabili di tabella è una funzionalità che fa parte della famiglia di funzionalità di [elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Per il livello di compatibilità del database pari a 140 o inferiore, questa configurazione con ambito di database non ha effetto.

ACCELERATED_PLAN_FORCING **=** { **ON** | OFF }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Abilita un meccanismo ottimizzato per l'uso forzato del piano di query, applicabile a tutte le forme di uso forzato dei piani, ad esempio [piani forzati da Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), l'[ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction) o l'hint per la query [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md#use-plan). Il valore predefinito è ON.

> [!NOTE]
> Non è consigliabile disabilitare l'uso forzato del piano accelerato.

GLOBAL_TEMPORARY_TABLE_AUTO_DROP **=** { **ON** | OFF }

**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di impostare la funzionalità di eliminazione automatica per le [tabelle temporanee globali](../../t-sql/statements/create-table-transact-sql.md#temporary-tables). Il valore predefinito è ON, il che significa che le tabelle temporanee globali vengono eliminate automaticamente quando non sono usate in nessuna sessione. Se impostato su OFF, le tabelle temporanee globali devono essere eliminate in modo esplicito usando un'istruzione DROP TABLE, altrimenti verranno eliminate automaticamente al riavvio del server.

- Con database singoli e pool elastici di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], questa opzione può essere impostata nei singoli database utente del server di database SQL.
- In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e in Istanza gestita di SQL di Azure questa opzione è impostata in `TempDB` e l'impostazione dei singoli database utente non ha effetto.

<a name="lqp"></a>

LIGHTWEIGHT_QUERY_PROFILING **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare l'[infrastruttura leggera di profilatura query](../../relational-databases/performance/query-profiling-infrastructure.md). L'infrastruttura leggera di profilatura query restituisce dati sulle prestazioni delle query in modo più efficiente rispetto ai meccanismi di profilatura standard ed è abilitata per impostazione predefinita.

<a name="verbose-truncation"></a>

VERBOSE_TRUNCATION_WARNINGS **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare il nuovo messaggio di errore `String or binary data would be truncated`. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce un nuovo messaggio di errore più specifico (2628) per questo scenario:

`String or binary data would be truncated in table '%.*ls', column '%.*ls'. Truncated value: '%.*ls'.`

Se impostato su ON nel livello di compatibilità del database 150, gli errori di troncamento generano il nuovo messaggio di errore 2628 per offrire maggiore contesto e semplificare il processo di risoluzione dei problemi.

Se impostato su OFF nel livello di compatibilità del database 150, gli errori di troncamento generano il messaggio di errore 8152 precedente.

Per il livello di compatibilità del database 140 o inferiore, il 2628 rimane un messaggio di errore che prevede il consenso esplicito e richiede l'abilitazione del [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 460 e questa configurazione con ambito database non ha alcun effetto.

LAST_QUERY_PLAN_STATS **=** {on | **Off** }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di abilitare o disabilitare la raccolta delle statistiche dell'ultimo piano di query (equivalente a un piano di esecuzione effettivo) in [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).

PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES

**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

L'opzione `PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES` determina la durata (in minuti) della sospensione dell'indice ripristinabile prima che venga interrotto automaticamente dal motore.

- Il valore predefinito è impostato su 1 giorno (1440 minuti)
- La durata minima è impostata su 1 minuto
- La durata massima corrisponde a 71582 minuti
- Se il valore è impostato su 0, un'operazione sospesa non viene mai interrotta automaticamente

Il valore corrente di questa opzione è visualizzato in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).

ISOLATE_SECURITY_POLICY_CARDINALITY **=** { ON | **OFF**}

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di controllare se un predicato di [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md) influisce sulla cardinalità del piano di esecuzione della query utente complessiva. Quando ISOLATE_SECURITY_POLICY_CARDINALITY è impostato su ON, un predicato di Sicurezza a livello di riga non influisce sulla cardinalità di un piano di esecuzione. Si considerino, ad esempio, una tabella contenente 1 milione di righe e un predicato di Sicurezza a livello di riga che limita il risultato a 10 righe per un utente specifico che esegue la query. Se questa configurazione con ambito di database è impostata su OFF, la stima della cardinalità di questo predicato sarà 10. Se invece è impostata su ON, l'ottimizzazione delle query stimerà 1 milione righe. È consigliabile usare il valore predefinito per la maggior parte dei carichi di lavoro.

DW_COMPATIBILITY_LEVEL **=** {**AUTO** | 10 | 20 }

**Si applica a**: [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] solo

Imposta [!INCLUDE[tsql](../../includes/tsql-md.md)] i comportamenti di elaborazione delle query e li rende compatibili con la versione specificata del motore di database. Una volta impostata, quando viene eseguita una query su tale database, verranno applicate solo le funzionalità compatibili.  Per impostazione predefinita, il livello di compatibilità di un database viene impostato su AUTO al momento della creazione. Il livello di compatibilità viene mantenuto anche dopo operazioni di sospensione/ripresa o backup/ripristino del database. 

|Livello di compatibilità    |   Commenti|  
|-----------------------|--------------|
|**AUTO**| Valore predefinito.  Il valore viene aggiornato automaticamente dal motore di Synapse Analytics.  Il valore corrente è 20.|
|**10**| Applica i comportamenti Transact-SQL e di elaborazione delle query prima dell'introduzione del supporto del livello di compatibilità.|
|**20**| Primo livello di compatibilità che include i comportamenti di elaborazione delle query e Transact-SQL controllati. |

EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS **=** { **on** | OFF

**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Consente di controllare se le statistiche di esecuzione per funzioni definite dall'utente scalari (UDF) vengono visualizzate nella vista di sistema [sys.dm_exec_function_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-function-stats-transact-sql.md) . Per alcuni carichi di lavoro intensivi che sono scalari UDF-Heavy, la raccolta delle statistiche di esecuzione della funzione può causare un overhead significativo delle prestazioni. Questa operazione può essere evitata impostando la `EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS` configurazione con ambito database su `OFF` .

ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY **=** {on | **Off** }

**SI APPLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (la funzionalità è disponibile in anteprima pubblica)

Se è attivo l'aggiornamento asincrono delle statistiche, con questa configurazione abilitata, la richiesta in background di aggiornamento delle statistiche resterà in attesa di un blocco di modifica dello schema in una coda separata con priorità bassa, in modo da evitare il blocco di altre sessioni in scenari di concorrenza elevata. Per altre informazioni, vedere [AUTO_UPDATE_STATISTICS_ASYNC](../../relational-databases/statistics/statistics.md#auto_update_statistics_async).

## <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

Richiede `ALTER ANY DATABASE SCOPED CONFIGURATION` per il database. Questa autorizzazione può essere concessa da un utente con `CONTROL` autorizzazione per un database.

## <a name="general-remarks"></a>Osservazioni generali

Tutti i database secondari usano la stessa configurazione, nonostante sia possibile configurarli in modo che abbiamo impostazioni di configurazione con ambiti diversi rispetto al database primario. Non è possibile configurare impostazioni diverse per singoli database secondari.

Se viene eseguita questa istruzione, viene cancellata la cache delle procedure nel database corrente, il che significa che è necessario ricompilare tutte le query.

Per le query con nome in 3 parti, vengono prese in considerazione le impostazioni per la connessione al database corrente per la query, a differenza dei moduli SQL (ad esempio procedure, funzioni e trigger) che vengono compilati nel contesto di un altro database e quindi usano le opzioni del database in cui risiedono. Analogamente, quando si aggiornano le statistiche in modo asincrono, viene rispettata l'impostazione di ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY relativa al database in cui risiedono le statistiche.

L'evento `ALTER_DATABASE_SCOPED_CONFIGURATION` viene aggiunto come evento DDL che può essere usato per attivare un trigger DDL ed è elemento figlio del gruppo di trigger `ALTER_DATABASE_EVENTS`.

Le impostazioni di configurazione con ambito database verranno trasferite con il database, il che significa che quando un determinato database viene ripristinato o collegato, le impostazioni di configurazione esistenti rimangono.

A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] , alcuni nomi di opzioni sono stati modificati:      
-  `DISABLE_INTERLEAVED_EXECUTION_TVF` è diventata `INTERLEAVED_EXECUTION_TVF`
-  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` è diventata `BATCH_MODE_MEMORY_GRANT_FEEDBACK`
-  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` è diventata `BATCH_MODE_ADAPTIVE_JOINS`

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

### <a name="maxdop"></a>MAXDOP

Le impostazioni granulari possono sostituire quelle globali e Resource Governor può limitare tutte le altre impostazioni MAXDOP. La logica per l'impostazione MAXDOP è la seguente:

- L'hint per la query sostituisce sia `sp_configure`, sia la configurazione con ambito database. Se il parametro MAXDOP del gruppo di risorse è impostato per il gruppo di carico di lavoro:

  - Se l'hint per la query è impostato su zero (0), viene sostituito dall'impostazione di Resource Governor.

  - Se l'hint per la query non è zero (0), viene limitato dall'impostazione di Resource Governor.

- La configurazione con ambito database, a meno che non sia zero, sostituisce l'impostazione `sp_configure`, sempre che non sia presente un hint per la query e non vi sia un limite di impostazione di Resource Governor.

- L'impostazione `sp_configure` viene sostituita dall'impostazione di Resource Governor.

### <a name="query_optimizer_hotfixes"></a>QUERY_OPTIMIZER_HOTFIXES

Quando `QUERYTRACEON` l'hint viene utilizzato per abilitare la query optimizer predefinita di SQL Server 7,0 tramite le [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] versioni o gli hotfix di query optimizer, si tratta di una condizione OR tra l'hint per la query e l'impostazione di configurazione con ambito database, vale a dire se è abilitata, vengono applicate le configurazioni con ambito database.

### <a name="geo-dr"></a>Geo DR

I database secondari leggibili (Always On [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] i gruppi di disponibilità e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] i database con replica geografica) utilizzano il valore secondario controllando lo stato del database. Anche se la ricompilazione non avviene in caso di failover e tecnicamente il nuovo database primario contiene le query che usano le impostazioni del database secondario, l'idea è che l'impostazione tra primario e secondario vari solo quando il carico di lavoro è diverso e pertanto le query memorizzate nella cache usino le impostazioni ottimali, mentre le nuove query selezionino le nuove impostazioni adatte a loro.

### <a name="dacfx"></a>DacFx

Poiché `ALTER DATABASE SCOPED CONFIGURATION` è una nuova funzionalità di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] ) che influiscono sullo schema del database, le esportazioni dello schema (con o senza dati) non possono essere importate in una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] . Ad esempio, un'esportazione in [DACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md) o [BACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) da un database di [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] in cui è stata usata questa nuova funzionalità non può essere importata in un server legacy.

### <a name="elevate_online"></a>ELEVATE_ONLINE

Questa opzione si applica solo alle istruzioni DDL che supportano la sintassi `WITH (ONLINE = <syntax>)`. Gli indici XML non sono interessati.

### <a name="elevate_resumable"></a>ELEVATE_RESUMABLE

Questa opzione si applica solo alle istruzioni DDL che supportano la sintassi `WITH (RESUMABLE = <syntax>)`. Gli indici XML non sono interessati.

## <a name="metadata"></a>Metadati

La vista di sistema [database_scoped_configurations &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) contiene informazioni sulle configurazioni con ambito database. Le opzioni di configurazione con ambito database vengono visualizzate solo in sys.database_scoped_configurations in quanto vengono sostituite dalle impostazioni predefinite a livello di server. La vista di sistema [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) contiene solo impostazioni a livello di server.

## <a name="examples"></a>Esempi

In questi esempi viene illustrato l'uso di ALTER DATABASE SCOPED CONFIGURATION

### <a name="a-grant-permission"></a>R. Concessione dell'autorizzazione

In questo esempio viene concessa all'utente Joe l'autorizzazione necessaria per eseguire ALTER DATABASE SCOPED CONFIGURATION.

```sql
GRANT ALTER ANY DATABASE SCOPED CONFIGURATION to [Joe] ;
```

### <a name="b-set-maxdop"></a>B. Impostare MAXDOP

In questo esempio viene impostato il parametro MAXDOP = 1 per un database primario e MAXDOP = 4 per un database secondario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 1 ;
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = 4 ;
```

In questo esempio viene impostato il parametro MAXDOP per un database secondario in modo che corrisponda a quello impostato per il database primario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY ;
```

### <a name="c-set-legacy_cardinality_estimation"></a>C. Impostare LEGACY_CARDINALITY_ESTIMATION

In questo esempio il parametro LEGACY_CARDINALITY_ESTIMATION viene impostato su ON per un database secondario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = ON ;
```

In questo esempio viene impostato il parametro LEGACY_CARDINALITY_ESTIMATION per un database secondario come nel database primario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = PRIMARY ;
```

### <a name="d-set-parameter_sniffing"></a>D. Impostare PARAMETER_SNIFFING

In questo esempio il parametro PARAMETER_SNIFFING viene impostato su OFF per un database primario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SNIFFING = OFF ;
```

In questo esempio il parametro PARAMETER_SNIFFING viene impostato su OFF per un database secondario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = OFF ;
```

In questo esempio viene impostato il parametro PARAMETER_SNIFFING per un database secondario come nel database primario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = PRIMARY ;
```

### <a name="e-set-query_optimizer_hotfixes"></a>E. Impostare QUERY_OPTIMIZER_HOTFIXES

Impostare il parametro QUERY_OPTIMIZER_HOTFIXES su ON per un database primario in uno scenario di replica geografica.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET QUERY_OPTIMIZER_HOTFIXES = ON ;
```

### <a name="f-clear-procedure-cache"></a>F. Cancellare la cache delle procedure

In questo esempio viene cancellata la cache delle procedure. È possibile solo per un database primario.

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
```

### <a name="g-set-identity_cache"></a>G. Impostare IDENTITY_CACHE

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In questo esempio viene disabilitata la cache Identity.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET IDENTITY_CACHE = OFF ;
```

### <a name="h-set-optimize_for_ad_hoc_workloads"></a>H. Impostare OPTIMIZE_FOR_AD_HOC_WORKLOADS

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In questo esempio viene abilitato uno stub del piano compilato da memorizzare nella cache quando un batch viene compilato per la prima volta.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET OPTIMIZE_FOR_AD_HOC_WORKLOADS = ON;
```

### <a name="i-set-elevate_online"></a>I. Impostare ELEVATE_ONLINE

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In questo esempio viene impostato il parametro ELEVATE_ONLINE su FAIL_UNSUPPORTED.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_ONLINE = FAIL_UNSUPPORTED ;
```

### <a name="j-set-elevate_resumable"></a>J. Impostare ELEVATE_RESUMABLE

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In questo esempio viene impostato il parametro ELEVATE_RESUMABLE su WHEN_SUPPORTED.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_RESUMABLE = WHEN_SUPPORTED ;
```

### <a name="k-clear-a-query-plan-from-the-plan-cache"></a>K. Cancellare un piano di query dalla cache dei piani

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Questo esempio cancella un piano specifico dalla cache delle procedure

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE 0x06000500F443610F003B7CD12C02000001000000000000000000000000000000000000000000000000000000;
```

### <a name="l-set-paused-duration"></a>L. Impostare la durata della sospensione

**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Questo esempio imposta la durata della sospensione di un indice ripristinabile su 60 minuti.

```sql
ALTER DATABASE SCOPED CONFIGURATION
SET PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = 60
```

## <a name="additional-resources"></a>Risorse aggiuntive

### <a name="maxdop-resources"></a>Risorse di MAXDOP

- [Grado di parallelismo](../../relational-databases/query-processing-architecture-guide.md#DOP)
- [Indicazioni e linee guida per l'opzione di configurazione "max degree of parallelism" in SQL Server](https://support.microsoft.com/kb/2806535)

### <a name="legacy_cardinality_estimation-resources"></a>Risorse di LEGACY_CARDINALITY_ESTIMATION

- [Stima della cardinalità (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Ottimizzazione dei piani di query con la funzionalità di stima di cardinalità di SQL Server 2014](/previous-versions/dn673537(v=msdn.10))

### <a name="parameter_sniffing-resources"></a>Risorse di PARAMETER_SNIFFING

- [Analisi dei parametri](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing)
- ["I smell a parameter!"](/archive/blogs/queryoptteam/i-smell-a-parameter) (Uso dei parametri)

### <a name="query_optimizer_hotfixes-resources"></a>Risorse di QUERY_OPTIMIZER_HOTFIXES

- [Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [SQL Server query optimizer hotfix trace flag 4199 servicing model](https://support.microsoft.com/kb/974006)(Modello di manutenzione del flag di traccia 4199 di Query Optimizer in SQL Server)

### <a name="elevate_online-resources"></a>Risorse di ELEVATE_ONLINE

[Linee guida per le operazioni sugli indici online](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

### <a name="elevate_resumable-resources"></a>Risorse di ELEVATE_RESUMABLE

[Linee guida per le operazioni sugli indici online](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

## <a name="more-information"></a>Ulteriori informazioni   
 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)      
 [Indicazioni e linee guida per l'opzione di configurazione "max degree of parallelism" in SQL Server](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)      
 [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)    
 [Viste del catalogo di database e file](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)    
 [Opzioni di configurazione del server](../../database-engine/configure-windows/server-configuration-options-sql-server.md)    
 [Funzionamento delle operazioni sugli indici online](../../relational-databases/indexes/how-online-index-operations-work.md)    
 [Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)    
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)
