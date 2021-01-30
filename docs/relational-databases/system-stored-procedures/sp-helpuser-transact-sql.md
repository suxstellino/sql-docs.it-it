---
description: sp_helpuser (Transact-SQL)
title: sp_helpuser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpuser
- sp_helpuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpuser
ms.assetid: 9c70b41d-ef4c-43df-92da-bd534c287ca1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ce2bf386978412c6b1409b31e371f8caa15eccdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103075"
---
# <a name="sp_helpuser-transact-sql"></a>sp_helpuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulle entità a livello di database nel database corrente.  
  
> [!IMPORTANT]  
>  **sp_helpuser** non restituisce informazioni sulle entità a protezione diretta introdotte in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . In alternativa, usare [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpuser [ [ @name_in_db = ] 'security_account' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name_in_db = ] 'security_account'` Nome dell'utente del database o del ruolo del database nel database corrente. *security_account* deve esistere nel database corrente. *security_account* è di **tipo sysname** e il valore predefinito è null. Se non si specifica *security_account* , **sp_helpuser** restituisce informazioni su tutte le entità di database.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Nella tabella seguente viene illustrato il set di risultati quando non viene specificato né un account utente né un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utente di Windows o per *security_account*.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**UserName**|**sysname**|Utenti nel database corrente.|  
|**RoleName**|**sysname**|Ruoli a cui appartiene il **nome utente** .|  
|**LoginName**|**sysname**|Account di accesso del **nome utente**.|  
|**DefDBName**|**sysname**|**Nome utente** predefinito del database.|  
|**DefSchemaName**|**sysname**|Schema predefinito dell'utente del database.|  
|**UserID**|**smallint**|ID del **nome utente** nel database corrente.|  
|**SID**|**smallint**|ID di sicurezza dell'utente (SID)|  
  
 Nella tabella seguente viene illustrato il set di risultati quando non si specifica alcun account utente ed esistono alias nel database corrente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Account di accesso associati come alias agli utenti del database corrente.|  
|**UserNameAliasedTo**|**sysname**|Nome utente nel database corrente associato come alias all'account utente.|  
  
 Nella tabella seguente viene illustrato il set di risultati quando si specifica un ruolo per *security_account*.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Role_name**|**sysname**|Nome del ruolo nel database corrente.|  
|**Role_id**|**smallint**|ID del ruolo nel database corrente.|  
|**Users_in_role**|**sysname**|Membro del ruolo nel database corrente.|  
|**UserID**|**smallint**|ID utente del membro del ruolo.|  
  
## <a name="remarks"></a>Commenti  
 Per visualizzare informazioni sull'appartenenza dei ruoli del database, utilizzare [sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md). Per visualizzare informazioni sui membri del ruolo del server, utilizzare [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)e per visualizzare informazioni sulle entità a livello di server, utilizzare [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
 Le informazioni restituite sono soggette a limitazioni di accesso ai metadati. Non vengono visualizzate le entità per le quali l'entità di database non dispone dell'autorizzazione. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-all-users"></a>R. Visualizzazione di un elenco di tutti gli utenti  
 Nell'esempio seguente vengono elencati tutti gli utenti nel database corrente.  
  
```  
EXEC sp_helpuser;  
```  
  
### <a name="b-listing-information-for-a-single-user"></a>B. Visualizzazione di informazioni relative a un singolo utente  
 Nell'esempio seguente vengono restituite informazioni sul proprietario del database utente (`dbo`).  
  
```  
EXEC sp_helpuser 'dbo';  
```  
  
### <a name="c-listing-information-for-a-database-role"></a>C. Visualizzazione di informazioni relative a un ruolo del database  
 Nell'esempio seguente vengono restituite informazioni sul ruolo predefinito del database `db_securityadmin`.  
  
```  
EXEC sp_helpuser 'db_securityadmin';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
  
