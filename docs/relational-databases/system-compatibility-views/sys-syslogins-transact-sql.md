---
description: sys.syslogins (Transact-SQL)
title: Account di accesso sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syslogins_TSQL
- syslogins
- sys.syslogins
- sys.syslogins_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syslogins compatibility view
- syslogins system table
ms.assetid: 4cb34f17-a4bb-469f-a218-71f074e6308f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: acf9e8ff2592927c8107b1b6bacd02a6de9d3a48
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094160"
---
# <a name="syssyslogins-transact-sql"></a>sys.syslogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni account di accesso.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] alla [versione corrente](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**SID**|**varbinary(85)**|Identificatore di sicurezza.|  
|**Stato**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**CreateDate**|**datetime**|Data in cui l'account di accesso è stato aggiunto.|  
|**updatedate**|**datetime**|Data di aggiornamento dell'account di accesso.|  
|**accdate**|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totcpu**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totio**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**spacelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**timelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resultlimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**nome**|**sysname**|Nome dell'account di accesso dell'utente.|  
|**dbname**|**sysname**|Nome del database predefinito dell'utente quando viene stabilita una connessione.|  
|**password**|**nvarchar(128)**|Restituisce NULL.|  
|**language**|**sysname**|Lingua predefinita dell'utente.|  
|**denylogin**|**int**|1 = L'account di accesso è un utente o un gruppo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] a cui è stato negato l'accesso.|  
|**hasaccess**|**int**|1 = L'account di accesso dispone dell'accesso al server.|  
|**isntname**|**int**|1 = L'account di accesso è un utente o un gruppo di Windows.<br /><br /> 0 = L'account di accesso è un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isntgroup**|**int**|1 = L'account di accesso è un gruppo di Windows.|  
|**isntuser**|**int**|1 = L'account di accesso è un utente di Windows.|  
|**sysadmin**|**int**|1 = l'account di accesso è un membro del ruolo del server **sysadmin** .|  
|**securityadmin**|**int**|1 = l'account di accesso è un membro del ruolo del server **securityadmin** .|  
|**serveradmin**|**int**|1 = l'account di accesso è un membro del ruolo predefinito del server **serveradmin** .|  
|**setupadmin**|**int**|1 = l'account di accesso è un membro del ruolo predefinito del server **setupadmin** .|  
|**processadmin**|**int**|1 = l'account di accesso è un membro del ruolo predefinito del server **processadmin** .|  
|**diskadmin**|**int**|1 = l'account di accesso è un membro del ruolo predefinito del server **diskadmin** .|  
|**dbcreator**|**int**|1 = l'account di accesso è un membro del ruolo predefinito del server **dbcreator** .|  
|**bulkadmin**|**int**|1 = l'account di accesso è un membro del ruolo predefinito del server **bulkadmin** .|  
|**LoginName**|**nvarchar(128)**|Nome dell'account di accesso dell'utente. Disponibile per compatibilità con le versioni precedenti.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
