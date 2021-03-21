---
description: LTRIM (Transact-SQL)
title: LTRIM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LTRIM
- LTRIM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- leading blanks
- deleting blank spaces
- characters [SQL Server], blanks
- removing blank spaces
- LTRIM function
- blank characters [SQL Server]
ms.assetid: 369ed340-1a09-4597-a9eb-6720156cd39a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e6c6b416e1a46082c8a2fa042b303dd2bdcf4282
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747991"
---
# <a name="ltrim-transact-sql"></a>LTRIM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una espressione di caratteri dopo aver rimosso gli spazi vuoti iniziali.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
LTRIM ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *character_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di dati di tipo carattere o binario. *character_expression* può essere una costante, una variabile o una colonna. *character_expression* deve essere di un tipo di dati, tranne **text**, **ntext** e **image**, che può essere convertito in modo implicito in **varchar**. In caso contrario usare [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) per convertire in modo esplicito *character_expression*.  
  
## <a name="return-type"></a>Tipo restituito  
 **varchar** o **nvarchar**  
  
## <a name="examples"></a>Esempi  

### <a name="a-simple-example"></a>R. Esempio semplice   

 Nell'esempio seguente viene usato LTRIM per rimuovere gli spazi iniziali da un'espressione di caratteri.  
  
```sql  
SELECT LTRIM('     Five spaces are at the beginning of this string.');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ---------------------------------------------------------------  
  Five spaces are at the beginning of this string.
  ```  

### <a name="b-example-using-a-variable"></a>B: Esempio di uso di una variabile   
  
 Nell'esempio seguente viene utilizzato `LTRIM` per rimuovere gli spazi iniziali da una variabile di tipo carattere.  
  
```sql  
DECLARE @string_to_trim VARCHAR(60);  
SET @string_to_trim = '     5 spaces are at the beginning of this string.';  
SELECT 
    @string_to_trim AS 'Original string',
    LTRIM(@string_to_trim) AS 'Without spaces';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Original string Without spaces
--------------------------------------------------- ---------------------------------------------
     5 spaces are at the beginning of this string.  5 spaces are at the beginning of this string.
```  
  
## <a name="see-also"></a>Vedere anche  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


