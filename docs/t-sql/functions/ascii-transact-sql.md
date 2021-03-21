---
description: ASCII (Transact-SQL)
title: ASCII (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASCII_TSQL
- ASCII
dev_langs:
- TSQL
helpviewer_keywords:
- ASCII function
- characters [SQL Server], ASCII
- code [SQL Server], ASCII
- leftmost character of expression
ms.assetid: 45c2044a-0593-4805-8bae-0fad4bde2e6b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bc38a1198aae090b4198779b311dd4bd921cd6c1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740421"
---
# <a name="ascii-transact-sql"></a>ASCII (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce il codice ASCII del carattere più a sinistra in un'espressione di caratteri.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ASCII ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*character_expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di tipo **char** o **varchar**.
  
## <a name="return-types"></a>Tipi restituiti
 **int**  
  
## <a name="remarks"></a>Osservazioni
ASCII è l'acronimo di **A** merican **S** tandard **C** ode for **I** nformation **I** nterchange. Si tratta di uno standard di codifica dei caratteri per i computer moderni. Per un elenco dei caratteri ASCII, vedere la sezione **Printable characters** (Caratteri stampabili) in [ASCII](https://www.wikipedia.org/wiki/ASCII).

ASCII è un set di caratteri a 7 bit. ASCII esteso o ASCII alto è un set di caratteri a 8 bit non gestito dalla funzione `ASCII`. 

## <a name="examples"></a>Esempi 

### <a name="a-this-example-assumes-an-ascii-character-set-and-returns-the-ascii-value-for-6-characters"></a>R. L'esempio seguente presuppone l'uso del set di caratteri ASCII e restituisce il valore `ASCII` per 6 caratteri.
  
```sql
SELECT ASCII('A') AS A, ASCII('B') AS B,   
ASCII('a') AS a, ASCII('b') AS b,  
ASCII(1) AS [1], ASCII(2) AS [2];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
A           B           a           b           1           2  
----------- ----------- ----------- ----------- ----------- -----------  
65          66          97          98          49          50  
```  
  
### <a name="b-this-examples-shows-how-a-7-bit-ascii-value-is-returned-correctly-but-an-8-bit-extended-ascii-value-is-not-handled"></a>B. Questo esempio illustra come viene restituito correttamente un valore ASCII a 7 bit, ma non viene gestito un valore ASCII esteso a 8 bit.

```sql
SELECT ASCII('P') AS [ASCII], ASCII('æ') AS [Extended_ASCII];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ASCII       Extended_ASCII
----------- --------------
80          195
```

Per verificare se i risultati precedenti sono mappati al punto di codice carattere corretto, usare i valori di output con la funzione `CHAR` o `NCHAR`:

```sql
SELECT NCHAR(80) AS [CHARACTER], NCHAR(195) AS [CHARACTER];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
CHARACTER CHARACTER
--------- ---------
P         Ã
```

Dal risultato precedente si noti che il carattere per il punto di codice 195 è **Ã** e non **æ**. Ciò è dovuto al fatto che la funzione `ASCII` è in grado di leggere il primo flusso a 7 bit, ma non il bit aggiuntivo. Il punto di codice corretto per i carattere `æ` può essere trovato usando la funzione `UNICODE`, che è in grado di restituire il punto di codice carattere corretto:

```sql
SELECT UNICODE('æ') AS [Extended_ASCII], NCHAR(230) AS [CHARACTER];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Extended_ASCII CHARACTER
-------------- ---------
230            æ
```

## <a name="see-also"></a>Vedere anche
 [CHAR &#40;Transact-SQL&#41;](../../t-sql/functions/char-transact-sql.md)  
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)  
 [UNICODE &#40;Transact-SQL&#41;](../../t-sql/functions/unicode-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
  
