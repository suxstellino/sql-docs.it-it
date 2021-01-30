---
description: sp_dropremotelogin (Transact-SQL)
title: sp_dropremotelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropremotelogin
- sp_dropremotelogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropremotelogin
ms.assetid: 9f097652-a286-40b2-be73-568d77ada698
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 7b468ff83bffdd581695b9553c636b06fbe6c784
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187037"
---
# <a name="sp_dropremotelogin-transact-sql"></a>sp_dropremotelogin (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un account di accesso remoto di cui è stato eseguito il mapping a un account di accesso locale utilizzato per eseguire stored procedure remote nel server locale in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilizzare i server collegati e le stored procedure per i server collegati in alternativa.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_dropremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @remoteserver = ] 'remoteserver'` Nome del server remoto di cui è stato eseguito il mapping all'account di accesso remoto da rimuovere. *RemoteServer* è di **tipo sysname** e non prevede alcun valore predefinito. *RemoteServer* deve esistere già.  
  
`[ @loginame = ] 'login'` Nome dell'account di accesso facoltativo nel server locale associato al server remoto. *login* è di tipo **sysname** e il valore predefinito è NULL. Se specificato, l' *account di accesso* deve esistere già.  
  
`[ @remotename = ] 'remote_name'` Nome facoltativo dell'account di accesso remoto di cui è stato eseguito il mapping all' *account* di accesso quando si esegue l'accesso dal server remoto. *remote_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Se viene specificato solo *RemoteServer* , tutti gli account di accesso remoti per quel server remoto vengono rimossi dal server locale. Se viene specificato anche *login* , tutti gli account di accesso remoti da *RemoteServer* mappati a tale account di accesso locale specifico vengono rimossi dal server locale. Se viene specificato anche *remote_name* , viene rimosso dal server locale solo l'account di accesso remoto per tale utente remoto da *RemoteServer* .  
  
 Per aggiungere gli utenti del server locale, utilizzare **sp_addlogin**. Per rimuovere gli utenti del server locale, utilizzare **sp_droplogin**.  
  
 Gli account di accesso remoti sono necessari solo in caso di utilizzo di versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 e versioni successive utilizzano invece account di accesso dei server collegati. Utilizzare **sp_addlinkedsrvlogin** e **sp_droplinkedsrvlogin** per aggiungere e rimuovere gli account di accesso al server collegato.  
  
 Impossibile eseguire **sp_dropremotelogin** in una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza ai ruoli predefiniti del server **sysadmin** o **securityadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-all-remote-logins-for-a-remote-server"></a>R. Eliminazione di tutti gli account di accesso remoti per un server remoto  
 Nell'esempio seguente viene rimossa la voce relativa al server remoto `ACCOUNTS`, con la conseguente rimozione di tutti i mapping tra gli account di accesso nel server locale e gli account di accesso remoti nel server remoto.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-dropping-a-login-mapping"></a>B. Eliminazione di un mapping tra account di accesso  
 Nell'esempio seguente viene rimossa la voce per il mapping degli account di accesso remoti tra il server remoto `ACCOUNTS` e l'account di accesso locale `Albert`.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-dropping-a-remote-user"></a>C. Eliminazione di un utente remoto  
 Nell'esempio seguente viene rimosso l'account di accesso remoto `Chris` del server remoto `ACCOUNTS` sul quale è stato eseguito il mapping all'account di accesso locale `salesmgr`.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addremotelogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)   
 [sp_addserver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
