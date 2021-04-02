---
description: sys.geo_replication_links (database SQL di Azure)
title: sys.geo_replication_links (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_links_TSQL
- dm_geo_replication_links
- sys.dm_geo_replication_links
- sys.dm_geo_replication_links_TSQL
helpviewer_keywords:
- sys.dm_geo_replication_links dynamic management view
- dm_geo_replication_links dynamic management view
ms.assetid: 58911798-1d60-4f28-87ab-2def2bfc3de7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: af2de4a8fdb5aadc2b11b2057239b27749384e9e
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177102"
---
# <a name="sysgeo_replication_links-azure-sql-database"></a>sys.geo_replication_links (database SQL di Azure)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contiene una riga per ogni collegamento di replica tra i database primari e secondari in una relazione di replica geografica. La vista risiede nel database master logico.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID del database corrente nella vista sys. databases.|  
|start_date|**datetimeoffset**|Ora UTC in un data center del database SQL locale quando è stata avviata la replica del database.|  
|modify_date|**datetimeoffset**|Ora UTC in un data center del database SQL locale quando la replica geografica del database è stata completata. Il nuovo database viene sincronizzato con il database primario al momento.|  
|link_guid|**uniqueidentifier**|ID univoco del collegamento per la replica geografica.|  
|partner_server|**sysname**|Nome del server di database SQL contenente il database con replica geografica.|  
|partner_database|**sysname**|Nome del database con replica geografica sul server di database SQL collegato.|  
|replication_state|**tinyint**|Stato della replica geografica per il database, uno dei seguenti:<br /><br /> 0 = in sospeso. La creazione del database secondario attivo è pianificata, ma i passaggi necessari per la preparazione non sono ancora stati completati.<br /><br /> 1 = seeding. È in corso il seeding della destinazione di replica geografica, ma i due database non sono ancora sincronizzati. Fino al completamento del seeding, non è possibile connettersi al database secondario. Se si rimuove il database secondario dalla replica primaria, l'operazione di seeding viene annullata.<br /><br /> 2 = aggiornamento. Il database secondario è in uno stato coerente a livello di transazione ed è costantemente sincronizzato con il database primario.|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|ruolo|**tinyint**|Ruolo replica geografica, uno dei seguenti:<br /><br /> 0 = primario. Il database_id fa riferimento al database primario nella relazione di replica geografica.<br /><br /> 1 = secondario.  Il database_id fa riferimento al database primario nella relazione di replica geografica.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Tipo secondario, uno di:<br /><br /> 0 = No. Il database secondario non è accessibile fino al failover.<br /><br /> 1 = sola lettura. Il database secondario è accessibile solo alle connessioni client con ApplicationIntent = ReadOnly.<br /><br /> 2 = Tutte. Il database secondario è accessibile a qualsiasi connessione client.|  
|secondary_allow_connections _desc|**nvarchar(256)**|No<br /><br /> Tutti<br /><br /> Read-Only|  
|percent_copied|**int**|Avanzamento del seeding in percentuale|

## <a name="permissions"></a>Autorizzazioni

Questa vista è disponibile solo nel database **Master** per l'account di accesso dell'entità di livello server.  
  
## <a name="example"></a>Esempio

Mostra tutti i database con collegamenti di replica geografica.  

```sql
SELECT
     database_id  
   , start_date  
   , partner_server  
   , partner_database  
   , replication_state  
   , role_desc  
   , secondary_allow_connections_desc
FROM sys.geo_replication_links;  
```

## <a name="see-also"></a>Vedere anche

 [ALTER DATABASE (database SQL di Azure)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.dm_geo_replication_link_status &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [sys.dm_operation_status &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)
