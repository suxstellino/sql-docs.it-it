---
description: sys.dm_user_db_resource_governance (Transact-SQL)
title: sys.dm_user_db_resource_governance (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governance
- sys.resource_governance_TSQL
- resource_governance
- resource_governance_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governance catalog view
ms.assetid: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current
ms.openlocfilehash: 8580ae3857b4c8c7f54599e70134879a076e89ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99131364"
---
# <a name="sysdm_user_db_resource_governance-transact-sql"></a>sys.dm_user_db_resource_governance (Transact-SQL)

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

Restituisce le impostazioni di configurazione e capacità effettive utilizzate dai meccanismi di governance delle risorse nel database o nel pool elastico corrente.
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|INT|ID del database, univoco all'interno di un server di database SQL di Azure.|
|**logical_database_guid**|UNIQUEIDENTIFIER|GUID logico per il database utente che rimane lungo il ciclo di vita di un database utente.  Se si rinomina il database o se ne modifica l'obiettivo del livello di servizio, questo valore non verrà modificato.|
|**physical_database_guid**|UNIQUEIDENTIFIER|GUID fisico per un database utente che rimane per tutta la durata dell'istanza fisica del database utente. Se si modifica l'obiettivo del livello di servizio del database, questo valore verrà modificato.|
|**server_name**|NVARCHAR|Nome del server logico.|
|**database_name**|NVARCHAR|Nome del database logico.|
|**slo_name**|NVARCHAR|Obiettivo del livello di servizio, inclusa la generazione di hardware.|
|**dtu_limit**|INT|Limite DTU del database (NULL per vCore).|
|**cpu_limit**|INT|limite vCore del database (NULL per i database DTU).|
|**min_cpu**|TINYINT|Valore MIN_CPU_PERCENT del pool di risorse del carico di lavoro dell'utente. Vedere [concetti relativi al pool di risorse](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_cpu**|TINYINT|Valore MAX_CPU_PERCENT del pool di risorse del carico di lavoro dell'utente. Vedere [concetti relativi al pool di risorse](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**cap_cpu**|TINYINT|Valore CAP_CPU_PERCENT del pool di risorse del carico di lavoro dell'utente. Vedere [concetti relativi al pool di risorse](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**min_cores**|SMALLINT|Solo per uso interno.|
|**max_dop**|SMALLINT|Valore MAX_DOP per il gruppo del carico di lavoro dell'utente. Vedere [creare un gruppo di carico di lavoro](../../t-sql/statements/create-workload-group-transact-sql.md).|
|**min_memory**|INT|Valore MIN_MEMORY_PERCENT del pool di risorse del carico di lavoro dell'utente. Vedere [concetti relativi al pool di risorse](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_memory**|INT|Valore MAX_MEMORY_PERCENT del pool di risorse del carico di lavoro dell'utente. Vedere [concetti relativi al pool di risorse](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_sessions**|INT|Numero massimo di sessioni consentite nel gruppo del carico di lavoro dell'utente.|
|**max_memory_grant**|INT|Valore REQUEST_MAX_MEMORY_GRANT_PERCENT per il gruppo del carico di lavoro dell'utente. Vedere [creare un gruppo di carico di lavoro](../../t-sql/statements/create-workload-group-transact-sql.md).|
|**max_db_memory**|INT|Solo per uso interno.|
|**govern_background_io**|bit|Solo per uso interno.|
|**min_db_max_size_in_mb**|bigint|Valore di max_size minimo per un file di dati, in MB. Vedere [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**max_db_max_size_in_mb**|bigint|Valore max_size massimo per un file di dati, in MB. Vedere [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**default_db_max_size_in_mb**|bigint|Valore predefinito max_size per un file di dati, in MB. Vedere [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**db_file_growth_in_mb**|bigint|Incremento di crescita predefinito per un file di dati, in MB. Vedere [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**initial_db_file_size_in_mb**|bigint|Dimensioni predefinite per il nuovo file di dati, in MB. Vedere [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**log_size_in_mb**|bigint|Dimensioni predefinite per il nuovo file di log, in MB. Vedere [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**instance_cap_cpu**|INT|Solo per uso interno.|
|**instance_max_log_rate**|bigint|Limite di velocità di generazione del log per l'istanza di SQL Server, in byte al secondo. Si applica a tutti i log generati dall'istanza, inclusi `tempdb` e altri database di sistema. In un pool elastico si applica al log generato da tutti i database nel pool.|
|**instance_max_worker_threads**|INT|Limite di thread di lavoro per l'istanza di SQL Server.|
|**replica_type**|INT|Tipo di replica, dove 0 è primario e 1 è secondario.|
|**max_transaction_size**|bigint|Spazio di log massimo utilizzato da qualsiasi transazione, in KB.|
|**checkpoint_rate_mbps**|INT|Solo per uso interno.|
|**checkpoint_rate_io**|INT|Solo per uso interno.|
|**last_updated_date_utc**|Datetime|Data e ora dell'Ultima modifica o riconfigurazione dell'ultima impostazione, in formato UTC.|
|**primary_group_id**|INT|ID del gruppo di carico di lavoro per il carico di lavoro dell'utente nella replica primaria e nelle repliche secondarie.|
|**primary_group_max_workers**|INT|Limite thread di lavoro per il gruppo del carico di lavoro dell'utente.|
|**primary_min_log_rate**|bigint|Velocità minima di log in byte al secondo a livello di gruppo del carico di lavoro dell'utente. La governance delle risorse non tenterà di ridurre la velocità del log al di sotto di questo valore.|
|**primary_max_log_rate**|bigint|Frequenza massima di log in byte al secondo a livello di gruppo del carico di lavoro dell'utente. La governance delle risorse non consentirà la velocità del log superiore a questo valore.|
|**primary_group_min_io**|INT|Numero minimo di IOPS per il gruppo del carico di lavoro dell'utente. La governance delle risorse non tenterà di ridurre i IOPS al di sotto di questo valore.|
|**primary_group_max_io**|INT|Numero massimo di IOPS per il gruppo del carico di lavoro dell'utente. La governance delle risorse non consentirà IOPS al di sopra di questo valore.|
|**primary_group_min_cpu**|float|Percentuale minima della CPU per il livello del gruppo di carico di lavoro dell'utente. La governance delle risorse non tenterà di ridurre l'utilizzo della CPU al di sotto di questo valore.|
|**primary_group_max_cpu**|float|Percentuale massima della CPU per il livello del gruppo di carico di lavoro dell'utente. La governance delle risorse non consente l'utilizzo della CPU al di sopra di questo valore.|
|**primary_log_commit_fee**|INT|Costo di commit della governance della frequenza dei log per il gruppo di carico di lavoro dell'utente, in byte Una tariffa di commit aumenta le dimensioni di ogni i/o di log in base a un valore fisso ai fini dell'accounting solo della frequenza dei log. L'i/o del log effettivo nell'archiviazione non è aumentato.|
|**primary_pool_max_workers**|INT|Limite di thread di lavoro per il pool di risorse del carico di lavoro dell'utente.|
|**pool_max_io**|INT|Limite massimo di IOPS per il pool di risorse del carico di lavoro dell'utente.|
|**govern_db_memory_in_resource_pool**|bit|Solo per uso interno.|
|**volume_local_iops**|INT|Solo per uso interno.|
|**volume_managed_xstore_iops**|INT|Solo per uso interno.|
|**volume_external_xstore_iops**|INT|Solo per uso interno.|
|**volume_type_local_iops**|INT|Solo per uso interno.|
|**volume_type_managed_xstore_iops**|INT|Solo per uso interno.|
|**volume_type_external_xstore_iops**|INT|Solo per uso interno.|
|**volume_pfs_iops**|INT|Solo per uso interno.|
|**volume_type_pfs_iops**|INT|Solo per uso interno.|
|||

## <a name="permissions"></a>Autorizzazioni

Questa vista richiede l'autorizzazione VIEW DATABASE STATE.

## <a name="remarks"></a>Commenti

Per una descrizione della governance delle risorse nel database SQL di Azure, vedere [limiti delle risorse del database SQL](/azure/sql-database/sql-database-resource-limits-database-server).

> [!IMPORTANT]
> La maggior parte dei dati restituiti da questa DMV è destinata al consumo interno ed è soggetta a modifiche in qualsiasi momento.

## <a name="examples"></a>Esempio

La query seguente, eseguita nel contesto di un database utente, restituisce la velocità massima di log e il numero massimo di IOPS a livello del gruppo di carico di lavoro e del pool di risorse dell'utente. Per un singolo database, viene restituita una riga. Per un database in un pool elastico, viene restituita una riga per ogni database nel pool.

```
SELECT database_name,
       primary_group_id,
       primary_max_log_rate,
       primary_group_max_io,
       pool_max_io
FROM sys.dm_user_db_resource_governance
ORDER BY database_name;  
```

## <a name="see-also"></a>Vedere anche

- [Resource Governor](../resource-governor/resource-governor.md)
- [sys.dm_resource_governor_resource_pools (Transact-SQL)](./sys-dm-resource-governor-resource-pools-transact-sql.md)
- [sys.dm_resource_governor_workload_groups (Transact-SQL)](./sys-dm-resource-governor-workload-groups-transact-sql.md)
- [sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)](./sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database.md)
- [sys.dm_resource_governor_workload_groups_history_ex (database SQL di Azure)](./sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database.md)
- [Governance della frequenza del log delle transazioni](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Limiti delle risorse di DTU per database singolo](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
- [Limiti delle risorse di vCore per database singolo](/azure/sql-database/sql-database-vcore-resource-limits-single-databases)