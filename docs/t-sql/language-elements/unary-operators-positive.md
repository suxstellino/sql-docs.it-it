---
description: + (Operatore più unario) (Transact-SQL)
title: + (Operatore più unario) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- + (positive)
- +
- positive
dev_langs:
- TSQL
helpviewer_keywords:
- + (positive operator)
- positive operator (+)
- positive values [SQL Server]
ms.assetid: 0f31c5cc-3078-4f6a-9870-7eb1a98053fb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3175367df6e6277e22711a3119688c952f23802
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754521"
---
# <a name="unary-operators---positive"></a>Operatori unari - positivo
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Restituisce il valore di un'espressione numerica (operatore unario). Gli operatori unari eseguono un'operazione in una sola espressione di un tipo di dati della categoria numerici.   
  
|Operatore|Significato|  
|--------------|-------------|  
|[+ (positivo)](../../t-sql/language-elements/unary-operators-positive.md)|Valore numerico positivo.|  
|[- (negativo)](../../t-sql/language-elements/unary-operators-negative.md)|Valore numerico negativo.|  
|[~ (NOT bit per bit)](../../t-sql/language-elements/bitwise-not-transact-sql.md)|Restituisce il complemento a uno del numero.|  
  
 Gli operatori + (positivo) e - (negativo) possono essere utilizzati in qualsiasi espressione di un tipo di dati della categoria numerici. L'operatore ~ (NOT bit per bit) può essere utilizzato solo in espressioni di un tipo di dati della categoria integer.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
+ numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *numeric_expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida di un qualsiasi tipo di dati della categoria dei tipi di dati numerici, ad eccezione dei tipi di dati **datetime** e **smalldatetime**.  
  
## <a name="result-types"></a>Tipi restituiti  
 Restituisce il tipo di dati di *numeric_expression*.  
  
## <a name="remarks"></a>Osservazioni  
 Sebbene sia possibile aggiungere un operatore più unario prima di qualsiasi espressione numerica, in questo caso non viene eseguita alcuna operazione sul valore restituito dall'espressione. In particolare, non verrà restituito il valore positivo di un'espressione negativa. Per restituire il valore positivo di un'espressione negativa, usare la funzione [ABS](../../t-sql/functions/abs-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-setting-a-variable-to-a-positive-value"></a>R. Impostazione di una variabile su un valore positivo  
 Nell'esempio seguente una variabile viene impostata su un valore positivo.  
  
```sql  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = +123.45;  
SELECT @MyNumber;  
GO  
```  
  
 Set di risultati:  
  
```  
-----------   
123.45            
  
(1 row(s) affected)  
```  
  
### <a name="b-using-the-unary-plus-operator-with-a-negative-value"></a>B. Utilizzo dell'operatore più unario con un valore negativo  
 Nell'esempio seguente viene illustrato l'utilizzo dell'operatore più unario con un'espressione negativa e della funzione ABS() sulla stessa espressione negativa. L'operatore più unario non influisce sull'espressione, ma la funzione ABS restituisce il valore positivo dell'espressione.  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = -5;  
SELECT +@Num1, ABS(@Num1);  
GO  
```  
  
 Set di risultati:  
  
```  
----------- -----------  
-5          5  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [ABS &#40;Transact-SQL&#41;](../../t-sql/functions/abs-transact-sql.md)  
  
  
