---
description: Precedenza degli operatori (Transact-SQL)
title: Precedenza degli operatori (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], precedence
- operator precedence [Transact-SQL]
- order of operator execution [Transact-SQL]
- precedence [SQL Server], operators
ms.assetid: f04d2439-6fff-4e4c-801f-cc62faef510a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7ef9b05c0ecdbb0054c9ad0ef3e1a88b123fb87b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191911"
---
# <a name="operator-precedence-transact-sql"></a>Precedenza degli operatori (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quando in un'espressione complessa vengono usati più operatori, è la precedenza degli operatori a determinare la sequenza di esecuzione delle operazioni. L'ordine di esecuzione può modificare in modo significativo il valore restituito.  
  
 Nella tabella seguente sono indicati i livelli di precedenza degli operatori. Un operatore con livello di precedenza più alto viene valutato prima di un operatore con livello di precedenza più basso. Nella tabella seguente il livello più alto è 1 e il livello più basso è 8.
  
|Level|Operatori|  
|-----------|---------------|  
|1|~ (NOT bit per bit)|  
|2|* (moltiplicazione), / (divisione), % (modulo)|  
|3|+ (positivo), - (negativo), + (addizione), + (concatenazione), - (sottrazione), & (AND bit per bit), ^ (OR esclusivo bit per bit), &#124; (OR bit per bit)|  
|4|=, >, \<, >=, <=, <>, !=, !>, !< (operatori di confronto)|  
|5|NOT|  
|6|AND|  
|7|ALL, ANY, BETWEEN, IN, LIKE, OR, SOME|  
|8|= (assegnazione)|  
  
 Se due operatori di un'espressione hanno lo stesso livello di precedenza, vengono valutati da sinistra verso destra in base alla posizione nell'espressione. Nell'espressione utilizzata nell'istruzione `SET` seguente, ad esempio, l'operatore di sottrazione viene valutato prima dell'operatore di addizione.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 4 - 2 + 27;  
-- Evaluates to 2 + 27 which yields an expression result of 29.  
SELECT @MyNumber;  
```  
  
 Per ignorare l'ordine di precedenza predefinito degli operatori in un'espressione, è possibile utilizzare le parentesi. Tutto ciò che è racchiuso tra parentesi viene valutato per restituire un singolo valore. Tale valore può essere usato dagli operatori all'esterno delle parentesi.  
  
 Nell'espressione utilizzata nell'istruzione `SET` seguente, ad esempio, l'operatore di moltiplicazione ha un livello di precedenza più alto rispetto all'operatore di addizione L'operazione di moltiplicazione viene quindi valutata per prima. Il risultato dell'espressione è `13`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * 4 + 5;  
-- Evaluates to 8 + 5 which yields an expression result of 13.  
SELECT @MyNumber;  
```  
  
 Nell'espressione usata nell'istruzione `SET` seguente le parentesi fanno sì che venga valutata per prima l'addizione. Il risultato dell'espressione è `18`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + 5);  
-- Evaluates to 2 * 9 which yields an expression result of 18.  
SELECT @MyNumber;  
```  
  
 Se in un'espressione vengono utilizzate parentesi nidificate, viene valutata per prima l'espressione più interna. Nell'esempio seguente con parentesi nidificate, l'espressione `5 - 3` corrisponde al set di parentesi più interno della struttura nidificata. Questa espressione restituisce il valore `2`. L'operatore di addizione (`+`) somma quindi questo risultato a `4`: il risultato prodotto è `6`. Infine, il valore `6` viene moltiplicato per `2` e il risultato dell'espressione `12`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + (5 - 3) );  
-- Evaluates to 2 * (4 + 2) which then evaluates to 2 * 6, and   
-- yields an expression result of 12.  
SELECT @MyNumber;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Operatori logici &#40;Transact-SQL&#41;](../../t-sql/language-elements/logical-operators-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
  
