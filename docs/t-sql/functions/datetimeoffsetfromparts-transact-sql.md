---
description: DATETIMEOFFSETFROMPARTS (Transact-SQL)
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab37520d60c482afccb40bde7392de2515a79753
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101193"
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce un valore di tipo **datetimeoffset** per gli argomenti date e time. Il valore restituito ha una precisione specificata dall'argomento precision e una differenza specificata dagli argomenti di offset.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
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
  
*hour_offset*  
Espressione Integer che specifica la parte di ora della differenza di fuso orario.  
  
*minute_offset*  
Espressione Integer che specifica la parte di minuto della differenza di fuso orario.  
  
*precisione*  
Valore letterale Integer che specifica la precisione del valore **datetimeoffset** che verrà restituito da `DATETIMEOFFSETFROMPARTS`.  
  
## <a name="return-types"></a>Tipi restituiti
**datetimeoffset(** *precision* **)**  
  
## <a name="remarks"></a>Commenti  

`DATETIMEOFFSETFROMPARTS` restituisce un tipo di dati **datetimeoffset** completamente inizializzato. Gli argomenti di offset rappresentano la differenza di fuso orario. Per gli argomenti di offset omessi, `DATETIMEOFFSETFROMPARTS` presuppone una differenza di fuso orario pari a `00:00`, ovvero nessuna differenza di fuso orario. Per gli argomenti di offset specificati, `DATETIMEOFFSETFROMPARTS` prevede per entrambi gli argomenti valori entrambi positivi o entrambi negativi. Se *minute_offset* ha un valore e *hour_offset* non ha valori, `DATETIMEOFFSETFROMPARTS` genererà un errore. `DATETIMEOFFSETFROMPARTS` genererà un errore se gli altri argomenti hanno valori non validi. Se almeno un argomento obbligatorio ha un valore `NULL`, `DATETIMEOFFSETFROMPARTS` restituirà `NULL`. Se tuttavia l'argomento *precision* ha un valore `NULL`, `DATETIMEOFFSETFROMPARTS` genererà un errore.  
  
L'argomento *fractions* dipende dall'argomento precision. Ad esempio, per un valore di precision pari a 7, ogni frazione rappresenta 100 nanosecondi, mentre per precision pari a 3, ogni frazione rappresenta un millisecondo. Per un valore di precision pari a zero, anche il valore di fractions deve essere zero. In caso contrario, `DATETIMEOFFSETFROMPARTS` genererà un errore.  
  
Questa funzione supporta la comunicazione remota con i server [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] e versioni successive. Non supporterà la comunicazione remota con i server con versioni precedenti a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Esempi  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>R. Esempio senza frazioni di un secondo  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------
2010-12-31 14:23:23.0000000 +12:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Esempio con frazioni di un secondo  

Questo esempio illustra l'uso dei parametri *fractions* e *precision*:  

1. Se *fractions* ha valore 5 e *precision* ha valore 1, il valore di *fractions* corrisponde a 5/10 di secondo.  

2. Se *fractions* ha valore 50 e *precision* ha valore 2, il valore di *fractions* corrisponde a 50/100 di secondo.  

3. Se *fractions* ha valore 500 e *precision* ha valore 3, il valore di *fractions* corrisponde a 500/1000 di secondo.  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedi anche
[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


