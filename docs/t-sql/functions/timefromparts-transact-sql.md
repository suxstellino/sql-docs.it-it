---
description: TIMEFROMPARTS (Transact-SQL)
title: TIMEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TIMEFROMPARTS_TSQL
- TIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- TIMEFROMPARTS function
ms.assetid: 786c65a1-2b3f-4e4b-82b6-4940d62f3801
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db314c5d21ab27e81133048b90546efbbe5bb100
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205500"
---
# <a name="timefromparts-transact-sql"></a>TIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un valore **time** per l'ora specificata e con la precisione indicata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TIMEFROMPARTS ( hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *hour*  
 Espressione intera che specifica le ore.  
  
 *minute*  
 Espressione intera che specifica i minuti.  
  
 *secondi*  
 Espressione intera che specifica i secondi.  
  
 *fractions*  
 Espressione intera che specifica le frazioni.  
  
 *precisione*  
 Valore letterale intero che specifica la precisione del valore **time** da restituire.  
  
## <a name="return-types"></a>Tipi restituiti  
 **time(** *precision* **)**  
  
## <a name="remarks"></a>Osservazioni  
 TIMEROMPARTS restituisce un valore relativo all'ora completamente inizializzato. Se gli argomenti non sono validi, viene generato un errore. Se un parametro è Null, viene restituito Null. Se tuttavia l'argomento *precision* è Null viene generato un errore.  
  
 L'argomento *fractions* dipende dall'argomento *precision*. Se ad esempio *precision* è 7, ogni frazione rappresenta 100 nanosecondi, mentre se *precision* è 3 ogni frazione rappresenta un millisecondo. Se il valore di *precision* è zero, anche il valore di *fractions* deve essere zero. In caso contrario, viene generato un errore.  
  
 Questa funzione può essere eseguita in modalità remota in server [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, ma non in server con versioni precedenti a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-example-without-fractions-of-a-second"></a>R. Esempio semplice senza frazioni di un secondo  
  
```sql
SELECT TIMEFROMPARTS ( 23, 59, 59, 0, 0 ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------------------  
23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Esempio con frazioni di un secondo  
 L'esempio seguente illustra l'uso dei parametri *fractions* e *precision*:  
  
1.  Se *fractions* ha valore 5 e *precision* ha valore 1, il valore di *fractions* corrisponde a 5/10 di secondo.  
  
2.  Se *fractions* ha valore 50 e *precision* ha valore 2, il valore di *fractions* corrisponde a 50/100 di secondo.  
  
3.  Se *fractions* ha valore 500 e *precision* ha valore 3, il valore di *fractions* corrisponde a 500/1000 di secondo.  
  
```sql  
SELECT TIMEFROMPARTS ( 14, 23, 44, 5, 1 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 50, 2 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 500, 3 );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------  
14:23:44.5  
  
(1 row(s) affected)  
  
----------------  
14:23:44.50  
  
(1 row(s) affected)  
  
----------------  
14:23:44.500  
  
(1 row(s) affected)  
```  
  

