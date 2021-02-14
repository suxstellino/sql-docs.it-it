---
description: sys.dm_hadr_ag_threads (Transact-SQL)
title: sys.dm_hadr_ag_threads (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_ag_threads_TSQL
- sys.dm_hadr_ag_threads
- dm_hadr_ag_threads_TSQL
- dm_hadr_ag_threads
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_ag_threads catalog view
ms.assetid: ''
author: kfarlee
ms.author: kfarlee
monikerRange: ''
ms.openlocfilehash: c8487dd07e424b7edccc17b8a9587cfc91f74268
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100354130"
---
# <a name="sysdm_hadr_ag_threads-transact-sql"></a>sys.dm_hadr_ag_threads (Transact-SQL)

I dati di telemetria del thread HADR DMV (**sys.dm_hadr_ag_threads** e [sys.dm_hadr_db_threads](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-db-threads-transact-sql.md)) consentono agli utenti di ottenere rapidamente informazioni sull'utilizzo dei thread per gruppo di disponibilità e per database a disponibilità elevata. Comprendere questo utilizzo dei thread è un importante benchmark per l'ottimizzazione dei gruppi di disponibilità.

Questa DMV segnala l'utilizzo dei thread a livello del gruppo di disponibilità.

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificatore del gruppo di disponibilità.|
|**nome**|**nvarchar(128)**|Nome del gruppo di disponibilità.|
|**num_databases**|**int**|Numero di database nel gruppo di disponibilità.|
|**num_capture_threads**|**int**|Numero di thread di acquisizione dei log utilizzati da tutti i database in questo gruppo di disponibilità.|
|**num_redo_threads**|**int**|Numero di thread di rollforward utilizzati da tutti i database in questo gruppo di disponibilità.|
|**num_parallel_redo_threads**|**int**|Numero di thread di rollforward paralleli utilizzati da tutti i database in questo gruppo di disponibilità.|
|**num_hadr_threads**|**int**|Numero di tutti i thread always on utilizzati da tutti i database in questo gruppo di disponibilità.|

## <a name="permissions"></a>Autorizzazioni  

 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  

 [Funzioni e viste a gestione dinamica dei gruppi di disponibilità Always On &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Viste del catalogo dei gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Monitorare i gruppi di disponibilità &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  