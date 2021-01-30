---
description: sys.dm_exec_query_memory_grants (Transact-SQL)
title: sys.dm_exec_query_memory_grants (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_memory_grants_TSQL
- sys.dm_exec_query_memory_grants
- sys.dm_exec_query_memory_grants_TSQL
- dm_exec_query_memory_grants
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_memory_grants dynamic management view
ms.assetid: 2c417747-2edd-4e0d-8a9c-e5f445985c1a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7a59efc8b1e6795082d4d7f4e4973522cf371ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198562"
---
# <a name="sysdm_exec_query_memory_grants-transact-sql"></a>sys.dm_exec_query_memory_grants (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni su tutte le query che hanno richiesto e sono in attesa di una concessione di memoria o di una concessione di memoria. Le query che non richiedono una concessione di memoria non verranno visualizzate in questa visualizzazione. Ad esempio, le operazioni di ordinamento e hash join hanno concessioni di memoria per l'esecuzione di query, mentre le query senza una clausola **Order by** non avranno una concessione di memoria.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata. Inoltre, i valori nelle colonne **scheduler_id**, **wait_order**, **pool_id** **group_id** vengono filtrati; il valore della colonna è impostato su NULL.  
  
> [!NOTE]  
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_exec_query_memory_grants**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|ID (SPID) della sessione nella quale viene eseguita la query.|  
|**request_id**|**int**|ID della richiesta. Valore univoco nel contesto della sessione.|  
|**scheduler_id**|**int**|ID dell'utilità di pianificazione che sta pianificando la query.|  
|**dop**|**smallint**|Grado di parallelismo della query.|  
|**request_time**|**datetime**|Data e ora in cui la query ha richiesto la concessione di memoria.|  
|**grant_time**|**datetime**|Data e ora in cui la memoria è stata concessa alla query. È NULL se la memoria non è stata ancora concessa.|  
|**requested_memory_kb**|**bigint**|Quantità totale di memoria richiesta, espressa in kilobyte.|  
|**granted_memory_kb**|**bigint**|Quantità totale di memoria effettivamente concessa, espressa in kilobyte. Può essere NULL se la memoria non è stata ancora concessa. In una situazione tipica questo valore dovrebbe corrispondere al valore di **requested_memory_kb**. In caso di creazione di indici, il server può concedere ulteriore memoria su richiesta in aggiunta alla memoria concessa inizialmente.|  
|**required_memory_kb**|**bigint**|Quantità minima di memoria necessaria per l'esecuzione della query, espressa in kilobyte. **requested_memory_kb** è uguale o maggiore di questa quantità.|  
|**used_memory_kb**|**bigint**|Memoria fisica attualmente in uso, espressa in kilobyte.|  
|**max_used_memory_kb**|**bigint**|Memoria fisica massima utilizzata fino a questo momento, espressa in kilobyte.|  
|**query_cost**|**float**|Costo stimato della query.|  
|**timeout_sec**|**int**|Timeout in secondi prima che la query rinunci alla richiesta di concessione di memoria.|  
|**resource_semaphore_id**|**smallint**|ID non univoco del semaforo di risorsa su cui la query è in attesa.<br /><br /> **Nota:** Questo ID è univoco nelle versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . La modifica può influire sulla risoluzione dei problemi relativi all'esecuzione di query. Per ulteriori informazioni, vedere la sezione "Osservazioni" di seguito in questo argomento.|  
|**queue_id**|**smallint**|ID della coda nella quale la query sta attendendo la concessione di memoria. È NULL se la memoria è già stata concessa.|  
|**wait_order**|**int**|Ordine sequenziale delle query in attesa all'interno del **queue_id** specificato. Questo valore può essere modificato per una determinata query se altre query ottengono la concessione di memoria o il timeout. NULL se la memoria è già stata concessa.|  
|**is_next_candidate**|**bit**|Candidato alla concessione di memoria successiva.<br /><br /> 1 = Sì<br /><br /> 0 = No<br /><br /> NULL = Memoria già concessa|  
|**wait_time_ms**|**bigint**|Periodo di attesa espresso in millisecondi. È NULL se la memoria è già stata concessa.|  
|**plan_handle**|**varbinary(64)**|Identificatore del piano di query. Utilizzare **sys.dm_exec_query_plan** per estrarre il piano XML effettivo.|  
|**sql_handle**|**varbinary(64)**|Identificatore del testo [!INCLUDE[tsql](../../includes/tsql-md.md)] della query. Utilizzare **sys.dm_exec_sql_text** per ottenere il testo [!INCLUDE[tsql](../../includes/tsql-md.md)] effettivo.|  
|**group_id**|**int**|ID per il gruppo del carico di lavoro nel quale viene eseguita la query.|  
|**pool_id**|**int**|ID del pool di risorse a cui appartiene il gruppo del carico di lavoro.|  
|**is_small**|**tinyint**|Se il valore è 1, questa concessione utilizza il semaforo piccolo di risorsa. Se il valore è 0, viene utilizzato un semaforo normale.|  
|**ideal_memory_kb**|**bigint**|Dimensioni, in kilobyte (KB), della concessione di memoria per inserire tutto nella memoria fisica. Si basa su una stima della cardinalità.|  
|**pdw_node_id**|**int**|Identificatore del nodo su cui si trova questa distribuzione.<br /><br /> **Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
|**reserved_worker_count**|**bigint**|Numero di [thread di lavoro](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling)riservati.<br /><br />**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|**used_worker_count**|**bigint**|Numero di [thread di lavoro](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) utilizzati in questo momento.<br /><br />**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**max_used_worker_count**|**bigint**|Numero massimo di [thread di lavoro](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) utilizzati fino a questo momento.<br /><br />**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**reserved_node_bitmap**|**bigint**|Bitmap dei nodi NUMA in cui sono riservati i [thread di lavoro](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) .<br /><br />**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è richiesta l'autorizzazione `VIEW DATABASE STATE` per il database.   
   
## <a name="remarks"></a>Commenti  
 Di seguito è illustrato un tipico scenario di debug per il timeout delle query:  
  
-   Verificare lo stato complessivo della memoria di sistema utilizzando [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md), [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) e diversi contatori di prestazioni.  
  
-   Verificare le prenotazioni di memoria per l'esecuzione di query in **sys.dm_os_memory_clerks** dove `type = 'MEMORYCLERK_SQLQERESERVATIONS'`.  
  
-   Verificare la presenza di query in attesa <sup>1</sup> per le concessioni usando **sys.dm_exec_query_memory_grants**.  
  
    ```sql  
    --Find all queries waiting in the memory queue  
    SELECT * FROM sys.dm_exec_query_memory_grants where grant_time is null  
    ```  
    
    <sup>1</sup> In questo scenario il tipo di attesa è in genere RESOURCE_SEMAPHORE. Per altre informazioni, vedere [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). 
  
-   Cache di ricerca per le query con concessioni di memoria tramite [sys.dm_exec_cached_plans &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) e [sys.dm_exec_query_plan &#40;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)&#41;  
  
    ```sql  
    -- retrieve every query plan from the plan cache  
    USE master;  
    GO  
    SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
    GO  
    ```  
  
-   Esaminare in maggiore dettaglio le query che utilizzano una quantità elevata di memoria utilizzando [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).  
  
    ```sql  
    --Find top 5 queries by average CPU time  
    SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
      plan_handle, query_plan   
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
    ORDER BY total_worker_time/execution_count DESC;  
    GO  
    ```  
  
-   Se si sospetta la presenza di una query con eccessivo tempo di esecuzione, esaminare lo Showplan di [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) e il testo del batch di [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 Le query che utilizzano viste a gestione dinamica che includono `ORDER BY` o aggregazioni possono aumentare il consumo di memoria e quindi contribuire al problema che si sta risolvendo.  
  
 La funzionalità Resource Governor consente a un amministratore di database di distribuire risorse del server fra un massimo di 64 pool di risorse. A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , ogni pool si comporta come una piccola istanza indipendente del server e richiede 2 semafori. Il numero di righe restituite da **sys.dm_exec_query_resource_semaphores** può essere fino a 20 volte superiore alle righe restituite in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_exec_query_resource_semaphores &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)     
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
 [Guida sull'architettura dei thread e delle attività](../../relational-databases/thread-and-task-architecture-guide.md)   
  
