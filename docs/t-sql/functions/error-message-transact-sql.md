---
description: ERROR_MESSAGE (Transact-SQL)
title: ERROR_MESSAGE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_MESSAGE_TSQL
- ERROR_MESSAGE
dev_langs:
- TSQL
helpviewer_keywords:
- ERROR_MESSAGE function
- errors [SQL Server], text of
- messages [SQL Server], text of
- TRY...CATCH [SQL Server]
- CATCH block
ms.assetid: f32877a6-5f17-418c-a32c-5a1a344b3c45
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5e2a943c525a637d6d6c092593e55782ba677d2e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747561"
---
# <a name="error_message-transact-sql"></a>ERROR_MESSAGE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il testo del messaggio dell'errore che ha causato l'esecuzione del blocco CATCH di un costrutto TRY...CATCH.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
ERROR_MESSAGE ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **nvarchar(4000)**  
  
## <a name="return-value"></a>Valore restituito  
Quando viene chiamata in un blocco CATCH, `ERROR_MESSAGE` restituisce il testo completo del messaggio di errore che ha causato l'esecuzione del blocco `CATCH`. Il testo include i valori forniti da qualsiasi parametro sostituibile, ad esempio lunghezze, nomi di oggetti oppure orari.  
  
`ERROR_MESSAGE` restituisce NULL quando viene chiamata all'esterno dell'ambito di un blocco CATCH.  
  
## <a name="remarks"></a>Osservazioni  
`ERROR_MESSAGE` supporta le chiamate da un qualsiasi punto nell'ambito di un blocco CATCH.  
  
`ERROR_MESSAGE` restituisce un messaggio di errore pertinente indipendentemente dal numero di esecuzioni o dalla posizione in cui viene eseguita nell'ambito del blocco `CATCH`. Questo tipo di comportamento è in contrasto con una funzione come @@ERROR, che restituisce solo un numero di errore nell'istruzione immediatamente successiva a quella che ha provocato un errore.  
  
Nei blocchi `CATCH` annidati `ERROR_MESSAGE` restituisce il messaggio di errore specifico dell'ambito del blocco `CATCH` che ha fatto riferimento a tale blocco `CATCH`. Ad esempio, il blocco `CATCH` di un costrutto esterno TRY...CATCH potrebbe includere un costrutto `TRY...CATCH` interno. In tale blocco `CATCH` interno `ERROR_MESSAGE` restituisce il messaggio dall'errore che ha richiamato il blocco `CATCH` interno. Se `ERROR_MESSAGE` viene eseguito nel blocco `CATCH` esterno, restituisce il messaggio dall'errore che ha richiamato il blocco `CATCH` esterno.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-error_message-in-a-catch-block"></a>R. Utilizzo di ERROR_MESSAGE in un blocco CATCH  
In questo esempio viene illustrata un'istruzione `SELECT` che genera un errore di divisione per zero. Il blocco `CATCH` restituisce il messaggio di errore.  
  
```sql   
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 
```
-----------

(0 row(s) affected)

ErrorMessage
----------------------------------
Divide by zero error encountered.

(1 row(s) affected)

```  
  
### <a name="b-using-error_message-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilizzo di ERROR_MESSAGE in un blocco CATCH con altri strumenti di gestione degli errori  
In questo esempio viene illustrata un'istruzione `SELECT` che genera un errore di divisione per zero. Con il messaggio di errore, il blocco `CATCH` restituisce le informazioni su tale errore.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 
```
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure  ErrorLine  ErrorMessage
----------- ------------- ----------- --------------- ---------- ----------------------------------
8134        16            1           NULL            4          Divide by zero error encountered.

(1 row(s) affected)

```
  
## <a name="see-also"></a>Vedere anche  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)     
 [Guida di riferimento ad errori e eventi &#40;motore di database&#41;](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

