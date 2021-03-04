---
description: sys.dm_os_memory_nodes (Transact-SQL)
title: sys.dm_os_memory_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_nodes_TSQL
- sys.dm_os_memory_nodes
- sys.dm_os_memory_nodes_TSQL
- dm_os_memory_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_nodes dynamic management view
ms.assetid: bf4032fe-7db1-40e9-a62e-d69cebff4b44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7bec1e868a8531c8a007b94c69a925a05711bde2
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836838"
---
# <a name="sysdm_os_memory_nodes-transact-sql"></a>sys.dm_os_memory_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le allocazioni interne a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzano il gestore della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il rilevamento delle differenze tra i contatori della memoria del processo da **sys.dm_os_process_memory** e i contatori interni può indicare l'utilizzo della memoria da parte dei componenti esterni nello [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spazio di memoria.  
  
 I nodi vengono creati per nodi di memoria NUMA fisici. Potrebbero essere diversi dai nodi CPU in **sys.dm_os_nodes**.  
  
 Nessuna delle allocazioni eseguite direttamente tramite le routine di allocazione di memoria di Windows viene registrata. Nella tabella seguente sono fornite informazioni sulle allocazioni di memoria eseguite solo utilizzando interfacce dello strumento di gestione della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_nodes**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**memory_node_id**|**smallint**|Specifica l'ID del nodo di memoria. Correlato all' **memory_node_id** di **sys.dm_os_memory_clerks**. Non ammette i valori NULL.|  
|**virtual_address_space_reserved_kb**|**bigint**|Indica il numero di indirizzi virtuali riservati, in kilobyte (KB), di cui non è stato eseguito il commit né il mapping a pagine fisiche. Non ammette i valori NULL.|  
|**virtual_address_space_committed_kb**|**bigint**|Specifica la quantità di indirizzo virtuale, in KB, di cui è stato eseguito il commit o il mapping a pagine fisiche. Non ammette i valori NULL.|  
|**locked_page_allocations_kb**|**bigint**|Specifica la quantità di memoria fisica, in KB, bloccata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori NULL.|  
|**single_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità di memoria riservata, in KB, allocata utilizzando l'allocatore di pagine singole da thread in esecuzione sul nodo. Questa memoria è allocata dal pool di buffer. Questo valore indica il nodo in cui si è verificata la richiesta di allocazione, non la posizione fisica in cui la richiesta di allocazione è stata soddisfatta.|  
|**pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Specifica la quantità di memoria di cui è stato eseguito il commit, in KB, allocata da questo nodo NUMA dall'allocatore di pagine del gestore della memoria. Non ammette i valori NULL.|  
|**multi_pages_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tramite [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantità di memoria riservata, in KB, allocata utilizzando l'allocatore di più pagine da thread in esecuzione sul nodo. Questa memoria è esterna al pool di buffer. Questo valore indica il nodo in cui si sono verificate le richieste di allocazione, non la posizione fisica in cui le richieste di allocazione sono state soddisfatte.|  
|**shared_memory_reserved_kb**|**bigint**|Specifica la quantità di memoria condivisa del nodo, in KB, che è stata riservata. Non ammette i valori NULL.|  
|**shared_memory_committed_kb**|**bigint**|Specifica la quantità di memoria condivisa del nodo, in KB, di cui è stato eseguito il commit. Non ammette i valori NULL.|  
|**cpu_affinity_mask**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Solo per uso interno. Non ammette i valori NULL.|  
|**online_scheduler_mask**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Solo per uso interno. Non ammette i valori NULL.|  
|**processor_group**|**smallint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Solo per uso interno. Non ammette i valori NULL.|  
|**foreign_committed_kb**|**bigint**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Viene specificata la quantità totale di memoria di cui è stato eseguito il commit, in KB, da altri nodi di memoria. Non ammette i valori NULL.|  
|**target_kb** |**bigint** |**Si applica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Specifica l'obiettivo della memoria per il nodo di memoria, in KB. |   
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
