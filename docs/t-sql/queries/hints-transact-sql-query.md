---
description: Hint (Transact-SQL) - Query
title: Hint per la query (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Query_Hint_TSQL
- Query_TSQL
- Query
- Query Hint
- MAX_GRANT_PERCENT
- MAX_GRANT_PERCENT_TSQL
- MIN_GRANT_PERCENT
- MIN_GRANT_PERCENT_TSQL
- EXTERNALPUSHDOWN
- EXTERNALPUSHDOWN_TSQL
- NOLOCK_TSQL
- MAXDOP_TSQL
- USE_HINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REPORT PLAN query hint
- FORCE ORDER query hint
- HASH JOIN query hint
- query hints [SQL Server]
- OPTIMIZE FOR query hint
- FORCESCAN query hint
- RECOMPILE query hint
- MAXRECURSION query hint
- MERGE JOIN query hint
- HASH GROUP query hint
- KEEP PLAN query hint
- UNION query hint
- FORCESEEK query hint
- ORDER GROUP query hint
- LOOP JOIN query hint
- KEEPFIXED PLAN query hint
- FAST query hint
- MAXDOP query hint
- PARAMETERIZATION query hint
- hints [SQL Server], query
- JOIN query hint
- USE PLAN query hint
- EXPAND VIEWS query hint
- MAX_GRANT_PERCENT query hint
- MIN_GRANT_PERCENT query hint
- EXTERNALPUSHDOWN query hint
- USE HINT query hint
- QUERY_PLAN_PROFILE query hint
ms.assetid: 66fb1520-dcdf-4aab-9ff1-7de8f79e5b2d
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 8f884c24acbb4499cbab2e7837367ec49e0c7cbb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172323"
---
# <a name="hints-transact-sql---query"></a>Hint (Transact-SQL) - Query
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gli hint per le query specificano che gli hint indicati vengono usati nell'ambito di una query. e influiscono su tutti gli operatori dell'istruzione. Se la query principale include l'operatore UNION, la clausola OPTION può essere specificata solo nell'ultima query che prevede un'operazione di tipo UNION. Gli hint per la query vengono specificati come parte della [clausola OPTION](../../t-sql/queries/option-clause-transact-sql.md). L'errore 8622 si verifica se in seguito alla presenza di uno o più hint per la query non viene generato un piano valido da Query Optimizer.  
  
> [!CAUTION]  
> Poiché Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in genere seleziona il piano di esecuzione migliore per una query, è consigliabile usare hint solo se strettamente necessario e sempre da parte di sviluppatori e amministratori esperti di database.  
  
**Si applica a:**  
  
[DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
[INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
[SELECT](../../t-sql/queries/select-transact-sql.md)  
  
[UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
[MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
<query_hint> ::=   
{ { HASH | ORDER } GROUP   
  | { CONCAT | HASH | MERGE } UNION   
  | { LOOP | MERGE | HASH } JOIN   
  | EXPAND VIEWS   
  | FAST <integer_value>   
  | FORCE ORDER   
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX  
  | KEEP PLAN   
  | KEEPFIXED PLAN  
  | MAX_GRANT_PERCENT = <numeric_value>  
  | MIN_GRANT_PERCENT = <numeric_value>  
  | MAXDOP <integer_value>   
  | MAXRECURSION <integer_value>   
  | NO_PERFORMANCE_SPOOL   
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )  
  | OPTIMIZE FOR UNKNOWN  
  | PARAMETERIZATION { SIMPLE | FORCED }   
  | QUERYTRACEON <integer_value>   
  | RECOMPILE  
  | ROBUST PLAN   
  | USE HINT ( <use_hint_name> [ , ...n ] )
  | USE PLAN N'<xml_plan>'  
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [, ]...n ] ] )  
}  
  
<table_hint> ::=  
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ]  
  | FORCESCAN  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT  
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK  
}  

<use_hint_name> ::=
{ 'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'
  | 'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'
  | 'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'
  | 'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_DEFERRED_COMPILATION_TV'
  | 'DISABLE_INTERLEAVED_EXECUTION_TVF'
  | 'DISABLE_OPTIMIZED_NESTED_LOOP'
  | 'DISABLE_OPTIMIZER_ROWGOAL'
  | 'DISABLE_PARAMETER_SNIFFING'
  | 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_TSQL_SCALAR_UDF_INLINING'
  | 'DISALLOW_BATCH_MODE'
  | 'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'
  | 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'
  | 'FORCE_DEFAULT_CARDINALITY_ESTIMATION'
  | 'FORCE_LEGACY_CARDINALITY_ESTIMATION'
  | 'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'
  | 'QUERY_PLAN_PROFILE' 
}
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
{ HASH | ORDER } GROUP  
Specifica che le aggregazioni descritte dalla clausola GROUP BY o DISTINCT della query devono usare operazioni di hashing o di ordinamento.  
  
{ MERGE | HASH | CONCAT } UNION  
Specifica che tutte le operazioni UNION vengono eseguite tramite unione, hashing o concatenazione dei set UNION. Se viene specificato più di un hint UNION, Query Optimizer seleziona la strategia meno onerosa tra gli hint specificati.  
  
{ LOOP | MERGE | HASH } JOIN  
Specifica che tutte le operazioni JOIN vengono eseguite tramite LOOP JOIN, MERGE JOIN o HASH JOIN nell'intera query. Se si specificano più hint di join, Query Optimizer seleziona la strategia di join meno onerosa tra quelle consentite.  
  
Se si specifica un hint di join nella clausola FROM della stessa query per una coppia di tabelle specifica, tale hint ha la precedenza rispetto all'unione in join delle due tabelle. Gli hint per la query devono essere comunque rispettati. L'hint di join per la coppia di tabelle può limitare solo la selezione dei metodi di join consentiti nell'hint per la query. Per altre informazioni, vedere [Hint &#40;Transact-SQL&#41; - Join](../../t-sql/queries/hints-transact-sql-join.md).  
  
EXPAND VIEWS  
Specifica che le viste indicizzate vengono espanse. Specifica anche che Query Optimizer non prenderà in considerazione alcuna vista indicizzata in sostituzione di qualsiasi parte della query. Una vista viene espansa quando il nome della vista viene sostituito dalla definizione della vista nel testo della query.  
  
Con questo hint per la query viene praticamente disabilitato l'utilizzo diretto di viste indicizzate e di relativi indici nel piano di query.  
  
> [!NOTE]
> La vista indicizzata rimane compressa se c'è un riferimento diretto alla vista nella sezione SELECT della query. La vista rimane compressa anche se si specifica WITH (NOEXPAND) o WITH (NOEXPAND, INDEX( _<index\_value>_ [ **,** _...n_ ] ) ). Per altre informazioni sull'hint per la query NOEXPAND, vedere [Utilizzo di NOEXPAND](../../t-sql/queries/hints-transact-sql-table.md#using-noexpand).  
  
L'hint influisce solo sulle viste nella sezione SELECT delle istruzioni, comprese le viste nelle istruzioni INSERT, UPDATE, MERGE e DELETE.  
  
FAST _<integer\_value>_  
Indica che la query è ottimizzata per il recupero rapido del numero di righe specificate da _<integer\_value>_ . Questo risultato è un numero intero non negativo. Dopo la restituzione del numero di righe definito da _<integer\_value>_ , l'esecuzione della query continua e viene generato il set di risultati completo.  
  
FORCE ORDER  
Specifica che l'ordine di join indicato dalla sintassi della query viene conservato durante l'ottimizzazione della query. L'uso di FORCE ORDER non ha alcun effetto sul possibile comportamento di inversione dei ruoli in Query Optimizer.  
  
> [!NOTE]  
> In un'istruzione MERGE, l'accesso alla tabella di origine viene eseguito prima della tabella di destinazione come ordine di join predefinito, a meno che non sia specificata la clausola WHEN SOURCE NOT MATCHED. Specificando FORCE ORDER viene conservato questo comportamento predefinito.  
  
{ FORCE | DISABLE } EXTERNALPUSHDOWN  
Forzare o disabilitare il pushdown del calcolo delle espressioni di qualificazione in Hadoop. Si applica solo alle query con PolyBase. Non esegue il push in Archiviazione di Azure.  

{ FORCE | DISABLE } SCALEOUTEXECUTION Forzare o disabilitare l'esecuzione con ridimensionamento delle query PolyBase che usano le tabelle esterne nei cluster Big Data di SQL Server 2019. Questo hint verrà applicato solo da una query che usa l'istanza master di un cluster Big Data di SQL Server. Il ridimensionamento avverrà in tutto il pool di calcolo del cluster Big Data. 

KEEP PLAN  
Forza in Query Optimizer l'impostazione di una soglia di ricompilazione stimata meno restrittiva per una query. La soglia di ricompilazione stimata comporta l'avvio della ricompilazione automatica della query quando in una tabella è stato apportato il numero stimato di modifiche alle colonne indicizzate mediante l'esecuzione delle istruzioni seguenti:

* UPDATE
* DELETE
* MERGE
* INSERT

Specificando KEEP PLAN è possibile assicurarsi che una query non venga ricompilata troppo frequentemente in caso di più aggiornamenti di una tabella.  
  
KEEPFIXED PLAN  
Impedisce a Query Optimizer di ricompilare una query in seguito a modifiche alle statistiche. Se si specifica KEEPFIXED PLAN, una query viene ricompilata solo se lo schema delle tabelle sottostanti cambia o se si esegue **sp_recompile** in tali tabelle.  
  
IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX       
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  
  
Impedisce alla query di usare un indice columnstore ottimizzato per la memoria non cluster. Se la query contiene l'hint per la query per evitare l'uso dell'indice columnstore e un hint per l'indice per usare un indice columnstore, gli hint sono in conflitto e la query restituisce un errore.  
  
MAX_GRANT_PERCENT = <numeric_value>     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

Dimensioni massime della concessione di memoria in percentuale del limite di memoria configurato. Nella query è garantito il non superamento di questo limite. Il limite effettivo può essere inferiore se l'impostazione di Resource Governor è inferiore al valore specificato da questo hint. I valori validi sono compresi tra 0,0 e 100,0.  
  
MIN_GRANT_PERCENT = <numeric_value>        
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

Dimensioni minime della concessione di memoria in percentuale del limite di memoria configurato. Nella query è garantito il recupero del valore `MAX(required memory, min grant)` poiché è necessaria almeno la memoria richiesta per avviare una query. I valori validi sono compresi tra 0,0 e 100,0.  
 
MAXDOP <integer_value>      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Esegue l'override dell'opzione di configurazione **max degree of parallelism** di **sp_configure**. Esegue l'override anche di Resource Governor per la query che specifica questa opzione. L'hint per la query MAXDOP può superare il valore configurato con sp_configure. Se MAXDOP supera il valore configurato con Resource Governor, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa il valore MAXDOP di Resource Governor descritto in [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md). Quando si usa l'hint per la query MAXDOP sono valide tutte le regole semantiche usate con l'opzione di configurazione **max degree of parallelism**. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!WARNING]     
> Se MAXDOP è impostato su zero, il server sceglie il grado massimo di parallelismo.  
  
MAXRECURSION <integer_value>     
Specifica il numero massimo di ricorsioni consentito per questa query. _number_ è un valore intero non negativo compreso tra 0 e 32,767. Se è specificato 0, non viene applicato alcun limite. Se questa opzione non viene specificata, il limite predefinito per il server è 100.  
  
Se durante l'esecuzione della query viene raggiunto il valore specificato o predefinito per il limite MAXRECURSION, la query termina e viene restituito un errore.  
  
A causa di questo errore, verrà eseguito il rollback di tutti gli effetti dell'istruzione. Se l'istruzione è un'istruzione SELECT, è possibile che vengano restituiti risultati parziali oppure nessun risultato. È possibile che eventuali risultati parziali non includano tutte le righe nei livelli di ricorsione che superano il livello di ricorsione massimo specificato.  
  
Per altre informazioni, vedere [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).     
  
NO_PERFORMANCE_SPOOL    
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
Impedisce l'aggiunta di un operatore spool ai piani di query (ad eccezione dei piani in cui spool è necessario per garantire una semantica di aggiornamento valida). In alcuni scenari l'operatore spool può ridurre le prestazioni. Ad esempio, poiché lo spool usa tempdb potrebbe verificarsi un conflitto per tempdb in presenza di numerose query simultanee in esecuzione con le operazioni di spooling.  
  
OPTIMIZE FOR ( _\@variable\_name_ { UNKNOWN | = <literal_constant> }_ [ **,** ..._n_ ] )     
Imposta Query Optimizer in modo che usi un valore specifico per una variabile locale quando la query viene compilata e ottimizzata. Il valore viene utilizzato solo durante l'ottimizzazione della query e non durante l'esecuzione.  
  
_\@variable\_name_  
Nome di una variabile locale utilizzata in una query alla quale è possibile assegnare un valore da utilizzare con l'hint per la query OPTIMIZE FOR.  
  
_UNKNOWN_  
Specifica che Query Optimizer usa dati statistici anziché il valore iniziale per determinare il valore per una variabile locale durante l'ottimizzazione della query.  
  
_literal\_constant_  
Valore letterale costante a cui assegnare _\@variable\_name_ per l'uso con l'hint per la query OPTIMIZE FOR. _literal\_constant_ viene usato solo durante l'ottimizzazione della query e non come valore _\@variable\_name_ durante l'esecuzione della query. _literal\_constant_ può essere di qualsiasi tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che è possibile esprimere come valore letterale costante. Il tipo di dati di _literal\_constant_ deve supportare la conversione implicita nel tipo di dati a cui _\@variable\_name_ fa riferimento nella query.  
  
OPTIMIZE FOR può neutralizzare il comportamento predefinito di rilevamento dei parametri di Query Optimizer. Usare OPTIMIZE FOR anche quando si creano guide di piano. Per altre informazioni, vedere [Ricompilare una stored procedure](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md).  
  
OPTIMIZE FOR UNKNOWN  
Indica a Query Optimizer di applicare la selettività media del predicato a tutti i valori di colonna, anziché usare il valore del parametro di runtime quando la query viene compilata e ottimizzata.  
  
Se si usano OPTIMIZE FOR @variable_name = _literal\_constant_ e OPTIMIZE FOR UNKNOWN nello stesso hint per la query, Query Optimizer userà il valore _literal\_constant_ indicato per un valore specifico. Query Optimizer userà UNKNOWN per i restanti valori di variabile. I valori vengono utilizzati durante l'ottimizzazione della query e non durante l'esecuzione di questa.  

PARAMETERIZATION { SIMPLE | FORCED }     
Specifica le regole di parametrizzazione applicate da Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alla query durante la compilazione.  
  
> [!IMPORTANT]  
> È possibile specificare l'hint per la query PARAMETERIZATION all'interno di una guida di piano per sostituire l'impostazione corrente dell'opzione SET del database PARAMETERIZATION e non direttamente all'interno di una query.    
> Per altre informazioni, vedere [Specificare il comportamento di parametrizzazione delle query tramite guide di piano](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).
  
SIMPLE indica a Query Optimizer di tentare la parametrizzazione semplice. FORCED indica a Query Optimizer di tentare la parametrizzazione forzata. Per altre informazioni, vedere [Parametrizzazione forzata in Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#ForcedParam), e [Parametrizzazione semplice in Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).  

QUERYTRACEON <integer_value>    
Questa opzione consente di abilitare un flag di traccia che influisce sul piano solo durante la compilazione di una singola query. Analogamente ad altre opzioni a livello di query, è possibile usarla insieme alle guide di piano per trovare il testo corrispondente di una query eseguita da qualsiasi sessione e applicare automaticamente un flag di traccia che influisce sul piano quando la query viene compilata. L'opzione QUERYTRACEON è supportata solo per i flag di traccia di Query Optimizer. Per altre informazioni, vedere [Flag di traccia](../database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). 

> [!NOTE]  
> Questa opzione non restituirà alcun errore o avviso se viene usato un numero di flag di traccia non supportato. Se il flag di traccia specificato non influisce su un piano di esecuzione di query, l'opzione verrà ignorata automaticamente.

> [!NOTE]  
> Per usare più di un flag di traccia in una query, specificare un hint QUERYTRACEON per ogni numero di flag di traccia diverso.

RECOMPILE  
Indica a [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di generare un nuovo piano temporaneo per la query ed eliminare immediatamente tale piano al termine dell'esecuzione della query. Il piano di query generato non sostituisce un piano archiviato nella cache quando la stessa query viene eseguita senza l'hint RECOMPILE. Se RECOMPILE non viene specificato, i piani di query vengono inseriti nella cache e riutilizzati da [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Quando si compilano piani di query, l'hint per la query RECOMPILE usa i valori correnti delle variabili locali incluse nella query. Se la query è contenuta in una stored procedure, vengono usati i valori correnti passati ai parametri.  
  
RECOMPILE rappresenta una valida alternativa alla creazione di una stored procedure. RECOMPILE usa la clausola WITH RECOMPILE quando è necessario ricompilare solo un subset di query all'interno della stored procedure, invece dell'intera stored procedure. Per altre informazioni, vedere [Ricompilare una stored procedure](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). RECOMPILE risulta utile anche durante la creazione delle guide di piano.  
  
ROBUST PLAN  
Impone in Query Optimizer l'applicazione di un piano che funziona anche con dimensioni di riga massime, eventualmente a scapito delle prestazioni. Quando la query viene elaborata, è possibile che tabelle e operatori intermedi debbano archiviare ed elaborare righe con dimensioni maggiori rispetto a qualsiasi riga di input. Talvolta le righe possono essere talmente estese che l'operatore specifico non è in grado di elaborarle. In questi casi, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore durante l'esecuzione della query. Usando ROBUST PLAN è possibile indicare a Query Optimizer di ignorare i piani di query in cui potrebbe verificarsi questo problema.  
  
Se non è possibile implementare tale piano, Query Optimizer restituisce un errore invece di posticipare il rilevamento degli errori fino all'esecuzione della query. Le righe possono includere colonne di lunghezza variabile. In [!INCLUDE[ssDE](../../includes/ssde-md.md)] è consentito definire righe con dimensioni massime superiori alla capacità di elaborazione di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. In un'applicazione tuttavia vengono in genere archiviate righe le cui dimensioni effettive rientrano nei limiti della capacità di elaborazione di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Se [!INCLUDE[ssDE](../../includes/ssde-md.md)] rileva una riga di lunghezza eccessiva, viene restituito un errore di esecuzione.  
 
<a name="use_hint"></a> USE HINT ( **'** _hint\_name_ **'** )    
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
 
Specifica uno o più hint aggiuntivi per Query Processor. Gli hint aggiuntivi vengono specificati tramite un nome di hint **racchiuso tra virgolette singole**.   

Sono supportati i nomi di hint seguenti:    
 
*  'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS' <a name="use_hint_join_containment"></a>       
   Fa in modo che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generi un piano di query che usi il presupposto di indipendenza semplice anziché il presupposto predefinito di indipendenza di base per i join, nel modello di [stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md) di Query Optimizer in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o versione successiva. Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9476. 
*  'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES' <a name="use_hint_correlation"></a>      
   Fa in modo che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generi un piano che usa la selettività minima durante la stima dei predicati AND per i filtri per tenere conto della correlazione. Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4137 usato con il modello di stima della cardinalità di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni precedenti e ha un effetto simile al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9471 usato con il modello di stima della cardinalità di [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o versioni successive.
*  'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'       
   Disabilita i join adattivi in modalità batch. Per altre informazioni, vedere [Join adattivi in modalità batch](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins).     
   **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'       
   Disabilita il feedback delle concessioni di memoria in modalità batch. Per altre informazioni, vedere [Feedback delle concessioni di memoria in modalità batch](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-memory-grant-feedback).     
   **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
* 'DISABLE_DEFERRED_COMPILATION_TV'    
  Disabilita la compilazione posticipata delle variabili di tabella. Per altre informazioni, vedere [Compilazione posticipata delle variabili di tabella](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).     
  **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_INTERLEAVED_EXECUTION_TVF'      
   Disabilita l'esecuzione interleaved per funzioni con valori di tabella a più istruzioni. Per altre informazioni, vedere [Esecuzione interleaved per funzioni con valori di tabella a più istruzioni](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).     
   **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_OPTIMIZED_NESTED_LOOP'      
   Indica a Query Processor di non usare un'operazione di ordinamento (ordinamento batch) per i join a cicli annidati ottimizzati durante la generazione di un piano. Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2340.
*  'DISABLE_OPTIMIZER_ROWGOAL' <a name="use_hint_rowgoal"></a>      
   Fa in modo che SQL Server generi un piano che non usa le rettifiche degli obiettivi di riga con query contenenti queste parole chiave: 
   
   * TOP
   * OPTION (FAST N)
   * IN
   * EXISTS
   
   Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4138.
*  'DISABLE_PARAMETER_SNIFFING'      
   Indica a Query Optimizer di usare una distribuzione dei dati media durante la compilazione di una query con uno o più parametri. Questa istruzione rende il piano di query indipendente dal valore del parametro usato inizialmente durante la compilazione della query. Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4136 o all'impostazione [Configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`PARAMETER_SNIFFING = OFF`.
* 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'    
  Disabilita il feedback delle concessioni di memoria in modalità riga. Per altre informazioni, vedere [Feedback delle concessioni di memoria in modalità riga](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).      
  **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
* 'DISABLE_TSQL_SCALAR_UDF_INLINING'    
  Abilita l'inlining di funzioni scalari definite dall'utente. Per altre informazioni, vedere [Inlining di funzioni definite dall'utente scalari](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).     
  **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).    
* 'DISALLOW_BATCH_MODE'    
  Disabilita l'esecuzione in modalità batch. Per altre informazioni, vedere [Modalità di esecuzione](../../relational-databases/query-processing-architecture-guide.md#execution-modes).     
  **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
*  'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'      
   Abilita le statistiche rapide generate automaticamente (modifica istogramma) per qualsiasi colonna di indice iniziale per cui è necessaria la stima della cardinalità. L'istogramma usato per la stima della cardinalità viene modificato durante la compilazione della query per tenere conto del valore massimo o minimo effettivo di questa colonna. Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4139. 
*  'ENABLE_QUERY_OPTIMIZER_HOTFIXES'     
   Abilita gli hotfix di Query Optimizer (modifiche rilasciate negli aggiornamenti cumulativi e nei Service Pack di SQL Server). Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4199 o all'impostazione [Configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`QUERY_OPTIMIZER_HOTFIXES = ON`.
*  'FORCE_DEFAULT_CARDINALITY_ESTIMATION'      
   Forza in Query Optimizer l'utilizzo del modello di [stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md) che corrisponde al livello di compatibilità del database corrente. Usare questo hint per eseguire l'override dell'impostazione [Configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`LEGACY_CARDINALITY_ESTIMATION = ON` o il [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481.
*  'FORCE_LEGACY_CARDINALITY_ESTIMATION' <a name="use_hint_ce70"></a>      
   Forza in Query Optimizer l'uso del modello di [stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md) di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni precedenti. Questo nome di hint equivale al [flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481 o all'impostazione [Configurazione con ambito database](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`LEGACY_CARDINALITY_ESTIMATION = ON`.
*  'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'          
 Forza il comportamento di Query Optimizer a livello di query. Il comportamento rispecchia quello che si verifica se la query viene compilata con il livello di compatibilità del database _n_, dove _n_ è un livello di compatibilità del database supportato (ad esempio 100, 130 e così via). Fare riferimento a [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) per un elenco dei valori attualmente supportati per _n_.      
   **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU10).    

   > [!NOTE]
   > L'hint QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n non esegue l'override dell'impostazione di stima della cardinalità legacy o predefinita, se viene forzato tramite configurazione con ambito database, flag di traccia o un altro hint per la query, ad esempio QUERYTRACEON.   
   > Questo hint influisce solo sul comportamento di Query Optimizer. Non interessa altre funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che possono dipendere dal [livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md), ad esempio la disponibilità di determinate funzionalità di database.  
   > Per altre informazioni su questo hint, vedere [Developer's Choice: Hinting Query Execution model](/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model) (La scelta dello sviluppatore: modello di esecuzione di hint per la query).
    
*  'QUERY_PLAN_PROFILE'      
 Abilita la profilatura leggera per la query. Quando una query contenente questo nuovo hint termina, viene generato un nuovo evento esteso, query_plan_profile. Questo evento esteso espone le statistiche di esecuzione e il codice XML del piano di esecuzione effettivo in modo simile all'evento esteso query_post_execution_showplan, ma solo per le query contenenti il nuovo hint.    
   **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU3 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11). 

   > [!NOTE]
   > Se si abilita la raccolta dell'evento esteso query_post_execution_showplan, verrà aggiunta un'infrastruttura di profilatura standard a ogni query in esecuzione nel server e le prestazioni generali del server potrebbero rallentare.      
   > Se invece si abilita la raccolta dell'evento esteso *query_thread_profile* per usare l'infrastruttura di profilatura leggera, l'overhead delle prestazioni verrà considerevolmente ridotto, ma le prestazioni generali del server verranno comunque rallentate.       
   > Se si abilita l'evento esteso query_plan_profile, l'infrastruttura di profilatura leggera verrà abilitata solo per una query eseguita con QUERY_PLAN_PROFILE e quindi gli altri carichi di lavoro sul server non ne saranno interessati. Usare questo hint per profilare una query specifica senza effetti sulle altre parti del carico di lavoro del server.
   > Per altre informazioni sulla profilatura leggera, vedere [Infrastruttura di profilatura delle query](../../relational-databases/performance/query-profiling-infrastructure.md).
 
È possibile eseguire una query nell'elenco di tutti i nomi USE HINT supportati usando la DMV [sys.dm_exec_valid_use_hints ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md).    

> [!TIP]
> Per i nomi degli hint non viene fatta distinzione tra maiuscole e minuscole.   
  
> [!IMPORTANT] 
> Alcuni hint USE HINT possono essere in conflitto con i flag di traccia abilitati a livello globale o sessione o con le impostazioni di configurazione con ambito database. In questo caso, l'hint del livello di query (USE HINT) ha sempre la precedenza. Se un hint USE HINT è in conflitto con un altro hint per la query o un flag di traccia abilitato a livello di query (ad esempio tramite QUERYTRACEON), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un errore quando si tenta di eseguire la query. 

<a name="use-plan"></a> USE PLAN N' _<xml\_plan>_ '  
 Forza in Query Optimizer l'uso di un piano di query esistente per una query specificata da **'** _xml\_plan_ **'** . USE PLAN non può essere specificato nelle istruzioni INSERT, UPDATE, MERGE o DELETE.  
  
TABLE HINT **(** _<exposed\_object\_name>_ [ **,** \<table_hint> [ [ **,** ]..._n_ ] ] **)** Applica l'hint di tabella specificato alla tabella o alla vista corrispondente a _exposed\_object\_name_. È consigliabile usare un hint di tabella come hint per la query solo nel contesto di una [guida di piano](../../relational-databases/performance/plan-guides.md).  
  
 _<exposed\_object\_name>_ può essere uno dei riferimenti seguenti:  
  
-   Quando viene usato un alias per la tabella o la vista nella clausola [FROM](../../t-sql/queries/from-transact-sql.md) della query, _exposed\_object\_name_ è l'alias.  
  
-   Quando non viene usato un alias, _exposed\_object\_name_ corrisponde esattamente alla tabella o alla vista a cui viene fatto riferimento nella clausola FROM. Se, ad esempio, viene fatto riferimento alla tabella o alla vista usando un nome in due parti, _exposed\_object\_name_ è lo stesso nome in due parti.  
  
 Quando si specifica _exposed\_object\_name_ senza specificare anche un hint di tabella, gli indici specificati nella query come parte di un hint di tabella per l'oggetto non vengono considerati. Query Optimizer determina quindi l'utilizzo degli indici. È possibile usare questa tecnica per eliminare l'effetto di un hint di tabella INDEX quando non è possibile modificare la query originale. Vedere l'esempio J.  
  
**\<table_hint> ::=** { NOEXPAND [ , INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) ] \| INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) \| FORCESEEK [ **(** _<index\_value>_ **(** _<index\_column\_name>_ [ **,** ... ] **))** ] \| FORCESCAN \| HOLDLOCK \| NOLOCK \| NOWAIT \| PAGLOCK \| READCOMMITTED \| READCOMMITTEDLOCK \| READPAST \| READUNCOMMITTED \| REPEATABLEREAD \| ROWLOCK \| SERIALIZABLE \| SNAPSHOT \| SPATIAL_WINDOW_MAX_CELLS = _<integer\_value>_ \| TABLOCK \| TABLOCKX \| UPDLOCK \| XLOCK }    
Hint di tabella da applicare alla tabella o alla vista corrispondente a *exposed_object_name* come hint per la query. Per una descrizione di questi hint, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Gli hint di tabella diversi da INDEX, FORCESCAN e FORCESEEK non sono consentiti come hint per la query, a meno che la query non disponga già di una clausola WITH che specifica l'hint di tabella. Per altre informazioni, vedere la [sezione Osservazioni](#remarks).  
  
> [!CAUTION]
> Se si specifica FORCESEEK con parametri, il numero di piani che possono essere considerati da Query Optimizer viene limitato più di quanto avvenga se si specifica FORCESEEK senza parametri. In questo caso potrebbe venire generato un errore "Impossibile generare il piano" con maggiore frequenza. In una versione futura le modifiche interne a Query Optimizer potrebbero consentire di prendere in considerazione più piani.  
  
## <a name="remarks"></a>Osservazioni  
 Gli hint per la query non possono essere specificati in un'istruzione INSERT, eccetto quando viene usata una clausola SELECT all'interno dell'istruzione.  
  
 È possibile specificare gli hint per la query solo nella query di livello principale e non nelle sottoquery. Quando un hint di tabella viene specificato come hint per la query, l'hint può essere specificato nella query di livello superiore o in una sottoquery. Tuttavia, il valore specificato per _<exposed\_object\_name>_ nella clausola TABLE HINT deve corrispondere esattamente al nome esposto nella query o nella sottoquery.  
  
## <a name="specifying-table-hints-as-query-hints"></a>Specifica di hint di tabella come hint per la query  
 È consigliabile usare l'hint di tabella INDEX, FORCESCAN o FORCESEEK come hint per la query solo nel contesto di una [guida di piano](../../relational-databases/performance/plan-guides.md). Le guide di piano sono utili quando non è possibile modificare la query originale, ad esempio perché si tratta di un'applicazione di terze parti. L'hint per la query specificato nella guida di piano viene aggiunto alla query prima della compilazione e dell'ottimizzazione. Per le query ad hoc, utilizzare la clausola TABLE HINT solo quando si testano istruzioni della guida di piano. Per tutte le altre query ad hoc, è consigliabile specificare tali hint solo come hint di tabella.  
  
 Se specificati come hint per la query, gli hint di tabella INDEX, FORCESCAN e FORCESEEK sono validi per gli oggetti seguenti:  
  
-   Tabelle  
-   Viste  
-   Viste indicizzate  
-   Espressioni di tabella comuni (l'hint deve essere specificato nell'istruzione SELECT il cui set di risultati popola l'espressione di tabella comune)  
-   Viste a gestione dinamica (DMV)
-   Sottoquery denominate  
  
È possibile specificare gli hint di tabella INDEX, FORCESCAN e FORCESEEK come hint per la query per una query che non ha hint di tabella esistenti. È anche possibile usare tali hint per sostituire rispettivamente gli hint INDEX, FORCESCAN o FORCESEEK esistenti nella query. 

Gli hint di tabella diversi da INDEX, FORCESCAN e FORCESEEK non sono consentiti come hint per la query, a meno che la query non disponga già di una clausola WITH che specifica l'hint di tabella. In questo caso, è necessario specificare anche un hint corrispondente come hint per la query. Per specificare l'hint corrispondente come hint per la query, usare TABLE HINT nella clausola OPTION. In questo modo, è possibile mantenere la semantica della query. Ad esempio, se la query contiene l'hint di tabella NOLOCK, la clausola OPTION nel parametro **\@hints** della guida del piano deve contenere anch'essa l'hint NOLOCK. Vedere l'esempio K. 
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-merge-join"></a>R. Utilizzo di MERGE JOIN  
 L'esempio seguente specifica che MERGE JOIN esegue l'operazione JOIN nella query. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *   
FROM Sales.Customer AS c  
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID  
WHERE TerritoryID = 5  
OPTION (MERGE JOIN);  
GO    
```  
  
### <a name="b-using-optimize-for"></a>B. Utilizzo di OPTIMIZE FOR  
 Nell'esempio seguente si indica a Query Optimizer di usare il valore `'Seattle'` per `@city_name` e di applicare la selettività media del predicato a tutti i valori di colonna per `@postal_code` durante l'ottimizzazione della query. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),  
 @postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address  
WHERE City = @city_name AND PostalCode = @postal_code  
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );  
GO
```  
  
### <a name="c-using-maxrecursion"></a>C. Utilizzo di MAXRECURSION  
 È possibile utilizzare MAXRECURSION per evitare che un'espressione di tabella comune (CTE) ricorsiva non corretta provochi un ciclo infinito. L'esempio seguente crea intenzionalmente un ciclo infinito e usa l'hint MAXRECURSION per limitare a due il numero di livelli di ricorsione. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
--Creates an infinite loop  
WITH cte (CustomerID, PersonID, StoreID) AS  
(  
    SELECT CustomerID, PersonID, StoreID  
    FROM Sales.Customer  
    WHERE PersonID IS NOT NULL  
  UNION ALL  
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID  
    FROM cte   
    JOIN  Sales.Customer AS e   
        ON cte.PersonID = e.CustomerID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT CustomerID, PersonID, StoreID  
FROM cte  
OPTION (MAXRECURSION 2);  
GO  
```  
  
 Dopo la correzione dell'errore del codice, MAXRECURSION non è più necessario.  
  
### <a name="d-using-merge-union"></a>D. Utilizzo di MERGE UNION  
 L'esempio seguente usa l'hint per la query MERGE UNION. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *  
FROM HumanResources.Employee AS e1  
UNION  
SELECT *  
FROM HumanResources.Employee AS e2  
OPTION (MERGE UNION);  
GO  
```  
  
### <a name="e-using-hash-group-and-fast"></a>E. Utilizzo di HASH GROUP e FAST  
 L'esempio seguente usa gli hint per la query HASH GROUP e FAST. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO    
```  
  
### <a name="f-using-maxdop"></a>F. Utilizzo di MAXDOP  
 L'esempio seguente usa l'hint per la query MAXDOP. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```  
  
### <a name="g-using-index"></a>G. Utilizzo di INDEX  
 Nell'esempio seguente viene utilizzato l'hint INDEX. Nel primo esempio viene specificato un singolo indice. Nel secondo esempio vengono specificati più indici per un singolo riferimento alla tabella. In entrambi gli esempi, dal momento che l'hint INDEX viene applicato a una tabella che usa un alias, anche la clausola TABLE HINT deve specificare lo stesso alias del nome dell'oggetto esposto. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';  
GO  
EXEC sp_create_plan_guide   
    @name = N'Guide2',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';  
GO    
```  
  
### <a name="h-using-forceseek"></a>H. Utilizzo di FORCESEEK  
 Nell'esempio seguente viene utilizzato l'hint di tabella FORCESEEK. Anche la clausola TABLE HINT deve specificare lo stesso nome in due parti del nome dell'oggetto esposto. Specificare il nome quando si applica l'hint INDEX a una tabella che usa un nome in due parti. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide3',   
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title  
              FROM HumanResources.Employee  
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID  
              WHERE HumanResources.Employee.ManagerID = 3  
              ORDER BY c.LastName, c.FirstName;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';  
GO    
```  
  
### <a name="i-using-multiple-table-hints"></a>I. Utilizzo di più hint di tabella  
 Nell'esempio seguente vengono applicati l'hint INDEX a una tabella e l'hint FORCESEEK a un'altra. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide4',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))   
                       , TABLE HINT (c, FORCESEEK))';  
GO  
```  
  
### <a name="j-using-table-hint-to-override-an-existing-table-hint"></a>J. Utilizzo di TABLE HINT per eseguire l'override di un hint di tabella esistente  
L'esempio seguente illustra come usare l'hint TABLE HINT. È possibile usare l'hint senza specificare un hint per eseguire l'override del comportamento dell'hint di tabella INDEX specificato nella clausola FROM della query. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide5',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e))';  
GO    
```  
  
### <a name="k-specifying-semantics-affecting-table-hints"></a>K. Specifica di hint di tabella che influiscono sulla semantica  
L'esempio seguente contiene due hint di tabella nella query: NOLOCK, che influisce sulla semantica, e INDEX, che non influisce sulla semantica. Per mantenere la semantica della query l’hint NOLOCK viene specificato nella clausola OPTIONS della guida di piano. Oltre all'hint NOLOCK, specificare gli hint INDEX e FORCESEEK e sostituire l'hint INDEX che non influisce sulla semantica nella query quando l'istruzione viene compilata e ottimizzata. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide6',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';  
GO    
```  
  
Nell'esempio seguente viene illustrato un metodo alternativo per mantenere la semantica della query e consentire a Query Optimizer di scegliere un indice diverso da quello specificato nell'hint di tabella. Per consentire a Query Optimizer di scegliere, specificare l'hint NOLOCK nella clausola OPTIONS. Si specifica l'hint perché influisce sulla semantica. Specificare quindi la parola chiave TABLE HINT con solo un riferimento a tabella e senza hint INDEX. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide7',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';  
GO  
```  

### <a name="l-using-use-hint"></a>L. Uso di USE HINT  
 L'esempio seguente usa gli hint per la query RECOMPILE e USE HINT. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING')); 
GO  
```  

### <a name="m-using-querytraceon-hint"></a>M. Uso di QUERYTRACEON HINT  
 Nell'esempio seguente vengono usati gli hint per la query QUERYTRACEON. Nell'esempio viene utilizzato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. È possibile abilitare tutti gli hotfix per i piani controllati dal flag di traccia 4199 per una determinata query usando la query seguente:
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```  

 È anche possibile usare più flag di traccia come nella query seguente:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION  (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

## <a name="see-also"></a>Vedere anche  
[Hint &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
[sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
[sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
[Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)       
[Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)      
