---
description: sp_renamedb (Transact-SQL)
title: sp_renamedb (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_renamedb
- sp_renamedb_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_renamedb
ms.assetid: 7dd9d4ff-20e1-4857-9a8e-a5bff767cf76
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5b2e0ceff1889df17d1ab5ca62c6819c41679374
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179202"
---
# <a name="sp_renamedb-transact-sql"></a>sp_renamedb (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica il nome di un database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilizzare ALTER DATABASE MODIFY NAME in alternativa. Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_renamedb [ @dbname = ] 'old_name' , [ @newname = ] 'new_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'old_name'` Nome corrente del database. *old_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @newname = ] 'new_name'` Nuovo nome del database. *new_name* necessario attenersi alle regole per gli identificatori. *new_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo)  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza ai ruoli predefiniti del server **sysadmin** o **dbcreator** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creato il database `Accounting` e quindi il nome viene modificato in `Financial`. Viene quindi eseguita una query sulla vista del catalogo `sys.databases` per verificare il nuovo nome del database.  
  
```  
USE master;  
GO  
CREATE DATABASE Accounting;  
GO  
EXEC sp_renamedb N'Accounting', N'Financial';  
GO  
SELECT name, database_id, modified_date  
FROM sys.databases  
WHERE name = N'Financial';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_helpdb &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
