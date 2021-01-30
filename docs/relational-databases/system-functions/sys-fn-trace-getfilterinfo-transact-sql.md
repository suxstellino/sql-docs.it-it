---
description: sys.fn_trace_getfilterinfo (Transact-SQL)
title: sys.fn_trace_getfilterinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_trace_getfilterinfo
- fn_trace_getfilterinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], filters
- sys.fn_trace_getfilterinfo function
- filters [SQL Server], traces
- fn_trace_getfilterinfo function
ms.assetid: 09fe4a28-ff8a-4655-9da1-4654d5bc514d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7e20ee696ab4d371c6b12dfb24bd5433170080ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200370"
---
# <a name="sysfn_trace_getfilterinfo-transact-sql"></a>sys.fn_trace_getfilterinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui filtri applicati alla traccia specificata.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_trace_getfilterinfo ( trace_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 *trace_id*  
 ID della traccia. *trace_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Restituisce le informazioni seguenti. Per ulteriori informazioni sulle colonne, vedere [sp_trace_setfilter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**ColumnID**|**int**|ID della colonna a cui è applicato il filtro.|  
|**logical_operator**|**int**|Specifica se viene applicato l'operatore AND o OR.|  
|**comparison_operator**|**int**|Specifica il tipo di confronto eseguito:<br /><br /> 0 = Uguale a<br /><br /> 1 = Diverso da<br /><br /> 2 = Maggiore di<br /><br /> 3 = Minore di<br /><br /> 4 = Maggiore o uguale a<br /><br /> 5 = Minore o uguale a<br /><br /> 6 = Simile a<br /><br /> 7 = Non simile a|  
|**value**|**sql_variant**|Specifica il valore a cui viene applicato il filtro.|  
  
## <a name="remarks"></a>Commenti  
 L'utente imposta *trace_id* valore per identificare, modificare e controllare la traccia. Quando viene passato l'ID di una traccia specifica, **fn_trace_getfilterinfo** restituisce informazioni su qualsiasi filtro in tale traccia. Se alla traccia specificata non è associato un filtro, questa funzione restituisce un set di righe vuoto. Se viene passato un ID non valido, questa funzione restituisce un set di righe vuoto. Per informazioni analoghe sulle tracce, vedere [sys.fn_trace_getinfo &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER TRACE nel server.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni su tutti i filtri relativi al numero di traccia 2.  
  
```  
SELECT * FROM fn_trace_getfilterinfo(2) ;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
