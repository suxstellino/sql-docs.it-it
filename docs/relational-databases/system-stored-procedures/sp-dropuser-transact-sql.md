---
description: sp_dropuser (Transact-SQL)
title: sp_dropuser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropuser
- sp_dropuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropuser
ms.assetid: e28f18f9-7ecf-4568-89f4-fe5c520df386
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba5b9eb814f48003d93bf6d7b05c2b1c37c1ea76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157145"
---
# <a name="sp_dropuser-transact-sql"></a>sp_dropuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un utente di database dal database corrente. **sp_dropuser** garantisce la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [drop user](../../t-sql/statements/drop-user-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropuser [ @name_in_db = ] 'user'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name_in_db = ] 'user'` Nome dell'utente da rimuovere. l' *utente* è di **tipo sysname** e non prevede alcun valore predefinito. l' *utente* deve esistere nel database corrente. Quando si specifica un account di accesso di Windows, utilizzare il nome con il quale il database identifica l'account di accesso.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropuser** esegue **sp_revokedbaccess** per rimuovere l'utente dal database corrente.  
  
 Utilizzare **sp_helpuser** per visualizzare un elenco dei nomi utente che possono essere rimossi dal database corrente.  
  
 Quando si rimuove un utente di database, vengono rimossi anche tutti gli alias di tale utente. Se l'utente è proprietario di uno schema vuoto che ha lo stesso suo nome, lo schema verrà rimosso. Se l'utente è proprietario di altre entità a protezione diretta nel database, l'utente non verrà rimosso. La proprietà degli oggetti deve prima essere trasferita ad un'altra entità. Per altre informazioni, vedere [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md). Se si rimuove un utente di database, vengono rimosse automaticamente anche le autorizzazioni corrispondenti e l'utente viene rimosso dai ruoli di database di cui è membro.  
  
 non è possibile utilizzare **sp_dropuser** per rimuovere il proprietario del database (**dbo**) **INFORMATION_SCHEMA** utenti o l'utente **Guest** dai database **Master** o **tempdb** . Nei database non di sistema, l' `EXEC sp_dropuser 'guest'` autorizzazione Connect viene revocata dall'utente **Guest**. ma l'utente stesso non verrà rimosso.  
  
 Impossibile eseguire **sp_dropuser** in una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY USER per il database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente l'utente `Albert` viene rimosso dal database corrente.  
  
```  
EXEC sp_dropuser 'Albert';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [sp_revokedbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
