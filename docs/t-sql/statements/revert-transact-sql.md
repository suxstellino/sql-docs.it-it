---
description: REVERT (Transact-SQL)
title: REVERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVERT_TSQL
- REVERT
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- context switching [SQL Server], reverting
- reverting execution context
- REVERT WITH COOKIE statement
- execution context [SQL Server]
- COOKIE clause
ms.assetid: 4688b17a-dfd1-4f03-8db4-273a401f879f
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: f8966a7c1204ec61f2271d4cd44a559a593473a2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209978"
---
# <a name="revert-transact-sql"></a>REVERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]   

  Riporta il contesto di esecuzione al chiamante dell'ultima istruzione EXECUTE AS.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
REVERT  
    [ WITH COOKIE = @varbinary_variable ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 WITH COOKIE = @*varbinary_variable*  
 Specifica il cookie creato in un'istruzione autonoma [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) corrispondente. *\@varbinary_variable* è **varbinary(100)** .  
  
## <a name="remarks"></a>Commenti  
 È possibile specificare REVERT all'interno di un modulo, ad esempio una stored procedure o una funzione definita dall'utente, oppure come un'istruzione autonoma. Se specificata all'interno di un modulo, l'istruzione REVERT è applicabile solo alle istruzioni EXECUTE AS definite nel modulo. Ad esempio, la stored procedure seguente esegue un'istruzione `EXECUTE AS` seguita da un'istruzione `REVERT`.  
  
```sql  
CREATE PROCEDURE dbo.usp_myproc   
  WITH EXECUTE AS CALLER  
AS   
    SELECT SUSER_NAME(), USER_NAME();  
    EXECUTE AS USER = 'guest';  
    SELECT SUSER_NAME(), USER_NAME();  
    REVERT;  
    SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
 Si supponga che nella sessione in cui viene eseguita la stored procedure il contesto di esecuzione della sessione venga modificato in modo esplicito in `login1`, come illustrato nell'esempio seguente.  
  
```sql 
  -- Sets the execution context of the session to 'login1'.  
EXECUTE AS LOGIN = 'login1';  
GO  
EXECUTE dbo.usp_myproc;   
```  
  
 L'istruzione `REVERT` definita all'interno di `usp_myproc` cambia il contesto di esecuzione impostato all'interno del modulo, ma non quello impostato al suo esterno. In sintesi, il contesto di esecuzione della sessione rimane impostato su `login1`.  
  
 Se specificata come istruzione autonoma, l'istruzione REVERT è applicabile alle istruzioni EXECUTE AS definite all'interno di un batch o una sessione. L'istruzione REVERT non ha alcun effetto se la corrispondente istruzione EXECUTE AS contiene la clausola WITH NO REVERT. In questo caso, il contesto di esecuzione rimane valido fino all'eliminazione della sessione.  
  
## <a name="using-revert-with-cookie"></a>Utilizzo di REVERT WITH COOKIE  
 L'istruzione EXECUTE AS usata per impostare il contesto di esecuzione di una sessione può includere la clausola facoltativa WITH NO REVERT COOKIE = @*varbinary_variable*. Quando questa istruzione viene eseguita, [!INCLUDE[ssDE](../../includes/ssde-md.md)] passa il cookie a @*varbinary_variable*. Il contesto di esecuzione impostato da tale istruzione può essere riportato al contesto precedente solo se l'istruzione REVERT WITH COOKIE = @*varbinary_variable* contiene il valore *\@varbinary_variable* corretto.  
  
 Questo meccanismo risulta utile in un ambiente in cui vengono utilizzati pool di connessioni. Tramite i pool di connessioni vengono gestiti i gruppi di connessioni al database in modo che tali connessioni possano essere riutilizzate dalle applicazioni tra più utenti finali. Poiché il valore passato a *\@varbinary_variable* è noto solo al chiamante dell'istruzione EXECUTE AS (in questo caso l'applicazione), il chiamante è in grado di garantire che il contesto di esecuzione stabilito non venga modificato dall'utente finale che chiama l'applicazione. Dopo il ripristino del contesto di esecuzione l'applicazione può cambiare il contesto a un'altra entità.  
  
## <a name="permissions"></a>Autorizzazioni  
 Non sono necessarie autorizzazioni.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-execute-as-and-revert-to-switch-context"></a>R. Utilizzo di EXECUTE AS e REVERT per cambiare contesto  
 Nell'esempio seguente viene creato uno stack di contesti di esecuzione utilizzando più entità. Viene quindi utilizzata l'istruzione REVERT per ripristinare il contesto di esecuzione al chiamante precedente. L'istruzione REVERT viene eseguita più volte per innalzare di livello lo stack finché il contesto di esecuzione viene impostato sul chiamante originale.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create two temporary principals.  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
-- Give IMPERSONATE permissions on user2 to user1  
-- so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
-- Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
-- Verify that the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
-- Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
-- Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1, and login2.  
-- The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
-- Display the current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
-- Display the current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
-- Remove the temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Utilizzo della clausola WITH COOKIE  
 Nell'esempio seguente il contesto di esecuzione di una sessione viene impostato su un utente specifico e viene specificata la clausola WITH NO REVERT COOKIE = @*varbinary_variable*. Nell'istruzione `REVERT` è necessario specificare il valore passato alla variabile `@cookie` nell'istruzione `EXECUTE AS` per ripristinare correttamente il contesto al chiamante originale. Per eseguire questo esempio, l'account di accesso `login1` e l'utente `user1` creato nell'esempio A devono esistere.  
  
```sql 
DECLARE @cookie VARBINARY(100);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie somewhere safe in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(100);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)  
  
  
