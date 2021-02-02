---
description: sys.dm_exec_query_statistics_xml (Transact-SQL)
title: sys.dm_exec_query_statistics_xml (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_statistics_xml
- sys.dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml
helpviewer_keywords:
- sys.dm_exec_query_statistics_xml management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfecb
author: pmasl
ms.author: pelopes
ms.openlocfilehash: a74539209e56e1e2da9053cfffccd7d528684a30
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235836"
---
# <a name="sysdm_exec_query_statistics_xml-transact-sql"></a>sys.dm_exec_query_statistics_xml (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Restituisce il piano di esecuzione della query per le richieste in corso. Utilizzare questa DMV per recuperare Showplan XML con statistiche temporanee. 

## <a name="syntax"></a>Sintassi

```
sys.dm_exec_query_statistics_xml(session_id)  
``` 

## <a name="arguments"></a>Argomenti 
*session_id*  
 ID della sessione in cui è in esecuzione il batch da cercare. *session_id* è **smallint**. è possibile ottenere *session_id* dagli oggetti a gestione dinamica seguenti:  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  

## <a name="table-returned"></a>Tabella restituita

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|
|session_id|**smallint**|ID della sessione. Non ammette i valori NULL.|
|request_id|**int**|ID della richiesta. Non ammette i valori NULL.|
|sql_handle|**varbinary(64)**|Token che identifica in modo univoco il batch o stored procedure di cui fa parte la query. Ammette valori Null.|
|plan_handle|**varbinary(64)**|Token che identifica in modo univoco un piano di esecuzione della query per un batch attualmente in esecuzione. Ammette valori Null.|
|query_plan|**xml**|Contiene la rappresentazione Showplan di runtime del piano di esecuzione della query specificato con *plan_handle* contenenti statistiche parziali. La rappresentazione Showplan è in formato XML. Viene generato un piano per ogni batch contenente ad esempio istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, chiamate di stored procedure e chiamate di funzioni definite dall'utente. Ammette valori Null.|

## <a name="remarks"></a>Commenti
Questa funzione di sistema è disponibile a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1. Vedere KB [3190871](https://support.microsoft.com/help/3190871)

Questa funzione di sistema funziona con l'infrastruttura di profilatura delle statistiche di esecuzione di query **standard** e **Lightweight** . Per altre informazioni, vedere [Infrastruttura di profilatura query](../../relational-databases/performance/query-profiling-infrastructure.md).  

Nelle condizioni seguenti non viene restituito alcun output Showplan nella colonna **query_plan** della tabella restituita per **sys.dm_exec_query_statistics_xml**:  
  
-   Se il piano di query che corrisponde al *session_id* specificato non è più in esecuzione, la colonna **query_plan** della tabella restituita è null. Questa condizione, ad esempio, può verificarsi se si verifica un ritardo tra il momento in cui l'handle del piano è stato acquisito e il momento in cui è stato utilizzato con **sys.dm_exec_query_statistics_xml**.  
    
A causa di una limitazione del numero di livelli annidati consentiti nel tipo di dati **XML** , **sys.dm_exec_query_statistics_xml** non può restituire piani di query che soddisfino o superino 128 livelli di elementi annidati. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa condizione impediva il completamento del piano di query e generava l'errore 6335. In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 e versioni successive, la colonna **QUERY_PLAN** restituisce null.   

## <a name="permissions"></a>Autorizzazioni  
In è [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiesta `VIEW SERVER STATE` l'autorizzazione per il server.  
Nei [!INCLUDE[ssSDS](../../includes/sssds-md.md)] livelli Premium, richiede l' `VIEW DATABASE STATE` autorizzazione nel database. Nei [!INCLUDE[ssSDS](../../includes/sssds-md.md)] livelli standard e Basic, richiede l' **amministratore del server** o un account **amministratore Azure Active Directory** .

## <a name="examples"></a>Esempi  
  
### <a name="a-looking-at-live-query-plan-and-execution-statistics-for-a-running-batch"></a>R. Analisi delle statistiche di esecuzione e del piano di query in tempo reale per un batch in esecuzione  
 Nell'esempio seguente viene eseguita una query **sys.dm_exec_requests** per trovare la query interessante e copiarne l'oggetto `session_id` dall'output.  
  
```sql  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 Per ottenere il piano di query e le statistiche di esecuzione in tempo reale, usare la funzione copiata `session_id` con la funzione di sistema **sys.dm_exec_query_statistics_xml**.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_query_statistics_xml(< copied session_id >);  
GO  
```   

 O combinato per tutte le richieste in esecuzione.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_requests
CROSS APPLY sys.dm_exec_query_statistics_xml(session_id);  
GO  
```   
  
## <a name="see-also"></a>Vedere anche
  [Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

