---
description: SETUSER (Transact-SQL)
title: SETUSER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3cfa4dfcd316a91fa6224ef4ea9a8807740cca74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189394"
---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente a un membro del ruolo predefinito del server **sysadmin** o al proprietario di un database di rappresentare un altro utente.  
  
> [!IMPORTANT]  
>  SETUSER è disponibile solo per garantire la compatibilità con le versioni precedenti in quanto nelle future versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe non essere più supportata. È invece consigliabile usare [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *username* **'**  
 Nome di un utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o di Windows nel database corrente che viene rappresentato. Se *username* viene omesso, viene ripristinata l'identità originale dell'amministratore di sistema o del proprietario del database che rappresenta l'utente.  
  
 WITH NORESET  
 Specifica che le istruzioni SETUSER successive (prive di argomento *username*) non ripristinano l'identità dell'utente sull'amministratore di sistema o sul proprietario del database.  
  
## <a name="remarks"></a>Commenti  
 L'istruzione SETUSER consente ai membri del ruolo predefinito del server **sysadmin** o al proprietario di un database di adottare l'identità di un altro utente in modo da verificarne le autorizzazioni. L'appartenenza al ruolo predefinito del database db_owner non è sufficiente.  
  
 Utilizzare l'istruzione SETUSER solo per utenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in quanto non è supportata per gli utenti di Windows. Dopo l'esecuzione di SETUSER per assumere l'identità di un altro utente, tutti gli oggetti che vengono creati sono di proprietà dell'utente rappresentato. Se ad esempio il proprietario del database assume l'identità dell'utente **Margaret** e crea la tabella **orders**, la tabella **orders** è di proprietà dell'utente **Margaret** e non dell'amministratore del sistema.  
  
 L'istruzione SETUSER rimane valida fino a quando non viene eseguita un'altra istruzione SETUSER o il database corrente viene modificato tramite l'istruzione USE.  
  
> [!NOTE]  
>  Se il proprietario del database o l'amministratore del sistema utilizza l'istruzione SETUSER WITH NORESET, per ristabilire le proprie autorizzazioni deve disconnettersi e quindi riconnettersi.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o la proprietà del database. L'appartenenza al ruolo predefinito del database **db_owner** non è sufficiente.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato come il proprietario del database può adottare l'identità di un altro utente. L'utente `mary` ha creato una tabella denominata `computer_types`. Tramite l'istruzione SETUSER, il proprietario del database rappresenta `mary` per concedere all'utente `joe` l'accesso alla tabella `computer_types` e quindi ripristina la propria identità.  
  
```sql
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
  
  
