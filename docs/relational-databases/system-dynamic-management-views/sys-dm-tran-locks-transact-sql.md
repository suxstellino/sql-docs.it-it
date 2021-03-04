---
description: sys.dm_tran_locks (Transact-SQL)
title: sys.dm_tran_locks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_tran_locks
- sys.dm_tran_locks
- sys.dm_tran_locks_TSQL
- dm_tran_locks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_locks dynamic management view
ms.assetid: f0d3b95a-8a00-471b-9da4-14cb8f5b045f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95dfd0be129d7f6116ef92fbe54d7869af45fca1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837968"
---
# <a name="sysdm_tran_locks-transact-sql"></a>sys.dm_tran_locks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni sulle risorse di Gestione blocchi attualmente attive in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Ogni riga rappresenta una richiesta attualmente attiva di un blocco concesso o in attesa di essere concesso, effettuata a Gestione blocchi.  
  
 Le colonne nel set di risultati sono divise in due gruppi principali: risorsa e richiesta. Nel gruppo relativo alle risorse viene descritta la risorsa per cui viene effettuata la richiesta, mentre nel gruppo relativo alle richieste viene descritta la richiesta di blocco.  
  
> [!NOTE]  
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_tran_locks**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**resource_type**|**nvarchar(60)**|Rappresenta il tipo di risorsa. Può essere uno dei valori seguenti: DATABASE, FILE, OBJECT, PAGE, KEY, EXTENT, RID, APPLICATION, METADATA, HOBT o ALLOCATION_UNIT.|  
|**resource_subtype**|**nvarchar(60)**|Rappresenta un sottotipo di **resource_type**. È tecnicamente possibile acquisire un blocco di un sottotipo senza mantenere un blocco senza sottotipi del tipo padre. Sottotipi diversi non sono in conflitto reciproco o con il tipo padre senza sottotipi. Non tutti i tipi di risorse hanno sottotipi.|  
|**resource_database_id**|**int**|ID del database in cui la risorsa è definita a livello di ambito. Tutte le risorse gestite da Gestione blocchi sono definite a livello di ambito dell'ID del database.|  
|**resource_description**|**nvarchar(256)**|Descrizione della risorsa contenente solo le informazioni non disponibili in altre colonne delle risorse.|  
|**resource_associated_entity_id**|**bigint**|ID dell'entità in un database a cui è associata una risorsa. Può essere un ID di oggetto, un ID di risorsa HoBT o un ID di unità di allocazione, in base al tipo di risorsa.|  
|**resource_lock_partition**|**Int**|ID della partizione di blocco per una risorsa di blocco partizionata. Il valore per le risorse di blocco non partizionate è 0.|  
|**request_mode**|**nvarchar(60)**|Modalità relativa alla richiesta. Per le richieste concesse, indica la modalità concessa. Per le richieste in attesa, indica la modalità richiesta. <br /><br /> NULL = Non è concesso l'accesso alla risorsa. Funge da segnaposto.<br /><br /> SCH-S (stabilità dello schema) = assicura che un elemento dello schema, ad esempio una tabella o un indice, non venga eliminato mentre una sessione include un blocco di stabilità dello schema sull'elemento dello schema.<br /><br /> SCH-M (modifica dello schema) = deve essere utilizzato da qualsiasi sessione che desidera modificare lo schema della risorsa specificata. Assicura che nessun'altra sessione faccia riferimento all'oggetto specificato.<br /><br /> S (condiviso) = alla sessione di conservazione viene concesso l'accesso condiviso alla risorsa.<br /><br /> U (Update) = indica un blocco di aggiornamento acquisito sulle risorse che potrebbero essere aggiornate. Viene utilizzato per evitare una forma comune di deadlock che si verifica quando in più sessioni vengono bloccate risorse che potrebbero essere aggiornate in futuro.<br /><br /> X (Exclusive) = alla sessione di conservazione viene concesso l'accesso esclusivo alla risorsa.<br /><br /> IS (preventivo condiviso) = indica l'intenzione di inserire blocchi S su una risorsa subordinata nella gerarchia dei blocchi.<br /><br /> IU (Intent Update) = indica l'intenzione di inserire i blocchi U su una risorsa subordinata nella gerarchia dei blocchi.<br /><br /> IX (preventivo esclusivo) = indica l'intenzione di posizionare i blocchi X su una risorsa subordinata nella gerarchia dei blocchi.<br /><br /> SIU (Shared Intent Update) = indica l'accesso condiviso a una risorsa con lo scopo di acquisire i blocchi di aggiornamento sulle risorse subordinate nella gerarchia dei blocchi.<br /><br /> SIX (Shared preventivo Exclusive) = indica l'accesso condiviso a una risorsa con l'intenzione di acquisire blocchi esclusivi su risorse subordinate nella gerarchia dei blocchi.<br /><br /> UIX (Update Intent Exclusive) = indica un blocco di aggiornamento in una risorsa con l'intenzione di acquisire blocchi esclusivi su risorse subordinate nella gerarchia dei blocchi.<br /><br /> BU = usato dalle operazioni bulk.<br /><br /> RangeS_S (Shared Key-Range e blocco di risorse condivise) = indica un'analisi dell'intervallo serializzabile.<br /><br /> RangeS_U (Shared Key-Range e Update Resource Lock) = indica l'analisi degli aggiornamenti serializzabile.<br /><br /> RangeI_N (Insert Key-Range and null Resource Lock) = usato per verificare gli intervalli prima di inserire una nuova chiave in un indice.<br /><br /> RangeI_S = Blocco di conversione Key-Range, creato da una sovrapposizione dei blocchi RangeI_N e S.<br /><br /> RangeI_U = Blocco di conversione Key-Range, creato da una sovrapposizione dei blocchi RangeI_N e U.<br /><br /> RangeI_X = Blocco di conversione Key-Range, creato da una sovrapposizione dei blocchi RangeI_N e X.<br /><br /> RangeX_S = Key-Range blocco di conversione, creato da una sovrapposizione di RangeI_N e RangeS_S. RangeS_S.<br /><br /> RangeX_U = Blocco di conversione Key-Range, creato da una sovrapposizione dei blocchi RangeI_N e RangeS_U.<br /><br /> RangeX_X (esclusiva Key-Range e blocco di risorsa esclusivo) = si tratta di un blocco di conversione usato per l'aggiornamento di una chiave in un intervallo.|  
|**request_type**|**nvarchar(60)**|Tipo di richiesta. Il valore è LOCK.|  
|**request_status**|**nvarchar(60)**|Stato corrente della richiesta. I valori possibili sono GRANTED, CONVERT, WAIT, LOW_PRIORITY_CONVERT, LOW_PRIORITY_WAIT o ABORT_BLOCKERS. Per ulteriori informazioni sulle attese con priorità bassa e i blocchi di interruzione, vedere la sezione *low_priority_lock_wait* di [ALTER INDEX &#40;&#41;Transact-SQL](../../t-sql/statements/alter-index-transact-sql.md).|  
|**request_reference_count**|**smallint**|Restituisce un numero approssimativo di volte che lo stesso richiedente ha richiesto la risorsa.|  
|**request_lifetime**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**request_session_id**|**int**|ID di sessione attualmente proprietario della richiesta. L'ID di sessione proprietario può cambiare per le transazioni distribuite e associate. Il valore -2 indica che la richiesta appartiene a una transazione distribuita orfana. Il valore -3 indica che la richiesta appartiene a una transazione di recupero posticipata, ad esempio una transazione per cui durante il recupero un rollback è stato posticipato poiché non poteva essere completato correttamente.|  
|**request_exec_context_id**|**int**|ID del contesto di esecuzione del processo attualmente proprietario della richiesta.|  
|**request_request_id**|**int**|ID della richiesta (ID batch) del processo attualmente proprietario della richiesta. Questo valore verrà modificato ogni volta che viene modificata la connessione MARS (Multiple Active Result Set) attiva per una transazione.|  
|**request_owner_type**|**nvarchar(60)**|Tipo di entità proprietaria della richiesta. Le richieste di Gestione blocchi possono appartenere a varie entità. I valori possibili sono:<br /><br /> TRANSACTION = La richiesta appartiene a una transazione.<br /><br /> CURSOR = La richiesta appartiene a un cursore.<br /><br /> SESSION = La richiesta appartiene a una sessione utente.<br /><br /> SHARED_TRANSACTION_WORKSPACE = La richiesta appartiene alla parte condivisa dell'area di lavoro della transazione.<br /><br /> EXCLUSIVE_TRANSACTION_WORKSPACE = la richiesta è di proprietà della parte esclusiva dell'area di lavoro della transazione.<br /><br /> NOTIFICATION_OBJECT = la richiesta è di proprietà di un componente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interno. Questo componente ha richiesto a Gestione blocchi di inviare una notifica quando un altro componente è in attesa del blocco. La caratteristica FileTable è un componente che utilizza questo valore.<br /><br /> **Nota:** Gli spazi di lavoro vengono usati internamente per mantenere attivi i blocchi per le sessioni integrate.|  
|**request_owner_id**|**bigint**|ID del proprietario specifico della richiesta.<br /><br /> Quando una transazione è il proprietario della richiesta, questo valore contiene l'ID transazione.<br /><br /> Quando una tabella FileTable è il proprietario della richiesta, i possibili valori di **request_owner_id** sono i seguenti:<br /> <ul><li>**-4** : un blocco di database è stato applicato a una tabella FileTable.<li> **-3** : un blocco di tabella è stato applicato a una tabella FileTable.<li> **Altro valore** : il valore rappresenta un handle di file. Questo valore viene inoltre visualizzato come **fcb_id** nella vista a gestione dinamica [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md).</li></ul>|  
|**request_owner_guid**|**uniqueidentifier**|GUID del proprietario specifico della richiesta. Questo valore viene utilizzato soltanto da una transazione distribuita nei casi in cui corrisponde al GUID MS DTC della transazione.|  
|**request_owner_lockspace_id**|**nvarchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Questo valore rappresenta l'ID dello spazio di blocco del richiedente. L'ID dello spazio di blocco determina se due richiedenti sono reciprocamente compatibili e possono ottenere blocchi in modalità altrimenti in conflitto.|  
|**lock_owner_address**|**varbinary (8)**|Indirizzo di memoria della struttura dei dati interna utilizzata per tener traccia della richiesta. Questa colonna può essere unita in join con la colonna **resource_address** in **sys.dm_os_waiting_tasks**.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="remarks"></a>Commenti  
 Uno stato di richiesta concessa indica che un blocco è stato concesso per una risorsa al richiedente. Una richiesta in attesa indica che la richiesta non è stata ancora concessa. La colonna **request_status** restituisce i tipi di richieste in attesa seguenti:  
  
-   Uno stato di richiesta di conversione indica che il richiedente ha già ottenuto una richiesta per la risorsa ed è attualmente in attesa di un aggiornamento alla richiesta iniziale.  
  
-   Uno stato di richiesta in attesa indica che il richiedente non possiede una richiesta concessa per la risorsa.  
  
 Poiché **sys.dm_tran_locks** viene popolata da strutture di dati di Gestione blocchi interne, il mantenimento di queste informazioni non determina un overhead aggiuntivo rispetto alla normale elaborazione. Per la materializzazione di questa vista è richiesto l'accesso alle strutture di dati interne di Gestione blocchi. Ciò può produrre effetti minimi sulla normale elaborazione nel server, che interesseranno solo le risorse con carichi elevati e non saranno altrimenti rilevati. Poiché i dati nella vista corrispondono allo stato di Gestione blocchi in tempo reale, tali dati sono soggetti a modifiche in qualsiasi momento e le righe vengono aggiunte e rimosse con l'acquisizione e il rilascio di blocchi. La vista non contiene informazioni cronologiche.  
  
 Due richieste operano sulla stessa risorsa solo se tutte le colonne del gruppo relativo alle risorse sono uguali.  
  
 È possibile controllare il blocco delle operazioni di lettura tramite gli strumenti seguenti:  
  
-   SET TRANSACTION ISOLATION LEVEL per specificare il livello di blocco per una sessione. Per altre informazioni, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
-   Hint di tabelle di blocco per specificare il livello di blocco per un riferimento individuale di una tabella in una clausola FROM. Per la sintassi e le restrizioni, vedere [hint di tabella &#40;&#41;Transact-SQL ](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Una risorsa in esecuzione in un ID di sessione può ottenere la concessione di più blocchi. Diverse entità in esecuzione in una sessione possono possedere ognuna un blocco sulla stessa risorsa. Le informazioni vengono visualizzate nelle colonne **request_owner_type** e **request_owner_id** restituite da **sys.dm_tran_locks**. Se esistono più istanze dello stesso **request_owner_type**, per distinguere ogni istanza viene utilizzata la colonna **request_owner_id**. Per le transazioni distribuite, nelle colonne **request_owner_type** e **request_owner_guid** verranno visualizzate le informazioni relative alle diverse entità.  
  
 Ad esempio, la sessione S1 possiede un blocco condiviso sulla tabella **Table1** e la transazione T1, in esecuzione nella sessione S1, possiede ugualmente un blocco condiviso sulla tabella **Table1**. In questo caso, la colonna **resource_description** restituita da **sys.dm_tran_locks** conterrà due istanze della stessa risorsa. Nella colonna **request_owner_type** un'istanza verrà visualizzata come sessione e l'altra come transazione. La colonna **resource_owner_id** conterrà inoltre valori diversi.  
  
 Più cursori in esecuzione in una sessione non sono distinguibili e vengono considerati come un'unica entità.  
  
 Le transazioni distribuite non associate a un valore di ID di sessione sono transazioni orfane a cui viene assegnato il valore di ID di sessione -2. Per altre informazioni, vedere [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md).  

## <a name="locks"></a><a name="locks"></a> Serrature
I blocchi sulle risorse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio sulle righe lette o modificate durante una transazione, impediscono che le risorse vengano utilizzate contemporaneamente da transazioni diverse. Ad esempio, se una transazione mantiene attivo un blocco esclusivo (X) su una riga all'interno di una tabella, nessun'altra transazione potrà modificare la riga fino a quando il blocco non viene rilasciato. La riduzione dei blocchi aumenta la concorrenza e, di conseguenza, potrebbe migliorare le prestazioni. 

## <a name="resource-details"></a>Informazioni dettagliate sulle risorse  
 Nella tabella seguente sono elencate le risorse rappresentate nella colonna **resource_associated_entity_id**.  
  
|Tipo di risorsa|Descrizione della risorsa|Resource_associated_entity_id|  
|-------------------|--------------------------|--------------------------------------|  
|DATABASE|Rappresenta un database.|Non applicabile|  
|FILE|Rappresenta un file di database. Può essere un file di dati o di log.|Non applicabile|  
|OBJECT|Rappresenta un oggetto di database. Può essere una tabella di dati, una vista, una stored procedure, una stored procedure estesa o qualsiasi oggetto con ID di oggetto.|ID dell'oggetto.|  
|PAGE|Rappresenta una pagina singola in un file di dati.|ID HoBT. Questo valore corrisponde a **sys.partitions.hobt_id**. L'ID di risorsa HoBT non è sempre disponibile per le risorse PAGE, poiché rappresenta informazioni aggiuntive che possono essere fornite solo da alcuni chiamanti.|  
|KEY|Rappresenta una riga in un indice.|ID HoBT. Questo valore corrisponde a **sys.partitions.hobt_id**.|  
|EXTENT|Rappresenta un extent di file di dati, ovvero un gruppo di otto pagine contigue.|Non applicabile|  
|RID|Rappresenta una riga fisica in un heap.|ID HoBT. Questo valore corrisponde a **sys.partitions.hobt_id**. L'ID di risorsa HoBT non è sempre disponibile per le risorse RID, poiché rappresenta informazioni aggiuntive che possono essere fornite solo da alcuni chiamanti.|  
|APPLICATION|Rappresenta una risorsa specificata dall'applicazione.|Non applicabile|  
|METADATI|Rappresenta informazioni sui metadati.|Non applicabile|  
|HOBT|Rappresenta un heap o un albero B. Si tratta delle strutture del percorso di accesso di base.|ID HoBT. Questo valore corrisponde a **sys.partitions.hobt_id**.|  
|ALLOCATION_UNIT|Rappresenta un set di pagine correlate, ad esempio una partizione dell'indice. Ogni unità di allocazione ricopre una singola catena della mappa di allocazione degli indici (IAM, Index Allocation Map).|ID unità di allocazione. Questo valore corrisponde a **sys.allocation_units.allocation_unit_id**.|  
  
 Nella tabella seguente sono elencati i sottotipi associati a ogni tipo di risorsa.  
  
|ResourceSubType|Sincronizzazione|  
|---------------------|------------------|  
|ALLOCATION_UNIT.BULK_OPERATION_PAGE|Pagine pre-assegnate usate per le operazioni bulk.|  
|ALLOCATION_UNIT.PAGE_COUNT|Statistiche relative al conteggio delle pagine delle unità di allocazione durante operazioni di rimozione posticipate.|  
|DATABASE.BULKOP_BACKUP_DB|Backup di database con operazioni bulk.|  
|DATABASE.BULKOP_BACKUP_LOG|Backup di log di database con operazioni bulk.|  
|DATABASE.CHANGE_TRACKING_CLEANUP|Attività di pulizia per il rilevamento delle modifiche.|  
|DATABASE.CT_DDL|Operazioni DDL per il rilevamento delle modifiche a livello di tabella e database.|  
|DATABASE.CONVERSATION_PRIORITY|Operazioni con priorità di conversazione di Service Broker come CREATE BROKER PRIORITY.|  
|DATABASE.DDL|Operazioni DDL (Data Definition Language) con operazioni relative a filegroup, come la rimozione.|  
|DATABASE.ENCRYPTION_SCAN|Sincronizzazione della crittografia TDE.|  
|DATABASE.PLANGUIDE|Sincronizzazione della guida di piano.|  
|DATABASE.RESOURCE_GOVERNOR_DDL|Istruzioni DDL per le operazioni di Resource Governor come ALTER RESOURCE POOL.|  
|DATABASE.SHRINK|Operazioni di compattazione database.|  
|DATABASE.STARTUP|Utilizzato per la sincronizzazione all'avvio del database.|  
|FILE.SHRINK|Operazioni di compattazione file.|  
|HOBT.BULK_OPERATION|Operazioni di caricamento bulk ottimizzate per gli heap con analisi simultanea nei livelli di isolamento snapshot, Read uncommitted e Read committed con controllo delle versioni delle righe.|  
|HOBT.INDEX_REORGANIZE|Operazioni di riorganizzazione di heap o indici.|  
|OBJECT.COMPILE|Compilazione di stored procedure.|  
|OBJECT.INDEX_OPERATION|Operazioni sugli indici.|  
|OBJECT.UPDSTATS|Aggiornamenti alle statistiche di una tabella.|  
|METADATA.ASSEMBLY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_CLR_NAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_TOKEN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASYMMETRIC_KEY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_ACTIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_SPECIFICATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AVAILABILITY_GROUP|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CERTIFICATE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CHILD_INSTANCE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_FRAGMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_ROWSET|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_RECV|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_SEND|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_GROUP|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_PRIORITY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CREDENTIAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CRYPTOGRAPHIC_PROVIDER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATA_SPACE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE_PRINCIPAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_SESSION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_WITNESS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_PRINCIPAL_SID|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT_WEBMETHOD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_COLUMN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_STOPLIST|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEX_EXTENSION_SCHEME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEXSTATS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INSTANTIATED_TYPE_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.MESSAGE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.METADATA_CACHE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PARTITION_FUNCTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PASSWORD_POLICY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PERMISSIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE_SCOPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.REMOTE_SERVICE_BINDING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ROUTE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SCHEMA|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_CACHE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_DESCRIPTOR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SEQUENCE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_EVENT_SESSIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_PRINCIPAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_BROKER_GUID|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_CONTRACT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_MESSAGE_TYPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.STATS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SYMMETRIC_KEY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.USER_TYPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COLLECTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COMPONENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_INDEX_QNAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 Nella tabella seguente viene descritto il formato della colonna **resource_description** per ogni tipo di risorsa.  
  
|Risorsa|Formato|Descrizione|  
|--------------|------------|-----------------|  
|DATABASE|Non applicabile|L'ID del database è già disponibile nella colonna **resource_database_id**.|  
|FILE|<file_id>|ID del file rappresentato dalla risorsa.|  
|OBJECT|<object_id>|ID dell'oggetto rappresentato dalla risorsa. Può essere qualsiasi oggetto elencato in **sys.objects**, non solo una tabella.|  
|PAGE|<file_id>:<page_in_file>|Rappresenta l'ID di pagina e di file della pagina rappresentata dalla risorsa.|  
|KEY|<hash_value>|Rappresenta un hash delle colonne chiave dalla riga rappresentata dalla risorsa.|  
|EXTENT|<file_id>:<page_in_files>|Rappresenta l'ID di pagina e di file dell'extent rappresentato dalla risorsa. L'ID di extent corrisponde all'ID di pagina della prima pagina nell'extent.|  
|RID|<file_id>:<page_in_file>:<row_on_page>|Rappresenta l'ID di pagina e l'ID di riga della riga rappresentata dalla risorsa. Se l'ID dell'oggetto associato è 99, la risorsa rappresenta uno degli otto slot di pagine miste nella prima pagina IAM di una catena IAM.|  
|APPLICATION|\<DbPrincipalId>: \<upto 32 characters> :(<hash_value>)|Rappresenta l'ID dell'entità di database utilizzata per definire l'ambito della risorsa di blocco dell'applicazione. È incluso anche un massimo di 32 caratteri della stringa della risorsa corrispondente alla risorsa di blocco dell'applicazione. In certi casi, è possibile visualizzare solo 2 caratteri perché la stringa completa non è più disponibile. Ciò si verifica solo in fase di recupero del database per i blocchi dell'applicazione che vengono riacquisiti nell'ambito del processo di recupero. Il valore hash rappresenta un hash della stringa di risorsa completa corrispondente a questa risorsa di blocco dell'applicazione.|  
|HOBT|Non applicabile|L'ID di risorsa HoBT è incluso come **resource_associated_entity_id**.|  
|ALLOCATION_UNIT|Non applicabile|L'ID dell'unità di allocazione è incluso come **resource_associated_entity_id**.|  
|METADATA.ASSEMBLY|assembly_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_CLR_NAME|$qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_TOKEN|assembly_id = A, $token_id|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSYMMETRIC_KEY|asymmetric_key_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT|audit_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_ACTIONS|device_id = D, major_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_SPECIFICATION|audit_specification_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AVAILABILITY_GROUP|availability_group_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CERTIFICATE|certificate_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CHILD_INSTANCE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_FRAGMENT|object_id = O , compressed_fragment_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_ROW|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_RECV|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_SEND|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_GROUP|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_PRIORITY|conversation_priority_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CREDENTIAL|credential_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CRYPTOGRAPHIC_PROVIDER|provider_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATA_SPACE|data_space_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE|database_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE_PRINCIPAL|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_SESSION|database_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_WITNESS|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_PRINCIPAL_SID|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT|endpoint_id = E|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT_WEBMETHOD|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|fulltext_catalog_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_COLUMN|object_id = O, column_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_HASH|object_id = O, $hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|fulltext_catalog_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_STOPLIST|fulltext_stoplist_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEX_EXTENSION_SCHEME|index_extension_id = I|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEXSTATS|object_id = O, index_id o stats_id = I|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INSTANTIATED_TYPE_HASH|user_type_id = U, hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.MESSAGE|message_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.METADATA_CACHE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PARTITION_FUNCTION|function_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PASSWORD_POLICY|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PERMISSIONS|class = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE|plan_guide_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA. PLAN_GUIDE_HASH|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA. PLAN_GUIDE_SCOPE|scope_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME|$qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME_HASH|$qname_scope_id = Q, $qname_hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.REMOTE_SERVICE_BINDING|remote_service_binding_id = R|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ROUTE|route_id = R|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SCHEMA|schema_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_CACHE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_DESCRIPTOR|sd_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SEQUENCE|$seq_type = S, object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER|server_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_EVENT_SESSIONS|event_session_id = E|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_PRINCIPAL|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE|service_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_BROKER_GUID|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_CONTRACT|service_contract_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_MESSAGE_TYPE|message_type_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.STATS|object_id = O, stats_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SYMMETRIC_KEY|symmetric_key_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.USER_TYPE|user_type_id = U|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COLLECTION|xml_collection_id = X|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COMPONENT|xml_component_id = X|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_INDEX_QNAME|object_id = O, $qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 I seguenti XEvent sono correlati al **cambio** di partizione e alla ricompilazione dell'indice online. Per informazioni sulla sintassi, vedere [ALTER TABLE &#40;Transact-sql&#41;](../../t-sql/statements/alter-table-transact-sql.md) e [alter index &#40;transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 Il **Progress_report_online_index_operation** XEvent esistente per le operazioni sugli indici online è stato esteso aggiungendo **partition_number** e **partition_id**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-sysdm_tran_locks-with-other-tools"></a>R. Utilizzo di sys.dm_tran_locks con altri strumenti  
 L'esempio seguente illustra uno scenario in cui un'operazione di aggiornamento è bloccata da un'altra transazione. Utilizzando **sys.dm_tran_locks** e altri strumenti, vengono fornite informazioni sulle risorse di blocco.  
  
```sql  
USE tempdb;  
GO  
  
-- Create test table and index.  
CREATE TABLE t_lock  
    (  
    c1 int, c2 int  
    );  
GO  
  
CREATE INDEX t_lock_ci on t_lock(c1);  
GO  
  
-- Insert values into test table  
INSERT INTO t_lock VALUES (1, 1);  
INSERT INTO t_lock VALUES (2,2);  
INSERT INTO t_lock VALUES (3,3);  
INSERT INTO t_lock VALUES (4,4);  
INSERT INTO t_lock VALUES (5,5);  
INSERT INTO t_lock VALUES (6,6);  
GO  
  
-- Session 1  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
  
BEGIN TRAN  
    SELECT c1  
        FROM t_lock  
        WITH(holdlock, rowlock);  
  
-- Session 2  
BEGIN TRAN  
    UPDATE t_lock SET c1 = 10  
```  
  
 La query seguente visualizza informazioni sui blocchi. Il valore di `<dbid>` deve essere sostituito con il valore **database_id** di **sys.databases**.  
  
```sql  
SELECT resource_type, resource_associated_entity_id,  
    request_status, request_mode,request_session_id,  
    resource_description   
    FROM sys.dm_tran_locks  
    WHERE resource_database_id = <dbid>  
```  
  
 La query seguente restituisce informazioni sugli oggetti utilizzando `resource_associated_entity_id` dalla query precedente. Questa query deve essere eseguita durante la connessione al database contenente l'oggetto.  
  
```  
SELECT object_name(object_id), *  
    FROM sys.partitions  
    WHERE hobt_id=<resource_associated_entity_id>  
```  
  
 La query seguente visualizza informazioni di blocco.  
  
```sql  
SELECT   
        t1.resource_type,  
        t1.resource_database_id,  
        t1.resource_associated_entity_id,  
        t1.request_mode,  
        t1.request_session_id,  
        t2.blocking_session_id  
    FROM sys.dm_tran_locks as t1  
    INNER JOIN sys.dm_os_waiting_tasks as t2  
        ON t1.lock_owner_address = t2.resource_address;  
```  
  
 Rilasciare le risorse eseguendo il rollback delle transazioni.  
  
```sql  
-- Session 1  
ROLLBACK;  
GO  
  
-- Session 2  
ROLLBACK;  
GO  
```  
  
### <a name="b-linking-session-information-to-operating-system-threads"></a>B. Collegamento tra informazioni di sessione e thread del sistema operativo  
 Nell'esempio seguente vengono restituite le informazioni che associato un ID di sessione a un ID di thread di Windows. È possibile utilizzare Performance Monitor di Windows per monitorare le prestazioni del thread. Questa query non restituisce gli ID di sessione attualmente sospesi.  
  
```sql  
SELECT STasks.session_id, SThreads.os_thread_id  
    FROM sys.dm_os_tasks AS STasks  
    INNER JOIN sys.dm_os_threads AS SThreads  
        ON STasks.worker_address = SThreads.worker_address  
    WHERE STasks.session_id IS NOT NULL  
    ORDER BY STasks.session_id;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[sys.dm_tran_database_transactions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)      
[Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[Funzioni e viste a gestione dinamica relative alle transazioni &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)      
[Oggetto Locks di SQL Server](../../relational-databases/performance-monitor/sql-server-locks-object.md)