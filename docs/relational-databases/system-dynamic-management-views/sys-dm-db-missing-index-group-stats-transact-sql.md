---
description: sys.dm_db_missing_index_group_stats (Transact-SQL)
title: sys.dm_db_missing_index_group_stats (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_TSQL
- sys.dm_db_missing_index_group_stats
- dm_db_missing_index_group_stats_TSQL
- dm_db_missing_index_group_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d2271f7b1b90f34d25370ed156348892b0a4ddb5
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551493"
---
# <a name="sysdm_db_missing_index_group_stats-transact-sql"></a>sys.dm_db_missing_index_group_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni di riepilogo su gruppi di indici mancanti, escludendo gli indici spaziali.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le viste a gestione dinamica non possono esporre le informazioni che influenzerebbero l'indipendenza del database o le informazioni sugli altri database a cui l'utente dispone di accesso. Per evitare di esporre queste informazioni, ogni riga che contiene dati che non appartengono al tenant connesso viene filtrata.  
    
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifica un gruppo di indici mancanti. Questo identificatore è univoco a livello di server.<br /><br /> Le altre colonne contengono informazioni su tutte le query per cui l'indice del gruppo viene considerati mancante.<br /><br /> Un gruppo di indici contiene un solo indice.<BR><BR>Può essere aggiunto a `index_group_handle` in [sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md).|  
|**unique_compiles**|**bigint**|Numero di compilazioni e ricompilazioni per cui può essere utile questo gruppo di indici mancanti. Il valore di questa colonna può essere determinato dalle compilazioni e ricompilazioni di numerose query diverse.|  
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
 Le informazioni restituite da `sys.dm_db_missing_index_group_stats` vengono aggiornate a ogni esecuzione della query, non a ogni compilazione o ricompilazione di query. Le statistiche di utilizzo non sono rese permanente e vengono mantenute solo fino al riavvio del motore di database. Per mantenere le statistiche di utilizzo anche dopo il riciclo del server, gli amministratori di database devono eseguire periodicamente copie di backup delle informazioni relative agli indici mancanti. Utilizzare la `sqlserver_start_time` colonna [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) per individuare l'ultima ora di avvio del motore di database.   

  >[!NOTE]
  >Il set di risultati per questa DMV è limitato a 600 righe. Ogni riga contiene un indice mancante. Se sono presenti più di 600 indici mancanti, è necessario indirizzare gli indici mancanti esistenti in modo da poter visualizzare quelli più recenti.

 Un gruppo di indici mancanti può includere diverse query che hanno richiesto lo stesso indice. Per ulteriori informazioni sulle singole query che necessitano di un indice specifico in questa DMV, vedere [sys.dm_db_missing_index_group_stats_query](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md).
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire query su questa vista a gestione dinamica, è necessario che agli utenti sia stata concessa l'autorizzazione VIEW SERVER STATE o qualsiasi autorizzazione che include l'autorizzazione VIEW SERVER STATE.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti viene illustrato come utilizzare la `sys.dm_db_missing_index_group_stats` vista a gestione dinamica.  
  
### <a name="a-find-the-10-missing-indexes-with-the-highest-anticipated-improvement-for-user-queries"></a>R. Trovare i 10 indici mancanti con il massimo miglioramento previsto per le query utente  
 La query seguente determina i 10 indici mancanti in grado di determinare il massimo miglioramento cumulativo previsto, in ordine decrescente, per le query utente.  
  
```sql
SELECT TOP 10 *  
FROM sys.dm_db_missing_index_group_stats  
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans)DESC;  
```  
  
### <a name="b-find-the-individual-missing-indexes-and-their-column-details-for-a-particular-missing-index-group"></a>B. Trovare i singoli indici mancanti e i relativi dettagli delle colonne per un determinato gruppo di indici mancanti  
 La query seguente identifica gli indici mancanti che costituiscono un determinato gruppo di indici mancanti e ne visualizza i dettagli delle colonne. A scopo di questo esempio, l'indice mancante `group_handle` è 24.  
  
```sql
SELECT migs.group_handle, mid.*  
FROM sys.dm_db_missing_index_group_stats AS migs  
INNER JOIN sys.dm_db_missing_index_groups AS mig  
    ON (migs.group_handle = mig.index_group_handle)  
INNER JOIN sys.dm_db_missing_index_details AS mid  
    ON (mig.index_handle = mid.index_handle)  
WHERE migs.group_handle = 24;  
```  
  
 Questa query indica il nome del database, dello schema e della tabella in cui un indice risulta mancante, nonché i nomi delle colonne da utilizzare per la chiave di indice. Quando si scrive l'istruzione DDL CREATE INDEX per implementare indici mancanti, nella \<*table_name*> clausola on dell'istruzione create index elencare innanzitutto le colonne di uguaglianza e quindi le colonne di disuguaglianza. Le colonne incluse devono essere elencate nella clausola INCLUDE dell'istruzione CREATE INDEX. Per determinare un ordine efficiente per le colonne di uguaglianza, ordinarle in base alla selettività a partire dalle colonne più selettive, all'estrema sinistra nell'elenco di colonne.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_db_missing_index_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats_query &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
 [sys.dm_os_sys_info &#40;&#41;Transact-SQL ](sys-dm-os-sys-info-transact-sql.md)  
  
