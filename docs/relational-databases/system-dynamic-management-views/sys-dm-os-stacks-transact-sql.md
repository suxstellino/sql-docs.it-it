---
description: sys.dm_os_stacks (Transact-SQL)
title: sys.dm_os_stacks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_stacks
- dm_os_stacks_TSQL
- sys.dm_os_stacks
- sys.dm_os_stacks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_stacks dynamic management view
ms.assetid: a69b06c4-28f0-4535-8fa1-9f132db4d916
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e5c42d0a82ce7726883e46f2e633aacc624e4db
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099721"
---
# <a name="sysdm_os_stacks-transact-sql"></a>sys.dm_os_stacks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Questa vista a gestione dinamica viene utilizzata internamente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per:  
  
-   Tenere traccia dei dati di debug quali le allocazioni in sospeso.  
  
-   Utilizzare o convalidare la logica utilizzata dai componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dove il componente specifico presume che sia stata eseguita una determinata chiamata.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**stack_address**|**varbinary (8)**|Indirizzo univoco per l'allocazione di stack. Non ammette i valori Null.|  
|**frame_index**|**int**|Ogni riga rappresenta una chiamata di funzione che, se ordinata in ordine crescente in base all'indice del frame per una particolare **stack_address**, restituisce lo stack di chiamate completo. Non ammette i valori Null.|  
|**frame_address**|**varbinary (8)**|Indirizzo della chiamata di funzione. Non ammette i valori Null.|  
  
## <a name="remarks"></a>Osservazioni  
 **sys.dm_os_stacks** richiede che i simboli del server e di altri componenti siano presenti nel server per visualizzare correttamente le informazioni.  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   


## <a name="see-also"></a>Vedere anche  
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
