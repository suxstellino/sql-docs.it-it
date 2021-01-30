---
description: sp_changeobjectowner (Transact-SQL)
title: sp_changeobjectowner (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changeobjectowner_TSQL
- sp_changeobjectowner
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changeobjectowner
ms.assetid: 45b3dc1c-1cde-45b7-a248-5195c12973e9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 482fcbdb2d7c1b79eb0a8b5ed88d1295785b994b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159824"
---
# <a name="sp_changeobjectowner-transact-sql"></a>sp_changeobjectowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica il proprietario di un oggetto del database corrente.  
  
> [!IMPORTANT]
>  Questo stored procedure funziona solo con gli oggetti disponibili in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, utilizzare [ALTER schema](../../t-sql/statements/alter-schema-transact-sql.md) o [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) . **sp_changeobjectowner** modifica lo schema e il proprietario. Per mantenere la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], questa stored procedure modifica i proprietari degli oggetti solo quando sia il proprietario corrente che il nuovo proprietario possiedono schemi il cui nome corrisponde al relativo nome utente di database.  
> 
> [!IMPORTANT]
>  A questa stored procedure è stato aggiunto un nuovo requisito di autorizzazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changeobjectowner [ @objname = ] 'object' , [ @newowner = ] 'owner'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @objname = ] 'object'` Nome di una tabella, vista, funzione definita dall'utente o stored procedure esistente nel database corrente. l' *oggetto* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito. l' *oggetto* può essere qualificato con il proprietario dell'oggetto esistente, nel formato _existing_owner_**.** _oggetto_ se lo schema e il relativo proprietario hanno lo stesso nome.  
  
`[ @newowner = ] 'owner_ '` Nome dell'account di sicurezza che sarà il nuovo proprietario dell'oggetto. *owner* è di **tipo sysname** e non prevede alcun valore predefinito. il *proprietario* deve essere un utente del database, un ruolo del server, un [!INCLUDE[msCoName](../../includes/msconame-md.md)] account di accesso di Windows o un gruppo di Windows valido con accesso al database corrente. Se il nuovo proprietario è un utente di Windows o un gruppo di Windows per cui non esiste un'entità corrispondente a livello di database, verrà creato un utente di database.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_changeobjectowner** vengono rimosse tutte le autorizzazioni esistenti dall'oggetto. Dopo l'esecuzione di **sp_changeobjectowner** sarà necessario riapplicare le autorizzazioni che si desidera gestire. È pertanto consigliabile inserire in uno script le autorizzazioni esistenti prima di eseguire **sp_changeobjectowner**. Dopo avere modificato il proprietario dell'oggetto, è possibile eseguire lo script per riapplicare le autorizzazioni. È prima necessario modificare il proprietario dell'oggetto nello script.  
  
 Per modificare il proprietario di un'entità a sicurezza diretta, utilizzare ALTER AUTHORIZATION. Per modificare uno schema, utilizzare ALTER SCHEMA.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **db_owner** o l'appartenenza al ruolo predefinito del database **db_ddladmin** e al ruolo predefinito del database **DB_SECURITYADMIN** e anche all'autorizzazione CONTROL per l'oggetto.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene modificato il proprietario della tabella `authors` in `Corporate\GeorgeW`.  
  
```  
EXEC sp_changeobjectowner 'authors', 'Corporate\GeorgeW';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
