---
description: sys.dm_os_latch_stats (Transact-SQL)
title: sys.dm_os_latch_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_latch_stats_TSQL
- dm_os_latch_stats_TSQL
- dm_os_latch_stats
- sys.dm_os_latch_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_latch_stats dynamic management view
ms.assetid: 2085d9fc-828c-453e-82ec-b54ed8347ae5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6f49fad9ab19da7e6017e2d6fe5c63bebfa3a242
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342862"
---
# <a name="sysdm_os_latch_stats-transact-sql"></a>sys.dm_os_latch_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce informazioni relative a tutte le attese di latch organizzate per classe. 
  
> [!NOTE]  
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_latch_stats**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|latch_class|**nvarchar(60)**|Nome della classe di latch.|  
|waiting_requests_count|**bigint**|Numero di attese di latch nella classe specifica. Questo contatore viene incrementato all'inizio di un'attesa di latch.|  
|wait_time_ms|**bigint**|Tempo totale di attesa dei latch, espresso in millisecondi, nella classe specifica.<br /><br /> **Nota:** Questa colonna viene aggiornata ogni cinque minuti durante un'attesa di latch e al termine di un'attesa del latch.|  
|max_wait_time_ms|**bigint**|Tempo massimo che un oggetto memoria ha atteso il latch specifico. Un valore insolitamente elevato può indicare un deadlock interno.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare la vista sys.dm_os_latch_stats per identificare l'origine della contesa di latch mediante l'analisi dei numeri di attesa relativi e dei tempi di attesa per le varie classi di latch. In alcune situazioni è possibile risolvere o ridurre la contesa di latch. Si possono tuttavia presentare situazioni in cui è necessario contattare il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
È possibile ripristinare il contenuto di sys.dm_os_latch_stats utilizzando `DBCC SQLPERF` come illustrato di seguito:  
  
```sql  
DBCC SQLPERF ('sys.dm_os_latch_stats', CLEAR);  
GO  
```  
  
 Questo comando reimposta tutti i contatori su 0.  
  
> [!NOTE]  
>  Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene riavviato, le statistiche non sono persistenti. Tutti i dati sono cumulativi a partire dall'ultima reimpostazione delle statistiche oppure dall'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="latches"></a><a name="latches"></a> Fermi  
 Un latch è un oggetto di sincronizzazione Lightweight interno simile a un blocco utilizzato da vari [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] componenti. Un latch viene utilizzato principalmente per sincronizzare le pagine del database durante operazioni quali il buffer o l'accesso ai file. Ogni latch è associato a un'unica unità di allocazione. 
  
 Un'attesa di latch si verifica quando non è possibile concedere immediatamente una richiesta di latch perché il latch è mantenuto attivo da un altro thread con una modalità in conflitto. A differenza dei blocchi, i latch vengono rilasciati subito dopo l'operazione, anche nel caso di operazioni di scrittura.  
  
 I latch sono raggruppati in classi in base ai componenti e all'utilizzo. In un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere presenti zero o più latch di una classe specifica in un momento qualsiasi.  
  
> [!NOTE]  
> `sys.dm_os_latch_stats` non tiene traccia delle richieste di latch concesse immediatamente oppure che hanno avuto esito negativo senza alcuna attesa.  
  
 Nella tabella seguente è riportata una breve descrizione delle varie classi di latch.  
  
|Classe di latch|Descrizione|  
|-----------------|-----------------|  
|ALLOC_CREATE_RINGBUF|Utilizzato internamente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per inizializzare la sincronizzazione della creazione di un buffer circolare per le allocazioni.|  
|ALLOC_CREATE_FREESPACE_CACHE|Utilizzato per inizializzare la sincronizzazione delle cache dello spazio disponibile interno per gli heap.|  
|ALLOC_CACHE_MANAGER|Utilizzato per sincronizzare i test di coerenza interna.|  
|ALLOC_FREESPACE_CACHE|Utilizzato per sincronizzare l'accesso a una cache di pagine con spazio disponibile per heap e oggetti BLOB (Binary Large Object). È possibile che si verifichino contese a livello di latch appartenenti a questa classe se più connessioni cercano contemporaneamente di inserire righe in un heap o oggetto BLOB. È possibile ridurre le contese tramite il partizionamento dell'oggetto. Ogni partizione dispone del proprio latch. Il partizionamento distribuirà gli inserimenti tra più latch.|  
|ALLOC_EXTENT_CACHE|Utilizzato per sincronizzare l'accesso a una cache di extent contenente le pagine non allocate. È possibile che si verifichino contese a livello di latch appartenenti a questa classe se più connessioni cercano contemporaneamente di allocare pagine di dati nella stessa unità di allocazione. È possibile ridurre le contese tramite il partizionamento dell'oggetto a cui appartiene l'unità di allocazione specifica.|  
|ACCESS_METHODS_DATASET_PARENT|Utilizzato per sincronizzare l'acceso al set di dati figlio al set di dati padre durante le operazioni parallele.|  
|ACCESS_METHODS_HOBT_FACTORY|Utilizzato per sincronizzare l'accesso alla tabella hash interna.|  
|ACCESS_METHODS_HOBT|Utilizzato per sincronizzare l'accesso alla rappresentazione in memoria di una risorsa HOBT.|  
|ACCESS_METHODS_HOBT_COUNT|Utilizzato per sincronizzare l'accesso ai contatori di pagine e righe HOBT.|  
|ACCESS_METHODS_HOBT_VIRTUAL_ROOT|Utilizzato per sincronizzare l'accesso all'astrazione di pagina radice di un albero B interno.|  
|ACCESS_METHODS_CACHE_ONLY_HOBT_ALLOC|Utilizzato per sincronizzare l'accesso alle tabelle di lavoro.|  
|ACCESS_METHODS_BULK_ALLOC|Utilizzato per sincronizzare l'accesso all'interno degli allocatori bulk.|  
|ACCESS_METHODS_SCAN_RANGE_GENERATOR|Utilizzato per sincronizzare l'accesso a un generatore di intervalli durante le analisi parallele.|  
|ACCESS_METHODS_KEY_RANGE_GENERATOR|Utilizzato per sincronizzare l'accesso alle operazioni read-ahead durante le analisi parallele degli intervalli di chiavi.|  
|APPEND_ONLY_STORAGE_INSERT_POINT|Utilizzato per sincronizzare gli inserimenti nelle unità di archiviazione rapide che supportano solo l'accodamento di allocazioni.|  
|APPEND_ONLY_STORAGE_FIRST_ALLOC|Utilizzato per sincronizzare la prima allocazione nelle unità di archiviazione rapide che supportano solo l'accodamento di allocazioni.|  
|APPEND_ONLY_STORAGE_UNIT_MANAGER|Utilizzato per la sincronizzazione dell'accesso alle strutture interne di dati all'interno della gestione nelle unità di archiviazione rapide che supportano solo l'accodamento di allocazioni|  
|APPEND_ONLY_STORAGE_MANAGER|Utilizzato per sincronizzare le operazioni di compattazione nella gestione delle unità di archiviazione rapide che supportano solo l'accodamento di allocazioni.|  
|BACKUP_RESULT_SET|Utilizzato per sincronizzare i set di risultati del backup parallelo.|  
|BACKUP_TAPE_POOL|Utilizzato per sincronizzare i pool di nastri di backup.|  
|BACKUP_LOG_REDO|Utilizzato per sincronizzare le operazioni di rollforward del log dal backup.|  
|BACKUP_INSTANCE_ID|Utilizzato per sincronizzare la generazione degli ID di istanza per i contatori di monitoraggio delle prestazioni del backup.|  
|BACKUP_MANAGER|Utilizzato per sincronizzare la gestione dei backup interni.|  
|BACKUP_MANAGER_DIFFERENTIAL|Utilizzato per sincronizzare le operazioni di backup differenziale con DBCC.|  
|BACKUP_OPERATION|Utilizzato per la sincronizzazione delle strutture interne di dati all'interno di un'operazione di backup, ad esempio il backup di database, log o file.|  
|BACKUP_FILE_HANDLE|Utilizzato per sincronizzare le operazioni di apertura di file durante un'operazione di ripristino.|  
|BUFFER|Utilizzato per sincronizzare l'accesso a breve termine alle pagine di database. È richiesto un latch del buffer prima di leggere o modificare qualsiasi pagina di database. Una contesa di latch del buffer può indicare numerosi problemi, inclusi I/O lenti e pagine utilizzate con maggiore frequenza.<br /><br /> Questa classe di latch copre tutti i possibili utilizzi dei latch di pagina. sys.dm_os_wait_stats fa una differenza tra le attese di latch di pagina provocate da operazioni di I/O e operazioni di lettura e scrittura nella pagina.|  
|BUFFER_POOL_GROW|Utilizzato per la sincronizzazione della gestione dei buffer interni durante le operazioni di aumento delle dimensioni del pool di buffer.|  
|DATABASE_CHECKPOINT|Utilizzato per serializzare i checkpoint all'interno di un database.|  
|CLR_PROCEDURE_HASHTABLE|Solo per uso interno.|  
|CLR_UDX_STORE|Solo per uso interno.|  
|CLR_DATAT_ACCESS|Solo per uso interno.|  
|CLR_XVAR_PROXY_LIST|Solo per uso interno.|  
|DBCC_CHECK_AGGREGATE|Solo per uso interno.|  
|DBCC_CHECK_RESULTSET|Solo per uso interno.|  
|DBCC_CHECK_TABLE|Solo per uso interno.|  
|DBCC_CHECK_TABLE_INIT|Solo per uso interno.|  
|DBCC_CHECK_TRACE_LIST|Solo per uso interno.|  
|DBCC_FILE_CHECK_OBJECT|Solo per uso interno.|  
|DBCC_PERF|Utilizzato per sincronizzare i contatori di monitoraggio delle prestazioni interne.|  
|DBCC_PFS_STATUS|Solo per uso interno.|  
|DBCC_OBJECT_METADATA|Solo per uso interno.|  
|DBCC_HASH_DLL|Solo per uso interno.|  
|EVENTING_CACHE|Solo per uso interno.|  
|FCB|Utilizzato per sincronizzare l'accesso al blocco di controllo file.|  
|FCB_REPLICA|Solo per uso interno.|  
|FGCB_ALLOC|Utilizzato per sincronizzare l'accesso alle informazioni sull'allocazione round robin all'interno di un filegroup.|  
|FGCB_ADD_REMOVE|Utilizzare per sincronizzare l'accesso ai filegroup per le operazioni di aggiunta, eliminazione, espansione e compattazione dei file.|  
|FILEGROUP_MANAGER|Solo per uso interno.|  
|FILE_MANAGER|Solo per uso interno.|  
|FILESTREAM_FCB|Solo per uso interno.|  
|FILESTREAM_FILE_MANAGER|Solo per uso interno.|  
|FILESTREAM_GHOST_FILES|Solo per uso interno.|  
|FILESTREAM_DFS_ROOT|Solo per uso interno.|  
|LOG_MANAGER|Solo per uso interno.|  
|FULLTEXT_DOCUMENT_ID|Solo per uso interno.|  
|FULLTEXT_DOCUMENT_ID_TRANSACTION|Solo per uso interno.|  
|FULLTEXT_DOCUMENT_ID_NOTIFY|Solo per uso interno.|  
|FULLTEXT_LOGS|Solo per uso interno.|  
|FULLTEXT_CRAWL_LOG|Solo per uso interno.|  
|FULLTEXT_ADMIN|Solo per uso interno.|  
|FULLTEXT_AMDIN_COMMAND_CACHE|Solo per uso interno.|  
|FULLTEXT_LANGUAGE_TABLE|Solo per uso interno.|  
|FULLTEXT_CRAWL_DM_LIST|Solo per uso interno.|  
|FULLTEXT_CRAWL_CATALOG|Solo per uso interno.|  
|FULLTEXT_FILE_MANAGER|Solo per uso interno.|  
|DATABASE_MIRRORING_REDO|Solo per uso interno.|  
|DATABASE_MIRRORING_SERVER|Solo per uso interno.|  
|DATABASE_MIRRORING_CONNECTION|Solo per uso interno.|  
|DATABASE_MIRRORING_STREAM|Solo per uso interno.|  
|QUERY_OPTIMIZER_VD_MANAGER|Solo per uso interno.|  
|QUERY_OPTIMIZER_ID_MANAGER|Solo per uso interno.|  
|QUERY_OPTIMIZER_VIEW_REP|Solo per uso interno.|  
|RECOVERY_BAD_PAGE_TABLE|Solo per uso interno.|  
|RECOVERY_MANAGER|Solo per uso interno.|  
|SECURITY_OPERATION_RULE_TABLE|Solo per uso interno.|  
|SECURITY_OBJPERM_CACHE|Solo per uso interno.|  
|SECURITY_CRYPTO|Solo per uso interno.|  
|SECURITY_KEY_RING|Solo per uso interno.|  
|SECURITY_KEY_LIST|Solo per uso interno.|  
|SERVICE_BROKER_CONNECTION_RECEIVE|Solo per uso interno.|  
|SERVICE_BROKER_TRANSMISSION|Solo per uso interno.|  
|SERVICE_BROKER_TRANSMISSION_UPDATE|Solo per uso interno.|  
|SERVICE_BROKER_TRANSMISSION_STATE|Solo per uso interno.|  
|SERVICE_BROKER_TRANSMISSION_ERRORS|Solo per uso interno.|  
|SSBXmitWork|Solo per uso interno.|  
|SERVICE_BROKER_MESSAGE_TRANSMISSION|Solo per uso interno.|  
|SERVICE_BROKER_MAP_MANAGER|Solo per uso interno.|  
|SERVICE_BROKER_HOST_NAME|Solo per uso interno.|  
|SERVICE_BROKER_READ_CACHE|Solo per uso interno.|  
|SERVICE_BROKER_WAITFOR_MANAGER| Utilizzato per sincronizzare una mappa a livello di istanza delle code dei camerieri. Esiste una coda per ID database, versione database e tupla ID coda. La contesa sui latch di questa classe può verificarsi quando molte connessioni sono: in uno stato di attesa (ricezione) Wait; chiamata di ASPETTER (RECEIVE); superamento del timeout di attesa; ricezione di un messaggio; esecuzione del commit o del rollback della transazione che contiene l'oggetto ASPETTER (RECEIVE); È possibile ridurre la contesa riducendo il numero di thread in uno stato di attesa (ricezione). |  
|SERVICE_BROKER_WAITFOR_TRANSACTION_DATA|Solo per uso interno.|  
|SERVICE_BROKER_TRANSMISSION_TRANSACTION_DATA|Solo per uso interno.|  
|SERVICE_BROKER_TRANSPORT|Solo per uso interno.|  
|SERVICE_BROKER_MIRROR_ROUTE|Solo per uso interno.|  
|TRACE_ID|Solo per uso interno.|  
|TRACE_AUDIT_ID|Solo per uso interno.|  
|TRACE|Solo per uso interno.|  
|TRACE_CONTROLLER|Solo per uso interno.|  
|TRACE_EVENT_QUEUE|Solo per uso interno.|  
|TRANSACTION_DISTRIBUTED_MARK|Solo per uso interno.|  
|TRANSACTION_OUTCOME|Solo per uso interno.|  
|NESTING_TRANSACTION_READONLY|Solo per uso interno.|  
|NESTING_TRANSACTION_FULL|Solo per uso interno.|  
|MSQL_TRANSACTION_MANAGER|Solo per uso interno.|  
|DATABASE_AUTONAME_MANAGER|Solo per uso interno.|  
|UTILITY_DYNAMIC_VECTOR|Solo per uso interno.|  
|UTILITY_SPARSE_BITMAP|Solo per uso interno.|  
|UTILITY_DATABASE_DROP|Solo per uso interno.|  
|UTILITY_DYNAMIC_MANAGER_VIEW|Solo per uso interno.|  
|UTILITY_DEBUG_FILESTREAM|Solo per uso interno.|  
|UTILITY_LOCK_INFORMATION|Solo per uso interno.|  
|VERSIONING_TRANSACTION|Solo per uso interno.|  
|VERSIONING_TRANSACTION_LIST|Solo per uso interno.|  
|VERSIONING_TRANSACTION_CHAIN|Solo per uso interno.|  
|VERSIONING_STATE|Solo per uso interno.|  
|VERSIONING_STATE_CHANGE|Solo per uso interno.|  
|KTM_VIRTUAL_CLOCK|Solo per uso interno.|  
  
## <a name="see-also"></a>Vedere anche  
[DBCC SQLPERF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)       
[SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Oggetto Latch di SQL Server](../../relational-databases/performance-monitor/sql-server-latches-object.md)      
