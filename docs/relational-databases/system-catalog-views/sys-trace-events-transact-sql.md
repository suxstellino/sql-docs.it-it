---
description: sys.trace_events (Transact-SQL)
title: sys.trace_events (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trace_events_TSQL
- trace_events
- sys.trace_events
- sys.trace_events_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_events catalog view
ms.assetid: e7d2c5df-0e17-4e94-9d41-d36c7ee60662
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d896216ce6a18cdcb8b4bf11a057bdc55fc3bbb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208355"
---
# <a name="systrace_events-transact-sql"></a>sys.trace_events (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Nella vista del catalogo **sys.trace_events** è incluso un elenco di tutti gli eventi di traccia SQL. Questi eventi di traccia non variano in una versione specifica di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
> **IMPORTANTE** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare le viste del catalogo di Eventi estesi.  
  
 Per ulteriori informazioni su questi eventi di traccia, vedere [SQL Server riferimento alla classe di evento](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**trace_event_id**|**smallint**|ID univoco dell'evento. Questa colonna è inoltre presente nelle viste del catalogo **sys.trace_event_bindings** e **sys.trace_subclass_values** .|  
|**category_id**|**smallint**|ID della categoria dell'evento. Questa colonna si trova anche nella vista del catalogo **sys.trace_categories** .|  
|**nome**|**nvarchar(128)**|Nome univoco dell'evento. Questo parametro non è localizzato.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedi anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys. Traces &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys.trace_categories &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [sys.trace_event_bindings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)   
 [sys.trace_subclass_values &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  
