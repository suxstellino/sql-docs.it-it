---
description: sys.availability_databases_cluster (Transact-SQL)
title: sys.availability_databases_cluster (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_databases_cluster_TSQL
- sys.availability_databases_cluster
- availability_databases_cluster_TSQL
- availability_databases_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.availability_databases_cluster catalog view
- Availability Groups [SQL Server], databases
ms.assetid: 8d9c57e5-7f39-4315-b466-92748231140a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: caf27210827b7c3ab981efe65c320bc5ddf6dab5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338957"
---
# <a name="sysavailability_databases_cluster-transact-sql"></a>sys.availability_databases_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni database di disponibilità nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è ospitata una replica di disponibilità per qualsiasi gruppo di disponibilità always on nel cluster WSFC (Windows Server Failover Clustering), indipendentemente dal fatto che il database della copia locale sia già stato aggiunto al gruppo di disponibilità.  
  
> [!NOTE]  
>  Quando un database viene aggiunto a un gruppo di disponibilità, viene automaticamente creato un join del database primario con il gruppo. È necessario preparare i database secondari su ogni replica secondaria prima di poterne creare un join al gruppo di disponibilità.   
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificatore univoco del gruppo di disponibilità a cui partecipa il database.<br /><br /> NULL = il database non fa parte di una replica di disponibilità di un gruppo di disponibilità.|  
|**group_database_id**|**uniqueidentifier**|Identificatore univoco del database nel gruppo di disponibilità a cui partecipa il database. **group_database_id** è lo stesso per il database nella replica primaria e in ogni replica secondaria in cui il database è stato aggiunto al gruppo di disponibilità.<br /><br /> NULL = il database non fa parte di una replica di disponibilità in alcun gruppo di disponibilità.|  
|**database_name**|**sysname**|Nome del database aggiunto al gruppo di disponibilità.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Se il chiamante di **sys.availability_databases_cluster** non è il proprietario del database, le autorizzazioni minime necessarie per visualizzare la riga corrispondente sono ALTER ANY database o View any database a livello di server oppure l'autorizzazione Create database nel database **Master** .  
  
## <a name="see-also"></a>Vedere anche  
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.dm_hadr_database_replica_states &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)   
 [sys.dm_hadr_database_replica_cluster_states &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)   
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
