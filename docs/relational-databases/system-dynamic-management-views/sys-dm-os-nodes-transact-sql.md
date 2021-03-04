---
description: sys.dm_os_nodes (Transact-SQL)
title: sys.dm_os_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_nodes
- dm_os_nodes_TSQL
- dm_os_nodes
- sys.dm_os_nodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_nodes dynamic management view
ms.assetid: c768b67c-82a4-47f5-850b-0ea282358d50
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef649af50f58218f9be06dc9b0e65e3b744d1b01
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839266"
---
# <a name="sysdm_os_nodes-transact-sql"></a>sys.dm_os_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Un componente interno denominato SQLOS crea le strutture di nodi che imitano la località del processore hardware. Queste strutture possono essere modificate usando [Soft-NUMA](../../database-engine/configure-windows/soft-numa-sql-server.md) per creare layout dei nodi personalizzati.  

> [!NOTE]
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilizzerà automaticamente soft-NUMA per determinate configurazioni hardware. Per altre informazioni, vedere [Soft-NUMA automatico](../../database-engine/configure-windows/soft-numa-sql-server.md#automatic-soft-numa).
  
Nella tabella seguente sono incluse informazioni su questi nodi.  
  
> [!NOTE]
> Per chiamare questa DMV da [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_nodes**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|node_id|**smallint**|ID del nodo.|  
|node_state_desc|**nvarchar(256)**|Descrizione dello stato del nodo. I valori sono visualizzati con i valori reciprocamente esclusivi all'inizio, seguiti dai valori combinabili. Ad esempio:<br /> Online, Thread Resources Low, Lazy Preemptive<br /><br />Sono disponibili quattro valori di node_state_desc che si escludono a vicenda. Sono elencate di seguito con le relative descrizioni.<br /><ul><li>ONLINE: il nodo è online<li>OFFLINE: il nodo è offline<li>IDLE: il nodo non ha richieste di lavoro in sospeso ed è entrato in uno stato inattivo.<li>IDLE_READY: il nodo non ha richieste di lavoro in sospeso ed è pronto per entrare in uno stato inattivo.</li></ul><br />Sono disponibili tre valori node_state_desc combinabili, elencati di seguito con le relative descrizioni.<br /><ul><li>DAC: questo nodo è riservato per la [connessione amministrativa dedicata](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).<li>THREAD_RESOURCES_LOW: non è possibile creare nuovi thread in questo nodo a causa di una condizione di memoria insufficiente.<li>AGGIUNTA a caldo: indica che i nodi sono stati aggiunti in risposta a un evento di aggiunta di CPU a caldo.</li></ul>|  
|memory_object_address|**varbinary (8)**|Indirizzo dell'oggetto memoria associato al nodo. Una relazione uno-a-uno con [sys.dm_os_memory_objects](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).memory_object_address.|  
|memory_clerk_address|**varbinary (8)**|Indirizzo del clerk di memoria associato al nodo. Una relazione uno-a-uno con [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).memory_clerk_address.|  
|io_completion_worker_address|**varbinary (8)**|Indirizzo del thread di lavoro assegnato al completamento I/O per il nodo. Una relazione uno-a-uno con [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).worker_address.|  
|memory_node_id|**smallint**|ID del nodo di memoria al quale questo nodo appartiene. Relazione molti-a-uno con [sys.dm_os_memory_nodes](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md).memory_node_id.|  
|cpu_affinity_mask|**bigint**|Bitmap che identifica le CPU alle quali questo nodo è associato.|  
|online_scheduler_count|**smallint**|Numero di utilità di pianificazione in linea gestite da questo nodo.|  
|idle_scheduler_count|**smallint**|Numero di utilità di pianificazione online che non dispongono di thread di lavoro attivi.|  
|active_worker_count|**int**|Numero di thread di lavoro attivi su tutte le utilità di pianificazione gestite da questo nodo.|  
|avg_load_balance|**int**|Media del numero di attività per utilità di pianificazione su questo nodo.|  
|timer_task_affinity_mask|**bigint**|Bitmap che identifica le utilità di pianificazione che possono avere attività di timer assegnate.|  
|permanent_task_affinity_mask|**bigint**|Bitmap che identifica le utilità di pianificazione che possono avere attività permanenti assegnate.|  
|resource_monitor_state|**bit**|A ogni nodo viene assegnato un monitoraggio risorse. Il monitoraggio risorse può essere in esecuzione o inattivo. Il valore 1 indica che è in esecuzione, il valore 0 indica che è inattivo.|  
|online_scheduler_mask|**bigint**|Identifica la maschera di affinità del processo per questo nodo.|  
|processor_group|**smallint**|Identifica il gruppo di processori per questo nodo.|  
|cpu_count |**int** |Numero di CPU disponibili per questo nodo. |
|pdw_node_id|**int**|Identificatore del nodo su cui si trova questa distribuzione.<br /><br /> **Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche    
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
