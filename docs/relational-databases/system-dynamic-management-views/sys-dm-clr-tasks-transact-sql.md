---
description: sys.dm_clr_tasks (Transact-SQL)
title: sys.dm_clr_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_tasks
- sys.dm_clr_tasks_TSQL
- dm_clr_tasks
- dm_clr_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_tasks dynamic management view
ms.assetid: 462b9061-09fa-4858-9707-03d6cc19c769
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c16eea33c1c0211b37823ecff1e2ae1131d2a335
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837618"
---
# <a name="sysdm_clr_tasks-transact-sql"></a>sys.dm_clr_tasks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per tutte le attività CLR (Common Language Runtime) in esecuzione. Un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] contenente un riferimento a una routine CLR crea un'attività distinta per l'esecuzione di tutto il codice gestito nel batch. La stessa attività CLR viene utilizzata da più istruzioni nel batch che richiedono l'esecuzione del codice gestito. L'attività CLR è responsabile del mantenimento di oggetti e stato relativi all'esecuzione del codice gestito, nonché delle transizioni tra l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e CLR.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary (8)**|Indirizzo dell'attività CLR.|  
|**sos_task_address**|**varbinary (8)**|Indirizzo dell'attività batch [!INCLUDE[tsql](../../includes/tsql-md.md)] sottostante.|  
|**appdomain_address**|**varbinary (8)**|Indirizzo del dominio applicazione in cui è in esecuzione l'attività.|  
|**state**|**nvarchar(128)**|Stato corrente dell'attività.|  
|**abort_state**|**nvarchar(128)**|Stato corrente dell'interruzione se l'attività è stata annullata. Durante l'interruzione delle attività si possono verificare più stati.|  
|**type**|**nvarchar(128)**|Tipo di attività.|  
|**affinity_count**|**int**|Affinità dell'attività.|  
|**forced_yield_count**|**int**|Numero di volte che l'attività è obbligata a restituire il controllo.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative a Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
