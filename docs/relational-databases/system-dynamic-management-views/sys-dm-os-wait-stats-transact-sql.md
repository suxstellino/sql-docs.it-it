---
description: sys.dm_os_wait_stats (Transact-SQL)
title: sys.dm_os_wait_stats (Transact-SQL)
ms.custom: contperf-fy21q3
ms.date: 01/27/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_wait_stats_TSQL
- dm_os_wait_stats
- sys.dm_os_wait_stats
- sys.dm_os_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_wait_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e2620a3e5df464e964f0bc911897481093aca245
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835330"
---
# <a name="sysdm_os_wait_stats-transact-sql"></a>sys.dm_os_wait_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce informazioni su tutte le attese rilevate dai thread eseguiti. È possibile usare questa visualizzazione aggregata per diagnosticare problemi a livello di prestazioni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e anche in query e batch specifici. [sys.dm_exec_session_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md) fornisce informazioni simili per sessione.  
  
> [!NOTE] 
> Per chiamare questo oggetto da **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] o**, usare il nome **sys.dm_pdw_nodes_os_wait_stats**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Nome del tipo di attesa. Per ulteriori informazioni, vedere la sezione [Tipi di attesa](#WaitTypes) di seguito in questo argomento.|  
|waiting_tasks_count|**bigint**|Numero di attese del tipo specificato. Questo contatore viene incrementato all'inizio di ogni attesa.|  
|wait_time_ms|**bigint**|Tempo di attesa totale, espresso in millisecondi, per il tipo di attesa specifico. Il tempo comprende signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Tempo di attesa massimo per il tipo di attesa specifico.|  
|signal_wait_time_ms|**bigint**|Differenza tra il momento in cui è stato rilevato il thread in attesa e quello in cui è stata avviata l'esecuzione del thread.|  
|pdw_node_id|**int**|Identificatore del nodo su cui si trova questa distribuzione. <br/> **Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

##  <a name="types-of-waits"></a><a name="WaitTypes"></a> Tipi di attese  
 Le **attese di risorse** si verificano quando un thread di lavoro richiede l'accesso a una risorsa che non è disponibile perché la risorsa è utilizzata da un altro thread di lavoro o non è ancora disponibile. Esempi di attese di risorse sono blocchi, latch, rete e attese di I/O su disco. Le attese di blocchi e latch sono attese a livello di oggetti di sincronizzazione.  
  
Le **attese della coda** si verificano quando un thread di lavoro è inattivo, in attesa dell'assegnazione di lavoro. Le attese di code si verificano principalmente nell'ambito di attività di sistema in background quali, ad esempio, il monitoraggio dei deadlock e le attività di pulizia dei record eliminati. Queste attività attenderanno l'inserimento delle richieste di lavoro in una coda di elaborazione. È possibile che le attese di code diventino periodicamente attive anche se non sono stati inseriti nuovi pacchetti nella coda.  
  
 Le **attese esterne** si verificano quando un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] thread di lavoro è in attesa del completamento di un evento esterno, ad esempio una chiamata stored procedure estesa o una query del server collegato. Se vengono rilevati problemi di blocco, è opportuno ricordare che le attese esterne non sempre implicano che il thread di lavoro sia inattivo in quanto è possibile che il thread di lavoro esegua attivamente codice esterno.  
  
 `sys.dm_os_wait_stats` visualizza la durata delle attese completate. Questa DMV non visualizza le attese correnti.  
  
 Un thread di lavoro di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene considerato in attesa se si verifica uno dei seguenti casi:  
  
-   Una risorsa diventa disponibile.  
  
-   Una coda non è vuota.  
  
-   Un processo esterno viene completato.  
  
 Anche se il thread non è più in attesa, la sua esecuzione non deve necessariamente essere avviata subito perché tale thread viene innanzitutto inserito nella coda dei thread di lavoro eseguibili e pertanto deve attendere un quantum per essere eseguito nell'utilità di pianificazione.  
  
 Nei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contatori del tempo di attesa sono valori di **bigint** e pertanto non sono soggetti a rollover come contatori equivalenti nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Tipi specifici di tempi di attesa durante l'esecuzione di query possono indicare colli di bottiglia oppure punti di stallo all'interno della query. In modo analogo, tempi di attesa o conteggi di attesa rilevanti a livello di server possono indicare colli di bottiglia o aree critiche nelle interazioni tra query all'interno dell'istanza del server. Ad esempio, le attese di blocco indicano contese a livello di dati da parte delle query, le attese di latch di I/O di pagina indicano tempi di risposta I/O bassi, mentre le attese di aggiornamento dei latch di pagina indicano layout di file errati.  
  
 Il contenuto di questa DMV può essere ripristinato mediante l'esecuzione del comando seguente:  
  
```sql  
DBCC SQLPERF ('sys.dm_os_wait_stats', CLEAR);  
GO  
```  
  
Questo comando reimposta tutti i contatori su 0.  
  
> [!NOTE]
> Queste statistiche non sono persistenti tra i vari riavvii di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e tutti i dati sono cumulativi dall'ultimo ripristino delle statistiche oppure dall'ultimo avvio del server.  
  
 Nella tabella seguente sono elencati i tipi di attesa rilevati dalle attività.  

|tipo |Descrizione| 
|-------------------------- |--------------------------| 
|ABR |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| | 
|AM_INDBUILD_ALLOCATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|AM_SCHEMAMGR_UNSHARED_CACHE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|ASSEMBLY_FILTER_HASHTABLE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|ASSEMBLY_LOAD |Si verifica durante l'accesso esclusivo al caricamento di assembly.| 
|ASYNC_DISKPOOL_LOCK |Si verifica in caso di tentativo di sincronizzazione di thread paralleli che eseguono attività quali, ad esempio, la creazione o l'inizializzazione di un file.| 
|ASYNC_IO_COMPLETION |Si verifica quando un'attività è in attesa del completamento dell'I/O.| 
|ASYNC_NETWORK_IO |Si verifica durante le operazioni di scrittura in rete quando l'attività è bloccata in rete. Verificare che il client stia elaborando dati dal server.| 
|ASYNC_OP_COMPLETION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|ASYNC_OP_CONTEXT_READ |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|ASYNC_OP_CONTEXT_WRITE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|ASYNC_SOCKETDUP_IO |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|AUDIT_GROUPCACHE_LOCK |Si verifica in caso di attesa su un blocco che controlla accesso a una cache speciale. La cache contiene le informazioni sui controlli usati per controllare ogni gruppo di azioni di controllo.| 
|AUDIT_LOGINCACHE_LOCK |Si verifica in caso di attesa su un blocco che controlla accesso a una cache speciale. La cache contiene le informazioni sui controlli usati per controllare i gruppi di azioni di controllo accesso.| 
|AUDIT_ON_DEMAND_TARGET_LOCK |Si verifica in caso di attesa su un blocco usato per assicurare l'inizializzazione singola delle destinazioni degli eventi estesi relative ai controlli.| 
|AUDIT_XE_SESSION_MGR |Si verifica in caso di attesa su un blocco usato per sincronizzare l'avvio e l'arresto delle sessioni degli eventi estesi relative ai controlli.| 
|BACKUP |Si verifica quando un'attività è bloccata in quanto parte dell'elaborazione di un backup.| 
|BACKUP_OPERATOR |Si verifica quando un'attività è in attesa del montaggio del nastro. Per visualizzare lo stato del nastro, eseguire una query su sys.dm_io_backup_tapes. Se un'operazione di montaggio non è in sospeso, questo tipo di attesa potrebbe indicare un problema a livello di hardware nell'unità nastro.| 
|BACKUPBUFFER |Si verifica quando un'attività di backup è in attesa di dati oppure di un buffer in cui archiviare dati. Questo tipo di attesa non è comune, tranne quando un'attività è in attesa del montaggio di un nastro.| 
|BACKUPIO |Si verifica quando un'attività di backup è in attesa di dati oppure di un buffer in cui archiviare dati. Questo tipo di attesa non è comune, tranne quando un'attività è in attesa del montaggio di un nastro.| 
|BACKUPTHREAD |Si verifica quando un'attività è in attesa del completamento di un'attività di backup. I tempi di attesa possono essere lunghi, da alcuni minuti a parecchie ore. Se l'attività per la quale si è verificata l'attesa è un processo di I/O, questo tipo di attesa non indica un problema.| 
|BAD_PAGE_PROCESS |Si verifica quando il logger in background delle pagine sospette tenta di evitare l'esecuzione con una frequenza superiore a 5 secondi. Un numero eccessivo di pagine sospette determina una frequente esecuzione del logger.| 
|BLOB_METADATA |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|BMPALLOCATION |Si verifica con piani in modalità batch paralleli quando si sincronizza l'allocazione di un filtro bitmap di grandi dimensioni. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|BMPBUILD |Si verifica con piani in modalità batch paralleli quando si sincronizza la compilazione di un filtro bitmap di grandi dimensioni. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|BMPREPARTITION |Si verifica con piani in modalità batch paralleli durante la sincronizzazione del partizionamento di un filtro bitmap di grandi dimensioni. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|BMPREPLICATION |Si verifica con piani in modalità batch paralleli quando si sincronizza la replica di un filtro bitmap di grandi dimensioni tra thread di lavoro. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|BPSORT |Si verifica con piani in modalità batch paralleli durante la sincronizzazione dell'ordinamento di un set di dati tra più thread. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|BROKER_CONNECTION_RECEIVE_TASK |Si verifica durante l'attesa dell'accesso per la ricezione di un messaggio su un endpoint della connessione. L'accesso per la ricezione all'endpoint viene serializzato.| 
|BROKER_DISPATCHER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|BROKER_ENDPOINT_STATE_MUTEX |Si verifica in caso di contesa per l'accesso allo stato di un endpoint di connessione Service Broker. L'accesso allo stato per le modifiche viene serializzato.| 
|BROKER_EVENTHANDLER |Si verifica quando un'attività è in attesa nel gestore eventi primario della Service Broker. Questo tipo di attesa si verifica per brevissimi periodi.| 
|BROKER_FORWARDER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|BROKER_INIT |Si verifica durante l'inizializzazione di Service Broker in ogni database attivo. Questo tipo di attesa si verifica raramente.| 
|BROKER_MASTERSTART |Si verifica quando un'attività è in attesa dell'avvio del gestore eventi primario del Service Broker. Questo tipo di attesa si verifica per brevissimi periodi.| 
|BROKER_RECEIVE_WAITFOR |Si verifica quando l'istruzione RECEIVE WAITFOR è in attesa. Questo potrebbe significare che non è possibile ricevere messaggi nella coda o che un conflitto di blocco impedisce la ricezione di messaggi dalla coda.| 
|BROKER_REGISTERALLENDPOINTS |Si verifica durante l'inizializzazione di un endpoint della connessione Service Broker. Questo tipo di attesa si verifica per brevissimi periodi.| 
|BROKER_SERVICE |Si verifica quando l'elenco di destinazione Service Broker associato a un servizio di destinazione viene aggiornato o riassegnato in ordine di priorità.| 
|BROKER_SHUTDOWN |Si verifica in caso di chiusura pianificata di Service Broker. Questo tipo di attesa si verifica saltuariamente ed eventualmente per brevissimi periodi.| 
|BROKER_START |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|BROKER_TASK_SHUTDOWN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|BROKER_TASK_STOP |Si verifica quando il gestore dell'attività Service Broker Queue tenta di arrestare l'attività. Il controllo di stato è serializzato e deve essere in uno stato di esecuzione prima dell'operazione.| 
|BROKER_TASK_SUBMIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|BROKER_TO_FLUSH |Si verifica quando il Service Broker lo scaricatore Lazy Scarica gli oggetti di trasmissione in memoria in una tabella di lavoro.| 
|BROKER_TRANSMISSION_OBJECT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|BROKER_TRANSMISSION_TABLE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|BROKER_TRANSMISSION_WORK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|BROKER_TRANSMITTER |Si verifica quando il trasmettitore Service Broker è in attesa di lavoro. Service Broker dispone di un componente noto come trasmettitore che pianifica i messaggi provenienti da più finestre di dialogo da inviare in transito su uno o più endpoint di connessione. Il trasmettitore ha 2 thread dedicati a questo scopo. Questo tipo di attesa viene addebitato quando questi thread di trasmissione sono in attesa dell'invio dei messaggi di dialogo utilizzando le connessioni di trasporto. Valori elevati di waiting_tasks_count per questo tipo di attesa puntano a lavoro intermittente per questi thread di trasmissione e non sono indicazioni di alcun problema di prestazioni. Se Service Broker non viene usato, waiting_tasks_count dovrebbe essere 2 (per i 2 thread di trasmissione) e wait_time_ms deve essere il doppio della durata dall'avvio dell'istanza. Vedere le [statistiche di attesa di Service Broker](/archive/blogs/sql_service_broker/service-broker-wait-types).|
|BUILTIN_HASHKEY_MUTEX |Può verificarsi dopo l'avvio dell'istanza, durante l'inizializzazione delle strutture di dati interne. Non si ripete dopo l'inizializzazione delle strutture di dati.| 
|CHANGE_TRACKING_WAITFORCHANGES |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|CHECK_PRINT_RECORD |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|CHECK_SCANNER_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|CHECK_TABLES_INITIALIZATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|CHECK_TABLES_SINGLE_SCAN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|CHECK_TABLES_THREAD_BARRIER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|CHECKPOINT_QUEUE |Si verifica quando l'attività di checkpoint è in attesa della successiva richiesta di checkpoint.| 
|CHKPT |Si verifica all'avvio del server per indicare il thread di gestione dei checkpoint che è possibile avviare.| 
|CLEAR_DB |Si verifica durante le operazioni che modificano lo stato di un database, ad esempio l'apertura o la chiusura.| 
|CLR_AUTO_EVENT |Si verifica quando un'attività sta effettuando un'esecuzione CLR (Common Language Runtime) ed è in attesa dell'inizializzazione di un evento automatico specifico. In genere si verificano attese prolungate, che tuttavia non indicano un problema.| 
|CLR_CRST |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di iniziare una parte critica dell'attività usata da un'altra attività.| 
|CLR_JOIN |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa del completamento di un'altra attività. Questo stato di attesa si verifica quando è presente un join tra attività.| 
|CLR_MANUAL_EVENT |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa dell'inizializzazione di un evento manuale specifico.| 
|CLR_MEMORY_SPY |Si verifica durante l'attesa dell'acquisizione del blocco per una struttura di dati usata per registrare tutte le allocazioni della memoria virtuale che provengono da CLR. La struttura di dati è bloccata per gestire l'integrità se l'accesso è parallelo.| 
|CLR_MONITOR |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di acquisire un blocco per il monitoraggio.| 
|CLR_RWLOCK_READER |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di un blocco del reader.| 
|CLR_RWLOCK_WRITER |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di un blocco del writer.| 
|CLR_SEMAPHORE |Si verifica quando un'attività sta effettuando un'esecuzione CLR ed è in attesa di un semaforo.| 
|CLR_TASK_START |Si verifica durante l'attesa del completamento dell'avvio di un'attività CLR.| 
|CLRHOST_STATE_ACCESS |Si verifica in caso di attesa per l'acquisizione dell'accesso esclusivo alle strutture di dati di hosting CLR. Questo tipo di attesa si verifica durante la configurazione o la rimozione del runtime CLR.| 
|CMEMPARTITIONED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|CMEMTHREAD |Si verifica quando un'attività è in attesa di un oggetto memoria affidabile. Il tempo di attesa potrebbe aumentare in caso di contesa causata da più attività che tentano di allocare memoria dallo stesso oggetto memoria.| 
|COLUMNSTORE_BUILD_THROTTLE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|COLUMNSTORE_COLUMNDATASET_SESSION_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|COMMIT_TABLE |Solo per uso interno.| 
|CONNECTION_ENDPOINT_LOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|COUNTRECOVERYMGR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|CREATE_DATINISERVICE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|CXCONSUMER <a name="cxconsumer"></a>|Si verifica con piani di query paralleli quando un thread consumer (padre) attende che un thread producer invii righe. Le attese CXCONSUMER sono causate da un iteratore di scambio che esaurisce le righe dal thread producer. Si tratta di una parte normale dell'esecuzione di query parallele. <br /><br /> **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]|
|CXPACKET <a name="cxpacket"></a>|Si verifica con piani di query paralleli quando si è in attesa di sincronizzare l' [iteratore di scambio](../../relational-databases/showplan-logical-and-physical-operators-reference.md)di query processor e durante la produzione e l'utilizzo di righe. Se l'attesa è eccessiva e non può essere ridotta ottimizzando la query, ad esempio aggiungendo indici, è consigliabile modificare la soglia dei costi per il parallelismo o abbassare il grado massimo di parallelismo (MaxDOP). <br /><br /> **Nota:** A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3, CXPACKET fa riferimento solo all'attesa per la sincronizzazione dell'iteratore di Exchange e alla produzione di righe. I thread che utilizzano righe vengono rilevati separatamente nel tipo di attesa CXCONSUMER. Se i thread consumer sono troppo lenti, il buffer dell'iteratore di Exchange potrebbe essere pieno e causare CXPACKET attese. <br /><br /> **Nota:** In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] CXPACKET fa riferimento solo all'attesa di thread che producono righe. La sincronizzazione degli iteratori di Exchange viene rilevata separatamente nella CXSYNC_PORT e CXSYNC_CONSUMER i tipi di attesa. I thread che utilizzano righe vengono rilevati separatamente nel tipo di attesa CXCONSUMER.<br /> | 
|CXSYNC_PORT|Si verifica con piani di query paralleli durante l'attesa di apertura, chiusura e sincronizzazione delle porte degli [iteratori di Exchange](../../relational-databases/showplan-logical-and-physical-operators-reference.md) tra i thread producer e consumer. Se, ad esempio, un piano di query dispone di un'operazione di ordinamento lungo, CXSYNC_PORT attese potrebbe essere maggiore perché l'ordinamento deve essere completato prima che la porta dell'iteratore di Exchange possa essere sincronizzata. <br /><br /> **Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXSYNC_CONSUMER|Si verifica con piani di query paralleli in attesa di raggiungere un punto di sincronizzazione dell' [iteratore di Exchange](../../relational-databases/showplan-logical-and-physical-operators-reference.md) tra tutti i thread consumer. <br /><br /> **Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXROWSET_SYNC |Si verifica durante un'analisi di intervalli parallela.| 
|DAC_INIT |Si verifica durante l'inizializzazione della connessione amministrativa dedicata.| 
|DBCC_SCALE_OUT_EXPR_CACHE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|DBMIRROR_DBM_EVENT |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|DBMIRROR_DBM_MUTEX |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|DBMIRROR_EVENTS_QUEUE |Si verifica quando il mirroring del database è in attesa di eventi da elaborare.| 
|DBMIRROR_SEND |Si verifica quando un'attività è in attesa della cancellazione di un backlog delle comunicazioni nel livello rete per essere in grado di inviare messaggi. Indica che nel livello comunicazioni si sta verificando un overload che può pregiudicare la velocità effettiva dei dati di mirroring del database.| 
|DBMIRROR_WORKER_QUEUE |Indica che l'attività di lavoro del mirroring del database è in attesa di ulteriore lavoro.| 
|DBMIRRORING_CMD |Si verifica quando un'attività è in attesa dello scaricamento su disco dei record di log. Questo stato di attesa viene in genere mantenuto per lunghi periodi di tempo.| 
|DBSEEDING_FLOWCONTROL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|DBSEEDING_OPERATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|DEADLOCK_ENUM_MUTEX |Si verifica quando il monitoraggio dei deadlock e sys.dm_os_waiting_tasks cercano di garantire che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non esegua più ricerche di deadlock contemporaneamente.| 
|DEADLOCK_TASK_SEARCH |Un tempo di attesa elevato per questa risorsa indica che il server sta eseguendo query su sys.dm_os_waiting_tasks e che tali query bloccano l'esecuzione della ricerca di deadlock nell'ambito del monitoraggio dei deadlock. Questo tipo di attesa viene usato soltanto dalla funzionalità di monitoraggio dei deadlock. Le query su sys.dm_os_waiting_tasks utilizzano DEADLOCK_ENUM_MUTEX.| 
|DEBUG |Si verifica durante il debug di Transact-SQL e CLR per la sincronizzazione interna.| 
|DIRECTLOGCONSUMER_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DIRTY_PAGE_POLL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|DIRTY_PAGE_SYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|DIRTY_PAGE_TABLE_LOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DISABLE_VERSIONING |Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue il polling della gestione delle transazioni di versione per verificare se il timestamp della transazione attiva meno recente è successivo al timestamp relativo al momento in cui lo stato è stato modificato. In questo caso, vengono completate tutte le transazioni snapshot avviate prima dell'esecuzione dell'istruzione ALTER DATABASE. Questo stato di attesa viene utilizzando quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disabilita il controllo delle versioni tramite l'istruzione ALTER DATABASE.| 
|DISKIO_SUSPEND |Si verifica quando un'attività è in attesa di accedere a un file quando è attivo un backup esterno. Questo tipo di attesa viene segnalato per ogni processo utente in attesa. Un conteggio maggiore di 5 per processo utente può indicare che il completamento del backup esterno sta richiedendo troppo tempo.| 
|DISPATCHER_PRIORITY_QUEUE_SEMAPHORE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|DISPATCHER_QUEUE_SEMAPHORE |Si verifica quando un thread del pool di dispatcher è in attesa di più lavori da elaborare. Si prevede che il tempo di attesa per questo tipo di attesa aumenti quando il dispatcher è inattivo.| 
|DLL_LOADING_MUTEX |Si verifica una volta durante l'attesa del caricamento della DLL del parser XML.| 
|DPT_ENTRY_LOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DROP_DATABASE_TIMER_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|DROPTEMP |Si verifica tra diversi tentativi di eliminare un oggetto temporaneo, se il tentativo precedente ha avuto esito negativo. La durata dell'attesa aumenta in modo esponenziale con ogni tentativo di eliminazione non riuscito.| 
|DTC |Si verifica quando un'attività è in attesa di un evento usato per gestire la transizione di stato. Questo stato controlla quando si verifica il ripristino delle transazioni di Microsoft Distributed Transaction Coordinator (MS DTC) dopo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che riceve una notifica che il servizio MS DTC non è più disponibile.| 
|DTC_ABORT_REQUEST |Si verifica in una sessione di lavoro MS DTC quando la sessione è in attesa di acquisire la proprietà di una transazione MS DTC. Non appena MS DTC acquisisce la proprietà della transazione, la sessione può eseguire il rollback della transazione. In genere, la sessione attenderà un'altra sessione che sta utilizzando la transazione.| 
|DTC_RESOLVE |Si verifica quando un'attività di recupero è in attesa del database master in una transazione tra database in modo che l'attività possa eseguire una query sul risultato della transazione.| 
|DTC_STATE |Si verifica quando un'attività è in attesa di un evento che impedisce le modifiche all'oggetto stato globale MS DTC interno. Questo stato deve essere mantenuto per brevissimi periodi di tempo.| 
|DTC_TMDOWN_REQUEST |Si verifica in una sessione di lavoro MS DTC quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve una notifica indicante che il servizio MS DTC non è disponibile. Il thread di lavoro attenderà innanzitutto l'avvio del processo di recupero MS DTC. Il thread di lavoro attende quindi di ottenere il risultato della transazione distribuita su cui sta lavorando. Ciò potrebbe continuare finché non viene ristabilita la connessione al servizio MS DTC.| 
|DTC_WAITFOR_OUTCOME |Si verifica quando le attività di recupero attendono l'attivazione di MS DTC per consentire la risoluzione delle transazioni preparate.| 
|DTCNEW_ENLIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DTCNEW_PREPARE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DTCNEW_RECOVERY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DTCNEW_TM |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DTCNEW_TRANSACTION_ENLISTMENT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|DTCPNTSYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|DUMP_LOG_COORDINATOR |Si verifica quando un'attività principale è in attesa che una sottoattività generi dati. In genere, questo stato non si verifica mai. Un tempo di attesa prolungato indica un blocco imprevisto. È pertanto necessario verificare la sottoattività.| 
|DUMP_LOG_COORDINATOR_QUEUE |Solo per uso interno.| 
|DUMPTRIGGER |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|EC |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|EE_PMOLOCK |Si verifica durante la sincronizzazione di determinati tipi di allocazioni di memoria nel corso dell'esecuzione di istruzioni.| 
|EE_SPECPROC_MAP_INIT |Si verifica durante la sincronizzazione della creazione della tabella hash delle procedure interne. Questa attesa può verificarsi solo durante l'accesso iniziale alla tabella hash dopo l'avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|ENABLE_EMPTY_VERSIONING |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|ENABLE_VERSIONING |Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attende il completamento di tutte le transazioni di aggiornamento nel database prima di dichiarare il database pronto per la transizione allo stato in cui è consentito l'isolamento dello snapshot. Questo stato viene usato quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilita l'isolamento dello snapshot tramite l'istruzione ALTER DATABASE.| 
|ERROR_REPORTING_MANAGER |Si verifica durante la sincronizzazione delle inizializzazioni di più log degli errori simultanei.| 
|EXCHANGE |Si verifica durante la sincronizzazione nell'iteratore di scambio di Query Processor nel corso di query parallele.| 
|EXECSYNC |Si verifica durante query parallele nel corso della sincronizzazione in Query Processor in aree non correlate all'iteratore di scambio. Tali aree sono, ad esempio, bitmap, oggetti BLOB (Binary Large Object) e l'iteratore di spool. È possibile che gli oggetti BLOB utilizzino di frequente questo stato di attesa.| 
|EXECUTION_PIPE_EVENT_INTERNAL |Si verifica durante la sincronizzazione tra parti del produttore e dell'utente dell'esecuzione batch inviate tramite il contesto di connessione.| 
|EXTERNAL_RG_UPDATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|EXTERNAL_SCRIPT_NETWORK_IO |Solo per uso interno. <br /><br /> **Si applica a: da a** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] corrente.| 
|EXTERNAL_SCRIPT_PREPARE_SERVICE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|EXTERNAL_SCRIPT_SHUTDOWN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|EXTERNAL_WAIT_ON_LAUNCHER, |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|FABRIC_HADR_TRANSPORT_CONNECTION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FABRIC_REPLICA_CONTROLLER_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FABRIC_REPLICA_CONTROLLER_STATE_AND_CONFIG |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FABRIC_REPLICA_PUBLISHER_EVENT_PUBLISH |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FABRIC_REPLICA_PUBLISHER_SUBSCRIBER_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FABRIC_WAIT_FOR_BUILD_REPLICA_EVENT_PROCESSING |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FAILPOINT |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FCB_REPLICA_READ |Si verifica quando le letture di un file sparse snapshot oppure di uno snapshot temporaneo creato da DBCC vengono sincronizzate.| 
|FCB_REPLICA_WRITE |Si verifica durante la sincronizzazione di operazioni di push o pull di una pagina in un file sparse di uno snapshot oppure di uno snapshot temporaneo creato da DBCC.| 
|FEATURE_SWITCHES_UPDATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FFT_NSO_DB_KILL_FLAG |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_DB_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_FCB |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_FCB_FIND |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_FCB_PARENT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_FCB_RELEASE_CACHED_ENTRIES |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_FCB_STATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|FFT_NSO_FILEOBJECT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NSO_TABLE_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_NTFS_STORE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_RECOVERY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_RSFX_COMM |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_RSFX_WAIT_FOR_MEMORY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_STARTUP_SHUTDOWN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_STORE_DB |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_STORE_ROWSET_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FFT_STORE_TABLE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILE_VALIDATION_THREADS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|FILESTREAM_CACHE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILESTREAM_CHUNKER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILESTREAM_CHUNKER_INIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILESTREAM_FCB |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILESTREAM_FILE_OBJECT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILESTREAM_WORKITEM_QUEUE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FILETABLE_SHUTDOWN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FOREIGN_REDO |Solo per uso interno. <br /><br /> **Si applica a: da a** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] corrente.| 
|FORWARDER_TRANSITION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|FS_FC_RWLOCK |Si verifica in caso di attesa del Garbage Collector di FILESTREAM per le seguenti operazioni:| 
|FS_GARBAGE_COLLECTOR_SHUTDOWN |Si verifica quando il Garbage Collector di FILESTREAM attende il completamento delle attività di pulizia.| 
|FS_HEADER_RWLOCK |Si verifica in caso di attesa per l'accesso all'intestazione FILESTREAM di un contenitore di dati FILESTREAM per leggere o aggiornare il contenuto nel file di intestazione (Filestream.hdr) di FILESTREAM.| 
|FS_LOGTRUNC_RWLOCK |Si verifica in caso di attesa per l'accesso al troncamento del log di FILESTREAM per le seguenti operazioni:| 
|FSA_FORCE_OWN_XACT |Si verifica quando un'operazione di I/O del file FILESTREAM deve essere associata alla transazione associata, ma la transazione appartiene attualmente a un'altra sessione.| 
|FSAGENT |Si verifica quando un'operazione di I/O del file FILESTREAM attende una risorsa dell'agente di FILESTREAM usata da un'altra operazione di I/O di file.| 
|FSTR_CONFIG_MUTEX |Si verifica in caso di attesa per il completamento di un'altra riconfigurazione di funzionalità di FILESTREAM.| 
|FSTR_CONFIG_RWLOCK |Si verifica in caso di attesa per serializzare l'accesso ai parametri di configurazione di FILESTREAM.| 
|FT_COMPROWSET_RWLOCK |Full-text è in attesa di un'operazione di metadati di frammenti. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FT_IFTS_RWLOCK |Full-text è in attesa di sincronizzazione interna. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FT_IFTS_SCHEDULER_IDLE_WAIT |Tipo di attesa sospensione dell'utilità di pianificazione full-text. L'utilità di pianificazione è inattiva.| 
|FT_IFTSHC_MUTEX |Full-text è in attesa di un'operazione di controllo fdhost. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FT_IFTSISM_MUTEX |Full-text è in attesa di un'operazione di comunicazione. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FT_MASTER_MERGE |Full-text è in attesa di un'operazione di unione nell'indice master. Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FT_MASTER_MERGE_COORDINATOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FT_METADATA_MUTEX |Documentato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|FT_PROPERTYLIST_CACHE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|FT_RESTART_CRAWL |Si verifica quando è necessario riavviare una ricerca per indicizzazione full-text dall'ultimo punto valido noto per correggere un errore temporaneo. L'attesa consente alle attività di lavoro usate per il popolamento di completare il passaggio corrente o uscirne.| 
|FULLTEXT GATHERER |Si verifica durante la sincronizzazione delle operazioni full-text.| 
|GDMA_GET_RESOURCE_OWNER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|GHOSTCLEANUP_UPDATE_STATS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|GHOSTCLEANUPSYNCMGR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|GLOBAL_QUERY_CANCEL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|GLOBAL_QUERY_CLOSE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|GLOBAL_QUERY_CONSUMER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|GLOBAL_QUERY_PRODUCER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|GLOBAL_TRAN_CREATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|GLOBAL_TRAN_UCS_SESSION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|GUARDIAN |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|HADR_AG_MUTEX |Si verifica quando un'istruzione Always On DDL o un comando Windows Server failover clustering è in attesa dell'accesso in lettura/scrittura esclusivo alla configurazione di un gruppo di disponibilità. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_AR_CRITICAL_SECTION_ENTRY |Si verifica quando un'istruzione Always On DDL o un comando Windows Server failover clustering è in attesa dell'accesso in lettura/scrittura esclusivo allo stato di runtime della replica locale del gruppo di disponibilità associato. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_AR_MANAGER_MUTEX |Si verifica quando l'arresto di una replica di disponibilità è in attesa del completamento dell'avvio o l'avvio di una replica di disponibilità è in attesa del completamento dell'arresto. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_AR_UNLOAD_COMPLETED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_ARCONTROLLER_NOTIFICATIONS_SUBSCRIBER_LIST |Il server di pubblicazione di un evento di replica di disponibilità, ad esempio una modifica di stato o di configurazione, è in attesa dell'accesso in lettura/scrittura esclusivo all'elenco di sottoscrittori eventi. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_BACKUP_BULK_LOCK |Il database primario Always On ha ricevuto una richiesta di backup da un database secondario ed è in attesa del completamento dell'elaborazione della richiesta da parte del thread in background per l'acquisizione o il rilascio del blocco BulkOp. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_BACKUP_QUEUE |Il thread in background di backup del database primario Always On è in attesa di una nuova richiesta di lavoro dal database secondario. In genere, questo errore si verifica quando il database primario contiene il registro BulkOp ed è in attesa del database secondario per indicare che il database primario può rilasciare il blocco. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_CLUSAPI_CALL |Un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] thread è in attesa di passare dalla modalità non preemptive (pianificata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) alla modalità preemptive (pianificata dal sistema operativo) per richiamare le API di Windows Server failover clustering. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_COMPRESSED_CACHE_SYNC |Attesa dell'accesso alla cache di blocchi di log compressi usati per evitare una compressione ridondante dei blocchi di log inviati a più database secondari. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_CONNECTIVITY_INFO |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DATABASE_FLOW_CONTROL |Attesa di messaggi da inviare al partner quando viene raggiunto il numero massimo di messaggi in coda. Indica che le analisi del log vengono eseguite più rapidamente degli invii di rete. Si tratta di un problema solo se invii di questo tipo sono più lenti del previsto. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DATABASE_VERSIONING_STATE |Si verifica in caso di modifica dello stato di controllo delle versioni di un Always On database secondario. Questa attesa è per le strutture di dati interne e in genere è molto breve senza effetti diretti sull'accesso ai dati. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DATABASE_WAIT_FOR_RECOVERY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_DATABASE_WAIT_FOR_RESTART |In attesa del riavvio del database in Always On controllo dei gruppi di disponibilità. In condizioni normali, non si tratta di un problema del cliente dal momento che le attese sono previste. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING |Una query sugli oggetti in un database secondario leggibile di un gruppo di disponibilità Always On è bloccata nel controllo delle versioni delle righe durante l'attesa del commit o del rollback di tutte le transazioni in corso quando la replica secondaria è stata abilitata per i carichi di lavoro di lettura. Questo tipo di attesa assicura la disponibilità delle versioni delle righe prima dell'esecuzione di una query con isolamento dello snapshot. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DB_COMMAND |In attesa di risposte a messaggi di conversazione (che richiedono una risposta esplicita dall'altro lato, usando l'infrastruttura dei messaggi in conversazione Always On). Questo tipo di attesa viene usato da molti tipi di messaggi diversi. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DB_OP_COMPLETION_SYNC |In attesa di risposte a messaggi di conversazione (che richiedono una risposta esplicita dall'altro lato, usando l'infrastruttura dei messaggi in conversazione Always On). Questo tipo di attesa viene usato da molti tipi di messaggi diversi. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DB_OP_START_SYNC |Un'istruzione Always On DDL o un comando Windows Server failover clustering è in attesa dell'accesso serializzato a un database di disponibilità e al relativo stato di Runtime. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DBR_SUBSCRIBER |Il server di pubblicazione di un evento di replica di disponibilità, ad esempio una modifica di stato o di configurazione, è in attesa dell'accesso in lettura/scrittura esclusivo allo stato di runtime di un sottoscrittore eventi che corrisponde a un database di disponibilità. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DBR_SUBSCRIBER_FILTER_LIST |Il server di pubblicazione di un evento di replica di disponibilità, ad esempio una modifica di stato o di configurazione, è in attesa dell'accesso in lettura/scrittura esclusivo all'elenco di sottoscrittori eventi che corrispondono ai database di disponibilità. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_DBSEEDING |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|HADR_DBSEEDING_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|HADR_DBSTATECHANGE_SYNC |Attesa del controllo della concorrenza per l'aggiornamento dello stato interno della replica di database. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FABRIC_CALLBACK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|HADR_FILESTREAM_BLOCK_FLUSH |Il gestore del trasporto Always On FILESTREAM è in attesa fino al completamento dell'elaborazione di un blocco del log. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FILESTREAM_FILE_CLOSE |Il gestore del trasporto Always On FILESTREAM è in attesa fino a quando il file FILESTREAM successivo non viene elaborato e il relativo handle viene chiuso. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FILESTREAM_FILE_REQUEST |Una replica secondaria Always On è in attesa che la replica primaria invii tutti i file FILESTREAM richiesti durante il rollback. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FILESTREAM_IOMGR |Il gestore del trasporto Always On FILESTREAM è in attesa del blocco R/W che protegge la gestione Always On di I/O di FILESTREAM durante l'avvio o l'arresto. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FILESTREAM_IOMGR_IOCOMPLETION |Il gestore di i/O Always On FILESTREAM è in attesa del completamento I/O. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FILESTREAM_MANAGER |Il gestore del trasporto Always On FILESTREAM è in attesa del blocco R/W che protegge il gestore di trasporto Always On FILESTREAM durante l'avvio o l'arresto. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_FILESTREAM_PREPROC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_GROUP_COMMIT |L'elaborazione del commit della transazione è in attesa di consentire un commit di gruppo in modo che sia possibile l'inserimento di più record di log del commit in un unico blocco del log. Questa attesa è una condizione prevista che consente di ottimizzare le operazioni di invio, di acquisizione e di I/O del log. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_LOGCAPTURE_SYNC |Controllo della concorrenza relativa all'acquisizione del log o applicazione dell'oggetto durante la creazione o l'eliminazione delle analisi. Si tratta di un'attesa prevista quando i partner modificano lo stato o lo stato di connessione. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_LOGCAPTURE_WAIT |Attesa che diventino disponibili i record di log. Si può verificare quando si attende la generazione di nuovi record di log tramite connessioni o il completamento di I/O quando la lettura del log non viene eseguita nella cache. Si tratta di un'attesa prevista se l'analisi del log viene recuperata alla fine del log o viene letta dal disco. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_LOGPROGRESS_SYNC |Attesa del controllo della concorrenza quando si aggiorna lo stato del log di repliche di database. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_NOTIFICATION_DEQUEUE |Un'attività in background tramite cui vengono elaborate notifiche di Windows Server Failover Clustering è in attesa della prossima notifica. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_NOTIFICATION_WORKER_EXCLUSIVE_ACCESS |Gestione repliche di disponibilità Always On è in attesa dell'accesso serializzato allo stato di runtime di un'attività in background che elabora le notifiche di Windows Server failover clustering. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_NOTIFICATION_WORKER_STARTUP_SYNC |Un'attività in background è in attesa del completamento dell'avvio di un'attività in background tramite cui vengono elaborate notifiche di Windows Server Failover Clustering. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_NOTIFICATION_WORKER_TERMINATION_SYNC |Un'attività in background è in attesa del completamento di un'attività in background tramite cui vengono elaborate notifiche di Windows Server Failover Clustering. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_PARTNER_SYNC |Attesa del controllo della concorrenza nell'elenco di partner. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_READ_ALL_NETWORKS |Attesa di ottenere l'accesso in lettura o scrittura all'elenco di reti WSFC. Solo per uso interno. Nota: il motore mantiene un elenco di reti WSFC utilizzate nelle viste a gestione dinamica (ad esempio sys.dm_hadr_cluster_networks) o per convalidare Always On istruzioni Transact-SQL che fanno riferimento alle informazioni di rete WSFC. Questo elenco viene aggiornato all'avvio del motore, alle notifiche relative a WSFC e al riavvio interno del Always On, ad esempio la perdita e il ricupero del quorum WSFC. Le attività verranno di solito bloccate quando è in corso un aggiornamento nell'elenco in questione. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_RECOVERY_WAIT_FOR_CONNECTION |Attesa della connessione del database secondario al database primario prima dell'esecuzione del recupero. Si tratta di un'attesa prevista che può prolungarsi se la connessione al database primario richiede tempo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_RECOVERY_WAIT_FOR_UNDO |Il recupero del database è in attesa del completamento della fase di ripristino e inizializzazione da parte del database secondario che ne consente il ripristino al punto di log comune con il database primario. Si tratta di un'attesa prevista dopo i failover. Lo stato di annullamento può essere rilevato tramite il monitor di sistema di Windows (perfmon.exe) e le viste a gestione dinamica. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_REPLICAINFO_SYNC |Attesa dell'aggiornamento dello stato corrente della replica da parte del controllo della concorrenza. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_SEEDING_CANCELLATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_SEEDING_FILE_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_SEEDING_LIMIT_BACKUPS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_SEEDING_SYNC_COMPLETION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_SEEDING_TIMEOUT_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_SEEDING_WAIT_FOR_COMPLETION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_SYNC_COMMIT |Attesa dell'elaborazione del commit della transazione per la finalizzazione del log da parte dei database secondari sincronizzati. Questa attesa viene inoltre riflessa dal contatore delle prestazioni Ritardo transazioni. Questo tipo di attesa è previsto per gruppi di disponibilità sincronizzati e indica il tempo di invio, scrittura e riconoscimento del log ai database secondari. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_SYNCHRONIZING_THROTTLE |Attesa dell'elaborazione del commit della transazione per consentire l'aggiornamento da parte di un database secondario di sincronizzazione alla fine del log primario per la transazione allo stato sincronizzato. Si tratta di un'attesa prevista quando un database secondario viene aggiornato. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_TDS_LISTENER_SYNC |Il sistema Always On interno o il cluster WSFC richiederà l'avvio o l'arresto dei listener. L'elaborazione di questa richiesta è sempre asincrona ed è disponibile un meccanismo di rimozione delle richieste ridondanti. Inoltre, si possono verificare momenti in cui questa elaborazione viene sospesa a causa di modifiche di configurazione. Questo tipo di attesa viene usato da tutte le attese correlate a questo meccanismo di sincronizzazione del listener. Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_TDS_LISTENER_SYNC_PROCESSING |Utilizzato alla fine di un'istruzione Transact-SQL Always On che richiede l'avvio e/o l'arresto di un listener del gruppo di disponibilità. Dal momento che l'operazione di avvio/arresto viene eseguita in modo asincrono, il thread dell'utente verrà bloccato tramite questo tipo di attesa finché la situazione del listener non sarà nota. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_THROTTLE_LOG_RATE_GOVERNOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Si verifica quando un database secondario con replica geografica è configurato con una dimensione di calcolo inferiore (SLO inferiore) rispetto al database primario. Un database primario viene limitato a causa del consumo di log ritardato da parte del database secondario. Ciò è dovuto al fatto che il database secondario non dispone di capacità di calcolo sufficienti per restare al passo con la frequenza di modifica del database primario. <br /><br /> **Si applica a**: database SQL di Azure| 
|HADR_THROTTLE_LOG_RATE_LOG_SIZE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|HADR_THROTTLE_LOG_RATE_SEEDING |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|HADR_TIMER_TASK |Attesa per ottenere il blocco sull'oggetto attività del timer, nonché per le attese effettive tra i tempi in cui il lavoro viene effettuato. Ad esempio, per un'attività che viene eseguita ogni 10 secondi, dopo un'esecuzione Always On gruppi di disponibilità attendono circa 10 secondi per la ripianificazione dell'attività e l'attesa viene inclusa qui. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_TRANSPORT_DBRLIST |Attesa dell'accesso all'elenco di repliche di database del livello di trasporto. Utilizzata per lo spinlock che ne assicura il relativo accesso. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_TRANSPORT_FLOW_CONTROL |Attesa quando il numero di messaggi di Always On non riconosciuti in attesa supera la soglia di controllo del flusso di uscita. Si tratta di una base tra repliche di disponibilità e non tra database. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_TRANSPORT_SESSION |Always On gruppi di disponibilità è in attesa durante la modifica o l'accesso allo stato di trasporto sottostante. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_WORK_POOL |Attesa del controllo della concorrenza sull'oggetto attività lavoro in background dei gruppi di disponibilità Always On. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_WORK_QUEUE |Always On thread di lavoro in background dei gruppi di disponibilità in attesa dell'assegnazione di un nuovo lavoro. Si tratta di un'attesa prevista in presenza di thread di lavoro pronti in attesa di un nuovo lavoro, vale a dire lo stato normale. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HADR_XRF_STACK_ACCESS |Accesso (ricerca, aggiunta ed eliminazione) dello stack dei fork di recupero esteso per un database di disponibilità Always On. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HCCO_CACHE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HK_RESTORE_FILEMAP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HKCS_PARALLEL_MIGRATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HKCS_PARALLEL_RECOVERY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|HTBUILD |Si verifica con piani in modalità batch paralleli quando si sincronizza la compilazione della tabella hash sul lato di input di un hash join o un'aggregazione. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HTDELETE |Si verifica con piani in modalità batch paralleli durante la sincronizzazione alla fine di un hash join/aggregazione. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|HTMEMO |Si verifica con piani in modalità batch paralleli durante la sincronizzazione prima di analizzare la tabella hash per restituire corrispondenze/non corrispondenze in hash join/aggregazione. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|HTREINIT |Si verifica con piani in modalità batch paralleli durante la sincronizzazione prima di reimpostare un hash join o un'aggregazione per il successivo join parziale. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|HTREPARTITION |Si verifica con piani in modalità batch paralleli quando si sincronizza il partizionamento della tabella hash sul lato di input di un hash join o un'aggregazione. Se l'attesa è eccessiva e non è possibile ridurla ottimizzando la query (ad esempio aggiungendo indici), regolare l'opzione Cost threshold for parallelism o abbassare il grado di parallelismo.<br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|HTTP_ENUMERATION |Si verifica all'avvio per enumerare gli endpoint HTTP per avviare HTTP.| 
|HTTP_START |Si verifica quando una connessione è in attesa che HTTP completi l'inizializzazione.| 
|HTTP_STORAGE_CONNECTION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|IMPPROV_IOWAIT |Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attende il completamento di un I/O di caricamento bulk.| 
|INSTANCE_LOG_RATE_GOVERNOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|INTERNAL_TESTING |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|IO_AUDIT_MUTEX |Si verifica durante la sincronizzazione di buffer di eventi di traccia.| 
|IO_COMPLETION |Si verifica durante l'attesa del completamento di operazioni di I/O. Questo tipo di attesa rappresenta in genere operazioni di I/O su pagine non di dati. Le attese di completamento I/O della pagina di dati vengono visualizzate come attese di PAGEIOLATCH \_ \* .| 
|IO_QUEUE_LIMIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|IO_RETRY |Si verifica quando un'operazione di I/O, quale un'operazione di lettura o scrittura sul disco, non riesce a causa di risorse insufficienti e viene ritentata l'operazione.| 
|IOAFF_RANGE_QUEUE |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|KSOURCE_WAKEUP |Viene usato dall'attività di controllo dei servizi durante l'attesa di richieste da Gestione controllo servizi. Sono previste attese prolungate, che non indicano un problema.| 
|KTM_ENLISTMENT |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|KTM_RECOVERY_MANAGER |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|KTM_RECOVERY_RESOLUTION |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|LATCH_DT |Si verifica durante l'attesa di un latch di eliminazione (DT). Non include i latch del buffer o i latch di contrassegno di transazione. \_ \* In sys.dm_os_latch_stats è disponibile un elenco di attese di latch. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.| 
|LATCH_EX |Si verifica durante l'attesa di un latch esclusivo (EX). Non include i latch del buffer o i latch di contrassegno di transazione. \_ \* In sys.dm_os_latch_stats è disponibile un elenco di attese di latch. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.| 
|LATCH_KP |Si verifica durante l'attesa di un latch conservativo (KP). Non include i latch del buffer o i latch di contrassegno di transazione. \_ \* In sys.dm_os_latch_stats è disponibile un elenco di attese di latch. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.| 
|LATCH_NL |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|LATCH_SH |Si verifica durante l'attesa di un latch di condivisione (SH). Non include i latch del buffer o i latch di contrassegno di transazione. \_ \* In sys.dm_os_latch_stats è disponibile un elenco di attese di latch. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.| 
|LATCH_UP |Si verifica durante l'attesa di un latch di aggiornamento (UP). Non include i latch del buffer o i latch di contrassegno di transazione. \_ \* In sys.dm_os_latch_stats è disponibile un elenco di attese di latch. Le attese sys.dm_os_latch_stats LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX e LATCH_DT sono raggruppate.| 
|LAZYWRITER_SLEEP |Si verifica quando le attività del writer Lazy vengono sospese. Si tratta di una misura della durata dell'attesa delle attività in background. Non considerare questo stato durante il rilevamento di stalli a livello di utente.| 
|LCK_M_BU |Si verifica quando un'attività è in attesa di acquisire un blocco aggiornamenti bulk (BU).| 
|LCK_M_BU_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco aggiornamenti bulk (BU) con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_BU_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco aggiornamenti bulk (BU) con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_IS |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo condiviso (IS).| 
|LCK_M_IS_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo condiviso (IS) con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_IS_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo condiviso (IS) con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_IU |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo di aggiornamento (IU).| 
|LCK_M_IU_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo di aggiornamento (IU) con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_IU_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo di aggiornamento (IU) con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_IX |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo esclusivo (IX).| 
|LCK_M_IX_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo esclusivo (IX) con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_IX_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco preventivo esclusivo (IX) con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_NL |Si verifica quando un'attività è in attesa di acquisire un blocco NULL per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente. Un blocco NULL per la chiave è un blocco a rilascio immediato.| 
|LCK_M_RIn_NL_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco NULL con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo di inserimento con blocchi di interruzione tra la chiave corrente e quella precedente. Un blocco NULL per la chiave è un blocco a rilascio immediato. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_NL_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco NULL con priorità bassa per il valore di chiave corrente e un blocco di intervallo di inserimento con priorità bassa tra la chiave corrente e quella precedente. Un blocco NULL per la chiave è un blocco a rilascio immediato. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_S |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente.| 
|LCK_M_RIn_S_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo di inserimento con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_S_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con priorità bassa per il valore di chiave corrente e un blocco di intervallo di inserimento con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_U |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente.| 
|LCK_M_RIn_U_ABORT_BLOCKERS |L'attività è in attesa di acquisire un blocco di aggiornamento con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo di inserimento con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_U_LOW_PRIORITY |L'attività è in attesa di acquisire un blocco di aggiornamento con priorità bassa per il valore di chiave corrente e un blocco di intervallo di inserimento con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_X |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo per il valore di chiave corrente e un blocco di intervallo di inserimento tra la chiave corrente e quella precedente.| 
|LCK_M_RIn_X_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo di inserimento con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RIn_X_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo con priorità bassa per il valore di chiave corrente e un blocco di intervallo di inserimento con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RS_S |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso per il valore di chiave corrente e un blocco di intervallo condiviso tra la chiave corrente e quella precedente.| 
|LCK_M_RS_S_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo condiviso con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RS_S_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con priorità bassa per il valore di chiave corrente e un blocco di intervallo condiviso con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RS_U |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento per il valore di chiave corrente e un blocco di intervallo di aggiornamento tra la chiave corrente e quella precedente.| 
|LCK_M_RS_U_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo di aggiornamento con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RS_U_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento con priorità bassa per il valore di chiave corrente e un blocco di intervallo di aggiornamento con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RX_S |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso per il valore di chiave corrente e un blocco di intervallo esclusivo tra la chiave corrente e quella precedente.| 
|LCK_M_RX_S_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo esclusivo con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RX_S_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con priorità bassa per il valore di chiave corrente e un blocco di intervallo esclusivo con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RX_U |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento per il valore di chiave corrente e un blocco di intervallo esclusivo tra la chiave corrente e quella precedente.| 
|LCK_M_RX_U_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo esclusivo con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RX_U_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento con priorità bassa per il valore di chiave corrente e un blocco di intervallo esclusivo con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RX_X |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo per il valore di chiave corrente e un blocco di intervallo esclusivo tra la chiave corrente e quella precedente.| 
|LCK_M_RX_X_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo con blocchi di interruzione per il valore di chiave corrente e un blocco di intervallo esclusivo con blocchi di interruzione tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_RX_X_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo con priorità bassa per il valore di chiave corrente e un blocco di intervallo esclusivo con priorità bassa tra la chiave corrente e quella precedente. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_S |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso.| 
|LCK_M_S_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_S_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SCH_M |Si verifica quando un'attività è in attesa di acquisire un blocco di modifica dello schema.| 
|LCK_M_SCH_M_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco di modifica dello schema con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SCH_M_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco di modifica dello schema con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SCH_S |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso dello schema.| 
|LCK_M_SCH_S_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco di condivisione dello schema con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SCH_S_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco di condivisione dello schema con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SIU |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo di aggiornamento.| 
|LCK_M_SIU_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo di aggiornamento con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SIU_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo di aggiornamento con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SIX |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo esclusivo.| 
|LCK_M_SIX_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo esclusivo con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_SIX_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco condiviso preventivo esclusivo con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_U |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento.| 
|LCK_M_U_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_U_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_UIX |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento preventivo esclusivo.| 
|LCK_M_UIX_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento preventivo esclusivo con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_UIX_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco di aggiornamento preventivo esclusivo con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_X |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo.| 
|LCK_M_X_ABORT_BLOCKERS |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo con blocchi di interruzione. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LCK_M_X_LOW_PRIORITY |Si verifica quando un'attività è in attesa di acquisire un blocco esclusivo con priorità bassa. (Correlato all'opzione di attesa con priorità bassa di ALTER TABLE e ALTER INDEX.), <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|LOG_POOL_SCAN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|LOG_RATE_GOVERNOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|LOGBUFFER |Si verifica quando un'attività è in attesa di spazio nel buffer del log per l'archiviazione di un record di log. Valori costantemente alti possono indicare che i dispositivi di log non sono in grado di far fronte alla quantità di log generati dal server.| 
|LOGCAPTURE_LOGPOOLTRUNCPOINT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGGENERATION |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|LOGMGR |Si verifica quando un'attività è in attesa del completamento di qualsiasi operazione di I/O di log in sospeso prima di chiudere il log durante la chiusura del database.| 
|LOGMGR_FLUSH |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|LOGMGR_PMM_LOG |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|LOGMGR_QUEUE |Si verifica quando l'attività di scrittura del log è in attesa di richieste di lavoro.| 
|LOGMGR_RESERVE_APPEND |Si verifica quando un'attività è in attesa di verificare se il troncamento del log libera spazio di log per consentire all'attività di scrivere un nuovo record di log. Valutare la possibilità di aumentare le dimensioni dei file di log per il database interessato allo scopo di ridurre l'attesa.| 
|LOGPOOL_CACHESIZE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGPOOL_CONSUMER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGPOOL_CONSUMERSET |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGPOOL_FREEPOOLS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGPOOL_MGRSET |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGPOOL_REPLACEMENTSET |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOGPOOLREFCOUNTEDOBJECT_REFDONE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|LOWFAIL_MEMMGR_QUEUE |Si verifica durante l'attesa di memoria disponibile per l'utilizzo.| 
|MD_AGENT_YIELD |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|MD_LAZYCACHE_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|MEMORY_ALLOCATION_EXT |Si verifica durante l'allocazione della memoria dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pool di memoria interno o dal sistema operativo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|MEMORY_GRANT_UPDATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|METADATA_LAZYCACHE_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|MIGRATIONBUFFER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|MISCELLANEOUS |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|MISCELLANEOUS |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|MSQL_DQ |Si verifica quando un'attività è in attesa del completamento di un'operazione di query distribuita. Viene usato per rilevare potenziali deadlock di applicazioni MARS (Multiple Active Result Set). L'attesa si arresta al completamento della chiamata della query distribuita.| 
|MSQL_XACT_MGR_MUTEX |Si verifica quando un'attività è in attesa di acquisire la proprietà della gestione transazioni della sessione per eseguire un'operazione di transazione a livello di sessione.| 
|MSQL_XACT_MUTEX |Si verifica durante la sincronizzazione dell'utilizzo della transazione. Per poter usare la transazione, una richiesta deve prima acquisire il mutex.| 
|MSQL_XP |Si verifica quando un'attività è in attesa del completamento di una stored procedure estesa. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza questo stato di attesa per rilevare potenziali deadlock di applicazioni MARS. L'attesa si arresta al completamento della chiamata della stored procedure estesa.| 
|MSSEARCH |Si verifica durante le chiamate di ricerca full-text. L'attesa si arresta al completamento dell'operazione full-text. Non indica contesa, bensì la durata delle operazioni full-text.| 
|NET_WAITFOR_PACKET |Si verifica quando una connessione è in attesa di un pacchetto di rete durante una lettura in rete.| 
|NETWORKSXMLMGRLOAD |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|NODE_CACHE_MUTEX |Solo per uso interno.| 
|OLEDB |Si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiama il provider di OLE DB SNAC (SQLNCLI) o il driver Microsoft OLE DB per SQL Server (MSOLEDBSQL). Questo tipo di attesa non viene usato per la sincronizzazione. Indica invece la durata delle chiamate al provider OLE DB.| 
|ONDEMAND_TASK_QUEUE |Si verifica quando un'attività in background è in attesa di richieste di attività di sistema con priorità elevata. Tempi di attesa prolungati indicano l'assenza di richieste con priorità elevata da elaborare e non costituiscono un problema.| 
|PAGEIOLATCH_DT |Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità di eliminazione. Attese prolungate possono indicare problemi con il sottosistema disco.| 
|PAGEIOLATCH_EX |Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità esclusiva, ovvero una modalità usata quando il buffer viene scritto su disco. Attese prolungate possono indicare problemi con il sottosistema disco.| 
|PAGEIOLATCH_KP |Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità di mantenimento. Attese prolungate possono indicare problemi con il sottosistema disco.| 
|PAGEIOLATCH_NL |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PAGEIOLATCH_SH |Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità condivisa, ovvero una modalità usata quando il buffer viene letto dal disco. Attese prolungate possono indicare problemi con il sottosistema disco.| 
|PAGEIOLATCH_UP |Si verifica quando un'attività è in attesa di un latch per un buffer in una richiesta di I/O. La richiesta di latch è in modalità di aggiornamento. Attese prolungate possono indicare problemi con il sottosistema disco.| 
|PAGELATCH_DT |Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità di eliminazione. Prima di eliminare il contenuto di una pagina, è necessario acquisire la modalità di eliminazione. Per ulteriori informazioni, vedere [latch modes](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) .| 
|PAGELATCH_EX |Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità esclusiva. impedisce ad altri thread di scrivere o leggere dalla pagina (buffer). </br></br> Uno scenario comune che conduce a questo latch è la contesa di latch del buffer "Last-page Insert". Per comprendere e risolvere questo problema, usare Risolvi la contesa di [inserimento PAGELATCH_EX Ultima pagina](/troubleshoot/sql/performance/resolve-pagelatch-ex-contention) e la risoluzione dei conflitti di [inserimento dell'ultima pagina nella SQL Server](../diagnose-resolve-latch-contention.md#last-pagetrailing-page-insert-contention). Un altro scenario è la [contesa di latch per le tabelle di piccole dimensioni con un indice non cluster e inserimenti casuali (tabella della coda)](../diagnose-resolve-latch-contention.md#latch-contention-on-small-tables-with-a-non-clustered-index-and-random-inserts-queue-table).| 
|PAGELATCH_KP |Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità di mantenimento che impedisce la distruzione della pagina da parte di un altro thread. Per ulteriori informazioni, vedere [latch modes](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) .| 
|PAGELATCH_NL |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PAGELATCH_SH |Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità condivisa che consente a più thread di leggere, ma non modificare, un buffer (pagina). Per ulteriori informazioni, vedere [latch modes](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) .| 
|PAGELATCH_UP |Si verifica quando un'attività è in attesa di un latch per un buffer non incluso in una richiesta di I/O. La richiesta di latch è in modalità di aggiornamento. Questo tipo di attesa può essere comunemente osservato quando una pagina di sistema (buffer) come PFS, GAM, SGAM è bloccato. Per ulteriori informazioni, vedere [latch modes](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) . </br></br> Per la risoluzione di uno scenario comune con questo latch, vedere [ridurre i conflitti di allocazione nel database SQL Server tempdb](/troubleshoot/sql/performance/recommendations-reduce-allocation-contention).| 
|PARALLEL_BACKUP_QUEUE |Si verifica durante la serializzazione dell'output generato da RESTORE HEADERONLY, RESTORE FILELISTONLY o RESTORE LABELONLY.| 
|PARALLEL_REDO_DRAIN_WORKER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PARALLEL_REDO_FLOW_CONTROL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PARALLEL_REDO_LOG_CACHE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PARALLEL_REDO_TRAN_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PARALLEL_REDO_TRAN_TURN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PARALLEL_REDO_WORKER_SYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PARALLEL_REDO_WORKER_WAIT_WORK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PERFORMANCE_COUNTERS_RWLOCK |Solo per uso interno.| 
|PHYSICAL_SEEDING_DMV |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|POOL_LOG_RATE_GOVERNOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PREEMPTIVE_ABR |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG |Si verifica quando l'utilità di pianificazione del sistema operativo di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] (SQLOS) passa alla modalità preemptive per scrivere un evento di controllo nel registro eventi di Windows. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG |Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per scrivere un evento di controllo nel registro di sicurezza di Windows. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|PREEMPTIVE_CLOSEBACKUPMEDIA |Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per chiudere i supporti di backup.| 
|PREEMPTIVE_CLOSEBACKUPTAPE |Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per chiudere un dispositivo di backup su nastro.| 
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE |Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per chiudere un dispositivo di backup virtuale.| 
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL |Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per eseguire operazioni di cluster di failover di Windows.| 
|PREEMPTIVE_COM_COCREATEINSTANCE |Si verifica quando l'utilità di pianificazione del sistema operativo di SQL Server passa alla modalità preemptive per creare un oggetto COM.| 
|PREEMPTIVE_COM_COGETCLASSOBJECT |Solo per uso interno.| 
|PREEMPTIVE_COM_CREATEACCESSOR |Solo per uso interno.| 
|PREEMPTIVE_COM_DELETEROWS |Solo per uso interno.| 
|PREEMPTIVE_COM_GETCOMMANDTEXT |Solo per uso interno.| 
|PREEMPTIVE_COM_GETDATA |Solo per uso interno.| 
|PREEMPTIVE_COM_GETNEXTROWS |Solo per uso interno.| 
|PREEMPTIVE_COM_GETRESULT |Solo per uso interno.| 
|PREEMPTIVE_COM_GETROWSBYBOOKMARK |Solo per uso interno.| 
|PREEMPTIVE_COM_LBFLUSH |Solo per uso interno.| 
|PREEMPTIVE_COM_LBLOCKREGION |Solo per uso interno.| 
|PREEMPTIVE_COM_LBREADAT |Solo per uso interno.| 
|PREEMPTIVE_COM_LBSETSIZE |Solo per uso interno.| 
|PREEMPTIVE_COM_LBSTAT |Solo per uso interno.| 
|PREEMPTIVE_COM_LBUNLOCKREGION |Solo per uso interno.| 
|PREEMPTIVE_COM_LBWRITEAT |Solo per uso interno.| 
|PREEMPTIVE_COM_QUERYINTERFACE |Solo per uso interno.| 
|PREEMPTIVE_COM_RELEASE |Solo per uso interno.| 
|PREEMPTIVE_COM_RELEASEACCESSOR |Solo per uso interno.| 
|PREEMPTIVE_COM_RELEASEROWS |Solo per uso interno.| 
|PREEMPTIVE_COM_RELEASESESSION |Solo per uso interno.| 
|PREEMPTIVE_COM_RESTARTPOSITION |Solo per uso interno.| 
|PREEMPTIVE_COM_SEQSTRMREAD |Solo per uso interno.| 
|PREEMPTIVE_COM_SEQSTRMREADANDWRITE |Solo per uso interno.| 
|PREEMPTIVE_COM_SETDATAFAILURE |Solo per uso interno.| 
|PREEMPTIVE_COM_SETPARAMETERINFO |Solo per uso interno.| 
|PREEMPTIVE_COM_SETPARAMETERPROPERTIES |Solo per uso interno.| 
|PREEMPTIVE_COM_STRMLOCKREGION |Solo per uso interno.| 
|PREEMPTIVE_COM_STRMSEEKANDREAD |Solo per uso interno.| 
|PREEMPTIVE_COM_STRMSEEKANDWRITE |Solo per uso interno.| 
|PREEMPTIVE_COM_STRMSETSIZE |Solo per uso interno.| 
|PREEMPTIVE_COM_STRMSTAT |Solo per uso interno.| 
|PREEMPTIVE_COM_STRMUNLOCKREGION |Solo per uso interno.| 
|PREEMPTIVE_CONSOLEWRITE |Solo per uso interno.| 
|PREEMPTIVE_CREATEPARAM |Solo per uso interno.| 
|PREEMPTIVE_DEBUG |Solo per uso interno.| 
|PREEMPTIVE_DFSADDLINK |Solo per uso interno.| 
|PREEMPTIVE_DFSLINKEXISTCHECK |Solo per uso interno.| 
|PREEMPTIVE_DFSLINKHEALTHCHECK |Solo per uso interno.| 
|PREEMPTIVE_DFSREMOVELINK |Solo per uso interno.| 
|PREEMPTIVE_DFSREMOVEROOT |Solo per uso interno.| 
|PREEMPTIVE_DFSROOTFOLDERCHECK |Solo per uso interno.| 
|PREEMPTIVE_DFSROOTINIT |Solo per uso interno.| 
|PREEMPTIVE_DFSROOTSHARECHECK |Solo per uso interno.| 
|PREEMPTIVE_DTC_ABORT |Solo per uso interno.| 
|PREEMPTIVE_DTC_ABORTREQUESTDONE |Solo per uso interno.| 
|PREEMPTIVE_DTC_BEGINTRANSACTION |Solo per uso interno.| 
|PREEMPTIVE_DTC_COMMITREQUESTDONE |Solo per uso interno.| 
|PREEMPTIVE_DTC_ENLIST |Solo per uso interno.| 
|PREEMPTIVE_DTC_PREPAREREQUESTDONE |Solo per uso interno.| 
|PREEMPTIVE_FILESIZEGET |Solo per uso interno.| 
|PREEMPTIVE_FSAOLEDB_ABORTTRANSACTION |Solo per uso interno.| 
|PREEMPTIVE_FSAOLEDB_COMMITTRANSACTION |Solo per uso interno.| 
|PREEMPTIVE_FSAOLEDB_STARTTRANSACTION |Solo per uso interno.| 
|PREEMPTIVE_FSRECOVER_UNCONDITIONALUNDO |Solo per uso interno.| 
|PREEMPTIVE_GETRMINFO |Solo per uso interno.| 
|PREEMPTIVE_HADR_LEASE_MECHANISM |Pianificazione della gestione dei lease dei gruppi di disponibilità Always On per la diagnostica supporto tecnico Microsoft. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PREEMPTIVE_HTTP_EVENT_WAIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PREEMPTIVE_HTTP_REQUEST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PREEMPTIVE_LOCKMONITOR |Solo per uso interno.| 
|PREEMPTIVE_MSS_RELEASE |Solo per uso interno.| 
|PREEMPTIVE_ODBCOPS |Solo per uso interno.| 
|PREEMPTIVE_OLE_UNINIT |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_ABORTORCOMMITTRAN |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_ABORTTRAN |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_GETDATASOURCE |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_GETLITERALINFO |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_GETPROPERTIES |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_GETPROPERTYINFO |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_GETSCHEMALOCK |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_JOINTRANSACTION |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_RELEASE |Solo per uso interno.| 
|PREEMPTIVE_OLEDB_SETPROPERTIES |Solo per uso interno.| 
|PREEMPTIVE_OLEDBOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_ACCEPTSECURITYCONTEXT |Solo per uso interno.| 
|PREEMPTIVE_OS_ACQUIRECREDENTIALSHANDLE |Solo per uso interno.| 
|PREEMPTIVE_OS_AUTHENTICATIONOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_AUTHORIZATIONOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_AUTHZGETINFORMATIONFROMCONTEXT |Solo per uso interno.| 
|PREEMPTIVE_OS_AUTHZINITIALIZECONTEXTFROMSID |Solo per uso interno.| 
|PREEMPTIVE_OS_AUTHZINITIALIZERESOURCEMANAGER |Solo per uso interno.| 
|PREEMPTIVE_OS_BACKUPREAD |Solo per uso interno.| 
|PREEMPTIVE_OS_CLOSEHANDLE |Solo per uso interno.| 
|PREEMPTIVE_OS_CLUSTEROPS |Solo per uso interno.| 
|PREEMPTIVE_OS_COMOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_COMPLETEAUTHTOKEN |Solo per uso interno.| 
|PREEMPTIVE_OS_COPYFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_CREATEDIRECTORY |Solo per uso interno.| 
|PREEMPTIVE_OS_CREATEFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_CRYPTACQUIRECONTEXT |Solo per uso interno.| 
|PREEMPTIVE_OS_CRYPTIMPORTKEY |Solo per uso interno.| 
|PREEMPTIVE_OS_CRYPTOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_DECRYPTMESSAGE |Solo per uso interno.| 
|PREEMPTIVE_OS_DELETEFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_DELETESECURITYCONTEXT |Solo per uso interno.| 
|PREEMPTIVE_OS_DEVICEIOCONTROL |Solo per uso interno.| 
|PREEMPTIVE_OS_DEVICEOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_DIRSVC_NETWORKOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_DISCONNECTNAMEDPIPE |Solo per uso interno.| 
|PREEMPTIVE_OS_DOMAINSERVICESOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_DSGETDCNAME |Solo per uso interno.| 
|PREEMPTIVE_OS_DTCOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_ENCRYPTMESSAGE |Solo per uso interno.| 
|PREEMPTIVE_OS_FILEOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_FINDFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_FLUSHFILEBUFFERS |Solo per uso interno.| 
|PREEMPTIVE_OS_FORMATMESSAGE |Solo per uso interno.| 
|PREEMPTIVE_OS_FREECREDENTIALSHANDLE |Solo per uso interno.| 
|PREEMPTIVE_OS_FREELIBRARY |Solo per uso interno.| 
|PREEMPTIVE_OS_GENERICOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_GETADDRINFO |Solo per uso interno.| 
|PREEMPTIVE_OS_GETCOMPRESSEDFILESIZE |Solo per uso interno.| 
|PREEMPTIVE_OS_GETDISKFREESPACE |Solo per uso interno.| 
|PREEMPTIVE_OS_GETFILEATTRIBUTES |Solo per uso interno.| 
|PREEMPTIVE_OS_GETFILESIZE |Solo per uso interno.| 
|PREEMPTIVE_OS_GETFINALFILEPATHBYHANDLE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PREEMPTIVE_OS_GETLONGPATHNAME |Solo per uso interno.| 
|PREEMPTIVE_OS_GETPROCADDRESS |Solo per uso interno.| 
|PREEMPTIVE_OS_GETVOLUMENAMEFORVOLUMEMOUNTPOINT |Solo per uso interno.| 
|PREEMPTIVE_OS_GETVOLUMEPATHNAME |Solo per uso interno.| 
|PREEMPTIVE_OS_INITIALIZESECURITYCONTEXT |Solo per uso interno.| 
|PREEMPTIVE_OS_LIBRARYOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_LOADLIBRARY |Solo per uso interno.| 
|PREEMPTIVE_OS_LOGONUSER |Solo per uso interno.| 
|PREEMPTIVE_OS_LOOKUPACCOUNTSID |Solo per uso interno.| 
|PREEMPTIVE_OS_MESSAGEQUEUEOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_MOVEFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_NETGROUPGETUSERS |Solo per uso interno.| 
|PREEMPTIVE_OS_NETLOCALGROUPGETMEMBERS |Solo per uso interno.| 
|PREEMPTIVE_OS_NETUSERGETGROUPS |Solo per uso interno.| 
|PREEMPTIVE_OS_NETUSERGETLOCALGROUPS |Solo per uso interno.| 
|PREEMPTIVE_OS_NETUSERMODALSGET |Solo per uso interno.| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICY |Solo per uso interno.| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICYFREE |Solo per uso interno.| 
|PREEMPTIVE_OS_OPENDIRECTORY |Solo per uso interno.| 
|PREEMPTIVE_OS_PDH_WMI_INIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PREEMPTIVE_OS_PIPEOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_PROCESSOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_QUERYCONTEXTATTRIBUTES |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PREEMPTIVE_OS_QUERYREGISTRY |Solo per uso interno.| 
|PREEMPTIVE_OS_QUERYSECURITYCONTEXTTOKEN |Solo per uso interno.| 
|PREEMPTIVE_OS_REMOVEDIRECTORY |Solo per uso interno.| 
|PREEMPTIVE_OS_REPORTEVENT |Solo per uso interno.| 
|PREEMPTIVE_OS_REVERTTOSELF |Solo per uso interno.| 
|PREEMPTIVE_OS_RSFXDEVICEOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_SECURITYOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_SERVICEOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_SETENDOFFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_SETFILEPOINTER |Solo per uso interno.| 
|PREEMPTIVE_OS_SETFILEVALIDDATA |Solo per uso interno.| 
|PREEMPTIVE_OS_SETNAMEDSECURITYINFO |Solo per uso interno.| 
|PREEMPTIVE_OS_SQLCLROPS |Solo per uso interno.| 
|PREEMPTIVE_OS_SQMLAUNCH |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] tramite [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]. |  
|PREEMPTIVE_OS_VERIFYSIGNATURE |Solo per uso interno.| 
|PREEMPTIVE_OS_VERIFYTRUST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PREEMPTIVE_OS_VSSOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_WAITFORSINGLEOBJECT |Solo per uso interno.| 
|PREEMPTIVE_OS_WINSOCKOPS |Solo per uso interno.| 
|PREEMPTIVE_OS_WRITEFILE |Solo per uso interno.| 
|PREEMPTIVE_OS_WRITEFILEGATHER |Solo per uso interno.| 
|PREEMPTIVE_OS_WSASETLASTERROR |Solo per uso interno.| 
|PREEMPTIVE_REENLIST |Solo per uso interno.| 
|PREEMPTIVE_RESIZELOG |Solo per uso interno.| 
|PREEMPTIVE_ROLLFORWARDREDO |Solo per uso interno.| 
|PREEMPTIVE_ROLLFORWARDUNDO |Solo per uso interno.| 
|PREEMPTIVE_SB_STOPENDPOINT |Solo per uso interno.| 
|PREEMPTIVE_SERVER_STARTUP |Solo per uso interno.| 
|PREEMPTIVE_SETRMINFO |Solo per uso interno.| 
|PREEMPTIVE_SHAREDMEM_GETDATA |Solo per uso interno.| 
|PREEMPTIVE_SNIOPEN |Solo per uso interno.| 
|PREEMPTIVE_SOSHOST |Solo per uso interno.| 
|PREEMPTIVE_SOSTESTING |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PREEMPTIVE_SP_SERVER_DIAGNOSTICS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PREEMPTIVE_STARTRM |Solo per uso interno.| 
|PREEMPTIVE_STREAMFCB_CHECKPOINT |Solo per uso interno.| 
|PREEMPTIVE_STREAMFCB_RECOVER |Solo per uso interno.| 
|PREEMPTIVE_STRESSDRIVER |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PREEMPTIVE_TESTING |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PREEMPTIVE_TRANSIMPORT |Solo per uso interno.| 
|PREEMPTIVE_UNMARSHALPROPAGATIONTOKEN |Solo per uso interno.| 
|PREEMPTIVE_VSS_CREATESNAPSHOT |Solo per uso interno.| 
|PREEMPTIVE_VSS_CREATEVOLUMESNAPSHOT |Solo per uso interno.| 
|PREEMPTIVE_XE_CALLBACKEXECUTE |Solo per uso interno.| 
|PREEMPTIVE_XE_CX_FILE_OPEN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|PREEMPTIVE_XE_CX_HTTP_CALL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|PREEMPTIVE_XE_DISPATCHER |Solo per uso interno.| 
|PREEMPTIVE_XE_ENGINEINIT |Solo per uso interno.| 
|PREEMPTIVE_XE_GETTARGETSTATE |Solo per uso interno.| 
|PREEMPTIVE_XE_SESSIONCOMMIT |Solo per uso interno.| 
|PREEMPTIVE_XE_TARGETFINALIZE |Solo per uso interno.| 
|PREEMPTIVE_XE_TARGETINIT |Solo per uso interno.| 
|PREEMPTIVE_XE_TIMERRUN |Solo per uso interno.| 
|PREEMPTIVE_XETESTING |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|PRINT_ROLLBACK_PROGRESS |Utilizzato per l'attesa durante la conclusione di processi utente in un database in cui si è verificata una transizione tramite la clausola di terminazione ALTER DATABASE. Per altre informazioni, vedere ALTER DATABASE (Transact-SQL).| 
|PRU_ROLLBACK_DEFERRED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_ALL_COMPONENTS_INITIALIZED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_COOP_SCAN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_DIRECTLOGCONSUMER_GETNEXT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PWAIT_EVENT_SESSION_INIT_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_FABRIC_REPLICA_CONTROLLER_DATA_LOSS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PWAIT_HADR_ACTION_COMPLETED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC |Si verifica quando un'attività in background è in attesa del completamento dell'attività in background mediante cui vengono ricevute (tramite polling) notifiche di Windows Server Failover Clustering. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_CLUSTER_INTEGRATION |Un'operazione di Accodamento, sostituzione e/o rimozione è in attesa di acquisire un blocco di scrittura su un Always On elenco interno, ad esempio un elenco di reti, indirizzi di rete o listener del gruppo di disponibilità. Solo per uso interno, <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_FAILOVER_COMPLETED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_JOIN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|PWAIT_HADR_OFFLINE_COMPLETED |Un'operazione di eliminazione del gruppo di disponibilità Always On è in attesa del passaggio alla modalità offline del gruppo di disponibilità di destinazione prima dell'eliminazione degli oggetti di Windows Server failover clustering. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_ONLINE_COMPLETED |Un'operazione di creazione o failover di un gruppo di disponibilità Always On è in attesa che il gruppo di disponibilità di destinazione torni in linea. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_POST_ONLINE_COMPLETED |Un'operazione di eliminazione del gruppo di disponibilità Always On è in attesa della chiusura di qualsiasi attività in background pianificata come parte di un comando precedente. Ad esempio, è possibile che sia in corso la transizione al ruolo primario di database di disponibilità di un'attività in background. Per evitare situazioni di race condition è necessaria l'attesa del completamento dell'attività in background da parte di DROP AVAILABILITY GROUP DDL. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_SERVER_READY_CONNECTIONS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADR_WORKITEM_COMPLETED |Attesa interna da un thread in attesa del completamento di un'attività di lavoro asincrona. Si tratta di un'attesa prevista e consente l'utilizzo di CSS. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_HADRSIM |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|PWAIT_LOG_CONSOLIDATION_IO |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|PWAIT_LOG_CONSOLIDATION_POLL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|PWAIT_MD_LOGIN_STATS |Si verifica durante la sincronizzazione interna nei metadati per le statistiche di accesso. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_MD_RELATION_CACHE |Si verifica durante la sincronizzazione interna nei metadati per la tabella o l'indice. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_MD_SERVER_CACHE |Si verifica durante la sincronizzazione interna nei metadati per i server collegati. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_MD_UPGRADE_CONFIG |Si verifica durante la sincronizzazione interna nell'aggiornamento delle configurazioni per l'intero server. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_PREEMPTIVE_APP_USAGE_TIMER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|PWAIT_PREEMPTIVE_AUDIT_ACCESS_WINDOWSLOG |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_QRY_BPMEMORY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_REPLICA_ONLINE_INIT_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|PWAIT_SBS_FILE_OPERATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|PWAIT_XTP_FSSTORAGE_MAINTENANCE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|PWAIT_XTP_HOST_STORAGE_WAIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_ASYNC_CHECK_CONSISTENCY_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_ASYNC_PERSIST_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_ASYNC_PERSIST_TASK_START |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_ASYNC_QUEUE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|QDS_BCKG_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_BLOOM_FILTER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|QDS_CLEANUP_STALE_QUERIES_TASK_MAIN_LOOP_SLEEP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_CTXS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_DB_DISK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_DYN_VECTOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_EXCLUSIVE_ACCESS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|QDS_HOST_INIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|QDS_LOADDB |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_PERSIST_TASK_MAIN_LOOP_SLEEP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_QDS_CAPTURE_INIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|QDS_SHUTDOWN_QUEUE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_STMT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_STMT_DISK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_TASK_SHUTDOWN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QDS_TASK_START |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QE_WARN_LIST_SYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|QPJOB_KILL |Indica che un aggiornamento automatico asincrono delle statistiche è stato annullato da una chiamata al comando KILL all'avvio dell'esecuzione dell'aggiornamento. Il thread di interruzione viene sospeso e rimarrà in attesa dei comandi KILL. Un valore ottimale è minore di un secondo.| 
|QPJOB_WAITFOR_ABORT |Indica che un aggiornamento automatico asincrono delle statistiche è stato annullato da una chiamata al comando KILL durante l'esecuzione. L'aggiornamento è completato ma viene sospeso fino al completamento del coordinamento dei messaggi dei thread di interruzione. Questo stato è comune ma si verifica raramente e dovrebbe essere molto breve. Un valore ottimale è minore di un secondo.| 
|QRY_MEM_GRANT_INFO_MUTEX |Si verifica quando la gestione della memoria dell'esecuzione di query cerca di controllare l'accesso all'elenco di informazioni statiche sulle concessioni. Questo stato elenca le informazioni sulle richieste di memoria correnti concesse e in attesa. Questo stato rappresenta un semplice stato di controllo dell'accesso. In questo stato non si dovrebbero mai verificare attese lunghe. Se questo mutex non viene rilasciato, tutte le nuove query che utilizzano memoria non risponderanno più.| 
|QRY_PARALLEL_THREAD_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|QRY_PROFILE_LIST_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|QUERY_ERRHDL_SERVICE_DONE |Identificato solo a scopo informativo. Non supportata. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|QUERY_WAIT_ERRHDL_SERVICE |Identificato solo a scopo informativo.  Non supportata. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo.  |  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN |Si verifica in determinati casi in cui la compilazione di indici offline viene eseguita in parallelo e i diversi thread di lavoro in cui viene eseguito l'ordinamento sincronizzano l'accesso ai file di ordinamento.| 
|QUERY_NOTIFICATION_MGR_MUTEX |Si verifica durante la sincronizzazione della coda di Garbage Collection nell'utilità di gestione delle notifiche delle query.| 
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX |Si verifica durante la sincronizzazione dello stato per le transazioni nelle notifiche delle query.| 
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX |Si verifica durante la sincronizzazione interna nell'utilità di gestione delle notifiche delle query.| 
|QUERY_NOTIFICATION_UNITTEST_MUTEX |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|QUERY_OPTIMIZER_PRINT_MUTEX |Si verifica durante la sincronizzazione della produzione di output di dati diagnostici di Query Optimizer. Questo tipo di attesa si verifica solo se le impostazioni di diagnostica sono state abilitate in direzione del supporto tecnico Microsoft.| 
|QUERY_TASK_ENQUEUE_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|QUERY_TRACEOUT |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|RBIO_WAIT_VLF |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|RBIO_RG_STORAGE |Si verifica quando un nodo di calcolo di un database iperscalato viene limitato a causa del consumo di log ritardato nei server di pagine. <br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure.|
|RBIO_RG_DESTAGE |Si verifica quando un nodo di calcolo di un database iperscalato viene limitato a causa del consumo di log ritardato per l'archiviazione dei log a lungo termine. <br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure.|
|RBIO_RG_REPLICA |Si verifica quando un nodo di calcolo di un database iperscalato viene limitato a causa del consumo di log ritardato da parte dei nodi della replica secondaria leggibile. <br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure.|
|RBIO_RG_LOCALDESTAGE |Si verifica quando un nodo di calcolo di un database iperscalato viene limitato a causa del consumo di log ritardato da parte del servizio log. <br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure.|
|RECOVER_CHANGEDB |Si verifica durante la sincronizzazione dello stato del database in modalità standby a caldo (warm standby).| 
|RECOVERY_MGR_LOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|REDO_THREAD_PENDING_WORK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|REDO_THREAD_SYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|REMOTE_BLOCK_IO |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|REMOTE_DATA_ARCHIVE_MIGRATION_DMV |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|REMOTE_DATA_ARCHIVE_SCHEMA_DMV |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|REMOTE_DATA_ARCHIVE_SCHEMA_TASK_QUEUE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|REPL_CACHE_ACCESS |Si verifica durante la sincronizzazione in una cache degli articoli di replica. Durante queste attese si riscontra uno stallo nella lettura del log delle repliche e il blocco delle istruzioni DDL (Data Definition Language) per una tabella pubblicata.| 
|REPL_HISTORYCACHE_ACCESS |Solo per uso interno.| 
|REPL_SCHEMA_ACCESS |Si verifica durante la sincronizzazione delle informazioni sulla versione dello schema di replica. Questo stato esiste quando vengono eseguite istruzioni DDL sull'oggetto replicato e quando nella lettura del log viene compilato o usato uno schema con versione basato sull'occorrenza DDL. I conflitti possono essere visualizzati in questo tipo di attesa se si dispone di molti database pubblicati in un singolo server di pubblicazione con replica transazionale e se i database pubblicati sono molto attivi.| 
|REPL_TRANFSINFO_ACCESS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|REPL_TRANHASHTABLE_ACCESS |Solo per uso interno.| 
|REPL_TRANTEXTINFO_ACCESS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|REPLICA_WRITES |Si verifica quando un'attività è in attesa del completamento di scritture di pagina in snapshot di database o repliche DBCC.| 
|REQUEST_DISPENSER_PAUSE |Si verifica quando un'attività è in attesa del completamento di tutte le operazioni di I/O in sospeso, per poter bloccare l'I/O in un file per il backup snapshot.| 
|REQUEST_FOR_DEADLOCK_SEARCH |Si verifica quando il monitoraggio dei deadlock è in attesa di avviare la successiva ricerca di deadlock. L'attesa tra rilevamenti di deadlock è prevista e un tempo di attesa totale prolungato per questa risorsa non indica un problema.| 
|RESERVED_MEMORY_ALLOCATION_EXT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|RESMGR_THROTTLED |Si verifica quando giunge una nuova richiesta rallentata in base all'impostazione GROUP_MAX_REQUESTS.| 
|RESOURCE_GOVERNOR_IDLE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|RESOURCE_QUEUE |Si verifica durante la sincronizzazione di diverse code di risorse interne.| 
|RESOURCE_SEMAPHORE |Si verifica quando una richiesta di memoria di query durante l'esecuzione di query non può essere concessa immediatamente a causa di altre query simultanee. Attese e tempi di attesa rilevanti possono indicare un numero eccessivo di query simultanee o quantità eccessive di richieste di memoria. Un numero eccessivo di attese di questo tipo può generare l' [errore SQL 8645](../errors-events/mssqlserver-8645-database-engine-error.md)"si è verificato un timeout durante l'attesa di risorse di memoria per l'esecuzione della query. Eseguire di nuovo la query ".<br /><br /> Per informazioni dettagliate e per la risoluzione dei problemi relativi alle attese di concessione di memoria, vedere questo [post di Blog](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-the-mysterious-sql-server-memory-consumer-with/ba-p/333994)| 
|RESOURCE_SEMAPHORE_MUTEX |Si verifica quando una query è in attesa che venga soddisfatta la richiesta di prenotazione di thread, nonché durante la sincronizzazione di richieste di compilazione di query e concessione di memoria.| 
|RESOURCE_SEMAPHORE_QUERY_COMPILE |Si verifica quando il numero di compilazioni di query simultanee raggiunge un limite massimo. Attese e tempi di attesa elevati possono indicare un numero eccessivo di compilazioni, ricompilazioni o piani non memorizzabili nella cache.| 
|RESOURCE_SEMAPHORE_SMALL_QUERY |Si verifica quando una richiesta di memoria per una query di dimensioni ridotte non può essere concessa immediatamente a causa di altre query simultanee. Il tempo di attesa non deve superare pochi secondi, poiché se non è in grado di concedere la memoria richiesta entro pochi secondi il server trasferisce la richiesta al pool di memoria per query principale. Attese rilevanti possono indicare un numero eccessivo di query di dimensioni ridotte simultanee mentre il pool di memoria principale è bloccato da query in attesa. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|RESTORE_FILEHANDLECACHE_ENTRYLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|RESTORE_FILEHANDLECACHE_LOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|RG_RECONFIG |Solo per uso interno.| 
|ROWGROUP_OP_STATS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|ROWGROUP_VERSION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|RTDATA_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|SATELLITE_CARGO |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SATELLITE_SERVICE_SETUP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SATELLITE_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SBS_DISPATCH |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|SBS_RECEIVE_TRANSPORT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|SBS_TRANSPORT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|SCAN_CHAR_HASH_ARRAY_INITIALIZATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SEC_DROP_TEMP_KEY |Si verifica dopo un tentativo non riuscito di eliminare una chiave di sicurezza temporanea, prima di un nuovo tentativo.| 
|SECURITY_CNG_PROVIDER_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|SECURITY_CRYPTO_CONTEXT_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SECURITY_DBE_STATE_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SECURITY_KEYRING_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SECURITY_MUTEX |Si verifica in caso di attesa dei mutex che controllano l'accesso all'elenco globale dei provider di crittografia EKM e l'elenco con ambito sessione di sessioni EKM.| 
|SECURITY_RULETABLE_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SEMPLAT_DSI_BUILD |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SEQUENCE_GENERATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SEQUENTIAL_GUID |Si verifica durante l'ottenimento di un nuovo GUID sequenziale.| 
|SERVER_IDLE_CHECK |Si verifica durante la sincronizzazione dello stato di inattività dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quando un monitoraggio risorse tenta di dichiarare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come inattiva o in corso di attivazione.| 
|SERVER_RECONFIGURE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SESSION_WAIT_STATS_CHILDREN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SHARED_DELTASTORE_CREATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SHUTDOWN |Si verifica quando un'istruzione di chiusura è in attesa dell'interruzione delle connessioni attive.| 
|SLEEP_BPOOL_FLUSH |Si verifica quando un checkpoint limita il rilascio di nuove operazioni di I/O per evitare il sovraccarico del sottosistema disco.| 
|SLEEP_BUFFERPOOL_HELPLW |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SLEEP_DBSTARTUP |Si verifica durante l'avvio di database, in attesa del recupero di tutti i database.| 
|SLEEP_DCOMSTARTUP |Si verifica al massimo una volta durante l'avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in attesa del completamento dell'inizializzazione di DCOM.| 
|SLEEP_MASTERDBREADY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SLEEP_MASTERMDREADY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SLEEP_MASTERUPGRADED |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SLEEP_MEMORYPOOL_ALLOCATEPAGES |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SLEEP_MSDBSTARTUP |Si verifica quando Traccia SQL è in attesa del completamento dell'avvio del database msdb.| 
|SLEEP_RETRY_VIRTUALALLOC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SLEEP_SYSTEMTASK |Si verifica durante l'avvio di un'attività in background, in attesa che venga completato l'avvio di tempdb.| 
|SLEEP_TASK |Si verifica quando un'attività viene sospesa in attesa di un evento generico.| 
|SLEEP_TEMPDBSTARTUP |Si verifica quando un'attività è in attesa del completamento dell'avvio di tempdb.| 
|SLEEP_WORKSPACE_ALLOCATEPAGE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SLO_UPDATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|SMSYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SNI_CONN_DUP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|SNI_CRITICAL_SECTION |Si verifica durante la sincronizzazione interna nei componenti di rete di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SNI_HTTP_WAITFOR_0_DISCON |Si verifica durante l'arresto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in attesa della chiusura delle connessioni HTTP in sospeso.| 
|SNI_LISTENER_ACCESS |Si verifica durante l'attesa dei nodi NUMA (Non-Uniform Memory Access) per aggiornare il cambiamento dello stato. L'accesso alla modifica dello stato è serializzato.| 
|SNI_TASK_COMPLETION |Si verifica durante l'attesa del completamento di tutte le attività durante il cambiamento di stato di un nodo NUMA.| 
|SNI_WRITE_ASYNC |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|SOAP_READ |Si verifica durante l'attesa del completamento di una lettura in rete HTTP.| 
|SOAP_WRITE |Si verifica durante l'attesa del completamento di una scrittura in rete HTTP.| 
|SOCKETDUPLICATEQUEUE_CLEANUP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|SOS_CALLBACK_REMOVAL |Si verifica durante l'esecuzione della sincronizzazione in un elenco di callback allo scopo di rimuovere un callback. Dopo il completamento dell'inizializzazione del server non è prevista alcuna modifica di questo contatore.| 
|SOS_DISPATCHER_MUTEX |Si verifica durante la sincronizzazione interna del pool di dispatcher e quando il pool viene modificato.| 
|SOS_LOCALALLOCATORLIST |Si verifica durante la sincronizzazione interna nel gestore della memoria di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|SOS_MEMORY_TOPLEVELBLOCKALLOCATOR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SOS_MEMORY_USAGE_ADJUSTMENT |Si verifica quando l'utilizzo della memoria è regolato tra i pool.| 
|SOS_OBJECT_STORE_DESTROY_MUTEX |Si verifica durante la sincronizzazione interna in pool di memoria, in caso di eliminazione di oggetti dal pool.| 
|SOS_PHYS_PAGE_CACHE |Considera il tempo che un thread attende per acquisire il mutex necessario prima che siano allocate le pagine fisiche o prima che tali pagine siano restituite al sistema operativo. L'attesa di questo tipo viene visualizzata solo se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza memoria AWE. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SOS_PROCESS_AFFINITY_MUTEX |Si verifica durante la sincronizzazione dell'accesso a impostazioni relative all'affinità di processo.| 
|SOS_RESERVEDMEMBLOCKLIST |Si verifica durante la sincronizzazione interna nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestore della memoria. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|SOS_SCHEDULER_YIELD |Si verifica quando un'attività cede il controllo dell'utilità di pianificazione per consentire l'esecuzione di altre attività. Durante questa attesa, l'attività attende il rinnovo del quantum.| 
|SOS_SMALL_PAGE_ALLOC |Si verifica durante l'allocazione e la liberazione della memoria gestita da alcuni oggetti della memoria.| 
|SOS_STACKSTORE_INIT_MUTEX |Si verifica durante la sincronizzazione dell'inizializzazione dell'archivio interno.| 
|SOS_SYNC_TASK_ENQUEUE_EVENT |Si verifica quando un'attività viene avviata in modo sincrono. La maggior parte delle attività in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviata in modo asincrono, in base a cui il controllo viene restituito al processo che ha richiesto l'avvio delle attività immediatamente dopo l'inserimento della richiesta dell'attività nella coda di elaborazione.| 
|SOS_VIRTUALMEMORY_LOW |Si verifica quando un'allocazione di memoria è in attesa di un Gestione risorse liberare memoria virtuale.| 
|SOSHOST_EVENT |Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione eventi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_INTERNAL |Si verifica durante la sincronizzazione dei callback del gestore della memoria usate da componenti hosted, ad esempio CLR.| 
|SOSHOST_MUTEX |Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione mutex di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_RWLOCK |Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione di tipo lettura/scrittura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_SEMAPHORE |Si verifica quando un componente hosted, ad esempio CLR, è in attesa in un oggetto di sincronizzazione semafori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_SLEEP |Si verifica quando un'attività hosted viene sospesa in attesa di un evento generico. Le attività hosted vengono usate da componenti hosted come CLR.| 
|SOSHOST_TRACELOCK |Si verifica durante la sincronizzazione dell'accesso a flussi di traccia.| 
|SOSHOST_WAITFORDONE |Si verifica quando un componente hosted, ad esempio CLR, è in attesa del completamento di un'attività.| 
|SP_PREEMPTIVE_SERVER_DIAGNOSTICS_SLEEP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SP_SERVER_DIAGNOSTICS_BUFFER_ACCESS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SP_SERVER_DIAGNOSTICS_INIT_MUTEX |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SP_SERVER_DIAGNOSTICS_SLEEP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SQLCLR_APPDOMAIN |Si verifica quando CLR è in attesa che venga completato l'avvio di un dominio applicazione.| 
|SQLCLR_ASSEMBLY |Si verifica durante l'attesa dell'accesso all'elenco degli assembly caricati nel dominio applicazione.| 
|SQLCLR_DEADLOCK_DETECTION |Si verifica quando CLR è in attesa che venga completato il rilevamento dei deadlock.| 
|SQLCLR_QUANTUM_PUNISHMENT |Si verifica quando viene applicata una limitazione a un'attività CLR poiché ha superato il quantum di esecuzione. Questa limitazione ha lo scopo di ridurre l'effetto di questa attività a elevato utilizzo di risorse sulle altre attività.| 
|SQLSORT_NORMMUTEX |Si verifica durante la sincronizzazione interna, nel corso dell'inizializzazione delle strutture di ordinamento interne.| 
|SQLSORT_SORTMUTEX |Si verifica durante la sincronizzazione interna, nel corso dell'inizializzazione delle strutture di ordinamento interne.| 
|SQLTRACE_BUFFER_FLUSH |Si verifica quando un'attività è in attesa di un'attività in background per scaricare i buffer di traccia su disco ogni 4 secondi. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|SQLTRACE_FILE_BUFFER |Si verifica durante la sincronizzazione in buffer di traccia nel corso di una traccia di file. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SQLTRACE_FILE_READ_IO_COMPLETION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SQLTRACE_FILE_WRITE_IO_COMPLETION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SQLTRACE_INCREMENTAL_FLUSH_SLEEP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SQLTRACE_LOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|SQLTRACE_PENDING_BUFFER_WRITERS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|SQLTRACE_SHUTDOWN |Si verifica quando la chiusura della traccia è in attesa del completamento degli eventi di traccia in sospeso.| 
|SQLTRACE_WAIT_ENTRIES |Si verifica quando una coda degli eventi di Traccia SQL è in attesa di pacchetti in arrivo nella coda.| 
|SRVPROC_SHUTDOWN |Si verifica quando il processo di chiusura è in attesa del rilascio di risorse interne per essere completato correttamente.| 
|STARTUP_DEPENDENCY_MANAGER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|TDS_BANDWIDTH_STATE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|TDS_INIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|TDS_PROXY_CONTAINER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|TEMPOBJ |Si verifica quando vengono sincronizzate le eliminazioni di oggetti temporanei. Questo tipo di attesa è raro e si verifica solo se un'attività ha richiesto l'accesso esclusivo per eliminazioni di tabelle temp.| 
|TEMPORAL_BACKGROUND_PROCEED_CLEANUP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|TERMINATE_LISTENER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|THREADPOOL |Si verifica quando un'attività è in attesa di un thread di lavoro in cui essere eseguita. Può indicare che il numero massimo di thread di lavoro impostato è troppo basso oppure che le esecuzioni dei batch richiedono una quantità di tempo insolitamente elevata, riducendo così il numero dei thread di lavoro disponibili per soddisfare altri batch.| 
|TIMEPRIV_TIMEPERIOD |Si verifica durante la sincronizzazione interna del timer degli eventi estesi.| 
|TRACE_EVTNOTIF |Solo per uso interno.| 
|TRACEWRITE |Si verifica quando il provider di traccia del set di righe di Traccia SQL è in attesa di un buffer libero o un buffer con eventi da elaborare.| 
|TRAN_MARKLATCH_DT |Si verifica durante l'attesa di un latch in modalità di eliminazione in un latch di contrassegno di transazione. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.| 
|TRAN_MARKLATCH_EX |Si verifica durante l'attesa di un latch in modalità esclusiva in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.| 
|TRAN_MARKLATCH_KP |Si verifica durante l'attesa di un latch in modalità conservativa in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.| 
|TRAN_MARKLATCH_NL |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|TRAN_MARKLATCH_SH |Si verifica durante l'attesa di un latch in modalità condivisa in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.| 
|TRAN_MARKLATCH_UP |Si verifica durante l'attesa di un latch in modalità di aggiornamento in una transazione contrassegnata. I latch di contrassegno di transazione vengono usati per la sincronizzazione di commit con transazioni contrassegnate.| 
|TRANSACTION_MUTEX |Si verifica durante la sincronizzazione dell'accesso a una transazione da parte di più batch.| 
|UCS_ENDPOINT_CHANGE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|UCS_MANAGER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|UCS_MEMORY_NOTIFICATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|UCS_SESSION_REGISTRATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|UCS_TRANSPORT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|UCS_TRANSPORT_STREAM_CHANGE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|UTIL_PAGE_ALLOC |Si verifica quando le analisi di log delle transazioni sono in attesa di memoria disponibile in caso di numero eccessivo di richieste di memoria.| 
|VDI_CLIENT_COMPLETECOMMAND |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|VDI_CLIENT_GETCOMMAND |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|VDI_CLIENT_OPERATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|VDI_CLIENT_OTHER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|VERSIONING_COMMITTING |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|VIA_ACCEPT |Si verifica quando una connessione del provider VIA (Virtual Interface Adapter) viene completata durante l'avvio.| 
|VIEW_DEFINITION_MUTEX |Si verifica durante la sincronizzazione dell'accesso a definizioni delle viste memorizzate nella cache.| 
|WAIT_FOR_RESULTS |Si verifica durante l'attesa dell'attivazione di una notifica di query.| 
|WAIT_ON_SYNC_STATISTICS_REFRESH |Si verifica durante l'attesa del completamento dell'aggiornamento delle statistiche sincrone prima che sia possibile riprendere la compilazione e l'esecuzione delle query.<br /><br /> **Si applica a**: A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|
|WAIT_SCRIPTDEPLOYMENT_REQUEST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_SCRIPTDEPLOYMENT_WORKER |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XLOGREAD_SIGNAL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|WAIT_XTP_ASYNC_TX_COMPLETION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_CKPT_AGENT_WAKEUP |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_CKPT_CLOSE |Si verifica durante l'attesa del completamento di un checkpoint. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_CKPT_ENABLED |Si verifica quando il checkpoint è disabilitato e si è in attesa che venga abilitato. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_CKPT_STATE_LOCK |Si verifica durante la sincronizzazione del controllo dello stato del checkpoint. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_COMPILE_WAIT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|WAIT_XTP_GUEST |Si verifica quando l'allocatore di memoria del database non riceve più notifiche di memoria insufficiente. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|WAIT_XTP_HOST_WAIT |Si verifica quando le attese vengono attivate dal motore di database e implementate dall'host. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_OFFLINE_CKPT_BEFORE_REDO |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_OFFLINE_CKPT_LOG_IO |Si verifica quando il checkpoint offline è in attesa del completamento dell'attività di I/O della lettura del log. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_OFFLINE_CKPT_NEW_LOG |Si verifica quando il checkpoint offline è in attesa dell'analisi di nuovi record di log. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_PROCEDURE_ENTRY |Si verifica quando un comando drop procedure è in attesa del completamento di tutte le esecuzioni correnti della procedura stessa. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_RECOVERY |Si verifica quando il recupero del database è in attesa del completamento del ripristino degli oggetti con ottimizzazione per la memoria. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAIT_XTP_SERIAL_RECOVERY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|WAIT_XTP_SWITCH_TO_INACTIVE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|WAIT_XTP_TASK_SHUTDOWN |Si verifica durante l'attesa del completamento di un thread OLTP in memoria. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|WAIT_XTP_TRAN_DEPENDENCY |Si verifica durante l'attesa delle dipendenze della transazione. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WAITFOR |Si verifica come risultato di un'istruzione Transact-SQL ASPETTER. La durata dell'attesa è determinata dai parametri per l'istruzione. Si tratta di un'attesa avviata dall'utente.| 
|WAITFOR_PER_QUEUE |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|WAITFOR_TASKSHUTDOWN |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|WAITSTAT_MUTEX |Si verifica durante la sincronizzazione dell'accesso alla raccolta di statistiche utilizzata per popolare sys.dm_os_wait_stats.| 
|WCC |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|WINDOW_AGGREGATES_MULTIPASS |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|WINFAB_API_CALL |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WINFAB_REPLICA_BUILD_OPERATION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|WINFAB_REPORT_FAULT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|WORKTBL_DROP |Si verifica durante la sospensione che precede un nuovo tentativo dopo un'eliminazione di tabella di lavoro non riuscita.| 
|WRITE_COMPLETION |Si verifica quando è in esecuzione un'operazione di scrittura.| 
|WRITELOG |Si verifica durante l'attesa del completamento di uno scaricamento del log. Checkpoint e commit delle transazioni costituiscono operazioni comuni che causano scaricamenti del log.| 
|XACT_OWN_TRANSACTION |Si verifica durante l'attesa dell'acquisizione della proprietà di una transazione.| 
|XACT_RECLAIM_SESSION |Si verifica durante l'attesa del rilascio della proprietà della sessione da parte del proprietario corrente.| 
|XACTLOCKINFO |Si verifica durante la sincronizzazione dell'accesso all'elenco dei blocchi per una transazione. In aggiunta alla transazione stessa, all'elenco dei blocchi accedono operazioni come il rilevamento dei deadlock e la migrazione dei blocchi durante le suddivisioni delle pagine.| 
|XACTWORKSPACE_MUTEX |Si verifica durante la sincronizzazione delle esclusioni da una transazione, nonché del numero di blocchi di database tra i membri integrati di una transazione.| 
|XDB_CONN_DUP_HASH |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XDES_HISTORY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|XDES_OUT_OF_ORDER_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|XDES_SNAPSHOT |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|XDESTSVERMGR |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|XE_BUFFERMGR_ALLPROCESSED_EVENT |Si verifica quando i buffer della sessione degli eventi estesi vengono scaricati nelle destinazioni. Questa attesa si verifica in un thread in background.| 
|XE_BUFFERMGR_FREEBUF_EVENT |Si verifica quando viene soddisfatta una delle condizioni seguenti:| 
|XE_CALLBACK_LIST |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|XE_CX_FILE_READ |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|XE_DISPATCHER_CONFIG_SESSION_LIST |Si verifica quando una sessione degli eventi estesi che utilizza destinazioni asincrone viene avviata o arrestata. Questa attesa indica una delle due situazioni seguenti:| 
|XE_DISPATCHER_JOIN |Si verifica quando un thread in background usato per le sessioni degli eventi estesi verrà terminato.| 
|XE_DISPATCHER_WAIT |Si verifica quando un thread in background usato per le sessioni degli eventi estesi è in attesa dell'elaborazione dei buffer degli eventi.| 
|XE_FILE_TARGET_TVF |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XE_LIVE_TARGET_TVF |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.| 
|XE_MODULEMGR_SYNC |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|XE_OLS_LOCK |Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.| 
|XE_PACKAGE_LOCK_BACKOFF |Identificato solo a scopo informativo. Non supportata. <br /><br /> **Si applica a**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] solo. |  
|XE_SERVICES_EVENTMANUAL |Solo per uso interno.| 
|XE_SERVICES_MUTEX |Solo per uso interno.| 
|XE_SERVICES_RWLOCK |Solo per uso interno.| 
|XE_SESSION_CREATE_SYNC |Solo per uso interno.| 
|XE_SESSION_FLUSH |Solo per uso interno.| 
|XE_SESSION_SYNC |Solo per uso interno.| 
|XE_STM_CREATE |Solo per uso interno.| 
|XE_TIMER_EVENT |Solo per uso interno.| 
|XE_TIMER_MUTEX |Solo per uso interno.| 
|XE_TIMER_TASK_DONE |Solo per uso interno.| 
|XIO_CREDENTIAL_MGR_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XIO_CREDENTIAL_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XIO_EDS_MGR_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|XIO_EDS_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|XIO_IOSTATS_BLOBLIST_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|XIO_IOSTATS_FCBLIST_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e versioni successive.| 
|XIO_LEASE_RENEW_MGR_RWLOCK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XTP_HOST_DB_COLLECTION |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|XTP_HOST_LOG_ACTIVITY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|XTP_HOST_PARALLEL_RECOVERY |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XTP_PREEMPTIVE_TASK |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XTP_TRUNCATION_LSN |Solo per uso interno. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive.| 
|XTPPROC_CACHE_ACCESS |Si verifica all'accesso a tutti gli oggetti cache della stored procedure compilata in modo nativo. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.| 
|XTPPROC_PARTITIONED_STACK_CREATE |Si verifica quando si allocano le strutture cache delle stored procedure compilate in modo nativo per nodo NUMA (thread singolo) per una stored procedure specificata. <br /><br /> **Si applica a**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] e versioni successive.|

  
 I seguenti XEvent sono correlati al **cambio** di partizione e alla ricompilazione dell'indice online. Per informazioni sulla sintassi, vedere [ALTER TABLE &#40;Transact-sql&#41;](../../t-sql/statements/alter-table-transact-sql.md) e [alter index &#40;transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 Per una matrice di compatibilità dei blocchi, vedere [sys.dm_tran_locks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
## <a name="see-also"></a>Vedi anche  
    
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_session_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md)   
 [sys.dm_db_wait_stats &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md)  
  
