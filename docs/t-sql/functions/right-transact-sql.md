---
description: RIGHT (Transact-SQL)
title: RIGHT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RIGHT_TSQL
- RIGHT
dev_langs:
- TSQL
helpviewer_keywords:
- rightmost character of expression
- RIGHT function
- character strings [SQL Server], RIGHT
ms.assetid: 43f1fe1f-aa18-47e3-ba20-e03e32254a6d
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7929ad28533f0b20483eab9da0d99813c3b92364
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747931"
---
# <a name="right-transact-sql"></a>RIGHT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce la parte finale di una stringa di caratteri, di lunghezza pari al numero di caratteri specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
RIGHT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *character_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di dati di tipo carattere o binario. *character_expression* può essere una costante, una variabile o una colonna. *character_expression* può essere di qualsiasi tipo di dati, eccetto **text** o **ntext**, implicitamente convertibile in **varchar** o **nvarchar**. In alternativa usare la funzione [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) per convertire in modo esplicito *character_expression*.  
   
> [!NOTE]  
> Se *string_expression* è di tipo **binary** o **varbinary**, RIGHT eseguirà una conversione implicita in **varchar** e pertanto non manterrà l'input binario.  
  
 *integer_expression*  
 Valore Integer positivo che specifica quanti caratteri di *character_expression* verranno restituiti. Se l'argomento *integer_expression* è negativo, viene restituito un errore. Se *integer_expression* è di tipo **bigint** e contiene un valore elevato, *character_expression* deve essere di un tipo di dati di grandi dimensioni, ad esempio **varchar(max)** .  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce **varchar** quando *character_expression* è un tipo di dati carattere non Unicode.  
  
 Restituisce **nvarchar** quando *character_expression* è un tipo di dati carattere Unicode.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
 Quando si utilizzano le regole di confronto SC, la funzione RIGHT considera una coppia di surrogati UTF-16 come un singolo carattere. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-right-with-a-column"></a>A: Uso di RIGHT con una colonna  
 Nell'esempio seguente vengono restituiti gli ultimi cinque caratteri del nome di ciascuna persona nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT RIGHT(FirstName, 5) AS 'First Name'  
FROM Person.Person  
WHERE BusinessEntityID < 5  
ORDER BY FirstName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
First Name  
----------  
Ken  
Terri  
berto  
Rob  
  
(4 row(s) affected)  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-using-right-with-a-column"></a>B. Uso di RIGHT con una colonna  
 Nell'esempio seguente vengono restituiti gli ultimi cinque caratteri di ogni cognome nella tabella `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT RIGHT(LastName, 5) AS Name  
FROM dbo.DimEmployee  
ORDER BY EmployeeKey;  
```  
  
 Set di risultati parziale:  
  
 ```
Name
-----
lbert
Brown
rello
lters
 ```  
  
### <a name="c-using-right-with-a-character-string"></a>C. Uso di LEFT con una stringa di caratteri  
 Nell'esempio seguente viene usata la funzione `RIGHT` per restituire gli ultimi due caratteri della stringa di caratteri `abcdefg`.  
  
```sql  
SELECT RIGHT('abcdefg', 2); 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
-------  
fg
```  
  
## <a name="see-also"></a>Vedere anche  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

