---
description: sys.dm_os_memory_pools (Transact-SQL)
title: sys.dm_os_memory_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_pools_TSQL
- dm_os_memory_pools
- dm_os_memory_pools_TSQL
- sys.dm_os_memory_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_pools dynamic management view
ms.assetid: 1ef053f3-c6f3-456e-82b6-26e4bd630d46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 738a1d9c12d51c0d16900630dc4006afaef19393
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184810"
---
# <a name="sysdm_os_memory_pools-transact-sql"></a>sys.dm_os_memory_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni archivio di oggetti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile utilizzare questa vista per monitorare l'utilizzo della memoria cache e per identificare l'errato funzionamento della memorizzazione nella cache.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_pools**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**memory_pool_address**|**varbinary (8)**|Indirizzo di memoria della voce che rappresenta il pool di memoria. Non ammette i valori Null.|  
|**pool_id**|**int**|ID di un pool specifico all'interno di un set di pool. Non ammette i valori Null.|  
|**type**|**nvarchar(60)**|Tipo di pool di oggetti. Non ammette i valori Null. Per ulteriori informazioni, vedere [sys.dm_os_memory_clerks &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**nome**|**nvarchar(256)**|Nome assegnato dal sistema dell'oggetto memoria. Non ammette i valori Null.|  
|**max_free_entries_count**|**bigint**|Numero massimo di voci libere che un pool può avere. Non ammette i valori Null.|  
|**free_entries_count**|**bigint**|Numero di voci libere incluse nel pool. Non ammette i valori Null.|  
|**removed_in_all_rounds_count**|**bigint**|Numero di voci rimosse dal pool dall'avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori Null.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="remarks"></a>Commenti  
 I componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] talvolta utilizzano una struttura comune di pool per memorizzare nella cache tipi di dati omogenei e senza informazioni sullo stato. La struttura di pool è più semplice della struttura di cache. Tutte le voci nei pool sono considerate uguali. Internamente i pool sono clerk di memoria e possono essere utilizzati nelle stesse posizioni in cui vengono utilizzati i clerk di memoria.  
  
## <a name="see-also"></a>Vedere anche  
 
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


