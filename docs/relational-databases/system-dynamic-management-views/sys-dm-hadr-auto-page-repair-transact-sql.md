---
description: sys.dm_hadr_auto_page_repair (Transact-SQL)
title: sys.dm_hadr_auto_page_repair (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_auto_page_repair_TSQL
- sys.dm_hadr_auto_page_repair
- sys.dm_hadr_auto_page_repair_TSQL
- dm_hadr_auto_page_repair
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- automatic page repair
- sys.dm_hadr_auto_page_repair dynamic management view
ms.assetid: d7840adf-4a1b-41ac-bc94-102c07ad1c79
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 255350fdb2a2de80950575350b5082c6b7bcc6ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206774"
---
# <a name="sysdm_hadr_auto_page_repair-transact-sql"></a>sys.dm_hadr_auto_page_repair (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni tentativo di correzione automatica della pagina in qualsiasi database di disponibilità in una replica di disponibilità ospitata per qualsiasi gruppo di disponibilità dall'istanza del server. Questa vista contiene le righe degli ultimi tentativi automatici di correzione automatica della pagina in un database primario o secondario, con un massimo di 100 righe per database. Non appena un database raggiunge il limite massimo, la riga per il tentativo successivo di correzione automatica della pagina sostituisce una delle voci esistenti.
  
  La tabella seguente definisce il significato delle varie colonne:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database al quale corrisponde questa riga.|  
|**file_id**|**int**|ID del file in cui si trova la pagina.|  
|**page_id**|**bigint**|ID della pagina nel file.|  
|**error_type**|**int**|Tipo di errore. I valori possibili sono i seguenti.<br /><br /> **-** 1 = tutti gli errori hardware 823<br /><br /> 1 = Errori 824 diversi da un errore del checksum o da una pagina incompleta, ad esempio l'ID di una pagina danneggiata<br /><br /> 2 = Errore nel checksum<br /><br /> 3 = Pagina incompleta|  
|**page_status**|**int**|La stato del tentativo di ripristino della pagina:<br /><br /> 2 = in coda per la richiesta dal partner.<br /><br /> 3 = richiesta inviata al partner.<br /><br /> 4 = la pagina è stata corretta.<br /><br /> 5 = la pagina non è stata ripristinata durante l'ultimo tentativo/correzione automatica della pagina tenterà di ripristinare nuovamente la pagina.|  
|**modification_time**|**datetime**|Ora dell'ultima modifica dello stato della pagina.|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Correzione automatica della pagina &#40;Gruppi di disponibilità: Mirroring del database&#41;](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Gestione della tabella suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  

