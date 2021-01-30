---
description: sys.dm_exec_text_query_plan (Transact-SQL)
title: sys.dm_exec_text_query_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_text_query_plan
- sys.dm_exec_text_query_plan_TSQL
- dm_exec_text_query_plan_TSQL
- sys.dm_exec_text_query_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_text_query_plan dynamic management function
ms.assetid: 9d5e5f59-6973-4df9-9eb2-9372f354ca57
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 95259700c5235ebe748ae4eaaa0f320df8c7a595
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192950"
---
# <a name="sysdm_exec_text_query_plan-transact-sql"></a>sys.dm_exec_text_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce il piano Showplan in formato testo per un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] o per un'istruzione specifica nel batch. Il piano di query specificato tramite l'handle del piano può essere memorizzato nella cache o in esecuzione. Questa funzione con valori di tabella è simile a [sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md), ma presenta le differenze seguenti:  
  
-   L'output del piano di query viene restituito in formato testo.  
-   Per l'output del piano di query non sono previsti limiti di dimensioni.  
-   È possibile specificare singole istruzioni nel batch.  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_exec_text_query_plan   
(   
    plan_handle   
    , { statement_start_offset | 0 | DEFAULT }  
        , { statement_end_offset | -1 | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Argomenti  
*plan_handle*  
È un token che identifica in modo univoco un piano di esecuzione di query per un batch eseguito e il relativo piano risiede nella cache dei piani oppure è attualmente in esecuzione. *plan_handle* è di tipo **varbinary (64)**.   

Il *plan_handle* può essere ottenuto dagli oggetti a gestione dinamica seguenti: 
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
*statement_start_offset* | 0 | PREDEFINITA  
Indica, in byte, la posizione iniziale della query descritta dalla riga all'interno del testo del batch o dell'oggetto persistente. *statement_start_offset* è di **tipo int**. Il valore 0 indica l'inizio del batch. Il valore predefinito è 0.  
  
È possibile ottenere l'offset iniziale dell'istruzione dagli oggetti a gestione dinamica seguenti:  
  
-  [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-  [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
*statement_end_offset* | -1 | PREDEFINITA  
Indica, in byte, la posizione finale della query descritta dalla riga all'interno del testo del batch o dell'oggetto persistente.  
  
*statement_start_offset* è di **tipo int**.  
  
Il valore -1 indica la fine del batch. Il valore predefinito è -1.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID del database di contesto attivo al momento della compilazione dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] corrispondente a questo piano. Per istruzioni SQL ad hoc e preparate, l'ID del database in cui sono state compilate le istruzioni.<br /><br /> La colonna ammette i valori Null.|  
|**objectid**|**int**|ID dell'oggetto (ad esempio, stored procedure o funzione definita dall'utente) per il piano della query. Per i batch ad hoc e preparati, questa colonna è **null**.<br /><br /> La colonna ammette i valori Null.|  
|**number**|**smallint**|Valore intero della stored procedure numerata. Ad esempio, le procedure utilizzate per l'applicazione **orders** potrebbero essere denominate **orderproc;1**, **orderproc;2** e così via. Per i batch ad hoc e preparati, questa colonna è **null**.<br /><br /> La colonna ammette i valori Null.|  
|**crittografati**|**bit**|Indica se la stored procedure corrispondente è crittografata.<br /><br /> 0 = non crittografata<br /><br /> 1 = crittografata<br /><br /> La colonna non ammette i valori Null.|  
|**query_plan**|**nvarchar(max)**|Contiene la rappresentazione Showplan della fase di compilazione del piano di esecuzione della query specificato con *plan_handle*. La rappresentazione Showplan è in formato testo. Viene generato un piano per ogni batch contenente ad esempio istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, chiamate di stored procedure e chiamate di funzioni definite dall'utente.<br /><br /> La colonna ammette i valori Null.|  
  
## <a name="remarks"></a>Commenti  
 Nelle condizioni seguenti non viene restituito alcun output Showplan nella colonna **plan** della tabella restituita per **sys.dm_exec_text_query_plan**:  
  
-   Se il piano di query specificato utilizzando *plan_handle* è stato eliminato dalla cache dei piani, la colonna **query_plan** della tabella restituita è null. Questa condizione si verifica, ad esempio, in presenza di un ritardo di tempo tra l'acquisizione dell'handle del piano e il relativo utilizzo in **sys.dm_exec_text_query_plan**.  
  
-   Alcune istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] non vengono memorizzate nella cache, ad esempio le istruzioni per operazioni bulk o le istruzioni che contengono valori letterali stringa con dimensioni maggiori di 8 KB. Non è possibile recuperare Showplan per tali istruzioni tramite **sys.dm_exec_text_query_plan** perché non esistono nella cache.  
  
-   Se un [!INCLUDE[tsql](../../includes/tsql-md.md)] batch o stored procedure contiene una chiamata a una funzione definita dall'utente o una chiamata a SQL dinamico, ad esempio tramite exec (*String*), lo Showplan XML compilato per la funzione definita dall'utente non viene incluso nella tabella restituita da **sys.dm_exec_text_query_plan** per il batch o stored procedure. È invece necessario eseguire una chiamata separata a **sys.dm_exec_text_query_plan** per il *plan_handle* che corrisponde alla funzione definita dall'utente.  
  
Quando una query ad hoc utilizza la [parametrizzazione](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) [semplice](../../relational-databases/query-processing-architecture-guide.md#SimpleParam) o forzata, la colonna **query_plan** conterrà solo il testo dell'istruzione e non il piano di query effettivo. Per restituire il piano di query, chiamare **sys.dm_exec_text_query_plan** per l'handle del piano della query con parametri preparata. È possibile determinare se è stata eseguita la parametrizzazione della query facendo riferimento alla colonna **sql** della vista [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) o alla colonna di testo della DMV [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire **sys.dm_exec_text_query_plan**, è necessario che l'utente sia un membro del ruolo predefinito del server **sysadmin** oppure che disponga dell'autorizzazione VIEW SERVER STATE nel server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-retrieving-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>R. Recupero del piano della query memorizzato nella cache per un query o un batch Transact-SQL con esecuzione prolungata  
 Se l'esecuzione di una query o un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] risulta prolungata in una particolare connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile recuperare il piano di esecuzione di tale query o batch per individuare le cause del ritardo. Nell'esempio seguente viene illustrato come recuperare il piano Showplan per una query o un batch con esecuzione prolungata.  
  
> [!NOTE]  
> Per eseguire questo esempio, sostituire i valori per *session_id* e *plan_handle* con i valori specifici del server.  
  
 Utilizzare innanzitutto la stored procedure `sp_who` per recuperare l'ID del processo server (SPID, Server Process ID) per il processo che esegue la query o il batch:  
  
```sql  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
 Il set dei risultati restituito da `sp_who` indica che il valore di SPID è `54`. È possibile utilizzare questo SPID con la vista a gestione dinamica `sys.dm_exec_requests` per recuperare l'handle del piano utilizzando la query seguente:  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 La tabella restituita da **sys.dm_exec_requests** indica che l'handle del piano per la query o il batch con esecuzione prolungata è `0x06000100A27E7C1FA821B10600`. Nell'esempio seguente viene restituito il piano di query per l'handle del piano specificato e vengono utilizzati i valori predefiniti 0 e -1 per restituire tutte le istruzioni nella query o nel batch.  
  
```sql  
USE master;  
GO  
SELECT query_plan   
FROM sys.dm_exec_text_query_plan (0x06000100A27E7C1FA821B10600,0,-1);  
GO  
```  
  
### <a name="b-retrieving-every-query-plan-from-the-plan-cache"></a>B. Recupero di tutti i piani di query dalla cache dei piani  
 Per recuperare uno snapshot di tutti i piani di query disponibili nella cache dei piani, è possibile recuperare gli handle per tutti i piani di query nella cache eseguendo una query sulla vista a gestione dinamica `sys.dm_exec_cached_plans`. Gli handle dei piani sono archiviati nella colonna `plan_handle` della vista `sys.dm_exec_cached_plans`. È quindi necessario utilizzare l'operatore CROSS APPLY per passare gli handle dei piani a `sys.dm_exec_text_query_plan` come illustrato di seguito. L'output Showplan per ogni piano attualmente nella cache dei piani si trova nella `query_plan` colonna della tabella restituita.  
  
```sql  
USE master;  
GO  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_text_query_plan(cp.plan_handle, DEFAULT, DEFAULT);  
GO  
```  
  
### <a name="c-retrieving-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Recupero di tutti i piani di query per cui il server ha raccolto informazioni statistiche sulle query dalla cache dei piani  
 Per recuperare uno snapshot di tutti i piani di query disponibili nella cache dei piani per i quali il server ha raccolto informazioni statistiche, è possibile recuperare gli handle dei piani per tutti i piani di query nella cache eseguendo una query sulla vista a gestione dinamica `sys.dm_exec_query_stats`. Gli handle dei piani sono archiviati nella colonna `plan_handle` della vista `sys.dm_exec_query_stats`. È quindi necessario utilizzare l'operatore CROSS APPLY per passare gli handle dei piani a `sys.dm_exec_text_query_plan` come illustrato di seguito. L'output Showplan per ogni piano viene indicato nella colonna `query_plan` della tabella restituita.  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset);  
GO  
```  
  
### <a name="d-retrieving-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Recupero di informazioni sulle prime cinque query in base al tempo medio di CPU  
 Nell'esempio seguente vengono restituiti i piani di query e il tempo medio di CPU per le prime cinque query. La funzione **sys.dm_exec_text_query_plan** specifica i valori predefiniti 0 e -1 per restituire tutte le istruzioni nel batch nel piano di query.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
Plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, 0, -1)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_exec_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
