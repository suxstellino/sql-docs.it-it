---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
ms.assetid: 1d556c67-5c12-46d5-aa8c-7ec1bb858df7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2d6278f03b3bb49c4083c90c0967b5ed6ac8907c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190192"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il set di tutti i clerk di memoria attivi nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_clerks**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary (8)**|Specifica l'indirizzo di memoria univoco del clerk di memoria. Si tratta della colonna chiave primaria. Non ammette i valori Null.|  
|**type**|**nvarchar(60)**|Specifica il tipo di clerk di memoria. Ogni clerk è associato a un tipo specifico, ad esempio i clerk CLR MEMORYCLERK_SQLCLR. Non ammette i valori Null.|  
|**nome**|**nvarchar(256)**|Specifica il nome assegnato internamente del clerk di memoria. Un componente può disporre di più clerk di memoria di un tipo specifico. Un componente può scegliere di utilizzare nomi specifici per identificare i clerk di memoria dello stesso tipo. Non ammette i valori Null.|  
|**memory_node_id**|**smallint**|Specifica l'ID del nodo di memoria. Non ammette i valori NULL.|  
|**single_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].|  
|**pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità di memoria in kilobyte (KB) allocata in pagine per questo clerk di memoria. Non ammette i valori Null.|  
|**multi_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità di memoria di più pagine allocata, espressa in KB. Si tratta della quantità di memoria allocata tramite l'utilizzo dell'allocatore di più pagine dei nodi di memoria. Questa memoria viene allocata all'esterno del pool di buffer e utilizza l'allocatore virtuale dei nodi di memoria. Non ammette i valori Null.|  
|**virtual_memory_reserved_kb**|**bigint**|Specifica la quantità di memoria virtuale riservata da un clerk di memoria. Non ammette i valori Null.|  
|**virtual_memory_committed_kb**|**bigint**|Specifica la quantità di memoria virtuale di cui è stato eseguito il commit da un clerk di memoria. La quantità di memoria di cui è stato eseguito il commit deve sempre essere minore della quantità di memoria riservata. Non ammette i valori Null.|  
|**awe_allocated_kb**|**bigint**|Specifica la quantità di memoria in kilobyte (KB) bloccata nella memoria fisica di cui non è stato eseguito il page out dal sistema operativo. Non ammette i valori Null.|  
|**shared_memory_reserved_kb**|**bigint**|Specifica la quantità di memoria condivisa riservata da un clerk di memoria, ovvero la quantità di memoria riservata per l'utilizzo da parte della memoria condivisa e del mapping di file. Non ammette i valori Null.|  
|**shared_memory_committed_kb**|**bigint**|Specifica la quantità di memoria condivisa di cui il clerk di memoria ha eseguito il commit. Non ammette i valori Null.|  
|**page_size_in_bytes**|**bigint**|Specifica la granularità dell'allocazione di pagina per questo clerk di memoria. Non ammette i valori Null.|  
|**page_allocator_address**|**varbinary (8)**|Specifica l'indirizzo dell'allocatore di pagine. Questo indirizzo è univoco per un clerk di memoria e può essere usato in **sys.dm_os_memory_objects** per individuare gli oggetti di memoria associati a questo Clerk. Non ammette i valori Null.|  
|**host_address**|**varbinary (8)**|Specifica l'indirizzo di memoria dell'host per il clerk di memoria. Per ulteriori informazioni, vedere [sys.dm_os_hosts &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). I componenti di, ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] native client, accedono alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risorse di memoria tramite l'interfaccia host.<br /><br /> 0x00000000 = Il clerk di memoria appartiene a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Non ammette i valori Null.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni 

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="remarks"></a>Commenti  
 Il gestore della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è strutturato in una gerarchia a tre livelli. I nodi di memoria occupano la parte inferiore della gerarchia. Il livello intermedio è occupato da clerk di memoria, cache in memoria e pool di memoria. Il primo livello è costituito dagli oggetti memoria. Questi oggetti vengono in genere utilizzati per allocare memoria in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 I nodi di memoria rendono disponibili l'interfaccia e l'implementazione per gli allocatori di livello inferiore. All'interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], solo i clerk di memoria hanno accesso ai nodi di memoria. I clerk di memoria accedono alle interfacce dei nodi di memoria per allocare memoria. I nodi di memoria tengono inoltre traccia della memoria allocata tramite l'utilizzo del clerk per la diagnostica. Ogni componente che alloca una quantità significativa di memoria deve creare un proprio clerk di memoria e allocare tutta la relativa memoria tramite l'utilizzo delle interfacce del clerk. I componenti creano spesso i clerk corrispondenti all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  

 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
  


