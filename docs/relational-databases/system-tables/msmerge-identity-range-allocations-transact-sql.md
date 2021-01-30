---
description: MSmerge_identity_range_allocations (Transact-SQL)
title: MSmerge_identity_range_allocations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_identity_range_allocations
- MSmerge_identity_range_allocations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_identity_range_allocations system table
ms.assetid: 6362e35e-0ab3-4638-855b-1ce013f5fd6d
author: cawrites
ms.author: chadam
ms.openlocfilehash: c86e1c65af5852d08ea3e1bd331ab04be8c5a5ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209608"
---
# <a name="msmerge_identity_range_allocations-transact-sql"></a>MSmerge_identity_range_allocations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_identity_range_allocations** viene utilizzata per tenere traccia della cronologia delle assegnazioni degli intervalli di valori Identity, per gli autori e i sottoscrittori, per gli articoli pubblicati. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|ID del server di pubblicazione.|  
|**publisher_db**|**nvarchar(128)**|Nome del database di pubblicazione.|  
|**pubblicazione**|**nvarchar(128)**|Nome della pubblicazione.|  
|**articolo**|**nvarchar(128)**|Nome dell'articolo.|  
|**Sottoscrittore**|**nvarchar(128)**|Nome del Sottoscrittore.|  
|**subscriber_db**|**nvarchar(128)**|Nome del database di sottoscrizione.|  
|**is_pub_range**|**bit**|Indica se l'intervallo di valori Identity è assegnato a un server di pubblicazione.|  
|**ranges_allocated**|**tinyint**|Numero di intervalli di valori Identity assegnati.|  
|**range_begin**|**numerico (38)**|Valore iniziale dell'intervallo.|  
|**range_end**|**numerico (38)**|Valore finale dell'intervallo.|  
|**next_range_begin**|**numerico (38)**|Valore iniziale dell'intervallo successivo da assegnare.|  
|**next_range_end**|**numerico (38)**|Valore finale dell'intervallo successivo da assegnare.|  
|**max_used**|**numerico (38)**|Valore Identity più alto utilizzato.|  
|**time_of_allocation**|**datetime**|Ora in cui è stata eseguita l'assegnazione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
