---
description: sys.dm_exec_function_stats (Transact-SQL)
title: sys.dm_exec_function_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be2d102b81bf5535354b99cc3405d50a0bf94915
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342980"
---
# <a name="sysdm_exec_function_stats-transact-sql"></a>sys.dm_exec_function_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Restituisce statistiche sulle prestazioni aggregate per le funzioni memorizzate nella cache. La vista restituisce una riga per ogni piano di funzioni memorizzato nella cache e la durata della riga è purché la funzione rimanga memorizzata nella cache. Quando una funzione viene rimossa dalla cache, la riga corrispondente viene eliminata da questa visualizzazione. A questo punto, viene generato un evento di traccia SQL di Performance Statistics simile a **sys.dm_exec_query_stats**. Restituisce informazioni sulle funzioni scalari, incluse le funzioni in memoria e le funzioni scalari CLR. Non restituisce informazioni sulle funzioni con valori di tabella e sulle funzioni scalari che sono inline con l' [incorporamento UDF scalare](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  
  
> [!NOTE]
> I risultati di **sys.dm_exec_function_stats**  possono variare a seconda dell'esecuzione, perché i dati riflettono solo le query finite e non quelli ancora in corso. 

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database in cui risiede la funzione.|  
|**object_id**|**int**|Numero di identificazione dell'oggetto della funzione.|  
|**type**|**char(2)**|Tipo dell'oggetto: FN = funzioni a valori scalari|  
|**type_desc**|**nvarchar(60)**|Descrizione del tipo di oggetto: SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary(64)**|Questo può essere usato per la correlazione con le query in **sys.dm_exec_query_stats** eseguite dall'interno di questa funzione.|  
|**plan_handle**|**varbinary(64)**|Identificatore del piano in memoria. Si tratta di un identificatore temporaneo, che rimane costante solo se il piano rimane nella cache. Questo valore può essere utilizzato con la vista a gestione dinamica **sys.dm_exec_cached_plans** .<br /><br /> Sarà sempre 0x000 quando una funzione compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**cached_time**|**datetime**|Ora in cui la funzione è stata aggiunta alla cache.|  
|**last_execution_time**|**datetime**|Ora dell'ultima esecuzione della funzione.|  
|**execution_count**|**bigint**|Numero di esecuzioni della funzione a partire dall'ultima compilazione.|  
|**total_worker_time**|**bigint**|Quantità totale di tempo di CPU, in microsecondi, utilizzato dalle esecuzioni di questa funzione a partire dalla relativa compilazione.<br /><br /> Per le funzioni compilate in modo nativo, **total_worker_time** potrebbe non essere accurata se molte esecuzioni importano meno di un millisecondo.|  
|**last_worker_time**|**bigint**|Tempo di CPU, in microsecondi, utilizzato durante l'ultima esecuzione della funzione. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Tempo minimo di CPU, in microsecondi, utilizzato da questa funzione durante una singola esecuzione. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Tempo massimo di CPU, in microsecondi, utilizzato da questa funzione durante una singola esecuzione. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Numero totale di letture fisiche effettuate dalle esecuzioni di questa funzione a partire dalla relativa compilazione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_physical_reads**|**bigint**|Numero di letture fisiche eseguite durante l'ultima esecuzione della funzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_physical_reads**|**bigint**|Numero minimo di letture fisiche effettuate da questa funzione durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_physical_reads**|**bigint**|Numero massimo di letture fisiche effettuate da questa funzione durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_logical_writes**|**bigint**|Numero totale di scritture logiche effettuate dalle esecuzioni di questa funzione a partire dalla relativa compilazione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_logical_writes**|**bigint**|Numero del numero di pagine del pool di buffer diventate dirty durante l'ultima esecuzione del piano. Se una pagina è già dirty (modificata) le scritture non vengono conteggiate.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_logical_writes**|**bigint**|Numero minimo di scritture logiche eseguite da questa funzione durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_logical_writes**|**bigint**|Numero massimo di scritture logiche eseguite da questa funzione durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_logical_reads**|**bigint**|Numero totale di letture logiche effettuate dalle esecuzioni di questa funzione a partire dalla relativa compilazione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**last_logical_reads**|**bigint**|Numero di letture logiche eseguite durante l'ultima esecuzione della funzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**min_logical_reads**|**bigint**|Numero minimo di letture logiche effettuate da questa funzione durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**max_logical_reads**|**bigint**|Numero massimo di letture logiche effettuate da questa funzione durante una singola esecuzione.<br /><br /> È sempre 0 con esecuzione di query su una tabella ottimizzata per la memoria.|  
|**total_elapsed_time**|**bigint**|Tempo totale trascorso, in microsecondi, per le esecuzioni completate di questa funzione.|  
|**last_elapsed_time**|**bigint**|Tempo trascorso, in microsecondi, per l'esecuzione completata più di recente di questa funzione.|  
|**min_elapsed_time**|**bigint**|Tempo minimo trascorso, in microsecondi, per qualsiasi esecuzione completata della funzione.|  
|**max_elapsed_time**|**bigint**|Tempo massimo trascorso, in microsecondi, per qualsiasi esecuzione completata della funzione.|  
|**total_page_server_reads**|**bigint**|Numero totale di letture di pagine del server eseguite dalle esecuzioni di questa funzione a partire dalla relativa compilazione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure.|  
|**last_page_server_reads**|**bigint**|Numero di letture di pagine del server eseguite durante l'ultima esecuzione della funzione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure.|  
|**min_page_server_reads**|**bigint**|Numero minimo di letture di pagine del server eseguite da questa funzione durante una singola esecuzione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure.|  
|**max_page_server_reads**|**bigint**|Numero massimo di letture di pagine del server eseguite da questa funzione durante una singola esecuzione.<br /><br /> **Si applica a:** Iperscalabilità del database SQL di Azure.|
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sulle prime dieci funzioni identificate in base al tempo medio trascorso.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [sys.dm_exec_procedure_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  
  
