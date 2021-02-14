---
description: sys.dm_exec_query_profiles (Transact-SQL)
title: sys.dm_exec_query_profiles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_profiles_TSQL
- sys.dm_exec_query_profiles_TSQL
- dm_exec_query_profiles
- sys.dm_exec_query_profiles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_profiles dynamic management view
ms.assetid: 54efc6cb-eea8-4f6d-a4d0-aa05eeb54081
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ac678e88eaa21a8bb0f074e1bd899108b390bd49
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342967"
---
# <a name="sysdm_exec_query_profiles-transact-sql"></a>sys.dm_exec_query_profiles (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Monitora lo stato di avanzamento delle query in tempo reale mentre la query è in esecuzione. Usare, ad esempio, questa DMV per determinare la parte della query la cui esecuzione è lenta. Creare un join di questa DMV ad altre DMV di sistema utilizzando le colonne identificate nel campo descrizione. In alternativa, creare un join di questa DMV con altri contatori di prestazioni, ad esempio Performance Monitor, xperf, usando le colonne di tipo timestamp.  
  
## <a name="table-returned"></a>Tabella restituita  
I contatori restituiti sono specifici per ogni operatore per ogni thread. I risultati sono dinamici e non corrispondono ai risultati delle opzioni esistenti, ad esempio la `SET STATISTICS XML ON` creazione di output solo al termine della query.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifica la sessione in cui viene eseguita la query. Fa riferimento a dm_exec_sessions.session_id.|  
|request_id|**int**|Identifica la richiesta di destinazione. Fa riferimento a dm_exec_sessions.request_id.|  
|sql_handle|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure di cui fa parte la query. Fa riferimento a dm_exec_query_stats.sql_handle.|  
|plan_handle|**varbinary(64)**|È un token che identifica in modo univoco un piano di esecuzione di query per un batch eseguito e il relativo piano risiede nella cache dei piani oppure è attualmente in esecuzione. Fa riferimento a dm_exec_query_stats. plan_handle.|  
|physical_operator_name|**nvarchar(256)**|Nome dell'operatore fisico.|  
|node_id|**int**|Identifica un nodo operatore nell'albero della query.|  
|thread_id|**int**|Distingue i thread (per una query parallela) che appartengono allo stesso nodo operatore della query.|  
|task_address|**varbinary (8)**|Identifica l'attività SQLOS utilizzata da questo thread. Fa riferimento a dm_os_tasks.task_address.|  
|row_count|**bigint**|Numero di righe restituite finora dall'operatore.|  
|rewind_count|**bigint**|Numero di ripristini finora.|  
|rebind_count|**bigint**|Numero di riassociazioni finora.|  
|end_of_scan_count|**bigint**|Numero di analisi terminate finora.|  
|estimate_row_count|**bigint**|Numero stimato di righe. Può essere utile per confrontare il valore estimated_row_count con il valore row_count effettivo.|  
|first_active_time|**bigint**|Ora, in millisecondi, in cui l'operatore è stato chiamato la prima volta.|  
|last_active_time|**bigint**|Ora, in millisecondi, in cui l'operatore è stato chiamato l'ultima volta.|  
|open_time|**bigint**|Timestamp apertura in millisecondi.|  
|first_row_time|**bigint**|Timestamp in cui è stata aperta la prima riga in millisecondi.|  
|last_row_time|**bigint**|Timestamp in cui è stata aperta l'ultima riga in millisecondi.|  
|close_time|**bigint**|Timestamp chiusura in millisecondi.|  
|elapsed_time_ms|**bigint**|Tempo totale trascorso (in millisecondi) utilizzato dalle operazioni del nodo di destinazione fino a questo punto.|  
|cpu_time_ms|**bigint**|Tempo totale CPU (in millisecondi) utilizzato dalle operazioni del nodo di destinazione fino a questo punto.|  
|database_id|**smallint**|ID del database contenente l'oggetto in cui vengono eseguite le letture e le scritture.|  
|object_id|**int**|Identificatore dell'oggetto in cui vengono eseguite le letture e le scritture. Fa riferimento a sys.objects.object_id.|  
|index_id|**int**|Indice in cui viene aperto il set di righe.|  
|scan_count|**bigint**|Numero di analisi tabella/indice.|  
|logical_read_count|**bigint**|Numero di letture logiche.|  
|physical_read_count|**bigint**|Numero di letture fisiche.|  
|read_ahead_count|**bigint**|Numero di letture anticipate.|  
|write_page_count|**bigint**|Numero di scritture di pagina a causa dello spill.|  
|lob_logical_read_count|**bigint**|Numero di letture logiche LOB.|  
|lob_physical_read_count|**bigint**|Numero di letture fisiche LOB.|  
|lob_read_ahead_count|**bigint**|Numero di letture anticipate LOB.|  
|segment_read_count|**int**|Numero di letture anticipate di segmenti.|  
|segment_skip_count|**int**|Numero di segmenti ignorati finora.| 
|actual_read_row_count|**bigint**|Numero di righe lette da un operatore prima dell'applicazione del predicato residuo.| 
|estimated_read_row_count|**bigint**|**Si applica a:** A partire da [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1. <br/>Numero di righe stimate per la lettura da parte di un operatore prima dell'applicazione del predicato residuo.|  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Se il nodo del piano di query non dispone di I/O, tutti i contatori correlati all'I/O vengono impostati su NULL.  
  
 I contatori correlati all'I/O segnalati da questa DMV sono più granulari di quelli segnalati da `SET STATISTICS IO` nei due modi seguenti:  
  
-   `SET STATISTICS IO` raggruppa i contatori per tutte le I/O in una tabella specificata insieme. Con questa DMV si otterranno contatori distinti per ogni nodo nel piano di query che esegue le operazioni di I/O nella tabella.  
  
-   In caso di analisi parallela, questa DMV restituisce i contatori per ogni thread parallelo usato nell'analisi.
 
A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, l' *infrastruttura di profilatura delle statistiche di esecuzione delle query standard* è affiancata a un'infrastruttura di *profilatura delle statistiche di esecuzione delle query Lightweight*. `SET STATISTICS XML ON` e `SET STATISTICS PROFILE ON` utilizzano sempre l' *infrastruttura di profilatura delle statistiche di esecuzione delle query standard*. Per `sys.dm_exec_query_profiles` essere popolato, è necessario abilitare una delle infrastrutture di profilatura delle query. Per altre informazioni, vedere [Infrastruttura di profilatura query](../../relational-databases/performance/query-profiling-infrastructure.md).    

>[!NOTE]
> La query in fase di analisi deve essere avviata **dopo** l'abilitazione dell'infrastruttura di profilatura delle query, in modo che dopo l'avvio della query non venga generato alcun risultato `sys.dm_exec_query_profiles` . Per altre informazioni su come abilitare le infrastrutture di profilatura delle query, vedere [eseguire query sull'infrastruttura di profilatura](../../relational-databases/performance/query-profiling-infrastructure.md).

## <a name="permissions"></a>Autorizzazioni  
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] e istanza gestita SQL `VIEW DATABASE STATE` di Azure sono necessarie l'autorizzazione e l'appartenenza al `db_owner` ruolo del database.   
Nei [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] livelli Premium, richiede l' `VIEW DATABASE STATE` autorizzazione nel database. In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sugli obiettivi del servizio Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
   
## <a name="examples"></a>Esempio  
 Passaggio 1: accedere a una sessione in cui si prevede di eseguire la query con cui si eseguirà l'analisi `sys.dm_exec_query_profiles` . Per configurare la query per l'utilizzo della profilatura `SET STATISTICS PROFILE ON` . Eseguire la query in questa stessa sessione.  
  
```sql  
--Configure query for profiling with sys.dm_exec_query_profiles  
SET STATISTICS PROFILE ON;  
GO  

--Or enable query profiling globally under SQL Server 2016 SP1 or above (not needed in SQL Server 2019)  
DBCC TRACEON (7412, -1);  
GO 
  
--Next, run your query in this session, or in any other session if query profiling has been enabled globally 
```  
  
 Passaggio 2: accedere a una seconda sessione diversa dalla sessione in cui è in esecuzione la query.  
  
 L'istruzione seguente riepiloga lo stato di avanzamento della query attualmente in esecuzione nella sessione 54. A tale scopo, viene calcolato il numero totale di righe di output restituite da tutti i thread per ogni nodo e confrontato con il numero stimato di righe di output per tale nodo.  
  
```sql  
--Run this in a different session than the session in which your query is running. 
--Note that you may need to change session id 54 below with the session id you want to monitor.
SELECT node_id,physical_operator_name, SUM(row_count) row_count, 
  SUM(estimate_row_count) AS estimate_row_count, 
  CAST(SUM(row_count)*100 AS float)/SUM(estimate_row_count)  
FROM sys.dm_exec_query_profiles   
WHERE session_id=54
GROUP BY node_id,physical_operator_name  
ORDER BY node_id;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
 
