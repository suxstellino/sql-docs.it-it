---
description: sys.dm_exec_query_plan (Transact-SQL)
title: sys.dm_exec_query_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18d6e4e8e3c6fe097676d1bf65f337610127566a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094117"
---
# <a name="sysdm_exec_query_plan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce il piano Showplan in formato XML per il batch specificato dall'handle di piano. Il piano specificato tramite l'handle di piano può essere memorizzato nella cache o in esecuzione.  
  
Il XML Schema per lo Showplan è pubblicato e disponibile in [questo sito Web Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409). È inoltre disponibile nella directory di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_exec_query_plan(plan_handle)  
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
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID del database di contesto attivo al momento della compilazione dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] corrispondente a questo piano. Per istruzioni SQL ad hoc e preparate, l'ID del database in cui sono state compilate le istruzioni.<br /><br /> La colonna ammette i valori Null.|  
|**objectid**|**int**|ID dell'oggetto (ad esempio, stored procedure o funzione definita dall'utente) per il piano della query. Per i batch ad hoc e preparati, questa colonna è **null**.<br /><br /> La colonna ammette i valori Null.|  
|**number**|**smallint**|Valore intero della stored procedure numerata. Ad esempio, le procedure utilizzate per l'applicazione **orders** potrebbero essere denominate **orderproc;1**, **orderproc;2** e così via. Per i batch ad hoc e preparati, questa colonna è **null**.<br /><br /> La colonna ammette i valori Null.|  
|**crittografati**|**bit**|Indica se la stored procedure corrispondente è crittografata.<br /><br /> 0 = non crittografata<br /><br /> 1 = crittografata<br /><br /> La colonna non ammette i valori Null.|  
|**query_plan**|**xml**|Contiene la rappresentazione Showplan della fase di compilazione del piano di esecuzione della query specificato con *plan_handle*. La rappresentazione Showplan è in formato XML. Viene generato un piano per ogni batch contenente ad esempio istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, chiamate di stored procedure e chiamate di funzioni definite dall'utente.<br /><br /> La colonna ammette i valori Null.|  
  
## <a name="remarks"></a>Osservazioni  
 Nelle condizioni seguenti non viene restituito alcun output Showplan nella colonna **query_plan** della tabella restituita per **sys.dm_exec_query_plan**:  
  
-   Se il piano di query specificato utilizzando *plan_handle* è stato eliminato dalla cache dei piani, la colonna **query_plan** della tabella restituita è null. Questa condizione si verifica, ad esempio, in presenza di un ritardo di tempo tra l'acquisizione dell'handle del piano e il relativo utilizzo in **sys.dm_exec_query_plan**.  
  
-   Alcune istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] non vengono memorizzate nella cache, ad esempio le istruzioni per operazioni bulk o le istruzioni che contengono valori letterali stringa con dimensioni maggiori di 8 KB. Non è possibile recuperare Showplan XML per tali istruzioni tramite **sys.dm_exec_query_plan** a meno che il batch non sia in esecuzione, perché non esistono nella cache.  
  
-   Se un [!INCLUDE[tsql](../../includes/tsql-md.md)] batch o stored procedure contiene una chiamata a una funzione definita dall'utente o una chiamata a SQL dinamico, ad esempio tramite exec (*String*), lo Showplan XML compilato per la funzione definita dall'utente non viene incluso nella tabella restituita da **sys.dm_exec_query_plan** per il batch o stored procedure. È invece necessario eseguire una chiamata separata a **sys.dm_exec_query_plan** per l'handle di piano corrispondente alla funzione definita dall'utente.  
  
 Quando una query ad hoc utilizza la parametrizzazione semplice o forzata, la colonna **query_plan** conterrà solo il testo dell'istruzione e non il piano di query effettivo. Per restituire il piano di query, chiamare **sys.dm_exec_query_plan** per l'handle del piano della query con parametri preparata. È possibile determinare se è stata eseguita la parametrizzazione della query facendo riferimento alla colonna **sql** della vista [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) o alla colonna di testo della DMV [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
> [!NOTE] 
> A causa di una limitazione del numero di livelli annidati consentiti nel tipo di dati **XML** , **sys.dm_exec_query_plan** non può restituire piani di query che soddisfino o superino 128 livelli di elementi annidati. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa condizione impediva il completamento del piano di query e generava l'errore 6335. In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 e versioni successive, la colonna **QUERY_PLAN** restituisce null.   
> È possibile utilizzare la sys.dm_exec_text_query_plan &#40;funzione a gestione dinamica [&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) per restituire l'output del piano di query in formato testo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire **sys.dm_exec_query_plan**, un utente deve essere un membro del ruolo predefinito del server **sysadmin** o disporre dell' `VIEW SERVER STATE` autorizzazione per il server.  
  
## <a name="examples"></a>Esempi  
 Negli esempi seguenti viene illustrato l'utilizzo della vista a gestione dinamica **sys.dm_exec_query_plan**.  
  
 Per visualizzare i Showplan XML, eseguire le query seguenti nell'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi fare clic su **ShowPlanXML** nella colonna **query_plan** della tabella restituita da **sys.dm_exec_query_plan**. Il piano Showplan XML verrà visualizzato nel riquadro di riepilogo di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Per salvare lo Showplan XML in un file, fare clic con il pulsante destro del mouse su **ShowplanXml** nella colonna **query_plan** , fare clic su **Salva risultati** con nome, denominare il file nel formato \<*file_name*> . sqlplan, ad esempio ShowplanXML. sqlplan.  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>R. Recupero del piano di query memorizzato nella cache per un query o un batch Transact-SQL con esecuzione prolungata  
 I piani delle query per vari tipi di batch [!INCLUDE[tsql](../../includes/tsql-md.md)], come batch ad hoc, stored procedure e funzioni definite dall'utente, vengono memorizzati nella cache in un'area della memoria denominata cache dei piani. Ogni piano della query memorizzato nella cache è identificato da un ID univoco denominato handle del piano. È possibile specificare l'handle del piano con la vista a gestione dinamica **sys.dm_exec_query_plan** per recuperare il piano di esecuzione per una query o un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] specifico.  
  
 Se l'esecuzione di una query o un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] risulta prolungata in una particolare connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile recuperare il piano di esecuzione di tale query o batch per individuare le cause del ritardo. Nell'esempio seguente viene illustrato come recuperare il piano Showplan XML per una query o un batch con esecuzione prolungata.  
  
> [!NOTE]  
>  Per eseguire questo esempio, sostituire i valori per *session_id* e *plan_handle* con i valori specifici del server.  
  
 Utilizzare innanzitutto la stored procedure `sp_who` per recuperare l'ID del processo server (SPID, Server Process ID) per il processo che esegue la query o il batch:  
  
```sql  
USE master;  
GO  
exec sp_who;  
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
  
 La tabella restituita da **sys.dm_exec_requests** indica che l'handle del piano per la query o il batch con esecuzione prolungata è `0x06000100A27E7C1FA821B10600` , che è possibile specificare come argomento *plan_handle* con `sys.dm_exec_query_plan` per recuperare il piano di esecuzione in formato XML, come indicato di seguito. Il piano di esecuzione in formato XML per la query o il batch con esecuzione prolungata è contenuto nella colonna **query_plan** della tabella restituita da `sys.dm_exec_query_plan`.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>B. Recupero di tutti i piani di query dalla cache dei piani  
 Per recuperare uno snapshot di tutti i piani di query disponibili nella cache dei piani, è possibile recuperare gli handle per tutti i piani di query nella cache eseguendo una query sulla vista a gestione dinamica `sys.dm_exec_cached_plans`. Gli handle dei piani sono archiviati nella colonna `plan_handle` della vista `sys.dm_exec_cached_plans`. È quindi necessario utilizzare l'operatore CROSS APPLY per passare gli handle dei piani a `sys.dm_exec_query_plan` come illustrato di seguito. L'output Showplan XML per ogni piano disponibile nella cache dei piani viene indicato nella colonna `query_plan` della tabella restituita.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_cached_plans AS cp 
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Recupero di tutti i piani di query per cui sono state raccolte informazioni statistiche sulle query dalla cache dei piani da parte del server  
 Per recuperare uno snapshot di tutti i piani di query disponibili nella cache dei piani per i quali il server ha raccolto informazioni statistiche, è possibile recuperare gli handle dei piani per tutti i piani di query nella cache eseguendo una query sulla vista a gestione dinamica `sys.dm_exec_query_stats`. Gli handle dei piani sono archiviati nella colonna `plan_handle` della vista `sys.dm_exec_query_stats`. È quindi necessario utilizzare l'operatore CROSS APPLY per passare gli handle dei piani a `sys.dm_exec_query_plan` come illustrato di seguito. L'output Showplan XML per ogni piano disponibile nella cache dei piani per cui il server ha raccolto informazioni statistiche viene indicato nella colonna `query_plan` della tabella restituita.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_stats AS qs 
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Recupero di informazioni sulle prime cinque query in base al tempo medio di CPU  
 Nell'esempio seguente vengono restituiti i piani e il tempo medio di CPU per le prime cinque query.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
   plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  
