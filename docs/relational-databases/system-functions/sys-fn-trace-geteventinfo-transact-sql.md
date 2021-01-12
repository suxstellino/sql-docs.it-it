---
description: sys.fn_trace_geteventinfo (Transact-SQL)
title: sys.fn_trace_geteventinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_geteventinfo
- fn_trace_geteventinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- events [SQL Server], status information
- fn_trace_geteventinfo function
- sys.fn_trace_geteventinfo function
- status information [SQL Server], events
ms.assetid: 5b1c858a-ca43-4e2b-9d67-8654daaf0cc5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 463c31506de4bdeae5d1c998dd0d6619d3222a5e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101322"
---
# <a name="sysfn_trace_geteventinfo-transact-sql"></a>sys.fn_trace_geteventinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni su un evento tracciato.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_trace_geteventinfo ( trace_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 *trace_id*  
 ID della traccia. *trace_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
## <a name="tables-returned"></a>Tabelle restituite  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**EventID**|**int**|ID dell'evento tracciato|  
|**ColumnID**|**int**|ID di tutte le colonne raccolte da ogni evento|  
  
## <a name="remarks"></a>Osservazioni  
 Quando viene passato l'ID di una traccia specifica, **fn_trace_geteventinfo** restituisce informazioni su tale traccia. Se viene passato un ID non valido, questa funzione restituisce un set di righe vuoto.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER TRACE nel server.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono restituite informazioni sul numero di traccia 2.  
  
```  
SELECT * FROM fn_trace_geteventinfo(2) ;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)  
  
  
