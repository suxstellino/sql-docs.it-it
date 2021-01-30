---
description: sp_helpsrvrolemember (Transact-SQL)
title: sp_helpsrvrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpsrvrolemember
- sp_helpsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpsrvrolemember
ms.assetid: d0714913-8d6b-4de3-b042-3ae9934f839d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f17a4be7d5e9b1511a840857c933ed240d0e3d31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198694"
---
# <a name="sp_helpsrvrolemember-transact-sql"></a>sp_helpsrvrolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui membri di un ruolo predefinito del server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpsrvrolemember [ [ @srvrolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @srvrolename = ] 'role'` Nome di un ruolo predefinito del server. *Role* è di **tipo sysname** e il valore predefinito è null. Se *Role* non è specificato, il set di risultati include informazioni su tutti i ruoli predefiniti del server.  
  
 *Role* può essere uno dei valori seguenti.  
  
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
|MemberName|**sysname**|Nome di un membro di ServerRole|  
|MemberSID|**varbinary(85)**|ID di sicurezza di MemberName|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare sp_helprolemember per visualizzare i membri di un ruolo del database.  
  
 Tutti gli account di accesso sono membri di Public. sp_helpsrvrolemember non riconosce il ruolo public perché, internamente, non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa public come ruolo.  
  
 Per aggiungere o rimuovere membri dai ruoli del server, vedere [ALTER server role &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 sp_helpsrvrolemember non accetta un ruolo del server definito dall'utente come argomento. Per determinare i membri di un ruolo del server definito dall'utente, vedere gli esempi in [ALTER server role &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono elencati i membri del ruolo predefinito del server `sysadmin`.  
  
```  
EXEC sp_helpsrvrolemember 'sysadmin';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_helprole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
