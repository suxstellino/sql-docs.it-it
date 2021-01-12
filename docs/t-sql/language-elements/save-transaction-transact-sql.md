---
description: SAVE TRANSACTION (Transact-SQL)
title: SAVE TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SAVE
- SAVE_TSQL
- SAVE_TRANSACTION_TSQL
- SAVE TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- rolling back transactions, SAVE TRANSACTION
- SAVE TRANSACTION statement
- transactions [SQL Server], rolling back
- marking transactions [SQL Server]
- savepoints [SQL Server]
- marked transactions [SQL Server], SAVE TRANSACTION statement
- duplicate savepoints
ms.assetid: b953c3f1-f96d-42f1-95a2-30e314292b35
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4f7d9aac54c323527c91f671d25e9ebdf436f6f7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100316"
---
# <a name="save-transaction-transact-sql"></a>SAVE TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Imposta un punto di salvataggio in una transazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

 ## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SAVE { TRAN | TRANSACTION } { savepoint_name | @savepoint_variable }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *savepoint_name*  
 Nome assegnato al punto di salvataggio. I nomi di punto di salvataggio devono essere conformi alle regole per gli identificatori, ma non possono contenere più di 32 caratteri. In *savepoint_name* viene sempre applicata la distinzione tra maiuscole e minuscole, anche quando l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non prevede distinzione tra maiuscole e minuscole.  
  
 @*savepoint_variable*  
 Nome di una variabile definita dall'utente contenente un nome di punto di salvataggio valido. La variabile deve essere dichiarata con un tipo di dati **char**, **varchar**, **nchar** o **nvarchar**. È possibile passare alla variabile più di 32 caratteri, ma in tal caso verranno utilizzati solo i primi 32.  
  
## <a name="remarks"></a>Commenti  
 Un utente può impostare un punto di salvataggio o marcatore all'interno di una transazione. Il punto di salvataggio consente di contrassegnare il punto fino a cui conservare una transazione se parte della transazione viene annullata in modo condizionale. Se si esegue il rollback di una transazione fino a un punto di salvataggio, è necessario che la transazione venga completata (se necessario con più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e con un'istruzione COMMIT TRANSACTION) oppure annullata completamente (tramite il rollback fino all'inizio della transazione). Per annullare un'intera transazione, usare la sintassi ROLLBACK TRANSACTION *transaction_name*. In tal caso, verranno annullate tutte le istruzioni o procedure della transazione.  
  
 In una transazione è consentito l'utilizzo di nomi di punto di salvataggio duplicati. Un'istruzione ROLLBACK TRANSACTION contenente un nome di punto di salvataggio, tuttavia, eseguirà il rollback della transazione solo fino all'istruzione SAVE TRANSACTION più recente che utilizza tale nome.  
  
 L'istruzione SAVE TRANSACTION non è supportata in transazioni distribuite avviate in modo esplicito con l'istruzione BEGIN DISTRIBUTED TRANSACTION o risultanti dall'escalation di una transazione locale.  
  
> [!IMPORTANT]  
>  Un'istruzione ROLLBACK TRANSACTION che specifica un argomento savepoint_name rilascia tutti i blocchi acquisiti oltre il punto di salvataggio, ad eccezione di escalation e conversioni. Tali blocchi non vengono rilasciati né riconvertiti alla loro precedente modalità di blocco.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato come utilizzare un punto di salvataggio di una transazione per eseguire il rollback delle sole modifiche apportate da una stored procedure nel caso in cui sia avviata una transazione attiva prima dell'esecuzione della stored procedure.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
           WHERE name = N'SaveTranExample')  
    DROP PROCEDURE SaveTranExample;  
GO  
CREATE PROCEDURE SaveTranExample  
    @InputCandidateID INT  
AS  
    -- Detect whether the procedure was called  
    -- from an active transaction and save  
    -- that for later use.  
    -- In the procedure, @TranCounter = 0  
    -- means there was no active transaction  
    -- and the procedure started one.  
    -- @TranCounter > 0 means an active  
    -- transaction was started before the   
    -- procedure was called.  
    DECLARE @TranCounter INT;  
    SET @TranCounter = @@TRANCOUNT;  
    IF @TranCounter > 0  
        -- Procedure called when there is  
        -- an active transaction.  
        -- Create a savepoint to be able  
        -- to roll back only the work done  
        -- in the procedure if there is an  
        -- error.  
        SAVE TRANSACTION ProcedureSave;  
    ELSE  
        -- Procedure must start its own  
        -- transaction.  
        BEGIN TRANSACTION;  
    -- Modify database.  
    BEGIN TRY  
        DELETE HumanResources.JobCandidate  
            WHERE JobCandidateID = @InputCandidateID;  
        -- Get here if no errors; must commit  
        -- any transaction started in the  
        -- procedure, but not commit a transaction  
        -- started before the transaction was called.  
        IF @TranCounter = 0  
            -- @TranCounter = 0 means no transaction was  
            -- started before the procedure was called.  
            -- The procedure must commit the transaction  
            -- it started.  
            COMMIT TRANSACTION;  
    END TRY  
    BEGIN CATCH  
        -- An error occurred; must determine  
        -- which type of rollback will roll  
        -- back only the work done in the  
        -- procedure.  
        IF @TranCounter = 0  
            -- Transaction started in procedure.  
            -- Roll back complete transaction.  
            ROLLBACK TRANSACTION;  
        ELSE  
            -- Transaction started before procedure  
            -- called, do not roll back modifications  
            -- made before the procedure was called.  
            IF XACT_STATE() <> -1  
                -- If the transaction is still valid, just  
                -- roll back to the savepoint set at the  
                -- start of the stored procedure.  
                ROLLBACK TRANSACTION ProcedureSave;  
                -- If the transaction is uncommitable, a  
                -- rollback to the savepoint is not allowed  
                -- because the savepoint rollback writes to  
                -- the log. Just return to the caller, which  
                -- should roll back the outer transaction.  
  
        -- After the appropriate rollback, echo error  
        -- information to the caller.  
        DECLARE @ErrorMessage NVARCHAR(4000);  
        DECLARE @ErrorSeverity INT;  
        DECLARE @ErrorState INT;  
  
        SELECT @ErrorMessage = ERROR_MESSAGE();  
        SELECT @ErrorSeverity = ERROR_SEVERITY();  
        SELECT @ErrorState = ERROR_STATE();  
  
        RAISERROR (@ErrorMessage, -- Message text.  
                   @ErrorSeverity, -- Severity.  
                   @ErrorState -- State.  
                   );  
    END CATCH  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)  
  
  
