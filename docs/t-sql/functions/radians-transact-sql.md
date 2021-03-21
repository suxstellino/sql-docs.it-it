---
description: RADIANS (Transact-SQL)
title: RADIANS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RADIANS
- RADIANS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RADIANS function
ms.assetid: e9f69951-ecda-45d9-8909-dcb716b1b1c0
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3f64ea615d78070e81745cd1705b7cbe11d1ccd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754951"
---
# <a name="radians-transact-sql"></a>RADIANS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce l'equivalente in radianti dell'espressione numerica specificata espressa in gradi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
RADIANS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *numeric_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) della categoria di tipi di dati numerici esatti o numerici approssimativi, ad eccezione del tipo di dati **bit**.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce lo stesso tipo di *numeric_expression*.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-radians-to-show-00"></a>R. Utilizzo di RADIANS con risultato 0.0  
 Nell'esempio seguente viene restituito il risultato `0.0` in quanto il valore dell'espressione numerica da convertire in radianti è troppo piccolo per la funzione `RADIANS`.  
  
```sql  
SELECT RADIANS(1e-307)  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------   
0.0                        
(1 row(s) affected)  
```  
  
### <a name="b-using-radians-to-return-the-equivalent-angle-of-a-float-expression"></a>B. Utilizzo di RADIANS per restituire l'angolo equivalente di un'espressione float  
 Nell'esempio seguente viene immessa un'espressione `float` e viene restituito il valore convertito da `RADIANS` per l'angolo specificato.  
  
```sql  
-- First value is -45.01.  
DECLARE @angle FLOAT  
SET @angle = -45.01  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is -181.01.  
DECLARE @angle FLOAT  
SET @angle = -181.01  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is 0.00.  
DECLARE @angle FLOAT  
SET @angle = 0.00  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is 0.1472738.  
DECLARE @angle FLOAT  
SET @angle = 0.1472738  
SELECT 'The RADIANS of the angle is: ' +  
    CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Last value is 197.1099392.  
DECLARE @angle FLOAT  
SET @angle = 197.1099392  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------------------------   
The RADIANS of the angle is: -0.785573                        
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: -3.15922                         
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 0                                
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 0.00257041                       
 (1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 3.44022                          
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [decimal e numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [float e real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)   
 [int, bigint, smallint e tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)   
 [Funzioni matematiche &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [money e smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
  
  

