---
description: DIFFERENCE (Transact-SQL)
title: DIFFERENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DIFFERENCE
- DIFFERENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DIFFERENCE function
- comparing SOUNDEX values
- SOUNDEX values
ms.assetid: c58ca25d-d6ea-48fa-93bb-c9374b0b2a2b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1829007e42e955b900ca885cd0ed01d43b1ca0c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158388"
---
# <a name="difference-transact-sql"></a>DIFFERENCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce un valore integer che misura la differenza tra i valori [SOUNDEX()](./soundex-transact-sql.md) di due espressioni di caratteri diverse.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DIFFERENCE ( character_expression , character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*character_expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) alfanumerica di dati di tipo carattere. *character_expression* può essere una costante, una variabile o una colonna.  
  
## <a name="return-types"></a>Tipi restituiti  
**int**  
 
## <a name="remarks"></a>Osservazioni  
`DIFFERENCE` confronta due valori `SOUNDEX` diversi e restituisce un valore integer. Questo valore misura il grado di corrispondenza di `SOUNDEX`, su una scala da 0 a 4. Il valore 0 indica una somiglianza scarsa o del tutto assente tra i valori SOUNDEX. 4 indica una somiglianza forte o l'esatta corrispondenza dei valori SOUNDEX.  
  
`DIFFERENCE` e `SOUNDEX` supportano la sensibilità delle regole di confronto.  
  
## <a name="examples"></a>Esempi  
La prima parte di questo esempio confronta i valori `SOUNDEX` di due stringhe molto simili. Per le regole di confronto Latin1_General, `DIFFERENCE` restituisce un valore `4`. La seconda parte dell'esempio confronta i valori `SOUNDEX` di due stringhe molto diverse. Per le regole di confronto Latin1_General `DIFFERENCE`, restituisce `0`.  
  
```sql  
-- Returns a DIFFERENCE value of 4, the least possible difference.  
SELECT SOUNDEX('Green'), SOUNDEX('Greene'), DIFFERENCE('Green','Greene');  
GO  
-- Returns a DIFFERENCE value of 0, the highest possible difference.  
SELECT SOUNDEX('Blotchet-Halls'), SOUNDEX('Greene'), DIFFERENCE('Blotchet-Halls', 'Greene');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----- ----- -----------   
G650  G650  4             
  
(1 row(s) affected)  
  
----- ----- -----------   
B432  G650  0             
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [SOUNDEX &#40;Transact-SQL&#41;](../../t-sql/functions/soundex-transact-sql.md)   
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

