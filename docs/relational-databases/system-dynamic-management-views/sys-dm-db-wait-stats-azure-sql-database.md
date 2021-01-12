---
description: sys.dm_db_wait_stats (Database di SQL Azure)
title: sys.dm_db_wait_stats (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_db_wait_stats_TSQL
- dm_db_wait_stats
- sys.dm_db_wait_stats
- sys.dm_db_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_wait_stats dynamic management view
- dm_db_wait_stats
ms.assetid: 00abd0a5-bae0-4d71-b173-f7a14cddf795
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 3d711b3eb0cb99c65582d8feacf8678fbca264c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092931"
---
# <a name="sysdm_db_wait_stats-azure-sql-database"></a>sys.dm_db_wait_stats (Database di SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce informazioni su tutte le attese rilevate dai thread eseguiti durante l'operazione. È possibile usare questa visualizzazione aggregata per diagnosticare problemi a livello di prestazioni in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e anche in query e batch specifici.  
  
 Tipi specifici di tempi di attesa durante l'esecuzione di query possono indicare colli di bottiglia oppure punti di stallo all'interno della query. In modo analogo, tempi di attesa o conteggi di attesa rilevanti a livello di server possono indicare colli di bottiglia o aree critiche nelle interazioni tra query all'interno dell'istanza del server. Ad esempio, le attese di blocco indicano contese a livello di dati da parte delle query, le attese di latch di I/O di pagina indicano tempi di risposta I/O bassi, mentre le attese di aggiornamento dei latch di pagina indicano layout di file errati.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Nome del tipo di attesa. Per ulteriori informazioni, vedere la sezione [Tipi di attesa](#WaitTypes) di seguito in questo argomento.|  
|waiting_tasks_count|**bigint**|Numero di attese del tipo specificato. Questo contatore viene incrementato all'inizio di ogni attesa.|  
|wait_time_ms|**bigint**|Tempo di attesa totale, espresso in millisecondi, per il tipo di attesa specifico. Il tempo comprende signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Tempo di attesa massimo per il tipo di attesa specifico.|  
|signal_wait_time_ms|**bigint**|Differenza tra il momento in cui è stato rilevato il thread in attesa e quello in cui è stata avviata l'esecuzione del thread.|  
  
## <a name="remarks"></a>Osservazioni  
  
-   In questa DMV vengono visualizzati solo i dati solo per il database corrente.  
  
-   In questa DMV è indicata la durata delle attese completate. Non sono indicate le attese correnti.  
  
-   I contatori vengono reimpostati su zero ogni volta che il database viene spostato o portato offline.  
  
-   Un thread di lavoro di SQL Server non viene considerato in attesa se una qualsiasi delle condizioni seguenti è vera:  
  
    -   Una risorsa diventa disponibile.  
  
    -   Una coda non è vuota.  
  
    -   Un processo esterno viene completato.  
  
-   Queste statistiche non vengono rese persistenti tra eventi di failover del database SQL e tutti i dati sono cumulativi dall'ultima reimpostazione delle statistiche.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il server.  
  
##  <a name="types-of-waits"></a><a name="WaitTypes"></a> Tipi di attese  
 Attesa di risorse  
 Questo tipo di attesa si verifica quando un thread di lavoro richiede l'accesso a una risorsa non disponibile perché usata da un altro thread di lavoro o perché non è ancora disponibile. Un esempio di attesa di risorse è rappresentato da blocchi, latch e attese di I/O su rete e su disco. Le attese blocchi e latch sono attese sugli oggetti di sincronizzazione.  
  
 Attesa di code  
 Questo tipo di attesa si verifica quando un thread di lavoro è inattivo ed è in attesa dell'assegnazione di lavoro. Le attese di code si verificano principalmente nell'ambito di attività di sistema in background quali, ad esempio, il monitoraggio dei deadlock e le attività di pulizia dei record eliminati. Queste attività attenderanno l'inserimento delle richieste di lavoro in una coda di elaborazione. È possibile che le attese di code diventino periodicamente attive anche se non sono stati inseriti nuovi pacchetti nella coda.  
  
 Attese esterne  
 Questo tipo di attesa si verifica quando un thread di lavoro di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in attesa del completamento di un evento esterno, ad esempio una chiamata di stored procedure estesa o una query di server collegato. Se vengono rilevati problemi di blocco, è opportuno ricordare che le attese esterne non sempre implicano che il thread di lavoro sia inattivo in quanto è possibile che il thread di lavoro esegua attivamente codice esterno.  
  
 Anche se il thread non è più in attesa, la sua esecuzione non deve necessariamente essere avviata subito perché tale thread viene innanzitutto inserito nella coda dei thread di lavoro eseguibili e pertanto deve attendere un quantum per essere eseguito nell'utilità di pianificazione.  
  
 Nei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contatori del tempo di attesa sono valori di **bigint** e pertanto non sono soggetti a rollover come contatori equivalenti nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Nella tabella seguente sono elencati i tipi di attesa rilevati dalle attività.  
  
|Tipo di attesa|Descrizione|  
|---------------|-----------------|  
|ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|ASSEMBLY_LOAD|Si verifica durante l'accesso esclusivo al caricamento di assembly.|  
|ASYNC_DISKPOOL_LOCK|Si verifica in caso di tentativo di sincronizzazione di thread paralleli che eseguono attività quali, ad esempio, la creazione o l'inizializzazione di un file.|  
|ASYNC_IO_COMPLETION|Si verifica quando un'attività è in attesa del completamento dell'I/O.|  
|ASYNC_NETWORK_IO|Si verifica durante le operazioni di scrittura in rete quando l'attività è bloccata in rete. Verificare che il client stia elaborando dati dal server.|  
|AUDIT_GROUPCACHE_LOCK|Si verifica in caso di attesa su un blocco che controlla accesso a una cache speciale. La cache contiene le informazioni sui controlli usati per controllare ogni gruppo di azioni di controllo.|  
|AUDIT_LOGINCACHE_LOCK|Si verifica in caso di attesa su un blocco che controlla accesso a una cache speciale. La cache contiene le informazioni sui controlli usati per controllare i gruppi di azioni di controllo accesso.|  
|AUDIT_ON_DEMAND_TARGET_LOCK|Si verifica in caso di attesa su un blocco usato per assicurare l'inizializzazione singola delle destinazioni degli eventi estesi relative ai controlli.|  
|AUDIT_XE_SESSION_MGR|Si verifica in caso di attesa su un blocco usato per sincronizzare l'avvio e l'arresto delle sessioni degli eventi estesi relative ai controlli.|  
|BACKUP|Si verifica quando un'attività è bloccata in quanto parte dell'elaborazione di un backup.|  
|BACKUP_OPERATOR|Si verifica quando un'attività è in attesa del montaggio del nastro. Per visualizzare lo stato del nastro, eseguire una query su [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md). Se un'operazione di montaggio non è in sospeso, questo tipo di attesa potrebbe indicare un problema a livello di hardware nell'unità nastro.|  
|BACKUPBUFFER|Si verifica quando un'attività di backup è in attesa di dati oppure di un buffer in cui archiviare dati. Questo tipo di attesa non è comune, tranne quando un'attività è in attesa del montaggio di un nastro.|  
|BACKUPIO|Si verifica quando un'attività di backup è in attesa di dati oppure di un buffer in cui archiviare dati. Questo tipo di attesa non è comune, tranne quando un'attività è in attesa del montaggio di un nastro.|  
|BACKUPTHREAD|Si verifica quando un'attività è in attesa del completamento di un'attività di backup. I tempi di attesa possono essere lunghi, da alcuni minuti a parecchie ore. Se l'attività per la quale si è verificata l'attesa è un processo di I/O, questo tipo di attesa non indica un problema.|  
|BAD_PAGE_PROCESS|Si verifica quando il logger in background delle pagine sospette tenta di evitare l'esecuzione con una frequenza superiore a 5 secondi. Un numero eccessivo di pagine sospette determina una frequente esecuzione del logger.|  
|BROKER_CONNECTION_RECEIVE_TASK|Si verifica durante l'attesa dell'accesso per la ricezione di un messaggio su un endpoint della connessione. L'accesso per la ricezione all'endpoint viene serializzato.|  
|BROKER_ENDPOINT_STATE_MUTEX|Si verifica in caso di contesa per l'accesso allo stato di un endpoint della connessione di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. L'accesso allo stato per le modifiche viene serializzato.|  
|BROKER_EVENTHANDLER|Si verifica quando un'attività è in attesa del gestore di eventi primario di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Questo tipo di attesa si verifica per brevissimi periodi.|  
|BROKER_INIT|Si verifica durante l'inizializzazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] in ciascun database attivo. Questo tipo di attesa si verifica raramente.|  
|BROKER_MASTERSTART|Si verifica quando un'attività è in attesa dell'avvio del gestore di eventi primario di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Questo tipo di attesa si verifica per brevissimi periodi.|  
|BROKER_RECEIVE_WAITFOR|Si verifica quando l'istruzione RECEIVE WAITFOR è in attesa. Questo tipo di attesa è comune se nessun messaggio è pronto per la ricezione.|  
|BROKER_REGISTERALLENDPOINTS|Si verifica durante l'inizializzazione di un endpoint della connessione di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Questo tipo di attesa si verifica per brevissimi periodi.|  
|BROKER_SERVICE|Si verifica quando l'elenco delle destinazioni [!INCLUDE[ssSB](../../includes/sssb-md.md)] associato a un servizio di destinazione viene aggiornato o riordinato.|  
|BROKER_SHUTDOWN|Si verifica in caso di chiusura pianificata di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Questo tipo di attesa si verifica saltuariamente ed eventualmente per brevissimi periodi.|  
|BROKER_TASK_STOP|Si verifica quando il gestore delle attività di coda [!INCLUDE[ssSB](../../includes/sssb-md.md)] tenta di chiudere l'attività. Il controllo di stato è serializzato e deve essere in uno stato di esecuzione prima dell'operazione.|  
|BROKER_TO_FLUSH|Si verifica quando lo svuotamento lento di [!INCLUDE[ssSB](../../includes/sssb-md.md)] svuota gli oggetti di trasmissione in memoria in una tabella di lavoro.|  
|BROKER_TRANSMITTER|Si verifica quando lo strumento di trasmissione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è in attesa di lavoro.|  
|BUILTIN_HASHKEY_MUTEX|Può verificarsi dopo l'avvio dell'istanza, durante l'inizializzazione delle strutture di dati interne. Non si ripete dopo l'inizializzazione delle strutture di dati.|  
|CHECK_PRINT_RECORD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|CHECKPOINT_QUEUE|Si verifica quando l'attività di checkpoint è in attesa della successiva richiesta di checkpoint.|  
|CHKPT|Si verifica all'avvio del server per indicare il thread di gestione dei checkpoint che è possibile avviare.|  
|CLEAR_DB|Si verifica durante le operazioni che modificano lo stato di un database, ad esempio l'apertura o la chiusura.|  
|CLR_AUTO_EVENT|Si verifica quando un'attività sta effettuando un'esecuzione CLR (Common Language Runtime) ed è in attesa dell'inizializzazione di un evento automatico specifico. In genere si verificano attese prolungate, che tuttavia non indicano un problema.|  
|CLR_CRST|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di iniziare una parte critica dell'attività usata da un'altra attività.|  
|CLR_JOIN|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa del completamento di un'altra attività. Questo stato di attesa si verifica quando è presente un join tra attività.|  
|CLR_MANUAL_EVENT|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa dell'inizializzazione di un evento manuale specifico.|  
|CLR_MEMORY_SPY|Si verifica durante l'attesa dell'acquisizione del blocco per una struttura di dati usata per registrare tutte le allocazioni della memoria virtuale che provengono da CLR. La struttura di dati è bloccata per gestire l'integrità se l'accesso è parallelo.|  
|CLR_MONITOR|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di acquisire un blocco per il monitoraggio.|  
|CLR_RWLOCK_READER|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di un blocco del reader.|  
|CLR_RWLOCK_WRITER|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di un blocco del writer.|  
|CLR_SEMAPHORE|Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di un semaforo.|  
|CLR_TASK_START|Si verifica durante l'attesa del completamento dell'avvio di un'attività CLR.|  
|CLRHOST_STATE_ACCESS|Si verifica in caso di attesa per l'acquisizione dell'accesso esclusivo alle strutture di dati di hosting CLR. Questo tipo di attesa si verifica durante la configurazione o la rimozione del runtime CLR.|  
|CMEMTHREAD|Si verifica quando un'attività è in attesa di un oggetto memoria affidabile. Il tempo di attesa potrebbe aumentare in caso di contesa causata da più attività che tentano di allocare memoria dallo stesso oggetto memoria.|  
|CXPACKET|Si verifica quando si cerca di sincronizzare l'iteratore di scambio di Query Processor. È possibile valutare la possibilità di ridurre il grado di parallelismo se la contesa per questo tipo di attesa costituisce un problema.|  
|CXROWSET_SYNC|Si verifica durante un'analisi di intervalli parallela.|  
|DAC_INIT|Si verifica durante l'inizializzazione della connessione amministrativa dedicata.|  
|DBMIRROR_DBM_EVENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_DBM_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_EVENTS_QUEUE|Si verifica quando il mirroring del database è in attesa di eventi da elaborare.|  
|DBMIRROR_SEND|Si verifica quando un'attività è in attesa della cancellazione di un backlog delle comunicazioni nel livello rete per essere in grado di inviare messaggi. Indica che nel livello comunicazioni si sta verificando un overload che può pregiudicare la velocità effettiva dei dati di mirroring del database.|  
|DBMIRROR_WORKER_QUEUE|Indica che l'attività di lavoro del mirroring del database è in attesa di ulteriore lavoro.|  
|DBMIRRORING_CMD|Si verifica quando un'attività è in attesa dello scaricamento su disco dei record di log. Questo stato di attesa viene in genere mantenuto per lunghi periodi di tempo.|  
|DEADLOCK_ENUM_MUTEX|Si verifica quando il monitoraggio dei deadlock e sys.dm_os_waiting_tasks cercano di garantire che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non esegua più ricerche di deadlock contemporaneamente.|  
|DEADLOCK_TASK_SEARCH|Un tempo di attesa elevato per questa risorsa indica che il server sta eseguendo query su sys.dm_os_waiting_tasks e che tali query bloccano l'esecuzione della ricerca di deadlock nell'ambito del monitoraggio dei deadlock. Questo tipo di attesa viene usato soltanto dalla funzionalità di monitoraggio dei deadlock. Le query su sys.dm_os_waiting_tasks utilizzano DEADLOCK_ENUM_MUTEX.|  
|DEBUG|Si verifica durante il debug CLR e [!INCLUDE[tsql](../../includes/tsql-md.md)] per la sincronizzazione interna.|  
|DISABLE_VERSIONING|Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue il polling della gestione delle transazioni di versione per verificare se il timestamp della transazione attiva meno recente è successivo al timestamp relativo al momento in cui lo stato è stato modificato. In questo caso, vengono completate tutte le transazioni snapshot avviate prima dell'esecuzione dell'istruzione ALTER DATABASE. Questo stato di attesa viene utilizzando quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disabilita il controllo delle versioni tramite l'istruzione ALTER DATABASE.|  
|DISKIO_SUSPEND|Si verifica quando un'attività è in attesa di accedere a un file quando è attivo un backup esterno. Questo tipo di attesa viene segnalato per ogni processo utente in attesa. Un conteggio maggiore di 5 per processo utente può indicare che il completamento del backup esterno sta richiedendo troppo tempo.|  
|DISPATCHER_QUEUE_SEMAPHORE|Si verifica quando un thread del pool di dispatcher è in attesa di più lavori da elaborare. Si prevede che il tempo di attesa per questo tipo di attesa aumenti quando il dispatcher è inattivo.|  
|DLL_LOADING_MUTEX|Si verifica una volta durante l'attesa del caricamento della DLL del parser XML.|  
|DROPTEMP|Si verifica tra diversi tentativi di eliminare un oggetto temporaneo, se il tentativo precedente ha avuto esito negativo. La durata dell'attesa aumenta in modo esponenziale con ogni tentativo di eliminazione non riuscito.|  
|DTC|Si verifica quando un'attività è in attesa di un evento usato per gestire la transizione di stato. Questo stato controlla se il recupero delle transazioni [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) si verifica dopo la ricezione da parte di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] della notifica indicante che il servizio MS DTC non è disponibile.<br /><br /> Questo stato descrive inoltre un'attività in attesa quando un commit di una transazione MS DTC viene avviato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in attesa del completamento del commit MS DTC.|  
|DTC_ABORT_REQUEST|Si verifica in una sessione di lavoro MS DTC quando la sessione è in attesa di acquisire la proprietà di una transazione MS DTC. Non appena MS DTC acquisisce la proprietà della transazione, la sessione può eseguire il rollback della transazione. In genere, la sessione attenderà un'altra sessione che sta utilizzando la transazione.|  
|DTC_RESOLVE|Si verifica quando un'attività di recupero è in attesa del database master in una transazione tra database in modo che l'attività possa eseguire una query sul risultato della transazione.|  
|DTC_STATE|Si verifica quando un'attività è in attesa di un evento che impedisce le modifiche all'oggetto stato globale MS DTC interno. Questo stato deve essere mantenuto per brevissimi periodi di tempo.|  
|DTC_TMDOWN_REQUEST|Si verifica in una sessione di lavoro MS DTC quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve una notifica indicante che il servizio MS DTC non è disponibile. Il thread di lavoro attenderà innanzitutto l'avvio del processo di recupero MS DTC. Il thread di lavoro attende quindi di ottenere il risultato della transazione distribuita su cui sta lavorando. Ciò potrebbe continuare finché non viene ristabilita la connessione al servizio MS DTC.|  
|DTC_WAITFOR_OUTCOME|Si verifica quando le attività di recupero attendono l'attivazione di MS DTC per consentire la risoluzione delle transazioni preparate.|  
|DUMP_LOG_COORDINATOR|Si verifica quando un'attività principale è in attesa che una sottoattività generi dati. In genere, questo stato non si verifica mai. Un tempo di attesa prolungato indica un blocco imprevisto. È pertanto necessario verificare la sottoattività.|  
|DUMPTRIGGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EE_PMOLOCK|Si verifica durante la sincronizzazione di determinati tipi di allocazioni di memoria nel corso dell'esecuzione di istruzioni.|  
|EE_SPECPROC_MAP_INIT|Si verifica durante la sincronizzazione della creazione della tabella hash delle procedure interne. Questa attesa può verificarsi solo durante l'accesso iniziale alla tabella hash dopo l'avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|ENABLE_VERSIONING|Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attende il completamento di tutte le transazioni di aggiornamento nel database prima di dichiarare il database pronto per la transizione allo stato in cui è consentito l'isolamento dello snapshot. Questo stato viene usato quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilita l'isolamento dello snapshot tramite l'istruzione ALTER DATABASE.|  
|ERROR_REPORTING_MANAGER|Si verifica durante la sincronizzazione delle inizializzazioni di più log degli errori simultanei.|  
|EXCHANGE|Si verifica durante la sincronizzazione nell'iteratore di scambio di Query Processor nel corso di query parallele.|  
|EXECSYNC|Si verifica durante query parallele nel corso della sincronizzazione in Query Processor in aree non correlate all'iteratore di scambio. Tali aree sono, ad esempio, bitmap, oggetti BLOB (Binary Large Object) e l'iteratore di spool. È possibile che gli oggetti BLOB utilizzino di frequente questo stato di attesa.|  
|EXECUTION_PIPE_EVENT_INTERNAL|Si verifica durante la sincronizzazione tra parti del produttore e dell'utente dell'esecuzione batch inviate tramite il contesto di connessione.|  
|FAILPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FCB_REPLICA_READ|Si verifica quando le letture di un file sparse snapshot oppure di uno snapshot temporaneo creato da DBCC vengono sincronizzate.|  
|FCB_REPLICA_WRITE|Si verifica durante la sincronizzazione di operazioni di push o pull di una pagina in un file sparse di uno snapshot oppure di uno snapshot temporaneo creato da DBCC.|  
|FS_FC_RWLOCK|Si verifica in caso di attesa del Garbage Collector di FILESTREAM per le seguenti operazioni:<br /><br /> Disabilitare Garbage Collection (usato da backup e ripristino).<br /><br /> Eseguire un ciclo del Garbage Collector di FILESTREAM.|  
|FS_GARBAGE_COLLECTOR_SHUTDOWN|Si verifica quando il Garbage Collector di FILESTREAM attende il completamento delle attività di pulizia.|  
|FS_HEADER_RWLOCK|Si verifica in caso di attesa per l'accesso all'intestazione FILESTREAM di un contenitore di dati FILESTREAM per leggere o aggiornare il contenuto nel file di intestazione (Filestream.hdr) di FILESTREAM.|  
|FS_LOGTRUNC_RWLOCK|Si verifica in caso di attesa per l'accesso al troncamento del log di FILESTREAM per le seguenti operazioni:<br /><br /> Disabilitare temporaneamente il troncamento del log (FSLOG) di FILESTREAM (usato da backup e ripristino).<br /><br /> Eseguire un ciclo del troncamento di FSLOG.|  
|FSA_FORCE_OWN_XACT|Si verifica quando un'operazione di I/O del file FILESTREAM deve essere associata alla transazione associata, ma la transazione appartiene attualmente a un'altra sessione.|  
|FSAGENT|Si verifica quando un'operazione di I/O del file FILESTREAM attende una risorsa dell'agente di FILESTREAM usata da un'altra operazione di I/O di file.|  
|FSTR_CONFIG_MUTEX|Si verifica in caso di attesa per il completamento di un'altra riconfigurazione di funzionalità di FILESTREAM.|  
|FSTR_CONFIG_RWLOCK|Si verifica in caso di attesa per serializzare l'accesso ai parametri di configurazione di FILESTREAM.|  
|FT_METADATA_MUTEX|Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|  
|FT_RESTART_CRAWL|Si verifica quando è necessario riavviare una ricerca per indicizzazione full-text dall'ultimo punto valido noto per correggere un errore temporaneo. L'attesa consente alle attività di lavoro usate per il popolamento di completare il passaggio corrente o uscirne.|  
|FULLTEXT GATHERER|Si verifica durante la sincronizzazione delle operazioni full-text.|  
|GUARDIAN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|HTTP_ENUMERATION|Si verifica all'avvio per enumerare gli endpoint HTTP per avviare HTTP.|  
|HTTP_START|Si verifica quando una connessione è in attesa che HTTP completi l'inizializzazione.|  
|IMPPROV_IOWAIT|Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attende il completamento di un I/O di caricamento bulk.|  
|INTERNAL_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|IO_AUDIT_MUTEX|Si verifica durante la sincronizzazione di buffer di eventi di traccia.|  
|IO_COMPLETION|Si verifica durante l'attesa del completamento di operazioni di I/O. Questo tipo di attesa rappresenta in genere operazioni di I/O su pagine non di dati. Le attese per il completamento di operazioni di I/O su pagine di dati vengono visualizzate come attese PAGEIOLATCH_*.|  
|IO_QUEUE_LIMIT|Si verifica quando la coda di IO asincroni per il [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] contiene troppi IO in sospeso. Le attività che provano a rilasciare un altro IO vengono bloccate in questo tipo di attesa fino a quando il numero di IO in sospeso non scende sotto la soglia. La soglia è proporzionale alle DTU assegnate al database.|  
|IO_RETRY|Si verifica quando un'operazione di I/O, quale un'operazione di lettura o scrittura sul disco, non riesce a causa di risorse insufficienti e viene ritentata l'operazione.|  
|IOAFF_RANGE_QUEUE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KSOURCE_WAKEUP|Viene usato dall'attività di controllo dei servizi durante l'attesa di richieste da Gestione controllo servizi. Sono previste attese prolungate, che non indicano un problema.|  
|KTM_ENLISTMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_MANAGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_RESOLUTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_DT|Si verifica durante l'attesa di un latch di eliminazione (DT). Non include i latch del buffer o i latch di contrassegno di transazione. In sys.dm_os_latch_stats è disponibile un elenco delle attese LATCH_*. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.|  
|LATCH_EX|Si verifica durante l'attesa di un latch esclusivo (EX). Non include i latch del buffer o i latch di contrassegno di transazione. In sys.dm_os_latch_stats è disponibile un elenco delle attese LATCH_*. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.|  
|LATCH_KP|Si verifica durante l'attesa di un latch conservativo (KP). Non include i latch del buffer o i latch di contrassegno di transazione. In sys.dm_os_latch_stats è disponibile un elenco delle attese LATCH_*. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.|  
|LATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_SH|Si verifica durante l'attesa di un latch di condivisione (SH). Non include i latch del buffer o i latch di contrassegno di transazione. In sys.dm_os_latch_stats è disponibile un elenco delle attese LATCH_*. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.|  
|LATCH_UP|Si verifica durante l'attesa di un latch di aggiornamento (UP). Non include i latch del buffer o i latch di contrassegno di transazione. In sys.dm_os_latch_stats è disponibile un elenco delle attese LATCH_*. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.|  
|LAZYWRITER_SLEEP|Si verifica quando le attività Lazywriter vengono sospese. Si tratta di una misura della durata dell'attesa delle attività in background. Non considerare questo stato durante il rilevamento di stalli a livello di utente.|  
|LCK_M_BU|Si verifica quando un'attività è in attesa di acquisire un blocco aggiornamenti bulk (BU). Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IS|Si verifica quando un'attività è in attesa di acquisire un blocco preventivo condiviso (IS). Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IU|Si verifica quando un'attività è in attesa di acquisire un blocco preventivo di aggiornamento (IU). Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IX|Si verifica quando un'attività è in attesa di acquisire un blocco preventivo esclusivo (IX). Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_NL|Si verifica quando un'attività è in attesa di acquisire un blocco NULL per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente. Un blocco NULL per la chiave è un blocco a rilascio immediato. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_S|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_U|Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_X|Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RS_S|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso per il valore di chiave corrente e un blocco di intervallo condiviso tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RS_U|Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento per il valore di chiave corrente e un blocco di intervallo di aggiornamento tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_S|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso per il valore di chiave corrente e un blocco di intervallo esclusivo tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_U|Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento per il valore di chiave corrente e un blocco di intervallo esclusivo tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_X|Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo per il valore di chiave corrente e un blocco di intervallo esclusivo tra la chiave corrente e quella precedente. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_S|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SCH_M|Si verifica quando un'attività è in attesa di acquisire un blocco di modifica dello schema. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SCH_S|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso dello schema. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SIU|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo di aggiornamento. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SIX|Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo esclusivo. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_U|Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_UIX|Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento preventivo esclusivo. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_X|Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo. Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LOG_RATE_GOVERNOR|Si verifica quando il DB è in attesa della quota da scrivere nel log.|  
|LOGBUFFER|Si verifica quando un'attività è in attesa di spazio nel buffer del log per l'archiviazione di un record di log. Valori costantemente alti possono indicare che i dispositivi di log non sono in grado di far fronte alla quantità di log generati dal server.|  
|LOGGENERATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR|Si verifica quando un'attività è in attesa del completamento di qualsiasi operazione di I/O di log in sospeso prima di chiudere il log durante la chiusura del database.|  
|LOGMGR_FLUSH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR_QUEUE|Si verifica quando l'attività di scrittura del log è in attesa di richieste di lavoro.|  
|LOGMGR_RESERVE_APPEND|Si verifica quando un'attività è in attesa di verificare se il troncamento del log libera spazio di log per consentire all'attività di scrivere un nuovo record di log. Valutare la possibilità di aumentare le dimensioni dei file di log per il database interessato allo scopo di ridurre l'attesa.|  
|LOWFAIL_MEMMGR_QUEUE|Si verifica durante l'attesa di memoria disponibile per l'utilizzo.|  
|MSQL_DQ|Si verifica quando un'attività è in attesa del completamento di un'operazione di query distribuita. Viene usato per rilevare potenziali deadlock di applicazioni MARS (Multiple Active Result Set). L'attesa si arresta al completamento della chiamata della query distribuita.|  
|MSQL_XACT_MGR_MUTEX|Si verifica quando un'attività è in attesa di acquisire la proprietà della gestione transazioni della sessione per eseguire un'operazione di transazione a livello di sessione.|  
|MSQL_XACT_MUTEX|Si verifica durante la sincronizzazione dell'utilizzo della transazione. Per poter usare la transazione, una richiesta deve prima acquisire il mutex.|  
|MSQL_XP|Si verifica quando un'attività è in attesa del completamento di una stored procedure estesa. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza questo stato di attesa per rilevare potenziali deadlock di applicazioni MARS. L'attesa si arresta al completamento della chiamata della stored procedure estesa.|  
|MSSEARCH|Si verifica durante le chiamate di ricerca full-text. L'attesa si arresta al completamento dell'operazione full-text. Non indica contesa, bensì la durata delle operazioni full-text.|  
|NET_WAITFOR_PACKET|Si verifica quando una connessione è in attesa di un pacchetto di rete durante una lettura in rete.|  
|OLEDB|Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiama il provider OLE DB per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Questo tipo di attesa non viene usato per la sincronizzazione. Indica invece la durata delle chiamate al provider OLE DB.|  
|ONDEMAND_TASK_QUEUE|Si verifica quando un'attività in background è in attesa di richieste di attività di sistema con priorità elevata. Tempi di attesa prolungati indicano l'assenza di richieste con priorità elevata da elaborare e non costituiscono un problema.|  
|PAGEIOLATCH_DT|Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità di eliminazione. Attese prolungate possono indicare problemi con il sottosistema disco.|  
|PAGEIOLATCH_EX|Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità esclusiva. Attese prolungate possono indicare problemi con il sottosistema disco.|  
|PAGEIOLATCH_KP|Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità di mantenimento. Attese prolungate possono indicare problemi con il sottosistema disco.|  
|PAGEIOLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGEIOLATCH_SH|Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità condivisa. Attese prolungate possono indicare problemi con il sottosistema disco.|  
|PAGEIOLATCH_UP|Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità di aggiornamento. Attese prolungate possono indicare problemi con il sottosistema disco.|  
|PAGELATCH_DT|Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità di eliminazione.|  
|PAGELATCH_EX|Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità esclusiva.|  
|PAGELATCH_KP|Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità di mantenimento.|  
|PAGELATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGELATCH_SH|Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità condivisa.|  
|PAGELATCH_UP|Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità di aggiornamento.|  
|PARALLEL_BACKUP_QUEUE|Si verifica durante la serializzazione dell'output generato da RESTORE HEADERONLY, RESTORE FILELISTONLY o RESTORE LABELONLY.|  
|PREEMPTIVE_ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG|Si verifica quando l'utilità di pianificazione del sistema operativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQLOS) passa alla modalità preemptive per scrivere un evento di controllo nel registro eventi di Windows.|  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG|Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per scrivere un evento di controllo nel registro di sicurezza di Windows.|  
|PREEMPTIVE_CLOSEBACKUPMEDIA|Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per chiudere i supporti di backup.|  
|PREEMPTIVE_CLOSEBACKUPTAPE|Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per chiudere un dispositivo di backup su nastro.|  
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE|Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per chiudere un dispositivo di backup virtuale.|  
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL|Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per eseguire operazioni di cluster di failover di Windows.|  
|PREEMPTIVE_COM_COCREATEINSTANCE|Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per creare un oggetto COM.|  
|PREEMPTIVE_HADR_LEASE_MECHANISM|Pianificazione della gestione dei lease dei gruppi di disponibilità Always On per la diagnostica CSS.|  
|PREEMPTIVE_SOSTESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_STRESSDRIVER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_XETESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PRINT_ROLLBACK_PROGRESS|Utilizzato per l'attesa durante la conclusione di processi utente in un database in cui si è verificata una transizione tramite la clausola di terminazione ALTER DATABASE. Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).|  
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC|Si verifica quando un'attività in background è in attesa del completamento dell'attività in background mediante cui vengono ricevute (tramite polling) notifiche di Windows Server Failover Clustering.  Solo per uso interno.|  
|PWAIT_HADR_CLUSTER_INTEGRATION|Un'operazione di Accodamento, sostituzione e/o rimozione è in attesa di acquisire un blocco di scrittura su un Always On elenco interno, ad esempio un elenco di reti, indirizzi di rete o listener del gruppo di disponibilità.  Solo per uso interno.|  
|PWAIT_HADR_OFFLINE_COMPLETED|Un'operazione di eliminazione del gruppo di disponibilità Always On è in attesa del passaggio alla modalità offline del gruppo di disponibilità di destinazione prima dell'eliminazione degli oggetti di Windows Server failover clustering.|  
|PWAIT_HADR_ONLINE_COMPLETED|Un'operazione di creazione o failover di un gruppo di disponibilità Always On è in attesa che il gruppo di disponibilità di destinazione torni in linea.|  
|PWAIT_HADR_POST_ONLINE_COMPLETED|Un'operazione di eliminazione del gruppo di disponibilità Always On è in attesa della chiusura di qualsiasi attività in background pianificata come parte di un comando precedente. Ad esempio, è possibile che sia in corso la transizione al ruolo primario di database di disponibilità di un'attività in background. Per evitare situazioni di race condition è necessaria l'attesa del completamento dell'attività in background da parte di DROP AVAILABILITY GROUP DDL.|  
|PWAIT_HADR_WORKITEM_COMPLETED|Attesa interna da un thread in attesa del completamento di un'attività di lavoro asincrona. Si tratta di un'attesa prevista e consente l'utilizzo di CSS.|  
|PWAIT_MD_LOGIN_STATS|Si verifica durante la sincronizzazione interna nei metadati per le statistiche di accesso.|  
|PWAIT_MD_RELATION_CACHE|Si verifica durante la sincronizzazione interna nei metadati per la tabella o l'indice.|  
|PWAIT_MD_SERVER_CACHE|Si verifica durante la sincronizzazione interna nei metadati per i server collegati.|  
|PWAIT_MD_UPGRADE_CONFIG|Si verifica durante la sincronizzazione interna nell'aggiornamento delle configurazioni per l'intero server.|  
|PWAIT_METADATA_LAZYCACHE_RWLOCk|Si verifica durante la sincronizzazione interna nella cache dei metadati insieme all'interazione dell'indice o delle statistiche in una tabella.|  
|QPJOB_KILL|Indica che un aggiornamento automatico asincrono delle statistiche è stato annullato da una chiamata al comando KILL all'avvio dell'esecuzione dell'aggiornamento. Il thread di interruzione viene sospeso e rimarrà in attesa dei comandi KILL. Un valore ottimale è minore di un secondo.|  
|QPJOB_WAITFOR_ABORT|Indica che un aggiornamento automatico asincrono delle statistiche è stato annullato da una chiamata al comando KILL durante l'esecuzione. L'aggiornamento è completato ma viene sospeso fino al completamento del coordinamento dei messaggi dei thread di interruzione. Questo stato è comune ma si verifica raramente e dovrebbe essere molto breve. Un valore ottimale è minore di un secondo.|  
|QRY_MEM_GRANT_INFO_MUTEX|Si verifica quando la gestione della memoria dell'esecuzione di query cerca di controllare l'accesso all'elenco di informazioni statiche sulle concessioni. Questo stato elenca le informazioni sulle richieste di memoria correnti concesse e in attesa. Questo stato rappresenta un semplice stato di controllo dell'accesso. In questo stato non si dovrebbero mai verificare attese lunghe. Se questo mutex non viene rilasciato, tutte le nuove query che utilizzano memoria non risponderanno più.|  
|QUERY_ERRHDL_SERVICE_DONE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN|Si verifica in determinati casi in cui la compilazione di indici offline viene eseguita in parallelo e i diversi thread di lavoro in cui viene eseguito l'ordinamento sincronizzano l'accesso ai file di ordinamento.|  
|QUERY_NOTIFICATION_MGR_MUTEX|Si verifica durante la sincronizzazione della coda di Garbage Collection nell'utilità di gestione delle notifiche delle query.|  
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX|Si verifica durante la sincronizzazione dello stato per le transazioni nelle notifiche delle query.|  
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX|Si verifica durante la sincronizzazione interna nell'utilità di gestione delle notifiche delle query.|  
|QUERY_NOTIFICATION_UNITTEST_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_OPTIMIZER_PRINT_MUTEX|Si verifica durante la sincronizzazione della produzione di output di dati diagnostici di Query Optimizer. Questo tipo di attesa si verifica solo se sono state abilitate impostazioni di diagnostica su richiesta del Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|QUERY_TRACEOUT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_WAIT_ERRHDL_SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|RECOVER_CHANGEDB|Si verifica durante la sincronizzazione dello stato del database in modalità standby a caldo (warm standby).|  
|REPL_CACHE_ACCESS|Si verifica durante la sincronizzazione in una cache degli articoli di replica. Durante queste attese si riscontra uno stallo nella lettura del log delle repliche e il blocco delle istruzioni DDL (Data Definition Language) per una tabella pubblicata.|  
|REPL_SCHEMA_ACCESS|Si verifica durante la sincronizzazione delle informazioni sulla versione dello schema di replica. Questo stato esiste quando vengono eseguite istruzioni DDL sull'oggetto replicato e quando nella lettura del log viene compilato o usato uno schema con versione basato sull'occorrenza DDL.|  
|REPLICA_WRITES|Si verifica quando un'attività è in attesa del completamento di scritture di pagina in snapshot di database o repliche DBCC.|  
|REQUEST_DISPENSER_PAUSE|Si verifica quando un'attività è in attesa del completamento di tutte le operazioni di I/O in sospeso, per poter bloccare l'I/O in un file per il backup snapshot.|  
|REQUEST_FOR_DEADLOCK_SEARCH|Si verifica quando il monitoraggio dei deadlock è in attesa di avviare la successiva ricerca di deadlock. L'attesa tra rilevamenti di deadlock è prevista e un tempo di attesa totale prolungato per questa risorsa non indica un problema.|  
|RESMGR_THROTTLED|Si verifica quando giunge una nuova richiesta rallentata in base all'impostazione GROUP_MAX_REQUESTS.|  
|RESOURCE_QUEUE|Si verifica durante la sincronizzazione di diverse code di risorse interne.|  
|RESOURCE_SEMAPHORE|Si verifica quando una richiesta di memoria per una query non può essere concessa immediatamente a causa di altre query simultanee. Attese e tempi di attesa rilevanti possono indicare un numero eccessivo di query simultanee o quantità eccessive di richieste di memoria.|  
|RESOURCE_SEMAPHORE_MUTEX|Si verifica quando una query è in attesa che venga soddisfatta la richiesta di prenotazione di thread, nonché durante la sincronizzazione di richieste di compilazione di query e concessione di memoria.|  
|RESOURCE_SEMAPHORE_QUERY_COMPILE|Si verifica quando il numero di compilazioni di query simultanee raggiunge un limite massimo. Attese e tempi di attesa rilevanti possono indicare una quantità eccessiva di compilazioni, ricompilazioni o piani non memorizzabili nella cache.|  
|RESOURCE_SEMAPHORE_SMALL_QUERY|Si verifica quando una richiesta di memoria per una query di dimensioni ridotte non può essere concessa immediatamente a causa di altre query simultanee. Il tempo di attesa non deve superare pochi secondi, poiché se non è in grado di concedere la memoria richiesta entro pochi secondi il server trasferisce la richiesta al pool di memoria per query principale. Attese rilevanti possono indicare un numero eccessivo di query di dimensioni ridotte simultanee mentre il pool di memoria principale è bloccato da query in attesa.|  
|SE_REPL_CATCHUP_THROTTLE|Si verifica quando la transazione è in attesa che uno dei database secondari compia un avanzamento.|  
|SE_REPL_COMMIT_ACK|Si verifica quando la transazione è in attesa dell'acknowledgement per un commit del quorum da repliche secondarie.|  
|SE_REPL_COMMIT_TURN|Si verifica quando la transazione è in attesa del commit dopo aver ricevuto acknowledgement per un commit del quorum.|  
|SE_REPL_ROLLBACK_ACK|Si verifica quando la transazione è in attesa dell'acknowledgement per un rollback del quorum da repliche secondarie.|  
|SE_REPL_SLOW_SECONDARY_THROTTLE|Si verifica quando il thread è in attesa di una delle repliche secondarie del database.|  
|SEC_DROP_TEMP_KEY|Si verifica dopo un tentativo non riuscito di eliminare una chiave di sicurezza temporanea, prima di un nuovo tentativo.|  
|SECURITY_MUTEX|Si verifica in caso di attesa dei mutex che controllano l'accesso all'elenco globale dei provider di crittografia EKM e l'elenco con ambito sessione di sessioni EKM.|  
|SEQUENTIAL_GUID|Si verifica durante l'ottenimento di un nuovo GUID sequenziale.|  
|SERVER_IDLE_CHECK|Si verifica durante la sincronizzazione dello stato di inattività dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quando un monitoraggio risorse tenta di dichiarare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come inattiva o in corso di attivazione.|  
|SHUTDOWN|Si verifica quando un'istruzione di chiusura è in attesa dell'interruzione delle connessioni attive.|  
|SLEEP_BPOOL_FLUSH|Si verifica quando un checkpoint limita il rilascio di nuove operazioni di I/O per evitare il sovraccarico del sottosistema disco.|  
|SLEEP_DBSTARTUP|Si verifica durante l'avvio di database, in attesa del recupero di tutti i database.|  
|SLEEP_DCOMSTARTUP|Si verifica al massimo una volta durante l'avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in attesa del completamento dell'inizializzazione di DCOM.|  
|SLEEP_MSDBSTARTUP|Si verifica quando Traccia SQL è in attesa del completamento dell'avvio del database msdb.|  
|SLEEP_SYSTEMTASK|Si verifica durante l'avvio di un'attività in background, in attesa che venga completato l'avvio di tempdb.|  
|SLEEP_TASK|Si verifica quando un'attività viene sospesa in attesa di un evento generico.|  
|SLEEP_TEMPDBSTARTUP|Si verifica quando un'attività è in attesa del completamento dell'avvio di tempdb.|  
|SNI_CRITICAL_SECTION|Si verifica durante la sincronizzazione interna nei componenti di rete di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SNI_HTTP_WAITFOR_0_DISCON|Si verifica durante l'arresto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in attesa della chiusura delle connessioni HTTP in sospeso.|  
|SNI_LISTENER_ACCESS|Si verifica durante l'attesa dei nodi NUMA (Non-Uniform Memory Access) per aggiornare il cambiamento dello stato. L'accesso alla modifica dello stato è serializzato.|  
|SNI_TASK_COMPLETION|Si verifica durante l'attesa del completamento di tutte le attività durante il cambiamento di stato di un nodo NUMA.|  
|SOAP_READ|Si verifica durante l'attesa del completamento di una lettura in rete HTTP.|  
|SOAP_WRITE|Si verifica durante l'attesa del completamento di una scrittura in rete HTTP.|  
|SOS_CALLBACK_REMOVAL|Si verifica durante l'esecuzione della sincronizzazione in un elenco di callback allo scopo di rimuovere un callback. Dopo il completamento dell'inizializzazione del server non è prevista alcuna modifica di questo contatore.|  
|SOS_DISPATCHER_MUTEX|Si verifica durante la sincronizzazione interna del pool di dispatcher e quando il pool viene modificato.|  
|SOS_LOCALALLOCATORLIST|Si verifica durante la sincronizzazione interna nel gestore della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOS_MEMORY_USAGE_ADJUSTMENT|Si verifica quando l'utilizzo della memoria è regolato tra i pool.|  
|SOS_OBJECT_STORE_DESTROY_MUTEX|Si verifica durante la sincronizzazione interna in pool di memoria, in caso di eliminazione di oggetti dal pool.|  
|SOS_PROCESS_AFFINITY_MUTEX|Si verifica durante la sincronizzazione dell'accesso a impostazioni relative all'affinità di processo.|  
|SOS_RESERVEDMEMBLOCKLIST|Si verifica durante la sincronizzazione interna nel gestore della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOS_SCHEDULER_YIELD|Si verifica quando un'attività cede il controllo dell'utilità di pianificazione per consentire l'esecuzione di altre attività. Durante questa attesa, l'attività attende il rinnovo del quantum.|  
|SOS_SMALL_PAGE_ALLOC|Si verifica durante l'allocazione e la liberazione della memoria gestita da alcuni oggetti della memoria.|  
|SOS_STACKSTORE_INIT_MUTEX|Si verifica durante la sincronizzazione dell'inizializzazione dell'archivio interno.|  
|SOS_SYNC_TASK_ENQUEUE_EVENT|Si verifica quando un'attività viene avviata in modo sincrono. La maggior parte delle attività in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviata in modo asincrono, in base a cui il controllo viene restituito al processo che ha richiesto l'avvio delle attività immediatamente dopo l'inserimento della richiesta dell'attività nella coda di elaborazione.|  
|SOS_VIRTUALMEMORY_LOW|Si verifica quando un'allocazione di memoria è in attesa che venga liberata memoria virtuale da uno strumento di gestione delle risorse.|  
|SOSHOST_EVENT|Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione eventi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_INTERNAL|Si verifica durante la sincronizzazione dei callback del gestore della memoria usate da componenti hosted, ad esempio CLR.|  
|SOSHOST_MUTEX|Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione mutex di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_RWLOCK|Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione di tipo lettura/scrittura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_SEMAPHORE|Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione semafori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_SLEEP|Si verifica quando un'attività hosted viene sospesa in attesa di un evento generico. Le attività hosted vengono usate da componenti hosted come CLR.|  
|SOSHOST_TRACELOCK|Si verifica durante la sincronizzazione dell'accesso a flussi di traccia.|  
|SOSHOST_WAITFORDONE|Si verifica quando un componente hosted, ad esempio CLR, è in attesa del completamento di un'attività.|  
|SQLCLR_APPDOMAIN|Si verifica quando CLR è in attesa che venga completato l'avvio di un dominio applicazione.|  
|SQLCLR_ASSEMBLY|Si verifica durante l'attesa dell'accesso all'elenco degli assembly caricati nel dominio applicazione.|  
|SQLCLR_DEADLOCK_DETECTION|Si verifica quando CLR è in attesa che venga completato il rilevamento dei deadlock.|  
|SQLCLR_QUANTUM_PUNISHMENT|Si verifica quando viene applicata una limitazione a un'attività CLR poiché ha superato il quantum di esecuzione. Questa limitazione ha lo scopo di ridurre l'effetto di questa attività a elevato utilizzo di risorse sulle altre attività.|  
|SQLSORT_NORMMUTEX|Si verifica durante la sincronizzazione interna, nel corso dell'inizializzazione delle strutture di ordinamento interne.|  
|SQLSORT_SORTMUTEX|Si verifica durante la sincronizzazione interna, nel corso dell'inizializzazione delle strutture di ordinamento interne.|  
|SQLTRACE_BUFFER_FLUSH|Si verifica quando un'attività è in attesa di un'attività in background per scaricare i buffer di traccia su disco ogni 4 secondi.|  
|SQLTRACE_LOCK|Si verifica durante la sincronizzazione in buffer di traccia nel corso di una traccia di file.|  
|SQLTRACE_SHUTDOWN|Si verifica quando la chiusura della traccia è in attesa del completamento degli eventi di traccia in sospeso.|  
|SQLTRACE_WAIT_ENTRIES|Si verifica quando una coda degli eventi di Traccia SQL è in attesa di pacchetti in arrivo nella coda.|  
|SRVPROC_SHUTDOWN|Si verifica quando il processo di chiusura è in attesa del rilascio di risorse interne per essere completato correttamente.|  
|TEMPOBJ|Si verifica quando vengono sincronizzate le eliminazioni di oggetti temporanei. Questo tipo di attesa è raro e si verifica solo se un'attività ha richiesto l'accesso esclusivo per eliminazioni di tabelle temp.|  
|THREADPOOL|Si verifica quando un'attività è in attesa di un thread di lavoro in cui essere eseguita. Può indicare che il numero massimo di thread di lavoro impostato è troppo basso oppure che le esecuzioni dei batch richiedono una quantità di tempo insolitamente elevata, riducendo così il numero dei thread di lavoro disponibili per soddisfare altri batch.|  
|TIMEPRIV_TIMEPERIOD|Si verifica durante la sincronizzazione interna del timer degli eventi estesi.|  
|TRACEWRITE|Si verifica quando il provider di traccia del set di righe di Traccia SQL è in attesa di un buffer libero o un buffer con eventi da elaborare.|  
|TRAN_MARKLATCH_DT|Si verifica durante l'attesa di un latch in modalità di eliminazione in un latch di contrassegno di transazione. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.|  
|TRAN_MARKLATCH_EX|Si verifica durante l'attesa di un latch in modalità esclusiva in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.|  
|TRAN_MARKLATCH_KP|Si verifica durante l'attesa di un latch in modalità conservativa in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.|  
|TRAN_MARKLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|TRAN_MARKLATCH_SH|Si verifica durante l'attesa di un latch in modalità condivisa in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.|  
|TRAN_MARKLATCH_UP|Si verifica durante l'attesa di un latch in modalità di aggiornamento in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.|  
|TRANSACTION_MUTEX|Si verifica durante la sincronizzazione dell'accesso a una transazione da parte di più batch.|  
|UTIL_PAGE_ALLOC|Si verifica quando le analisi di log delle transazioni sono in attesa di memoria disponibile in caso di numero eccessivo di richieste di memoria.|  
|VIA_ACCEPT|Si verifica quando una connessione del provider VIA (Virtual Interface Adapter) viene completata durante l'avvio.|  
|VIEW_DEFINITION_MUTEX|Si verifica durante la sincronizzazione dell'accesso a definizioni delle viste memorizzate nella cache.|  
|WAIT_FOR_RESULTS|Si verifica durante l'attesa dell'attivazione di una notifica di query.|  
|WAITFOR|Si verifica come risultato di un'istruzione WAITFOR di [!INCLUDE[tsql](../../includes/tsql-md.md)]. La durata dell'attesa è determinata dai parametri per l'istruzione. Si tratta di un'attesa avviata dall'utente.|  
|WAITFOR_TASKSHUTDOWN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WAITSTAT_MUTEX|Si verifica durante la sincronizzazione dell'accesso alla raccolta di statistiche utilizzata per popolare sys.dm_os_wait_stats.|  
|WCC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WORKTBL_DROP|Si verifica durante la sospensione che precede un nuovo tentativo dopo un'eliminazione di tabella di lavoro non riuscita.|  
|WRITE_COMPLETION|Si verifica quando è in esecuzione un'operazione di scrittura.|  
|WRITELOG|Si verifica durante l'attesa del completamento di uno scaricamento del log. Checkpoint e commit delle transazioni costituiscono operazioni comuni che causano scaricamenti del log.|  
|XACT_OWN_TRANSACTION|Si verifica durante l'attesa dell'acquisizione della proprietà di una transazione.|  
|XACT_RECLAIM_SESSION|Si verifica durante l'attesa del rilascio della proprietà della sessione da parte del proprietario corrente.|  
|XACTLOCKINFO|Si verifica durante la sincronizzazione dell'accesso all'elenco dei blocchi per una transazione. In aggiunta alla transazione stessa, all'elenco dei blocchi accedono operazioni come il rilevamento dei deadlock e la migrazione dei blocchi durante le suddivisioni delle pagine.|  
|XACTWORKSPACE_MUTEX|Si verifica durante la sincronizzazione delle esclusioni da una transazione, nonché del numero di blocchi di database tra i membri integrati di una transazione.|  
|XE_BUFFERMGR_ALLPROCESSED_EVENT|Si verifica quando i buffer della sessione degli eventi estesi vengono scaricati nelle destinazioni. Questa attesa si verifica in un thread in background.|  
|XE_BUFFERMGR_FREEBUF_EVENT|Si verifica quando viene soddisfatta una delle condizioni seguenti:<br /><br /> Una sessione degli eventi estesi è configurata in modo da non prevedere alcuna perdita di eventi e tutti i buffer della sessione sono attualmente pieni. Questa situazione può indicare che le dimensioni dei buffer per una sessione degli eventi estesi non sono sufficienti o che i buffer devono essere partizionati.<br /><br /> Nei controlli si è verificato un ritardo. Questa situazione può indicare la presenza di un collo di bottiglia del disco nell'unità in cui vengono scritti i controlli.|  
|XE_DISPATCHER_CONFIG_SESSION_LIST|Si verifica quando una sessione degli eventi estesi che utilizza destinazioni asincrone viene avviata o arrestata. Questa attesa indica una delle due situazioni seguenti:<br /><br /> Una sessione degli eventi estesi sta eseguendo una registrazione con un pool di thread in background.<br /><br /> Il pool di thread in background sta calcolando il numero necessario di thread in base al carico corrente.|  
|XE_DISPATCHER_JOIN|Si verifica quando un thread in background usato per le sessioni degli eventi estesi verrà terminato.|  
|XE_DISPATCHER_WAIT|Si verifica quando un thread in background usato per le sessioni degli eventi estesi è in attesa dell'elaborazione dei buffer degli eventi.|  
|XE_MODULEMGR_SYNC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_OLS_LOCK|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_PACKAGE_LOCK_BACKOFF|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FT_COMPROWSET_RWLOCK|Full-text è in attesa di un'operazione di metadati di frammenti. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|  
|FT_IFTS_RWLOCK|Full-text è in attesa di sincronizzazione interna. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|  
|FT_IFTS_SCHEDULER_IDLE_WAIT|Tipo di attesa sospensione dell'utilità di pianificazione full-text. L'utilità di pianificazione è inattiva.|  
|FT_IFTSHC_MUTEX|Full-text è in attesa di un'operazione di controllo fdhost. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|  
|FT_IFTSISM_MUTEX|Full-text è in attesa di un'operazione di comunicazione. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|  
|FT_MASTER_MERGE|Full-text è in attesa di un'operazione di unione nell'indice master. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|  
  
  
