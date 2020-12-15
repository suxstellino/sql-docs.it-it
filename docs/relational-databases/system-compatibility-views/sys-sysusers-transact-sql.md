---
description: sys.sysusers (Transact-SQL)
title: Utenti sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysusers
- sys.sysusers_TSQL
- sysusers_TSQL
- sysusers
dev_langs:
- TSQL
helpviewer_keywords:
- sysusers system table
- sys.sysusers compatibility view
ms.assetid: 5f0e6a8d-c983-44f6-97e9-aab5bff67d18
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a38e8fd5593228ca831c39add1942ce6b0b6621
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97428397"
---
# <a name="syssysusers-transact-sql"></a>sys.sysusers (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni [!INCLUDE[msCoName](../../includes/msconame-md.md)] utente di Windows, gruppo di Windows, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utente o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ruolo nel database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**UID**|**smallint**|ID utente, univoco all'interno del database.<br /><br /> 1 = Proprietario del database.<br /><br /> Causa un errore di overflow o restituisce NULL se il numero di utenti e ruoli è maggiore di 32.767.|  
|**Stato**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**nome**|**sysname**|Nome utente o di gruppo, univoco all'interno del database.|  
|**SID**|**varbinary(85)**|Identificatore di sicurezza per la voce specificata.|  
|**ruoli**|**varbinary(2048)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**CreateDate**|**datetime**|Data in cui è stato aggiunto l'account.|  
|**updateDate**|**datetime**|Data dell'ultima modifica dell'account.|  
|**altuid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Causa un errore di overflow o restituisce NULL se il numero di utenti e ruoli è maggiore di 32.767.|  
|**password**|**varbinary(256)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**gid**|**smallint**|ID del gruppo a cui appartiene l'utente. Se **UID** corrisponde a **GID**, questa voce definisce un gruppo. Causa un errore di overflow o restituisce NULL se il numero combinato di gruppi e utenti è maggiore di 32.767.|  
|**environ**|**varchar(255)**|Riservato.|  
|**HasDBAccess**|**int**|1 = L'account ha accesso al database.|  
|**islogin**|**int**|1 = L'account è un gruppo di Windows, un utente di Windows oppure un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con un account di accesso.|  
|**isntname**|**int**|1 = L'account è un utente o un gruppo di Windows.|  
|**isntgroup**|**int**|1 = L'account è un gruppo di Windows.|  
|**isntuser**|**int**|1 = L'account è un utente di Windows.|  
|**issqluser**|**int**|1 = L'account è un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isaliased**|**int**|1 = L'account è assegnato come alias a un altro utente.|  
|**issqlrole**|**int**|1 = L'account è un ruolo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isapprole**|**int**|1 = L'account è un ruolo applicazione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
