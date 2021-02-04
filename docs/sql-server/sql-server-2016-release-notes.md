---
title: Note sulla versione di SQL Server 2016 | Microsoft Docs
description: Questo documento descrive i problemi noti di cui è necessario essere a conoscenza prima di installare o risolvere i problemi relativi alle versioni di Microsoft SQL Server 2016.
ms.date: 04/25/2018
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- build notes
- release issues
ms.assetid: c64077a2-bec8-4c87-9def-3dbfb1ea1fb6
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: dad7665ffa825a80aa6bcb3050b04a96060c3f65
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076043"
---
# <a name="sql-server-2016-release-notes"></a>Note sulla versione di SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  
  Questo articolo descrive le limitazioni e i problemi relativi alle versioni di SQL Server 2016, inclusi i Service Pack. Per informazioni sulle novità, vedere [Novità di SQL Server 2016](./what-s-new-in-sql-server-2016.md).

- [![Download da Evaluation Center](../includes/media/download2.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)  Scaricare SQL Server 2016 da **[Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**
- [![Macchina virtuale di Azure piccola](../includes/media/azure-vm.png)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview) Se si ha un account di Azure,  fare clic **[qui](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)** per creare rapidamente una macchina virtuale in cui è già installato SQL Server 2016 SP1.
- [![Download di SSMS](../includes/media/download2.png)](../ssms/download-sql-server-management-studio-ssms.md) Per ottenere la versione più recente di SQL Server Management Studio, vedere **[Scaricare SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)** .

## <a name="sql-server-2016-service-pack-2-sp2"></a><a name="bkmk_2016sp2"></a>SQL Server 2016 Service Pack 2 (SP2)

![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP2 include tutti gli aggiornamenti cumulativi rilasciati dopo la versione 2016 SP1, fino a CU8 compreso.

- [![Area download Microsoft](../includes/media/download2.png)](https://www.microsoft.com/download/details.aspx?id=56836) [Scaricare SQL Server 2016 Service Pack 2 (SP2)](https://www.microsoft.com/download/details.aspx?id=56836)
- Per un elenco completo degli aggiornamenti, vedere [Informazioni sulla versione Service Pack 2 di SQL Server 2016](https://support.microsoft.com/help/4052908/sql-server-2016-service-pack-2-release-information)

L'installazione di SQL Server 2016 SP2 può richiedere il riavvio dopo l'installazione. Come procedura consigliata, è opportuno pianificare ed eseguire un riavvio dopo l'installazione di SQL Server 2016 SP2.

Miglioramenti relativi a prestazioni e scalabilità inclusi in SQL Server 2016 SP2.

|Funzionalità|Descrizione|Ulteriori informazioni|
|---|---|---|
|Procedura di pulizia del database di distribuzione migliorata   |   Una tabella del database di distribuzione di dimensioni eccessive è la causa di una situazione di blocco e di deadlock. Grazie a una procedura di pulizia migliorata è possibile eliminare scenari di blocco e di deadlock di questo genere.   |   [KB4040276](https://support.microsoft.com/help/4040276/fix-indirect-checkpoints-on-the-tempdb-database-cause-non-yielding)   |
|Pulizia rilevamento modifiche   |   Miglioramento delle prestazioni di pulizia di Rilevamento modifiche ed efficienza delle tabelle lato Rilevamento modifiche.   |   [KB4052129](https://support.microsoft.com//help/4052129/update-for-manual-change-tracking-cleanup-procedure-in-sql-server-2016)   |
|Usare il timeout della CPU per annullare la richiesta di Resource Governor   |   Migliora la gestione delle richieste di query annullando di fatto la richiesta, se viene raggiunta la soglia della CPU per una richiesta. Questo comportamento viene abilitato con il flag di traccia 2422.   |   [KB4038419](https://support.microsoft.com/help/4038419/add-cpu-timeout-to-resource-governor-request-max-cpu-time-sec)   |
|SELECT INTO per creare la tabella di destinazione in filegroup   |   A partire da SQL Server 2016 SP2, la sintassi T-SQL SELECT INTO supporta il caricamento di una tabella in un filegroup diverso da quello predefinito dell'utente usando la parola chiave <Filegroup name> ON nella sintassi T-SQL.   |      |
|Checkpoint indiretto migliorato per TempDB   |   Il checkpoint indiretto per TempDB è stato migliorato per ridurre al minimo la contesa di spinlock in DPLists. Questo miglioramento consente di aumentare per impostazione predefinita il numero di istanze del carico di lavoro TempDB in SQL Server 2016 se il checkpoint indiretto è attivo per TempDB.   |   [KB4040276](https://support.microsoft.com/help/4040276)   |
|Prestazioni di backup del database migliorate nei computer di memoria di grandi dimensioni   |   SQL Server 2016 SP2 ottimizza il modo in cui le operazioni di I/O in corso vengono svuotate durante il backup, comportando un significativo miglioramento delle prestazioni di backup per i database di piccole e medie dimensioni. È stato osservato un miglioramento più di 100 volte superiore trasferendo i backup del database di sistema in un computer da 2 TB. Il miglioramento delle prestazioni si riduce con l'aumentare delle dimensioni del database e delle pagine di cui eseguire il backup e l'I/O di backup richiede più tempo rispetto allo scorrimento del pool di buffer. Questa modifica consentirà di aumentare le prestazioni di backup per i clienti che ospitano più database di piccole dimensioni in server di fascia alta di grandi dimensioni con molta memoria.   |      |
|Supporto per la compressione del backup VDI per i database abilitati per TDE   |   In SQL Server 2016 SP2 è stato aggiunto il supporto VDI per consentire alle soluzioni di backup VDI di sfruttare la compressione per i database abilitati per TDE. Con questo miglioramento, è stato introdotto un nuovo formato di backup per supportare la compressione del backup per i database abilitati per TDE. Il motore di SQL Server gestirà in modo trasparente i formati di backup nuovi ed esistenti per ripristinare i backup.   |      |
|Caricamento dinamico dei parametri dei profili agenti di replica   |   Questo nuovo miglioramento consente di caricare i parametri degli agenti di replica in modo dinamico senza dover riavviare l'agente. Questa modifica è applicabile solo ai parametri dei profili agenti di uso più frequente.   |      |
|Opzione MAXDOP di supporto per creazione/aggiornamento di statistiche   |    Questo miglioramento consente non solo di specificare l'opzione MAXDOP per un'istruzione CREATE/UPDATE relativa alle statistiche, ma anche di verificare che sia usata l'impostazione MAXDOP corretta quando le statistiche vengono aggiornate durante la creazione o la ricompilazione di tutti i tipi di indici (se l'opzione MAXDOP è presente)   |   [KB4041809](https://support.microsoft.com/help/4041809)   |
|Aggiornamento delle statistiche automatico migliorato per le statistiche incrementali   |    In alcuni scenari, quando si è verificato un certo numero di modifiche ai dati tra più partizioni in una tabella in modo tale che il contatore di modifiche totali per le statistiche incrementate supera la soglia di aggiornamenti automatici, ma nessuna delle singole partizioni supera la soglia di aggiornamenti automatici, l'aggiornamento delle statistiche potrebbe venire rimandato finché nella tabella non si verifica un numero molto più elevato di modifiche. Questo comportamento è stato corretto con il flag di traccia 11024.   |      |

Miglioramenti relativi a supporto e diagnostica in SQL Server 2016 SP2.

|Funzionalità|Descrizione|Ulteriori informazioni|
|---|---|---|
|Supporto DTC completo per i database in un gruppo di disponibilità   |   Le transazioni tra database per i database che fanno parte di un gruppo di disponibilità non sono attualmente supportate per SQL Server 2016. Con SQL Server 2016 SP2, viene introdotto il supporto completo per le transazioni distribuite con i database di un gruppo di disponibilità.   |      |
|Aggiornamento della colonna is_encrypted di sys.databases per rispecchiare in modo preciso lo stato della crittografia di TempDB   |   Il valore della colonna is_encrypted in sys.databases è 1 per TempDB, anche dopo avere disattivato la crittografia per tutti i database utente e avere riavviato SQL Server. Il comportamento previsto è invece che questo valore sia 0, perché TempDB non è più crittografato in questa situazione. A partire da SQL Server 2016 SP2, is_encrypted di sys.databases rispecchia in modo preciso lo stato della crittografia per TempDB.   |      |
|Nuove opzioni DBCC CLONEDATABASE per generare un clone verificato e un backup   |   Con SQL Server 2016 SP2, DBCC CLONEDATABASE consente due nuove opzioni: generare un clone verificato o generare un clone di backup. Quando viene creato un database clone usando l'opzione WITH VERIFY_CLONEDB, viene creato e verificato un clone del database coerente che verrà supportato da Microsoft per l'uso in produzione. Per convalidare la verifica del clone, è stata introdotta la nuova proprietà SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone'). Quando viene creato un clone con l'opzione BACKUP_CLONEDB, viene generato un backup nella stessa cartella del file di dati per facilitare ai clienti lo spostamento del clone in un altro server o l'invio al supporto tecnico Microsoft per la risoluzione dei problemi.   |      |
|Supporto di Service Broker (SSB) per DBCC CLONEDATABASE   |   Comando DBCC CLONEDATABASE migliorato per consentire lo script di oggetti SSB.   |      |
|Nuova DMV per monitorare l'utilizzo dello spazio dell'archivio versioni in TempDB   |   In SQL Server 2016 SP2 è stata introdotta la nuova DMV sys.dm_tran_version_store_space_usage per consentire il monitoraggio dell'utilizzo dell'archivio versioni in TempDB. Gli amministratori di database ora possono pianificare in modo proattivo il ridimensionamento di TempDB in base ai requisiti di utilizzo dell'archivio versioni dei singoli database, senza sovraccarico delle prestazioni durante l'esecuzione nei server di produzione.   |      |
|Supporto dei dump completi per gli agenti di replica | Oggi, se gli agenti di replica incontrano un'eccezione non gestita, l'impostazione predefinita prevede la creazione di un breve dump dei sintomi dell'eccezione. La risoluzione dei problemi relativi alle eccezioni non gestite è quindi molto difficile. Attraverso questa modifica viene introdotta una nuova chiave del Registro di sistema che consentirà di creare un dump completo per gli agenti di replica.   |      |
|Miglioramento degli eventi estesi per l'errore di routing in lettura per un gruppo di disponibilità   |   In precedenza, se era presente un elenco di routing, veniva generato l'XEvent read_only_rout_fail, ma nessuno dei server nell'elenco di routing era disponibile per le connessioni. SQL Server 2016 SP2 aggiunge informazioni per facilitare la risoluzione dei problemi e coinvolgere anche gli elementi di codice in cui è attivo XEvent.   |      |
|Nuova DMV per monitorare il log delle transazioni |   È stata aggiunta una nuova DMV sys.dm_db_log_stats che restituisce attributi a livello di riepilogo e informazioni sui file registro transazioni di database.   |      |
|Nuova DMV per monitorare le informazioni VLF |   La nuova DMV sys.dm_db_log_info è stata introdotta in SQL Server 2016 SP2 per esporre le informazioni VLF simili a DBCC LOGINFO per monitorare, segnalare ed evitare potenziali problemi dei log delle transazioni riscontrati dai clienti.   |      |
|Informazioni sul processore in sys.dm_os_sys_info|   Nuove colonne aggiunte alla DMV sys.dm_os_sys_info per esporre le informazioni relative al processore, ad esempio socket_count e cores_per_numa.   |      |
|Informazioni sugli extent modificati in sys.dm_db_file_space_usage|   Nuova colonna aggiunta a sys.dm_db_file_space_usage per tenere traccia del numero di extent modificati dall'ultimo backup completo.   |      |
|Informazioni sui segmenti in sys.dm_exec_query_stats   |   Nuove colonne sono state aggiunte a sys.dm_exec_query_stats per tenere traccia del numero di segmenti columnstore ignorati e letti, ad esempio total_columnstore_segment_reads e total_columnstore_segment_skips.   |   [KB4051358](https://support.microsoft.com/help/4051358)   |
|Impostazione del livello di compatibilità corretto per i database di distribuzione   |   Dopo l'installazione del Service Pack, il livello di compatibilità del database di distribuzione passa a 90. La causa era un percorso del codice nella stored procedure sp_vupgrade_replication. Il Service Pack è stato modificato e ora imposta il livello di compatibilità corretto per il database di distribuzione.   |      |
|Esporre le ultime informazioni valide note DBCC CHECKDB   |   È stata aggiunta una nuova opzione di database per restituire a livello di codice la data dell'ultima esecuzione di DBCC CHECKDB riuscita. Gli utenti ora possono eseguire una query su DATABASEPROPERTYEX([database], 'lastgoodcheckdbtime') per ottenere un singolo valore che rappresenta la data/ora dell'ultima esecuzione di DBCC CHECKDB riuscita sul database specificato.   |      |
|Miglioramenti del file XML dello showplan|   [Informazioni sulle statistiche usate per compilare il piano di query](/archive/blogs/sql_server_team/sql-server-2017-showplan-enhancements), inclusi nome delle statistiche, contatore delle modifiche, percentuale di campionamento e ora dell'ultimo aggiornamento delle statistiche. Si noti che questa funzionalità è stata aggiunta solo per i modelli CE 120 e versioni successive. Ad esempio, non è supportata per CE 70.| |
| |Un nuovo attributo [EstimateRowsWithoutRowgoal](/archive/blogs/sql_server_team/more-showplan-enhancements-row-goal) viene aggiunto al file XML dello showplan se Query Optimizer usa la logica "obiettivo di riga".| |
| |Nuovi attributi di runtime [UdfCpuTime e UdfElapsedTime](/archive/blogs/sql_server_team/more-showplan-enhancements-udfs) nel file XML dello showplan effettivo, per tenere traccia del tempo impiegato per le funzioni definite dall'utente scalari.| |
| |Aggiunta del tipo di attesa CXPACKET all'[elenco dei primi 10 tipi di attesa possibili](/archive/blogs/sql_server_team/new-showplan-enhancements) nel file XML dello showplan effettivo. L'esecuzione di query parallele comporta spesso attese CXPACKET, ma questo tipo di attesa non era segnalato nel file XML dello showplan effettivo.   |      |
| |Estensione dell'avviso di spill di runtime per segnalare il numero di pagine scritte in TempDB durante lo spill di un operatore di parallelismo.| |
|Supporto della replica per i database con regole di confronto per i caratteri supplementari   |   La replica può ora essere supportata nei database che usano la regola di confronto per i caratteri supplementari.   |      |
|Gestione corretta di Service Broker in caso di failover del gruppo di disponibilità   |   Nell'implementazione corrente, quando Service Broker è abilitato in un database del gruppo di disponibilità, durante un failover del gruppo di disponibilità tutte le connessioni a Service Broker che hanno avuto origine nella replica primaria vengono lasciate aperte. Questo miglioramento ha lo scopo di chiudere tutte le connessioni aperte durante un failover del gruppo di disponibilità.   |      |
|Risoluzione dei problemi relativi alle attese di parallelismo migliorata   |   Aggiunta di una nuova attesa [CXCONSUMER](/archive/blogs/sql_server_team/making-parallelism-waits-actionable).   |      |
|Miglioramento della coerenza tra DMV per le stesse informazioni   |   La DMV sys.dm_exec_session_wait_stats ora tiene traccia delle attese CXPACKET e CXCONSUMER in modo coerente con la DMV sys.dm_os_wait_stats.   |      |
|Miglioramento della risoluzione dei problemi relativi ai deadlock con parallelismo interno alla query | Nuovo evento esteso exchange_spill per segnalare il numero di pagine scritte in TempDB durante lo spill di un operatore di parallelismo, nel nome del campo dell'XEvent worktable_physical_writes.| |
| |Le colonne spills nelle DMV sys.dm_exec_query_stats, sys.dm_exec_procedure_stats e sys.dm_exec_trigger_stats (ad esempio, total_spills) ora includono anche i dati distribuiti dagli operatori di parallelismo.| |
| |Miglioramento della classe Deadlock Graph XML per gli scenari di deadlock con parallelismo, con altri attributi aggiunti alla risorsa exchangeEvent.| |
| |Miglioramento della classe Deadlock Graph XML per i deadlock che coinvolgono operatori in modalità batch, con altri attributi aggiunti alla risorsa SyncPoint.| |
|Ricaricamento dinamico di alcuni parametri dei profili agenti di replica   |   Nell'implementazione corrente degli agenti di replica, per apportare modifiche al parametro del profilo dell'agente, è necessario arrestare e riavviare l'agente. Questi miglioramenti consentono di ricaricare in modo dinamico i parametri senza dover riavviare l'agente di replica.   |      |

![Screenshot di una barra orizzontale.](media/horizontal-bar.png)

## <a name="sql-server-2016-service-pack-1-sp1"></a><a name="bkmk_2016sp1"></a>SQL Server 2016 Service Pack 1 (SP1)
![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP1 include tutti gli aggiornamenti cumulativi fino a SQL Server 2016 RTM CU3, incluso l'aggiornamento della sicurezza MS16-136. Contiene un rollup delle soluzioni disponibili negli aggiornamenti cumulativi di SQL Server 2016 fino all'aggiornamento cumulativo più recente, CU3, incluso e all'aggiornamento della protezione MS16-136 rilasciato l'8 novembre 2016.

Le funzionalità seguenti sono disponibili nelle edizioni Standard, Web, Express e database locale di SQL Server SP1 (ad eccezione dei casi indicati):
- Always Encrypted
- Changed Data Capture (non disponibile in Express)
- columnstore
- Compressione
- Maschera dati dinamica
- Controllo con granularità fine
- OLTP in memoria (non disponibile nel database locale)
- Più contenitori FileStream (non disponibili nel database locale)
- Partizionamento
- PolyBase
- Protezione a livello di riga

La tabella seguente contiene un riepilogo dei principali miglioramenti disponibili in SQL Server 2016 SP1.

|Funzionalità|Descrizione|Ulteriori informazioni|
|---|---|---|
|Inserimento in blocco negli heap con TABLOCK automatico in TF 715| Il flag di traccia 715 abilita il blocco di tabella per le operazioni di caricamento bulk in heap senza indici non cluster.|[La migrazione dei carichi di lavoro SAP a SQL Server è 2,5 volte più veloce](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster)|
|CREATE o ALTER|Consentono la distribuzione di oggetti, ad esempio stored procedure, trigger, funzioni definite dall'utente e visualizzazioni.|[Blog sul motore di database di SQL Server](/archive/blogs/sqlserverstorageengine/create-or-alter-another-great-language-enhancement-in-sql-server-2016-sp1)|
|Supporto DROP TABLE per la replica|Supporto DDL DROP TABLE per la replica per consentire l'eliminazione degli articoli relativi alla replica.|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)|
|Firma del driver RsFx FileStream|Il driver RsFx FileStream viene firmato e certificato usando il dashboard del centro per sviluppatori di hardware Windows (portale per sviluppatori) che consente di installare il driver RsFx FileStream di SQL Server 2016 SP1 in Windows Server 2016 o Windows 10 senza alcun problema.|[La migrazione dei carichi di lavoro SAP a SQL Server è 2,5 volte più veloce](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster)|
|Privilegi LPIM per l'account del servizio SQL, identificazione a livello di codice|Consentono agli amministratori di database di verificare a livello di programmazione se il privilegio Blocco di pagine in memoria (LPIM) è attivo al momento dell'avvio di servizio.|[Developers Choice: identificare a livello di codice i privilegi LPIM e IFI in SQL Server](/archive/blogs/sql_server_team/developers-choice-programmatically-identify-lpim-and-ifi-privileges-in-sql-server)|
|Pulizia manuale rilevamento modifiche|Una nuova stored procedure elimina su richiesta la tabella interna del rilevamento delle modifiche.| [KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)|
|Modifiche dell'operatore parallelo INSERT...SELECT per le tabelle temporanee locali|Nuovo operatore INSERT parallelo nelle operazioni INSERT..SELECT.|[Team di consulenza clienti di SQL Server](/archive/blogs/sqlcat/real-world-parallel-insert-what-else-you-need-to-know)|
|Showplan XML|Diagnostica ampliata che include avvisi di concessione e memoria massima abilitata per una query, flag di traccia abilitati e altre informazioni di diagnostica. | [KB 3190761](https://support.microsoft.com/help/3190761/update-to-improve-diagnostics-by-expose-data-type-of-the-parameters-fo)|
|Storage Class Memory|Ottimizzare l'elaborazione delle transazioni usando Storage Class Memory in Windows Server 2016, che consente di accelerare i tempi di commit delle transazioni per ordini di grandezza.|[Blog sul motore di database di SQL Server](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)|
|USE HINT|Usare l'opzione di query `OPTION(USE HINT('<option>'))` per modificare il comportamento di Query Optimizer usando hint supportati per il livello di query. A differenza di QUERYTRACEON, l'opzione USE HINT non richiede privilegi sysadmin.|[Developers Choice: Hint per le query USE HINT](/archive/blogs/sql_server_team/developers-choice-use-hint-query-hints)|
|Aggiunte di XEvent|Nuove funzionalità di diagnostica di XEvent e Perfmon migliorano la risoluzione dei problemi di latenza.|[Eventi estesi](../relational-databases/extended-events/extended-events.md)|

Inoltre, tenere presenti le correzioni seguenti:
- In base ai suggerimenti degli amministratori di database e della community di SQL, a partire da SQL 2016 SP1 i messaggi di registrazione Hekaton sono ridotti al minimo.
- Esaminare i nuovi [flag di traccia](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
- Le versioni complete dei database di esempio WideWorldImporters ora funzionano con le edizioni Standard ed Express, a partire da SQL Server 2016 SP1, e sono disponibili su [GitHub]( https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0). Non sono necessarie modifiche nell'esempio. I backup del database creati in RTM per l'edizione Enterprise funzionano con le edizioni Standard ed Express in SP1.

L'installazione di SQL Server 2016 SP1 può richiedere il riavvio dopo l'installazione. Come procedura consigliata, è opportuno pianificare ed eseguire un riavvio dopo l'installazione di SQL Server 2016 SP1.

### <a name="download-pages-and-more-information"></a>Scaricare le pagine e altre informazioni

- [Download del Service Pack 1 per Microsoft SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=54276)
- [Blog sul rilascio di SQL Server 2016 Service Pack 1 (SP1)](/archive/blogs/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released)
- [Informazioni sulla versione di SQL Server 2016 Service Pack 1](https://support.microsoft.com/kb/3182545)
- ![info_tip](../sql-server/media/info-tip.png) [Centro aggiornamenti di SQL Server](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) per collegamenti e informazioni per tutte le versioni supportate, compresi i Service Pack di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)]

![Un altro screenshot di una barra orizzontale.](media/horizontal-bar.png)

##  <a name="sql-server-2016-release---general-availability-ga"></a><a name="bkmk_2016_ga"></a> SQL Server 2016 Release - General Availability (GA)
-   [Motore di database (GA)](#bkmk_ga_instalpatch)
-   [Stretch Database (GA)](#bkmk_ga_stretch)
-   [Archivio query (GA)](#bkmk_ga_query_store)
-   [Documentazione del prodotto (GA)](#bkmk_ga_docs)

### <a name="repl_icon_warn--install-patch-requirement-ga"></a>![repl_icon_warn](../database-engine/availability-groups/windows/media/repl-icon-warn.gif) <a name="bkmk_ga_instalpatch"></a> Requisito di installazione patch (disponibilità generale)
**Problema e impatto per i clienti:** Microsoft ha identificato un problema con i file binari di Microsoft VC++ 2013 Runtime che vengono installati come prerequisito da SQL Server 2016. È disponibile un aggiornamento per risolvere questo problema. Se questo aggiornamento dei file binari di VC++ Runtime non viene installato, potrebbero verificarsi problemi di stabilità di SQL Server 2016 in determinati scenari. Prima di installare SQL Server 2016, verificare se il computer richiede la patch descritta in [KB 3164398](https://support.microsoft.com/kb/3164398). La patch è inclusa anche nell'[aggiornamento cumulativo 1 (CU1) per SQL Server 2016 RTM](https://www.microsoft.com/download/details.aspx?id=53338).

**Risoluzione:** Usare una delle soluzioni seguenti:

- Installare l'aggiornamento per Visual C++ 2013 e Visual C++ Redistributable Package ( [KB 3138367](https://support.microsoft.com/kb/3138367)). La soluzione nella KB è la soluzione consigliata. È possibile installarlo prima o dopo l'installazione di SQL Server 2016.

    Se è già installato SQL Server 2016, eseguire i passaggi seguenti nell'ordine indicato:

    1.  Scaricare il file *vcredist_\*exe*.
    1.  Arrestare il servizio SQL Server per tutte le istanze del motore di database.
    1.  Installare **KB 3138367**.
    1.  Riavviare il computer.


 - Installare l'aggiornamento critico per i prerequisiti della libreria MSVCRT per SQL Server 2016 (  [KB 3164398](https://support.microsoft.com/kb/3164398)).

    Se si usa **KB 3164398**, è possibile installare durante l'installazione di SQL Server, tramite Microsoft Update o dall'Area download Microsoft.

    - **Durante l'installazione di SQL Server 2016:** se il computer che esegue il programma di installazione di SQL Server ha accesso a Internet, l'aggiornamento viene cercato durante il processo di installazione di SQL Server. Se si accetta l'aggiornamento, i file binari vengono scaricati e aggiornati durante l'installazione.

    - **Microsoft Update:** l'aggiornamento è disponibile tramite Microsoft Update come aggiornamento critico per SQL Server 2016 non correlato alla sicurezza. Quando si esegue l'installazione tramite Microsoft Update, SQL Server 2016 richiede quindi il riavvio del server.

    - **Area download:** l'aggiornamento è infine disponibile nell'Area download Microsoft. È possibile scaricare il software per l'aggiornamento e installarlo nei server in cui è presente SQL Server 2016.


### <a name="stretch-database"></a><a name="bkmk_ga_stretch"></a>Stretch Database

#### <a name="problem-with-a-specific-character-in-a-database-or-table-name"></a>Problema con un carattere specifico in un nome di database o tabella

**Problema e impatto per i clienti:** se si tenta di abilitare Stretch Database in un database o in una tabella, si ottiene un errore. Il problema si verifica se il nome dell'oggetto include un carattere che viene considerato come un carattere diverso quando viene convertito da minuscolo a maiuscolo. Un esempio di carattere che causa questo problema è il carattere "ƒ" (creato digitando ALT+159).

**Soluzione alternativa:** se si vuole abilitare Stretch Database nel database o nella tabella, l'unica possibilità è rinominare l'oggetto e rimuovere il carattere che causa il problema.

#### <a name="problem-with-an-index-that-uses-the-include-keyword"></a>Problema relativo a un indice che usa la parola chiave INCLUDE

**Problema e impatto per i clienti:** se si tenta di abilitare Stretch Database in una tabella con un indice che usa la parola chiave INCLUDE per includere altre colonne nell'indice, l'operazione non riesce e viene visualizzato un errore.

**Soluzione alternativa:** eliminare l'indice che include la parola chiave INCLUDE, abilitare Stretch Database nella tabella e quindi ricreare l'indice. In questo caso, assicurarsi di seguire i criteri e le procedure di manutenzione dell'organizzazione per eliminare o ridurre al minimo l'impatto per gli utenti della tabella interessata.

### <a name="query-store"></a><a name="bkmk_ga_query_store"></a>Query Store

#### <a name="problem-with-automatic-data-cleanup-on-editions-other-than-enterprise-and-developer"></a>Problema relativo alla pulizia automatica dei dati nelle edizioni diverse da Enterprise e Developer

 **Problema e impatto per i clienti:** la pulizia automatica dei dati non riesce nelle edizioni diverse da Enterprise e Developer.
Di conseguenza, se i dati non vengono eliminati manualmente, lo spazio usato da Query Store aumenterà nel tempo fino a quando non viene raggiunto il limite configurato. Se non viene risolto, questo problema determinerà anche l'esaurimento dello spazio su disco allocato per i log degli errori, dato che a ogni tentativo di pulizia viene generato un file di dump. Il periodo di attivazione della pulizia dipende dalla frequenza del carico di lavoro, ma non supera i 15 minuti.

 **Soluzione alternativa:** se si prevede di usare Query Store in edizioni diverse da Enterprise e Developer, è necessario disattivare i criteri di pulizia in modo esplicito. Questa operazione si può eseguire da SQL Server Management Studio (pagina delle proprietà del database) oppure tramite script Transact-SQL:

```ALTER DATABASE <database name> SET QUERY_STORE (OPERATION_MODE = READ_WRITE, CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 0), SIZE_BASED_CLEANUP_MODE = OFF)```

Inoltre, prendere in considerazione le opzioni per la pulizia manuale per evitare che l'archivio query passi alla modalità di sola lettura. Ad esempio, eseguire periodicamente la query seguente per pulire l'intero spazio dati:

```ALTER DATABASE <database name> SET QUERY_STORE CLEAR```

Inoltre, eseguire periodicamente le stored procedure dell'archivio query per pulire statistiche di runtime, query specifiche o piani:

- `sp_query_store_reset_exec_stats`

- `sp_query_store_remove_plan`

- `sp_query_store_remove_query`


###  <a name="product-documentation-ga"></a><a name="bkmk_ga_docs"></a> Documentazione del prodotto (GA)
 **Problema e impatto per i clienti:** una versione scaricabile della documentazione di SQL Server 2016 non è ancora disponibile. Quando si usa Gestione librerie della Guida per provare a **installare contenuto online**, viene visualizzata la documentazione di SQL Server 2012 e SQL Server 2014, ma non è disponibile alcuna opzione per la documentazione di SQL Server 2016.

 **Soluzione alternativa:** usare una delle soluzioni alternative seguenti:

 ![Gestione impostazioni della Guida per SQL Server](../sql-server/media/docs-sql2016-managehelpsettings.png "Gestione impostazioni della Guida per SQL Server")

-   Usare l'opzione **Scegliere di utilizzare la Guida online o locale** e configurare la Guida per l'uso della Guida online.

-   Usare l'opzione **Installa contenuto da online** e scaricare i contenuti relativi a SQL Server 2014.

 **Guida sensibile al contesto:** in base alla progettazione, quando si preme F1 in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] viene visualizzata nel browser la versione online degli articoli della Guida sensibile al contesto. Il problema è costituito dalla visualizzazione della Guida basata su browser anche se è stata installata e configurata la Guida locale.

**Aggiornamento del contenuto:** In SQL Server Management Studio e Visual Studio l'applicazione Visualizzatore della Guida potrebbe smettere di rispondere durante l'aggiunta della documentazione. Per risolvere questo problema, eseguire la procedura seguente. Per altre informazioni su questo problema, vedere [Il visualizzatore della Guida di Visual Studio si blocca](/previous-versions/mt654096(v=vs.140)).

* Aprire il file %LOCALAPPDATA%\Microsoft\HelpViewer2.2\HlpViewer_SSMS16_en-US.settings | HlpViewer_VisualStudio14_en-US.settings nel Blocco note e impostare una data futura nel codice seguente.

```
     Cache LastRefreshed="12/31/2017 00:00:00"
```

## <a name="additional-information"></a>Informazioni aggiuntive
+ [Installazione di SQL Server 2016](../database-engine/install-windows/install-sql-server.md)
+ [SQL Server Update Center (Centro aggiornamenti di SQL Server): collegamenti e informazioni per tutte le versioni supportate](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png "MS_Logo_X-Small")
