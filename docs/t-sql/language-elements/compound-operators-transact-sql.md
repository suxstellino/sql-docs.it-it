---
description: Operatori composti (Transact-SQL)
title: Operatori composti (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators
- compound operators, described
ms.assetid: 5072fe91-02d3-42a7-831f-756eff714a17
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9fe8dedde26a421d58930c9df4861c9ce64d8973
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208208"
---
# <a name="compound-operators-transact-sql"></a>Operatori composti (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gli operatori composti consentono di eseguire alcune operazioni e impostano un valore originale sul risultato dell'operazione. Ad esempio, se una variabile @x è uguale a 35, all'espressione @x += 2 viene assegnato il valore originale di @x, quindi viene aggiunto il valore 2 e @x viene impostata sul nuovo valore (37).  
  
 In [!INCLUDE[tsql](../../includes/tsql-md.md)] sono disponibili i seguenti operatori composti:  
  
|Operatore|Collegamento a ulteriori informazioni|Azione|  
|--------------|------------------------------|------------|  
|+=|[+= &#40;Add Assignment&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/add-equals-transact-sql.md)|Aggiunge una quantità al valore originale e imposta il valore originale sul risultato.|  
|-=|[-= &#40;assegnazione di sottrazione&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/subtract-equals-transact-sql.md)|Sottrae una quantità dal valore originale e imposta il valore originale sul risultato.|  
|*=|[&#42;= &#40;assegnazione di moltiplicazione&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/multiply-equals-transact-sql.md)|Moltiplica una valore per una quantità e imposta il valore originale sul risultato.|  
|/=|[&#40;assegnazione di divisione&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/divide-equals-transact-sql.md)|Divide un valore per una quantità e imposta il valore originale sul risultato.|  
|%=|[Assegnazione di modulo &#40;Transact-SQL&#41;](../../t-sql/language-elements/modulo-equals-transact-sql.md)|Divide un valore per una quantità e imposta il valore originale sul modulo.|  
|&=|[&= &#40;assegnazione AND bit per bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)|Esegue un'operazione con AND bit per bit e imposta il valore originale sul risultato.|  
|^=|[^= &#40;Assegnazione OR esclusivo bit per bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)|Esegue un'operazione con OR esclusivo bit per bit e imposta il valore originale sul risultato.|  
|&#124;=|[&#124;= &#40;Assegnazione OR bit per bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)|Esegue un'operazione con OR bit per bit e imposta il valore originale sul risultato.|  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
expression operator expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida di uno dei tipi di dati della categoria dei tipi numerici.  
  
## <a name="result-types"></a>Tipi restituiti  
 Restituisce il tipo di dati dell'argomento con la priorità più alta. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Commenti  
 Per ulteriori informazioni, vedere gli argomenti correlati a ogni operatore.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti vengono illustrate le operazioni composte.  
  
```sql  
DECLARE @x1 INT = 27;  
SET @x1 += 2 ;  
SELECT @x1 AS Added_2;  
  
DECLARE @x2 INT = 27;  
SET @x2 -= 2 ;  
SELECT @x2 AS Subtracted_2;  
  
DECLARE @x3 INT = 27;  
SET @x3 *= 2 ;  
SELECT @x3 AS Multiplied_by_2;  
  
DECLARE @x4 INT = 27;  
SET @x4 /= 2 ;  
SELECT @x4 AS Divided_by_2;  
  
DECLARE @x5 INT = 27;  
SET @x5 %= 2 ;  
SELECT @x5 AS Modulo_of_27_divided_by_2;  
  
DECLARE @x6 INT = 9;  
SET @x6 &= 13 ;  
SELECT @x6 AS Bitwise_AND;  
  
DECLARE @x7 INT = 27;  
SET @x7 ^= 2 ;  
SELECT @x7 AS Bitwise_Exclusive_OR;  
  
DECLARE @x8 INT = 27;  
SET @x8 |= 2 ;  
SELECT @x8 AS Bitwise_OR;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Operatori bit per bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  
