---
description: sys.dm_tran_database_transactions (Transact-SQL)
title: sys.dm_tran_database_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_database_transactions
- sys.dm_tran_database_transactions_TSQL
- dm_tran_database_transactions_TSQL
- sys.dm_tran_database_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_database_transactions dynamic management view
ms.assetid: 82a44295-4cbc-4a5b-891a-8ebaf307b8f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9932a3f57c02307699ff75d53efaf81f25594e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097500"
---
# <a name="sysdm_tran_database_transactions-transact-sql"></a>sys.dm_tran_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni sulle transazioni a livello di database.  
  
> [!NOTE]  
>  Per chiamare questa DMV da [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_tran_database_transactions**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|ID della transazione a livello di istanza, non a livello di database. L'ID è univoco solo in tutti i database all'interno di un'istanza specifica, ma non tra tutte le istanze del server.|  
|database_id|**int**|ID del database associato alla transazione.|  
|database_transaction_begin_time|**datetime**|Ora in cui il database viene coinvolto nella transazione. In particolare, si tratta dell'ora del primo record di log nel database per la transazione.|  
|database_transaction_type|**int**|1 = Transazione di lettura/scrittura<br /><br /> 2 = Transazione di sola lettura<br /><br /> 3 = Transazione di sistema|  
|database_transaction_state|**int**|1 = La transazione non è stata inizializzata.<br /><br /> 3 = La transazione è stata inizializzata ma non ha generato alcun record di log.<br /><br /> 4 = La transazione ha generato record di log.<br /><br /> 5 = La transazione è stata preparata.<br /><br /> 10 = È stato eseguito il commit della transazione.<br /><br /> 11 = È stato eseguito il rollback della transazione.<br /><br /> 12 = L'esecuzione del commit della transazione è in corso. (Il record di log viene generato ma non è stato materializzato o reso permanente).|  
|database_transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_log_record_count|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di record di log generati nel database per la transazione.|  
|database_transaction_replicate_record_count|**int**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di record di log generati nel database per la transazione replicata.|  
|database_transaction_log_bytes_used|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di byte finora utilizzati nel log del database per la transazione.|  
|database_transaction_log_bytes_reserved|**bigint**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di byte riservati all'utilizzo nel log del database per la transazione.|  
|database_transaction_log_bytes_used_system|**int**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di byte finora utilizzati nel log del database per le transazioni di sistema per conto della transazione.|  
|database_transaction_log_bytes_reserved_system|**int**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di byte riservati per l'utilizzo nel log del database per le transazioni di sistema per conto della transazione.|  
|database_transaction_begin_lsn|**numeric(25,0)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di sequenza del file di log (LSN) del record di inizio per la transazione nel log del database.|  
|database_transaction_last_lsn|**numeric(25,0)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> LSN del log registrato più di recente per la transazione nel log del database.|  
|database_transaction_most_recent_savepoint_lsn|**numeric(25,0)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> LSN del punto di salvataggio più recente per la transazione nel log del database.|  
|database_transaction_commit_lsn|**numeric(25,0)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> LSN del record di log del commit per la transazione nel log del database.|  
|database_transaction_last_rollback_lsn|**numeric(25,0)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> LSN fino al quale è stato eseguito il rollback più recente. Se non si è verificato alcun rollback, il valore è MaxLSN.|  
|database_transaction_next_undo_lsn|**numeric(25,0)**|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> LSN del record successivo da annullare.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="see-also"></a>Vedere anche  
 [sys.dm_tran_active_transactions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-transactions-transact-sql.md)   
 [sys.dm_tran_session_transactions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative alle transazioni &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


