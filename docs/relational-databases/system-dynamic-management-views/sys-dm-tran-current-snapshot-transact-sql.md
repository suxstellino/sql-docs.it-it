---
description: sys.dm_tran_current_snapshot (Transact-SQL)
title: sys.dm_tran_current_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_current_snapshot_TSQL
- dm_tran_current_snapshot
- dm_tran_current_snapshot_TSQL
- sys.dm_tran_current_snapshot
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_current_snapshot dynamic management view
ms.assetid: 7509d595-c0e1-4237-a5ac-b41ad934544c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c0488d01e23a69019734f41587b4aa6bc5e8af6c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095101"
---
# <a name="sysdm_tran_current_snapshot-transact-sql"></a>sys.dm_tran_current_snapshot (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una tabella virtuale che visualizza tutte le transazioni attive al momento dell'avvio della transazione snapshot corrente. Se la transazione corrente non è una transazione snapshot, questa funzione non restituisce alcuna riga. **sys.dm_tran_current_snapshot** è simile a **sys.dm_tran_transactions_snapshot**, ad eccezione del fatto che **sys.dm_tran_current_snapshot** restituisce solo le transazioni attive per la transazione snapshot corrente.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_tran_current_snapshot**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_tran_current_snapshot  
```  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Numero di sequenza della transazione attiva.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato uno scenario di test in cui quattro transazioni simultanee, ognuna identificata da un numero di sequenza della transazione (XSN), vengono eseguite in un database con le opzioni ALLOW_SNAPSHOT_ISOLATION e READ_COMMITTED_SNAPSHOT impostate su ON. Vengono eseguite le transazioni seguenti:  
  
-   XSN-57 è un'operazione di aggiornamento con isolamento serializzabile.  
  
-   XSN-58 è uguale a XSN-57.  
  
-   XSN-59 è un'operazione di selezione con isolamento dello snapshot.  
  
-   XSN-60 è uguale a XSN-59.  
  
 Nell'ambito di XSN-59 viene eseguita la query seguente.  
  
```  
SELECT   
    transaction_sequence_num  
  FROM sys.dm_tran_current_snapshot;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num  
------------------------  
57  
58  
```  
  
 I risultati indicano che le transazioni XSN-57 e XSN-58 erano attive all'avvio della transazione snapshot XSN-59. Questi stessi risultati sono persistenti fino al termine della transazione snapshot, anche in seguito a commit o rollback di XSN-57 e XSN-58.  
  
 La stessa query viene eseguita nell'ambito di XSN-60.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num  
------------------------  
57  
58  
59  
```  
  
 L'output per XSN-60 include le stesse transazioni riportate per XSN-59, ma anche la transazione XSN-59 che era attiva all'avvio di XSN-60.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative alle transazioni &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


