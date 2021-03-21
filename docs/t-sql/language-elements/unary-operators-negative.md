---
description: '- (Negativo) (Transact-SQL)'
title: '- (negativo) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- negative
dev_langs:
- TSQL
helpviewer_keywords:
- '- (negative)'
- negative operator (-)
- negative values
ms.assetid: d6c14d14-d379-403b-82db-c197ad58c896
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0dcd07b4ae448c57eb3a5cace38110095771efbc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750481"
---
# <a name="unary-operators---negative"></a>Operatori unari - Negativo
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il valore negativo del valore di un'espressione numerica (operatore unario). Gli operatori unari eseguono un'operazione in una sola espressione di un tipo di dati della categoria numerici.   
  
|Operatore|Significato|  
|--------------|-------------|  
|[+ (positivo)](../../t-sql/language-elements/unary-operators-positive.md)|Valore numerico positivo.|  
|[- (negativo)](../../t-sql/language-elements/unary-operators-negative.md)|Valore numerico negativo.|  
|[~ (NOT bit per bit)](../../t-sql/language-elements/bitwise-not-transact-sql.md)|Restituisce il complemento a uno del numero.|  
  
 Gli operatori + (positivo) e - (negativo) possono essere utilizzati in qualsiasi espressione di un tipo di dati della categoria numerici. L'operatore ~ (NOT bit per bit) può essere utilizzato solo in espressioni di un tipo di dati della categoria integer. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
- numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *numeric_expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida con uno qualsiasi dei tipi di dati appartenenti alla categoria dei tipi di dati numerici, con l'eccezione della categoria di data e ora.  
  
## <a name="result-types"></a>Tipi restituiti  
 Restituisce il tipo di dati di *numeric_expression*, con la sola eccezione che un'espressione di tipo **tinyint** senza segno viene promossa e convertita in un risultato **smallint** con segno.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-setting-a-variable-to-a-negative-value"></a>R. Impostazione di una variabile su un valore negativo  
 Nell'esempio seguente viene impostata una variabile su un valore negativo.  
  
```sql 
USE tempdb;  
GO  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = -123.45;  
SELECT @MyNumber AS NegativeValue;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
NegativeValue  
---------------------------------------  
-123.45  
  
(1 row(s) affected)  
  
```  
  
### <a name="b-changing-a-variable-to-a-negative-value"></a>B. Modifica di una variabile su un valore negativo  
 Nell'esempio seguente viene modificata una variabile su un valore negativo.  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = 5;  
SELECT @Num1 AS VariableValue, -@Num1 AS NegativeValue;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
VariableValue NegativeValue  
------------- -------------  
5             -5  
  
(1 row(s) affected)  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-the-negative-of-a-positive-constant"></a>C. Restituzione del valore negativo di una costante positiva  
 Nell'esempio seguente viene restituito il valore negativo di una costante positiva.  
  
```sql  
USE ssawPDW;  
  
SELECT TOP (1) - 17 FROM DimEmployee;  
```  
  
 Restituisce  
  
```  
-17  
```  
  
### <a name="d-returning-the-positive-of-a-negative-constant"></a>D. Restituzione del valore positivo di una costante negativa  
 Nell'esempio seguente viene restituito il valore positivo di una costante negativa.  
  
```sql  
USE ssawPDW;  
  
SELECT TOP (1) - ( - 17) FROM DimEmployee;  
```  
  
 Restituisce  
  
```  
17  
```  
  
### <a name="e-returning-the-negative-of-a-column"></a>E. Restituzione del valore negativo di una colonna  
 Nell'esempio seguente viene restituito il valore negativo del valore `BaseRate` per ogni dipendente nella tabella `dimEmployee`.  
  
```sql  
USE ssawPDW;  
  
SELECT - BaseRate FROM DimEmployee;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  

