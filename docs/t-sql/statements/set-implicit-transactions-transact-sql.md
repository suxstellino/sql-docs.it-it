---
description: SET IMPLICIT_TRANSACTIONS (Transact-SQL)
title: SET IMPLICIT_TRANSACTIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IMPLICIT_TRANSACTIONS
- SET IMPLICIT_TRANSACTIONS
- IMPLICIT_TRANSACTIONS_TSQL
- SET_IMPLICIT_TRANSACTIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- implicit transactions
- transactions [SQL Server], implicit
- connections [SQL Server], implicit transaction mode
- SET IMPLICIT_TRANSACTIONS statement
- IMPLICIT_TRANSACTIONS option
ms.assetid: a300ac43-e4c0-4329-8b79-a1a05e63370a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38b45a33588220591326841a7b22f117022627e5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754301"
---
# <a name="set-implicit_transactions-transact-sql"></a>SET IMPLICIT_TRANSACTIONS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Imposta la modalità BEGIN TRANSACTION su *implicita* per la connessione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SET IMPLICIT_TRANSACTIONS { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 Quando è impostata su ON, il sistema è in modalità di transazione *implicita*. Ciò significa che se @@TRANCOUNT = 0, una qualsiasi delle istruzioni Transact-SQL seguenti avvia una nuova transazione. Equivale a un'istruzione BEGIN TRANSACTION non visibile eseguita per prima:  

:::row:::
    :::column:::
        MODIFICA TABELLA
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        BEGIN TRANSACTION
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT (vedere l'eccezione riportata di seguito)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DELETE
    :::column-end:::
    :::column:::
        MERGE
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

 Quando è impostata su OFF, ognuna delle istruzioni T-SQL precedenti è vincolata da un'istruzione BEGIN TRANSACTION e COMMIT TRANSACTION non visibile. Quando è impostata su OFF, la modalità di transazione è nota come *autocommit*. Se il codice T-SQL esegue un'istruzione BEGIN TRANSACTION in modo visibile, la modalità di transazione è *esplicita*.  
  
 È necessario comprendere e chiarire alcuni punti:  
  
-   Quando la modalità transazione è implicita, non viene eseguita alcuna istruzione BEGIN TRANSACTION non visibile se @@trancount > 0. Le istruzioni BEGIN TRANSACTION esplicite continuano tuttavia a incrementare @@TRANCOUNT.  
  
-   Quando le istruzioni INSERT e le altre attività nell'unità di lavoro sono state completate, è necessario eseguire le istruzioni COMMIT TRANSACTION fino a quando @@TRANCOUNT non viene di nuovo ridotto a 0. Oppure è possibile eseguire un'istruzione ROLLBACK TRANSACTION.  
  
-   Le istruzioni SELECT che non effettuano la selezione da una tabella non avviano transazioni implicite. Ad esempio `SELECT GETDATE();` o `SELECT 1, 'ABC';` non richiedono transazioni.  
  
-   Le transazioni implicite potrebbero essere inaspettatamente impostate su ON a causa dei valori ANSI predefiniti. Per informazioni dettagliate, vedere [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md).  
  
     IMPLICIT_TRANSACTIONS ON non è un'impostazione molto diffusa. Nella maggior parte dei casi, l'impostazione di IMPLICIT_TRANSACTIONS su ON dipende dal fatto che si è scelto di impostare SET ANSI_DEFAULTS su ON.  
  
-   Il provider OLE DB per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il driver ODBC per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client impostano automaticamente l'opzione IMPLICIT_TRANSACTIONS su OFF al momento della connessione. Per le connessioni con il provider gestito SQLClient e le richieste SOAP ricevute tramite gli endpoint HTTP, l'impostazione predefinita di SET IMPLICIT_TRANSACTIONS è OFF.  
  
 Per visualizzare l'impostazione corrente per IMPLICIT_TRANSACTIONS, eseguire la query riportata di seguito.  
  
```sql
DECLARE @IMPLICIT_TRANSACTIONS VARCHAR(3) = 'OFF';  
IF ( (2 & @@OPTIONS) = 2 ) SET @IMPLICIT_TRANSACTIONS = 'ON';  
SELECT @IMPLICIT_TRANSACTIONS AS IMPLICIT_TRANSACTIONS;  
```  
  
## <a name="examples"></a>Esempi  
 Lo script Transact-SQL seguente esegue alcuni test case diversi. Viene anche fornito l'output di testo che illustra il comportamento e i risultati dettagliati di ogni test case.  
  
```sql  
-- Transact-SQL.  
-- Preparations.  
SET NOCOUNT ON;  
SET IMPLICIT_TRANSACTIONS OFF;  
GO  
WHILE (@@TranCount > 0) COMMIT TRANSACTION;  
GO  
IF (OBJECT_ID(N'dbo.t1',N'U') IS NOT NULL) DROP TABLE dbo.t1;  
GO  
CREATE table dbo.t1 (a INT);  
GO  
  
PRINT N'-------- [Test A] ---- OFF ----';  
PRINT N'[A.01] Now, SET IMPLICIT_TRANSACTIONS OFF.';  
PRINT N'[A.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS OFF;  
GO 
INSERT INTO dbo.t1 VALUES (11);  
INSERT INTO dbo.t1 VALUES (12);  
PRINT N'[A.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO  
  
PRINT N' ';  
PRINT N'-------- [Test B] ---- ON ----';  
PRINT N'[B.01] Now, SET IMPLICIT_TRANSACTIONS ON.';  
PRINT N'[B.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS ON;  
GO
INSERT INTO dbo.t1 VALUES (21);  
INSERT INTO dbo.t1 VALUES (22);  
PRINT N'[B.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO 
COMMIT TRANSACTION;  
PRINT N'[B.04] @@TranCount, after COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO
  
PRINT N' ';  
PRINT N'-------- [Test C] ---- ON, then BEGIN TRAN ----';  
PRINT N'[C.01] Now, SET IMPLICIT_TRANSACTIONS ON.';  
PRINT N'[C.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS ON;  
GO  
BEGIN TRANSACTION;  
INSERT INTO dbo.t1 VALUES (31);  
INSERT INTO dbo.t1 VALUES (32);  
PRINT N'[C.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO  
COMMIT TRANSACTION;  
PRINT N'[C.04] @@TranCount, after a COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
COMMIT TRANSACTION;  
PRINT N'[C.05] @@TranCount, after another COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO
  
PRINT N' ';  
PRINT N'-------- [Test D] ---- ON, INSERT, BEGIN TRAN, INSERT ----';  
PRINT N'[D.01] Now, SET IMPLICIT_TRANSACTIONS ON.';  
PRINT N'[D.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS ON;  
GO 
INSERT INTO dbo.t1 VALUES (41);  
BEGIN TRANSACTION;  
INSERT INTO dbo.t1 VALUES (42);  
PRINT N'[D.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO 
COMMIT TRANSACTION;  
PRINT N'[D.04] @@TranCount, after a COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
COMMIT TRANSACTION;  
PRINT N'[D.05] @@TranCount, after another COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO
  
-- Clean up.  
SET IMPLICIT_TRANSACTIONS OFF;  
GO  
WHILE (@@TranCount > 0) COMMIT TRANSACTION;  
GO  
DROP TABLE dbo.t1;  
GO
```  
  
 Di seguito è riportato l'output di testo dello script Transact-SQL precedente.  
  
```
-- Text output from Transact-SQL:  
  
-------- [Test A] ---- OFF ----  
[A.01] Now, SET IMPLICIT_TRANSACTIONS OFF.  
[A.02] @@TranCount, at start, == 0  
[A.03] @@TranCount, after INSERTs, == 0  
  
-------- [Test B] ---- ON ----  
[B.01] Now, SET IMPLICIT_TRANSACTIONS ON.  
[B.02] @@TranCount, at start, == 0  
[B.03] @@TranCount, after INSERTs, == 1  
[B.04] @@TranCount, after COMMIT, == 0  
  
-------- [Test C] ---- ON, then BEGIN TRAN ----  
[C.01] Now, SET IMPLICIT_TRANSACTIONS ON.  
[C.02] @@TranCount, at start, == 0  
[C.03] @@TranCount, after INSERTs, == 2  
[C.04] @@TranCount, after a COMMIT, == 1  
[C.05] @@TranCount, after another COMMIT, == 0  
  
-------- [Test D] ---- ON, INSERT, BEGIN TRAN, INSERT ----  
[D.01] Now, SET IMPLICIT_TRANSACTIONS ON.  
[D.02] @@TranCount, at start, == 0  
[D.03] @@TranCount, after INSERTs, == 2  
[D.04] @@TranCount, after INSERTs, == 1  
[D.05] @@TranCount, after INSERTs, == 0  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [MERGE &#40;Transact-sql&#41;](../../t-sql/statements/merge-transact-sql.md) [aprire &#40;transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
  
