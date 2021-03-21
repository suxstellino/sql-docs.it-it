---
description: XACT_STATE (Transact-SQL)
title: XACT_STATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- XACT_STATE
- XACT_STATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- states [SQL Server], transactions
- uncommittable transactions
- sessions [SQL Server], active transactions
- XACT_STATE function
- transactions [SQL Server], state
- active transactions
ms.assetid: e9300827-e793-4eb6-9042-ffa0204aeb50
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfaf2b06022579eaeb58fdfe7c2aad13c8b0df83
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754001"
---
# <a name="xact_state-transact-sql"></a>XACT_STATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Funzione scalare che segnala lo stato della transazione utente di una richiesta attualmente in esecuzione. XACT_STATE indica se la richiesta include una transazione utente attiva e se è possibile eseguire il commit di tale transazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
XACT_STATE()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="return-type"></a>Tipo restituito  
 **smallint**  
  
## <a name="remarks"></a>Osservazioni  
 XACT_STATE restituisce i valori seguenti.  
  
|Valore restituito|Significato|  
|------------------|-------------|  
|1|La richiesta corrente include una transazione utente attiva. La richiesta può eseguire qualsiasi azione, tra cui la scrittura di dati e il commit della transazione.|  
|0|La richiesta corrente non include una transazione utente attiva.|  
|-1|La richiesta corrente include una transazione utente attiva, ma si è verificato un errore che ne ha causato la classificazione come transazione bloccata. La richiesta non può eseguire il commit della transazione né eseguirne il rollback fino a un punto di salvataggio. L'unica operazione consentita è la richiesta di un rollback completo della transazione. Finché non verrà eseguito il rollback della transazione, non sarà possibile eseguire operazioni di scrittura e saranno consentite solo operazioni di lettura. Dopo il rollback della transazione, la richiesta potrà eseguire operazioni sia di lettura che di scrittura e potrà avviare una nuova transazione.<br /><br /> Al termine dell'esecuzione del batch più esterno, [!INCLUDE[ssDE](../../includes/ssde-md.md)] eseguirà automaticamente il rollback di tutte le transazioni attive di cui non è possibile eseguire il commit. Se non sono stati inviati messaggi di errore al momento dell'attivazione dello stato di blocco per la transazione, al termine dell'esecuzione del batch, verrà inviato un messaggio di errore all'applicazione client. Questo messaggio indica che è stata rilevata una transazione di cui non è possibile eseguire il commit e che ne è stato eseguito il rollback.|  
  
 Per verificare se la richiesta corrente include una transazione utente attiva, è possibile usare entrambe le funzioni XACT_STATE e @@TRANCOUNT. Non è possibile usare @@TRANCOUNT per determinare se la transazione è stata classificata come transazione di cui non è possibile eseguire il commit. Non è possibile utilizzare XACT_STATE per determinare se esistono transazioni nidificate.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzata la funzione `XACT_STATE` nel blocco `CATCH` di un costrutto `TRY...CATCH` per determinare se è necessario eseguire il commit o il rollback di una transazione. Poiché il valore di `SET XACT_ABORT` è `ON`, l'errore di violazione del vincolo attiva lo stato di blocco per la transazione.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- SET XACT_ABORT ON will render the transaction uncommittable  
-- when the constraint violation occurs.  
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the delete operation succeeds, commit the transaction. The CATCH  
    -- block will not execute.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Test XACT_STATE for 0, 1, or -1.  
    -- If 1, the transaction is committable.  
    -- If -1, the transaction is uncommittable and should   
    --     be rolled back.  
    -- XACT_STATE = 0 means there is no transaction and  
    --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT 'The transaction is in an uncommittable state.' +  
              ' Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is active and valid.  
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT 'The transaction is committable.' +   
              ' Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  
