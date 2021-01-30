---
description: sys.trace_subclass_values (Transact-SQL)
title: sys.trace_subclass_values (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.trace_subclass_values
- trace_subclass_values_TSQL
- sys.trace_subclass_values_TSQL
- trace_subclass_values
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_subclass_values catalog view
ms.assetid: 542b19ca-61c8-41ca-aa2e-0aba8906cc24
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f56994ed647544da390687c9e229b6657d63c815
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208336"
---
# <a name="systrace_subclass_values-transact-sql"></a>sys.trace_subclass_values (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista del catalogo **sys.trace_subclass_values** contiene un elenco di valori di colonna denominati. Questi valori di sottoclasse non cambiano per una determinata versione di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Per un elenco completo degli eventi di traccia supportati, vedere [SQL Server riferimento alla classe di evento](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare le viste del catalogo di Eventi estesi.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**trace_event_id**|**smallint**|ID dell'evento di traccia. Questo parametro si trova anche nella vista del catalogo **sys.trace_events** .|  
|**trace_column_id**|**smallint**|ID della colonna di traccia utilizzato per l'enumerazione. Questo parametro si trova anche nella vista del catalogo **sys.trace_columns** .|  
|**subclass_name**|**nvarchar(128)**|Significato del valore della colonna.|  
|**subclass_value**|**smallint**|Valore della colonna.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys. Traces &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys.trace_categories &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [sys.trace_events &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys.trace_event_bindings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)  
  
  
