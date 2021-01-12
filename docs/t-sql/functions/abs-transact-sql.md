---
description: ABS (Transact-SQL)
title: ABS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ABS_TSQL
- ABS
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], positive
- values [SQL Server], absolute
- ABS function
- absolute positive value
ms.assetid: e2ea7a6d-3e2f-472c-afbc-437d3b835c03
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42ea812d8d54a307951bbc1c88ab018cc31dc52a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090802"
---
# <a name="abs-transact-sql"></a>ABS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Funzione matematica che restituisce il valore assoluto (positivo) dell'espressione numerica specificata. `ABS` cambia i valori negativi in valori positivi. `ABS` non ha effetto sui valori pari a zero o positivi.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ABS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*numeric_expression*  
Espressione della categoria del tipo di dati numerici esatti o numerici approssimati.
  
## <a name="return-types"></a>Tipi restituiti  
Restituisce lo stesso tipo di *numeric_expression*.
  
## <a name="examples"></a>Esempi  
L'esempio illustra i risultati dell'uso della funzione `ABS` in tre numeri diversi.
  
```sql
SELECT ABS(-1.0), ABS(0.0), ABS(1.0);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---- ---- ----  
1.0  .0   1.0  
```  
  
La funzione `ABS` può produrre un errore di overflow quando il valore assoluto di un numero è maggiore del numero più alto che può essere rappresentato dal tipo di dati specificato. Ad esempio, il tipo di dati `int` ha intervallo di valori da `-2,147,483,648` a `2,147,483,647`. Il calcolo del valore assoluto per il valore integer con segno `-2,147,483,648` provoca un errore di overflow, in quanto il valore assoluto corrispondente è maggiore dell'intervallo positivo per il tipo di dati `int`.
  
```sql
DECLARE @i INT;  
SET @i = -2147483648;  
SELECT ABS(@i);  
GO  
```  
  
Restituisce il messaggio di errore seguente:
  
"Messaggio 8115, livello 16, stato 2, riga 3"
  
"Errore di runtime: si è verificato un errore di overflow aritmetico durante la conversione del tipo di dati da espressione a int".

  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Funzioni matematiche &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  
[Funzioni predefinite &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)
  
  

