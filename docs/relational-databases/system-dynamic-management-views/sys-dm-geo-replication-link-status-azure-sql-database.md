---
description: sys.dm_geo_replication_link_status (database SQL di Azure)
title: sys.dm_geo_replication_link_status
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_link_status
- dm_geo_replication_link_status_TSQL
- sys.dm_geo_replication_link_status
- sys.dm_geo_replication_link_status_TSQL
helpviewer_keywords:
- dm_geo_replication_link_status dynamic management view
- sys.dm_geo_replication_link_status dynamic management view
ms.assetid: d763d679-470a-4c21-86ab-dfe98d37e9fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 2134a7890d23a7a080a576f8337e750bee2563eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092806"
---
# <a name="sysdm_geo_replication_link_status-azure-sql-database"></a>sys.dm_geo_replication_link_status (database SQL di Azure)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contiene una riga per ogni collegamento di replica tra i database primari e secondari in una relazione di replica geografica. Sono inclusi il database primario e i database secondari. Se esiste più di un collegamento di replica continua per un determinato database primario, questa tabella contiene una riga per ogni relazione. La vista viene creata in tutti i database, incluso il database master logico. Tuttavia, se si esegue una query su questa vista nel database master logico viene restituito un set vuoto.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|link_guid|**uniqueidentifier**|ID univoco del collegamento di replica.|  
|partner_server|**sysname**|Nome del server di database SQL contenente il database collegato.|  
|partner_database|**sysname**|Nome del database collegato nel server di database SQL collegato.|  
|last_replication|**datetimeoffset**|Timestamp dell'ultima conferma della transazione da parte del database secondario in base all'orologio del database primario. Questo valore è disponibile solo nel database primario.|  
|replication_lag_sec|**int**|Differenza di tempo in secondi tra il valore last_replication e il timestamp del commit di tale transazione nel database primario in base all'orologio del database primario.  Questo valore è disponibile solo nel database primario.|  
|replication_state|**tinyint**|Stato della replica geografica per il database, uno dei seguenti:<br /><br /> 1 = seeding. È in corso il seeding della destinazione di replica geografica, ma i due database non sono ancora sincronizzati. Fino al completamento del seeding, non è possibile connettersi al database secondario. Se si rimuove il database secondario dalla replica primaria, l'operazione di seeding viene annullata.<br /><br /> 2 = aggiornamento. Il database secondario è in uno stato coerente a livello di transazione ed è costantemente sincronizzato con il database primario.<br /><br /> 4 = sospesa. Non è presente una relazione di copia continua attiva. Questo stato indica in genere che la larghezza di banda disponibile per l'interlink è insufficiente per il livello di attività di transazione nel database primario. La relazione di copia continua tuttavia rimane invariata.|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|ruolo|**tinyint**|Ruolo replica geografica, uno dei seguenti:<br /><br /> 0 = primario. Il database_id fa riferimento al database primario nella relazione di replica geografica.<br /><br /> 1 = secondario.  Il database_id fa riferimento al database primario nella relazione di replica geografica.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Tipo secondario, uno di:<br /><br /> 0 = nessuna connessione diretta è consentita al database secondario e il database non è disponibile per l'accesso in lettura.<br /><br /> 2 = tutte le connessioni sono consentite al database nella REPL secondaria; ication per l'accesso in sola lettura.|  
|secondary_allow_connections_desc|**nvarchar(256)**|No<br /><br /> Tutti|  
|last_commit|**datetimeoffset**|Ora dell'ultima transazione di cui è stato eseguito il commit nel database. Se recuperato nel database primario, indica l'ora dell'ultimo commit nel database primario. Se recuperata nel database secondario, indica l'ora dell'ultimo commit nel database secondario. Se viene recuperato nel database secondario quando il collegamento primario del collegamento di replica è inattivo, indica fino a che punto il database secondario non è stato aggiornato.|
  
> [!NOTE]  
>  Se la relazione di replica viene terminata rimuovendo il database secondario (sezione 4,2), la riga relativa a tale database nella vista **sys.dm_geo_replication_link_status** scomparirà.  
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi account con autorizzazione view_database_state può eseguire query **sys.dm_geo_replication_link_status**.  
  
## <a name="example"></a>Esempio  
 Mostra i ritardi di replica e l'ora dell'ultima replica dei database secondari.  
  
```  
SELECT   
     link_guid  
   , partner_server  
   , last_replication  
   , replication_lag_sec   
FROM sys.dm_geo_replication_link_status;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;database SQL di Azure&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.geo_replication_links &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [sys.dm_operation_status &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)   
 [sp_wait_for_database_copy_sync](../system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync.md)
