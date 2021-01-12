---
description: sys.dm_db_xtp_transactions (Transact-SQL)
title: sys.dm_db_xtp_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_transactions
- sys.dm_db_xtp_transactions_TSQL
- dm_db_xtp_transactions
- dm_db_xtp_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_transactions dynamic management view
ms.assetid: 5c1a0a7a-e851-4b6f-8dfd-c9655fbf5a51
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d18ccebebcb4803f42c442b45844964b5c0c19d0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099856"
---
# <a name="sysdm_db_xtp_transactions-transact-sql"></a>sys.dm_db_xtp_transactions (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Indica le transazioni attive nel motore di database OLTP in memoria.  
  
 Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
    
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|xtp_transaction_id|**bigint**|ID interno della transazione in Gestione transazioni XTP.|  
|transaction_id|**bigint**|ID transazione. Join con ID transazione negli altri DMV correlati alla transazione, ad esempio sys.dm_tran_active_transactions.<br /><br /> 0 per le transazioni solo XTP, ad esempio le transazioni avviate da stored procedure compilate in modo nativo.|  
|session_id|**smallint**|Identificatore della sessione che esegue questa transazione. Join con sys.dm_exec_sessions.|  
|begin_tsn|**bigint**|Numero di serie di inizio della transazione.|  
|end_tsn|**bigint**|Numero di serie di fine della transazione.|  
|state|**int**|Stato della transazione:<br /><br /> 0=ACTIVE<br /><br /> 1=COMMITTED<br /><br /> 2=ABORTED<br /><br /> 3=VALIDATING|  
|state_desc|**nvarchar**|Descrizione dello stato della transazione.|  
|result|**int**|Risultato della transazione. Di seguito sono indicati i valori possibili.<br /><br /> 0 - IN PROGRESS<br /><br /> 1 - SUCCESS<br /><br /> 2 - ERROR<br /><br /> 3 - COMMIT DEPENDENCY<br /><br /> 4 - VALIDATION FAILED (RR)<br /><br /> 5 - VALIDATION FAILED (SR)<br /><br /> 6 - ROLLBACK|  
|result_desc|**nvarchar**|Risultato della transazione. Di seguito sono indicati i valori possibili.<br /><br /> IN PROGRESS<br /><br /> SUCCESS<br /><br /> ERRORE<br /><br /> COMMIT DEPENDENCY<br /><br /> VALIDATION FAILED (RR)<br /><br /> VALIDATION FAILED (SR)<br /><br /> ROLLBACK|  
|last_error|**int**|Solo per uso interno.|  
|is_speculative|**bit**|Solo per uso interno.|  
|is_prepared|**bit**|Solo per uso interno.|  
|is_delayed_durability|**bit**|Solo per uso interno.|  
|memory_address|**varbinary**|Solo per uso interno.|  
|database_address|**varbinary**|Solo per uso interno.|  
|thread_id|**int**|Solo per uso interno.|  
|read_set_row_count|**int**|Solo per uso interno.|  
|write_set_row_count|**int**|Solo per uso interno.|  
|scan_set_count|**int**|Solo per uso interno.|  
|savepoint_garbage_count|**int**|Solo per uso interno.|  
|log_bytes_required|**bigint**|Solo per uso interno.|  
|count_of_allocations|**int**|Solo per uso interno.|  
|allocated_bytes|**int**|Solo per uso interno.|  
|reserved_bytes|**int**|Solo per uso interno.|  
|commit_dependency_count|**int**|Solo per uso interno.|  
|commit_dependency_total_attempt_count|**int**|Solo per uso interno.|  
|scan_area|**int**|Solo per uso interno.|  
|scan_area_desc|**nvarchar**|Solo per uso interno.|  
|scan_location|**int**|Solo per uso interno.|  
|dependent_1_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_2_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_3_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_4_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_5_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_6_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_7_address|**varbinary (8)**|Solo per uso interno.|  
|dependent_8_address|**varbinary (8)**|Solo per uso interno.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
