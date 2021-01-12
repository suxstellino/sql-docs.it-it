---
description: APPLOCK_TEST (Transact-SQL)
title: APPLOCK_TEST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- APPLOCK_TEST_TSQL
- APPLOCK_TEST
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- APPLOCK_TEST function
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- testing application locks
ms.assetid: 4ea33d04-f8e9-46ff-ae61-985bd3eaca2c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4babfd8fba4184e255b366b8a949fe195df466de
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98089933"
---
# <a name="applock_test-transact-sql"></a>APPLOCK_TEST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce le informazioni relative alla possibilità o meno di concedere un blocco per una risorsa dell'applicazione specifica a un proprietario di blocchi specifico senza acquisire il blocco stesso. APPLOCK_TEST è una funzione di blocco a livello di applicazione che viene eseguita nel database corrente. L'ambito dei blocchi a livello di applicazione è il database.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
APPLOCK_TEST ( 'database_principal' , 'resource_name' , 'lock_mode' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
**'** *database_principal* **'**  
Utente, ruolo o ruolo applicazione a cui è possibile concedere autorizzazioni per gli oggetti nel database. Affinché la chiamata della funzione abbia esito positivo, è necessario che il chiamante sia un membro del ruolo predefinito del database *database_principal*, dbo o db_owner.
  
**'** *resource_name* **'**  
Nome di una risorsa di blocco specificato nell'applicazione client. L'applicazione deve garantire che il nome della risorsa sia univoco. Il nome specificato viene sottoposto internamente ad hashing per creare un valore che è possibile archiviare internamente in Gestione blocchi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  *resource_name* è di tipo **nvarchar(255)** e non prevede alcun valore predefinito. Per *resource_name* viene eseguito un confronto binario ed è supportata la distinzione tra maiuscole e minuscole, indipendentemente dalle impostazioni delle regole di confronto del database corrente.
  
**'** *lock_mode* **'**  
Modalità di blocco da ottenere per una risorsa specifica. *lock_mode* è di tipo **nvarchar(32)** e non dispone di valore predefinito. *lock_mode* può avere uno dei valori seguenti: **Shared**, **Update**, **IntentShared**, **IntentExclusive**, **Exclusive**.
  
**'** *lock_owner* **'**  
Proprietario del blocco, ovvero il valore di *lock_owner* al momento della richiesta del blocco. *lock_owner* è di tipo **nvarchar(32)** e il valore può essere **Transaction** (impostazione predefinita) o **Session**. Se si specifica in modo esplicito il valore predefinito o **Transaction**, è necessario eseguire APPLOCK_TEST dall'interno di una transazione.
  
## <a name="return-types"></a>Tipi restituiti
**smallint**
  
## <a name="return-value"></a>Valore restituito
0 se il blocco non può essere concesso al proprietario specificato. In caso contrario, restituisce 1.
  
## <a name="function-properties"></a>Proprietà delle funzioni
**Non deterministica**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>Esempi  
Si supponga che due utenti (**User A** e **User B**) eseguano la sequenza di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] riportata di seguito in sessioni separate.
  
**User A** esegue:
  
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
  
Quindi **User B** esegue:
  
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
  
Quindi **User A** esegue:
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
Quindi **User B** esegue:
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Quindi sia **User A** che **User B** eseguono:
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche
[APPLOCK_MODE &#40;Transact-SQL&#41;](../../t-sql/functions/applock-mode-transact-sql.md)  
[sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
