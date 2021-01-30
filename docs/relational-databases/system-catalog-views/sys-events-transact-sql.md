---
description: sys.events (Transact-SQL)
title: sys. Events (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.events_TSQL
- sys.events
- events_TSQL
- events
dev_langs:
- TSQL
helpviewer_keywords:
- sys.events catalog view
ms.assetid: f245a97a-80fc-43fb-a6e4-139420c9a47a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c4bbf0027af329447b818235a76190259dcd9987
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203851"
---
# <a name="sysevents-transact-sql"></a>sys.events (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una riga per ogni evento per il quale si verifica l'attivazione di un trigger o di una notifica degli eventi. Questi eventi rappresentano i tipi di evento specificati quando viene creato il trigger o la notifica degli eventi utilizzando [Crea trigger](../../t-sql/statements/create-trigger-transact-sql.md) o [Crea notifica evento](../../t-sql/statements/create-event-notification-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID del trigger o della notifica degli eventi. Questo valore, insieme al **tipo**, identifica in modo univoco la riga.|  
|**type**|**int**|Evento che attiva il trigger.|  
|**type_desc**|**nvarchar(60)**|Descrizione dell'evento che attiva il trigger.|  
|**is_trigger_event**|**bit**|1 = evento di trigger.<br /><br /> 0 = evento di notifica.|  
|**event_group_type**|**int**|Gruppo di eventi nel quale viene creato il trigger o la notifica di evento, o null se non viene creato in un gruppo di eventi.|  
|**event_group_type_desc**|**nvarchar(60)**|Descrizione del gruppo di eventi nel quale viene creato il trigger o la notifica di evento, o null se non viene creato in un gruppo di eventi.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
