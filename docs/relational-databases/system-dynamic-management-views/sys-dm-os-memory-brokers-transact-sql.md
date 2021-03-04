---
description: sys.dm_os_memory_brokers (Transact-SQL)
title: sys.dm_os_memory_brokers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_brokers
- dm_os_memory_brokers_TSQL
- sys.dm_os_memory_brokers_TSQL
- dm_os_memory_brokers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_brokers dynamic management view
ms.assetid: 48dd6ad9-0d36-4370-8a12-4921d0df4b86
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40f661b1b7f8470f643612e8a1725b33884786cf
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839166"
---
# <a name="sysdm_os_memory_brokers-transact-sql"></a>sys.dm_os_memory_brokers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le allocazioni interne a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzano il gestore della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il rilevamento delle differenze tra i contatori della memoria del processo da **sys.dm_os_process_memory** e i contatori interni può indicare l'utilizzo della memoria da parte dei componenti esterni nello [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spazio di memoria.  
  
 I broker della memoria distribuiscono equamente le allocazioni di memoria tra i vari componenti all'interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in base all'utilizzo corrente e previsto. I broker di memoria non eseguono allocazioni. Le registrano solo per calcolare la distribuzione.  
  
 Nella tabella seguente sono disponibili informazioni sui broker di memoria.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_memory_brokers**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pool_id**|**int**|ID del pool di risorse se associato a un pool di Resource Governor.|  
|**memory_broker_type**|**nvarchar(60)**|Tipo di broker di memoria. Esistono attualmente tre tipi di broker di memoria in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , elencati di seguito con le relative descrizioni.<br /><br /> **MEMORYBROKER_FOR_CACHE** : memoria allocata per l'utilizzo da oggetti memorizzati nella cache (non cache del pool di buffer).<br /><br /> **MEMORYBROKER_FOR_STEAL** : memoria rubata dal pool di buffer. Tale memoria non è disponibile per il riutilizzo da parte di altri componenti fino a che non viene liberata dal proprietario corrente.<br /><br /> **MEMORYBROKER_FOR_RESERVE** : memoria riservata per l'uso futuro da richieste attualmente in esecuzione.|  
|**allocations_kb**|**bigint**|Quantità di memoria, in kilobyte (KB), allocata a questo tipo di broker.|  
|**allocations_kb_per_sec**|**bigint**|Velocità delle allocazioni di memoria in kilobyte (KB) al secondo. Il valore può essere negativo per le deallocazioni di memoria.|  
|**predicted_allocations_kb**|**bigint**|Quantità stimata di memoria allocata dal broker. Si basa sul modello di utilizzo della memoria.|  
|**target_allocations_kb**|**bigint**|Quantità consigliata di memoria allocata, in kilobyte (KB) basata sulle impostazioni correnti e sul modello di utilizzo della memoria. Il broker deve crescere o diminuire fino a questo numero.|  
|**future_allocations_kb**|**bigint**|Numero previsto di allocazioni, in kilobyte (KB), che verranno effettuate nei prossimi secondi.|  
|**overall_limit_kb**|**bigint**|Quantità massima di memoria, espressa in kilobyte (KB), che può essere allocata dal broker.|  
|**last_notification**|**nvarchar(60)**|Indicazione sull'utilizzo della memoria basato sulle impostazioni correnti e sul modello di utilizzo. I valori validi sono i seguenti:<br /><br /> grow<br /><br /> shrink<br /><br /> stabile|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="see-also"></a>Vedere anche  

  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
