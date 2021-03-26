---
description: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
title: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_query_TSQL
- sys.dm_db_missing_index_group_stats_query
- dm_db_missing_index_group_stats_query_TSQL
- dm_db_missing_index_group_stats_query
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats_query dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats_query dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 01554b6abf8728ff317638f3e5e304b3823890ac
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551544"
---
# <a name="sysdm_db_missing_index_group_stats_query-transact-sql"></a>sys.dm_db_missing_index_group_stats_query (Transact-SQL)
[!INCLUDE [SQL Server 2019 SQL Database](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi.md)]

  Restituisce informazioni sulle query che necessitano di un indice mancante da gruppi di indici mancanti, esclusi gli indici spaziali. È possibile che venga restituita più di una query per ogni gruppo di indici mancanti. Un gruppo di indici mancanti può includere diverse query che hanno richiesto lo stesso indice.
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  
    
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifica un gruppo di indici mancanti. Questo identificatore è univoco a livello di server.<br /><br /> Le altre colonne contengono informazioni su tutte le query per cui l'indice del gruppo viene considerati mancante.<br /><br /> Un gruppo di indici contiene un solo indice.<BR><BR>Può essere aggiunto a `index_group_handle` in [sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md).|  
|**query_hash**|**binario (8)**|Valore hash binario calcolato sulla query che consente di identificare query con logica analoga. È possibile utilizzare il valore hash della query per determinare l'utilizzo delle risorse aggregate per query che differiscono solo per valori letterali.|  
|**query_plan_hash**|**binario (8)**|Valore hash binario calcolato sul piano di esecuzione di query che consente di identificare piani di esecuzioni analoghi. È possibile utilizzare il valore hash del piano di query per individuare il costo cumulativo di query con piani di esecuzione analoghi.<br /><br /> È sempre 0x000 quando una stored procedure compilata in modo nativo esegue una query su una tabella ottimizzata per la memoria.|  
|**last_sql_handle**|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure dell'ultima istruzione compilata necessaria per l'indice.<BR><BR>`last_sql_handle` può essere utilizzato per recuperare il testo SQL della query chiamando la funzione a gestione dinamica [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .|
|**last_statement_start_offset**|**int**|Indica, in byte, a partire da 0, la posizione iniziale della query descritta dalla riga all'interno del testo del batch o dell'oggetto permanente per l'ultima istruzione compilata che ha richiesto questo indice nel batch SQL.|
|**last_statement_end_offset**|**int**|Indica, in byte, a partire da 0, la posizione finale della query descritta dalla riga all'interno del testo del batch o dell'oggetto permanente per l'ultima istruzione compilata che ha richiesto questo indice nel batch SQL.|
|**last_statement_sql_handle**|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure dell'ultima istruzione compilata necessaria per l'indice.<BR><BR>`last_statement_sql_handle`, insieme a `last_statement_start_offset` e `last_statement_end_offset` , può essere utilizzato per recuperare il testo SQL della query chiamando la funzione a gestione dinamica [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .<BR><BR>Se Query Store non è stato abilitato al momento della compilazione della query, restituisce 0.|
|**user_seeks**|**bigint**|Numero di operazioni Seek causate da query utente per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**user_scans**|**bigint**|Numero di analisi causate da query utente per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**last_user_seek**|**datetime**|Data e ora dell'ultima operazione Seek causata da query utente per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**last_user_scan**|**datetime**|Data e ora dell'ultima analisi causata da query utente per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**avg_total_user_cost**|**float**|Costo medio delle query utente che potrebbe essere ridotto dall'indice del gruppo.|  
|**avg_user_impact**|**float**|Vantaggio percentuale medio che potrebbe essere garantito alle query utente con l'implementazione del gruppo di indici mancanti. Questo valore indica la percentuale di riduzione media del costo delle query in caso di implementazione del gruppo di indici mancanti.|  
|**system_seeks**|**bigint**|Numero di operazioni Seek causate da query di sistema, ad esempio query su statistiche automatiche, per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo. Per altre informazioni, vedere [classe di evento auto stats](../../relational-databases/event-classes/auto-stats-event-class.md).|  
|**system_scans**|**bigint**|Numero di analisi causate da query di sistema per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**last_system_seek**|**datetime**|Data e ora dell'ultima operazione Seek di sistema causata da query di sistema per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**last_system_scan**|**datetime**|Data e ora dell'ultima analisi di sistema causata da query di sistema per cui avrebbe potuto essere utilizzato l'indice consigliato del gruppo.|  
|**avg_total_system_cost**|**float**|Costo medio delle query di sistema che potrebbe essere ridotto dall'indice del gruppo.|  
|**avg_system_impact**|**float**|Vantaggio percentuale medio che potrebbe essere garantito alle query di sistema con l'implementazione del gruppo di indici mancanti. Questo valore indica la percentuale di riduzione media del costo delle query in caso di implementazione del gruppo di indici mancanti.|  
  
## <a name="remarks"></a>Commenti  
 Le informazioni restituite da `sys.dm_db_missing_index_group_stats_query` vengono aggiornate a ogni esecuzione della query, non a ogni compilazione o ricompilazione di query. Le statistiche di utilizzo non sono rese permanente e vengono mantenute solo fino al riavvio del motore di database. Per mantenere le statistiche di utilizzo anche dopo il riciclo del server, gli amministratori di database devono eseguire periodicamente copie di backup delle informazioni relative agli indici mancanti. Utilizzare la `sqlserver_start_time` colonna [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) per individuare l'ultima ora di avvio del motore di database.   
 
  >[!NOTE]
  >Il set di risultati per questa DMV è limitato a 600 righe. Ogni riga contiene un indice mancante. Se sono presenti più di 600 indici mancanti, è necessario indirizzare gli indici mancanti esistenti in modo da poter visualizzare quelli più recenti.

## <a name="permissions"></a>Autorizzazioni  
 Per eseguire query su questa vista a gestione dinamica, è necessario che agli utenti sia stata concessa l'autorizzazione VIEW SERVER STATE o qualsiasi autorizzazione che include l'autorizzazione VIEW SERVER STATE.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti viene illustrato come utilizzare la `sys.dm_db_missing_index_group_stats_query` vista a gestione dinamica.  
  
  
### <a name="a-find-the-latest-query-text-for-the-top-10-highest-anticipated-improvements-for-user-queries"></a>R. Trovare il testo più recente della query per i primi 10 miglioramenti massimi previsti per le query utente 
 La query seguente restituisce l'ultimo testo della query registrata per i 10 indici mancanti che produrrebbe il massimo miglioramento cumulativo previsto, in ordine decrescente.  

```sql
SELECT TOP 10 
    SUBSTRING
    (
            sql_text.text,
            misq.last_statement_start_offset / 2 + 1,
            (
            CASE misq.last_statement_Start_offset
                WHEN -1 THEN datalength(sql_text.text)
                ELSE misq.last_statement_end_offset
            END - misq.last_statement_start_offset
            ) / 2 + 1
    ),
    misq.*
FROM sys.dm_db_missing_index_group_stats_query AS misq
CROSS APPLY sys.dm_exec_sql_text(last_sql_handle) AS sql_text
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans) DESC; 
```
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_db_missing_index_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
 [sys.dm_os_sys_info &#40;&#41;Transact-SQL ](sys-dm-os-sys-info-transact-sql.md)