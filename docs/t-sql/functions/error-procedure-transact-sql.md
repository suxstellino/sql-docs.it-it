---
description: ERROR_PROCEDURE (Transact-SQL)
title: ERROR_PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_PROCEDURE_TSQL
- ERROR_PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ERROR_PROCEDURE function
- messages [SQL Server], trigger where occurred
- errors [SQL Server], stored procedure where occurred
- TRY...CATCH [SQL Server]
- CATCH block
- messages [SQL Server], stored procedure where occurred
- errors [SQL Server], trigger where occurred
ms.assetid: b81edbf0-856a-498f-ba87-48ff1426d980
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 53779b6fd942f5ffdaf60532463ddba19264d15d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747551"
---
# <a name="error_procedure-transact-sql"></a>ERROR_PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]  

Questa funzione restituisce il nome della stored procedure o del trigger in cui si verifica un errore se tale errore ha causato l'esecuzione del blocco CATCH di un costrutto TRY...CATCH. 
- Dalla versione SQL Server 2017 alla [versione corrente](../../sql-server/what-s-new-in-sql-server-ver15.md) restituisce schema_name.stored_procedure_name
- SQL Server 2016 restituisce stored_procedure_name

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
ERROR_PROCEDURE ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
**nvarchar(128)**  
  
## <a name="return-value"></a>Valore restituito  
Quando viene chiamato in un blocco CATCH, `ERROR_PROCEDURE` restituisce il nome della stored procedure o del trigger in cui si è verificato l'errore.
  
`ERROR_PROCEDURE` restituisce NULL se l'errore non si è verificato all'interno di una stored procedure o un trigger.  
  
`ERROR_PROCEDURE` restituisce NULL quando viene chiamata all'esterno dell'ambito di un blocco CATCH.  
  
## <a name="remarks"></a>Osservazioni  
`ERROR_PROCEDURE` supporta le chiamate da un qualsiasi punto nell'ambito di un blocco CATCH.  
  
`ERROR_PROCEDURE` restituisce il nome della stored procedure o del trigger in cui si verifica un errore, indipendentemente dal numero di esecuzioni o dalla posizione in cui viene eseguita nell'ambito del blocco `CATCH`. Questo tipo di comportamento è in contrasto con una funzione come @@ERROR, che restituisce solo un numero di errore nell'istruzione immediatamente successiva a quella che ha provocato un errore.  
   
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
### <a name="a-using-error_procedure-in-a-catch-block"></a>R. Utilizzo di ERROR_PROCEDURE in un blocco CATCH  
In questo esempio viene illustrata una stored procedure che genera un errore di divisione per zero. `ERROR_PROCEDURE` restituisce il nome della stored procedure in cui si è verificato l'errore.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_PROCEDURE() AS ErrorProcedure;  
END CATCH;  
GO  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```  
-----------

(0 row(s) affected)

ErrorProcedure
--------------------
usp_ExampleProc

(1 row(s) affected)
```  

  
### <a name="b-using-error_procedure-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilizzo di ERROR_PROCEDURE in un blocco CATCH con altri strumenti di gestione degli errori  
In questo esempio viene illustrata una stored procedure che genera un errore di divisione per zero. Insieme al nome della stored procedure in cui si è verificato l'errore, la stored restituisce informazioni sull'errore.  
  
```sql
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_MESSAGE() AS ErrorMessage,  
        ERROR_LINE() AS ErrorLine;  
        END CATCH;  
GO
``` 

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
``` 
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure   ErrorMessage                       ErrorLine
----------- ------------- ----------- ---------------- ---------------------------------- -----------
8134        16            1           usp_ExampleProc  Divide by zero error encountered.  6

(1 row(s) affected)

``` 

  
## <a name="see-also"></a>Vedere anche  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  

