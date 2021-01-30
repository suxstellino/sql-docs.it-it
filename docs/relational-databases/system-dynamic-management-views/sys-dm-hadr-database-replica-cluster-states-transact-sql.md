---
description: sys.dm_hadr_database_replica_cluster_states (Transact-SQL)
title: sys.dm_hadr_database_replica_cluster_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_database_replica_cluster_states
- dm_hadr_database_replica_cluster_states_TSQL
- sys.dm_hadr_database_replica_cluster_states_TSQL
- dm_hadr_database_replica_cluster_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_database_replica_cluster_states dynamic management view
ms.assetid: 6f719071-ebce-470d-aebd-1f55ee8cd70a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b96476a0b9b089fa82b3291f783782359e60636
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182067"
---
# <a name="sysdm_hadr_database_replica_cluster_states-transact-sql"></a>sys.dm_hadr_database_replica_cluster_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga contenente informazioni relative all'integrità dei database di disponibilità nei gruppi di disponibilità Always On nel cluster WSFC (Windows Server Failover Clustering). Eseguire una query **sys.dm_hadr_database_replica_states** per rispondere alle domande seguenti:  
  
-   Tutti i database di un gruppo di disponibilità sono pronti per un failover?  
  
-   Dopo un failover forzato, un database secondario è stato sospeso in locale e il suo stato sospeso è stato riconosciuto nella nuova replica primaria?  
  
-   Se la replica primaria non è attualmente disponibile, quale replica secondaria limiterebbe la perdita di dati se diventasse la replica primaria?  
  
-   Quando il valore della colonna [sys. databases](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)   **log_reuse_wait_desc** è "AVAILABILITY_REPLICA", quale replica secondaria in un gruppo di disponibilità trattiene il troncamento del log in un determinato database primario?  
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Identificatore della replica di disponibilità all'interno del gruppo di disponibilità.|  
|**group_database_id**|**uniqueidentifier**|Identificatore del database nel gruppo di disponibilità. L'identificatore è identico su ogni replica a cui è stato aggiunto questo database.|  
|**database_name**|**sysname**|Nome di un database che appartiene al gruppo di disponibilità.|  
|**is_failover_ready**|**bit**|Indica se il database secondario è sincronizzato con il database primario corrispondente. uno di:<br /><br /> 0 = Il database non è contrassegnato come sincronizzato nel cluster. Il database non è pronto per un failover.<br /><br /> 1 = Il database non è contrassegnato come sincronizzato nel cluster. Il database è pronto per un failover.|  
|**is_pending_secondary_suspend**|**bit**|Indica se, dopo un failover forzato, il database verrà sospeso, uno di:<br /><br /> 0 = Qualsiasi stato a eccezione di HADR_SYNCHRONIZED_ SUSPENDED.<br /><br /> 1 = HADR_SYNCHRONIZED_ SUSPENDED. Al completamento di un failover forzato, ognuno dei database secondari viene impostato su HADR_SYNCHONIZED_SUSPENDED e rimane in questo stato finché la nuova replica primaria non riceverà un acknowledgement da quel database secondario per il messaggio SUSPEND.<br /><br /> NULL = Sconosciuto (senza quorum)|  
|**is_database_joined**|**bit**|Indica se al database su questa replica di disponibilità è stato unito in join al gruppo di disponibilità, uno di:<br /><br /> 0 = Database non unito in join al gruppo di disponibilità su questa replica di disponibilità.<br /><br /> 1 = Database non unito in join al gruppo di disponibilità su questa replica di disponibilità.<br /><br /> NULL = Sconosciuto (la replica di disponibilità non dispone del quorum.)|  
|**recovery_lsn**|**numeric(25,0)**|Sulla replica primaria la fine del log delle transazioni prima che la replica scriva qualsiasi nuovo record del log dopo il failover o il recupero. Sulla replica primaria la riga per un determinato database secondario conterrà il valore a cui la replica secondaria deve essere sincronizzata (ripristino e reinizializzazione).<br /><br /> Sulle repliche secondarie questo valore è NULL. Si noti che ogni replica secondaria avrà il valore MAX o un valore inferiore a cui la replica secondaria dovrà tornare.|  
|**truncation_lsn**|**numeric(25,0)**|Il valore di troncamento del log di [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] che potrebbe essere maggiore dell'LSN di troncamento locale se il troncamento del log locale è bloccato (ad esempio da un'operazione di backup).|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica dei gruppi di disponibilità Always On &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Viste del catalogo dei gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Monitorare i gruppi di disponibilità &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
  
