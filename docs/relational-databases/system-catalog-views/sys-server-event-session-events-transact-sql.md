---
description: sys.server_event_session_events (Transact-SQL)
title: sys.server_event_session_events (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_event_session_events
- server_event_session_events_TSQL
- sys.server_event_session_events
- sys.server_event_session_events_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_session_events catalog view
- xe
ms.assetid: 75986e91-1fc7-4f14-98ac-4e90154a74db
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d0476f0aefc103db45ad77c00d49ecf09d422c24
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203426"
---
# <a name="sysserver_event_session_events-transact-sql"></a>sys.server_event_session_events (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni evento in una sessione di eventi.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID della sessione dell'evento. Non ammette i valori Null.|  
|event_id|**int**|ID dell'evento. L'ID è univoco all'interno dell'oggetto della sessione dell'evento. Non ammette i valori Null.|  
|name|**sysname**|Nome dell'evento. Non ammette i valori Null.|  
|Pacchetto|**sysname**|Nome del pacchetto eventi che contiene l'evento. Non ammette i valori Null.|  
|module|**sysname**|Nome del modulo contenente l'evento. Non ammette i valori Null.|  
|predicate|**nvarchar (3000)**|Espressione del predicato applicata all'evento. Ammette i valori Null.|  
|predicate_xml|**nvarchar (3000)**|Espressione del predicato XML applicata all'evento. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="remarks"></a>Commenti  
 Questa vista ha le cardinalità della relazione seguenti.  
  
| Da | A | Relationship |
| ---- | -- | ------------ |
|sys.server_event_session_events.event_session_id|sys.server_event_sessions sys.server_event_sessions.event_session_id|Molti-a-uno|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo degli eventi estesi &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  
