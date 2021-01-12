---
description: sys.fn_trace_getinfo (Transact-SQL)
title: sys.fn_trace_getinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_getinfo
- fn_trace_getinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- traces [SQL Server], status information
- status information [SQL Server], traces
- sys.fn_trace_getinfo function
- fn_trace_getinfo function
ms.assetid: 04b140fe-110a-47b8-98b5-e4c161beb6c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c7426552dd52732b6b22f9947862b8607418f482
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093811"
---
# <a name="sysfn_trace_getinfo-transact-sql"></a>sys.fn_trace_getinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulla traccia specificata o sulle tracce esistenti.  
  
> **IMPORTANTE** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.    
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_trace_getinfo ( { trace_id | NULL | 0 | DEFAULT } )  
```  
  
## <a name="arguments"></a>Argomenti  
 *trace_id*  
 ID della traccia. *trace_id* è di **tipo int**.  Gli input validi sono il numero di ID di una traccia, NULL, 0 o DEFAULT. NULL, 0 e DEFAULT sono valori equivalenti in questo contesto. Specificare NULL, 0 o DEFAULT per restituire informazioni per tutte le tracce presenti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="tables-returned"></a>Tabelle restituite  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|traceId|**int**|ID della traccia.|  
|proprietà|**int**|Proprietà della traccia:<br /><br /> 1= Opzioni della traccia. Per ulteriori informazioni, vedere @options in [sp_trace_create &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md).<br /><br /> 2 = Nome del file<br /><br /> 3 = Dimensioni massime<br /><br /> 4 = Ora di arresto<br /><br /> 5 = Stato corrente della traccia. 0 = arrestato. 1 = in esecuzione.|  
|Valore|**sql_variant**|Informazioni sulla proprietà della traccia specificata.|  
  
## <a name="remarks"></a>Osservazioni  
 Se viene passato l'ID di una traccia specifica, fn_trace_getinfo restituisce le informazioni su tale traccia. Se viene passato un ID non valido, questa funzione restituisce un set di righe vuoto.  
  
 fn_trace_getinfo aggiunge un'estensione trc al nome di qualsiasi file di traccia incluso nel relativo set di risultati. Per informazioni sulla definizione di una traccia, vedere [sp_trace_create &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md). Per informazioni simili sui filtri di traccia, vedere [sys.fn_trace_getfilterinfo &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md).  
  
 Per un esempio completo dell'utilizzo delle stored procedure di traccia, vedere [creare una traccia &#40;&#41;Transact-SQL ](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER TRACE nel server.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono restituite informazioni su tutte le tracce attive.  
  
```  
SELECT * FROM sys.fn_trace_getinfo(0) ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
