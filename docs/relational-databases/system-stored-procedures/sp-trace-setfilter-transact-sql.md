---
description: sp_trace_setfilter (Transact-SQL)
title: sp_trace_setfilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setfilter
- sp_trace_setfilter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setfilter
ms.assetid: 11e7c7ac-a581-4a64-bb15-9272d5c1f7ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b068b4659b3c3205aa52d6ebffc0daf680c3cd9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209719"
---
# <a name="sp_trace_setfilter-transact-sql"></a>sp_trace_setfilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Applica un filtro a una traccia. **sp_trace_setfilter** possono essere eseguite solo su tracce esistenti interrotte (*lo stato* è **0**). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Restituisce un errore se questa stored procedure viene eseguita su una traccia che non esiste o il cui *stato* è diverso da **0**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_trace_setfilter [ @traceid = ] trace_id   
          , [ @columnid = ] column_id  
          , [ @logical_operator = ] logical_operator  
          , [ @comparison_operator = ] comparison_operator  
          , [ @value = ] value  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @traceid = ] trace_id` ID della traccia su cui è impostato il filtro. *trace_id* è di **tipo int** e non prevede alcun valore predefinito. L'utente utilizza questo *trace_id* valore per identificare, modificare e controllare la traccia.  
  
`[ @columnid = ] column_id` ID della colonna a cui viene applicato il filtro. *column_id* è di **tipo int** e non prevede alcun valore predefinito. Se *column_id* è null, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Cancella tutti i filtri per la traccia specificata.  
  
`[ @logical_operator = ] logical_operator` Specifica se viene applicato l'operatore AND (**0**) o OR (**1**). *logical_operator* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @comparison_operator = ] comparison_operator` Specifica il tipo di confronto da effettuare. *comparison_operator* è di **tipo int** e non prevede alcun valore predefinito. Nella tabella seguente vengono descritti gli operatori di confronto e i valori che li rappresentano.  
  
|Valore|Operatore di confronto|  
|-----------|-------------------------|  
|**0**|= (uguaglianza)|  
|**1**|<>  (non uguale a)|  
|**2**|> (maggiore di)|  
|**3**|< (minore di)|  
|**4**|>= (maggiore o uguale a)|  
|**5**|<= (minore o uguale a)|  
|**6**|LIKE|  
|**7**|Non simile a|  
  
`[ @value = ] value` Specifica il valore da filtrare. Il tipo di dati del *valore* deve corrispondere al tipo di dati della colonna da filtrare. Se, ad esempio, il filtro è impostato su una colonna ID oggetto che è un tipo di dati **int** , il *valore* deve essere **int**. Se *value* è di **tipo nvarchar** o **varbinary**, la lunghezza massima consentita è 8000.  
  
 Quando l'operatore di confronto è LIKE o NOT LIKE, l'operatore logico può includere "%" o un filtro appropriato per l'operazione LIKE.  
  
 È possibile specificare NULL per *value* per filtrare gli eventi con valori di colonna null. Solo gli operatori **0** (= EQUAL) e **1** (<> diverso da) sono validi con null. In questo caso, tali operatori sono equivalenti agli operatori [!INCLUDE[tsql](../../includes/tsql-md.md)] IS NULL e IS NOT NULL.  
  
 Per applicare il filtro tra un intervallo di valori di colonna, **sp_trace_setfilter** necessario eseguire due volte, una volta con un operatore di confronto maggiore o uguale a (' >=') e un'altra ora con un operatore di minore o uguale a (' <=').  
  
 Per ulteriori informazioni sui tipi di dati delle colonne di dati, vedere il [riferimento alla classe di evento SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nella tabella seguente vengono descritti i possibili valori di codice visualizzati al completamento della stored procedure.  
  
|Codice restituito|Descrizione|  
|-----------------|-----------------|  
|0|Nessun errore.|  
|1|Errore sconosciuto.|  
|2|La traccia è in esecuzione. Se si modifica la traccia mentre è in esecuzione, viene generato un errore.|  
|4|La colonna specificata non è valida.|  
|5|La colonna specificata non supporta l'applicazione di filtri. Questo valore viene restituito solo da **sp_trace_setfilter**.|  
|6|L'operatore di confronto specificato non è valido.|  
|7|L'operatore logico specificato non è valido.|  
|9|L'handle di traccia specificato non è valido.|  
|13|Memoria insufficiente. Restituito quando la quantità di memoria disponibile non è sufficiente per eseguire l'azione specificata.|  
|16|Funzione non valida per la traccia.|  
  
## <a name="remarks"></a>Commenti  
 **sp_trace_setfilter** è un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stored procedure che esegue molte delle azioni eseguite in precedenza dalle stored procedure estese disponibili nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilizzare **sp_trace_setfilter** anziché le stored procedure estese di **\* filtro xp_trace_set** per creare, applicare, rimuovere o modificare i filtri sulle tracce. Per ulteriori informazioni, vedere [filtrare una traccia](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 Tutti i filtri per una determinata colonna devono essere abilitati insieme in un'unica esecuzione di **sp_trace_setfilter**. Se, ad esempio, un utente desidera applicare due filtri alla colonna dei nomi di applicazione e un filtro alla colonna dei nomi utente, deve specificare i filtri per il nome dell'applicazione in sequenza. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore se l'utente tenta di specificare un filtro per il nome dell'applicazione nella chiamata a un stored procedure, seguito da un filtro per il nome utente e quindi da un altro filtro per il nome dell'applicazione.  
  
 I parametri di tutte le stored procedure di traccia SQL (**sp_trace_xx**) sono fortemente tipizzati. Se tali parametri non vengono chiamati con i tipi di dati corretti per i parametri di input, come indicato nella descrizione dell'argomento, la stored procedure restituisce un errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre delle autorizzazioni ALTER TRACE.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono impostati tre filtri in Trace `1`. I filtri `N'SQLT%'` e `N'MS%'` vengono applicati alla colonna `AppName`, valore `10`, tramite l'operatore di confronto "`LIKE`". Il filtro `N'joe'` viene applicato a una colonna diversa, ovvero `UserName`, valore `11`, tramite l'operatore di confronto "`EQUAL`".  
  
```  
sp_trace_setfilter  1, 10, 0, 6, N'SQLT%';  
sp_trace_setfilter  1, 10, 0, 6, N'MS%';  
sp_trace_setfilter  1, 11, 0, 0, N'joe';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [Traccia SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
