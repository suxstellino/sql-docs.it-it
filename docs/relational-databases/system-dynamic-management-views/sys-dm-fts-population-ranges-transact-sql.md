---
description: sys.dm_fts_population_ranges (Transact-SQL)
title: sys.dm_fts_population_ranges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_population_ranges
- sys.dm_fts_population_ranges_TSQL
- dm_fts_population_ranges_TSQL
- dm_fts_population_ranges
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_population_ranges dynamic management view
ms.assetid: 58d8564b-9c43-4965-a31c-2893890334ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ce4d78571837690ed06a9f2f506e2c19411d17a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836518"
---
# <a name="sysdm_fts_population_ranges-transact-sql"></a>sys.dm_fts_population_ranges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni sugli intervalli specifici correlati a un popolamento dell'indice full-text in corso.  
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**memory_address**|**varbinary (8)**|Indirizzo dei buffer di memoria allocati per l'attività correlata all'intervallo secondario corrente del popolamento di un indice full-text.|  
|**parent_memory_address**|**varbinary (8)**|Indirizzo dei buffer di memoria che rappresentano l'oggetto padre di tutti gli intervalli di popolamento correlati a un indice full-text.|  
|**is_retry**|**bit**|Se il valore è 1, questo intervallo secondario è responsabile del nuovo tentativo di esecuzione di righe in cui si sono verificati errori.|  
|**session_id**|**smallint**|ID della sessione che elabora l'attività.|  
|**processed_row_count**|**int**|Numero di righe elaborate dall'intervallo. Lo stato viene mantenuto e calcolato ogni 5 minuti, anziché con il commit di ogni batch.|  
|**error_count**|**int**|Numero di righe in cui l'intervallo ha rilevato errori. Lo stato viene mantenuto e calcolato ogni 5 minuti, anziché con il commit di ogni batch.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="physical-joins"></a>Join fisici  
 ![Join significativi di questa DMV](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-population-ranges-1.gif "Join significativi di questa DMV")  
  
## <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
  [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
