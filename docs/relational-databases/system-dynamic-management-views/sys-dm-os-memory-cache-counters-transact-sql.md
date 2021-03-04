---
description: sys.dm_os_memory_cache_counters (Transact-SQL)
title: sys.dm_os_memory_cache_counters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_cache_counters_TSQL
- dm_os_memory_cache_counters_TSQL
- sys.dm_os_memory_cache_counters
- dm_os_memory_cache_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_counters dynamic management view
ms.assetid: ca7bd036-d661-4c17-b00a-e1a975bd8932
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a61d9f9802c537b49393a04be3d608c497a4c824
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839142"
---
# <a name="sysdm_os_memory_cache_counters-transact-sql"></a>sys.dm_os_memory_cache_counters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene restituito uno snapshot dello stato di una cache in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **sys.dm_os_memory_cache_counters** fornisce informazioni di run-time sulle voci di cache allocate, sul relativo utilizzo e sull'origine di memoria per le voci della cache.  
  
> **Nota:** Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_cache_counters**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary (8)**|Indica l'indirizzo (chiave primaria) dei contatori associati a una cache specifica. Non ammette i valori Null.|  
|**nome**|**nvarchar(256)**|Specifica il nome della cache. Non ammette i valori Null.|  
|**type**|**nvarchar(60)**|Indica il tipo di cache associato a questa voce. Non ammette i valori Null.|  
|**single_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità, in kilobyte, della memoria a pagina singola allocata. Corrisponde alla quantità di memoria allocata tramite l'allocatore di pagine singole. Si riferisce alla pagine di 8 KB prelevate direttamente dal pool di buffer per questa cache. Non ammette i valori Null.|  
|**pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità di memoria, in kilobyte, allocata nella cache. Non ammette i valori Null.|  
|**multi_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità, in kilobyte, della memoria a più pagine allocata. Corrisponde alla quantità di memoria allocata tramite l'allocatore di pagine multiple del nodo di memoria. Questa memoria viene allocata all'esterno del pool di buffer e utilizza l'allocatore virtuale dei nodi di memoria. Non ammette i valori Null.|  
|**pages_in_use_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità di memoria, in kilobyte, allocata e in uso nella cache. Ammette i valori Null.  I valori per gli oggetti di tipo `USERSTORE_<*>` non vengono rilevati.  Viene riportato NULL.|  
|**single_pages_in_use_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità, in kilobyte, della memoria a pagina singola utilizzata. Ammette i valori Null. Queste informazioni non vengono rilevate per gli oggetti di tipo USERSTORE_ \<*> e questi valori saranno null.|  
|**multi_pages_in_use_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità, in kilobyte, della memoria a più pagine utilizzata. Ammette valori Null. Queste informazioni non vengono rilevate per gli oggetti di tipo USERSTORE_ \<*> e questi valori saranno null.|  
|**entries_count**|**bigint**|Indica il numero di voci nella cache. Non ammette i valori Null.|  
|**entries_in_use_count**|**bigint**|Indica il numero di voci della cache utilizzate. Non ammette i valori Null.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni 

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
