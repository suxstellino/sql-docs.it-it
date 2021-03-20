---
description: BEGIN...END (Transact-SQL)
title: BEGIN...END (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN
- BEGIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- enclosing statements [SQL Server]
- BEGIN statement
- control-of-flow language [SQL Server], BEGIN...END statement
- BEGIN...END keyword
- grouping statements, BEGIN...END statement
- executing Transact-SQL statements together [SQL Server]
- statements [SQL Server], grouping
ms.assetid: fc2c7f76-f1f9-4f91-beef-bc8ef0da2feb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a31ab1dc7bbfb6806a55b5d929d76e601dc923f2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104733505"
---
# <a name="beginend-transact-sql"></a>BEGIN...END (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Include una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per consentire l'esecuzione di un gruppo di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. BEGIN ed END sono parole chiave del linguaggio per il controllo di flusso.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
BEGIN  
    { sql_statement | statement_block }   
END  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 { *sql_statement* | *statement_block* }  
 Qualsiasi istruzione o raggruppamento di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] valide definito mediante l'utilizzo di un blocco di istruzioni.  
  
## <a name="remarks"></a>Commenti  
 I blocchi BEGIN...END possono essere nidificati.  
  
 Tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono valide all'interno di un blocco BEGIN...END. Alcune istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], tuttavia, non devono essere raggruppate nello stesso batch o blocco di istruzioni.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente `BEGIN` ed `END` definiscono una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite insieme. Se il blocco `BEGIN...END` non fosse incluso, verrebbero eseguite entrambe le istruzioni `ROLLBACK TRANSACTION` e verrebbero restituiti entrambi i messaggi `PRINT`.  
  
```sql
USE AdventureWorks2012
GO  
BEGIN TRANSACTION
GO  
IF @@TRANCOUNT = 0  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM Person.Person WHERE LastName = 'Adams'
    ROLLBACK TRANSACTION
    PRINT N'Rolling back the transaction two times would cause an error.'
END
ROLLBACK TRANSACTION
PRINT N'Rolled back the transaction.'
GO  
/*  
Rolled back the transaction.  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente `BEGIN` ed `END` definiscono una serie di istruzioni [!INCLUDE[DWsql](../../includes/dwsql-md.md)] eseguite insieme. Se il blocco `BEGIN...END` non è incluso, l'esempio seguente determinerà un ciclo continuo.  
  
```sql
-- Uses AdventureWorks  

DECLARE @Iteration Integer = 0  
WHILE @Iteration <10  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM dbo.DimCustomer WHERE LastName = 'Adams'
    SET @Iteration += 1  
END
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Elementi del linguaggio per il controllo di flusso &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [END &#40;BEGIN...END&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/end-begin-end-transact-sql.md)
