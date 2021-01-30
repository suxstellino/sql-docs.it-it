---
description: TRY...CATCH (Transact-SQL)
title: TRY...CATCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN_TRY_TSQL
- BEGIN_CATCH_TSQL
- TRY
- BEGIN TRY
- TRY_TSQL
- BEGIN CATCH
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CATCH statement
- uncommittable transactions
- errors [SQL Server], TRY...CATCH
- TRY block [SQL Server]
- XACT_STATE function
- TRY...CATCH [SQL Server]
- BEGIN TRY statement
- CATCH block
ms.assetid: 248df62a-7334-4bca-8262-235a28f4b07f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7e694b4366ac0b0382fb9a29f155bc7fe346653b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176381"
---
# <a name="trycatch-transact-sql"></a>TRY...CATCH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Implementa la gestione degli errori per [!INCLUDE[tsql](../../includes/tsql-md.md)], simile alla gestione delle eccezioni nei linguaggi [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# e [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++. Un gruppo di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] può essere racchiuso in un blocco TRY. Se si verifica un errore nel blocco TRY, il controllo viene passato a un altro gruppo di istruzioni racchiuso in un blocco CATCH.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
BEGIN TRY  
     { sql_statement | statement_block }  
END TRY  
BEGIN CATCH  
     [ { sql_statement | statement_block } ]  
END CATCH  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *sql_statement*  
 Qualsiasi istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 *statement_block*  
 Qualsiasi gruppo di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] in un batch o racchiuse in un blocco BEGIN…END.  
  
## <a name="remarks"></a>Osservazioni  
 Un costrutto TRY...CATCH intercetta tutti gli errori di esecuzione con livello di gravità superiore a 10 che non determinano la chiusura della connessione al database.  
  
 Un blocco TRY deve essere immediatamente seguito da un blocco CATCH associato. L'inclusione di qualsiasi altra istruzione tra le istruzioni END TRY e BEGIN CATCH genera un errore di sintassi.  
  
 Un costrutto TRY...CATCH non può estendersi a più batch. Un costrutto TRY...CATCH non può estendersi a più blocchi di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ad esempio, un costrutto TRY...CATCH non può estendersi a due blocchi BEGIN…END di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e non può estendersi a un costrutto IF…ELSE.  
  
 Se non si verificano errori nel codice racchiuso in un blocco TRY, al termine dell'esecuzione dell'ultima istruzione nel blocco TRY il controllo passa all'istruzione immediatamente successiva all'istruzione END CATCH associata.
 
 Se si verifica un errore nel codice racchiuso in un blocco TRY, il controllo passa alla prima istruzione nel blocco CATCH associato. Al termine del codice nel blocco CATCH, il controllo passa all'istruzione immediatamente successiva all'istruzione END CATCH. 
 
 > [!NOTE] 
 > Se l'istruzione END CATCH è l'ultima istruzione in una stored procedure o un trigger, il controllo viene restituito all'istruzione che ha chiamato la stored procedure o attivato il trigger. 
 
 Gli errori intercettati da un blocco CATCH non vengono restituiti all'applicazione chiamante. Se qualsiasi parte delle informazioni sugli errori deve essere restituita all'applicazione, il codice nel blocco CATCH deve utilizzare a tale scopo meccanismi come i set di risultati SELECT o le istruzioni RAISERROR e PRINT.  
  
 I costrutti TRY...CATCH possono essere nidificati. I costrutti TRY...CATCH nidificati possono essere contenuti in un blocco TRY oppure in un blocco CATCH. Ad esempio, un blocco CATCH può contenere un costrutto TRY...CATCH incorporato per gestire gli errori rilevati dal codice CATCH.  
  
 Gli errori rilevati in un blocco CATCH vengono considerati come gli errori generati in qualsiasi altra posizione. Se il blocco CATCH contiene un costrutto TRY...CATCH nidificato, qualsiasi errore nel blocco TRY nidificato passerà il controllo al blocco CATCH nidificato. Se non esiste un costrutto TRY...CATCH nidificato, l'errore viene restituito al chiamante.  
  
 I costrutti TRY...CATCH rilevano gli errori non gestiti dalle stored procedure o dai trigger eseguiti dal codice in un blocco TRY. In alternativa, le stored procedure o i trigger possono contenere i propri costrutti TRY...CATCH per gestire gli errori generati dal proprio codice. Ad esempio, quando un blocco TRY esegue una stored procedure e si verifica un errore nella stored procedure, l'errore può essere gestito nei modi seguenti:  
  
-   Se la stored procedure non include uno specifico costrutto TRY...CATCH, l'errore restituisce il controllo al blocco CATCH associato al blocco TRY contenente l'istruzione EXECUTE.  
  
-   Se la stored procedure include un costrutto TRY...CATCH, l'errore trasferisce il controllo al blocco CATCH nella stored procedure. Al termine del codice del blocco CATCH, il controllo viene restituito all'istruzione immediatamente successiva all'istruzione EXECUTE che ha chiamato la stored procedure.  
  
 Le istruzioni GOTO non possono essere utilizzate per immettere un blocco TRY o CATCH. Le istruzioni GOTO possono essere utilizzate per passare a un'etichetta all'interno dello stesso blocco TRY o CATCH oppure per uscire da un blocco TRY o CATCH.  
  
 Il costrutto TRY...CATCH non può essere utilizzato in una funzione definita dall'utente.  
  
## <a name="retrieving-error-information"></a>Recupero delle informazioni sugli errori  
 Nell'ambito di un blocco CATCH, è possibile utilizzare le funzioni di sistema seguenti per ottenere informazioni sull'errore che ha causato l'esecuzione del blocco CATCH:  
  
-   [ERROR_NUMBER()](../../t-sql/functions/error-number-transact-sql.md) restituisce il numero dell'errore.  
  
-   [ERROR_SEVERITY()](../../t-sql/functions/error-severity-transact-sql.md) restituisce la gravità.  
  
-   [ERROR_STATE()](../../t-sql/functions/error-state-transact-sql.md) restituisce il numero di contesto dell'errore.  
  
-   [ERROR_PROCEDURE()](../../t-sql/functions/error-procedure-transact-sql.md) restituisce il nome della stored procedure o del trigger in cui si è verificato l'errore.  
  
-   [ERROR_LINE()](../../t-sql/functions/error-line-transact-sql.md) restituisce il numero di riga all'interno della routine che ha causato l'errore.  
  
-   [ERROR_MESSAGE()](../../t-sql/functions/error-message-transact-sql.md) restituisce il testo completo del messaggio di errore. Il testo include i valori forniti da qualsiasi parametro sostituibile, ad esempio lunghezze, nomi di oggetti oppure orari.  
  
Queste funzioni restituiscono NULL se vengono chiamate all'esterno dell'ambito del blocco CATCH. È possibile recuperare informazioni sugli errori utilizzando queste funzioni in qualsiasi posizione all'interno dell'ambito del blocco CATCH. Nello script seguente, ad esempio, viene illustrata una stored procedure contenente funzioni di gestione degli errori. Nel blocco `CATCH` di un costrutto `TRY...CATCH` viene chiamata la stored procedure e vengono restituite informazioni sull'errore.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_GetErrorInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
SELECT  
    ERROR_NUMBER() AS ErrorNumber  
    ,ERROR_SEVERITY() AS ErrorSeverity  
    ,ERROR_STATE() AS ErrorState  
    ,ERROR_PROCEDURE() AS ErrorProcedure  
    ,ERROR_LINE() AS ErrorLine  
    ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
BEGIN TRY  
    -- Generate divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
END CATCH;   
```  
  
 Le funzioni ERROR\_\* possono essere utilizzate anche in un blocco `CATCH` all'interno di una [stored procedure con compilazione nativa](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
## <a name="errors-unaffected-by-a-trycatch-construct"></a>Errori su cui il costrutto TRY...CATCH non ha alcun effetto  
 I costrutti TRY...CATCH non intercettano le condizioni seguenti:  
  
-   Avvisi o messaggi informativi con livello di gravità minore o uguale a 10.  
  
-   Errori con livello di gravità maggiore o uguale a 20 che determinano l'arresto dell'elaborazione dell'attività di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] per la sessione. Se si verifica un errore con livello di gravità maggiore o uguale a 20 e la connessione al database non viene interrotta, l'errore verrà gestito da TRY...CATCH.  
  
-   Situazioni di attenzione, richieste di interruzione dei client o interruzione delle connessioni client.  
  
-   Quando la sessione viene terminata da un amministratore di sistema tramite l'istruzione KILL.  
  
I tipi di errore seguenti non vengono gestiti da un blocco CATCH quando si verificano allo stesso livello di esecuzione del costrutto TRY...CATCH:  
  
-   Errori di compilazione, ad esempio errori di sintassi, che impediscono l'esecuzione di un batch.  
  
-   Errori che si verificano durante la ricompilazione a livello di istruzione, ad esempio errori di risoluzione dei nomi degli oggetti che si verificano dopo la compilazione a causa della risoluzione dei nomi posticipata.  
-   Errori di risoluzione del nome oggetto   

  
Questi errori sono restituiti al livello che ha eseguito il batch, la stored procedure o il trigger.  
  
In caso di errore durante la compilazione o durante la ricompilazione a livello di istruzione a un livello di esecuzione inferiore (ad esempio, durante l'esecuzione di sp_executesql o di una stored procedure definita dall'utente) all'interno del blocco TRY, l'errore si verifica a un livello inferiore rispetto al costrutto TRY...CATCH e verrà gestito dal blocco CATCH associato.  
  
Nell'esempio seguente viene illustrato come un errore di risoluzione dei nomi degli oggetti generato da un'istruzione `SELECT` non venga intercettato dal costrutto `TRY...CATCH`, ma venga intercettato dal blocco `CATCH` quando la stessa istruzione `SELECT` viene eseguita all'interno di una stored procedure.  
  
```sql  
BEGIN TRY  
    -- Table does not exist; object name resolution  
    -- error not caught.  
    SELECT * FROM NonexistentTable;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
       ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH  
```  
  
 L'errore non viene intercettato e il controllo passa dal costrutto `TRY...CATCH` al livello immediatamente superiore.  
  
 Eseguendo l'istruzione `SELECT` all'interno di una stored procedure, l'errore si verificherà a un livello inferiore rispetto al blocco `TRY`. L'errore verrà gestito dal costrutto `TRY...CATCH`.  
  
```sql  
-- Verify that the stored procedure does not exist.  
IF OBJECT_ID ( N'usp_ExampleProc', N'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that will cause an   
-- object resolution error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT * FROM NonexistentTable;  
GO  
  
BEGIN TRY  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
```  
  
## <a name="uncommittable-transactions-and-xact_state"></a>Transazioni bloccate e XACT_STATE  
 Se un errore generato in un blocco TRY invalida lo stato della transazione corrente, la transazione viene classificata come transazione bloccata. Un errore che normalmente termina una transazione all'esterno di un blocco TRY causa il passaggio della transazione a uno stato di transazione bloccata quando si verifica all'interno di un blocco TRY. Una transazione bloccata può eseguire soltanto le operazioni di lettura o ROLLBACK TRANSACTION. La transazione non può eseguire istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che generano una operazione di scrittura o COMMIT TRANSACTION. La funzione XACT_STATE restituisce un valore -1 se una transazione è stata classificata come transazione bloccata. Al termine di un batch, [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue automaticamente il rollback di qualsiasi transazione bloccata attiva. Se non sono stati inviati messaggi di errore al momento dell'attivazione dello stato di blocco per la transazione, al termine dell'esecuzione del batch, verrà inviato un messaggio di errore all'applicazione client. Viene così indicato che è stata rilevata una transazione bloccata e ne è stato eseguito il rollback.  
  
 Per altre informazioni sulle transazioni di cui non è possibile eseguire il commit e la funzione XACT_STATE, vedere [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-trycatch"></a>R. Utilizzo di TRY...CATCH  
 Nell'esempio seguente viene illustrata un'istruzione `SELECT` che genera un errore di divisione per zero. L'errore determina il passaggio dell'esecuzione al blocco `CATCH` associato.  
  
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
  
### <a name="b-using-trycatch-in-a-transaction"></a>B. Utilizzo di TRY...CATCH in una transazione  
 Nell'esempio seguente viene illustrato il funzionamento di un blocco `TRY...CATCH` all'interno di una transazione. L'istruzione all'interno del blocco `TRY` genera un errore di violazione di vincolo.  
  
```sql  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Generate a constraint violation error.  
    DELETE FROM Production.Product  
    WHERE ProductID = 980;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
### <a name="c-using-trycatch-with-xact_state"></a>C. Utilizzo di TRY...CATCH con XACT_STATE  
 Nell'esempio seguente viene illustrato come utilizzare il costrutto `TRY...CATCH` per gestire gli errori che si verificano all'interno di una transazione. La funzione `XACT_STATE` determina se è necessario eseguire il commit o il rollback della transazione. In this example, l'evento `SET XACT_ABORT` è `ON`. Rende pertanto bloccata la transazione quando si verifica l'errore di violazione di vincolo.  
  
```sql  
-- Check to see whether this stored procedure exists.  
IF OBJECT_ID (N'usp_GetErrorInfo', N'P') IS NOT NULL  
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_LINE () AS ErrorLine  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
-- SET XACT_ABORT ON will cause the transaction to be uncommittable  
-- when the constraint violation occurs.   
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the DELETE statement succeeds, commit the transaction.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
  
    -- Test XACT_STATE:  
        -- If 1, the transaction is committable.  
        -- If -1, the transaction is uncommittable and should   
        --     be rolled back.  
        -- XACT_STATE = 0 means that there is no transaction and  
        --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT  
            N'The transaction is in an uncommittable state.' +  
            'Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is committable.
    -- You may want to commit a transaction in a catch block if you want to commit changes to statements that ran prior to the error.
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT  
            N'The transaction is committable.' +  
            'Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-trycatch"></a>D. Utilizzo di TRY...CATCH  
 Nell'esempio seguente viene illustrata un'istruzione `SELECT` che genera un errore di divisione per zero. L'errore determina il passaggio dell'esecuzione al blocco `CATCH` associato.  
  
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
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)   
 [Gravità degli errori del motore di database](../../relational-databases/errors-events/database-engine-error-severities.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [GOTO &#40;Transact-SQL&#41;](../../t-sql/language-elements/goto-transact-sql.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)   
 [SET XACT_ABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-xact-abort-transact-sql.md)  
  
