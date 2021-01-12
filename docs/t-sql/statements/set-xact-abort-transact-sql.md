---
description: SET XACT_ABORT (Transact-SQL)
title: SET XACT_ABORT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- XACT_ABORT_TSQL
- XACT_ABORT
- SET XACT_ABORT
- SET_XACT_ABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- XACT_ABORT option
- automatic transaction roll backs
- transactions [SQL Server], rolling back
- rolling back transactions, SET XACT_ABORT
- roll back transactions [SQL Server]
- SET XACT_ABORT statement
ms.assetid: cbcaa433-58f2-4dc3-a077-27273bef65b5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29737fa62f99028ebf35bca5d8e9c0e94fa1af21
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079991"
---
# <a name="set-xact_abort-transact-sql"></a>SET XACT_ABORT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> L'istruzione **THROW** rispetta **SET XACT_ABORT**, a differenza di **RAISERROR**. Le nuove applicazioni devono usare **THROW** invece di **RAISERROR**.

Specifica se in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito automaticamente il rollback della transazione corrente quando un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] genera un errore di run-time.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
SET XACT_ABORT { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni

Quando l'opzione SET XACT_ABORT è impostata su ON, se un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] genera un errore di run-time l'intera transazione viene terminata e viene eseguito il rollback.

Quando l'opzione SET XACT_ABORT è impostata su OFF, in alcuni casi viene eseguito il rollback solo dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che ha generato l'errore e l'elaborazione della transazione continua. In base alla gravità dell'errore, viene eseguito il rollback dell'intera transazione anche se l'opzione SET XACT_ABORT è impostata su OFF. OFF è l'impostazione predefinita in un'istruzione T-SQL, mentre ON è l'impostazione predefinita in un trigger.

L'opzione SET XACT_ABORT non ha alcun effetto sugli errori di compilazione, quali gli errori di sintassi.

L'opzione XACT_ABORT deve essere impostata su ON per le istruzioni di modifica dei dati, in una transazione implicita o esplicita, eseguite sulla maggior parte dei provider OLE DB, compreso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'unico caso in cui questa opzione non è necessaria è quando il provider supporta transazioni nidificate.

Quando ANSI_WARNINGS=OFF, le violazioni delle autorizzazioni causano l'interruzione delle transazioni.

L'opzione SET XACT_ABORT viene impostata in fase di esecuzione, non in fase di analisi.

Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito.

```sql
DECLARE @XACT_ABORT VARCHAR(3) = 'OFF';
IF ( (16384 & @@OPTIONS) = 16384 ) SET @XACT_ABORT = 'ON';
SELECT @XACT_ABORT AS XACT_ABORT;

```

## <a name="examples"></a>Esempi

In questo esempio di codice viene generato un errore di violazione della chiave esterna in una transazione che include altre istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'errore viene generato nel primo set di istruzioni, ma le altre istruzioni vengono eseguite completamente e viene eseguito il commit completo della transazione. Nel secondo set di istruzioni l'opzione `SET XACT_ABORT` viene impostata su `ON`. In seguito all'errore di istruzione il batch viene pertanto interrotto e viene eseguito il rollback della transazione.

```sql
IF OBJECT_ID(N't2', N'U') IS NOT NULL
    DROP TABLE t2;
GO
IF OBJECT_ID(N't1', N'U') IS NOT NULL
    DROP TABLE t1;
GO  
CREATE TABLE t1
    (a INT NOT NULL PRIMARY KEY);
CREATE TABLE t2
    (a INT NOT NULL REFERENCES t1(a));
GO
INSERT INTO t1 VALUES (1);
INSERT INTO t1 VALUES (3);
INSERT INTO t1 VALUES (4);
INSERT INTO t1 VALUES (6);
GO
SET XACT_ABORT OFF;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (1);
INSERT INTO t2 VALUES (2); -- Foreign key error.
INSERT INTO t2 VALUES (3);
COMMIT TRANSACTION;
GO
SET XACT_ABORT ON;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (4);
INSERT INTO t2 VALUES (5); -- Foreign key error.
INSERT INTO t2 VALUES (6);
COMMIT TRANSACTION;
GO
-- SELECT shows only keys 1 and 3 added.
-- Key 2 insert failed and was rolled back, but
-- XACT_ABORT was OFF and rest of transaction
-- succeeded.
-- Key 5 insert error with XACT_ABORT ON caused
-- all of the second transaction to roll back.
SELECT *
 FROM t2;
GO
```

## <a name="see-also"></a>Vedere anche

- [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)
- [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)
- [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)
- [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
- [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)
- [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
