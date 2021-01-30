---
description: sys.dm_tran_session_transactions (Transact-SQL)
title: sys.dm_tran_session_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7a48692345bc7f5466931edebf969e9b247c559
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99133355"
---
# <a name="sysdm_tran_session_transactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni di correlazione per le sessioni e le transazioni associate.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_tran_session_transactions**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|session_id|**int**|ID della sessione nella quale viene eseguita la transazione.|  
|transaction_id|**bigint**|ID della transazione.|  
|transaction_descriptor|**binario (8)**|Identificatore di transazione utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durante la comunicazione con il driver client.|  
|enlist_count|**int**|Numero di richieste attive nella sessione della transazione.|  
|is_user_transaction|**bit**|1 = La transazione è stata iniziata da una richiesta utente.<br /><br /> 0 = Transazione di sistema.|  
|is_local|**bit**|1 = Transazione locale.<br /><br /> 0 = Transazione distribuita o transazione di sessione associata integrata.|  
|is_enlisted|**bit**|1 = Transazione distribuita integrata.<br /><br /> 0 = Non è una transazione distribuita integrata.|  
|is_bound|**bit**|1 = La transazione è attiva nella sessione tramite sessioni associate.<br /><br /> 0 = La transazione non è attiva nella sessione tramite sessioni associate.|  
|open_transaction_count||Numero di transazioni aperte per ogni sessione.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="remarks"></a>Commenti  
 È possibile che una transazione venga eseguita in più di una sessione tramite sessioni associate e transazioni distribuite. In tali casi, sys.dm_tran_session_transactions visualizzerà più righe per lo stesso transaction_id, una per ogni sessione in cui viene eseguita la transazione.  
  
 Eseguendo più richieste in modalità autocommit e utilizzando MARS (Multiple Active Result Sets), è possibile che vi siano più transazioni attive in una singola sessione. In tali casi, sys.dm_tran_session_transactions visualizzerà più righe per lo stesso transaction_id, una per ogni transazione eseguita nella sessione.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative alle transazioni &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


