---
description: '&gt;= (maggiore o uguale a) (Transact-SQL)'
title: '&gt;= (maggiore o uguale a) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Greater
- '>='
- '>= (Greater Than or Equal To)'
- Equal To
- '>=_TSQL'
- Greater Than
- Equal
dev_langs:
- TSQL
helpviewer_keywords:
- greater than or equal to operator (>=)
- '>= (greater than or equal to operator)'
ms.assetid: 641ee28d-7536-46dd-a48a-6c63c2d59278
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 697c68adcc4f7b8446aae149a4c0d17ace8ac28e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99114361"
---
# <a name="gt-greater-than-or-equal-to-transact-sql"></a>&gt;= (maggiore o uguale a) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Confronta due espressioni e verifica la presenza di valori maggiori o uguali (operatore di confronto).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
expression >= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida. È necessario che il tipo di dati di entrambe le espressioni possa essere convertito in modo implicito. La conversione dipende dalle regole di [precedenza dei tipi di dati](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="result-types"></a>Tipi restituiti  
 Boolean  
  
## <a name="remarks"></a>Commenti  
 Quando si confrontano due espressioni diverse da Null, il risultato è TRUE se il valore dell'operando di sinistra è maggiore o uguale al valore di quello di destra. In caso contrario il risultato è FALSE.  
  
 A differenza dell'operatore di confronto = (uguaglianza), il risultato del confronto >= di due valori NULL non dipende dall'impostazione ANSI_NULLS.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using--in-a-simple-query"></a>R. Uso di >= in una query semplice  
 Nell'esempio seguente vengono restituite tutte le righe nella tabella `HumanResources.Department` che dispongono di un valore in `DepartmentID` maggiore o uguale a 13.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name  
FROM HumanResources.Department  
WHERE DepartmentID >= 13  
ORDER BY DepartmentID;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
DepartmentID Name  
------------ --------------------------------------------------  
13           Quality Assurance  
14           Facilities and Maintenance  
15           Shipping and Receiving  
16           Executive  
  
(4 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [= &#40;Equals&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/equals-transact-sql.md)   
 [&#62; &#40;Greater Than&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
