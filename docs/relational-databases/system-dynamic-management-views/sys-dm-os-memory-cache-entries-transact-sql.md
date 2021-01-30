---
description: sys.dm_os_memory_cache_entries (Transact-SQL)
title: sys.dm_os_memory_cache_entries (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4a274e7bc724cd838373b465029a3a02c4dfc645
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184831"
---
# <a name="sysdm_os_memory_cache_entries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vengono restituite informazioni su tutte le voci nelle cache di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilizzare questa vista per tracciare le informazioni relative alle voci della cache e agli oggetti associati. È anche possibile utilizzare la vista per ottenere le statistiche relative alle voci di cache.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_cache_entries**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary (8)**|Indirizzo della cache. Non ammette i valori Null.|  
|**nome**|**nvarchar(256)**|Nome della cache. Non ammette i valori Null.|  
|**type**|**varchar(60)**|Tipo di cache. Non ammette i valori Null.|  
|**entry_address**|**varbinary (8)**|Indirizzo del descrittore della voce di cache. Non ammette i valori Null.|  
|**entry_data_address**|**varbinary (8)**|Indirizzo dei dati utente nella voce di cache.<br /><br /> 0x00000000 = Indirizzo dei dati della voce non disponibile.<br /><br /> Non ammette i valori Null.|  
|**in_use_count**|**int**|Numero di utenti simultanei di questa voce di cache. Non ammette i valori Null.|  
|**is_dirty**|**bit**|Indica se questa voce di cache è contrassegnata per la rimozione. 1 = Contrassegnata per la rimozione. Non ammette i valori Null.|  
|**disk_ios_count**|**int**|Numero di I/O verificatisi durante la creazione di questa voce. Non ammette i valori Null.|  
|**context_switches_count**|**int**|Numero di cambi di contesto verificatisi durante la creazione di questa voce. Non ammette i valori Null.|  
|**original_cost**|**int**|Costo originale della voce. Questo valore è un'approssimazione del numero di I/O, del costo di istruzioni per la CPU e della quantità di memoria utilizzata dalla voce. Maggiore il costo, minori le possibilità che l'elemento venga rimosso dalla cache. Non ammette i valori Null.|  
|**current_cost**|**int**|Costo corrente della voce di cache. Questo valore viene aggiornato durante il processo di eliminazione delle voci. Il costo corrente viene reimpostato sul valore originale al riutilizzo della voce. Non ammette i valori Null.|  
|**memory_object_address**|**varbinary (8)**|Indirizzo dell'oggetto di memoria associato. Ammette i valori Null.|  
|**pages_allocated_count**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Numero di pagine da 8 KB per l'archiviazione della voce di cache. Non ammette i valori Null.|  
|**pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Quantità di memoria in kilobyte (KB) utilizzata da questa voce di cache.  Non ammette i valori Null.|  
|**entry_data**|**nvarchar(2048)**|Rappresentazione serializzata della voce archiviata nella cache. Queste informazioni dipendono dall'archiviazione nella cache. Ammette i valori Null.|  
|**pool_id**|**int**|**Si applica a**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] e versioni successive.<br /><br /> ID del pool di risorse associato alla voce. Ammette i valori Null.<br /><br /> non katmai|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni 

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
 
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


