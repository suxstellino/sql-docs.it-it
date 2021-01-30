---
description: USER_ID (Transact-SQL)
title: USER_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER_ID
- USER_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- USER_ID function
- identification numbers [SQL Server]
- IDs [SQL Server], databases
- users [SQL Server], database ID numbers
- database IDs [SQL Server]
- identification numbers [SQL Server], databases
ms.assetid: 67fd29bc-eda9-4d4d-b148-5d3659181a43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 860016421276c6c02c8d0d523476a856f70f227b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210624"
---
# <a name="user_id-transact-sql"></a>USER_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il numero di identificazione per un utente del database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Usare invece [DATABASE_PRINCIPAL_ID](../../t-sql/functions/database-principal-id-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
USER_ID ( [ 'user' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *user*  
 Nome utente da utilizzare. *user* è di tipo **nchar**. Se si specifica un valore **char**, viene eseguita la conversione implicita in **nchar**. È necessario utilizzare le parentesi.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 Se *user* viene omesso, viene presunto l'utente corrente. Se il parametro contiene la parola NULL, restituirà NULL. Se viene chiamata dopo EXECUTE AS, la funzione USER_ID restituirà l'ID del contesto rappresentato.  
  
 Se un'entità di Windows di cui non è stato eseguito il mapping a uno specifico utente del database accede a un database tramite l'appartenenza a un gruppo, USER_ID restituisce 0 (ID del ruolo public). Se tale entità crea un oggetto senza specificare uno schema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creerà un utente e uno schema impliciti di cui viene eseguito il mapping all'entità di Windows. L'utente creato in questo modo non può essere utilizzato per connettersi al database. Le chiamate alla funzione USER_ID da parte di un'entità di Windows di cui è eseguito il mapping a un utente implicito restituiranno l'ID dell'utente implicito.  
  
 È possibile utilizzare USER_ID in un elenco di selezione, in una clausola WHERE e in tutti i casi in cui è consentita un'espressione. Per altre informazioni, vedere [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituito il numero di identificazione per l'utente di `AdventureWorks2012``Harold`.  
  
```sql
USE AdventureWorks2012;  
SELECT USER_ID('Harold');  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [DATABASE_PRINCIPAL_ID &#40;Transact-SQL&#41;](../../t-sql/functions/database-principal-id-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
