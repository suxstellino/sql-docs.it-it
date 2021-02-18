---
title: Novità di SQL Server 2019 | Microsoft Docs
description: Informazioni sulle nuove funzionalità per SQL Server 2019 (15.x), che consentono di scegliere tra linguaggi di sviluppo, tipi di dati, ambienti e sistemi operativi.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: aadb59645e474557a92e3e87a0cd9c0c50df0b46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271748"
---
# <a name="whats-new-in-sql-server-2019"></a>Novità di [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] si basa sulle versioni precedenti per sviluppare SQL Server come piattaforma che offre una scelta di linguaggi di sviluppo, tipi di dati, sistemi operativi e ambienti di elaborazione locale o nel cloud.

Questo articolo riepiloga le nuove funzionalità e i miglioramenti per [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

Per altre informazioni e problemi noti, vedere le [Note sulla versione di [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](sql-server-version-15-release-notes.md).

Per un'esperienza ottimale con [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], usare gli [strumenti più recenti](../azure-data-studio/download-azure-data-studio.md).

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] per [!INCLUDE[sql-server](../includes/ssnoversion-md.md)]. Offre inoltre funzionalità aggiuntive e miglioramenti per il motore di database SQL Server, SQL Server Analysis Services, SQL Server Machine Learning Services, SQL Server in Linux e SQL Server Master Data Services.

Il video seguente offre un'introduzione di 13 minuti a SQL Server 2019:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


Le sezioni seguenti presentano una panoramica di queste funzionalità.

## <a name="data-virtualization-and-big-data-clusters-2019"></a>Virtualizzazione dei dati e [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]

Oggi le aziende si trovano spesso a gestire enormi raccolte di dati, costituite da una vasta gamma di set di dati in continua crescita, ospitati in origini dati distribuite tra le varie aree aziendali. È possibile ottenere informazioni dettagliate quasi in tempo reale da tutti i dati usando i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], che offrono un ambiente completo per la gestione di grandi set di dati, incluse funzionalità di Machine Learning e intelligenza artificiale.

| Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
| Soluzione Big Data scalabile | [Distribuire cluster scalabili](../big-data-cluster/deploy-get-started.md) di contenitori SQL Server, Spark e HDFS in esecuzione in Kubernetes. <br/><br/> Leggere, scrivere ed elaborare Big Data da Transact-SQL o Spark.<br/><br/> Combinare e analizzare con facilità dati relazionali di alto valore con volumi elevati di Big Data.<br/><br/>Eseguire query su origini dati esterne.<br/><br/>Archiviare Big Data in HDFS gestito da SQL Server.<br/><br/>Eseguire query sui dati da più origini dati esterne tramite il cluster.<br/><br/> Usare i dati per intelligenza artificiale, Machine Learning e altre attività di analisi.<br/><br/> [Distribuire ed eseguire applicazioni](../big-data-cluster/concept-application-deployment.md) in [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]. <br/><br/> L'istanza master di SQL Server offre disponibilità elevata e ripristino di emergenza per tutti i database tramite una tecnologia basata su gruppi di disponibilità Always On.<br/>|
|Virtualizzazione dei dati con PolyBase | È possibile eseguire query sui dati da origini dati esterne SQL Server, Oracle, Teradata, MongoDB e ODBC con tabelle esterne, ora con il [supporto della codifica UTF-8](../relational-databases/collations/collation-and-unicode-support.md). Per altre informazioni, vedere [Che cos'è PolyBase?](../relational-databases/polybase/polybase-guide.md).|
| &nbsp; | &nbsp; |

Per altre informazioni, vedere [Che cosa sono i [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] di SQL Server?](../big-data-cluster/big-data-cluster-overview.md).

## <a name="intelligent-database"></a>Database intelligente
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] si basa sulle innovazioni delle versioni precedenti per offrire prestazioni leader nel settore già in configurazione di base. Dall'[elaborazione intelligente delle query](../relational-databases/performance/intelligent-query-processing.md) al supporto di dispositivi con memoria persistente, le funzionalità del database intelligente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] migliorano le prestazioni e la scalabilità di tutti i carichi di lavoro di database senza apportare modifiche alla progettazione di applicazioni o database.

### <a name="intelligent-query-processing"></a>Elaborazione di query intelligenti
Con l'[elaborazione di query intelligenti](../relational-databases/performance/intelligent-query-processing.md), i carichi di lavoro paralleli critici migliorano quando sono in esecuzione su larga scala. Allo stesso tempo, restano adattabili al mondo dei dati in continua evoluzione. L'elaborazione di query intelligenti è disponibile per impostazione predefinita nell'impostazione del [livello di compatibilità del database](../t-sql/statements/alter-database-transact-sql-compatibility-level.md#differences-between-compatibility-level-140-and-level-150) più recente e include funzionalità ad ampio spettro che migliorano le prestazioni di carichi di lavoro esistenti con il minimo sforzo dal punto di vista dell'implementazione.

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Feedback delle concessioni di memoria in modalità riga |Estende la funzionalità di feedback delle concessioni di memoria in modalità batch adattando le dimensioni delle concessioni di memoria sia per gli operatori in modalità batch sia per quelli in modalità riga. Questo adattamento consente di correggere automaticamente le concessioni eccessive che comportano la perdita di memoria e la riduzione della concorrenza. Permette inoltre di correggere le concessioni di memoria insufficienti che causano costose distribuzioni su disco. Vedere [Feedback delle concessioni di memoria in modalità riga](../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback). |
|Modalità batch per rowstore | Abilita l'esecuzione in modalità batch senza richiedere indici columnstore. L'esecuzione in modalità batch usa la CPU in modo più efficiente nel caso di carichi di lavoro analitici, ma fino a [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] veniva usata solo quando una query includeva operazioni con indici columnstore. Alcune applicazioni, tuttavia, possono usare funzionalità che non sono supportate con gli indici columnstore e pertanto non riescono a sfruttare la modalità batch. A partire da [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], la modalità batch è abilitata nei carichi di lavoro analitici idonei, le cui query includono operazioni con qualsiasi tipo di indice (rowstore o columnstore). Vedere [Modalità batch per rowstore](../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore). |
|Inlining di funzioni definite dall'utente scalari|Trasforma automaticamente le funzioni definite dall'utente scalari in espressioni relazionali e le incorpora nella query SQL chiamante. Questa trasformazione migliora le prestazioni dei carichi di lavoro che sfruttano le funzioni definite dall'utente scalari. Vedere [Inlining di funzioni definite dall'utente scalari](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining).|
|Compilazione posticipata delle variabili di tabella|Migliora la qualità del piano e le prestazioni generali per le query che fanno riferimento a variabili di tabella. Durante l'ottimizzazione e la compilazione iniziale, questa funzionalità propaga le stime della cardinalità basate sui conteggi effettivi delle righe di variabili di tabella. Queste informazioni accurate sui conteggi di righe ottimizzano le operazioni del piano downstream. Vedere [Compilazione posticipata delle variabili di tabella](../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation). |
|Elaborazione delle query approssimativa con `APPROX_COUNT_DISTINCT` |Per gli scenari in cui la precisione assoluta non è importante, ma la velocità di risposta è cruciale, `APPROX_COUNT_DISTINCT` esegue aggregazioni tra set di dati di grandi dimensioni usando meno risorse rispetto a `COUNT(DISTINCT())` per un livello superiore di concorrenza. Vedere [Elaborazione delle query approssimativa](../relational-databases/performance/intelligent-query-processing.md#approximate-query-processing).|
| &nbsp; | &nbsp; |


### <a name="in-memory-database"></a>Database in memoria
Le tecnologie di [database in memoria](../relational-databases/in-memory-database.md) di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usano le ultime innovazioni in campo hardware per offrire prestazioni e scalabilità senza precedenti. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] si basa su innovazioni precedenti in quest'area, ad esempio l'elaborazione delle transazioni online (OLTP) in memoria, per rendere possibile un nuovo livello di scalabilità in tutti i carichi di lavoro del database.  

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Pool di buffer ibrido| Nuova funzionalità del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] che consente di accedere direttamente ai file di database che si trovano in un dispositivo con memoria persistente, quando necessario. Vedere [Pool di buffer ibrido](../database-engine/configure-windows/hybrid-buffer-pool.md).|
|Metadati TempDB ottimizzati per la memoria| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce una nuova funzionalità della famiglia di funzionalità [Database in memoria](../relational-databases/in-memory-database.md), i metadati TempDB ottimizzati per la memoria, che rimuove in modo efficace questo collo di bottiglia e sblocca un nuovo livello di scalabilità per i carichi di lavoro TempDB eccessivi. In [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] le tabelle di sistema coinvolte nella gestione dei metadati delle tabelle temporanee possono essere spostate in tabelle ottimizzate per la memoria non durevoli senza latch. Vedere [Metadati TempDB ottimizzati per la memoria](../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).|
| Supporto di OLTP in memoria per snapshot del database | In [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] è stato introdotto il supporto per la creazione di [snapshot di database](../relational-databases/databases/database-snapshots-sql-server.md) che includono filegroup ottimizzati per la memoria. |
| &nbsp; | &nbsp; |

### <a name="intelligent-performance"></a>Prestazioni intelligenti
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] si basa sulle innovazioni del database intelligente nelle versioni precedenti per garantire [una velocità superiore](/archive/blogs/bobsql/). Questi miglioramenti consentono di superare i colli di bottiglia delle risorse noti e offrono opzioni per configurare il server di database in modo da offrire prestazioni prevedibili in tutti i carichi di lavoro.

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|`OPTIMIZE_FOR_SEQUENTIAL_KEY`|Attiva un'ottimizzazione all'interno del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] che contribuisce a migliorare la velocità effettiva per gli inserimenti nell'indice con un elevato grado di concorrenza. Questa opzione è destinata agli indici che sono soggetti a conflitti di inserimento dell'ultima pagina, che generalmente si verificano con gli indici con una chiave sequenziale, come una colonna Identity, una sequenza o una colonna di data/ora. Vedere [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys).|
|Uso forzato di cursori statici e fast forward | Fornisce il supporto dell'uso forzato del piano di Query Store per cursori statici e fast forward. Vedere [Supporto dell'uso forzato del piano per cursori fast forward e statici](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#ctp23).|
|Governance delle risorse| Il valore configurabile per l'opzione `REQUEST_MAX_MEMORY_GRANT_PERCENT` di `CREATE WORKLOAD GROUP` e `ALTER WORKLOAD GROUP` è stato modificato da dal tipo di dati integer a float per consentire un controllo più granulare dei limiti di memoria. Vedere [ALTER WORKLOAD GROUP](../t-sql/statements/alter-workload-group-transact-sql.md) e [CREATE WORKLOAD GROUP](../t-sql/statements/create-workload-group-transact-sql.md).|
|Riduzione delle ricompilazioni per i carichi di lavoro| Migliora le prestazioni quando si usano tabelle temporanee in più ambiti riducendo le ricompilazioni non necessarie. Vedere [Riduzione delle ricompilazioni per i carichi di lavoro](../relational-databases/tables/tables.md#ctp23). |
|Scalabilità per il checkpoint indiretto |Vedere [Scalabilità migliorata per il checkpoint indiretto](../relational-databases/logs/database-checkpoints-sql-server.md#ctp23).|
|Aggiornamenti simultanei delle pagine PFS|Le [pagine PFS (Page Free Space)](https://techcommunity.microsoft.com/t5/SQL-Server/Under-the-covers-GAM-SGAM-and-PFS-pages/ba-p/383125) sono pagine speciali all'interno di un file di database che vengono usate da SQL Server per individuare lo spazio libero durante l'allocazione dello spazio per un oggetto. La contesa di latch nelle pagine PFS è comunemente associata a [TempDB](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d), ma può anche verificarsi nei database utente quando sono presenti molti thread simultanei di allocazione degli oggetti. Questo miglioramento cambia la modalità di gestione della concorrenza per gli aggiornamenti delle pagine PFS, in modo che l'aggiornamento possa essere eseguito in un latch condiviso anziché in un latch esclusivo. Questo comportamento è abilitato per impostazione predefinita in tutti i database (incluso TempDB) a partire da [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].|
|Migrazione dei ruoli di lavoro dell'utilità di pianificazione |La migrazione dei ruoli di lavoro consente a un'utilità di pianificazione inattiva di eseguire la migrazione di un ruolo di lavoro dalla coda eseguibile di un'altra utilità di pianificazione nello stesso nodo NUMA e riprendere immediatamente l'attività del ruolo di lavoro migrato. Questo miglioramento consente un utilizzo più equilibrato della CPU nelle situazioni in cui le attività a esecuzione prolungata vengono assegnate alla stessa utilità di pianificazione. Per altre informazioni, vedere [SQL Server 2019 Intelligent Performance - Worker Migration](https://techcommunity.microsoft.com/t5/SQL-Server/SQL-Server-2019-Intelligent-Performance-Worker-Migration/ba-p/939610) (Prestazioni intelligenti di SQL Server 2019 - Migrazione dei ruoli di lavoro). |
| &nbsp; | &nbsp; |

### <a name="monitoring"></a>Monitoraggio
I miglioramenti del monitoraggio consentono di accedere a informazioni dettagliate sulle prestazioni di qualsiasi carico di lavoro del database al momento necessario.

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|`WAIT_ON_SYNC_STATISTICS_REFRESH` | Un nuovo tipo di attesa nella DMV `sys.dm_os_wait_stats`. Mostra il tempo accumulato a livello di istanza dedicato alle operazioni di aggiornamento delle statistiche sincrone. Vedere [`sys.dm_os_wait_stats`](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|
|Criteri di acquisizione personalizzati per Query Store | Quando sono abilitati questi criteri, le configurazioni aggiuntive di Query Store sono disponibili in una nuova impostazione di criteri di acquisizione di Query Store per ottimizzare la raccolta dati in un server specifico. Vedere [Opzioni ALTER DATABASE SET](../t-sql/statements/alter-database-transact-sql-set-options.md).|
|`LIGHTWEIGHT_QUERY_PROFILING`| Una nuova configurazione con ambito database. Vedere [`LIGHTWEIGHT_QUERY_PROFILING`](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#lqp). |
|`sys.dm_exec_requests` colonna `command` | Mostra `SELECT (STATMAN)` se un'operazione `SELECT` è in attesa del completamento di un'operazione di aggiornamento delle statistiche sincrona prima di continuare l'esecuzione di query. Vedere [`sys.dm_exec_requests`](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|
|`sys.dm_exec_query_plan_stats` | Una nuova funzione a gestione dinamica (DMF) che restituisce l'equivalente dell'ultimo piano di esecuzione effettivo noto per tutte le query. Vedere [sys.dm_exec_query_plan_stats](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).|
|`LAST_QUERY_PLAN_STATS` | Una nuova configurazione con ambito database che abilita `sys.dm_exec_query_plan_stats`. Vedere [ALTER DATABASE SCOPED CONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).|
|`query_post_execution_plan_profile` | Un evento esteso che raccoglie l'equivalente di un piano di esecuzione effettivo in base alla profilatura leggera, a differenza di `query_post_execution_showplan` che usa la profilatura standard. Vedere [Infrastruttura di profilatura query](../relational-databases/performance/query-profiling-infrastructure.md).|
|`sys.dm_db_page_info(database_id, file_id, page_id, mode)` | Una nuova DMF che restituisce informazioni su una pagina in un database. Vedere [sys.dm_db_page_info (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="developer-experience"></a>Esperienza per gli sviluppatori
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] continua a offrire un'esperienza di sviluppo di qualità elevata, grazie a miglioramenti ai grafi e ai tipi di dati spaziali, al supporto UTF-8 e a un nuovo framework di estendibilità, che consente agli sviluppatori di usare il proprio linguaggio preferito per ottenere informazioni dettagliate su tutti i dati.

### <a name="graph"></a>Grafico

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Azioni di eliminazione propagate su un vincolo di arco | Ora è possibile definire azioni di eliminazione propagate su un vincolo ad arco in un database a grafo. Vedere [Vincoli di arco](../relational-databases/tables/graph-edge-constraints.md). |
|Nuova funzione grafo: `SHORTEST_PATH` | Ora è possibile usare `SHORTEST_PATH` all'interno di `MATCH` per trovare il percorso più breve tra due nodi in un grafo o eseguire attraversamenti di lunghezza arbitraria.|
|Partizionare tabelle e indici| Le tabelle grafo supportano ora il partizionamento di tabelle e indici. |
|Usare alias di tabelle o viste derivate nelle query corrispondenti ai grafi |Vedere [Query corrispondenti ai grafi](../t-sql/queries/match-sql-graph.md). |
| &nbsp; | &nbsp; |

### <a name="unicode-support"></a>Supporto Unicode
Per il supporto delle aziende estese in più paesi e aree geografiche, in cui il requisito di fornire servizi e applicazioni di database multilingue globali è essenziale per soddisfare le esigenze dei clienti e rispettare le normative specifiche del mercato. 

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Supporto per la codifica di caratteri UTF-8 |Supporta UTF-8 per la codifica di importazione ed esportazione e come regole di confronto a livello di colonna o a livello di database per i dati stringa. Il supporto include le tabelle esterne PolyBase e Always Encrypted (al di fuori dell'uso con enclavi). Vedere [Regole di confronto e supporto Unicode](../relational-databases/collations/collation-and-unicode-support.md).|
| &nbsp; | &nbsp; |

### <a name="language-extensions"></a>Estensioni del linguaggio

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Nuovo SDK del linguaggio Java | Semplifica lo sviluppo di applicazioni Java eseguibili da SQL Server. Vedere [Microsoft Extensibility SDK per Java per SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md). |
|L'SDK del linguaggio Java è open source |[Microsoft Extensibility SDK per Java per Microsoft SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md) è ora open source e [disponibile in GitHub](https://github.com/microsoft/sql-server-language-extensions).|
|Supporto per i tipi di dati Java|Vedere [Java data types](../language-extensions/how-to/java-to-sql-data-types.md) (Tipi di dati Java).|
|Nuovo runtime Java predefinito | SQL Server include ora il supporto di Zulu Embedded per Java di Azul System in tutto il prodotto. Vedere [Free supported Java in SQL Server 2019 is now available](https://cloudblogs.microsoft.com/sqlserver/2019/07/24/free-supported-java-in-sql-server-2019-is-now-available/) (È ora disponibile il supporto gratuito di Java in SQL Server 2019). |
|Estensioni del linguaggio di SQL Server| È possibile eseguire codice esterno con il framework di estendibilità. Vedere [Estensioni del linguaggio di SQL Server](../language-extensions/language-extensions-overview.md).
|Registrare i linguaggi esterni|Una nuova istruzione DDL (Data Definition Language), `CREATE EXTERNAL LANGUAGE`, registra i linguaggi esterni, ad esempio Java, in SQL Server. Vedere [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). |
| &nbsp; | &nbsp; |

### <a name="spatial"></a>Spatial

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
| Nuovi identificatori SRID (Spatial Reference Identifier) |[Australian GDA2020](http://www.ga.gov.au/scientific-topics/positioning-navigation/geodesy/datums-projections/gda2020) offre dati più stabili e accurati, più strettamente allineati ai sistemi di posizionamento globale. I nuovi identificatori SRID sono:<ul><li>7843 per 2D geografico</li><li>7844 per 3D geografico</li></ul> Per le definizioni dei nuovi identificatori SRID, vedere la vista [sys.spatial_reference_systems](../relational-databases/system-catalog-views/sys-spatial-reference-systems-transact-sql.md). |
| &nbsp; | &nbsp; |

### <a name="error-messages"></a>messaggi di errore
Tradizionalmente, quando un processo di estrazione, trasformazione e caricamento (ETL) ha esito negativo perché l'origine e la destinazione non hanno tipi di dati e/o lunghezze corrispondenti, la risoluzione dei problemi richiede molto tempo, soprattutto in set di dati di grandi dimensioni. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] consente di ottenere più velocemente dati analitici sugli errori di troncamento dei dati.

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Avvisi di troncamento dettagliati | Il messaggio di errore di troncamento dati include per impostazione predefinita i nomi di tabelle e colonne e il valore troncato. Vedere [VERBOSE_TRUNCATION_WARNINGS](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#verbose-truncation).|
| &nbsp; | &nbsp; |

## <a name="mission-critical-security"></a>Sicurezza cruciale
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fornisce un'architettura di sicurezza progettata per consentire agli amministratori di database e agli sviluppatori di creare applicazioni di database sicure e contrastare le minacce. Ogni versione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è stata migliorata rispetto alle versioni precedenti con l'introduzione di nuove caratteristiche e funzionalità e anche [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] prosegue in questa tradizione.

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Always Encrypted con enclave sicuri|Estende la funzionalità Always Encrypted con crittografia sul posto e calcoli avanzati abilitando i calcoli sui dati di testo non crittografato all'interno di un enclave sicuro sul lato server. La crittografia sul posto migliora le prestazioni e l'affidabilità delle operazioni di crittografia (crittografia delle colonne, rotazione delle chiavi di crittografia delle colonne e così via) in quanto evita di spostare i dati all'esterno del database.<br><br> Il supporto per i calcoli avanzati (criteri di ricerca e operazioni di confronto) consente l'uso della funzionalità Always Encrypted con un set molto più ampio di scenari e applicazioni che richiedono la protezione dei dati sensibili, oltre a funzionalità più avanzate nelle query Transact-SQL. Vedere [Always Encrypted con enclave sicuri](../relational-databases/security/encryption/always-encrypted-enclaves.md).|
|Gestione dei certificati in Gestione configurazione SQL Server|È ora possibile eseguire le attività di gestione dei certificati, ad esempio la visualizzazione e la distribuzione dei certificati, usando Gestione configurazione SQL Server. Vedere [Gestione dei certificati (Gestione configurazione SQL Server)](../database-engine/configure-windows/manage-certificates.md).|
|Individuazione dati e classificazione|Individuazione dati e classificazione offre funzionalità per la classificazione e l'etichettatura delle colonne nelle tabelle utente. La classificazione dei dati sensibili (dati aziendali, finanziari, medici, personali, ecc.) può avere un ruolo fondamentale nella protezione delle informazioni dell'organizzazione. Possono costituire l'infrastruttura per:<ul><li>Contribuire a soddisfare i requisiti in materia di privacy e di conformità alle normative</li><li>Supportare vari scenari di sicurezza, quali il monitoraggio (controllo) e gli avvisi in caso di accesso anomalo ai dati sensibili</li><li>Semplificare l'identificazione della posizione dei dati sensibili nell'organizzazione, in modo che gli amministratori possano adottare le misure necessarie per proteggere il database</li></ul>|
|SQL Server Audit|Anche il [controllo](../relational-databases/security/auditing/sql-server-audit-database-engine.md) è stato migliorato e il log di controllo include un nuovo campo `data_sensitivity_information` che contiene le classificazioni di riservatezza (etichette) dei dati effettivi restituiti dalla query. Per dettagli ed esempi, vedere [`ADD SENSITIVITY CLASSIFICATION`](../t-sql/statements/add-sensitivity-classification-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="high-availability"></a>Disponibilità elevata
Un'attività comune di cui deve tenere conto chiunque esegua una distribuzione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è verificare se tutte le istanze cruciali di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e i database all'interno delle stesse sono disponibili quando gli utenti finali e aziendali ne hanno bisogno. La disponibilità è un pilastro chiave della piattaforma [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce molte nuove funzionalità e miglioramenti che consentono alle aziende di garantire la disponibilità elevata degli ambienti di database.

### <a name="availability-groups"></a>Gruppi di disponibilità

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Fino a cinque repliche sincrone|[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] aumenta il numero massimo di repliche sincrone fino a 5. Il numero massimo era 3 in [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. È possibile configurare questo gruppo di cinque repliche per il failover automatico all'interno del gruppo. Sono presenti una replica primaria e 4 repliche secondarie sincrone.|
|Reindirizzamento della connessione di replica da secondaria a primaria| consente di orientare le connessioni dell'applicazione client alla replica primaria, indipendentemente dal server di destinazione specificato nella stringa di connessione. Per informazioni dettagliate, vedere [Secondary to primary replica read/write connection redirection (Always On Availability Groups)](../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md) (Reindirizzamento connessione in lettura/scrittura dalla replica secondaria alla primaria (Gruppi di disponibilità AlwaysOn)).|
|Vantaggi della disponibilità elevata e del ripristino di emergenza| Ogni cliente Software Assurance di SQL Server potrà usufruire di tre vantaggi aggiuntivi per qualsiasi versione di SQL Server supportata da Microsoft. Per informazioni dettagliate, vedere [questo annuncio.](https://cloudblogs.microsoft.com/sqlserver/2019/10/30/new-high-availability-and-disaster-recovery-benefits-for-sql-server/)
| &nbsp; | &nbsp; |

### <a name="recovery"></a>Ripristino

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Recupero del database accelerato | Il ripristino accelerato del database (Accelerated Database Recovery, ADR) riduce il tempo necessario per il recupero dopo un riavvio o un rollback delle transazioni a esecuzione prolungata. Vedere [Recupero del database accelerato](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr).|
| &nbsp; | &nbsp; |

### <a name="resumable-operations"></a>Operazioni ripristinabili

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Compilazione e ricompilazione degli indici columnstore cluster online | Vedere [Eseguire operazioni online sugli indici](../relational-databases/indexes/perform-index-operations-online.md). |
|Compilazione degli indici rowstore online ripristinabili | Vedere [Eseguire operazioni online sugli indici](../relational-databases/indexes/perform-index-operations-online.md). |
|Sospendere e riprendere l'analisi iniziale per Transparent Data Encryption (TDE)|Vedere [Analisi Transparent Data Encryption (TDE) con possibilità di sospensione e ripresa](../relational-databases/security/encryption/transparent-data-encryption.md#scan-suspend-resume).|
| &nbsp; | &nbsp; |

## <a name="platform-choice"></a>Scelta della piattaforma
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] si basa sulle innovazioni introdotte in [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] per consentire l'esecuzione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sulla propria piattaforma preferita con più funzionalità e sicurezza che mai.

### <a name="linux"></a><a id="sql-server-on-linux"></a>Linux

| Nuova funzionalità o aggiornamento | Dettagli |
|:-----|:-----|
|Supporto della replica | Vedere [Replica di SQL Server in Linux](../linux/sql-server-linux-replication.md). |
|Supporto di Microsoft Distributed Transaction Coordinator (MSDTC) | Vedere [Come configurare MSDTC in Linux](../linux/sql-server-linux-configure-msdtc.md). |
|Supporto OpenLDAP per provider AD di terze parti | Vedere [Esercitazione: Usare l'autenticazione di Azure Active Directory con SQL Server in Linux](../linux/sql-server-linux-active-directory-authentication.md). |
|Machine Learning Services in Linux | Vedere [Installare SQL Server Machine Learning Services (Python e R) in Linux](../linux/sql-server-linux-setup-machine-learning.md). |
|Miglioramenti di TempDB | Per impostazione predefinita, una nuova installazione di SQL Server in Linux crea più file di dati TempDB in base al numero di core logici (con un massimo di otto file di dati). Questo non vale per gli aggiornamenti sul posto di una versione principale o secondaria. Ogni file TempDB è di 8 MB, con un aumento automatico di 64 MB. Questo comportamento è simile all'installazione predefinita di SQL Server in Windows. |
|PolyBase in Linux | Vedere [Installare PolyBase](../relational-databases/polybase/polybase-linux-setup.md) in Linux per i connettori non Hadoop.<br/><br/>Vedere [Mapping dei tipi di PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Supporto di Change Data Capture (CDC) | Change Data Capture (CDC) è ora supportato in Linux per [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| &nbsp; | &nbsp; |

### <a name="containers"></a>Contenitori
Il modo più semplice per iniziare a lavorare con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] consiste nell'usare i contenitori. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] si basa sulle innovazioni introdotte nelle versioni precedenti per consentire la distribuzione di contenitori di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] su nuove piattaforme, in modo più sicuro e con più funzionalità.

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
| Registro Container Microsoft | Il [Registro Container Microsoft](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) sostituisce ora Docker Hub per le nuove immagini di contenitori Microsoft ufficiali, incluso [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| Contenitori non radice | In [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] è stata introdotta la possibilità di creare contenitori più sicuri avviando, per impostazione predefinita, il processo [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] come utente non radice. Vedere [Compilare ed eseguire contenitori di SQL Server come utente non radice](../linux/sql-server-linux-docker-container-deployment.md). |
| Immagini del contenitore con certificazione Red Hat | A partire da [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], è possibile eseguire i contenitori di SQL Server in Red Hat Enterprise Linux. |
| Supporto di Machine Learning e PolyBase| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce nuovi modi per lavorare con i contenitori di SQL Server, ad esempio Machine Learning Services e PolyBase. Alcuni esempi sono disponibili nel [repository GitHub di SQL Server in contenitore](https://github.com/microsoft/mssql-docker/tree/master/linux/preview/examples). |
| &nbsp; | &nbsp; |

## <a name="setup-options"></a>Opzioni di configurazione

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---| 
|Nuove opzioni di configurazione della memoria | Imposta le opzioni di configurazione del server *min server memory (MB)* e *max server memory (MB)* durante l'installazione. Vedere [Pagina Configurazione del motore di database - Memoria](./install/instance-configuration.md#memory) e i parametri `USESQLRECOMMENDEDMEMORYLIMITS`, `SQLMINMEMORY` e `SQLMAXMEMORY` in [Installare SQL Server dal prompt dei comandi](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). Il valore proposto è allineato con le linee guida per la configurazione della memoria riportate in [Opzioni di configurazione server memory](../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).| 
|Nuove opzioni di impostazione del parallelismo | Imposta l'opzione di configurazione del server *max degree of parallelism* durante l'installazione. Vedere [Pagina Configurazione del motore di database - MaxDOP](./install/instance-configuration.md#maxdop) e il parametro `SQLMAXDOP` in [Installare SQL Server dal prompt dei comandi](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). Il valore predefinito è allineato con le linee guida relative all'opzione relativa al massimo grado di parallelismo riportate in [Configurare l'opzione di configurazione del server max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).| 
|Impostare un avviso per il codice Product Key di una licenza Server/CAL|Se viene immesso un codice Product Key per una licenza CAL di Enterprise Server e il computer ha più di 20 core fisici o 40 core logici quando Hyper-Threading è abilitato, viene visualizzato un avviso durante l'installazione. Gli utenti possono riconoscere la limitazione e continuare l'installazione oppure immettere un codice di licenza che supporti il numero massimo di processori del sistema operativo.|
| &nbsp; | &nbsp; |

## <a name="sql-server-machine-learning-services"></a><a id="ml"></a> SQL Server Machine Learning Services

|Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Modellazione basata sulle partizioni | È possibile elaborare gli script esterni per le singole partizioni dei dati usando i nuovi parametri aggiunti a `sp_execute_external_script`. Questa funzionalità supporta il training di molti modelli di dimensioni ridotte (un modello per ogni partizione dei dati) invece del training di un solo modello di grandi dimensioni. Vedere [Creare modelli basati sulla partizione](../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).|
|Cluster di failover di Windows Server| È possibile configurare la disponibilità elevata per Machine Learning Services in un cluster di failover di Windows Server.|
| &nbsp; | &nbsp; |

## <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

Questa versione introduce nuove funzionalità e miglioramenti per le prestazioni, la governance delle risorse e il supporto di client.

| Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Gruppi di calcolo in modelli tabulari| I gruppi di calcolo possono ridurre significativamente il numero di misure ridondanti raggruppando le espressioni di misura comuni come *elementi di calcolo*. Per altre informazioni, vedere [Gruppi di calcolo in modelli tabulari](/analysis-services/tabular-models/calculation-groups). |
|Interfoliazione di query| L'interfoliazione di query è una configurazione di sistema in modalità tabulare che può migliorare i tempi di risposta delle query utente negli scenari con concorrenza elevata. Per altre informazioni, vedere [Interfoliazione di query](/analysis-services/tabular-models/query-interleaving). |
|Relazioni molti-a-molti nei modelli tabulari| Consente le relazioni molti-a-molti tra le tabelle in cui entrambe le colonne non sono univoche. Per altre informazioni, vedere [Relazioni nei modelli tabulari](/analysis-services/tabular-models/relationships-ssas-tabular).|
|Impostazioni delle proprietà per la governance delle risorse| Questa versione include nuove impostazioni di memoria: Memory\QueryMemoryLimit, DbpropMsmdRequestMemoryLimit e OLAP\Query\RowsetSerializationLimit per la governance delle risorse. Per altre informazioni, vedere le [impostazioni di memoria](/analysis-services/server-properties/memory-properties).|
|Impostazione di governance per gli aggiornamenti della cache di Power BI | Questa versione introduce la proprietà ClientCacheRefreshPolicy, che esegue l'override della memorizzazione nella cache dei dati dei riquadri del dashboard e dei dati del report per il caricamento iniziale del report Live Connect da parte del servizio Power BI. Per altre informazioni, vedere [Proprietà generali](/analysis-services/server-properties/general-properties). |
| Collegamento online  | Il collegamento online può essere usato per la sincronizzazione di repliche di sola lettura negli ambienti con scalabilità orizzontale delle query locali. Per altre informazioni, vedere [Collegamento online](/analysis-services/what-s-new-in-sql-server-analysis-services#online-attach). |
| &nbsp; | &nbsp; |

## <a name="sql-server-integration-services"></a>SQL Server Integration Services

In questa versione sono state introdotte nuove funzionalità per migliorare le operazioni sui file.

| Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Attività File flessibili |Eseguire operazioni sui file in file system locale, Archiviazione BLOB di Azure e Azure Data Lake Storage Gen2. Vedere [Attività File flessibili](../integration-services/control-flow/flexible-file-task.md).|
|Origine e destinazione di File flessibili |Leggere e scrivere dati per Archiviazione BLOB di Azure e Azure Data Lake Storage Gen2. Vedere [Origine di File flessibili](../integration-services/data-flow/flexible-file-source.md) e [Destinazione di File flessibili](../integration-services/data-flow/flexible-file-destination.md). |

## <a name="sql-server-master-data-services"></a>SQL Server [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]

| Nuova funzionalità o aggiornamento | Dettagli |
|:---|:---|
|Supporto per i database di Istanza gestita di SQL di Azure| Ospitare [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)] in Istanza gestita di SQL di Azure. Vedere [[!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]Installazione e configurazione](../master-data-services/master-data-services-installation-and-configuration.md#SetUpWeb).|
|Nuovi controlli HTML| I controlli HTML sostituiscono tutti i componenti di Silverlight precedenti. La dipendenza da Silverlight è stata rimossa.|
| &nbsp; | &nbsp; |

## <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

Questa versione di SQL Server Reporting Services offre il supporto per istanze gestite di SQL di Azure, set di dati di Power BI Premium, accessibilità avanzata, Azure Active Directory Application Proxy e Transparent Data Encryption. Include inoltre un aggiornamento di Generatore report Microsoft. Per informazioni dettagliate, vedere [Novità di SQL Server Reporting Services](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="see-also"></a>Vedere anche

- [`SqlServer` Modulo PowerShell](https://www.powershellgallery.com/packages/Sqlserver)
- [Documentazione di SQL Server PowerShell](../powershell/sql-server-powershell.md)

## <a name="next-steps"></a>Passaggi successivi

- [Workshop su SQL Server](https://aka.ms/sqlworkshops)
- [Note sulla versione di [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](sql-server-version-15-release-notes.md)
- [Microsoft [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]: white paper tecnico](https://aka.ms/sql2019whitepaper)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
