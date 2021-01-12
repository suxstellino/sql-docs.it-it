---
description: sys.elastic_pool_resource_stats (Database SQL di Azure)
title: sys.elastic_pool_resource_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.elastic_pool_resource_stats catalog view
helpviewer_keywords:
- sys.elastic_pool_resource_stats_TSQL
- sys.elastic_pool_resource_stats
- elastic_pool_resource_stats_TSQL
- elastic_pool_resource_stats
ms.assetid: f242c1bd-3cc8-4c8b-8aaf-c79b6a8a0329
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1b97fe07119f28211186e2efa3986308af477947
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102775"
---
# <a name="syselastic_pool_resource_stats-azure-sql-database"></a>sys.elastic_pool_resource_stats (Database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce statistiche di uso delle risorse per tutti i pool elastici in un server di database SQL di Azure. Per ogni pool elastico è presente una riga per ogni finestra di report di 15 secondi (quattro righe al minuto). Sono inclusi CPU, IO, Log, uso dell'archiviazione e uso di richieste/sessioni simultanee da parte di tutti i database nel pool. Questi dati vengono conservati per 14 giorni. 
  
||  
|-|  
|**Si applica a**:  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] V12.|  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime2**|Ora UTC che indica l'inizio dell'intervallo di Reporting di 15 secondi.|  
|**end_time**|**datetime2**|Ora UTC che indica la fine dell'intervallo di Reporting di 15 secondi.|  
|**elastic_pool_name**|**nvarchar(128)**|Nome del pool di database elastici.|  
|**avg_cpu_percent**|**Decimal (5, 2)**|Utilizzo medio del calcolo espresso in percentuale del limite del pool.|  
|**avg_data_io_percent**|**Decimal (5, 2)**|Utilizzo I/O medio espresso in percentuale sulla base del limite del pool.|  
|**avg_log_write_percent**|**Decimal (5, 2)**|Utilizzo delle risorse di scrittura medio espresso in percentuale del limite del pool.|  
|**avg_storage_percent**|**Decimal (5, 2)**|Utilizzo di spazio di archiviazione medio espresso in percentuale del limite di archiviazione del pool.|  
|**max_worker_percent**|**Decimal (5, 2)**|Numero massimo di ruoli di lavoro simultanei (richieste) espresso in percentuale sulla base del limite del pool.|  
|**max_session_percent**|**Decimal (5, 2)**|Numero massimo di sessioni simultanee espresso in percentuale sulla base del limite del pool.|  
|**elastic_pool_dtu_limit**|**int**|Impostazione del numero massimo DTU del pool elastico corrente per questo pool elastico durante l'intervallo.|  
|**elastic_pool_storage_limit_mb**|**bigint**|Impostazione del limite massimo di archiviazione del pool elastico corrente per questo pool elastico espresso in megabyte durante l'intervallo.|
|**avg_allocated_storage_percent**|**Decimal (5, 2)**|Percentuale di spazio dati allocato da tutti i database nel pool elastico.  Questo è il rapporto tra lo spazio dati allocato e le dimensioni massime dei dati per il pool elastico.  Per ulteriori informazioni, vedere la pagina relativa [alla gestione dello spazio file nel database SQL](/azure/sql-database/sql-database-file-space-management)|  
  
## <a name="remarks"></a>Osservazioni

 Questa vista è presente nel database master del server di database SQL. Per eseguire una query **sys.elastic_pool_resource_stats**, è necessario essere connessi al database master.  
  
## <a name="permissions"></a>Autorizzazioni

 È richiesta l'appartenenza al ruolo **dbmanager** .  
  
## <a name="examples"></a>Esempi

 Nell'esempio seguente vengono restituiti i dati di utilizzo delle risorse ordinati in base all'ora più recente per tutti i pool di database elastici nel server di database SQL corrente.  
  
```sql
SELECT * FROM sys.elastic_pool_resource_stats
ORDER BY end_time DESC;  
```

 Nell'esempio seguente viene calcolato il consumo di percentuale medio di DTU per un determinato pool.  

```sql
SELECT start_time, end_time,
  (SELECT Max(v)
FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS value(v)) AS [avg_DTU_percent]
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = '<your pool name>'
ORDER BY end_time DESC;  
```

## <a name="see-also"></a>Vedere anche

 [Aumentare la crescita esplosiva con i database elastici](/azure/azure-sql/database/elastic-pool-overview)   
 [Creare e gestire un pool di database elastici del database SQL](/azure/azure-sql/database/elastic-pool-overview)   
 [sys.resource_stats &#40;database SQL di Azure&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [sys.dm_db_resource_stats &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)  
  
