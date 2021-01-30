---
description: sp_changedbowner (Transact-SQL)
title: sp_changedbowner (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changedbowner
- sp_changedbowner_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changedbowner
ms.assetid: 516ef311-e83b-45c9-b9cd-0e0641774c04
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c11630e73b50eae7c1f972ac7bff1e6d809a464f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159030"
---
# <a name="sp_changedbowner-transact-sql"></a>sp_changedbowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica il proprietario del database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changedbowner [ @loginame = ] 'login'  
     [ , [ @map = ] remap_alias_flag ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @loginame =]'*login*'  
 ID di accesso del nuovo proprietario del database corrente. *login* è di **tipo sysname** e non prevede alcun valore predefinito. l' *account di accesso* deve essere un account di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accesso o un utente di Windows già esistente. l' *account di accesso* non può diventare il proprietario del database corrente se dispone già dell'accesso al database tramite un account di sicurezza utente esistente all'interno del database. Per evitare questa situazione, rimuovere l'utente dal database corrente.  
  
 [ @map =] *remap_alias_flag*  
 Il parametro *remap_alias_flag* è deprecato perché gli alias di accesso sono stati rimossi da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'uso del parametro *remap_alias_flag* non genera un errore, ma non ha alcun effetto.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Dopo l'esecuzione di sp_changedbowner, nel database il nuovo proprietario è noto come utente dbo. L'utente dbo dispone di autorizzazioni implicite per l'esecuzione di qualsiasi operazione nel database.  
  
 Il proprietario dei database di sistema master, model e tempdb non può essere modificato.  
  
 Per visualizzare un elenco dei valori di *accesso* validi, eseguire il sp_helplogins stored procedure.  
  
 L'esecuzione di sp_changedbowner solo con il parametro *login* modifica la proprietà del database per l' *accesso*.  
  
 È possibile modificare il proprietario di qualsiasi entità a protezione diretta mediante l'istruzione ALTER AUTHORIZATION. Per altre informazioni, vedere [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione TAKE OWNERSHIP per il database. Se il nuovo proprietario dispone di un utente corrispondente nel database, è richiesta l'autorizzazione IMPERSONATE per l'account di accesso. In caso contrario, è richiesta l'autorizzazione CONTROL SERVER per il server.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente l'account di accesso `Albert` diventa il proprietario del database corrente.  
  
```  
EXEC sp_changedbowner 'Albert';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [sp_dropalias &#40;&#41;Transact-SQL ](./system-stored-procedures-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helpdb &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helplogins &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
