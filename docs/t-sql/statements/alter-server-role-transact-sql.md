---
description: ALTER SERVER ROLE (Transact-SQL)
title: ALTER SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SERVER_ROLE_TSQL
- ALTER SERVER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, ALTER
- ALTER SERVER ROLE statement
ms.assetid: 7a4db7bb-c442-4e12-9a8a-114da5bc7710
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0708ee4181ce959b1bc307d9fa29b3c7ae408c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182155"
---
# <a name="alter-server-role-transact-sql"></a>ALTER SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

Modifica l'appartenenza di un ruolo del server o il nome di un ruolo del server definito dall'utente. Impossibile rinominare i ruoli predefiniti del server.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER SERVER ROLE server_role_name   
{  
    [ ADD MEMBER server_principal ]  
  | [ DROP MEMBER server_principal ]  
  | [ WITH NAME = new_server_role_name ]  
} [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER SERVER ROLE  server_role_name  ADD MEMBER login;  
  
ALTER SERVER ROLE  server_role_name  DROP MEMBER login;  
```  
  
## <a name="arguments"></a>Argomenti  
*server_role_name*  
Nome del ruolo del server da modificare.  
  
ADD MEMBER *server_principal*  
Aggiunge l'entità server specificata al ruolo del server. *server_principal* può essere un account di accesso o un ruolo del server definito dall'utente. *server_principal* non può essere un ruolo predefinito del server, un ruolo del database o sa.  
  
DROP MEMBER *server_principal*  
Rimuove l'entità server specificata dal ruolo del server. *server_principal* può essere un account di accesso o un ruolo del server definito dall'utente. *server_principal* non può essere un ruolo predefinito del server, un ruolo del database o sa.  
  
WITH NAME **=** _new_server_role_name_  
Specifica il nuovo nome del ruolo del server definito dall'utente. Il nome non deve essere già esistente nel server.  
  
## <a name="remarks"></a>Commenti  
La modifica del nome di un ruolo del server definito dall'utente non comporta la modifica del numero di ID, del proprietario o delle autorizzazioni del ruolo.  
  
Per la modifica dell'appartenenza al ruolo, `ALTER SERVER ROLE` sostituisce sp_addsrvrolemember e sp_dropsrvrolemember. Queste stored procedure sono deprecate.  
  
È possibile visualizzare i ruoli del server eseguendo una query sulle viste del catalogo `sys.server_role_members` e `sys.server_principals`.  
  
Per modificare il proprietario di un ruolo del server definito dall'utente, usare [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
Per modificare il nome di un ruolo del server definito dall'utente è richiesta l'autorizzazione `ALTER ANY SERVER ROLE` per il server.  
  
**Ruoli predefiniti del server**  
  
Per aggiungere un membro a un ruolo predefinito del server, è necessario essere membri di tale ruolo o del ruolo predefinito del server `sysadmin`.  
  
> [!NOTE]  
>  Le autorizzazioni `CONTROL SERVER` e `ALTER ANY SERVER ROLE` non sono sufficienti per eseguire `ALTER SERVER ROLE` per un ruolo predefinito del server e l'autorizzazione `ALTER` non può essere concessa per un ruolo predefinito del server.  
  
**Ruoli del server definiti dall'utente**  
  
Per aggiungere un membro a un ruolo del server definito dall'utente, è necessario essere membri del ruolo predefinito del server `sysadmin` oppure avere l'autorizzazione `CONTROL SERVER` o `ALTER ANY SERVER ROLE`. In alternativa è necessario avere l'autorizzazione `ALTER` per tale ruolo.  
  
> [!NOTE]  
>  A differenza dei ruoli predefiniti del server, i membri di un ruolo del server definito dall'utente non dispongono implicitamente dell'autorizzazione per aggiungere membri a quello stesso ruolo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-name-of-a-server-role"></a>R. Modifica del nome di un ruolo del server  
Nell'esempio seguente viene creato un ruolo del server denominato `Product`, viene quindi modificato il nome del ruolo del server in `Production`.  
  
```sql
CREATE SERVER ROLE Product ;  
ALTER SERVER ROLE Product WITH NAME = Production ;  
GO  
```  
  
### <a name="b-adding-a-domain-account-to-a-server-role"></a>B. Aggiunta di un account di dominio a un ruolo del server  
Nell'esempio seguente viene aggiunto un account di dominio denominato `adventure-works\roberto0` al ruolo del server definito dall'utente denominato `Production`.  
  
```sql
ALTER SERVER ROLE Production ADD MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="c-adding-a-sql-server-login-to-a-server-role"></a>C. Aggiunta di un account di accesso di SQL Server a un ruolo del server  
Nell'esempio seguente un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominato `Ted` viene aggiunto al ruolo predefinito del server `diskadmin`.  
  
```sql
ALTER SERVER ROLE diskadmin ADD MEMBER Ted ;  
GO  
```  
  
### <a name="d-removing-a-domain-account-from-a-server-role"></a>D. Rimozione di un account di dominio da un ruolo del server  
Nell'esempio seguente viene rimosso un account di dominio denominato `adventure-works\roberto0` dal ruolo del server definito dall'utente denominato `Production`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="e-removing-a-sql-server-login-from-a-server-role"></a>E. Rimozione di un account di accesso di SQL Server da un ruolo del server  
Nell'esempio seguente l'account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Ted` viene rimosso dal ruolo predefinito del server `diskadmin`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER Ted ;  
GO  
```  
  
### <a name="f-granting-a-login-the-permission-to-add-logins-to-a-user-defined-server-role"></a>F. Concessione di un account di accesso all'autorizzazione per aggiungere account di accesso a un ruolo del server definito dall'utente  
Nell'esempio seguente viene consentito a `Ted` di aggiungere altri account di accesso al ruolo del server definito dall'utente denominato `Production`.  
  
```sql
GRANT ALTER ON SERVER ROLE::Production TO Ted ;  
GO  
```  
  
### <a name="g-to-view-role-membership"></a>G. Per visualizzare l'appartenenza ai ruoli  
Per visualizzare l'appartenenza ai ruoli, usare la pagina **Ruolo server (Membri)** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o eseguire la query seguente:  
  
```sql
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-basic-syntax"></a>H. Sintassi di base  
Nell'esempio seguente l'account di accesso `Anna` viene aggiunto al ruolo del server `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC ADD MEMBER Anna;  
```  
  
### <a name="i-remove-a-login-from-a-resource-class"></a>I. Rimuove un account di accesso da una classe di risorse.  
L'esempio seguente rimuove l'appartenenza di Anna al ruolo del server `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC DROP MEMBER Anna;  
```  
  
## <a name="see-also"></a>Vedere anche  
[CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
[DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
[CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
[ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
[DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
[Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
[Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
[Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
[sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
