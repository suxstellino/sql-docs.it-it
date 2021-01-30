---
description: sys.dm_continuous_copy_status (database SQL di Azure)
title: sys.dm_continuous_copy_status
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_continuous_copy_status_TSQL
- dm_continuous_copy_status_TSQL
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
dev_langs:
- TSQL
helpviewer_keywords:
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
ms.assetid: 411b2e71-4421-4ef5-900d-5af068750899
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 7e0dc3ec69f4c35b1361cf12f501f4b23abf29c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207794"
---
# <a name="sysdm_continuous_copy_status-azure-sql-database"></a>sys.dm_continuous_copy_status (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce una riga per ogni database utente (V11) attualmente impegnato in una relazione di copia continua con replica geografica. Se è stata avviata più di una relazione di copia continua per un determinato database primario, questa tabella contiene una riga per ogni database secondario attivo.  
  
Se si usa il database SQL V12 è necessario usare [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (perché *sys.dm_continuous_copy_status* si applica solo a V11).

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**copy_guid**|**uniqueidentifier**|ID univoco del database di replica.|  
|**partner_server**|**sysname**|Nome del server di database SQL collegato.|  
|**partner_database**|**sysname**|Nome del database collegato nel server di database SQL collegato.|  
|**last_replication**|**datetimeoffset**|Timestamp dell'ultima transazione replicata applicata.|  
|**replication_lag_sec**|**int**|Differenza di tempo in secondi tra l'ora corrente e il timestamp dell'ultimo commit della transazione completato nel database primario che non è stato riconosciuto dal database secondario attivo.|  
|**replication_state**|**tinyint**|Stato della replica a copia continua per questo database. Di seguito sono riportati i valori possibili e le relative descrizioni.<br /><br /> 1: seeding. La destinazione della replica viene sottoposta a seeding ed è dal punto di vista transazionale in uno stato non coerente. Finché il seeding non viene completato, non è possibile connettersi al database secondario attivo. <br />2: recupero. Il database secondario attivo attualmente esegue l'aggiornamento al database primario e si trova in uno stato coerente dal punto di vista transazionale.<br />3: re-seeding. Il database secondario attivo automaticamente viene sottoposto nuovamente a seeding a causa di un errore di replica irreversibile.<br />4: sospeso. Non è presente una relazione di copia continua attiva. Questo stato indica in genere che la larghezza di banda disponibile per l'interlink è insufficiente per il livello di attività di transazione nel database primario. La relazione di copia continua tuttavia rimane invariata.|  
|**replication_state_desc**|**nvarchar(256)**|Descrizione di replication_state. I valori possibili sono:<br /><br /> SEEDING<br /><br /> CATCH_UP<br /><br /> RE_SEEDING<br /><br /> SUSPENDED|  
|**is_rpo_limit_reached**|**bit**|Questo valore è sempre impostato su 0|  
|**is_target_role**|**bit**|0 = Origine della relazione di copia<br /><br /> 1 = Destinazione della relazione di copia|  
|**is_interlink_connected**|**bit**|1 = L'interlink è connesso.<br /><br /> 0 = L'interlink è disconnesso.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per recuperare i dati, è richiesta l'appartenenza al ruolo del database **db_owner** . Anche l'utente dbo, i membri del ruolo del database **dbmanager** e l'account di accesso sa possono eseguire query su questa vista.  
  
## <a name="remarks"></a>Commenti  
 La vista **sys.dm_continuous_copy_status** viene creata nel database **Resource** ed è visibile in tutti i database, incluso il database master logico. Tuttavia, se si esegue una query su questa vista nel database master logico viene restituito un set vuoto.  
  
 Se la relazione di copia continua viene terminata su un database, la riga relativa a tale database nella vista **sys.dm_continuous_copy_status** scomparirà.  
  
 Analogamente alla visualizzazione **sys.dm_database_copies** , **sys.dm_continuous_copy_status** riflette lo stato della relazione di copia continua in cui il database è un database primario o secondario attivo. A differenza di **sys.dm_database_copies**, **sys.dm_continuous_copy_status** contiene diverse colonne che forniscono informazioni dettagliate sulle operazioni e sulle prestazioni. Queste colonne includono **last_replication** e **replication_lag_sec**.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_database_copies &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)   
 [Stored procedure Geo-Replication attive &#40;&#41;Transact-SQL ](../system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
