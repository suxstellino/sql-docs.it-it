---
description: sys.fn_get_sql (Transact-SQL)
title: sys.fn_get_sql (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_sql
- sys.fn_get_sql_TSQL
- fn_get_sql_TSQL
- sys.fn_get_sql
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_sql function
- text [SQL Server], SQL handles
- sys.fn_get_sql function
- valid SQL handles [SQL Server]
- SQL handles
ms.assetid: d5fe49b5-0813-48f2-9efb-9187716b2fd4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aee9ac3d8d75081ad3474f215d6bc289580e2da0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206058"
---
# <a name="sysfn_get_sql-transact-sql"></a>sys.fn_get_sql (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il testo dell'istruzione SQL per l'handle SQL specificato.  
  
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa a partire da una delle prossime versioni di Microsoft SQL Server. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Utilizzare sys.dm_exec_sql_text. Per ulteriori informazioni, vedere [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_get_sql ( SqlHandle )  
```  
  
## <a name="arguments"></a>Argomenti  
 *SqlHandle*  
 Valore dell'handle. *SQLHANDLE* è di tipo **varbinary (64)** e non prevede alcun valore predefinito.  
  
## <a name="tables-returned"></a>Tabelle restituite  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|dbid|**smallint**|ID del database. Per istruzioni SQL ad hoc e preparate, l'ID del database in cui sono state compilate le istruzioni.|  
|objectid|**int**|ID dell'oggetto di database. Per istruzioni SQL ad hoc viene restituito NULL.|  
|d'acquisto|**smallint**|Specifica il numero del gruppo, se le procedure sono raggruppate.<br /><br /> 0 = Le voci immesse non sono procedure.<br /><br /> NULL = Istruzioni SQL ad hoc.|  
|encrypted|**bit**|Specifica se l'oggetto è crittografato.<br /><br /> 0 = Non crittografato<br /><br /> 1 = Crittografato|  
|text|**text**|Testo dell'istruzione SQL. Per gli oggetti crittografati viene restituito NULL.|  
  
## <a name="remarks"></a>Commenti  
 È possibile ottenere un handle SQL valido dalla colonna sql_handle della vista a gestione dinamica [sys.dm_exec_requests &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) .  
  
 Se si passa un handle che non è più presente nella cache, fn_get_sq **l** restituisce un set di risultati vuoto. Se si passa un handle non valido, il batch viene arrestato e viene visualizzato un messaggio di errore.  
  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Non è in grado di memorizzare nella cache alcune [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni, ad esempio istruzioni per la copia bulk e istruzioni con valori letterali stringa di dimensioni maggiori di 8 KB. Gli handle relativi a tali istruzioni non possono essere recuperati tramite fn_get_sql.  
  
 La colonna di **testo** del set di risultati viene filtrata per il testo che può contenere password. Per ulteriori informazioni sulle stored procedure correlate alla sicurezza non monitorate, vedere [filtrare una traccia](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 La funzione fn_get_sql restituisce informazioni simili a quelle del comando [DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md) . Di seguito sono riportati due esempi di situazioni in cui è possibile utilizzare fn_get_sql e non DBCC INPUTBUFFER:  
  
-   Quando gli eventi includono più di 255 caratteri.  
  
-   Quando è necessario ottenere il livello di nidificazione massimo corrente di una stored procedure. Si supponga, ad esempio, che esistano due stored procedure denominate sp_1 e sp_2. Se sp_1 chiama sp_2 e si ottiene l'handle dalla vista a gestione dinamica sys.dm_exec_requests mentre sp_2 è in esecuzione, la funzione fn_get_sql restituirà informazioni su sp_2. Inoltre, la funzione fn_get_sql restituisce il testo completo della stored procedure al livello di nidificazione massimo corrente.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre dell'autorizzazione VIEW SERVER STATE nel server.  
  
## <a name="examples"></a>Esempio  
 Gli amministratori del database possono utilizzare la funzione fn_get_sql, come illustrato nell'esempio seguente, per individuare processi che causano problemi. Dopo avere identificato un ID di sessione che causa problemi, l'amministratore può recuperare l'handle SQL di tale sessione, chiamare la funzione fn_get_sql con tale handle e quindi utilizzare l'offset iniziale e quello finale per determinare il testo SQL dell'ID di sessione che crea problemi.  
  
```  
DECLARE @Handle varbinary(64);  
SELECT @Handle = sql_handle   
FROM sys.dm_exec_requests   
WHERE session_id = 52 and request_id = 0;  
SELECT * FROM sys.fn_get_sql(@Handle);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DBCC INPUTBUFFER &#40;&#41;Transact-SQL ](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)   
 [ Processi disys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
