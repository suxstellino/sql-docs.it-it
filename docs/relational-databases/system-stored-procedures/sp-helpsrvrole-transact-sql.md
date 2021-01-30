---
description: sp_helpsrvrole (Transact-SQL)
title: sp_helpsrvrole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpsrvrole_TSQL
- sp_helpsrvrole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpsrvrole
ms.assetid: 5c7f39f3-c261-4f70-8beb-08242d4ac242
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f3c2c937e03edaaff888b90199ffeea806829bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209297"
---
# <a name="sp_helpsrvrole-transact-sql"></a>sp_helpsrvrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco dei ruoli predefiniti del server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpsrvrole [ [ @srvrolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @srvrolename = ] 'role'` Nome del ruolo predefinito del server. *Role* è di **tipo sysname** e il valore predefinito è null. *Role* può essere uno dei valori seguenti.  
  
|Ruolo predefinito del server|Descrizione|  
|-----------------------|-----------------|  
|sysadmin|Amministratori di sistema|  
|securityadmin|Amministratori di sicurezza|  
|serveradmin|Amministratori di server|  
|setupadmin|Amministratori di installazione|  
|processadmin|Amministratori di processi|  
|diskadmin|Amministratori di dischi|  
|dbcreator|Creatori di database|  
|bulkadmin|Può eseguire le istruzioni BULK INSERT|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|ServerRole|**sysname**|Nome del ruolo del server|  
|Descrizione|**sysname**|Descrizione di ServerRole|  
  
## <a name="remarks"></a>Commenti  
 I ruoli predefiniti del server sono definiti a livello di server e dispongono delle autorizzazioni per l'esecuzione di attività amministrative specifiche a livello del server. Non è possibile aggiungere, rimuovere o modificare i ruoli predefiniti del server.  
  
 Per aggiungere o rimuovere membri dai ruoli del server, vedere [ALTER server role &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Tutti gli account di accesso sono membri di Public. sp_helpsrvrole non riconosce il ruolo public perché, internamente, non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa public come ruolo.  
  
 sp_helpsrvrole non accetta un ruolo del server definito dall'utente come argomento. Per elencare i ruoli del server definiti dall'utente, vedere gli esempi in [ALTER server role &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-the-fixed-server-roles"></a>R. Elenco dei ruoli predefiniti del server  
 La query seguente restituisce l'elenco dei ruoli predefiniti del server.  
  
```  
EXEC sp_helpsrvrole ;  
```  
  
### <a name="b-listing-fixed-and-user-defined-server-roles"></a>B. Elenco dei ruoli predefiniti del server e di quelli definiti dall'utente  
 La query seguente restituisce un elenco sia dei ruoli predefiniti del server che di quelli definiti dall'utente.  
  
```  
SELECT * FROM sys.server_principals WHERE type = 'R' ;  
```  
  
### <a name="c-returning-a-description-of-a-fixed-server-role"></a>C. Restituzione di una descrizione di un ruolo predefinito del server  
 La query seguente restituisce il nome e la descrizione dei ruoli predefiniti del server `diskadmin`.  
  
```  
sp_helpsrvrole 'diskadmin' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Ruoli a livello di server](../../relational-databases/security/authentication-access/server-level-roles.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_dropsrvrolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)   
 [sp_helpsrvrolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
