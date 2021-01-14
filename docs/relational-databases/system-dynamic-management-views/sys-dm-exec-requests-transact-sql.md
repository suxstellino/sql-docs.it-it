---
description: sys.dm_exec_requests (Transact-SQL)
title: sys.dm_exec_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_requests_TSQL
- sys.dm_exec_requests
- dm_exec_requests_TSQL
- dm_exec_requests
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_requests dynamic management view
ms.assetid: 4161dc57-f3e7-4492-8972-8cfb77b29643
author: pmasl
ms.author: pelopes
ms.reviewer: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a1ad13d4cdbcf8820a318a87f4ef9f52c488a406
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171263"
---
# <a name="sysdm_exec_requests-transact-sql"></a>sys.dm_exec_requests (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce informazioni su ogni richiesta in esecuzione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni sulle richieste, vedere la [Guida all'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md).
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|ID della sessione a cui la richiesta è correlata. Non ammette i valori Null.|  
|request_id|**int**|ID della richiesta. Valore univoco nel contesto della sessione. Non ammette i valori Null.|  
|start_time|**datetime**|Timestamp relativo all'arrivo della richiesta. Non ammette i valori Null.|  
|status|**nvarchar(30)**|Stato della richiesta. I possibili valori sono i seguenti:<br /><br /> Informazioni<br />In esecuzione<br />Eseguibile<br />Sospeso<br />Suspended<br /><br /> Non ammette i valori Null.|  
|.|**nvarchar(32)**|Identifica il tipo di comando corrente in corso di elaborazione. I tipi di comandi più comuni sono i seguenti:<br /><br /> SELECT<br />INSERT<br />UPDATE<br />DELETE<br />BACKUP LOG<br />BACKUP DATABASE<br />DBCC<br />FOR<br /><br /> Per recuperare il testo della richiesta, utilizzare sys.dm_exec_sql_text con il valore sql_handle corrispondente per la richiesta. I processi interni di sistema impostano il comando in base al tipo di attività effettuata. Di seguito sono riportate le attività:<br /><br /> LOCK MONITOR<br />CHECKPOINTLAZY<br />WRITER<br /><br /> Non ammette i valori Null.|  
|sql_handle|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure di cui fa parte la query. Ammette i valori Null.| 
|statement_start_offset|**int**|Indica, in byte, a partire da 0, la posizione iniziale dell'istruzione attualmente in esecuzione per il batch o l'oggetto permanente attualmente in esecuzione. Può essere usato insieme a `sql_handle` , `statement_end_offset` e alla `sys.dm_exec_sql_text` funzione a gestione dinamica per recuperare l'istruzione attualmente in esecuzione per la richiesta. Ammette i valori Null.|  
|statement_end_offset|**int**|Indica, in byte, a partire da 0, la posizione finale dell'istruzione attualmente in esecuzione per il batch o l'oggetto permanente attualmente in esecuzione. Può essere usato insieme a `sql_handle` , `statement_start_offset` e alla `sys.dm_exec_sql_text` funzione a gestione dinamica per recuperare l'istruzione attualmente in esecuzione per la richiesta. Ammette i valori Null.|  
|plan_handle|**varbinary(64)**|Token che identifica in modo univoco un piano di esecuzione della query per un batch attualmente in esecuzione. Ammette i valori Null.|  
|database_id|**smallint**|ID del database utilizzato per eseguire la richiesta. Non ammette i valori Null.|  
|user_id|**int**|ID dell'utente che ha inviato la richiesta. Non ammette i valori Null.|  
|connection_id|**uniqueidentifier**|ID della connessione nella quale è arrivata la richiesta. Ammette i valori Null.|  
|blocking_session_id|**smallint**|ID della sessione che sta bloccando la richiesta. Se questa colonna è NULL o uguale a 0, la richiesta non è bloccata oppure le informazioni sulla sessione della sessione di blocco non sono disponibili (o non possono essere identificate).<br /><br /> -2 = La risorsa di blocco appartiene a una transazione distribuita orfana.<br /><br /> -3 = La risorsa di blocco appartiene a una transazione di recupero posticipata.<br /><br /> -4 = Al momento non è stato possibile determinare l'ID sessione del proprietario del latch di blocco a causa di transizioni nello stato del latch interno.|  
|wait_type|**nvarchar(60)**|Se la richiesta è momentaneamente bloccata, in questa colonna viene restituito il tipo di attesa. Ammette i valori Null.<br /><br /> Per informazioni sui tipi di attese, vedere [sys.dm_os_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|wait_time|**int**|Se la richiesta è momentaneamente bloccata, in questa colonna viene restituita la durata dell'attesa corrente espressa in millisecondi. Non ammette i valori Null.|  
|last_wait_type|**nvarchar(60)**|Se la richiesta è stata precedentemente bloccata, questa colonna restituisce il tipo dell'ultima attesa. Non ammette i valori Null.|  
|wait_resource|**nvarchar(256)**|Se la richiesta è momentaneamente bloccata, questa colonna restituisce la risorsa per la quale la richiesta è in attesa. Non ammette i valori Null.|  
|open_transaction_count|**int**|Numero di transazioni aperte per la richiesta. Non ammette i valori Null.|  
|open_resultset_count|**int**|Numero di set di risultati aperti per la richiesta. Non ammette i valori Null.|  
|transaction_id|**bigint**|ID della transazione nella quale viene eseguita la richiesta. Non ammette i valori Null.|  
|context_info|**varbinary(128)**|Valore di CONTEXT_INFO della sessione. Ammette i valori Null.|  
|percent_complete|**real**|Percentuale di lavoro completata per i comandi seguenti:<br /><br /> ALTER INDEX REORGANIZE<br />Opzione AUTO_SHRINK con ALTER DATABASE<br />BACKUP DATABASE<br />DBCC CHECKDB<br />DBCC CHECKFILEGROUP<br />DBCC CHECKTABLE<br />DBCC INDEXDEFRAG<br />DBCC SHRINKDATABASE<br />DBCC SHRINKFILE<br />RECOVERY<br />RESTORE DATABASE<br />ROLLBACK<br />TDE ENCRYPTION<br /><br /> Non ammette i valori Null.|  
|estimated_completion_time|**bigint**|Solo interno. Non ammette i valori Null.|  
|cpu_time|**int**|Tempo della CPU utilizzato dalla richiesta, espresso in millisecondi. Non ammette i valori Null.|  
|total_elapsed_time|**int**|Tempo totale, in millisecondi, trascorso dall'arrivo della richiesta. Non ammette i valori Null.|  
|scheduler_id|**int**|ID dell'utilità di pianificazione che sta pianificando la richiesta. Non ammette i valori Null.|  
|task_address|**varbinary (8)**|Indirizzo di memoria allocato all'attività associata alla richiesta. Ammette i valori Null.|  
|reads|**bigint**|Numero di letture effettuate dalla richiesta. Non ammette i valori Null.|  
|writes|**bigint**|Numero di scritture effettuate dalla richiesta. Non ammette i valori Null.|  
|logical_reads|**bigint**|Numero di letture logiche effettuate dalla richiesta. Non ammette i valori Null.|  
|text_size|**int**|Impostazione di TEXTSIZE per la richiesta. Non ammette i valori Null.|  
|Linguaggio|**nvarchar(128)**|Impostazione di LANGUAGE per la richiesta. Ammette i valori Null.|  
|date_format|**nvarchar (3)**|Impostazione di DATEFORMAT per la richiesta. Ammette i valori Null.|  
|date_first|**smallint**|Impostazione di DATEFIRST per la richiesta. Non ammette i valori Null.|  
|quoted_identifier|**bit**|1 = QUOTED_IDENTIFIER è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|arithabort|**bit**|1 = ARITHABORT è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|ansi_null_dflt_on|**bit**|1 = ANSI_NULL_DFLT_ON è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|ansi_defaults|**bit**|1 = ANSI_DEFAULTS è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|ansi_warnings|**bit**|1 = ANSI_WARNINGS è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|ansi_padding|**bit**|1 = ANSI_PADDING è impostata su ON per la richiesta.<br /><br /> Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|ansi_nulls|**bit**|1 = ANSI_NULLS è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|concat_null_yields_null|**bit**|1 = CONCAT_NULL_YIELDS_NULL è impostata su ON per la richiesta. Negli altri casi è 0.<br /><br /> Non ammette i valori Null.|  
|transaction_isolation_level|**smallint**|Livello di isolamento con cui è stata creata la transazione per questa richiesta. Non ammette i valori Null.<br /> 0 = Non specificato<br /> 1 = ReadUncomitted<br /> 2 = ReadCommitted<br /> 3 = Repeatable<br /> 4 = Serializable<br /> 5 = Snapshot|  
|lock_timeout|**int**|Periodo di timeout del blocco, espresso in millisecondi, per la richiesta. Non ammette i valori Null.|  
|deadlock_priority|**int**|Impostazione di DEADLOCK_PRIORITY per la richiesta. Non ammette i valori Null.|  
|row_count|**bigint**|Numero di righe restituite al client dalla richiesta. Non ammette i valori Null.|  
|prev_error|**int**|Ultimo errore che si è verificato durante l'esecuzione della richiesta. Non ammette i valori Null.|  
|nest_level|**int**|Livello di nidificazione corrente del codice eseguito nella richiesta. Non ammette i valori Null.|  
|granted_query_memory|**int**|Numero di pagine allocate all'esecuzione di una query nella richiesta. Non ammette i valori Null.|  
|executing_managed_code|**bit**|Indica se una richiesta specifica sta eseguendo oggetti CLR (Common Language Runtime) quali routine, tipi e trigger. Il valore rimane impostato per l'intero periodo di permanenza di un oggetto CLR nello stack, anche durante l'esecuzione di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] internamente a CLR. Non ammette i valori Null.|  
|group_id|**int**|ID del gruppo del carico di lavoro a cui appartiene la query. Non ammette i valori Null.|  
|query_hash|**binario (8)**|Valore hash binario calcolato sulla query che consente di identificare query con logica analoga. È possibile utilizzare il valore hash della query per determinare l'utilizzo delle risorse aggregate per query che differiscono solo per valori letterali.|  
|query_plan_hash|**binario (8)**|Valore hash binario calcolato sul piano di esecuzione di query che consente di identificare piani di esecuzioni analoghi. È possibile utilizzare il valore hash del piano di query per individuare il costo cumulativo di query con piani di esecuzione analoghi.|  
|statement_sql_handle|**varbinary(64)**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Handle SQL della singola query.<br /><br />Se Query Store non è abilitato per il database, questa colonna è NULL. |  
|statement_context_id|**bigint**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Chiave esterna facoltativa da sys.query_context_settings.<br /><br />Se Query Store non è abilitato per il database, questa colonna è NULL. |  
|dop |**int** |**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.<br /><br /> Grado di parallelismo della query. |  
|parallel_worker_count |**int** |**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.<br /><br /> Numero di processi di lavoro paralleli riservati se si tratta di una query parallela.  |  
|external_script_request_id |**uniqueidentifier** |**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.<br /><br /> ID della richiesta di script esterno associato alla richiesta corrente. |  
|is_resumable |**bit** |**Si applica a**: [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] e versioni successive.<br /><br /> Indica se la richiesta è un'operazione sugli indici ripristinabili. |  
|page_resource |**binario (8)** |**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]<br /><br /> Rappresentazione esadecimale a 8 byte della risorsa della pagina se la `wait_resource` colonna contiene una pagina. Per ulteriori informazioni, vedere [sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md). |  
|page_server_reads|**bigint**|**Si applica a**: iperscalabilità del database SQL di Azure<br /><br /> Numero di letture di pagine del server eseguite dalla richiesta. Non ammette i valori Null.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>Commenti 
Per eseguire codice esterno a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio stored procedure estese e query distribuite, è necessario che un thread venga eseguito esternamente al controllo dell'utilità di pianificazione in modalità non preemptive. A tale scopo, un thread di lavoro passa alla modalità preemptive. I valori temporali restituiti da questa DMW non includono il tempo trascorso in modalità preemptive.

Quando si eseguono richieste parallele in [modalità riga](../../relational-databases/query-processing-architecture-guide.md#row-mode-execution), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna un thread di lavoro per coordinare i thread di lavoro responsabili del completamento delle attività ad essi assegnati. In questa DMV solo il thread coordinatore è visibile per la richiesta. Le colonne **Read**, **writes**, **logical_reads** e **ROW_COUNT** **non vengono aggiornate** per il thread coordinatore. Le colonne **wait_type**, **wait_time**, **last_wait_type**, **wait_resource** e **granted_query_memory** vengono **aggiornate solo** per il thread coordinatore. Per altre informazioni, vedere [Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md).

## <a name="permissions"></a>Autorizzazioni
Se l'utente dispone dell' `VIEW SERVER STATE` autorizzazione per il server, l'utente visualizzerà tutte le sessioni in esecuzione nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . in caso contrario, l'utente visualizzerà solo la sessione corrente. `VIEW SERVER STATE` non può essere concesso nel database SQL di Azure `sys.dm_exec_requests` , pertanto è sempre limitato alla connessione corrente.

Negli scenari di Always-On, se la replica secondaria è impostata **solo per finalità di lettura**, la connessione al database secondario deve specificare la finalità dell'applicazione nei parametri della stringa di connessione aggiungendo `applicationintent=readonly` . In caso contrario, il controllo di accesso per `sys.dm_exec_requests` non passerà per i database nel gruppo di disponibilità, anche se `VIEW SERVER STATE` è presente l'autorizzazione.

  
## <a name="examples"></a>Esempi  
  
### <a name="a-finding-the-query-text-for-a-running-batch"></a>R. Ricerca del testo della query per un batch in esecuzione

 Nell'esempio seguente viene eseguita una query su `sys.dm_exec_requests` per trovare la query specifica e copiare `sql_handle` dall'output.  

```sql
SELECT * FROM sys.dm_exec_requests;  
GO  
```  

Per ottenere il testo dell'istruzione, utilizzare il `sql_handle` copiato con la funzione di sistema `sys.dm_exec_sql_text(sql_handle)`.  

```sql
SELECT * FROM sys.dm_exec_sql_text(< copied sql_handle >);  
GO  
```

### <a name="b-finding-all-locks-that-a-running-batch-is-holding"></a>B. Ricerca di tutti i blocchi contenuti in un batch in esecuzione

Nell'esempio seguente viene eseguita una query **sys.dm_exec_requests** per trovare il batch interessante e copiarne l'oggetto `transaction_id` dall'output.

```sql
SELECT * FROM sys.dm_exec_requests;  
GO
```

Quindi, per trovare informazioni sul blocco, usare l'oggetto copiato `transaction_id` con la funzione di sistema **sys.dm_tran_locks**.  

```sql
SELECT * FROM sys.dm_tran_locks
WHERE request_owner_type = N'TRANSACTION'
    AND request_owner_id = < copied transaction_id >;
GO  
```

### <a name="c-finding-all-currently-blocked-requests"></a>C. Ricerca di tutte le richieste attualmente bloccate

Nell'esempio seguente viene eseguita una query **sys.dm_exec_requests** per trovare informazioni sulle richieste bloccate.  

```sql
SELECT session_id ,status ,blocking_session_id  
    ,wait_type ,wait_time ,wait_resource
    ,transaction_id
FROM sys.dm_exec_requests
WHERE status = N'suspended';  
GO  
```  

### <a name="d-ordering-existing-requests-by-cpu"></a>D. Ordinamento delle richieste esistenti per CPU

```sql
SELECT 
   req.session_id
   , req.start_time
   , cpu_time 'cpu_time_ms'
   , object_name(st.objectid,st.dbid) 'ObjectName' 
   , substring
      (REPLACE
        (REPLACE
          (SUBSTRING
            (ST.text
            , (req.statement_start_offset/2) + 1
            , (
               (CASE statement_end_offset
                  WHEN -1
                  THEN DATALENGTH(ST.text)  
                  ELSE req.statement_end_offset
                  END
                    - req.statement_start_offset)/2) + 1)
       , CHAR(10), ' '), CHAR(13), ' '), 1, 512)  AS statement_text  
FROM sys.dm_exec_requests AS req  
   CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) as ST
   ORDER BY cpu_time desc;
GO
```

## <a name="see-also"></a>Vedere anche
[Funzioni e viste a gestione dinamica](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[Funzioni e viste a gestione dinamica relative all'esecuzione](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)      
[sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)     
[sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)     
[sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)    
[sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)      
[SQL Server, oggetto Statistiche SQL](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)     
[Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md)    
