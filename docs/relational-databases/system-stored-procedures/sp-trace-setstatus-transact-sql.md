---
description: sp_trace_setstatus (Transact-SQL)
title: sp_trace_setstatus (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setstatus_TSQL
- sp_trace_setstatus
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setstatus
ms.assetid: 29e7a7d7-b9c1-414a-968a-fc247769750d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69090ce81bf7a70d28cf12959c412d88f270cfdd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209711"
---
# <a name="sp_trace_setstatus-transact-sql"></a>sp_trace_setstatus (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica lo stato corrente della traccia specificata.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_trace_setstatus [ @traceid = ] trace_id , [ @status = ] status  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @traceid = ] trace_id` ID della traccia da modificare. *trace_id* è di **tipo int** e non prevede alcun valore predefinito. L'utente utilizza questo *trace_id* valore per identificare, modificare e controllare la traccia. Per informazioni sul recupero del *trace_id*, vedere [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md).  
  
`[ @status = ] status` Specifica l'azione da implementare nella traccia. *status* è di **tipo int** e non prevede alcun valore predefinito.  
  
 Nella tabella seguente sono inclusi i possibili valori di stato.  
  
|Stato|Descrizione|  
|------------|-----------------|  
|**0**|Arresta la traccia specificata.|  
|**1**|Avvia la traccia specificata.|  
|**2**|Chiude la traccia specificata e ne elimina la definizione dal server.|  
  
> [!NOTE]  
>  È necessario che la traccia venga arrestata prima di chiuderla. Prima di visualizzare una traccia, è necessario arrestarla e chiuderla.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nella tabella seguente vengono descritti i possibili valori di codice visualizzati al completamento della stored procedure.  
  
|Codice restituito|Descrizione|  
|-----------------|-----------------|  
|**0**|Nessun errore.|  
|**1**|Errore sconosciuto.|  
|**8**|Lo stato specificato non è valido.|  
|**9**|L'handle di traccia specificato non è valido.|  
|**13**|Memoria insufficiente. Restituito quando la quantità di memoria disponibile non è sufficiente per eseguire l'azione specificata.|  
  
 Se la traccia è già nello stato specificato, restituirà [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **0**.  
  
## <a name="remarks"></a>Commenti  
 I parametri di tutte le stored procedure di traccia SQL (**sp_trace_xx**) sono fortemente tipizzati. Se questi parametri non vengono chiamati con i tipi di dati corretti per i parametri di input, come indicato nella descrizione dell'argomento, la stored procedure restituirà un errore.  
  
 Per un esempio dell'uso di stored procedure relative alla traccia, vedere [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre delle autorizzazioni ALTER TRACE.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fn_trace_geteventinfo &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [Traccia SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
