---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL)
ms.custom: ''
ms.date: 02/18/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9d02513db2bedda6e0ce6291bb0e0c3c308f6540
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750981"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il set di tutti i clerk di memoria attivi nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_clerks**. 
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary (8)**|Specifica l'indirizzo di memoria univoco del clerk di memoria. Si tratta della colonna chiave primaria. Non ammette i valori Null.|  
|**type**|**nvarchar(60)**|Specifica il tipo di clerk di memoria. Ogni clerk è associato a un tipo specifico, ad esempio i clerk CLR MEMORYCLERK_SQLCLR. Non ammette i valori Null.|  
|**nome**|**nvarchar(256)**|Specifica il nome assegnato internamente del clerk di memoria. Un componente può disporre di più clerk di memoria di un tipo specifico. Un componente può scegliere di utilizzare nomi specifici per identificare i clerk di memoria dello stesso tipo. Non ammette i valori Null.|  
|**memory_node_id**|**smallint**|Specifica l'ID del nodo di memoria. Non ammette i valori NULL.|  
|**single_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Per ulteriori informazioni, vedere [modifiche alla gestione della memoria a partire da SQL Server 2012 (11. x)](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).|  
|**pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità di memoria in kilobyte (KB) allocata in pagine per questo clerk di memoria. Non ammette i valori Null.|  
|**multi_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Per ulteriori informazioni, vedere [modifiche alla gestione della memoria a partire da SQL Server 2012 (11. x)](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).<br /><br /> Quantità di memoria di più pagine allocata, espressa in KB. Si tratta della quantità di memoria allocata tramite l'utilizzo dell'allocatore di più pagine dei nodi di memoria. Questa memoria viene allocata all'esterno del pool di buffer e utilizza l'allocatore virtuale dei nodi di memoria. Non ammette i valori Null.|  
|**virtual_memory_reserved_kb**|**bigint**|Specifica la quantità di memoria virtuale riservata da un clerk di memoria. Non ammette i valori Null.|  
|**virtual_memory_committed_kb**|**bigint**|Specifica la quantità di memoria virtuale di cui è stato eseguito il commit da un clerk di memoria. La quantità di memoria di cui è stato eseguito il commit deve sempre essere minore della quantità di memoria riservata. Non ammette i valori Null.|  
|**awe_allocated_kb**|**bigint**|Specifica la quantità di memoria in kilobyte (KB) bloccata nella memoria fisica di cui non è stato eseguito il page out dal sistema operativo. Non ammette i valori Null.|  
|**shared_memory_reserved_kb**|**bigint**|Specifica la quantità di memoria condivisa riservata da un clerk di memoria, ovvero la quantità di memoria riservata per l'utilizzo da parte della memoria condivisa e del mapping di file. Non ammette i valori Null.|  
|**shared_memory_committed_kb**|**bigint**|Specifica la quantità di memoria condivisa di cui il clerk di memoria ha eseguito il commit. Non ammette i valori Null.|  
|**page_size_in_bytes**|**bigint**|Specifica la granularità dell'allocazione di pagina per questo clerk di memoria. Non ammette i valori Null.|  
|**page_allocator_address**|**varbinary (8)**|Specifica l'indirizzo dell'allocatore di pagine. Questo indirizzo è univoco per un clerk di memoria e può essere usato in **sys.dm_os_memory_objects** per individuare gli oggetti di memoria associati a questo Clerk. Non ammette i valori Null.|  
|**host_address**|**varbinary (8)**|Specifica l'indirizzo di memoria dell'host per il clerk di memoria. Per ulteriori informazioni, vedere [sys.dm_os_hosts &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). I componenti di, ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] native client, accedono alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risorse di memoria tramite l'interfaccia host.<br /><br /> 0x00000000 = Il clerk di memoria appartiene a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Non ammette i valori Null.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  

## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]Per gli obiettivi di servizio Basic, S0 e S1 e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] obiettivi del servizio, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="remarks"></a>Commenti

 Il gestore della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è strutturato in una gerarchia a tre livelli. I nodi di memoria occupano la parte inferiore della gerarchia. Il livello intermedio è occupato da clerk di memoria, cache in memoria e pool di memoria. Il primo livello è costituito dagli oggetti memoria. Questi oggetti vengono utilizzati per allocare memoria in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 I nodi di memoria rendono disponibili l'interfaccia e l'implementazione per gli allocatori di livello inferiore. All'interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], solo i clerk di memoria hanno accesso ai nodi di memoria. I clerk di memoria accedono alle interfacce dei nodi di memoria per allocare memoria. I nodi di memoria tengono inoltre traccia della memoria allocata tramite l'utilizzo del clerk per la diagnostica. Ogni componente che alloca una quantità significativa di memoria deve creare un proprio clerk di memoria e allocare tutta la relativa memoria tramite l'utilizzo delle interfacce del clerk. I componenti creano spesso i clerk corrispondenti all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

### <a name="cachestore-and-userstore"></a>/E e userStore ambito

/E e userStore ambito sono clerk di memoria, ma funzionano come cache effettive. In genere, le cache mantengono le allocazioni fino a quando un criterio di rimozione della cache non rilascia tali allocazioni. Per evitare di ricrearlo, un'allocazione memorizzata nella cache viene mantenuta nella cache il più a lungo possibile e viene normalmente rimossa dalla cache quando è troppo vecchia per essere utile o quando lo spazio di memoria è necessario per le nuove informazioni (per altre informazioni, vedere [sweep della mano di clock](sys-dm-os-memory-cache-clock-hands-transact-sql.md#remarks)). Questo è uno dei due controlli principali per le cache, ovvero controllo della durata e controllo di visibilità.

L'archivio cache e l'archivio utenti variano in modo da controllare la durata delle allocazioni. Nel caso di un archivio cache, la durata delle voci è completamente controllata dal framework di memorizzazione nella cache di SQLOS. Con l'archivio utenti, la durata delle voci è controllata solo parzialmente da un archivio. L'implementazione di ogni archivio utente può essere specifica della natura delle allocazioni di memoria e quindi gli archivi utente partecipano al controllo della durata delle proprie voci. 

Il controllo di visibilità gestisce la visibilità di una voce. Una voce in una cache può esistere, ma potrebbe non essere visibile. Se, ad esempio, una voce della cache è contrassegnata solo per uso singolo, la voce non sarà visibile dopo l'utilizzo. Inoltre, la voce della cache potrebbe essere contrassegnata come Dirty; continuerà a essere disponibile nella cache, ma non sarà visibile a tutte le ricerche. Per entrambi i punti vendita, la visibilità delle voci è controllata dal framework di Caching.

Per ulteriori informazioni, vedere [SQLOS Caching](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching).

### <a name="objectstore"></a>OBJECTSTORE

Archivio oggetti è un semplice pool. Viene usato per memorizzare nella cache dati omogenei. Tutte le voci nei pool sono considerate uguali.  Gli archivi oggetti implementano un limite massimo per controllare le dimensioni rispetto ad altre cache.

Per ulteriori informazioni, vedere [SQLOS Caching](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching).

## <a name="types"></a>Tipi

Nella tabella seguente sono elencati i tipi di clerk di memoria:

|Tipo  |Descrizione  |
|---------|---------|
|CACHESTORE_BROKERDSH     |     Questo archivio della cache viene usato per archiviare le allocazioni per [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) cache delle intestazioni di sicurezza del dialogo    |
|CACHESTORE_BROKERKEK     |   Questo archivio cache viene usato per archiviare le allocazioni in base [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  cache chiave di scambio delle chiavi    |
|CACHESTORE_BROKERREADONLY     |     Questo archivio cache viene usato per archiviare le allocazioni in base [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) cache di sola lettura    |
|CACHESTORE_BROKERRSB     |  Questo archivio cache viene usato per archiviare le allocazioni per [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) cache di [associazione al servizio remoto](../../t-sql/statements/create-remote-service-binding-transact-sql.md) .    |
|CACHESTORE_BROKERTBLACS     |     Questo archivio cache viene usato per archiviare le allocazioni in base [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) per le strutture di accesso di sicurezza.    |
|CACHESTORE_BROKERTO     |  Questo archivio cache viene usato per archiviare le allocazioni per [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) cache degli [oggetti di trasmissione](../performance-monitor/sql-server-broker-to-statistics-object.md)   |
|CACHESTORE_BROKERUSERCERTLOOKUP     |    Questo archivio cache viene usato per archiviare le allocazioni per [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  cache di ricerca dei certificati utente  |
|CACHESTORE_COLUMNSTOREOBJECTPOOL     |     Questo archivio della cache viene usato per le allocazioni dagli [indici columnstore](../indexes/columnstore-indexes-overview.md) per [segmenti](../system-catalog-views/sys-column-store-segments-transact-sql.md) e [dizionari](../system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   |
|CACHESTORE_CONVPRI     |  Questo archivio della cache viene usato per archiviare le allocazioni [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) per tenere traccia delle   [priorità delle conversazioni](../system-catalog-views/sys-conversation-priorities-transact-sql.md)     |
|CACHESTORE_EVENTS     |     Questo archivio cache viene usato per archiviare le allocazioni per [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) [notifiche degli eventi](../service-broker/event-notifications.md) |
|CACHESTORE_FULLTEXTSTOPLIST     |    Questo clerk di memoria viene usato per le allocazioni dal motore di Full-Text per la funzionalità di [parole non significative](../search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) .       |
|CACHESTORE_NOTIF     |    Questo archivio della cache viene usato per le allocazioni tramite la funzionalità di [notifica delle query](../../connect/ado-net/sql/query-notifications-sql-server.md)    |
|CACHESTORE_OBJCP     |    Questo archivio cache viene usato per la memorizzazione nella cache di oggetti con piani compilati (CP): stored procedure, funzioni, trigger. Per illustrare, dopo la creazione di un piano di query per un stored procedure, il piano viene archiviato nella cache.  |
|CACHESTORE_PHDR     |    Questo archivio cache viene usato per la memorizzazione nella cache temporanea della memoria durante l'analisi di visualizzazioni, vincoli e valori predefiniti normalizzatore gli alberi durante la compilazione di una query. Una volta analizzata la query, la memoria deve essere rilasciata. Alcuni esempi includono: molte istruzioni in un batch, ovvero migliaia di inserimenti o aggiornamenti in un unico batch, un batch T-SQL che contiene una query generata dinamicamente di grandi dimensioni, un numero elevato di valori in una clausola IN.      |
|CACHESTORE_QDSRUNTIMESTATS     |   Questo archivio cache viene usato per memorizzare nella cache [query Store](../performance/monitoring-performance-by-using-the-query-store.md)  statistiche di runtime |
|CACHESTORE_SEARCHPROPERTYLIST     |     Questo archivio cache viene usato per le allocazioni dal motore di Full-Text per la cache degli [elenchi di proprietà](../search/search-document-properties-with-search-property-lists.md)  |
|CACHESTORE_SEHOBTCOLUMNATTRIBUTE     |  Questo archivio cache viene utilizzato dal motore di archiviazione per la memorizzazione nella cache di strutture di metadati della colonna HoBT (heap) o albero B.      |
|CACHESTORE_SQLCP     |    Questo archivio cache viene usato per la memorizzazione nella cache di query ad hoc, istruzioni preparate e cursori sul lato server nella cache dei piani. Le query ad hoc sono comunemente istruzioni T-SQL per gli eventi di linguaggio inviate al server senza parametrizzazione esplicita. Anche le istruzioni preparate usano questo archivio cache, che vengono inviate dall'applicazione usando chiamate API come [SQLPrepare ()](../../odbc/reference/syntax/sqlprepare-function.md) /  [SQLExecute](../../odbc/reference/syntax/sqlexecute-function.md) (ODBC) o [SqlCommand. Prepare](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.prepare) / [SqlCommand.ExecuteNonQuery](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.executenonquery) (ADO.NET) e verranno visualizzate nel server come [sp_prepare](../system-stored-procedures/sp-prepare-transact-sql.md) / [sp_execute](../system-stored-procedures/sp-execute-transact-sql.md) o [sp_prepexec](../system-stored-procedures/sp-prepexec-transact-sql.md) le esecuzioni delle procedure di sistema. Inoltre, i cursori sul lato server utilizzeranno da questo archivio cache ([sp_cursoropen](../system-stored-procedures/sp-cursoropen-transact-sql.md), [sp_cursorfetch](../system-stored-procedures/sp-cursorfetch-transact-sql.md) [sp_cursorclose](../system-stored-procedures/sp-cursorclose-transact-sql.md)).    |
|CACHESTORE_STACKFRAMES     |    Questo archivio cache viene usato per le allocazioni di strutture del sistema operativo SQL interne correlate agli stack frame.     |
|CACHESTORE_SYSTEMROWSET     |   Questo archivio cache viene utilizzato per le allocazioni di strutture interne correlate alla registrazione e al ripristino delle transazioni.      |
|CACHESTORE_TEMPTABLES     |     Questo archivio della cache viene usato per le allocazioni correlate alle [tabelle temporanee e alla memorizzazione nella](../databases/tempdb-database.md#performance-improvements-in-tempdb-for-sql-server) cache delle variabili di tabella, parte della cache dei piani.    |
|CACHESTORE_VIEWDEFINITIONS     |     Questo archivio cache viene usato per memorizzare nella cache le definizioni delle visualizzazioni come parte dell'ottimizzazione delle query.    |
|CACHESTORE_XML_SELECTIVE_DG     |     Questo archivio cache viene usato per memorizzare nella cache le strutture XML per l'elaborazione XML.    |
|CACHESTORE_XMLDBATTRIBUTE     |     Questo archivio cache viene usato per memorizzare nella cache le strutture degli attributi XML per attività XML come [XQuery](../../xquery/xquery-language-reference-sql-server.md).    |
|CACHESTORE_XMLDBELEMENT     |   Questo archivio cache viene usato per memorizzare nella cache le strutture degli elementi XML per attività XML come [XQuery](../../xquery/xquery-language-reference-sql-server.md).      |
|CACHESTORE_XMLDBTYPE     |    Questo archivio cache viene usato per memorizzare nella cache strutture XML per attività XML come XQuery.     |
|CACHESTORE_XPROC     |   Questo archivio cache viene usato per la memorizzazione nella cache di strutture per [le stored procedure estese (XPROCS)](../extended-stored-procedures-programming/database-engine-extended-stored-procedures-programming.md) nella cache dei piani.     |
|MEMORYCLERK_BACKUP     |    Questo clerk di memoria viene usato per varie allocazioni in base alla funzionalità di [backup](../../t-sql/statements/backup-transact-sql.md)    |
|MEMORYCLERK_BHF     |      Questo clerk di memoria viene usato per le allocazioni per la gestione di oggetti binari di grandi dimensioni (BLOB) durante l'esecuzione di query (supporto dell'handle di BLOB)  |
|MEMORYCLERK_BITMAP     |     Questo clerk di memoria viene usato per le allocazioni in base alle funzionalità del sistema operativo SQL per il filtro bitmap    |
|MEMORYCLERK_CSILOBCOMPRESSION     |  Questo clerk di memoria viene usato per le allocazioni in base alla [compressione BLOB (Binary Large Object) degli indici columnstore](../data-compression/data-compression.md#columnstore-and-columnstore-archive-compression)    |
|MEMORYCLERK_DRTLHEAP     |    Questo clerk di memoria viene usato per le allocazioni in base alle funzionalità del sistema operativo SQL   <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_EXPOOL     |    Questo clerk di memoria viene usato per le allocazioni in base alle funzionalità del sistema operativo SQL   <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_EXTERNAL_EXTRACTORS     |   Questo clerk di memoria viene usato per le allocazioni dal motore di esecuzione delle query per le operazioni in [modalità batch](../query-processing-architecture-guide.md#batch-mode-execution)   <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_FILETABLE     |      Questa memoria Clerk viene utilizzata per varie allocazioni in base alla funzionalità [FileTable](../blob/filetables-sql-server.md) .   |
|MEMORYCLERK_FSAGENT     |      Questo clerk di memoria viene usato per varie allocazioni in base alla funzionalità [FileStream](../blob/filestream-sql-server.md) .    |
|MEMORYCLERK_FSCHUNKER     |      Questa memoria Clerk viene utilizzata per varie allocazioni in base alla funzionalità [FileStream](../blob/filestream-sql-server.md) per la creazione di blocchi FILESTREAM.   |
|MEMORYCLERK_FULLTEXT     |     Questo clerk di memoria viene usato per le allocazioni da Full-Text strutture del motore.   |
|MEMORYCLERK_FULLTEXT_SHMEM     |   Questo clerk di memoria viene usato per le allocazioni dalle strutture del motore Full-Text relative alla connettività di memoria condivisa con il processo del daemon full-text.      |
|MEMORYCLERK_HADR     |     Questo clerk di memoria viene usato per le allocazioni di memoria per funzionalità AlwaysOn     |
|MEMORYCLERK_HOST     |    Questo clerk di memoria viene usato per le allocazioni in base alle funzionalità del sistema operativo SQL.     |
|MEMORYCLERK_LANGSVC     |     Questo clerk di memoria viene usato per le allocazioni da istruzioni e comandi SQL T-SQL (parser, normalizzatore e così via)    |
|MEMORYCLERK_LWC     |   Questo clerk di memoria viene usato per le allocazioni per Full-Text motore di [ricerca semantica](../search/semantic-search-sql-server.md)       |
|MEMORYCLERK_POLYBASE     |    Questo clerk di memoria tiene traccia delle allocazioni di memoria per la funzionalità di [polibase](../polybase/polybase-guide.md) all'interno SQL Server.     |
|MEMORYCLERK_QSRANGEPREFETCH     |  Questo clerk di memoria viene usato per le allocazioni durante l'esecuzione di query per la prelettura degli intervalli di analisi delle query.      |
|MEMORYCLERK_QUERYDISKSTORE     |     Questo clerk di memoria viene usato dalle allocazioni di memoria [query Store](../performance/monitoring-performance-by-using-the-query-store.md) all'interno SQL Server.    |
|MEMORYCLERK_QUERYDISKSTORE_HASHMAP     |   Questo clerk di memoria viene usato dalle allocazioni di memoria [query Store](../performance/monitoring-performance-by-using-the-query-store.md) all'interno SQL Server.      |
|MEMORYCLERK_QUERYDISKSTORE_STATS     |  Questo clerk di memoria viene usato dalle allocazioni di memoria [query Store](../performance/monitoring-performance-by-using-the-query-store.md) all'interno SQL Server.      |
|MEMORYCLERK_QUERYPROFILE     |  Questo clerk di memoria viene usato durante l'avvio del server per abilitare la profilatura delle query    <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_RTLHEAP     |    Questo clerk di memoria viene usato per le allocazioni in base alle funzionalità del sistema operativo SQL.  <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_SECURITYAPI     |    Questo clerk di memoria viene usato per le allocazioni in base alle funzionalità del sistema operativo SQL.  <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_SERIALIZATION     |     Solo per uso interno.    |
|MEMORYCLERK_SLOG     |     Questo clerk di memoria viene usato per le allocazioni da sgobbare (flusso del log in memoria secondario) nel [recupero accelerato del database](../accelerated-database-recovery-concepts.md) <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_SNI     |     Questo clerk di memoria alloca memoria per i componenti SNI (Server Network Interface). SNI gestisce la connettività e i pacchetti [TDS](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/893fcc7e-8a39-4b3c-815a-773b7b982c50) per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  |
|MEMORYCLERK_SOSMEMMANAGER     |   Questo clerk di memoria alloca strutture per la pianificazione dei thread SQLOS (SOS) e la memoria e la gestione di I/O.     |
|MEMORYCLERK_SOSNODE     |     Questo clerk di memoria alloca strutture per la pianificazione dei thread SQLOS (SOS) e la memoria e la gestione di I/O.    |
|MEMORYCLERK_SOSOS     |     Questo clerk di memoria alloca strutture per la pianificazione dei thread SQLOS (SOS) e la memoria e la gestione di I/O.    |
|MEMORYCLERK_SPATIAL     |    Questo clerk di memoria viene usato dai componenti [dei dati spaziali](../spatial/spatial-data-sql-server.md) per le allocazioni di memoria.     |
|MEMORYCLERK_SQLBUFFERPOOL     |    Questo clerk di memoria tiene traccia del consumer di memoria più grande nei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dati e nelle pagine di indice. Il pool di buffer o la cache dei dati mantiene le pagine di dati e di indice caricate in memoria per fornire accesso rapido ai dati. Per ulteriori informazioni, vedere [Gestione buffer](../memory-management-architecture-guide.md#buffer-management).     |
|MEMORYCLERK_SQLCLR     |     Questo clerk di memoria viene usato per le allocazioni di [SQLCLR ](../clr-integration/clr-integration-overview.md).     |
|MEMORYCLERK_SQLCLRASSEMBLY     |     Questo clerk di memoria viene usato per le allocazioni per gli assembly [SQLCLR ](../clr-integration/clr-integration-overview.md) .     |
|MEMORYCLERK_SQLCONNECTIONPOOL     |     Questo clerk di memoria memorizza nella cache le informazioni sul server per cui l'applicazione client potrebbe richiedere il server di cui tenere traccia. Un esempio è un'applicazione che crea handle di preparazione tramite  [sp_prepexecrpc](../system-stored-procedures/sp-prepexecrpc-transact-sql.md). L'applicazione deve preparare correttamente (chiudere) questi handle dopo l'esecuzione.  |
|MEMORYCLERK_SQLEXTENSIBILITY     |    Questo clerk di memoria viene usato per le allocazioni dal [Framework di estendibilità](../../machine-learning/concepts/extensibility-framework.md) per l'esecuzione di script di Python o R esterni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  <br /><br />**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive   |
|MEMORYCLERK_SQLGENERAL     |   Questo clerk di memoria potrebbe essere usato da più consumer all'interno del motore SQL. Gli esempi includono memoria di replica, debug interno/diagnostica, alcune [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funzionalità di avvio, alcune funzionalità del parser SQL, compilazione di indici di sistema, inizializzazione di oggetti memoria globale, creazione di una connessione OLEDB all'interno del server e query del server collegato, traccia del profiler lato server, creazione di dati Showplan, funzionalità di sicurezza, compilazione di colonne calcolate, memoria per le strutture di     |
|MEMORYCLERK_SQLHTTP     |    Deprecato     |
|MEMORYCLERK_SQLLOGPOOL     |   Questo clerk di memoria viene usato dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pool di log.  Il pool di log è una cache utilizzata per migliorare le prestazioni durante la lettura del log delle transazioni. In particolare, migliora l'utilizzo della cache del log durante le letture di più log, riduce le letture del log I/O del disco e consente la condivisione delle analisi dei log. I consumer primari del pool di log sono AlwaysOn (Change Capture e Send), gestione rollforward, recupero database-analisi/rollforward/Annulla, rollback Runtime transazione, replica/CDC, backup/ripristino.    |
|MEMORYCLERK_SQLOPTIMIZER     |     Questo clerk di memoria viene usato per le allocazioni di memoria durante le diverse fasi di compilazione di una query. Alcuni utilizzi includono l'ottimizzazione delle query, index Statistics Manager, la compilazione delle definizioni delle visualizzazioni, la generazione di istogrammi.   |
|MEMORYCLERK_SQLQERESERVATIONS     |     Questo clerk di memoria viene usato per le allocazioni di concessioni di memoria, ovvero la memoria allocata alle query per eseguire operazioni di ordinamento e hash durante l'esecuzione della query. Per altre informazioni sulle prenotazioni per l'esecuzione di query (concessioni di memoria), vedere [questo Blog](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-the-mysterious-sql-server-memory-consumer-with/ba-p/333994)     |
|MEMORYCLERK_SQLQUERYCOMPILE     |    Questo clerk di memoria viene utilizzato da Query Optimizer per l'allocazione della memoria durante la compilazione delle query.   |
|MEMORYCLERK_SQLQUERYEXEC     |    Questo clerk di memoria viene utilizzato per le allocazioni nelle aree seguenti: [elaborazione in modalità batch](../query-processing-architecture-guide.md#batch-mode-execution), esecuzione [parallela di query](../query-processing-architecture-guide.md#parallel-query-processing) , contesto di esecuzione delle query, mosaico dell' [indice spaziale](../spatial/spatial-indexes-overview.md#tessellation), operazioni di ordinamento e hash (tabelle di ordinamento, tabelle hash), alcuni DVM elaborazione, [aggiornamento statistiche](../statistics/update-statistics.md) esecuzione    |
|MEMORYCLERK_SQLQUERYPLAN     |     Questo clerk di memoria viene usato per le allocazioni in base alla gestione delle pagine [heap](../indexes/heaps-tables-without-clustered-indexes.md) , alle allocazioni [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) e alle allocazioni [sp_cursor * stored procedure](../system-stored-procedures/cursor-stored-procedures-transact-sql.md)   |
|MEMORYCLERK_SQLSERVICEBROKER     |   Questo clerk di memoria viene usato dalle allocazioni di memoria [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) .       |
|MEMORYCLERK_SQLSERVICEBROKERTRANSPORT     |     Questo clerk di memoria viene usato dalle allocazioni di memoria del trasporto [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) .    |
|MEMORYCLERK_SQLSLO_OPERATIONS     |      Questo clerk di memoria viene usato per raccogliere le statistiche sulle prestazioni <br /><br />**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   |
|MEMORYCLERK_SQLSOAP     |     Deprecato    |
|MEMORYCLERK_SQLSOAPSESSIONSTORE     |    Deprecato     |
|MEMORYCLERK_SQLSTORENG     |   Questo clerk di memoria viene usato per le allocazioni da più componenti del motore di archiviazione. Esempi di componenti includono strutture per file di database, gestione file di replica snapshot del database, monitoraggio deadlock, strutture DBTABLE, strutture di gestione log, alcune strutture di controllo delle versioni di tempdb, alcune funzionalità di avvio del server, contesto di esecuzione per thread figlio in query parallele.      |
|MEMORYCLERK_SQLTRACE     |     Questo clerk di memoria viene utilizzato per le allocazioni di memoria di [traccia SQL](../sql-trace/sql-trace.md) lato server.     |
|MEMORYCLERK_SQLUTILITIES     |    Questo clerk di memoria può essere usato da più allocatori all'interno SQL Server. Gli esempi includono backup e ripristino, log shipping, mirroring del database, comandi DBCC, codice BCP sul lato server, alcune operazioni di parallelismo delle query, buffer di analisi dei log.    |
|MEMORYCLERK_SQLXML     |     Questo clerk di memoria viene utilizzato per le allocazioni di memoria durante l'esecuzione di operazioni XML.    |
|MEMORYCLERK_SQLXP     |     Questo clerk di memoria viene utilizzato per le allocazioni di memoria durante la chiamata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [stored procedure estese](../extended-stored-procedures-reference/database-engine-extended-stored-procedures-reference.md).    |
|MEMORYCLERK_SVL     |    Questo clerk di memoria viene usato per le allocazioni di strutture del sistema operativo SQL interne |
|MEMORYCLERK_TEST     |    Solo per uso interno.   |
|MEMORYCLERK_UNITTEST     |      Solo per uso interno.  |
|MEMORYCLERK_WRITEPAGERECORDER     |    Questo clerk di memoria viene usato per le allocazioni tramite la registrazione della pagina di scrittura.   |
|MEMORYCLERK_XE     |    Questo clerk di memoria viene usato per le allocazioni di memoria [degli eventi estesi](../extended-events/extended-events.md)      |
|MEMORYCLERK_XE_BUFFER     |      Questo clerk di memoria viene usato per le allocazioni di memoria [degli eventi estesi](../extended-events/extended-events.md)   |
|MEMORYCLERK_XLOG_SERVER     |   Questo clerk di memoria viene usato per le allocazioni da xlog usato per la gestione dei file di log nel database di SQL Azure   <br /><br />**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |
|MEMORYCLERK_XTP     |    Questo clerk di memoria viene utilizzato per le allocazioni [di memoria OLTP in memoria](../in-memory-oltp/in-memory-oltp-in-memory-optimization.md) .     |
|OBJECTSTORE_LBSS     |    Questo archivio di oggetti viene utilizzato per allocare variabili LOB, parametri e risultati intermedi per le espressioni temporanei. Un esempio che usa questo archivio è il [parametro con valori di tabella](../../connect/ado-net/sql/table-valued-parameters.md) (TVP). Per ulteriori informazioni sulle correzioni in questo spazio, vedere l' [articolo della knowledge 4468102](https://support.microsoft.com/topic/kb4468102-fix-excessive-memory-usage-when-you-trace-rpc-events-that-involve-table-valued-parameters-in-sql-server-2016-and-2017-c68aa214-26f1-98de-6b4d-c7dcad82dbd4) e l'  [articolo KB 4051359](https://support.microsoft.com/topic/kb4051359-fix-sql-server-runs-out-of-memory-when-table-valued-parameters-are-captured-in-extended-events-sessions-in-sql-server-2016-even-if-collecting-statement-or-data-stream-isn-t-enabled-a3639efa-0618-82a8-f6b1-8cdcba29ce6d) .     |
|OBJECTSTORE_LOCK_MANAGER     |      Questo clerk di memoria tiene traccia delle allocazioni effettuate da [Gestione blocchi](../sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .   |
|OBJECTSTORE_SECAUDIT_EVENT_BUFFER     |   Questo archivio di oggetti viene utilizzato per le allocazioni di memoria di [controllo SQL Server](../security/auditing/sql-server-audit-database-engine.md) .        |
|OBJECTSTORE_SERVICE_BROKER     |     Questo archivio oggetti viene usato da [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)    |
|OBJECTSTORE_SNI_PACKET     |     Questo archivio oggetti viene usato dai componenti SNI (Server Network Interface) che gestiscono la connettività    |
|OBJECTSTORE_XACT_CACHE     |    Questo archivio oggetti viene usato per memorizzare nella cache le informazioni sulle transazioni     |
|USERSTORE_DBMETADATA     |      Questo archivio di oggetti viene utilizzato per le strutture di metadati     |
|USERSTORE_OBJPERM     |     Questo archivio viene usato per le strutture che tengono traccia della sicurezza dell'oggetto/autorizzazione     |
|USERSTORE_QDSSTMT     |  Questo archivio cache viene usato per memorizzare nella cache [query Store](../performance/monitoring-performance-by-using-the-query-store.md)  istruzioni       |
|USERSTORE_SCHEMAMGR     |    La cache di gestione schema archivia diversi tipi di informazioni sui metadati sugli oggetti di database in memoria, ad esempio tabelle. Un utente comune di questo archivio potrebbe essere il database tempdb con oggetti quali tabelle, procedure temporanee, variabili di tabella, parametri con valori di tabella, tabelle, file creati, archivio versioni.  |
|USERSTORE_SXC     |    Questo archivio utente viene usato per le allocazioni per archiviare tutti i parametri [RPC](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/619c43b6-9495-4a58-9e49-a4950db245b3) .     |
|USERSTORE_TOKENPERM     |    TokenAndPermUserStore è un singolo archivio utenti SOS che tiene traccia delle voci di sicurezza per il contesto di sicurezza, l'accesso, l'utente, l'autorizzazione e il controllo. Per archiviare questi oggetti vengono allocate più tabelle hash.    |

## <a name="see-also"></a>Vedere anche  

 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
