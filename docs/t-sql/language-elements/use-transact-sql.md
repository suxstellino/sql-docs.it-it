---
description: USE (Transact-SQL)
title: USE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USE_TSQL
- USE
dev_langs:
- TSQL
helpviewer_keywords:
- USE statement
- database context [SQL Server]
- context changes [SQL Server]
- modifying database context
ms.assetid: c05acac8-c063-4770-8e36-d7f71d500b10
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d83799db882df09ae57e7225d8c3f52cdb04cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208757"
---
# <a name="use-transact-sql"></a>USE (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Sostituisce il contesto di database con il database o lo snapshot del database specificato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
USE { database_name }   
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *database_name*  
 Nome del database o dello snapshot del database su cui viene impostato il contesto utente. I nomi di database e di snapshot del database devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] il parametro del database può fare riferimento solo al database corrente. Se viene specificato un database diverso da quello corrente, l'istruzione `USE` non consente il passaggio tra database e viene restituito il codice di errore 40508. Per cambiare database, è necessario connettersi direttamente al database. L'istruzione USE è contrassegnata come non applicabile al database SQL all'inizio di questa pagina, perché anche se è possibile includere l'istruzione `USE` in un batch, non esegue alcuna operazione.
  
## <a name="remarks"></a>Commenti  
 Quando un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tale account viene connesso automaticamente al relativo database predefinito e acquisisce il contesto di sicurezza di un utente del database. Se per l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è stato creato alcun utente di database, l'account si connette come guest. Se l'utente del database non dispone dell'autorizzazione CONNECT per il database, l'istruzione USE avrà esito negativo. Se all'account di accesso non è stato assegnato un database predefinito, verrà impostato il database master.  
  
 L'istruzione USE viene eseguita sia in fase di compilazione che in fase di esecuzione e ha effetto immediato. Pertanto, le istruzioni presenti in un batch dopo l'istruzione USE vengono eseguite nel database specificato.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONNECT per il database di destinazione.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il contesto di database viene impostato sul database `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../statements/create-database-transact-sql.md)   
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
  
