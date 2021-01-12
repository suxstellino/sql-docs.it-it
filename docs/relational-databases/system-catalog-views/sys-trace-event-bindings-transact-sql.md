---
description: sys.trace_event_bindings (Transact-SQL)
title: sys.trace_event_bindings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.trace_event_bindings_TSQL
- trace_event_bindings
- sys.trace_event_bindings
- trace_event_bindings_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_event_bindings catalog view
ms.assetid: 22f534e1-4ed6-4b3e-9ead-1d1001a1b0f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 709a6fdcf24036d5a8a26630902e8c7816478943
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094401"
---
# <a name="systrace_event_bindings-transact-sql"></a>sys.trace_event_bindings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Nella vista del catalogo **sys.trace_event_bindings** è incluso un elenco di tutte le possibili combinazioni di utilizzo di eventi e colonne. Per ogni evento elencato nella colonna **trace_event_id** , tutte le colonne disponibili sono elencate nella colonna **trace_column_id** . Quando si verifica un evento specifico, non tutte le colonne disponibili vengono popolate. Questi valori non cambiano per una versione specifica di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Per un elenco completo degli eventi di traccia supportati, vedere [SQL Server riferimento alla classe di evento](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare le viste del catalogo di Eventi estesi.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**trace_event_id**|**smallint**|ID dell'evento di traccia. Questa colonna si trova anche nella vista del catalogo **sys.trace_events** .|  
|**trace_column_id**|**smallint**|ID della colonna di traccia. Questa colonna si trova anche nella vista del catalogo **sys.trace_columns** .|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys. Traces &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys.trace_categories &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [sys.trace_events &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys.trace_subclass_values &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  
