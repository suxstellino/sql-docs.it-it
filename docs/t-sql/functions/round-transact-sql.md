---
description: ROUND (Transact-SQL)
title: ROUND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROUND_TSQL
- ROUND
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- ROUND function [Transact-SQL]
ms.assetid: 23921ed6-dd6a-4c9e-8c32-91c0d44fe4b7
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: efcf7dbbfebaf00bcbba94b0b28cae0cdfcc700a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734556"
---
# <a name="round-transact-sql"></a>ROUND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce un valore numerico arrotondato alla lunghezza o alla precisione specificata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ROUND ( numeric_expression , length [ ,function ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *numeric_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) della categoria di tipi di dati numerici esatti o numerici approssimativi, ad eccezione del tipo di dati **bit**.  
  
 *length*  
 Precisione per l'arrotondamento di *numeric_expression* . *length* deve essere un'espressione di tipo **tinyint**, **smallint** o **int**. Quando *length* è un numero positivo, *numeric_expression* viene arrotondato al numero di posizioni decimali specificato da *length*. Quando *length* è un numero negativo, *numeric_expression* viene arrotondato a sinistra del separatore decimale come specificato da *length*.  
  
 *function*  
 Tipo di operazione da eseguire. *function* deve essere **tinyint**, **smallint** o **int**. Quando *function* viene omesso oppure è un valore pari a 0 (impostazione predefinita), *numeric_expression* viene arrotondato. Se è specificato un valore diverso da 0, l'argomento *numeric_expression* viene troncato.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce i tipi di dati seguenti:  
  
|Risultato dell'espressione|Tipo restituito|  
|-----------------------|-----------------|  
|**tinyint**|**int**|  
|**smallint**|**int**|  
|**int**|**int**|  
|**bigint**|**bigint**|  
|Categoria **decimal** e **numeric** (p, s)|**decimal(p, s)**|  
|Categoria **money** e **smallmoney**|**money**|  
|Categoria **float** e **real**|**float**|  
  
## <a name="remarks"></a>Commenti  
 La funzione ROUND restituisce sempre un valore. Se l'argomento *length* è negativo e maggiore del numero di cifre che precedono il separatore decimale, la funzione ROUND restituisce 0.  
  
|Esempio|Risultato|  
|-------------|------------|  
|ROUND(748.58, -4)|0|  
  
 La funzione ROUND restituisce un valore *numeric_expression* arrotondato indipendentemente dal tipo di dati quando *length* è un numero negativo.  
  
|Esempi|Risultato|  
|--------------|------------|  
|ROUND(748.58, -1)|750.00|  
|ROUND(748.58, -2)|700.00|  
|ROUND(748.58, -3)|Genera un overflow aritmetico, perché 748.58 assume il valore decimal(5,2), tramite il quale non può essere restituito 1000.00.|  
|Per un arrotondamento fino a 4 cifre, modificare il tipo di dati di input. Ad esempio:<br /><br /> `SELECT ROUND(CAST (748.58 AS decimal (6,2)),-3);`|1000.00|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-round-and-estimates"></a>R. Utilizzo della funzione ROUND e delle stime  
 In questo esempio vengono utilizzate due espressioni per illustrare che con la funzione `ROUND` l'ultima cifra è sempre una stima.  
  
```sql  
SELECT ROUND(123.9994, 3), ROUND(123.9995, 3);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -----------  
123.9990    124.0000      
```  
  
### <a name="b-using-round-and-rounding-approximations"></a>B. Utilizzo della funzione ROUND e delle approssimazioni di arrotondamento  
 Nell'esempio seguente vengono illustrati l'arrotondamento e l'approssimazione.  
  
```  
SELECT ROUND(123.4545, 2), ROUND(123.45, -2);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

 ```
----------  ----------
123.4500    100.00
```
  
### <a name="c-using-round-to-truncate"></a>C. Utilizzo della funzione ROUND per ottenere un troncamento  
 Nell'esempio seguente vengono utilizzate due istruzioni `SELECT` per illustrare la differenza tra l'arrotondamento e il troncamento. La prima istruzione arrotonda il risultato, la seconda lo tronca.  
  
```sql  
SELECT ROUND(150.75, 0);  
GO  
SELECT ROUND(150.75, 0, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
151.00  
  
(1 row(s) affected)  
  
--------  
150.00  
  
(1 row(s) affected)  
```
  
## <a name="see-also"></a>Vedere anche  
 [CEILING &#40;Transact-SQL&#41;](../../t-sql/functions/ceiling-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FLOOR &#40;Transact-SQL&#41;](../../t-sql/functions/floor-transact-sql.md)   
 [Funzioni matematiche &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)
