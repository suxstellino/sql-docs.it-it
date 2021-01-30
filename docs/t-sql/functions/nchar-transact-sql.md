---
description: NCHAR (Transact-SQL)
title: NCHAR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- nchar
- nchar_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NCHAR function
- Unicode [SQL Server], NCHAR function
ms.assetid: 68cefc68-7c4f-4326-80c1-300f90cf19db
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e795f73a3e7d6295dc391f6b6539b72f704e397f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174456"
---
# <a name="nchar-transact-sql"></a>NCHAR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il carattere Unicode corrispondente al codice integer specificato, in base alla definizione dello standard Unicode.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
NCHAR ( integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *integer_expression*  
 Quando le regole di confronto del database non contengono il flag di [caratteri supplementari (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters), si tratta di un intero positivo compreso tra 0 e 65535 (tra 0 e 0xFFFF). Se viene specificato un valore non compreso in questo intervallo, viene restituito NULL. Per altre informazioni sui caratteri supplementari, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
 Quando le regole di confronto del database supportano il flag SC, si tratta di un intero positivo compreso tra 0 e 1114111 (tra 0 e 0x10FFFF). Se viene specificato un valore non compreso in questo intervallo, viene restituito NULL.  
  
## <a name="return-types"></a>Tipi restituiti  
 **nchar(1)** quando le regole di confronto predefinite del database non supportano i caratteri supplementari.  
  
 **nvarchar(2)** quando le regole di confronto predefinite del database supportano i caratteri supplementari.  
  
 Se il parametro *integer_expression* rientra nell'intervallo 0 - 0xFFFF, viene restituito un solo carattere. Per i valori superiori, NCHAR restituisce la coppia di surrogati corrispondente. Non creare una coppia di surrogati tramite `NCHAR(<High surrogate>) + NCHAR(\<Low Surrogate>)`. Utilizzare invece regole di confronto del database che supportano caratteri supplementari, quindi specificare il punto di codice Unicode per la coppia di surrogati. Nell'esempio seguente vengono illustrati sia il metodo obsoleto per la creazione di una coppia di surrogati che il metodo preferito per la specifica del punto di codice Unicode.  
  
```sql  
CREATE DATABASE test COLLATE Finnish_Swedish_100_CS_AS_SC;  
DECLARE @d NVARCHAR(10) = N'𣅿';
-- Old style method.  
SELECT NCHAR(0xD84C) + NCHAR(0xDD7F);   
  
-- Preferred method.   
SELECT NCHAR(143743);   
  
-- Alternative preferred method.  
SELECT NCHAR(UNICODE(@d));    
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-nchar-and-unicode"></a>R. Utilizzo delle funzioni NCHAR e UNICODE  
 Nell'esempio seguente vengono utilizzate le funzioni `UNICODE` e `NCHAR` per stampare il valore `UNICODE` e il risultato di `NCHAR` (carattere Unicode) del secondo carattere della stringa di caratteri `København`, nonché il secondo carattere effettivo, ovvero `ø`.  
  
```sql  
DECLARE @nstring NCHAR(8);  
SET @nstring = N'København';  
SELECT UNICODE(SUBSTRING(@nstring, 2, 1)),   
   NCHAR(UNICODE(SUBSTRING(@nstring, 2, 1)));  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -   
248         ø  
(1 row(s) affected)  
```  
  
### <a name="b-using-substring-unicode-convert-and-nchar"></a>B. Utilizzo di SUBSTRING, UNICODE, CONVERT e NCHAR  
 Nell'esempio seguente vengono utilizzate le funzioni `SUBSTRING`, `UNICODE`, `CONVERT`, e `NCHAR` per stampare il numero di caratteri, il carattere Unicode e il valore UNICODE di ogni carattere della stringa `København`.  
  
```sql  
-- The @position variable holds the position of the character currently  
-- being processed. The @nstring variable is the Unicode character   
-- string to process.  
DECLARE @position INT, @nstring NCHAR(9);  
-- Initialize the current position variable to the first character in   
-- the string.  
SET @position = 1;  
-- Initialize the character string variable to the string to process.  
-- Notice that there is an N before the start of the string. This   
-- indicates that the data following the N is Unicode data.  
SET @nstring = N'København';  
-- Print the character number of the position of the string you are at,   
-- the actual Unicode character you are processing, and the UNICODE   
-- value for this particular character.  
PRINT 'Character #' + ' ' + 'Unicode Character' + ' ' + 'UNICODE Value';  
WHILE @position <= DATALENGTH(@nstring)  
   BEGIN  
   SELECT @position,   
      NCHAR(UNICODE(SUBSTRING(@nstring, @position, 1))),  
      CONVERT(NCHAR(17), SUBSTRING(@nstring, @position, 1)),  
      UNICODE(SUBSTRING(@nstring, @position, 1))  
   SELECT @position = @position + 1  
   END;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Character # Unicode Character UNICODE Value  
  
----------- ---- ----------------- -----------   
1           K    K                 75  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
2           ø    ø                 248  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
3           b    b                 98  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
4           e    e                 101  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
5           n    n                 110  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
6           h    h                 104  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
7           a    a                 97  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
8           v    v                 118  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
9           n    n                 110  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
10          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
11          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
12          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
13          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
14          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
15          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
16          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
17          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
18          NULL                   NULL  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ASCII &#40;Transact-SQL&#41;](../../t-sql/functions/ascii-transact-sql.md)  
 [CHAR &#40;Transact-SQL&#41;](../../t-sql/functions/char-transact-sql.md)  
 [UNICODE &#40;Transact-SQL&#41;](../../t-sql/functions/unicode-transact-sql.md)  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
  
  

