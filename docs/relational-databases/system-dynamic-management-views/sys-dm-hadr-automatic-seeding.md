---
description: sys.dm_hadr_automatic_seeding (Transact-SQL)
title: sys.dm_hadr_automatic_seeding (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_automatic_seeding_TSQL
- sys.dm_hadr_automatic_seeding
- sys.dm_hadr_automatic_seeding_TSQL
- dm_hadr_automatic_seeding
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- automatic seeding
- sys.dm_hadr_automatic_seeding dynamic management view
ms.assetid: d7840adf-4a1b-41ac-bc94-102c07ad1c79
author: cawrites
ms.author: chadam
ms.openlocfilehash: b53e5077ee42357afc66426044d1caf8f4469c1c
ms.sourcegitcommit: 14f2051d329b69a7b5ff7bce1d136cf7f25bb219
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2021
ms.locfileid: "106232270"
---
# <a name="sysdm_hadr_automatic_seeding-transact-sql"></a>sys.dm_hadr_automatic_seeding (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Nella replica primaria eseguire una query sys.dm_hadr_automatic_seeding per verificare lo stato del processo di seeding automatico. La vista restituisce una riga per ogni processo di seeding.  
  
La tabella seguente definisce il significato delle varie colonne:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime**|Ora di avvio dell'operazione.|
|**completion_time**|**datetime**|Ora di completamento dell'operazione (null se in corso).|  
|**ag_id**|**uniqueidentifier**|ID univoco per ogni gruppo di disponibilità.|  
|**ag_db_id**|**uniqueidentifier**|ID univoco per ogni database nel gruppo disponibile.|  
|**ag_remote_replica_id**|**uniqueidentifier**|ID univoco per l'altra replica coinvolta in questa operazione di seeding.|
|**operation_id**|**uniqueidentifier**|Identificatore univoco per questa operazione di seeding.|  
|**is_source**|**bit**|Ottiene un valore che indica se questa replica è l'origine (primaria) dell'operazione di seeding.|
|**current_state**|**bit**|Ottiene lo stato di inizializzazione corrente in cui si trova l'operazione.|
|**performed_seeding**|**bit**|Il flusso del database per il seeding è inizializzato.|
|**failure_state**|**int**|Ottiene il motivo per cui l'operazione non è riuscita.|
|**failure_state_desc**|**ncharvar**|Ottiene la descrizione dell'operazione non riuscita.|
|**error_code**|**int**|Ottiene il codice di errore SQL rilevato durante il seeding.|
|**number_of_attempts**|**int**|Ottiene il numero di volte in cui è stata tentata questa operazione di seeding.|


## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Correzione automatica della pagina &#40;Gruppi di disponibilità: Mirroring del database&#41;](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Gestione della tabella suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
