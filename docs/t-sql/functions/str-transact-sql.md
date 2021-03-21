---
description: STR (Transact-SQL)
title: STR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STR
- STR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- converting numbers to characters
- characters [SQL Server], converting
- character data [SQL Server]
- STR function
ms.assetid: de03531b-d9e7-4c3c-9604-14e582ac20c6
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1151dd72f4f1ccf12ed38d39143a79d4180fc70a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753421"
---
# <a name="str-transact-sql"></a>STR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce dati di tipo carattere convertiti da dati di tipo numerico. I dati di tipo carattere sono giustificati a destra, con una lunghezza e una precisione decimale specificate. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
STR ( float_expression [ , length [ , decimal ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *float_expression*  
 Espressione del tipo di dati numerico approssimato (**float**) con un separatore decimale.  
  
 *length*  
 Lunghezza totale, che include il separatore decimale, il segno, le cifre e gli spazi. Il valore predefinito è 10.  
  
 *decimal*  
 Numero di posizioni a destra del separatore decimale. Il valore di *decimal* deve essere minore o uguale a 16. Se *decimal* è maggiore di 16, il risultato viene troncato in modo da includere sedici posizioni a destra del separatore decimale.  
  
## <a name="return-types"></a>Tipi restituiti  
 **varchar**  
  
## <a name="remarks"></a>Osservazioni  
 I valori dei parametri *length* e *decimal* della funzione STR devono essere positivi. Il numero viene arrotondato a un valore intero per impostazione predefinita o se il parametro decimal è 0. La lunghezza specificata deve essere maggiore o uguale alla parte intera del numero (a sinistra del separatore decimale) più il segno del numero, se disponibile. Un argomento di tipo *float_expression* breve viene allineato a destra in base alla lunghezza specificata, mentre un argomento di tipo *float_expression* lungo viene troncato al numero di cifre decimali specificato. Ad esempio, STR(12,10) restituisce il risultato 12, che viene allineato a destra nel set di risultati. STR(1223,2) tronca invece il set di risultati a \*\*. Le funzioni stringa possono essere nidificate.  
  
> [!NOTE]  
>  Per eseguire la conversione in dati Unicode, usare STR in una funzione di conversione CONVERT o [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente un'espressione composta da cinque cifre e un separatore decimale viene convertita in una stringa di caratteri con sei posizioni. La parte frazionaria del numero viene arrotondata a una cifra decimale.  
  
```sql
SELECT STR(123.45, 6, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------  
 123.5  
  
(1 row(s) affected)  
```  
  
 Quando l'espressione supera la lunghezza specificata, la stringa restituisce `**` per la lunghezza specificata.  
  
```sql
SELECT STR(123.45, 2, 2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--  
**  
  
(1 row(s) affected)  
```  
  
 Quando nella funzione `STR` i dati numerici sono nidificati, il risultato corrisponde comunque a dati di tipo carattere nel formato specificato.  
  
```sql
SELECT STR (FLOOR (123.45), 8, 3);
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
 123.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
 [FORMAT &#40;Transact-SQL&#41;](../../t-sql/functions/format-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

