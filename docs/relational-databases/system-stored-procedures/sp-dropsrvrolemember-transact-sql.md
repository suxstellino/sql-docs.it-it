---
description: sp_dropsrvrolemember (Transact-SQL)
title: sp_dropsrvrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropsrvrolemember
- sp_dropsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropsrvrolemember
ms.assetid: 7be99181-d221-49d0-9cb2-c930d8c044a0
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: d042fa55cf15dac997d7e52f9bd39eb7309fd1b5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158098"
---
# <a name="sp_dropsrvrolemember-transact-sql"></a>sp_dropsrvrolemember (Transact-SQL)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Rimuove un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure un utente o un gruppo di Windows da un ruolo predefinito del server.

> [!IMPORTANT]
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare [ALTER Server Role](../../t-sql/statements/alter-server-role-transact-sql.md) .

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```
sp_dropsrvrolemember [ @loginame = ] 'login' , [ @rolename = ] 'role'  
```

## <a name="arguments"></a>Argomenti

**[ @loginame =]** '_login_'  
Nome di un account di accesso che si desidera rimuovere dal ruolo predefinito del server. *login* è di **tipo sysname** e non prevede alcun valore predefinito. l' *account di accesso* deve esistere.  

**[ @rolename =]** '_Role_'  
Nome di un ruolo del server. *Role* è di **tipo sysname** e il valore predefinito è null. *Role* deve essere uno dei valori seguenti:  

-   sysadmin  
  
-   securityadmin  
  
-   serveradmin  
  
-   setupadmin  
  
-   processadmin  
  
-   diskadmin  
  
-   dbcreator  
  
-   bulkadmin 
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Per rimuovere un account di accesso da un ruolo predefinito del server, è possibile utilizzare solo la stored procedure sp_dropsrvrolemember. Utilizzare sp_droprolemember per rimuovere un membro da un ruolo del database.  
  
 L'account di accesso sa non può essere rimosso da nessun ruolo predefinito del server.  
  
 La stored procedure sp_dropsrvrolemember non può essere eseguita all'interno di una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server sysadmin o l'autorizzazione ALTER ANY LOGIN nel server e l'appartenenza al ruolo dal quale si desidera rimuovere il membro.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente l'account di accesso `JackO` viene rimosso dal ruolo predefinito del server `sysadmin`.  
  
```sql
EXEC sp_dropsrvrolemember 'JackO', 'sysadmin';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
