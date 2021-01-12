---
description: sys.dm_os_dispatcher_pools (Transact-SQL)
title: sys.dm_os_dispatcher_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_dispatcher_pools_TSQL
- dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], views
- sys.dm_os_dispatcher_pools DMV
ms.assetid: b9edbc83-c6bc-4753-9bb5-a454cfe7d6bf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 04dbc2fcedb889b54d8a95fa5357b23857cd062a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095120"
---
# <a name="sysdm_os_dispatcher_pools-transact-sql"></a>sys.dm_os_dispatcher_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni sui pool di dispatcher di sessione. I pool di dispatcher sono pool di thread utilizzati dai componenti del sistema per eseguire elaborazioni di fondo.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_dispatcher_pools**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|dispatcher_pool_address|**varbinary (8)**|Indirizzo del pool di dispatcher. dispatcher_pool_address è univoco. Non ammette i valori Null.|  
|type|**nvarchar(256)**|Tipo del pool di dispatcher. Non ammette i valori Null. Esistono due tipi di pool di dispatcher:<br /><br /> DISP_POOL_XE_ENGINE<br /><br /> DISP_POOL_XE_SESSION<br /><br /> Eseguire una query sulla DMV per l'elenco completo|  
|name|**nvarchar(256)**|Nome del pool di dispatcher. Non ammette i valori Null.|  
|dispatcher_count|**int**|Numero di thread del dispatcher attivi. Non ammette i valori Null.|  
|dispatcher_ideal_count|**int**|Numero di thread del dispatcher che il pool di dispatcher può iniziare a utilizzare. Non ammette i valori Null.|  
|dispatcher_timeout_ms|**int**|Il tempo, espresso in millisecondi, durante il quale un dispatcher dovrà attendere per un nuovo lavoro prima di uscire. Non ammette i valori Null.|  
|dispatcher_waiting_count|**int**|Numero di thread del dispatcher non attivi. Non ammette i valori Null.|  
|queue_length|**int**|Numero di elementi di lavoro che aspettano di essere gestiti dal pool del dispatcher. Non ammette i valori Null.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
  
  


