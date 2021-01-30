---
description: IDENTITY (funzione) (Transact-SQL)
title: IDENTITY (funzione) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY function
- SELECT statement [SQL Server], IDENTITY function
- inserting identity columns
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY function
ms.assetid: ebec77eb-fc02-4feb-b6c5-f0098d43ccb6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bad6595806a3b50cc0634cecbae2c4db098799ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189930"
---
# <a name="identity-function-transact-sql"></a>IDENTITY (funzione) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene usata solo in istruzioni SELECT che includono una clausola INTO *table* per l'inserimento di una colonna Identity in una nuova tabella. Pur essendo simili, la funzione IDENTITY e la proprietà IDENTITY utilizzata con CREATE TABLE e ALTER TABLE non sono equivalenti.  
  
> [!NOTE]  
>  Per creare un numero a incremento automatico da usare in più tabelle o da chiamare dalle applicazioni senza fare riferimento ad alcuna tabella, vedere [Numeri di sequenza](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *data_type*  
 Tipo di dati della colonna Identity. I tipi di dati validi per una colonna Identity sono tutti i tipi di dati della categoria integer, con l'eccezione di **bit** e **decimal**.  
  
 *seed*  
 Valore intero da assegnare alla prima riga della tabella. A ogni riga successiva viene assegnato il valore Identity successivo, che corrisponde all'ultimo valore IDENTITY più il valore *increment*. Se vengono omessi sia *seed* che *increment*, verrà usato il valore predefinito 1 per entrambi.  
  
 *increment*  
 Valore intero da aggiungere al valore *seed* per le righe successive della tabella.  
  
 *column_name*  
 Nome della colonna da inserire nella nuova tabella.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce lo stesso tipo dell'argomento *data_type*.  
  
## <a name="remarks"></a>Commenti  
 Poiché questa funzione crea una colonna in una tabella, nell'elenco di selezione è necessario specificare un nome per la colonna in uno dei modi seguenti:  
  
```sql  
--(1)  
SELECT IDENTITY(int, 1,1) AS ID_Num  
INTO NewTable  
FROM OldTable;  
  
--(2)  
SELECT ID_Num = IDENTITY(int, 1, 1)  
INTO NewTable  
FROM OldTable;  
```  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono inserite tutte le righe della tabella `Contact` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] in una nuova tabella denominata `NewContact`. La funzione IDENTITY viene utilizzata per assegnare i numeri di identificazione nella tabella `NewContact` a partire da 100 anziché da 1.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY &#40;proprietà&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SELECT @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/select-local-variable-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  
