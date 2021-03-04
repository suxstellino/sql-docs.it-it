---
description: sys.dm_io_pending_io_requests (Transact-SQL)
title: sys.dm_io_pending_io_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_pending_io_requests
- dm_io_pending_io_requests
- dm_io_pending_io_requests_TSQL
- sys.dm_io_pending_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_pending_io_requests dynamic management view
ms.assetid: d1fb46dd-5c74-4c04-9ecf-8934b1bedb5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 809592c892ba6f786b852b10ec4c19239218cc50
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837640"
---
# <a name="sysdm_io_pending_io_requests-transact-sql"></a>sys.dm_io_pending_io_requests (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni richiesta di I/O in sospeso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_io_pending_io_requests**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**io_completion_request_address**|**varbinary (8)**|Indirizzo di memoria della richiesta di I/O. Non ammette i valori Null.|  
|**io_type**|**nvarchar(60)**|Tipo di richiesta di I/O in sospeso. Non ammette i valori Null.|  
|**io_pending_ms_ticks**|**bigint**|Solo per uso interno. Non ammette i valori Null.| 
|**io_pending**|**int**|Indica se la richiesta di I/O è in sospeso o è stata completata da Windows. Una richiesta di I/O può essere ancora in sospeso anche quando Windows ha completato la richiesta, ma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non ha ancora eseguito uno scambio di contesto per l'elaborazione della richiesta di I/O e la relativa rimozione dall'elenco. Non ammette i valori Null.|  
|**io_completion_routine_address**|**varbinary (8)**|Funzione interna da chiamare quando la richiesta di I/O viene completata. Ammette i valori Null.|  
|**io_user_data_address**|**varbinary (8)**|Solo per uso interno. Ammette i valori Null.|  
|**scheduler_address**|**varbinary (8)**|Utilità di pianificazione sulla quale è stata eseguita la richiesta di I/O. La richiesta di I/O verrà visualizzata nell'elenco delle richieste di I/O in sospeso dell'utilità di pianificazione. Per ulteriori informazioni, vedere [sys.dm_os_schedulers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). Non ammette i valori Null.|  
|**io_handle**|**varbinary (8)**|Handle di file utilizzato nella richiesta di I/O. Ammette i valori Null.|  
|**io_offset**|**bigint**|Offset della richiesta di I/O. Non ammette i valori Null.|  
|**io_handle_path**|**nvarchar(256)**| Percorso del file utilizzato nella richiesta di I/O. Ammette i valori Null.|
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica correlate a I O &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
