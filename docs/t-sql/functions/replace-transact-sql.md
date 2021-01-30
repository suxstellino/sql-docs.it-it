---
title: REPLACE (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione REPLACE, che sostituisce tutte le occorrenze di un valore stringa specificato con un altro valore stringa.
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REPLACE_TSQL
- REPLACE
dev_langs:
- TSQL
helpviewer_keywords:
- first string expression [SQL Server]
- replacing string expression
- third string expressions [SQL Server]
- second string expressions [SQL Server]
- REPLACE function
ms.assetid: 8a7aaaf2-62e3-46c0-8e44-fa22290dd86b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbd685e29e9773eea41be391b1bdfd7b1c66fdb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182299"
---
# <a name="replace-transact-sql"></a>REPLACE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sostituisce tutte le occorrenze di un valore stringa specificato con un altro valore stringa.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
REPLACE ( string_expression , string_pattern , string_replacement )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *string_expression*  
 Stringa [expression](../../t-sql/language-elements/expressions-transact-sql.md) da cercare. Il tipo di dati di *string_expression* può essere character o binary.  
  
 *string\_pattern*  
 Sottostringa da individuare. Il tipo di dati di *string_pattern* può essere character o binary. *string_pattern* non può essere una stringa vuota (") e non deve superare il numero massimo di byte che rientra in una pagina.  
  
 *string\_replacement*  
 Stringa di sostituzione. Il tipo di dati di *string_replacement* può essere character o binary.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce **nvarchar** se uno degli argomenti di input è di tipo **nvarchar**. In caso contrario REPLACE restituisce **varchar**.  
  
 Viene restituito NULL se un argomento qualsiasi è NULL.  
  
 Se *string_expression* non è di tipo **varchar(max)** o **nvarchar(max), REPLACE** tronca il valore restituito a 8000 byte. Per restituire valori maggiori di 8000 byte è necessario eseguire il cast esplicito di *string_expression* a un tipo di dati appropriato per valori di grandi dimensioni.  
  
## <a name="remarks"></a>Osservazioni  
 Tramite REPLACE vengono effettuati confronti in base alle regole di confronto dell'input. Per eseguire un confronto in base a regole di confronto specifiche, è possibile usare [COLLATE](~/t-sql/statements/collations.md) per applicare regole di confronto esplicite all'input.  
  
 0x0000 (**char(0)** ) è un carattere non definito nelle regole di confronto di Windows e non può essere incluso in REPLACE.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente la stringa `cde` in `abcdefghi` viene sostituita da `xxx`.  
  
```sql  
SELECT REPLACE('abcdefghicde','cde','xxx');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
abxxxfghixxx  
(1 row(s) affected)  
```  
  
 Nell'esempio seguente viene utilizzata la funzione `COLLATE`.  
  
```sql  
SELECT REPLACE('This is a Test'  COLLATE Latin1_General_BIN,  
'Test', 'desk' );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
This is a desk  
(1 row(s) affected)  
```  

  
## <a name="see-also"></a>Vedere anche  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
