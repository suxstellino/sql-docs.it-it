---
description: sys.dm_os_child_instances (Transact-SQL)
title: sys.dm_os_child_instances (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_child_instances
- sys.dm_os_child_instances_TSQL
- dm_os_child_instances
- dm_os_child_instances_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server state information [SQL Server]
- sys.dm_os_child_instances dynamic management view
- monitoring server health
ms.assetid: 1bef3074-0ccc-48fa-8f3d-14f3d99df86b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b46f17c349015fee41f91ddb64fc6773ca8c5ee
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094059"
---
# <a name="sysdm_os_child_instances-transact-sql"></a>sys.dm_os_child_instances (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni istanza utente creata dall'istanza del server padre.  
  
> **IMPORTANTE** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Le informazioni restituite da **sys.dm_os_child_instances** possono essere utilizzate per determinare lo stato di ogni istanza utente (heart_beat) e per ottenere il nome della pipe (instance_pipe_name) che può essere utilizzato per creare una connessione all'istanza utente utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o sqlcmd. È possibile connettersi a un'istanza utente solo dopo che è stata avviata da un processo esterno, ad esempio un'applicazione client. Gli strumenti di gestione di SQL non possono avviare un'istanza utente.  
  
> **Nota:** Le istanze utente sono una funzionalità di [!INCLUDE[ssExpressEd11](../../includes/ssexpressed11-md.md)] .  
> 
> **Nota** Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_child_instances**.  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|**owning_principal_name**|**nvarchar(256)**|Nome dell'utente per cui è stata creata l'istanza utente.|  
|owning_principal_sid|nvarchar(256)|ID di sicurezza (SID) dell'entità proprietaria dell'istanza utente. Questo valore corrisponde al SID di Windows.|  
|owning_principal_sid_binary|varbinary(85)|Versione binaria del SID dell'utente proprietario dell'istanza utente|  
|**instance_name**|**nvarchar(128)**|Nome dell'istanza utente.|  
|**instance_pipe_name**|**nvarchar(260)**|Quando si crea un'istanza utente viene creata anche una named pipe a cui possono connettersi le applicazioni. Questo nome può essere utilizzato in una stringa di connessione per connettersi all'istanza utente.|  
|**os_process_id**|**Int**|ID del processo di Windows per l'istanza utente.|  
|**os_process_creation_date**|**DateTime**|Data e ora dell'ultimo avvio di questo processo dell'istanza utente.|  
|**heart_beat**|**nvarchar (5)**|Stato corrente dell'istanza utente, ovvero ALIVE o DEAD.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="remarks"></a>Osservazioni  
 Per ulteriori informazioni sulla vista a gestione dinamica, vedere [funzioni e viste a gestione dinamica &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione online di.  
  
## <a name="see-also"></a>Vedere anche  
 [Istanze utente per non amministratori](/previous-versions/sql/)  
  
