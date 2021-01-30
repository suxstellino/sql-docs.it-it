---
description: DATETIME2FROMPARTS (Transact-SQL)
title: DATETIME2FROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATETIME2FROMPARTS_TSQL
- DATETIME2FROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIME2FROMPARTS function
ms.assetid: 632b757d-d2d1-43a5-b870-792a779ae204
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 942d3942d467e0c9ee03ca917fc472dce64d06d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181820"
---
# <a name="datetime2fromparts-transact-sql"></a>DATETIME2FROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce un valore di tipo **datetime2** per gli argomenti date e time. Il valore restituito ha una precisione specificata dall'argomento precision.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*year*  
Espressione Integer che specifica un anno.
  
*month*  
Espressione Integer che specifica un mese.
  
*day*  
Espressione Integer che specifica un giorno.
  
*hour*  
Espressione Integer che specifica le ore.
  
*minute*  
Espressione Integer che specifica i minuti.
  
*secondi*  
Espressione Integer che specifica i secondi.
  
*fractions*  
Espressione Integer che specifica un valore per i secondi frazionari.
  
*precisione*  
Espressione Integer che specifica la precisione del valore **datetime2** che verrà restituito da `DATETIME2FROMPARTS`.
  
## <a name="return-types"></a>Tipi restituiti
**datetime2(** *precision* **)**
  
## <a name="remarks"></a>Commenti  
`DATETIME2FROMPARTS` restituisce un valore di tipo **datetime2** completamente inizializzato. `DATETIME2FROMPARTS` genererà un errore se almeno un argomento obbligatorio ha un valore non valido. `DATETIME2FROMPARTS` restituisce null se almeno un argomento obbligatorio ha un valore null. Se tuttavia l'argomento *precision* ha un valore null, `DATETIME2FROMPARTS` genererà un errore.

L'argomento *fractions* dipende dall'argomento *precision*. Ad esempio, per un valore di *precision* pari a 7, ogni frazione rappresenta 100 nanosecondi, mentre per *precision* pari a 3, ogni frazione rappresenta un millisecondo. Per un valore di *precision* pari a zero, anche il valore di *fractions* deve essere zero. In caso contrario, `DATETIME2FROMPARTS` genererà un errore.
  
Questa funzione supporta la comunicazione remota con i server [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] e versioni successive. Non supporterà la comunicazione remota con i server con versioni precedenti a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Esempi  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>R. Esempio senza frazioni di un secondo  
  
```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Esempio con frazioni di un secondo  
Questo esempio illustra l'uso dei parametri *fractions* e *precision*:
  
1.  Se *fractions* ha valore 5 e *precision* ha valore 1, il valore di *fractions* corrisponde a 5/10 di secondo.  
  
2.  Se *fractions* ha valore 50 e *precision* ha valore 2, il valore di *fractions* corrisponde a 50/100 di secondo.  
  
3.  Se *fractions* ha valore 500 e *precision* ha valore 3, il valore di *fractions* corrisponde a 500/1000 di secondo.  
  
```sql
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 5, 1 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 50, 2 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 500, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------  
2011-08-15 14:23:44.5  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.50  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.500  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedi anche
[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)
  
  

