---
description: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)
title: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 6794e073-0895-4507-aba3-c3545acc843f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a2a23c3fffb8bfffa1cd9343d36dfd4c12bf5f1f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094969"
---
# <a name="sysfn_stmt_sql_handle_from_sql_stmt-transact-sql"></a>sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Ottiene l' **stmt_sql_handle** per un' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione nel tipo di parametrizzazione specificato (semplice o forzata). In questo modo è possibile fare riferimento alle query archiviate nel Query Store usando il **stmt_sql_handle** quando si conosce il testo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.fn_stmt_sql_handle_from_sql_stmt   
(  
    'query_sql_text',   
    [ query_param_type   
) [;]  
```  
  
## <a name="arguments"></a>Argomenti  
 *query_sql_text*  
 Testo della query nell'archivio query di cui si desidera l'handle. *query_sql_text* è di **tipo nvarchar (max)** e non prevede alcun valore predefinito.  
  
 *query_param_type*  
 Tipo di parametro della query. *query_param_type* è di un **tinyint**. I valori possibili sono:  
  
-   NULL-il valore predefinito è 0  
  
-   0 - Nessuno  
  
-   1-utente  
  
-   2-semplice  
  
-   3-forzata  
  
## <a name="columns-returned"></a>Colonne restituite  
 Nella tabella seguente sono elencate le colonne restituite da sys.fn_stmt_sql_handle_from_sql_stmt.  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|**statement_sql_handle**|**varbinary(64)**|Handle SQL.|  
|**query_sql_text**|**nvarchar(max)**|Testo dell' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione.|  
|**query_parameterization_type**|**tinyint**|Tipo di parametrizzazione della query.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **Execute** per il database e l'autorizzazione **Delete** per le viste del catalogo di archivio query.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene eseguita un'istruzione, quindi viene utilizzato `sys.fn_stmt_sql_handle_from_sql_stmt` per restituire l'handle SQL dell'istruzione.  
  
```  
SELECT * FROM sys.databases;   
SELECT * FROM sys.fn_stmt_sql_handle_from_sql_stmt('SELECT * FROM sys.databases', NULL);  
```  
  
 Utilizzare la funzione per correlare Query Store dati con altre viste a gestione dinamica. L'esempio seguente:  
  
```  
SELECT qt.query_text_id, q.query_id, qt.query_sql_text, qt.statement_sql_handle,  
q.context_settings_id, qs.statement_context_id   
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_id  
CROSS APPLY sys.fn_stmt_sql_handle_from_sql_stmt (qt.query_sql_text, null) AS fn_handle_from_stmt  
JOIN sys.dm_exec_query_stats AS qs   
    ON fn_handle_from_stmt.statement_sql_handle = qs.statement_sql_handle;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_query_store_force_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_unforce_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [sp_query_store_remove_query &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [Viste del catalogo di Query Store &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
  
  
