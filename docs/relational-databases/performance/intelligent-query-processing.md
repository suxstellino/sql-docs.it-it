---
title: Elaborazione di query intelligenti
description: Funzionalità di elaborazione di query intelligenti e miglioramento delle prestazioni delle query in SQL Server e nel database SQL di Azure.
ms.custom: seo-dt-2019
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 40a2ba2ff9e3955c09a8d8529a67ec3275e17222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338964"
---
# <a name="intelligent-query-processing-in-sql-databases"></a>Elaborazione di query intelligenti nei database SQL

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

La famiglia di funzionalità di elaborazione di query intelligenti include funzionalità ad ampio spettro che migliorano le prestazioni di carichi di lavoro esistenti con un impegno minimo per l'implementazione. 

![Elaborazione di query intelligenti](./media/iqp-feature-family.png)

Guardare questo video di 6 minuti per una panoramica dell'elaborazione di query intelligenti:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Intelligent-Query-processing-in-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


È possibile impostare automaticamente i carichi di lavoro come idonei all'elaborazione di query intelligenti abilitando il livello di compatibilità applicabile per il database. Questa opzione è impostabile con [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ad esempio:  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

La tabella seguente illustra nel dettaglio tutte le funzionalità di elaborazione di query intelligenti con i rispettivi requisiti per il livello di compatibilità del database.

| **Funzionalità di elaborazione di query intelligenti** | **Supportata in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]** | **Supportata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** |**Descrizione** |
| ---------------- | ------- | ------- | ---------------- |
| [Join adattivi (modalità batch)](#batch-mode-adaptive-joins) | Sì, nel livello di compatibilità 140| Sì, a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] nel livello di compatibilità 140|I join adattivi selezionano in modo dinamico un tipo di join in fase di esecuzione in base alle righe di input effettive.|
| [Count Distinct approssimato](#approximate-query-processing) | Sì| Sì, a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|Consente di offrire il COUNT DISTINCT approssimativo per gli scenari Big Data con il vantaggio di prestazioni elevate mantenendo basso il footprint di memoria. |
| [Modalità batch per rowstore](#batch-mode-on-rowstore) | Sì, nel livello di compatibilità 150| Sì, a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] nel livello di compatibilità 150|Consente di specificare la modalità batch per i carichi di lavoro del data warehouse relazionale associati alla CPU senza richiedere gli indici columnstore.  | 
| [Esecuzione interleaved](#interleaved-execution-for-mstvfs) | Sì, nel livello di compatibilità 140| Sì, a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] nel livello di compatibilità 140|Consente di usare la cardinalità effettiva della funzione con valori di tabella con istruzioni multiple rilevata nella prima compilazione invece di una stima fissa.|
| [Feedback delle concessioni di memoria (modalità batch)](#batch-mode-memory-grant-feedback) | Sì, nel livello di compatibilità 140| Sì, a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] nel livello di compatibilità 140|Se una query in modalità batch contiene operazioni che eseguono lo spill su disco, aggiungere altra memoria per le esecuzioni consecutive. Se una query comporta uno spreco di oltre il 50% della memoria allocata, ridurre il margine di concessione di memoria per le esecuzioni consecutive.|
| [Feedback delle concessioni di memoria (modalità riga)](#row-mode-memory-grant-feedback) | Sì, nel livello di compatibilità 150| Sì, a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] nel livello di compatibilità 150|Se una query in modalità riga contiene operazioni che eseguono lo spill su disco, aggiungere altra memoria per le esecuzioni consecutive. Se una query comporta uno spreco di oltre il 50% della memoria allocata, ridurre il margine di concessione di memoria per le esecuzioni consecutive.|
| [Inlining di funzioni definite dall'utente scalari](#scalar-udf-inlining) | No | Sì, a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] nel livello di compatibilità 150|Le funzioni definite dall'utente scalari vengono trasformate in espressioni relazionali equivalenti che vengono rese inline nella query chiamante, ottenendo spesso significativi miglioramenti delle prestazioni.|
| [Compilazione posticipata delle variabili di tabella](#table-variable-deferred-compilation) | Sì, nel livello di compatibilità 150| Sì, a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] nel livello di compatibilità 150|Consente di usare la cardinalità effettiva della variabile tabella rilevata nella prima compilazione invece di una stima fissa.|

## <a name="batch-mode-adaptive-joins"></a>Join adattivi in modalità batch
La funzionalità di join adattivo in modalità batch consente di rimandare a **dopo** la scansione del primo input la scelta tra l'[esecuzione di un metodo hash join e l'esecuzione di un metodo join a cicli annidati](../../relational-databases/performance/joins.md), usando un singolo piano memorizzato nella cache. L'operatore Join adattivo definisce una soglia che viene usata per stabilire quando passare a un piano Cicli annidati. Durante l'esecuzione il piano può pertanto passare a una strategia di join più efficace.

Per altre informazioni, incluso come disabilitare i join adattivi senza modificare il livello di compatibilità, vedere [Informazioni sui join adattivi](../../relational-databases/performance/joins.md#adaptive).

## <a name="batch-mode-memory-grant-feedback"></a>Feedback delle concessioni di memoria in modalità batch
Un piano post esecuzione di una query in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] include la memoria minima richiesta per l'esecuzione e la dimensione della concessione di memoria sufficiente a far sì che tutte le righe siano incluse nella memoria. Se le dimensioni della concessione di memoria non vengono impostate correttamente le prestazioni possono risultare ridotte. Le concessioni di dimensioni eccessive causano memoria non usata e riduzione della concorrenza. Le concessioni di memoria di dimensioni insufficienti causano costose distribuzioni su disco. Incentrandosi sui carichi di lavoro ripetuti, il feedback delle concessioni di memoria in modalità batch ricalcola la memoria effettiva necessaria per una query, quindi aggiorna il valore della concessione per il piano nella cache. Quando viene eseguita un'istruzione query identica la query usa le dimensioni della concessione di memoria aggiornate, riducendo il numero eccessivo di concessioni che limita la concorrenza e correggendo il numero insufficiente di concessioni che causa costose distribuzioni su disco.
Il grafico seguente visualizza un esempio dell'uso del feedback delle concessioni di memoria in modalità batch. La durata della prima esecuzione della query è pari a **88 secondi** a causa del numero elevato di distribuzioni:   

```sql
DECLARE @EndTime datetime = '2016-09-22 00:00:00.000';
DECLARE @StartTime datetime = '2016-09-15 00:00:00.000';
SELECT TOP 10 hash_unique_bigint_id
FROM dbo.TelemetryDS
WHERE Timestamp BETWEEN @StartTime and @EndTime
GROUP BY hash_unique_bigint_id
ORDER BY MAX(max_elapsed_time_microsec) DESC;
```

![Numero elevato di distribuzioni](./media/2_AQPGraphHighSpills.png)

Con il feedback delle concessioni di memoria attivato la durata della seconda esecuzione della query si riduce a **1 secondo** (da 88 secondi), le distribuzioni su disco vengono rimosse completamente e la concessione è superiore: 

![Nessuna distribuzione](./media/3_AQPGraphNoSpills.png)

### <a name="memory-grant-feedback-sizing"></a>Dimensionamento tramite il feedback delle concessioni di memoria
Per una condizione di concessione di memoria di dimensioni eccessive, se la memoria concessa supera di oltre due volte la quantità di memoria realmente usata, il feedback delle concessioni di memoria ricalcola la concessione e aggiorna il piano memorizzato nella cache. I piani con concessioni di memoria di dimensioni inferiori a 1 MB non vengono ricalcolati per le eccedenze.
Per una condizione di concessione di memoria di dimensioni insufficienti che genera distribuzioni su disco per gli operatori in modalità batch, il feedback delle concessioni di memoria attiva il ricalcolo della concessione di memoria. Gli eventi di distribuzione vengono segnalati al feedback delle concessioni di memoria e possono essere esposti con l'evento xEvent *spilling_report_to_memory_grant_feedback*. Questo evento restituisce l'ID del nodo dal piano e il volume dei dati distribuiti su disco da tale nodo.

### <a name="memory-grant-feedback-and-parameter-sensitive-scenarios"></a>Feedback delle concessioni di memoria e scenari dipendenti dai parametri
Per risultati ottimali, valori dei parametri diversi possono richiedere piani di query diversi. Le query di questo tipo sono definite "sensibili ai parametri". Per i piani sensibili ai parametri il feedback delle concessioni di memoria si disattiva quando una query registra requisiti di memoria non stabili. Il piano viene disattivato dopo varie ripetizioni dell'esecuzione della query e la disattivazione può essere rilevata monitorando l'evento xEvent *memory_grant_feedback_loop_disabled*. Per altre informazioni sull'analisi e la sensibilità dei parametri, consultare la [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="memory-grant-feedback-caching"></a>Memorizzazione nella cache del feedback delle concessioni di memoria
Il feedback può essere archiviato nel piano memorizzato nella cache per una singola esecuzione. Tuttavia i vantaggi del feedback delle concessioni di memoria appaiono in caso di esecuzioni consecutive dell'istruzione. Questa funzionalità si applica all'esecuzione ripetuta di istruzioni. Il feedback delle concessioni di memoria modifica solo il piano memorizzato nella cache. Attualmente le modifiche non vengono acquisite in Query Store.
Se il piano viene rimosso dalla cache il feedback non viene mantenuto. Il feedback va perduto anche nel caso di un failover. Un'istruzione che usa `OPTION (RECOMPILE)` crea un nuovo piano e non lo memorizza nella cache. Dato che il piano non è memorizzato nella cache il feedback delle concessioni di memoria non viene generato e non viene archiviato per la compilazione e l'esecuzione. Se tuttavia un'istruzione equivalente (con lo stesso hash di query) che **non** ha usato `OPTION (RECOMPILE)` è stata memorizzata nella cache e quindi rieseguita, l'istruzione consecutiva può trarre vantaggio dal feedback delle concessioni di memoria.

### <a name="tracking-memory-grant-feedback-activity"></a>Rilevamento delle attività di feedback delle concessioni di memoria
È possibile tenere traccia di eventi di feedback delle concessioni di memoria usando l'evento xEvent *memory_grant_updated_by_feedback*. Questo evento rileva la cronologia del conteggio di esecuzione corrente, il numero di volte per il quale il piano è stato aggiornato dal feedback delle concessioni di memoria, la concessione di memoria aggiuntiva ideale prima della modifica e la concessione di memoria aggiuntiva ideale dopo che il feedback delle concessioni di memoria ha modificato il piano salvato nella cache.

### <a name="memory-grant-feedback-resource-governor-and-query-hints"></a>Feedback delle concessioni di memoria, Resource Governor e hint per la query
La memoria concessa reale è conforme al limite di memoria per le query determinato da Resource Governor o dall'hint per la query.

### <a name="disabling-batch-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Disabilitazione del feedback delle concessioni di memoria in modalità batch senza modificare il livello di compatibilità
È possibile disabilitare i commenti della concessione di memoria nell'ambito del database o dell'istruzione mantenendo comunque la compatibilità sul livello 140 o superiore. Per disabilitare i commenti della concessione di memoria in modalità batch per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Quando è abilitata, questa impostazione viene visualizzata come abilitata in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).

Per abilitare nuovamente i commenti della concessione di memoria in modalità batch per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

È anche possibile disabilitare i commenti sulla concessione di memoria in modalità batch per una query specifica definendo `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` come [hint per la query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Ad esempio:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK')); 
```

L'hint per la query USE HINT ha la precedenza rispetto una configurazione con ambito database o un'impostazione del flag di traccia.

## <a name="row-mode-memory-grant-feedback"></a>Feedback delle concessioni di memoria in modalità riga

**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Il feedback delle concessioni di memoria in modalità riga estende la funzionalità di feedback delle concessioni di memoria in modalità batch adattando le dimensioni delle concessioni di memoria sia per gli operatori in modalità batch che per quelli in modalità riga.  

Per abilitare i commenti sulla concessione di memoria in modalità riga in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], abilitare il livello di compatibilità del database 150 per il database a cui si è connessi quando si esegue la query.

L'attività di feedback delle concessioni di memoria in modalità riga sarà visibile tramite l'XEvent **memory_grant_updated_by_feedback**. 

A partire dal feedback delle concessioni di memoria in modalità riga, vengono visualizzati due nuovi attributi del piano di query per i piani post-esecuzione effettivi: ***IsMemoryGrantFeedbackAdjusted** _ e _*_LastRequestedMemory_*_, che vengono aggiunti all'elemento XML del piano di query _MemoryGrantInfo *. 

*LastRequestedMemory* mostra la memoria concessa in kilobyte (KB) dall'esecuzione della query precedente. L'attributo *IsMemoryGrantFeedbackAdjusted* consente di controllare lo stato dei commenti sulla concessione di memoria per l'istruzione all'interno di un piano di esecuzione query effettivo. I valori esposti in questo attributo sono i seguenti:

| Valore di IsMemoryGrantFeedbackAdjusted | Descrizione |
|---|---|
| No: First Execution | Il feedback delle concessioni di memoria non regola la memoria per la prima compilazione e l'esecuzione associata.  |
| No: Accurate Grant | In assenza di spill su disco e se l'istruzione usa almeno il 50% della memoria concessa, il feedback delle concessioni di memoria non viene attivato. |
| No: Feedback disabled | Se il feedback delle concessioni di memoria viene attivato continuamente e alterna tra operazioni di aumento della memoria e riduzione della memoria, il feedback delle concessioni di memoria verrà disabilitato per l'istruzione. |
| Yes: Adjusting | Il feedback delle concessioni di memoria è stato applicato e può essere ulteriormente regolato per l'esecuzione successiva. |
| Yes: Stable | Il feedback delle concessioni di memoria è stato applicato e la memoria concessa è ora stabile, ovvero quella concessa per l'esecuzione precedente è uguale a quella concessa per l'esecuzione corrente. |

### <a name="disabling-row-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Disabilitazione del feedback delle concessioni di memoria in modalità riga senza modificare il livello di compatibilità
È possibile disabilitare il feedback delle concessioni di memoria in modalità riga nell'ambito del database o dell'istruzione mantenendo comunque la compatibilità sul livello 150 o superiore. Per disabilitare il feedback delle concessioni di memoria in modalità riga per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Per riabilitare il feedback delle concessioni di memoria in modalità riga per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

È anche possibile disabilitare i commenti sulla concessione di memoria in modalità riga per una query specifica definendo `DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK` come [hint per la query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Ad esempio:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK')); 
```

L'hint per la query USE HINT ha la precedenza rispetto una configurazione con ambito database o un'impostazione del flag di traccia.

## <a name="interleaved-execution-for-mstvfs"></a>Esecuzione interleaved per funzioni con valori di tabella con più istruzioni
Con l'esecuzione interleaved, il conteggio effettivo delle righe viene usato per adottare decisioni più mirate relativamente a un piano di query downstream. Per altre informazioni sulle funzioni con valori di tabella con più istruzioni, vedere [Funzione con valori di tabella](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

L'esecuzione interleaved cambia il limite unidirezionale tra le fasi di ottimizzazione ed esecuzione nel caso di un'esecuzione a query singola e consente l'adattamento dei piani in base alle stime di cardinalità aggiornate. Se durante l'ottimizzazione viene rilevato un candidato per l'esecuzione interleaved, che attualmente corrisponde a una **funzione con valori di tabella con istruzioni multiple (MSTVF, Multi-Statement Table Valued Function)** si sospende l'ottimizzazione, si esegue il sottoalbero appropriato, si acquisiscono stime di cardinalità accurate e quindi si riprende l'ottimizzazione per le operazioni downstream.   

Le funzioni con valori di tabella con istruzioni multiple hanno una stima di cardinalità predefinita pari a 100 a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e pari a 1 nelle versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti. L'esecuzione interleaved riduce i problemi di prestazioni del carico di lavoro dovute alle stime della cardinalità fisse associate alle funzioni con valori di tabella con istruzioni multiple (MSTVF). Per altre informazioni sulle funzioni con valori di tabella con istruzioni multiple (MSTVF), vedere [Creazione di funzioni definite dall'utente (Motore di database)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

L'immagine seguente visualizza un output di [Statistiche query dinamiche](../../relational-databases/performance/live-query-statistics.md), un subset di un piano di esecuzione complessivo che visualizza l'impatto delle stime della cardinalità fisse da funzioni con valori di tabella con istruzioni multiple (MSTVF). È possibile visualizzare il flusso di righe effettivo e le righe stimate. Tre aree del piano sono degne di nota (il flusso va da destra a sinistra):
1. L'analisi di tabella MSTVF include una stima fissa pari a 100 righe. In questo esempio tuttavia il flusso della scansione di tabella MSTVF registra 527.597 righe, come visualizzato in Statistiche query dinamiche nel confronto *527597 di 100* tra valore effettivo e valore stimato. Si tratta di una deviazione notevole rispetto alla stima fissa.
1. Per l'operazione di join a cicli annidati è previsto che il lato esterno del join restituisca solo 100 righe. Dato l'elevato numero di righe di fatto restituite dalla funzione MSTVF, in questo caso può risultare utile la scelta di un altro algoritmo di join.
1. Per l'operazione Hash Match osservare il piccolo simbolo di avviso, che in questo caso indica un evento di distribuzione su disco.

![Flusso di righe effettivo e righe stimate](./media/7_AQPFlowThreeAreas.png)

Confrontare il piano precedente al piano reale generato con l'esecuzione interleaved attivata:

![Piano interleaved](./media/8_AQPInterleavedEnabledPlan.png)

1. Si noti che la scansione di tabella MSTVF ora presenta una stima di cardinalità accurata. Si noti anche il riordino della scansione di tabella e delle altre operazioni.
1. Quanto agli algoritmi di join l'operazione di join a cicli annidati è stata sostituita da un'operazione Hash Match, più indicata con un numero di righe molto elevato.
1. Si noti anche che gli avvisi di distribuzione su disco non sono più presenti, in quanto viene allocata una maggior quantità di memoria sulla base del conteggio reale delle righe del flusso della scansione di tabella MSTVF.

### <a name="interleaved-execution-eligible-statements"></a>Istruzioni idonee per l'esecuzione interleaved
Attualmente le funzioni MSTVF che fanno riferimento a istruzioni nell'esecuzione interleaved devono essere di sola lettura e non far parte di un'operazione di modifica dei dati. Le funzioni MSTV, poi, sono idonee per l'esecuzione interleaved solo se non usano costanti di runtime.

### <a name="interleaved-execution-benefits"></a>Vantaggi dell'esecuzione interleaved
In generale, maggiore è lo scarto tra il numero di righe stimato e il numero reale (associato al numero di operazioni del piano downstream), maggiore è l'impatto sulle prestazioni.
L'esecuzione interleaved può risultare vantaggiosa nelle query in cui:
1. Si registra uno scarto notevole tra il numero di righe stimato e il numero reale nel set di risultati intermedio (in questo caso la funzione MSTVF).
1. La query nel suo complesso è sensibile alla variazione delle dimensioni del risultato intermedio. Ciò accade di solito quando nel piano della query è presente un albero complesso sopra il sottoalbero.
Una semplice istruzione `SELECT *` di una funzione MSTVF non trae vantaggio dall'esecuzione interleaved.

### <a name="interleaved-execution-overhead"></a>Sovraccarichi dell'esecuzione interleaved
Il sovraccarico previsto è minimo o nullo. Le funzioni con valori di tabella con istruzioni multiple (MSTVF) venivano già materializzate prima dell'introduzione dell'esecuzione interleaved, ma ora grazie all'abilitazione dell'ottimizzazione differita tali funzioni sfruttano la stima della cardinalità del set di righe materializzate.
È possibile che in seguito alle modifiche alcuni piani registrino un miglioramento della cardinalità per il sottoalbero ma una riduzione dell'efficienza per la query nel suo complesso. La prevenzione può includere il ripristino del livello di compatibilità o l'uso dell'Archivio query per imporre la versione non regredita del piano.

### <a name="interleaved-execution-and-consecutive-executions"></a>Esecuzione interleaved ed esecuzioni consecutive
Dopo che un piano di esecuzione interleaved viene memorizzato nella cache, il piano con le stime aggiornate alla prima esecuzione viene usato per le esecuzioni consecutive e non viene creata di nuovo l'istanza di esecuzione interleaved.

### <a name="tracking-interleaved-execution-activity"></a>Rilevamento delle attività di esecuzione interleaved
È possibile visualizzare gli attributi d'uso nel piano di esecuzione query:

| Attributo del piano di esecuzione | Descrizione |
| --- | --- |
| ContainsInterleavedExecutionCandidates | Si applica al nodo *QueryPlan*. Quando è *true*, il piano contiene candidati per l'esecuzione interleaved. |
| IsInterleavedExecuted | Attributo dell'elemento *RuntimeInformation* in RelOp per il nodo TVF. Quando è *true*, l'operazione è stata materializzata come parte di un'operazione di esecuzione interleaved. |

È anche possibile rilevare le occorrenze di esecuzione interleaved con i seguenti eventi xEvent:

| xEvent | Descrizione |
| ---- | --- |
| interleaved_exec_status | Questo evento viene generato quando si verifica l'esecuzione interleaved. |
| interleaved_exec_stats_update | Questo evento descrive le stime della cardinalità aggiornate dall'esecuzione interleaved. |
| Interleaved_exec_disabled_reason | Questo evento viene generato quando in una query con un possibile candidato per l'esecuzione interleaved non viene applicata tale modalità di esecuzione. |

Per consentire all'esecuzione interleaved di rivedere le stime della cardinalità MSTVF è necessario eseguire la query. Tuttavia il piano di esecuzione stimato viene ancora visualizzato quando sono presenti candidati per l'esecuzione interleaved tramite l'attributo showplan `ContainsInterleavedExecutionCandidates`.    

### <a name="interleaved-execution-caching"></a>Memorizzazione nella cache dell'esecuzione interleaved
Se un piano è viene cancellato o espulso dalla cache, durante l'esecuzione della query l'esecuzione interleave viene usata da una nuova compilazione.
Un'istruzione che usa `OPTION (RECOMPILE)` crea un nuovo piano usando l'esecuzione interleaved e non lo memorizza nella cache.     

### <a name="interleaved-execution-and-query-store-interoperability"></a>Interoperabilità tra esecuzione interleaved e Archivio query
È possibile forzare i piani che usano l'esecuzione interleaved. Il piano è la versione che presenta stime della cardinalità corrette sulla base dell'esecuzione iniziale.    

### <a name="disabling-interleaved-execution-without-changing-the-compatibility-level"></a>Disabilitazione dell'esecuzione interleaved senza modificare il livello di compatibilità
È possibile disabilitare l'esecuzione interleaved nell'ambito del database o dell'istruzione mantenendo comunque la compatibilità sul livello 140 o superiore.  Per disabilitare l'esecuzione interleaved per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = OFF;
```

Quando è abilitata, questa impostazione viene visualizzata come abilitata in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).
Per abilitare nuovamente l'esecuzione interleaved per tutte le esecuzioni di query provenienti dal database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = OFF;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = ON;
```

È anche possibile disabilitare l'esecuzione interleaved per una query specifica definendo `DISABLE_INTERLEAVED_EXECUTION_TVF` come [hint per la query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Ad esempio:

```sql
SELECT [fo].[Order Key], [fo].[Quantity], [foo].[OutlierEventQuantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Fact].[WhatIfOutlierEventQuantity]('Mild Recession',
                            '1-01-2013',
                            '10-15-2014') AS [foo] ON [fo].[Order Key] = [foo].[Order Key]
                            AND [fo].[City Key] = [foo].[City Key]
                            AND [fo].[Customer Key] = [foo].[Customer Key]
                            AND [fo].[Stock Item Key] = [foo].[Stock Item Key]
                            AND [fo].[Order Date Key] = [foo].[Order Date Key]
                            AND [fo].[Picked Date Key] = [foo].[Picked Date Key]
                            AND [fo].[Salesperson Key] = [foo].[Salesperson Key]
                            AND [fo].[Picker Key] = [foo].[Picker Key]
OPTION (USE HINT('DISABLE_INTERLEAVED_EXECUTION_TVF'));
```

L'hint per la query USE HINT ha la precedenza rispetto una configurazione con ambito database o un'impostazione del flag di traccia.

## <a name="table-variable-deferred-compilation"></a>Compilazione posticipata delle variabili di tabella

**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

**La compilazione posticipata delle variabili di tabella** migliora la qualità del piano e le prestazioni generali per le query che fanno riferimento a variabili di tabella. Durante l'ottimizzazione e la compilazione iniziale del piano, questa funzionalità propagherà le stime della cardinalità basate sui conteggi effettivi delle righe di variabili di tabella. Queste informazioni esatte sui conteggi delle righe verranno quindi usate per ottimizzare le operazioni del piano a valle.

Con la compilazione posticipata delle variabili di tabella, la compilazione di un'istruzione che fa riferimento a una variabile di tabella viene posticipata fino alla prima esecuzione effettiva dell'istruzione. Questo comportamento della compilazione posticipata è identico a quello delle tabelle temporanee. Questo cambiamento determina l'uso della cardinalità effettiva invece dell'ipotesi originale di una sola riga. 

Per abilitare la compilazione posticipata delle variabili di tabella, abilitare il livello di compatibilità database 150 per il database a cui si è connessi quando si esegue la query.

La compilazione posticipata delle variabili di tabella **non** modifica altre caratteristiche delle variabili di tabella. Ad esempio, questa funzionalità non aggiunge statistiche di colonna alle variabili di tabella.

La compilazione posticipata delle variabili di tabella **non aumenta la frequenza di ricompilazione**. Piuttosto, sposta la posizione di esecuzione della compilazione iniziale. Il piano memorizzato nella cache risultante viene generato in base al conteggio delle righe delle variabili di tabella della compilazione posticipata iniziale. Il piano memorizzato nella cache viene riusato da query consecutive, fino a quando non viene rimosso o ricompilato. 

Il conteggio delle righe delle variabili di tabella usato per la compilazione del piano iniziale rappresenta un valore tipico e potrebbe essere diverso da un'ipotesi di conteggio di righe fisso. Se è diverso, è un vantaggio per le operazioni a valle. Se il conteggio delle righe delle variabili di tabella varia notevolmente tra le esecuzioni, questa funzionalità potrebbe non migliorare le prestazioni.

### <a name="disabling-table-variable-deferred-compilation-without-changing-the-compatibility-level"></a>Disabilitazione della compilazione posticipata delle variabili di tabella senza modificare il livello di compatibilità
Disabilitare la compilazione posticipata delle variabili di tabella nell'ambito del database o dell'istruzione mantenendo comunque un livello di compatibilità del database 150 o superiore. Per disabilitare la compilazione posticipata delle variabili di tabella per tutte le esecuzioni di query originate dal database, eseguire questo esempio nel contesto del database applicabile:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = OFF;
```

Per riabilitare la compilazione posticipata delle variabili di tabella per tutte le esecuzioni di query originate dal database, eseguire questo esempio nel contesto del database applicabile:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = ON;
```

È anche possibile disabilitare la compilazione posticipata delle variabili di tabella per una query specifica assegnando DISABLE_DEFERRED_COMPILATION_TV come hint per la query USE HINT.  Ad esempio:

```sql
DECLARE @LINEITEMS TABLE 
    (L_OrderKey INT NOT NULL,
     L_Quantity INT NOT NULL
    );

INSERT @LINEITEMS
SELECT L_OrderKey, L_Quantity
FROM dbo.lineitem
WHERE L_Quantity = 5;

SELECT O_OrderKey,
    O_CustKey,
    O_OrderStatus,
    L_QUANTITY
FROM    
    ORDERS,
    @LINEITEMS
WHERE    O_ORDERKEY    =    L_ORDERKEY
    AND O_OrderStatus = 'O'
OPTION (USE HINT('DISABLE_DEFERRED_COMPILATION_TV'));
```

## <a name="scalar-udf-inlining"></a>Inlining di funzioni definite dall'utente scalari

**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

L'inlining di funzioni definite dall'utente scalari trasforma automaticamente le [funzioni definite dall'utente scalari](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#Scalar) in espressioni relazionali e le incorpora nella query SQL chiamante. Questa trasformazione migliora le prestazioni dei carichi di lavoro che sfruttano le funzioni definite dall'utente scalari. L'inlining di funzioni definite dall'utente scalari facilita l'ottimizzazione basata sui costi delle operazioni all'interno delle funzioni definite dall'utente. I risultati sono efficienti, orientati ai set e paralleli, al contrario dei piani di esecuzione seriale iterativi e poco efficienti. Questa funzionalità è abilitata per impostazione predefinita nel livello di compatibilità del database 150.

Per altre informazioni, vedere [Inlining di funzioni definite dall'utente scalari](../user-defined-functions/scalar-udf-inlining.md).

## <a name="approximate-query-processing"></a>Elaborazione delle query approssimativa

**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

L'elaborazione delle query approssimativa è una nuova famiglia di funzionalità, che aggrega set di dati di grandi dimensioni in cui la velocità di risposta è più importante della precisione assoluta. Un esempio è il calcolo di **COUNT(DISTINCT())** su 10 miliardi di righe, per la visualizzazione in un dashboard. In questo caso, la precisione assoluta non è importante, ma la velocità di risposta è fondamentale. La nuova funzione di aggregazione **APPROX_COUNT_DISTINCT** restituisce il numero approssimativo di valori univoci non Null in un gruppo.

Per altre informazioni, vedere [APPROX_COUNT_DISTINCT (Transact-SQL)](../../t-sql/functions/approx-count-distinct-transact-sql.md).

## <a name="batch-mode-on-rowstore"></a>Modalità batch per rowstore 

**Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La modalità batch per rowstore abilita l'esecuzione in modalità batch per i carichi di lavoro analitici senza richiedere indici columnstore.  Questa funzionalità supporta l'esecuzione in modalità batch e i filtri bitmap per gli heap su disco e gli indici con albero B. La modalità batch per rowstore abilita il supporto per tutti gli operatori esistenti abilitati alla modalità batch.

### <a name="background"></a>Background
[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ha introdotto una nuova funzionalità per accelerare i carichi di lavoro analitici: gli indici columnstore. In ogni versione successiva sono stati estesi i casi d'uso e migliorate le prestazioni degli indici columnstore. Fino ad ora, tutte queste capacità sono state esposte e documentate come una singola funzionalità. Si creano gli indici columnstore nelle tabelle e il carico di lavoro di analisi diventa più veloce. Esistono tuttavia due set di tecnologie correlate ma distinte:
- Con gli indici **columnstore**, le query analitiche accedono solo ai dati nelle colonne necessarie. La compressione di pagina nel formato columnstore è anche più efficace rispetto alla compressione di pagina negli indici **rowstore** tradizionali. 
- Con l'elaborazione in **modalità batch**, gli operatori di query elaborano i dati in modo più efficiente, perché agiscono su un batch di righe invece che su una riga per volta. All'elaborazione in modalità batch sono associati numerosi altri miglioramenti per la scalabilità. Per altre informazioni sulla modalità batch, vedere [Modalità di esecuzione](../../relational-databases/query-processing-architecture-guide.md#execution-modes).

I due set di funzionalità interagiscono per migliorare input/output (I/O) e uso della CPU:
- Usando gli indice columnstore, viene salvata in memoria una quantità maggiore di dati. Questo riduce il carico di lavoro di I/O.
- L'elaborazione in modalità batch usa in modo più efficiente la CPU.

Le due tecnologie sfruttano i vantaggi reciproci laddove possibile. Ad esempio, le aggregazioni in modalità batch possono essere valutate come parte di un'analisi dell'indice columnstore. Anche i dati columnstore compressi con la codifica RLE vengono elaborati in modo molto più efficiente con i join e le aggregazioni in modalità batch. 
 
È tuttavia importante tenere presente che le due funzionalità sono indipendenti:
* È possibile ottenere piani in modalità riga che usano indici columnstore.
* È possibile ottenere piani in modalità batch che usano solo indici rowstore. 

Usando le due funzionalità insieme, si ottengono in genere i risultati migliori. Fino ad ora quindi, SQL Server Query Optimizer ha considerato l'elaborazione in modalità batch solo per le query che coinvolgono almeno una tabella con un indice columnstore.

Gli indici columnstore potrebbero non essere appropriati per alcune applicazioni. Un'applicazione potrebbe usare altre funzionalità non supportate con gli indici columnstore. Le modifiche sul posto, ad esempio, non sono compatibili con la compressione columnstore, quindi i trigger non sono supportati nelle tabelle con indici columnstore in cluster, ma soprattutto gli indici columnstore aggiungono un overhead per le istruzioni **DELETE** e **UPDATE**. 

Per alcuni carichi di lavoro ibridi analitico-transazionali l'overhead di un carico di lavoro transazionale è maggiore dei vantaggi offerti dagli indici columnstore. Questi scenari possono trarre vantaggio dall'uso ottimale della CPU tramite l'elaborazione solo in modalità batch. Questo è il motivo per cui la modalità batch per rowstore considera la modalità batch per tutte le query, indipendentemente dal tipo di indici interessati.

### <a name="workloads-that-might-benefit-from-batch-mode-on-rowstore"></a>Carichi di lavoro che possono trarre vantaggio dalla modalità batch per rowstore
I carichi di lavoro seguenti possono trarre vantaggio dalla modalità batch per rowstore:
* Una parte significativa del carico di lavoro è costituita da query analitiche. In genere queste query usano operatori come join o aggregazioni che elaborano centinaia di migliaia di righe o più.
* Il carico di lavoro è basato sulla CPU. Se il collo di bottiglia sono le operazioni di I/O, è comunque consigliabile prendere in considerazione un indice columnstore, se possibile.
* La creazione di un indice columnstore aggiunge un overhead eccessivo alla parte transazionale del carico di lavoro oppure la creazione di un indice columnstore non è implementabile perché l'applicazione dipende da una funzionalità non ancora supportata con gli indici columnstore.


> [!NOTE]
> La modalità batch per rowstore consente solo di ridurre l'utilizzo della CPU. Se il collo di bottiglia è associato alle operazioni di I/O e i dati non sono già memorizzati nella cache (cache "a freddo"), la modalità batch per rowstore non ridurrà il tempo trascorso per le query. Analogamente, se la memoria del computer non è sufficiente per memorizzare nella cache tutti i dati, un miglioramento delle prestazioni è improbabile.

### <a name="what-changes-with-batch-mode-on-rowstore"></a>Che cosa cambia con la modalità batch per rowstore

Impostare il livello di compatibilità 150 per il database. Non sono necessarie altre modifiche.

Anche se una query non accede a nessuna tabella con indici columnstore, Query Processor ora usa l'euristica per decidere se prendere in considerazione la modalità batch. L'euristica è costituita da questi controlli:
1. Un controllo iniziale delle dimensioni delle tabelle, degli operatori usati e delle cardinalità stimate nella query di input.
2. Checkpoint aggiuntivi, man mano che Query Optimizer individua piani nuovi e più economici per la query. Se questi piani alternativi non usano in modo significativo la modalità batch, Query Optimizer smette di esplorare le alternative in modalità batch.

Se la modalità batch per rowstore viene usata, la modalità di esecuzione effettiva visualizzata nel piano di query è la **modalità batch**. L'operatore di analisi usa la modalità batch per gli heap su disco e gli indici albero B. Questa analisi in modalità batch può valutare i filtri bitmap in modalità batch. Nel piano è possibile vedere anche altri operatori della modalità batch. Alcuni esempi sono gli hash join, le aggregazioni basate su hash, gli ordinamenti, le aggregazioni finestra, i filtri, la concatenazione e gli operatori scalari di calcolo.

### <a name="remarks"></a>Osservazioni

I piani di query non usano sempre la modalità batch. Query Optimizer potrebbe decidere che la modalità batch non è utile per la query. 

Lo spazio di ricerca di Query Optimizer cambia. Il piano in modalità riga eventualmente ottenuto potrebbe quindi non essere uguale a quello ottenuto a un livello di compatibilità inferiore e il piano in modalità batch eventualmente ottenuto potrebbe non essere uguale a quello ottenuto con un indice columnstore. 

I piani potrebbero anche cambiare per le query che combinano gli indici columnstore e rowstore a causa della nuova analisi rowstore in modalità batch.

Esistono attualmente alcune limitazioni per la nuova modalità batch per analisi di rowstore: 
- Non verrà applicata per le tabelle OLTP in memoria o per qualsiasi indice diverso da heap su disco e alberi B. 
- Non verrà neanche attivata in caso di recupero o filtro di una colonna LOB (Large Object). Questa limitazione include set di colonne di tipo sparse e colonne XML.

Esistono query per le quali la modalità batch non viene usata neppure con gli indici columnstore. Un esempio sono le query che richiedono cursori. Queste stesse esclusioni vengono estese anche alla modalità batch per rowstore.

### <a name="configure-batch-mode-on-rowstore"></a>Configurare la modalità batch per rowstore
La configurazione con ambito database **BATCH_MODE_ON_ROWSTORE** è attiva per impostazione predefinita. Disabilita la modalità batch per rowstore senza richiedere modifiche nel livello di compatibilità del database:

```sql
-- Disabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = OFF;

-- Enabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = ON;
```

È possibile disabilitare la modalità batch per rowstore tramite la configurazione con ambito database. È tuttavia possibile eseguire l'override dell'impostazione a livello di query usando l'hint per la query **ALLOW_BATCH_MODE**. L'esempio seguente abilita la modalità batch per rowstore anche con la funzionalità disabilitata tramite la configurazione con ambito database:

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('ALLOW_BATCH_MODE'));
```

È anche possibile disabilitare la modalità batch per rowstore per una query specifica usando l'hint per la query **DISALLOW_BATCH_MODE**. Vedere l'esempio seguente:

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('DISALLOW_BATCH_MODE'));
```

## <a name="see-also"></a>Vedere anche
[Centro prestazioni per il motore di database di SQL Server e il database SQL di Azure](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md)    
[Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
[Join](../../relational-databases/performance/joins.md)       
[Dimostrazione dell'elaborazione di query intelligenti](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/intelligent-query-processing)   
