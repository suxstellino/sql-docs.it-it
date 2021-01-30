---
description: sys.dm_exec_trigger_stats (Transact-SQL)
title: sys.dm_exec_trigger_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_trigger_stats
- dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_trigger_stats dynamic management function
ms.assetid: 863498b4-849c-434d-b748-837411458738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eef7f4d6a9dd20dd12d5248ba4b09b1f7031e237
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192924"
---
# <a name="sysdm_exec_trigger_stats-transact-sql"></a>sys.dm_exec_trigger_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce dati statistici aggregati sulle prestazioni dei trigger memorizzati nella cache. La vista contiene una riga per ogni trigger e la durata della riga è uguale al periodo in cui il trigger rimane memorizzato nella cache. Quando un trigger viene rimosso dalla cache, le righe corrispondenti vengono eliminate dalla vista. A questo punto, viene generato un evento di traccia SQL di Performance Statistics simile a **sys.dm_exec_query_stats**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database in cui è contenuto il trigger.|  
|**object_id**|**int**|Numero di identificazione del trigger.|  
|**type**|**char(2)**|Tipo dell'oggetto:<br /><br /> TA = Trigger di assembly (CLR)<br /><br /> TR = trigger SQL|  
|**Type_desc**|**nvarchar(60)**|Descrizione del tipo di oggetto:<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**sql_handle**|**varbinary(64)**|Questa operazione può essere utilizzata per la correlazione con le query in **sys.dm_exec_query_stats** eseguite dall'interno del trigger.|  
|**plan_handle**|**varbinary(64)**|Identificatore del piano in memoria. Si tratta di un identificatore temporaneo, che rimane costante solo se il piano rimane nella cache. Questo valore può essere utilizzato con la vista a gestione dinamica **sys.dm_exec_cached_plans** .|  
|**cached_time**|**datetime**|Ora in cui il trigger è stato aggiunto alla cache.|  
|**last_execution_time**|**datetime**|Ora dell'ultima esecuzione del trigger.|  
|**execution_count**|**bigint**|Numero di esecuzioni del trigger a partire dall'ultima compilazione.|  
|**total_worker_time**|**bigint**|Quantità totale di tempo di CPU, in microsecondi, utilizzato dalle esecuzioni del trigger a partire dalla relativa compilazione.|  
|**last_worker_time**|**bigint**|Tempo di CPU, in microsecondi, utilizzato durante l'ultima esecuzione del trigger.|  
|**min_worker_time**|**bigint**|Tempo massimo di CPU, in microsecondi, utilizzato dal trigger durante una singola esecuzione.|  
|**max_worker_time**|**bigint**|Tempo massimo di CPU, in microsecondi, utilizzato dal trigger durante una singola esecuzione.|  
|**total_physical_reads**|**bigint**|Numero totale di letture fisiche effettuate dalle esecuzioni del trigger a partire dalla relativa compilazione.|  
|**last_physical_reads**|**bigint**|Numero di letture fisiche eseguite durante l'ultima esecuzione del trigger.|  
|**min_physical_reads**|**bigint**|Numero minimo di letture fisiche effettuate dal trigger durante una singola esecuzione.|  
|**max_physical_reads**|**bigint**|Numero massimo di letture fisiche effettuate dal trigger durante una singola esecuzione.|  
|**total_logical_writes**|**bigint**|Numero totale di scritture logiche effettuate dalle esecuzioni del trigger a partire dalla relativa compilazione.|  
|**last_logical_writes**|**bigint**|Numero di scritture logiche eseguite durante l'ultima esecuzione del trigger.|  
|**min_logical_writes**|**bigint**|Numero minimo di scritture logiche effettuate dal trigger durante una singola esecuzione.|  
|**max_logical_writes**|**bigint**|Numero massimo di scritture logiche effettuate dal trigger durante una singola esecuzione.|  
|**total_logical_reads**|**bigint**|Numero totale di letture logiche effettuate dalle esecuzioni del trigger a partire dalla relativa compilazione.|  
|**last_logical_reads**|**bigint**|Numero di letture logiche eseguite durante l'ultima esecuzione del trigger.|  
|**min_logical_reads**|**bigint**|Numero minimo di letture logiche effettuate dal trigger durante una singola esecuzione.|  
|**max_logical_reads**|**bigint**|Numero massimo di letture logiche effettuate dal trigger durante una singola esecuzione.|  
|**total_elapsed_time**|**bigint**|Tempo totale trascorso, in microsecondi, per le esecuzioni completate del trigger.|  
|**last_elapsed_time**|**bigint**|Tempo trascorso, in microsecondi, per l'ultima esecuzione completata del trigger.|  
|**min_elapsed_time**|**bigint**|Tempo minimo trascorso, in microsecondi, per qualsiasi esecuzione completata del trigger.|  
|**max_elapsed_time**|**bigint**|Tempo massimo trascorso, in microsecondi, per qualsiasi esecuzione completata del trigger.| 
|**total_spills**|**bigint**|Numero totale di pagine distribuite dall'esecuzione del trigger a partire dalla relativa compilazione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Numero di pagine distribuite durante l'ultima esecuzione del trigger.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Numero minimo di pagine che questo trigger ha mai distribuito durante una singola esecuzione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Numero massimo di pagine che questo trigger ha mai distribuito durante una singola esecuzione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**total_page_server_reads**|**bigint**|Numero totale di letture di pagine del server eseguite dalle esecuzioni del trigger a partire dalla relativa compilazione.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
|**last_page_server_reads**|**bigint**|Numero di letture di pagine del server eseguite durante l'ultima esecuzione del trigger.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
|**min_page_server_reads**|**bigint**|Numero minimo di letture di pagine del server eseguite da questo trigger durante una singola esecuzione.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
|**max_page_server_reads**|**bigint**|Numero massimo di letture di pagine del server eseguite da questo trigger durante una singola esecuzione.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  

  
## <a name="remarks"></a>Commenti  
 In [!INCLUDE[ssSDS](../../includes/sssds-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  

Le statistiche nella vista vengono aggiornate quando viene completata una query.  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sui primi cinque trigger identificati in base al tempo medio trascorso.  
  
```sql  
SELECT TOP 5 d.object_id, d.database_id, DB_NAME(database_id) AS 'database_name',   
    OBJECT_NAME(object_id, database_id) AS 'trigger_name', d.cached_time,  
    d.last_execution_time, d.total_elapsed_time,   
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],   
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_trigger_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Vedere anche  
[Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
[sys.dm_exec_procedure_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)   
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
  
