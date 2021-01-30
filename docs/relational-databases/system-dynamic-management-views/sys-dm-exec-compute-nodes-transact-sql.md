---
description: sys.dm_exec_compute_nodes (Transact-SQL)
title: sys.dm_exec_compute_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_COMPUTE_NODES_TSQL
- DM_EXEC_COMPUTE_NODES
- SYS.DM_EXEC_COMPUTE_NODES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_compute_nodes management view
- PolyBase, views
- PolyBase management views
- dm_exec_compute_nodes management view
ms.assetid: 0de4b7a4-401f-4e2d-9ab0-c54587e05154
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 223878cdbfacd3c8bc754126b291b5d6750ccb93
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160072"
---
# <a name="sysdm_exec_compute_nodes-transact-sql"></a>sys.dm_exec_compute_nodes (Transact-SQL)

[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Include informazioni sui nodi utilizzati con la gestione dati di base. Viene elencata una riga per ogni nodo.  
  
 Usare questa DMV per visualizzare l'elenco di tutti i nodi nel cluster con scalabilità orizzontale con il ruolo, il nome e l'indirizzo IP.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|compute_node_id|**int**|ID numerico univoco associato al nodo. Chiave per questa visualizzazione.|Univoco nel cluster con scalabilità orizzontale indipendentemente dal tipo.|  
|tipo|**nvarchar(32)**|Tipo del nodo.|' COMPUTE ',' HEAD '|  
|name|**nvarchar(32)**|Nome logico del nodo.|Qualsiasi stringa di lunghezza appropriata.|  
|address|**nvarchar(32)**|Indirizzo IP del nodo.|Intervallo indirizzi IP|  
  
## <a name="see-also"></a>Vedere anche  
 [Risoluzione dei problemi di polibase con viste a gestione dinamica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
