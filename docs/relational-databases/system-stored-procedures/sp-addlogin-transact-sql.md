---
description: sp_addlogin (Transact-SQL)
title: sp_addlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlogin
- sp_addlogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlogin
ms.assetid: 030f19c3-a5e3-4b53-bfc4-de4bfca0fddc
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a85da6cb0e9f0d2a490c802e4d67f8096999f217
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202400"
---
# <a name="sp_addlogin-transact-sql"></a>sp_addlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un nuovo account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che consente a un utente di connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [Create Login](../../t-sql/statements/create-login-transact-sql.md) .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addlogin [ @loginame = ] 'login'   
    [ , [ @passwd = ] 'password' ]   
    [ , [ @defdb = ] 'database' ]   
    [ , [ @deflanguage = ] 'language' ]   
    [ , [ @sid = ] sid ]   
    [ , [ @encryptopt = ] 'encryption_option' ]   
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @loginame =]'*login*'  
 Nome dell'account di accesso. *login* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [ @passwd =]'*password*'  
 Password dell'account di accesso. *password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
 [ @defdb =]'*database*'  
 Database predefinito dell'account di accesso, ovvero il primo database a cui viene connesso l'account dopo l'accesso. il *database* è di **tipo sysname** e il valore predefinito è **Master**.  
  
 [ @deflanguage =]'*Language*'  
 Lingua predefinita dell'account di accesso. *Language* è di **tipo sysname** e il valore predefinito è null. Se *la lingua non* è specificata, la *lingua* predefinita del nuovo account di accesso viene impostata sulla lingua predefinita corrente del server.  
  
 [ @sid =]'*SID*'  
 ID di sicurezza (SID, Security Identification Number). *SID* è di tipo **varbinary (16)** e il valore predefinito è null. Se *SID* è null, il sistema genera un SID per il nuovo account di accesso. Nonostante l'utilizzo di un tipo di dati **varbinary** , i valori diversi da null devono avere una lunghezza esattamente di 16 byte e non devono essere già esistenti. La specifica di *SID* è utile, ad esempio, quando si crea uno script o si trasferiscono gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso da un server a un altro e si desidera che gli account di accesso dispongano dello stesso SID in server diversi.  
  
 [ @encryptopt =]'*encryption_option*'  
 Specifica se la password viene passata in forma non crittografata o come hash della password non crittografata. Si noti che non viene applicata alcuna crittografia. Le parole "crittografia" ed "encryption" vengono utilizzate in questa descrizione per compatibilità con le versioni precedenti. Se la password viene passata in forma non crittografata, per la password viene eseguito l'hashing. Il valore hash viene archiviato. *encryption_option* è di tipo **varchar (20)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|NULL|La password viene passata in forma non crittografata. Questo è il valore predefinito.|  
|**skip_encryption**|Per la password è già stato eseguito l'hashing. [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve archiviare il valore senza ripetere l'hashing.|  
|**skip_encryption_old**|Per la password specificata è stato eseguito l'hashing con una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve archiviare il valore senza ripetere l'hashing. Questa opzione è disponibile solo in funzione delle operazioni di aggiornamento.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 I nomi degli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere composti da 1 a 128 caratteri, inclusi lettere, simboli e numeri. Gli account di accesso non possono contenere una barra rovesciata ( \\ ), essere un nome di account di accesso riservato, ad esempio SA o Public, oppure essere già esistenti oppure essere null o una stringa vuota ( `''` ).  
  
 Se viene specificato il nome di un database predefinito, è possibile connettersi a tale database senza eseguire l'istruzione USE. Tuttavia, non è possibile utilizzare il database predefinito fino a quando non si dispone dell'accesso al database da parte del proprietario del database (utilizzando [sp_adduser](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md) o [sp_addrolemember](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)) o [sp_addrole](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md).  
  
 Il numero SID è un GUID che identificherà in modo univoco l'account di accesso nel server.  
  
 Se si modifica la lingua predefinita del server non viene modificata la lingua predefinita degli account di accesso esistenti. Per modificare la lingua predefinita del server, utilizzare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 L'utilizzo di **skip_encryption** per l'eliminazione dell'hashing delle password è utile se la password è già stata sottoposta a hashing quando l'account di accesso viene aggiunto a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se per la password è stato eseguito l'hashing con una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilizzare **skip_encryption_old**.  
  
 La stored procedure sp_addlogin non può essere eseguita in una transazione definita dall'utente.  
  
 Nella tabella seguente sono elencate varie stored procedure utilizzate in combinazione con sp_addlogin.  
  
|Stored procedure|Descrizione|  
|----------------------|-----------------|  
|[sp_grantlogin](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)|Aggiunge un utente o un gruppo di Windows.|  
|[sp_password](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)|Modifica la password di un utente.|  
|[sp_defaultdb](../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md)|Modifica il database predefinito di un utente.|  
|[sp_defaultlanguage](../../relational-databases/system-stored-procedures/sp-defaultlanguage-transact-sql.md)|Modifica la lingua predefinita di un utente.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LOGIN.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-sql-server-login"></a>R. Creazione di un account di accesso di SQL Server  
 Nell'esempio seguente viene creato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'utente `Victoria` con la password `B1r12-36`, senza specificare un database predefinito.  
  
```  
EXEC sp_addlogin 'Victoria', 'B1r12-36';  
GO  
```  
  
### <a name="b-creating-a-sql-server-login-that-has-a-default-database"></a>B. Creazione di un account di accesso di SQL Server con un database predefinito  
 Nell'esempio seguente viene creato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'utente `Albert` con la password `B5432-3M6` e il database predefinito `corporate`.  
  
```  
EXEC sp_addlogin 'Albert', 'B5432-3M6', 'corporate';  
GO  
```  
  
### <a name="c-creating-a-sql-server-login-that-has-a-different-default-language"></a>C. Creazione di un account di accesso di SQL Server con una diversa lingua predefinita  
 Nell'esempio seguente viene creato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'utente `TzTodorov` con la password `709hLKH7chjfwv`, il database predefinito `AdventureWorks2012` e la lingua predefinita `Bulgarian`.  
  
```  
EXEC sp_addlogin 'TzTodorov', '709hLKH7chjfwv', 'AdventureWorks2012', N'български'  
```  
  
### <a name="d-creating-a-sql-server-login-that-has-a-specific-sid"></a>D. Creazione di un account di accesso di SQL Server con un SID specifico  
 Nell'esempio seguente viene creato un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'utente `Michael`, con la password `B548bmM%f6`, il database predefinito `AdventureWorks2012`, la lingua predefinita `us_english` e il SID `0x0123456789ABCDEF0123456789ABCDEF`.  
  
```  
EXEC sp_addlogin 'Michael', 'B548bmM%f6', 'AdventureWorks2012', 'us_english', 0x0123456789ABCDEF0123456789ABCDEF  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_helpuser &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [xp_logininfo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/xp-logininfo-transact-sql.md)  
  
  
