---
description: sys.dm_exec_compute_node_errors (Transact-SQL)
title: sys.dm_exec_compute_node_errors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.DM_EXEC_COMPUTE_NODE_ERRORS_TSQL
- DM_EXEC_COMPUTE_NODE_ERRORS
- DM_EXEC_COMPUTE_NODE_ERRORS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
- PolyBase, views
- dm_exec_compute_node_errors
- sys.dm_exec_compute_node_errors management view
ms.assetid: 9a03c039-70e4-4974-95d8-d3fa45984ffb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6f2de93073776522947a227f60ef9aa3fd894323
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094091"
---
# <a name="sysdm_exec_compute_node_errors-transact-sql"></a>sys.dm_exec_compute_node_errors (Transact-SQL)

[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Restituisce gli errori che si verificano nei nodi di calcolo di base.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|error_id|`nvarchar(36)`|ID numerico univoco associato all'errore.|Univoco in tutti gli errori di query nel sistema|  
|source|`nvarchar(255)`|Descrizione del processo o del thread di origine||  
|type|`nvarchar(255)`|Tipo di errore.||  
|create_time|`datetime`|Data e ora dell'occorrenza dell'errore||  
|compute_node_id|`int`|Identificatore del nodo di calcolo specifico|Vedere compute_node_id di [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)|  
|rexecution_id|`nvarchar(36)`|Identificatore della query di polibase, se disponibile.||  
|spid|`int`|Identificatore della sessione di SQL Server||  
|thread_id|`int`|Identificatore numerico del thread in cui si è verificato l'errore.||  
|dettagli|nvarchar(4000)|Descrizione completa dei dettagli dell'errore.||
|compute_pool_id|`int`|Identificatore univoco per il pool.|

  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
