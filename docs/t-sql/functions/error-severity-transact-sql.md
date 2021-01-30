---
description: ERROR_SEVERITY (Transact-SQL)
title: ERROR_SEVERITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_SEVERITY_TSQL
- ERROR_SEVERITY
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], severity
- messages [SQL Server], severity
- TRY...CATCH [SQL Server]
- CATCH block
- ERROR_SEVERITY function
ms.assetid: 50228f2f-6949-4d2e-8e43-fad11bf973ab
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 937b3737d31aed7dbc34f210e5a64e043527fde3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158822"
---
# <a name="error_severity-transact-sql"></a>ERROR_SEVERITY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il valore di gravità dell'errore dove questo si verifica, se l'errore ha causato l'esecuzione del blocco CATCH di un costrutto TRY...CATCH.  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
ERROR_SEVERITY ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **int**  
  
## <a name="return-value"></a>Valore restituito  
Se chiamata in un blocco CATCH quando si verifica un errore, `ERROR_SEVERITY` restituisce il valore di gravità dell'errore che ha attivato l'esecuzione del blocco `CATCH`.  

`ERROR_SEVERITY` restituisce NULL se chiamata all'esterno dell'ambito di un blocco CATCH.  
  
## <a name="remarks"></a>Commenti  
`ERROR_SEVERITY` supporta le chiamate da un qualsiasi punto nell'ambito di un blocco CATCH.  
  
`ERROR_SEVERITY` restituisce il valore di gravità di un errore, indipendentemente dal numero di esecuzioni o dalla posizione in cui viene eseguita nell'ambito del blocco `CATCH`. Questo tipo di comportamento è in contrasto con una funzione come @@ERROR, che restituisce solo un numero di errore nell'istruzione immediatamente successiva a quella che ha provocato un errore.  
  
`ERROR_SEVERITY` opera in genere in un blocco `CATCH` annidato. `ERROR_SEVERITY` restituisce il valore di gravità dell'errore specifico dell'ambito del blocco `CATCH` che ha fatto riferimento a tale blocco `CATCH`. Ad esempio, il blocco `CATCH` di un costrutto esterno TRY...CATCH potrebbe includere un costrutto `TRY...CATCH` interno. In tale blocco `CATCH` interno `ERROR_SEVERITY` restituisce il valore di gravità dell'errore che ha richiamato il blocco `CATCH` interno. Se `ERROR_SEVERITY` viene eseguito nel blocco `CATCH` esterno, restituisce il valore di gravità dell'errore che ha richiamato il blocco `CATCH` esterno.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-error_severity-in-a-catch-block"></a>R. Utilizzo di ERROR_SEVERITY in un blocco CATCH  
In questo esempio viene illustrata una stored procedure che genera un errore di divisione per zero. `ERROR_SEVERITY` restituisce il valore di gravità di tale errore.  

```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_SEVERITY() AS ErrorSeverity;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorSeverity
-------------
16

(1 row(s) affected)

```  
  
### <a name="b-using-error_severity-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilizzo di ERROR_SEVERITY in un blocco CATCH con altri strumenti di gestione degli errori  
In questo esempio viene illustrata un'istruzione `SELECT` che genera un errore di divisione per zero. La stored procedure restituisce informazioni sull'errore.  

```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure  ErrorLine   ErrorMessage
----------- ------------- ----------- --------------- ----------- ----------------------------------
8134        16            1           NULL            4           Divide by zero error encountered.

(1 row(s) affected)

```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
 [Guida di riferimento ad errori e eventi &#40;motore di database&#41;](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

