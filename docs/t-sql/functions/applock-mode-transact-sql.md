---
description: APPLOCK_MODE (Transact-SQL)
title: APPLOCK_MODE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPLOCK_MODE_TSQL
- APPLOCK_MODE
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- APPLOCK_MODE function
ms.assetid: e43d4917-77f1-45cc-b231-68ba7fee3385
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6103d0d5a80f147eb59f3983a178aaeae9591a1f
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054894"
---
# <a name="applock_mode-transact-sql"></a>APPLOCK_MODE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce la modalità di blocco acquisita dal proprietario del blocco per una particolare risorsa di applicazione. APPLOCK_MODE è una funzione di blocco a livello di applicazione che viene eseguita nel database corrente. L'ambito dei blocchi a livello di applicazione è il database.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
APPLOCK_MODE( 'database_principal' , 'resource_name' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
'*database_principal*'  
Utente, ruolo o ruolo applicazione a cui è possibile concedere autorizzazioni per gli oggetti nel database. Affinché la chiamata della funzione abbia esito positivo, è necessario che il chiamante sia un membro del ruolo predefinito del database *database_principal*, dbo o db_owner.
  
'*resource_name*'  
Nome di una risorsa di blocco specificato nell'applicazione client. L'applicazione deve garantire che il nome della risorsa sia univoco. Il nome specificato viene sottoposto internamente ad hashing per creare un valore che è possibile archiviare internamente in Gestione blocchi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *resource_name* è di tipo **nvarchar(255)** e non prevede alcun valore predefinito. Per *resource_name* viene eseguito un confronto binario ed è supportata la distinzione tra maiuscole e minuscole, indipendentemente dalle impostazioni delle regole di confronto del database corrente.
  
'*lock_owner*'  
Proprietario del blocco, ovvero il valore di *lock_owner* al momento della richiesta del blocco. *lock_owner* è di tipo **nvarchar(32)** e il valore può essere **Transaction** (impostazione predefinita) o **Session**.
  
## <a name="return-types"></a>Tipi restituiti
**nvarchar(32)**
  
## <a name="return-value"></a>Valore restituito
Restituisce la modalità di blocco acquisita dal proprietario del blocco per una particolare risorsa di applicazione. I valori possibili della modalità di blocco sono i seguenti:

:::row:::
   :::column span="":::
      **NoLock**<br>      **Aggiornamento**<br>      **\*SharedIntentExclusive**
   :::column-end:::
   :::column span="":::
      **IntentShared**<br>      **IntentExclusive**<br>      **\*UpdateIntentExclusive**

   :::column-end:::
   :::column span="":::
      **Condivisa**<br>      **Exclusive**
   :::column-end:::
:::row-end:::
  
*Questa modalità di blocco risulta dalla combinazione di altre modalità di blocco e non può essere acquisita in modo esplicito tramite sp_getapplock.
  
## <a name="function-properties"></a>Proprietà delle funzioni
**Non deterministica**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>Esempi  
Si supponga che due utenti, l'utente A e l'utente B, eseguano la sequenza di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] riportata di seguito in sessioni separate.
  
L'utente A esegue:
  
```sql
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result INT;  
EXEC @result=sp_getapplock  
    @DbPrincipal='public',  
    @Resource='Form1',  
    @LockMode='Shared',  
    @LockOwner='Transaction';  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
GO  
```  
  
L'utente B quindi esegue:
  
```sql
Use AdventureWorks2012;  
GO  
BEGIN TRAN;  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
--Result set: NoLock  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Shared', 'Transaction');  
--Result set: 1 (Lock is grantable.)  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: 0 (Lock is not grantable.)  
GO  
```  
  
L'utente A quindi esegue:
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
L'utente B quindi esegue:
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Gli utenti A e B eseguono quindi l'istruzione seguente:
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche
[APPLOCK_TEST &#40;Transact-SQL&#41;](../../t-sql/functions/applock-test-transact-sql.md)  
[sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
  
  
