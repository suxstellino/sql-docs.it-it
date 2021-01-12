---
description: sys.traces (Transact-SQL)
title: sys. Traces (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- traces
- sys.traces_TSQL
- sys.traces
- traces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.traces catalog view
ms.assetid: 4a03be22-b7da-4e2a-97ff-94bed890a620
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 523589a6840581bf96c436eea7393f9fa03beca7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094383"
---
# <a name="systraces-transact-sql"></a>sys.traces (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista del catalogo **sys. Traces** contiene le tracce correnti in esecuzione nel sistema. Questa vista è concepita come sostituzione per la funzione **fn_trace_getinfo** .  
  
 Per un elenco completo degli eventi di traccia supportati, vedere [SQL Server riferimento alla classe di evento](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare le viste del catalogo di Eventi estesi.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID della traccia.|  
|**Stato**|**int**|Stato della traccia:<br /><br /> 0 = arrestato<br /><br /> 1 = in esecuzione|  
|**path**|**nvarchar(260)**|Percorso del file di traccia. Il valore è Null quando la traccia è una traccia di un set di righe.|  
|**max_size**|**bigint**|Dimensioni massime in megabyte (MB) per il file di traccia. Il valore è Null quando la traccia è una traccia di un set di righe.|  
|**stop_time**|**datetime**|Ora di arresto dell'esecuzione della traccia.|  
|**max_files**|**int**|Numero massimo di file di rollover. Il valore è Null se il numero massimo non è impostato.|  
|**is_rowset**|**bit**|1 = traccia di un set di righe.|  
|**is_rollover**|**bit**|1 = l'opzione di rollover è abilitata.|  
|**is_shutdown**|**bit**|1 = l'opzione di chiusura è abilitata.|  
|**is_default**|**bit**|1 = traccia predefinita.|  
|**buffer_count**|**int**|Numero di buffer in memoria utilizzati dalla traccia.|  
|**buffer_size**|**int**|Dimensioni di ogni buffer (KB).|  
|**file_position**|**bigint**|Ultimo percorso del file di traccia. Il valore è Null quando la traccia è una traccia di un set di righe.|  
|**reader_spid**|**int**|ID sessione del lettore di traccia del set di righe. Il valore è Null quando la traccia è una traccia di un file.|  
|**start_time**|**datetime**|Ora di inizio della traccia.|  
|**last_event_time**|**datetime**|Ora di generazione dell'ultimo evento.|  
|**event_count**|**bigint**|Numero totale di eventi generati.|  
|**dropped_event_count**|**int**|Numero totale di eventi eliminati.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys.trace_categories &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [sys.trace_events &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys.trace_event_bindings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)   
 [sys.trace_subclass_values &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  
