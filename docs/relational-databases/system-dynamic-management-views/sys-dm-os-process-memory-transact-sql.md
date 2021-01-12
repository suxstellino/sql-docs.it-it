---
description: sys.dm_os_process_memory (Transact-SQL)
title: sys.dm_os_process_memory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_process_memory_TSQL
- dm_os_process_memory_TSQL
- dm_os_process_memory
- sys.dm_os_process_memory
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_process_memory dynamic management view
ms.assetid: e838130c-95d4-4605-9e3b-eb0ab71cd250
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 53471004b1e3349ab49e15486479d55961f15fc1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098901"
---
# <a name="sysdm_os_process_memory-transact-sql"></a>sys.dm_os_process_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  La maggior parte delle allocazioni di memoria attribuite allo spazio di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono controllati tramite interfacce che consentono la registrazione e la contabilità delle allocazioni. Tuttavia, le allocazioni di memoria possono essere eseguite nello spazio degli indirizzi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ignora le routine interne di gestione memoria. I valori sono ottenuti tramite chiamate al sistema operativo di base. Non vengono modificati da metodi interni a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tranne quando vengono modificati per allocazioni di pagine bloccate o di grandi dimensioni.  
  
 Tutti i valori restituiti che indicano dimensioni della memoria sono espressi in kilobyte (KB). Il **total_virtual_address_space_reserved_kb** di colonna è un duplicato di **virtual_memory_in_bytes** da **sys.dm_os_sys_info**.  
  
 Nella tabella seguente è inclusa un'immagine completa dello spazio degli indirizzi di processo.  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_process_memory**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**physical_memory_in_use_kb**|**bigint**|Indica il working set del processo in KB, come riportato dal sistema operativo, nonché le allocazioni registrate utilizzando API per pagine di grandi dimensioni. Non ammette i valori NULL.|  
|**large_page_allocations_kb**|**bigint**|Indica la memoria fisica allocata utilizzando API per pagine di grandi dimensioni. Non ammette i valori NULL.|  
|**locked_page_allocations_kb**|**bigint**|Indica le pagine di memoria bloccate nella memoria. Non ammette i valori NULL.|  
|**total_virtual_address_space_kb**|**bigint**|Indica le dimensioni totali della parte della modalità utente dello spazio degli indirizzi virtuali. Non ammette i valori NULL.|  
|**virtual_address_space_reserved_kb**|**bigint**|Indica la quantità totale di spazio degli indirizzi virtuali riservato dal processo. Non ammette i valori NULL.|  
|**virtual_address_space_committed_kb**|**bigint**|Indica la quantità di spazio degli indirizzi virtuali riservato di cui è stato eseguito il commit o il mapping a pagine fisiche. Non ammette i valori NULL.|  
|**virtual_address_space_available_kb**|**bigint**|Indica la quantità di spazio degli indirizzi virtuali attualmente libera. Non ammette i valori NULL.<br /><br /> **Nota:** Possono esistere aree gratuite più piccole della granularità di allocazione. Tali aree non sono disponibili per le allocazioni.|  
|**page_fault_count**|**bigint**|Indica il numero di errori di pagina causati dal processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non ammette i valori NULL.|  
|**memory_utilization_percentage**|**int**|Specifica la percentuale di memoria di cui è stato eseguito il commit nel working set. Non ammette i valori NULL.|  
|**available_commit_limit_kb**|**bigint**|Indica la quantità di memoria disponibile per il commit da parte del processo. Non ammette i valori NULL.|  
|**process_physical_memory_low**|**bit**|Indica che il processo risponde a una notifica di memoria fisica insufficiente. Non ammette i valori NULL.|  
|**process_virtual_memory_low**|**bit**|Indica che è stata rilevata una condizione di memoria virtuale insufficiente. Non ammette i valori NULL.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


