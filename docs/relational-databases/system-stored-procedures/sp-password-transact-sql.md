---
description: sp_password (Transact-SQL)
title: sp_password (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_password
- sp_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_password
ms.assetid: 0ecbec81-e637-44a9-a61e-11bf060ef084
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f154661d7cd03edfde83dd7774685e73e79aeb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195430"
---
# <a name="sp_password-transact-sql"></a>sp_password (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge o modifica una password per un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_password [ [ @old = ] 'old_password' , ]  
     { [ @new =] 'new_password' }  
     [ , [ @loginame = ] 'login' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @old = ] 'old_password'` È la vecchia password. *old_password* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @new = ] 'new_password'` Nuova password. *new_password* è di **tipo sysname** e non prevede alcun valore predefinito. Se non si utilizzano parametri denominati, è necessario specificare *old_password* .  
  
> [!IMPORTANT]  
>  Non utilizzare una password NULL. Usare una password complessa. Per altre informazioni, vedere [Strong Passwords](../../relational-databases/security/strong-passwords.md).  
  
`[ @loginame = ] 'login'` Nome dell'account di accesso interessato dalla modifica della password. *login* è di tipo **sysname** e il valore predefinito è NULL. l' *account di accesso* deve esistere già e può essere specificato solo dai membri del ruolo predefinito del server **sysadmin** o **securityadmin** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_password** chiama ALTER LOGIN. che supporta opzioni aggiuntive. Per informazioni sulla modifica delle password, vedere [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md).  
  
 Impossibile eseguire **sp_password** in una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LOGIN. È inoltre richiesta l'autorizzazione CONTROL SERVER per reimpostare una password senza specificare la vecchia password oppure se l'account di accesso da modificare dispone dell'autorizzazione CONTROL SERVER.  
  
 Un'entità può modificare la propria password.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-password-of-a-login-without-knowing-the-old-password"></a>R. Modifica della password di un account di accesso con vecchia password non nota  
 Nell'esempio seguente viene illustrato l'utilizzo di `ALTER LOGIN` per modificare la password dell'account di accesso `Victoria` impostandola su `B3r1000d#2-36`. Questo è il metodo preferito. L'utente che esegue questo comando deve disporre dell'autorizzazione CONTROL SERVER.  
  
```  
ALTER LOGIN Victoria WITH PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
### <a name="b-changing-a-password"></a>B. Modifica di una password  
 Nell'esempio seguente viene illustrato l'utilizzo di `ALTER LOGIN` per modificare la password dell'account di accesso `Victoria` da `B3r1000d#2-36` a `V1cteAmanti55imE`. Questo è il metodo preferito. L'utente `Victoria` può eseguire questo comando senza disporre di autorizzazioni aggiuntive. Per gli altri utenti è richiesta l'autorizzazione ALTER ANY LOGIN.  
  
```  
ALTER LOGIN Victoria WITH   
     PASSWORD = 'V1cteAmanti55imE'   
     OLD_PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
