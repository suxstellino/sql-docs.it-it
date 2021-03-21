---
description: sys.dm_exec_procedure_stats (Transact-SQL)
title: sys.dm_exec_procedure_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats
- sys.dm_exec_procedure_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_procedure_stats dynamic management view
ms.assetid: ab8ddde8-1cea-4b41-a7e4-697e6ddd785a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0556a20a490aad3663144f8a34bab5ee54b65501
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751801"
---
# <a name="sysdm_exec_procedure_stats-transact-sql"></a>sys.dm_exec_procedure_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce dati statistici aggregati sulle prestazioni delle stored procedure memorizzate nella cache. La vista restituisce una riga per ogni piano di stored procedure memorizzato nella cache e la durata della riga è uguale al periodo in cui la stored procedure rimane memorizzata nella cache. Quando una stored procedure viene rimossa dalla cache, la riga corrispondente viene elimina dalla vista. A questo punto, viene generato un evento di traccia SQL di Performance Statistics simile a **sys.dm_exec_query_stats**.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  
  
> [!NOTE]
> I risultati di **sys.dm_exec_procedure_stats**  possono variare a seconda dell'esecuzione, perché i dati riflettono solo le query finite e non quelli ancora in corso.
> Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_exec_procedure_stats**. 

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------| 
|**database_id**|**int**|ID del database in cui è contenuta la stored procedure.|  
|**object_id**|**int**|Numero di identificazione dell'oggetto della stored procedure.|  
|**type**|**char(2)**|Tipo dell'oggetto:<br /><br /> P = stored procedure SQL<br /><br /> PC = stored procedure assembly (CLR)<br /><br /> X = stored procedure estesa|  
|**type_desc**|**nvarchar(60)**|Descrizione del tipo di oggetto:<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> EXTENDED_STORED_PROCEDURE|  
|**sql_handle**|**varbinary(64)**|Questa operazione può essere utilizzata per la correlazione con le query in **sys.dm_exec_query_stats** eseguite dall'interno di questo stored procedure.|  
|**plan_handle**|**varbinary(64)**|Identificatore del piano in memoria. Si tratta di un identificatore temporaneo, che rimane costante solo se il piano rimane nella cache. Questo valore può essere utilizzato con la vista a gestione dinamica **sys.dm_exec_cached_plans** .<br /><br /> È sempre 0x000 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**cached_time**|**datetime**|Ora in cui la stored procedure è stata aggiunta alla cache.|  
|**last_execution_time**|**datetime**|Ora dell'ultima esecuzione della stored procedure.|  
|**execution_count**|**bigint**|Il numero di volte in cui il stored procedure è stato eseguito dopo l'ultima compilazione.|  
|**total_worker_time**|**bigint**|Quantità totale di tempo di CPU, in microsecondi, utilizzato dalle esecuzioni di questo stored procedure da quando è stato compilato.<br /><br /> Per le stored procedure compilate in modo nativo, il valore di **total_worker_time** non può essere accurato se più esecuzioni richiedono meno di 1 millisecondo.|  
|**last_worker_time**|**bigint**|Tempo di CPU, in microsecondi, utilizzato durante l'ultima esecuzione della stored procedure. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Tempo minimo di CPU, in microsecondi, utilizzato da questo stored procedure durante una singola esecuzione. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Tempo massimo di CPU, in microsecondi, utilizzato da questo stored procedure durante una singola esecuzione. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Numero totale di letture fisiche effettuate dalle esecuzioni di questo stored procedure da quando è stato compilato.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_physical_reads**|**bigint**|Numero di letture fisiche eseguite durante l'ultima esecuzione del stored procedure.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_physical_reads**|**bigint**|Numero minimo di letture fisiche effettuate da questo stored procedure durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_physical_reads**|**bigint**|Numero massimo di letture fisiche effettuate da questo stored procedure durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_logical_writes**|**bigint**|Numero totale di scritture logiche effettuate dalle esecuzioni di questo stored procedure da quando è stato compilato.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_logical_writes**|**bigint**|Numero di pagine del pool di buffer sporcate durante l'ultima esecuzione del piano. Se una pagina è già dirty (modificata) le scritture non vengono conteggiate.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_logical_writes**|**bigint**|Numero minimo di scritture logiche effettuate da questo stored procedure durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_logical_writes**|**bigint**|Numero massimo di scritture logiche effettuate da questo stored procedure durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_logical_reads**|**bigint**|Numero totale di letture logiche effettuate dalle esecuzioni di questo stored procedure da quando è stato compilato.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_logical_reads**|**bigint**|Numero di letture logiche eseguite durante l'ultima esecuzione del stored procedure.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_logical_reads**|**bigint**|Numero minimo di letture logiche effettuate da questo stored procedure durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_logical_reads**|**bigint**|Numero massimo di letture logiche effettuate da questo stored procedure durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_elapsed_time**|**bigint**|Tempo totale trascorso, in microsecondi, per le esecuzioni completate di questo stored procedure.|  
|**last_elapsed_time**|**bigint**|Tempo trascorso, in microsecondi, per l'ultima esecuzione completata del stored procedure.|  
|**min_elapsed_time**|**bigint**|Tempo minimo trascorso, in microsecondi, per qualsiasi esecuzione completata di questo stored procedure.|  
|**max_elapsed_time**|**bigint**|Tempo massimo trascorso, in microsecondi, per qualsiasi esecuzione completata di questo stored procedure.|  
|**total_spills**|**bigint**|Numero totale di pagine rilasciate dall'esecuzione di questo stored procedure da quando è stato compilato.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Numero di pagine distribuite durante l'ultima esecuzione del stored procedure.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Numero minimo di pagine che questo stored procedure ha mai distribuito durante una singola esecuzione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Il numero massimo di pagine che questo stored procedure ha mai distribuito durante una singola esecuzione.<br /><br /> **Si applica a**: a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Identificatore del nodo su cui si trova questa distribuzione.<br /><br />**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
|**total_page_server_reads**|**bigint**|Numero totale di letture di pagine del server eseguite dalle esecuzioni di questo stored procedure da quando è stato compilato.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
|**last_page_server_reads**|**bigint**|Numero di letture di pagine del server eseguite durante l'ultima esecuzione del stored procedure.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
|**min_page_server_reads**|**bigint**|Numero minimo di letture di pagine del server eseguite da questo stored procedure durante una singola esecuzione.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
|**max_page_server_reads**|**bigint**|Numero massimo di letture di pagine del server eseguite da questo stored procedure durante una singola esecuzione.<br /><br /> **Si applica a**: iperscalabilità del database SQL di Azure|  
  
 <sup>1</sup> per le stored procedure compilate in modo nativo quando la raccolta delle statistiche è abilitata, il tempo di lavoro viene raccolto in millisecondi. Se la query viene eseguita in meno di un millisecondo, il valore sarà 0.  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
   
## <a name="remarks"></a>Commenti  
 Le statistiche nella vista vengono aggiornate quando viene completata l'esecuzione della stored procedure.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sulle prime dieci stored procedure identificate in base al tempo medio trascorso.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'proc name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_procedure_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Vedere anche  
[Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_query_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)    
[sys.dm_exec_trigger_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)    
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  
