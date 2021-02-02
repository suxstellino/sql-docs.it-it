---
description: sys.dm_exec_session_wait_stats (Transact-SQL)
title: sys.dm_exec_session_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_session_wait_stats
- sys.dm_exec_session_wait_stats_tsql
- dm_exec_session_wait_stats
- dm_exec_session_wait_stats_tsql
helpviewer_keywords:
- sys.dm_exec_session_wait_stats
ms.assetid: df84842a-71eb-4fda-b448-5953cf9985dc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f909ebd3f604fc8c56b08cadc1e70915da061de7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236886"
---
# <a name="sysdm_exec_session_wait_stats-transact-sql"></a>sys.dm_exec_session_wait_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce informazioni su tutte le attese rilevate dai thread eseguiti per ogni sessione. È possibile utilizzare questa visualizzazione per diagnosticare problemi di prestazioni con la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sessione e anche con query e batch specifici.  Questa vista restituisce le stesse informazioni aggregate per [sys.dm_os_wait_stats &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) , ma fornisce anche il numero **session_id** .  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|ID della sessione.|  
|wait_type|**nvarchar(60)**|Nome del tipo di attesa. Per altre informazioni, vedere [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|waiting_tasks_count|**bigint**|Numero di attese del tipo specificato. Questo contatore viene incrementato all'inizio di ogni attesa.|  
|wait_time_ms|**bigint**|Tempo di attesa totale, espresso in millisecondi, per il tipo di attesa specifico. Il tempo comprende signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Tempo di attesa massimo per il tipo di attesa specifico.|  
|signal_wait_time_ms|**bigint**|Differenza tra il momento in cui è stato rilevato il thread in attesa e quello in cui è stata avviata l'esecuzione del thread.|  
  
## <a name="remarks"></a>Commenti  
 Questa DMV Reimposta le informazioni per una sessione quando la sessione viene aperta o quando la sessione viene reimpostata (se il pool di connessioni)  
  
 Per informazioni sui tipi di attesa, vedere [sys.dm_os_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Se l'utente dispone dell'autorizzazione **View Server state** per il server, l'utente visualizzerà tutte le sessioni in esecuzione nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . in caso contrario, l'utente visualizzerà solo la sessione corrente.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)  
 
