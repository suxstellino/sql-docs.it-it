---
description: STRING_ESCAPE (Transact-SQL)
title: STRING_ESCAPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_ESCAPE
- STRING_ESCAPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_ESCAPE function
ms.assetid: 2163bc7a-3816-4304-9c40-8954804f5465
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: fb23ecedcaa224d66a6d23a113e696dd05caae05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207930"
---
# <a name="string_escape-transact-sql"></a>STRING_ESCAPE (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Aggiunge un carattere di escape prima di caratteri speciali nei testi e restituisce testo con caratteri preceduti da un carattere di escape. **STRING_ESCAPE** è una funzione deterministica introdotta in SQL Server 2016. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
STRING_ESCAPE( text , type )  
```  

## <a name="arguments"></a>Argomenti

 *text*  
 Espressione **nvarchar**[expression](../../t-sql/language-elements/expressions-transact-sql.md) che rappresenta l'oggetto al quale far precedere un carattere di escape.  
  
 *type*  
 Regole di escape che verranno applicate. Il valore supportato è attualmente `'json'`.  
  
## <a name="return-types"></a>Tipi restituiti

 Testo **nvarchar (max)** con caratteri speciali e di controllo preceduti da un carattere di escape. Attualmente **STRING_ESCAPE** fa precedere un carattere di escape solo ai caratteri speciali JSON illustrati nelle tabelle seguenti.  
  
|Carattere speciale|Sequenza codificata|  
|-----------------------|----------------------|  
|Virgoletta (")|\\"|  
|Barra rovesciata (\\)| \\\\ |  
|Barra (/)|\\/|  
|Backspace|\b|  
|Avanzamento carta|\f|  
|Nuova riga|\n|  
|Ritorno a capo|\r|  
|Tabulazione orizzontale|\t|  
  
|Carattere di controllo|Sequenza codificata|  
|-----------------------|----------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|...|...|  
|CHAR(31)|\u001f|  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="examples"></a>Esempi  
  
### <a name="a--escape-text-according-to-the-json-formatting-rules"></a>R.  Aggiungere un carattere di escape al testo in base alle regole di formattazione JSON

 La query seguente fa precedere da un carattere di escape i caratteri speciali mediante regole JSON e restituisce testo preceduto da un carattere di escape.  
  
```sql
SELECT STRING_ESCAPE('\   /  
\\    "     ', 'json') AS escapedText;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
escapedText  
-------------------------------------------------------------  
\\\t\/\n\\\\\t\"\t
```  
  
### <a name="b-format-json-object"></a>B. Formattare un oggetto JSON

 La query seguente crea testo JSON da variabili di stringa e numero e aggiunge un carattere di escape prima di qualsiasi carattere speciale JSON nelle variabili.  
  
```
SET @json = FORMATMESSAGE('{ "id": %d,"name": "%s", "surname": "%s" }',
    17, STRING_ESCAPE(@name,'json'), STRING_ESCAPE(@surname,'json') );  
```  
  
## <a name="see-also"></a>Vedere anche

- [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
- [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
- [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
- [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
- [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
- [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
- [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
- [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
- [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
- [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
