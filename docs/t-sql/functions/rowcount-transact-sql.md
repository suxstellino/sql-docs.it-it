---
description: '&#x40;&#x40;ROWCOUNT (Transact-SQL)'
title: '@@ROWCOUNT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@ROWCOUNT_TSQL'
- '@@ROWCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@ROWCOUNT function'
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 97a47998-81d9-4331-a244-9eb8b6fe4a56
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6eca1ac99c93c8faa062b46b09fe3715663a5c87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159718"
---
# <a name="x40x40rowcount-transact-sql"></a>&#x40;&#x40;ROWCOUNT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il numero di righe modificate dall'ultima istruzione. Se il numero di righe è superiore a 2 miliardi, usare [ROWCOUNT_BIG](../../t-sql/functions/rowcount-big-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
@@ROWCOUNT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] possono impostare il valore di @@ROWCOUNT come descritto di seguito:  
  
-   Impostare @@ROWCOUNT sul numero di righe modificate o lette. Le righe possono venire inviate al client o meno.  
  
-   Mantenere il valore di @@ROWCOUNT della precedente istruzione eseguita.  
  
-   Reimpostare @@ROWCOUNT su 0 senza restituire il valore al client.  
  
 Le istruzioni che effettuano un'assegnazione semplice impostano sempre il valore di @@ROWCOUNT su 1. Non vengono inviate righe al client. Esempi di istruzioni di questo tipo sono: set @*local_variable*, return, READTEXT e SELECT senza istruzioni di query, ad esempio SELECT GETDATE () o Select **'**_Generic Text_*_'_*.  
  
 Le istruzioni che effettuano un'assegnazione in una query o usano RETURN in una query impostano il valore di @@ROWCOUNT sul numero di righe modificate o lette dalla query, ad esempio: SELECT @*local_variable* = c1 FROM t1.  
  
 Le istruzioni DML (Data Manipulation Language) impostano il valore di @@ROWCOUNT sul numero di righe modificate dalla query e restituiscono tale valore al client. Le istruzioni DML possono non inviare righe al client.  
  
 DECLARE CURSOR e FETCH impostano il valore di @@ROWCOUNT su 1.  
  
 Le istruzioni EXECUTE mantengono il precedente valore di @@ROWCOUNT.  
  
 Le istruzioni di tipo USE, SET \<option>, DEALLOCATE CURSOR, CLOSE CURSOR, PRINT, RAISERROR, BEGIN TRANSACTION, e COMMIT TRANSACTION reimpostano il valore di ROWCOUNT su 0.  
  
 Nelle stored procedure compilate in modo nativo viene mantenuto il valore @@ROWCOUNT precedente. Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle stored procedure compilate in modo nativo non prevedono l'impostazione di @@ROWCOUNT. Per altre informazioni, vedere [Natively Compiled Stored Procedures](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md) (Stored procedure compilate in modo nativo).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene eseguita un'istruzione `UPDATE` e viene utilizzato `@@ROWCOUNT` per rilevare se sono state modificate righe.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee   
SET JobTitle = N'Executive'  
WHERE NationalIDNumber = 123456789  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were updated';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)  
  
