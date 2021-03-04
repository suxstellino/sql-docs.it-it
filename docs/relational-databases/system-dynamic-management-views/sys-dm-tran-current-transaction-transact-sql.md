---
description: sys.dm_tran_current_transaction (Transact-SQL)
title: sys.dm_tran_current_transaction (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_current_transaction
- sys.dm_tran_current_transaction_TSQL
- dm_tran_current_transaction_TSQL
- dm_tran_current_transaction
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_current_transaction dynamic management view
ms.assetid: 75d5697d-b390-4963-99b8-fa0b4244a40c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba8f53f07f9a6ab74f4224b9e1846825e12ac7e7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839068"
---
# <a name="sysdm_tran_current_transaction-transact-sql"></a>sys.dm_tran_current_transaction (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga singola che visualizza informazioni sullo stato della transazione nella sessione corrente.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_tran_current_transaction**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.dm_tran_current_transaction  
```  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|ID transazione dello snapshot corrente.|  
|**transaction_sequence_num**|**bigint**|Numero di sequenza della transazione che genera la versione del record.|  
|**transaction_is_snapshot**|**bit**|Stato di isolamento dello snapshot. Questo valore è 1 se la transazione viene avviata con isolamento dello snapshot. In caso contrario, il valore è 0.|  
|**first_snapshot_sequence_num**|**bigint**|Numero di sequenza più basso delle transazioni attive nel momento in cui è stato eseguito uno snapshot. Al momento dell'esecuzione, una transazione snapshot esegue uno snapshot di tutte le transazioni attive in quel momento. Per le transazioni non snapshot, il valore di questa colonna è 0.|  
|**last_transaction_sequence_num**|**bigint**|Numero di sequenza globale. Questo valore rappresenta l'ultimo numero di sequenza della transazione generato dal sistema.|  
|**first_useful_sequence_num**|**bigint**|Numero di sequenza globale. Questo valore rappresenta il numero di sequenza della transazione meno recente con versioni di riga che devono essere mantenute nell'archivio delle versioni. Le versioni di riga create da transazioni precedenti possono essere rimosse.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene utilizzato uno scenario di test in cui quattro transazioni simultanee, ognuna identificata da un numero di sequenza della transazione (XSN), vengono eseguite in un database con le opzioni ALLOW_SNAPSHOT_ISOLATION e READ_COMMITTED_SNAPSHOT impostate su ON. Vengono eseguite le transazioni seguenti:  
  
-   XSN-57 è un'operazione di aggiornamento con isolamento serializzabile.  
  
-   XSN-58 è uguale a XSN-57.  
  
-   XSN-59 è un'operazione di selezione con isolamento dello snapshot.  
  
-   XSN-60 è uguale a XSN-59.  
  
 Nell'ambito di ogni transazione viene eseguita la query seguente.  
  
```  
SELECT   
    transaction_id  
   ,transaction_sequence_num  
   ,transaction_is_snapshot  
   ,first_snapshot_sequence_num  
   ,last_transaction_sequence_num  
   ,first_useful_sequence_num  
  FROM sys.dm_tran_current_transaction;  
```  
  
 Set di risultati per XSN-59:  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9387                 59                       1                         
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
57                               61                        
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 L'output indica che XSN-59 è una transazione snapshot che utilizza XSN-57 come prima transazione attiva all'avvio di XSN-59. XSN-59 leggerà pertanto i dati con commit eseguito da transazioni dotate di numero di sequenza della transazione inferiore a XSN-57.  
  
 Set di risultati per XSN-57:  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9295                 57                       0  
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
NULL                        61  
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 Poiché XSN-57 non è una transazione snapshot, `first_snapshot_sequence_num` è `NULL`.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative alle transazioni &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
