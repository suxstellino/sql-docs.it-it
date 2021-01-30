---
description: sp_adduser (Transact-SQL)
title: sp_adduser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_adduser
- sp_adduser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adduser
ms.assetid: 61a40eb4-573f-460c-9164-bd1bbfaf8b25
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c250837ad65e0b14fc5612eae69be6f93d8f23e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198371"
---
# <a name="sp_adduser-transact-sql"></a>sp_adduser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge un nuovo utente al database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [Create User](../../t-sql/statements/create-user-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_adduser [ @loginame = ] 'login'   
    [ , [ @name_in_db = ] 'user' ]   
    [ , [ @grpname = ] 'role' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @loginame = ] 'login'` Nome dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso di o dell'account di accesso di Windows. *login* è di **tipo sysname** e non prevede alcun valore predefinito. *login* deve essere un account di accesso o un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows esistente.  
  
`[ @name_in_db = ] 'user'` Nome del nuovo utente del database. *User* è di **tipo sysname** e il valore predefinito è null. Se l' *utente* non è specificato, il nome del nuovo utente del database viene impostato sul nome dell' *account di accesso* . Se si specifica *User* , al nuovo utente viene assegnato un nome nel database diverso dal nome dell'account di accesso a livello di server.  
  
`[ @grpname = ] 'role'` Ruolo del database di cui il nuovo utente diventa membro. *Role* è di **tipo sysname** e il valore predefinito è null. *Role* deve essere un ruolo del database valido nel database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_adduser** creerà anche uno schema con il nome dell'utente.  
  
 Dopo avere aggiunto un utente, utilizzare le istruzioni GRANT, DENY e REVOKE per definire le autorizzazioni per controllare le attività che l'utente può svolgere.  
  
 Utilizzare **sys.server_principals** per visualizzare un elenco di nomi di account di accesso validi.  
  
 Utilizzare **sp_helprole** per visualizzare un elenco dei nomi di ruolo validi. Se si specifica un ruolo, l'utente ottiene automaticamente le autorizzazioni definite per tale ruolo. Se non si specifica un ruolo, l'utente ottiene le autorizzazioni concesse al ruolo **public** predefinito. Per aggiungere un utente a un ruolo, è necessario specificare un valore per il *nome utente* . il *nome utente* può essere lo stesso *login_ID*.  
  
 Il **Guest** utente esiste già in ogni database. L'aggiunta dell'utente **Guest** consentirà l'abilitazione di questo utente, se in precedenza era disabilitato. Per impostazione predefinita, il **Guest** utente è disabilitato nei nuovi database.  
  
 Impossibile eseguire **sp_adduser** all'interno di una transazione definita dall'utente.  
  
 Non è possibile aggiungere un utente **Guest** perché un utente **Guest** esiste già all'interno di ogni database. Per abilitare l'utente **Guest** , concedere l'autorizzazione Connect **Guest** come indicato di seguito:  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario essere il proprietario del database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-adding-a-database-user"></a>R. Aggiunta di un utente del database  
 Nell'esempio seguente l'utente del database `Vidur` viene aggiunto al ruolo esistente `Recruiting` nel database corrente utilizzando l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esistente `Vidur`.  
  
```  
EXEC sp_adduser 'Vidur', 'Vidur', 'Recruiting';  
```  
  
### <a name="b-adding-a-database-user-with-the-same-login-id"></a>B. Aggiunta di un utente del database con lo stesso ID di accesso  
 Nell'esempio seguente l'utente `Arvind` viene aggiunto al database corrente per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Arvind`. Questo utente appartiene al ruolo **pubblico** predefinito.  
  
```  
EXEC sp_adduser 'Arvind';  
```  
  
### <a name="c-adding-a-database-user-with-a-different-name-than-its-server-level-login"></a>C. Aggiunta di un utente del database con un nome diverso dall'account di accesso a livello di server  
 Nell'esempio seguente l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`BjornR` viene aggiunto al database corrente che include il nome utente `Bjorn`, quindi l'utente del database `Bjorn` viene aggiunto al ruolo del database `Production`.  
  
```  
EXEC sp_adduser 'BjornR', 'Bjorn', 'Production';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
