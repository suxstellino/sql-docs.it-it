---
description: LEN (Transact-SQL)
title: LEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/03/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEN
- LEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- LEN function
- characters [SQL Server], number of
- number of characters
ms.assetid: fa20fee4-884d-4301-891a-c03e901345ae
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4586ea73d3e64d60ac3f013e46fa43e75c30802
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204685"
---
# <a name="len-transact-sql"></a>LEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce il numero di caratteri dell'espressione stringa specificata, esclusi gli spazi finali.  
  
> [!NOTE]  
> Per restituire il numero di byte usati per rappresentare un'espressione, usare la funzione [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
LEN ( string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *string_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) stringa da valutare. *string_expression* può essere una costante, una variabile o una colonna di dati di tipo carattere o binario.  
  
## <a name="return-types"></a>Tipi restituiti  
 **bigint** se *expression* è del tipo di dati **varchar(max)**, **nvarchar(max)** o **varbinary(max)**. In caso contrario, **int**.  
  
 Se si utilizzano le regole di confronto SC, il valore intero restituito considererà le coppie di surrogati UTF-16 come un singolo carattere. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="remarks"></a>Osservazioni  
LEN esclude gli spazi finali. Se questo è un problema, provare a usare la funzione [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md) che non taglia la stringa. Se si elabora una stringa Unicode, DATALENGTH restituirà un numero che potrebbe non corrispondere al numero di caratteri. Nell'esempio seguente viene illustrato l'uso di LEN e DATALENGTH con uno spazio finale.  
  
```sql  
  DECLARE @v1 VARCHAR(40),  
    @v2 NVARCHAR(40);  
SELECT   
@v1 = 'Test of 22 characters ',   
@v2 = 'Test of 22 characters ';  
SELECT LEN(@v1) AS [VARCHAR LEN] , DATALENGTH(@v1) AS [VARCHAR DATALENGTH];  
SELECT LEN(@v2) AS [NVARCHAR LEN], DATALENGTH(@v2) AS [NVARCHAR DATALENGTH];  
```  

> [!NOTE]
> Usare [LEN](../../t-sql/functions/len-transact-sql.md) per restituire il numero di caratteri codificati in una determinata espressione stringa e [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) per restituire la dimensione in byte per un'espressione stringa specificata. Questi output possono variare a seconda del tipo di dati e del tipo di codifica usati nella colonna. Per altre informazioni sulle differenze di archiviazione tra tipi di codifica diversi, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).

## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono selezionati il numero di caratteri e i dati in `FirstName` per le persone residenti in `Australia`. In questo esempio viene utilizzato il database AdventureWorks.  
  
```sql  
SELECT LEN(FirstName) AS Length, FirstName, LastName   
FROM Sales.vIndividualCustomer  
WHERE CountryRegionName = 'Australia';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'esempio seguente restituisce il numero di caratteri nella colonna `FirstName` e i nomi e cognomi dei dipendenti in `Australia`.  
  
```sql  
USE AdventureWorks2016  
GO  
SELECT DISTINCT LEN(FirstName) AS FNameLength, FirstName, LastName   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.DimGeography AS g   
    ON e.SalesTerritoryKey = g.SalesTerritoryKey   
WHERE EnglishCountryRegionName = 'Australia';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
FNameLength  FirstName  LastName  
-----------  ---------  ---------------  
4            Lynn       Tsoflias
```  
  
## <a name="see-also"></a>Vedere anche  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [CHARINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/charindex-transact-sql.md)  
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)   
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
  
  
