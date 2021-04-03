---
description: sys.dm_hadr_physical_seeding_stats (Transact-SQL)
title: sys.dm_hadr_physical_seeding_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_physical_seeding_stats_TSQL
- sys.dm_hadr_physical_seeding_stats
- sys.dm_hadr_physical_seeding_stats_TSQL
- sys.dm_hadr_physical_seeding_stats
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- physical seeding
- sys.dm_hadr_physical_seeding_stats dynamic management view
author: cawrites
ms.author: chadam
ms.openlocfilehash: 89736fd8a97e9532f7729003e4750ac07e703735
ms.sourcegitcommit: 14f2051d329b69a7b5ff7bce1d136cf7f25bb219
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2021
ms.locfileid: "106232265"
---
# <a name="sysdm_hadr_physical_seeding_stats-transact-sql"></a>sys.dm_hadr_physical_seeding_stats (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Nella replica primaria eseguire una query sys.dm_hadr_physical_seeding_stats DMV per visualizzare le statistiche fisiche per ogni processo di seeding attualmente in esecuzione.

La tabella seguente definisce il significato delle varie colonne:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**local_physical_seeding_id**|**uniqueidentifier**|Identificatore univoco di questa operazione di seeding sulla replica locale.|  
|**remote_physical_seeding_id**|**uniqueidentifier**|Identificatore univoco di questa operazione di seeding sulla replica remota.|  
|**local_database_id**|**int**|ID database nella replica locale.|  
|**local_database_name**|**nvarchar**|Nome del database nella replica locale. |
|**remote_machine_name**|**nvarchar**|Nome del computer della replica remota.|  
|**role_desc**|**nvarchar**|Descrizione del ruolo di seeding. (valori disponibili: Source, Destination, server d'ForwarderDestination)|
|**internal_state_desc**|**nvarchar**|Descrizione dello stato della replica locale.|
|**transfer_rate_bytes_per_second**|**bigint**|Velocità di trasferimento del seeding corrente in byte al secondo.|
|**transfered_size_bytes**|**bigint**|Totale byte già trasferiti.|
|**database_size_bytes**|**bigint**|Dimensioni totali in byte del database sottoposta a seeding.|
|**start_time_utc**|**datetime**|Ora di inizio dell'operazione di seeding in formato UTC.|
|**end_time_utc**|**datetime**|Ora di fine dell'operazione di seeding in formato UTC.|
|**estimate_time_complete_utc**|**datetime**|Stima del tempo di completamento per un'operazione di seeding in-process, in formato UTC.|
|**total_disk_io_wait_time_ms**|**bigint**|Somma del tempo di attesa IO del disco rilevato, in ms.|
|**total_network_wait_time_ms**|**bigint**|Somma del tempo di attesa i/o di rete rilevato, in ms.|
|**failure_code**|**int**|Codice di errore per l'operazione di seeding.|
|**failure_message**|**nvarchar**|Messaggio che corrisponde al codice di errore.|
|**failure_time_utc**|**datetime**|Ora dell'errore, in formato UTC.|
|**is_compression_enabled**|**bit**|Indica se la compressione è abilitata per l'operazione di seeding.|
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Correzione automatica della pagina &#40;Gruppi di disponibilità: Mirroring del database&#41;](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Gestione della tabella suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  