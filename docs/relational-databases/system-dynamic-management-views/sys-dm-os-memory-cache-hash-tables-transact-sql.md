---
description: sys.dm_os_memory_cache_hash_tables (Transact-SQL)
title: sys.dm_os_memory_cache_hash_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_cache_hash_tables_TSQL
- sys.dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_hash_tables dynamic management view
ms.assetid: 68b94f35-8f80-4d2b-bcde-7a21934219af
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: abd71436278bc5580f9242cdc54f3f68719bd91a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839170"
---
# <a name="sysdm_os_memory_cache_hash_tables-transact-sql"></a>sys.dm_os_memory_cache_hash_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni cache attiva nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_cache_hash_tables**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary (8)**|Indirizzo (chiave primaria) della voce nella cache. Non ammette i valori Null.|  
|**nome**|**nvarchar(256)**|Nome della cache. Non ammette i valori Null.|  
|**type**|**nvarchar(60)**|Tipo di cache. Non ammette i valori Null.|  
|**table_level**|**int**|Numero di tabella hash. Una cache specifica può avere più tabelle hash corrispondenti a funzioni hash diverse. Non ammette i valori Null.|  
|**buckets_count**|**int**|Numero di bucket nella tabella hash. Non ammette i valori Null.|  
|**buckets_in_use_count**|**int**|Numero di bucket in uso. Non ammette i valori Null.|  
|**buckets_min_length**|**int**|Numero minimo di voci nella cache in un bucket. Non ammette i valori Null.|  
|**buckets_max_length**|**int**|Numero massimo di voci nella cache in un bucket. Non ammette i valori Null.|  
|**buckets_avg_length**|**int**|Numero medio di voci nella cache in ogni bucket. Non ammette i valori Null.|  
|**buckets_max_length_ever**|**int**|Numero massimo di voci nella cache in un hash bucket per la tabella hash corrente dall'avvio del server. Non ammette i valori Null.|  
|**hits_count**|**bigint**|Numero di riscontri nella cache. Non ammette i valori Null.|  
|**misses_count**|**bigint**|Numero di mancati riscontri nella cache. Non ammette i valori Null.|  
|**buckets_avg_scan_hit_length**|**int**|Numero medio di voci controllate in un bucket prima che venga individuato un elemento ricercato. Non ammette i valori Null.|  
|**buckets_avg_scan_miss_length**|**int**|Numero medio di voci controllate in un bucket prima che la ricerca venga conclusa con esito negativo. Non ammette i valori Null.|  
|**pdw_node_id**|**int**|Identificatore del nodo su cui si trova questa distribuzione.<br /><br /> **Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>Autorizzazioni 

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
 
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
