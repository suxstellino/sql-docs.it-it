---
description: sys.dm_fts_memory_buffers (Transact-SQL)
title: sys.dm_fts_memory_buffers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_memory_buffers
- dm_fts_memory_buffers_TSQL
- dm_fts_memory_buffers
- sys.dm_fts_memory_buffers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_memory_buffers dynamic management view
ms.assetid: 56895fe5-e8df-4d75-9adc-c1f7757cdef8
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9240f647994bdeb0eeee580fb14b12208e58aa54
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345317"
---
# <a name="sysdm_fts_memory_buffers-transact-sql"></a>sys.dm_fts_memory_buffers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni sui buffer di memoria appartenenti a uno specifico pool di memoria utilizzati in una ricerca o in un intervallo di ricerche per indicizzazione full-text.  
  
> [!NOTE]
> La colonna seguente verrà rimossa in una versione futura di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **ROW_COUNT**. Evitare l'utilizzo di questa colonna in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è implementata.  

  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|**pool_id**|**int**|ID del pool di memoria allocato.<br /><br /> 0 = Buffer di piccole dimensioni<br /><br /> 1 = Buffer di grandi dimensioni|  
|**memory_address**|**varbinary (8)**|Indirizzo del buffer di memoria allocato.|  
|**nome**|**nvarchar(4000)**|Nome del buffer di memoria condivisa per cui è stata eseguita questa allocazione.|  
|**is_free**|**bit**|Stato corrente del buffer di memoria.<br /><br /> 0 = Libero<br /><br /> 1 = Occupato|  
|**row_count**|**int**|Numero di righe gestite dal buffer.|  
|**bytes_used**|**int**|Quantità, espressa in byte, di memoria utilizzata nel buffer.|  
|**percent_used**|**int**|Percentuale della memoria allocata utilizzata.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="physical-joins"></a>Join fisici  
 ![Join significativi di questa DMV](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-memory-buffers-1.gif "Join significativi di questa DMV")  
  
## <a name="relationship-cardinalities"></a>Cardinalità delle relazioni  
  
|Da|A|Relationship|  
|----------|--------|------------------|  
|dm_fts_memory_buffers.pool_id|dm_fts_memory_pools.pool_id|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

