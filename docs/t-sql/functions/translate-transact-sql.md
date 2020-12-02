---
description: TRANSLATE (Transact-SQL)
title: TRANSLATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- TRANSLATE
- TRANSLATE_TSQL
helpviewer_keywords:
- TRANSLATE function
ms.assetid: 0426fa90-ef6d-4d19-8207-02ee59f74aec
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49a16c2b5f18eeda3a6abd1396650131543ead4f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88417137"
---
# <a name="translate-transact-sql"></a>TRANSLATE (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

Restituisce la stringa fornita come primo argomento dopo che alcuni caratteri specificati nel secondo argomento sono stati convertiti in un set di caratteri di destinazione specificato nel terzo argomento.

## <a name="syntax"></a>Sintassi

```syntaxsql
TRANSLATE ( inputString, characters, translations)
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

 *inputString*[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) stringa da cercare. *inputString* può essere qualsiasi tipo di dati carattere (nvarchar, varchar, nchar, char).

 *characters*[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) stringa contenente i caratteri da sostituire. *characters* può essere qualsiasi tipo di dati carattere.

*translations*[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) stringa contenente i caratteri sostitutivi. Il tipo di dati e la lunghezza di *translations* devono corrispondere al tipo di dati e alla lunghezza di *characters*.

## <a name="return-types"></a>Tipi restituiti

Restituisce un'espressione di caratteri dello stesso tipo di dati di `inputString` in cui i caratteri del secondo argomento vengono sostituiti con i caratteri corrispondenti del terzo argomento.

## <a name="remarks"></a>Osservazioni

La funzione `TRANSLATE` restituirà un errore se le espressioni *characters* e *translations* hanno lunghezze diverse. `TRANSLATE` restituisce NULL se uno degli argomenti è NULL.  

Il comportamento della funzione `TRANSLATE` è simile all'uso di più funzioni [REPLACE](../../t-sql/functions/replace-transact-sql.md). `TRANSLATE` non sostituisce, tuttavia, alcun carattere singolo in `inputString` più di una volta. Un singolo valore nel parametro `characters` può sostituire più caratteri in `inputString`. 

Questo comportamento è diverso da quello di più funzioni `REPLACE`, perché ogni chiamata di funzione sostituirà tutti i caratteri rilevanti, anche se sono stati sostituiti da una chiamata della funzione `REPLACE` annidata precedente. 

`TRANSLATE` riconosce sempre le regole di confronto SC.

## <a name="examples"></a>Esempi

### <a name="a-replace-square-and-curly-braces-with-regular-braces"></a>R. Sostituire le parentesi quadrate e graffe con parentesi normali

La query seguente sostituisce le parentesi quadrate e graffe nella stringa di input con parentesi normali:

```sql
SELECT TRANSLATE('2*[3+4]/{7-2}', '[]{}', '()()');
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

```text
2*(3+4)/(7-2)
```

#### <a name="equivalent-calls-to-replace"></a>Chiamate equivalenti a REPLACE

Nell'istruzione SELECT seguente è presente un gruppo di quattro chiamate nidificate dirette alla funzione REPLACE. Questo gruppo è equivalente alla chiamata singola effettuata alla funzione TRANSLATE nell'istruzione SELECT precedente:

```sql
SELECT
REPLACE
(
      REPLACE
      (
            REPLACE
            (
                  REPLACE
                  (
                        '2*[3+4]/{7-2}',
                        '[',
                        '('
                  ),
                  ']',
                  ')'
            ),
            '{',
            '('
      ),
      '}',
      ')'
);
```

### <a name="b-convert-geojson-points-into-wkt"></a>B. Convertire i punti GeoJSON in WKT (well-known text)

GeoJSON è un formato per la codifica di un'ampia gamma di strutture di dati geografici. La funzione `TRANSLATE` consente agli sviluppatori di convertire facilmente i punti GeoJSON in formato WKT e viceversa. La query seguente sostituisce le parentesi quadrate e graffe nell'input con parentesi normali:

```sql
SELECT TRANSLATE('[137.4, 72.3]' , '[,]', '( )') AS Point,
    TRANSLATE('(137.4 72.3)' , '( )', '[,]') AS Coordinates;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|Point  |Coordinate |  
|---------|--------- |
|(137.4  72.3) |[137.4,72.3] |

### <a name="c-use-the-translate-function"></a>C. Usare la funzione TRANSLATE

```sql
SELECT TRANSLATE('abcdef','abc','bcd') AS Translated,
       REPLACE(REPLACE(REPLACE('abcdef','a','b'),'b','c'),'c','d') AS Replaced;
```

I risultati sono:

| Convertito | Sostituito |  
| ---------|--------- |
| bcddef | ddddef |


## <a name="see-also"></a>Vedere anche

- [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
- [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
- [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
- [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
- [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
- [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
- [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
- [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
- [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
- [Funzioni per i valori stringa (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)
