---
description: sys.dm_exec_query_parallel_workers (Transact-SQL)
title: sys.dm_exec_query_parallel_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_parallel_workers_TSQL
- dm_exec_query_parallel_workers
- sys.dm_exec_query_parallel_workers_TSQL
- sys.dm_exec_query_parallel_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_parallel_workers dynamic management view
ms.assetid: 1d72cef1-22d8-4ae0-91db-6694fe918c9f
author: pelopes
ms.author: pelopes
manager: ajayj
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 30ea6f1642da78754560b26a8772c33ab341d2b4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477242"
---
# <a name="sysdm_exec_query_parallel_workers-transact-sql"></a>sys.dm_exec_query_parallel_workers (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni sulla disponibilità del ruolo di lavoro per nodo.  
  
|Nome|Tipo di dati|Descrizione|  
|----------|---------------|-----------------|  
|**node_id**|**int**|ID del nodo NUMA.|  
|**scheduler_count**|**int**|Numero di utilità di pianificazione in questo nodo.|  
|**max_worker_count**|**int**|Numero massimo di ruoli di lavoro per le query parallele.|  
|**reserved_worker_count**|**int**|Numero di ruoli di lavoro riservati da query parallele, più il numero di ruoli di lavoro principali utilizzati da tutte le richieste.| 
|**free_worker_count**|**int**|Numero di thread di lavoro disponibili per le attività.<br /><br />**Nota:** ogni richiesta in ingresso utilizza almeno un ruolo di lavoro, che viene sottratto dal numero di thread di lavoro liberi.  È possibile che il numero di thread di lavoro libero possa essere un numero negativo in un server con carico elevato.| 
|**used_worker_count**|**int**|Numero di thread di lavoro utilizzati da query parallele.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="examples"></a>Esempi  
  
### <a name="a-viewing-current-parallel-worker-availability"></a>R. Visualizzazione della disponibilità corrente del ruolo di lavoro parallelo  

```sql 
SELECT * FROM sys.dm_exec_query_parallel_workers;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_os_workers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md)
