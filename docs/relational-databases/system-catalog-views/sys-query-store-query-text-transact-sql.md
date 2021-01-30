---
description: sys.query_store_query_text (Transact-SQL)
title: sys.query_store_query_text (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.QUERY_STORE_QUERY_TEXT
- QUERY_STORE_QUERY_TEXT
- SYS.QUERY_STORE_QUERY_TEXT_TSQL
- QUERY_STORE_QUERY_TEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_store_query_text catalog view
- query_store_query_text catalog view
ms.assetid: f7032fa0-7c16-4492-bb82-685806c63a8c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a700c3825e835f85c90f3f9116853861dfd16f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204523"
---
# <a name="sysquery_store_query_text-transact-sql"></a>sys.query_store_query_text (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene il [!INCLUDE[tsql](../../includes/tsql-md.md)] testo e l'handle SQL della query.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**query_text_id**|**bigint**|Chiave primaria.|  
|**query_sql_text**|**nvarchar(max)**|Testo SQL della query, fornito dall'utente. Include spazi vuoti, suggerimenti e commenti. I commenti e gli spazi prima e dopo il testo della query vengono ignorati. I commenti e gli spazi all'interno del testo non vengono ignorati.|  
|**statement_sql_handle**|**vabinary (64)**|Handle SQL della singola query.|  
|**is_part_of_encrypted_module**|**bit**|Il testo della query fa parte di un modulo crittografato.<br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|
|**has_restricted_text**|**bit**|Il testo della query contiene una password o altre parole senza menzioni.<br/>**Nota:** Analisi delle sinapsi di Azure restituirà sempre zero (0).|
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **View database state** .  
  
## <a name="see-also"></a>Vedere anche  
 [sys.database_query_store_options &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Stored procedure di Archivio query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
