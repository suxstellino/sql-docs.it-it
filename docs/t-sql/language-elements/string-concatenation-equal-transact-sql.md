---
title: += Concatenazione di stringhe
description: Concatenare due stringhe e impostare la stringa sul risultato dell'operazione.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- concatenate strings
- string concatenation
- += (concatenate operator)
ms.assetid: 4aaeaab7-9b2b-48e0-8487-04ed672ebcb1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e2b0bf5c2ae5c1514a54207c0ece773b3e6d4ed2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752951"
---
# <a name="-string-concatenation-assignment-transact-sql"></a>+= (Assegnazione concatenazione di stringhe) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concatena due stringhe e imposta la stringa sul risultato dell'operazione. Se ad esempio una variabile @x è uguale a 'Adventure', @x += 'Works' comporta l'aggiunta di 'Works' al valore originale @x e l'impostazione di @x sul nuovo valore 'AdventureWorks'.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
expression += expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida di dati di tipo carattere.  
  
## <a name="result-types"></a>Tipi restituiti  
 Restituisce il tipo di dati definito per la variabile.  
  
## <a name="remarks"></a>Commenti  
 SET @v1 += 'expression' equivale a SET @v1 = @v1 + ('expression'). Inoltre SET @v1 = @v2 + @v3 + @v4 equivale a SET @v1 = (@v2 + @v3) + @v4.  
  
 L'operatore += non può essere utilizzato senza una variabile. Il codice riportato di seguito comporta, ad esempio, la generazione di un errore:  
  
```sql  
SELECT 'Adventure' += 'Works'  
```  
  
## <a name="examples"></a>Esempi  
### <a name="a-concatenation-using--operator"></a>R. Concatenazione usando l'operatore +=
 Nell'esempio seguente viene eseguita una concatenazione utilizzando l'operatore `+=`.  
  
```sql  
DECLARE @v1 VARCHAR(40);  
SET @v1 = 'This is the original.';  
SET @v1 += ' More text.';  
PRINT @v1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `This is the original. More text.`  
  
### <a name="b-order-of-evaluation-while-concatenating-using--operator"></a>B. Ordine di valutazione durante la concatenazione usando l'operatore +=
Nell'esempio seguente si concatenano più stringhe per formare una stringa lunga e quindi si prova a calcolare la lunghezza della stringa finale. Questo esempio illustrato l'ordine di valutazione e le regole di troncamento quando si usa l'operatore di concatenazione. 

```sql
DECLARE @x VARCHAR(4000) = REPLICATE('x', 4000)
DECLARE @z VARCHAR(8000) = REPLICATE('z',8000)
DECLARE @y VARCHAR(max);
 
SET @y = '';
SET @y += @x + @z;
SELECT LEN(@y) AS Y; -- 8000
 
SET @y = '';
SET @y = @y + @x + @z;
SELECT LEN(@y) AS Y; -- 12000
 
SET @y = '';
SET @y = @y +(@x + @z);
SELECT LEN(@y) AS Y; -- 8000
-- or
SET @y = '';
SET @y = @x + @z + @y;
SELECT LEN(@y) AS Y; -- 8000
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Y       
 ------- 
 8000 
  
 (1 row(s) affected) 
  
    
 Y       
 ------- 
 12000 
  
 (1 row(s) affected) 

 Y       
 ------- 
 8000 
  
 (1 row(s) affected) 
  
 Y       
 ------- 
 8000 
  
 (1 row(s) affected)
  ```   
   
## <a name="see-also"></a>Vedere anche  
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [+ = &#40;Aggiungi assegnazione&#41; &#40;&#41;Transact-SQL ](../../t-sql/language-elements/add-equals-transact-sql.md)   
 [+ &#40;String Concatenation&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
  
  
