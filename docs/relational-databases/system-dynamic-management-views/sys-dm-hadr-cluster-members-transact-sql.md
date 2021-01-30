---
description: sys.dm_hadr_cluster_members (Transact-SQL)
title: sys.dm_hadr_cluster_members (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster_members_TSQL
- sys.dm_hadr_cluster_members
- dm_hadr_cluster_members_TSQL
- dm_hadr_cluster_members
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_members catalog view
ms.assetid: feb20b3a-8835-41d3-9a1c-91d3117bc170
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d25fc76bef7bf2ba082c68f6a3949224b50ec9e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179307"
---
# <a name="sysdm_hadr_cluster_members-transact-sql"></a>sys.dm_hadr_cluster_members (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Se il nodo WSFC che ospita un'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilitata per [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] dispone di quorum WSFC, restituisce una riga per ogni membro che costituisce il quorum e lo stato di ognuno di essi. Sono inclusi tutti i nodi del cluster (restituiti con CLUSTER_ENUM_NODE tipo dalla funzione **ClusterEnum** ) e il server di controllo del disco o della condivisione file, se presente. La riga restituita per un determinato membro contiene informazioni sullo stato di quel membro. Ad esempio, per un cluster a cinque nodi con quorum di maggioranza dei nodi in cui un nodo è inattivo, quando **sys.dm_hadr_cluster_members** viene eseguita una query da un'istanza del server abilitata per [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] che risiede in un nodo con quorum, **sys.dm_hadr_cluster_members** riflette lo stato del nodo inattivo come "NODE_DOWN".  
  
 Se il nodo WSFC non dispone di quorum, non viene restituita alcuna riga.  
  
 Utilizzare questa DMV per rispondere alle domande seguenti:  
  
-   Quali nodi sono attualmente in esecuzione nel cluster WSFC?  
  
-   Quanti errori può ancora tollerare il cluster WSFC prima di perdere il quorum in uno scenario con nodi di maggioranza?  

 > [!TIP]
 > A partire [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] da, questa vista a gestione dinamica supporta always on istanze del cluster di failover oltre ai gruppi di disponibilità always on.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|Nome del membro che può essere un nome computer, una lettera di unità o un percorso di condivisione file.|  
|**member_type**|**tinyint**|Tipo di membro, uno di:<br /><br /> 0 = Nodo WSFC<br /><br /> 1 = Disco di controllo<br /><br /> 2 = Condivisione file di controllo<br /><br /> 3 = cloud Witness|  
|**member_type_desc**|**nvarchar(50)**|Descrizione di **member_type**, uno di:<br /><br /> CLUSTER_NODE<br /><br /> DISK_WITNESS<br /><br /> FILE_SHARE_WITNESS<br /><br /> CLOUD_WITNESS|  
|**member_state**|**tinyint**|Stato del membro, uno di:<br /><br /> 0 = Offline<br /><br /> 1 = Online|  
|**member_state_desc**|**nvarchar(60)**|Descrizione di **member_state**, uno di:<br /><br /> UP<br /><br /> DOWN|  
|**number_of_quorum_votes**|**tinyint**|Numero di voti quorum che possiede questo membro del quorum. Per i quorum Nessuna maggioranza: solo disco, il valore predefinito è 0. Per altri tipi di quorum, il valore predefinito è 1.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="examples"></a>Esempi  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica dei gruppi di disponibilità Always On &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Viste del catalogo dei gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Monitorare i gruppi di disponibilità &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
