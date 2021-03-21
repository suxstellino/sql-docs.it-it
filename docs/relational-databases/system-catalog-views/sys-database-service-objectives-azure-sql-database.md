---
description: sys.database_service_objectives (database SQL di Azure)
title: sys.database_service_objectives
titleSuffix: Azure SQL Database
ms.date: 03/21/2018
ms.service: sql-database
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.topic: conceptual
keywords:
- pool elastico
- pool elastico, gestione
f1_keywords:
- DATABASE_SERVICE_OBJECTIVES_TSQL
ms.assetid: cecd8c31-06c0-4aa7-85d3-ac590e6874fa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 786d236d676897da38753a885b021f2a45bb1ab6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754671"
---
# <a name="sysdatabase_service_objectives-azure-sql-database"></a>sys.database_service_objectives (database SQL di Azure)
[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

Restituisce l'edizione (livello di servizio), l'obiettivo di servizio (piano tariffario) e il nome del pool elastico, se presente, per un database SQL di Azure o un'analisi delle sinapsi di Azure. Se si è connessi al database master in un server di database SQL di Azure, restituisce informazioni su tutti i database. Per Azure sinapsi Analytics, è necessario essere connessi al database master.  
  
  
 Per informazioni sui prezzi, vedere [Opzioni e prestazioni del database SQL: prezzi del database SQL](https://azure.microsoft.com/pricing/details/sql-database/) e [prezzi di analisi delle sinapsi di Azure](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).  
  
 Per modificare le impostazioni del servizio, vedere [ALTER database (database SQL di Azure)](../../t-sql/statements/alter-database-transact-sql.md) e [ALTER database (Azure sinapsi Analytics)](../../t-sql/statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).  
  
 La vista sys.database_service_objectives contiene le colonne seguenti.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|INT|ID del database, univoco all'interno di un'istanza del server di database SQL di Azure. Joinable con [sys. databases &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|edition|sysname|Livello di servizio per il database o data warehouse: **Basic**, **standard**, **Premium** o **data warehouse**.|  
|service_objective|sysname|Piano tariffario del database. Se il database si trova in un pool elastico, restituisce **ElasticPool**.<br /><br /> Il livello **Basic** restituisce **Basic**.<br /><br /> Il **database singolo in un livello di servizio standard** restituisce uno dei seguenti: S0, S1, S2, S3, S4, S6, S7, S9 o S12.<br /><br /> Il **database singolo in un livello Premium** restituisce quanto segue: P1, P2, P4, P6, P11 o P15.<br /><br /> **Azure sinapsi Analytics** restituisce DW100 tramite DW30000c.<br /><br /> Per informazioni dettagliate, vedere [database singoli](/azure/sql-database/sql-database-dtu-resource-limits-single-databases/), [pool elastici](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools/), [data warehouse](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu/)|  
|elastic_pool_name|sysname|Nome del [pool elastico](/azure/azure-sql/database/elastic-pool-overview) a cui appartiene il database. Restituisce **null** se il database è un database singolo o un data warehouse.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **dbManager** per il database master.  A livello di database, l'utente deve essere l'autore o il proprietario.  
  
## <a name="examples"></a>Esempio  
 Questo esempio può essere eseguito nel database master o nei database utente del database SQL di Azure. La query restituisce le informazioni sul nome, il servizio e il livello di prestazioni dei database.  
  
```sql  
SELECT  d.name,   
     slo.*    
FROM sys.databases d   
JOIN sys.database_service_objectives slo    
ON d.database_id = slo.database_id;  
  
```  
  
