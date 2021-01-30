---
description: sp_dropapprole (Transact-SQL)
title: sp_dropapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropapprole_TSQL
- sp_dropapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropapprole
ms.assetid: ea1aefe6-8f7d-46e9-a3cb-7b037b393e73
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 0f322e8434e7ae14b357d48c58b0dbc21eadbf86
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200705"
---
# <a name="sp_dropapprole-transact-sql"></a>sp_dropapprole (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un ruolo applicazione dal database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [Drop application role](../../t-sql/statements/drop-application-role-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_dropapprole [@rolename = ] 'role'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rolename = ] 'role'` Ruolo applicazione da rimuovere. *Role* è di **tipo sysname** e non prevede alcun valore predefinito. il *ruolo* deve esistere nel database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropapprole** può essere utilizzato solo per rimuovere i ruoli applicazione. Se un ruolo possiede un'entità a protezione diretta, il ruolo non può essere rimosso. Prima di rimuovere un ruolo applicazione proprietario di entità a protezione diretta, è necessario innanzitutto trasferire la proprietà delle entità a protezione diretta oppure rimuoverle.  
  
 Impossibile eseguire **sp_dropapprole** in una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY APPLICATION ROLE nel database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente il ruolo applicazione `SalesApp` viene rimosso dal database corrente.  
  
```sql
EXEC sp_dropapprole 'SalesApp';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addapprole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addapprole-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_changeobjectowner &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changeobjectowner-transact-sql.md)   
 [sp_setapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
