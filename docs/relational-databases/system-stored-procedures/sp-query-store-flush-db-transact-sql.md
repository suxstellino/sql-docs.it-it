---
description: sp_query_store_flush_db (Transact-SQL)
title: sp_query_store_flush_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_query_store_flush_db_TSQL
- sys.sp_query_store_flush_db_TSQL
- sp_query_store_flush_db
- sys.sp_query_store_flush_db
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_query_store_flush_db
- sp_query_store_flush_db
ms.assetid: 580c03ae-57fc-4562-a6bb-5ec89521e38c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9e617ef50dde2053d5151cf7b23e915d64e73b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199538"
---
# <a name="sp_query_store_flush_db-transact-sql"></a>sp_query_store_flush_db (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Scarica la parte in memoria dei dati del Query Store su disco.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_query_store_flush_db [;]  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **ALTER** per il database.
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene svuotata la parte in memoria dei dati del Query Store su disco.  
  
```  
EXEC sp_query_store_flush_db;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_query_store_force_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_query &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [sp_query_store_unforce_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_reset_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [Viste del catalogo di Query Store &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
  
  
