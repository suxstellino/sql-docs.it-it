---
description: Funzioni delle regole di confronto - TERTIARY_WEIGHTS (Transact-SQL)
title: TERTIARY_WEIGHTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TERTIARY_WEIGHTS_TSQL
- TERTIARY_WEIGHTS
dev_langs:
- TSQL
helpviewer_keywords:
- weights [SQL Server]
- SQL tertiary collations
- TERTIARY_WEIGHTS function
ms.assetid: 7e1f5350-260b-4c61-8c84-69bb1a214f1f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d5b6e88f49b2452184a77ebd103e5050efe33a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209996"
---
# <a name="collation-functions---tertiary_weights-transact-sql"></a>Funzioni delle regole di confronto - TERTIARY_WEIGHTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce una stringa binaria di spessori per ogni carattere in un'espressione stringa non Unicode definita tramite una regola di confronto SQL terziaria.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TERTIARY_WEIGHTS( non_Unicode_character_string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*non_Unicode_character_string_expression*  
Un'[espressione](../../t-sql/language-elements/expressions-transact-sql.md) stringa di tipo **char**, **varchar** o **varchar(max)** definita tramite una regola di confronto SQL terziaria. Per un elenco di queste regole di confronto, vedere la sezione Osservazioni.
  
## <a name="return-types"></a>Tipi restituiti
`TERTIARY_WEIGHTS` restituisce **varbinary** quando *non_Unicode_character_string_expression* è **char** o **varchar** e restituisce **varbinary(max)** quando *non_Unicode_character_string_expression* ha il tipo di dati **varchar(max)**.
  
## <a name="remarks"></a>Osservazioni  
`TERTIARY_WEIGHTS` restituisce NULL quando una raccolta SQL terziaria non definisce *non_Unicode_character_string_expression*. La tabella seguente elenca le regole di confronto SQL terziarie:
  
|ID tipo di ordinamento|Regole di confronto SQL|  
|---|---|
|33|SQL_Latin1_General_Pref_CP437_CI_AS|  
|34|SQL_Latin1_General_CP437_CI_AI|  
|43|SQL_Latin1_General_Pref_CP850_CI_AS|  
|44|SQL_Latin1_General_CP850_CI_AI|  
|49|SQL_1xCompat_CP850_CI_AS|  
|53|SQL_Latin1_General_Pref_CP1_CI_AS|  
|54|SQL_Latin1_General_CP1_CI_AI|  
|56|SQL_AltDiction_Pref_CP850_CI_AS|  
|57|SQL_AltDiction_CP850_CI_AI|  
|58|SQL_Scandinavian_Pref_CP850_CI_AS|  
|82|SQL_Latin1_General_CP1250_CI_AS|  
|84|SQL_Czech_CP1250_CI_AS|  
|86|SQL_Hungarian_CP1250_CI_AS|  
|88|SQL_Polish_CP1250_CI_AS|  
|90|SQL_Romanian_CP1250_CI_AS|  
|92|SQL_Croatian_CP1250_CI_AS|  
|94|SQL_Slovak_CP1250_CI_AS|  
|96|SQL_Slovenian_CP1250_CI_AS|  
|106|SQL_Latin1_General_CP1251_CI_AS|  
|108|SQL_Ukrainian_CP1251_CI_AS|  
|113|SQL_Latin1_General_CP1253_CS_AS|  
|114|SQL_Latin1_General_CP1253_CI_AS|  
|130|SQL_Latin1_General_CP1254_CI_AS|  
|146|SQL_Latin1_General_CP1256_CI_AS|  
|154|SQL_Latin1_General_CP1257_CI_AS|  
|156|SQL_Estonian_CP1257_CI_AS|  
|158|SQL_Latvian_CP1257_CI_AS|  
|160|SQL_Lithuanian_CP1257_CI_AS|  
|183|SQL_Danish_Pref_CP1_CI_AS|  
|184|SQL_SwedishPhone_Pref_CP1_CI_AS|  
|185|SQL_SwedishStd_Pref_CP1_CI_AS|  
|186|SQL_Icelandic_Pref_CP1_CI_AS|  
  
Usare `TERTIARY_WEIGHTS` per la definizione di una colonna calcolata definita in base ai valori di una colonna di tipo **char**, **varchar** o **varchar(max)**. La definizione dell'indice sia nella colonna calcolata che nella colonna di tipo **char**, **varchar** o **varchar(max)** può migliorare le prestazioni quando la clausola ORDER BY di una query specifica quella colonna **char**, **varchar** o **varchar(max)**.
  
## <a name="examples"></a>Esempi  
In questo esempio viene creata una colonna calcolata in una tabella in cui la funzione `TERTIARY_WEIGHTS` viene applicata ai valori di una colonna di tipo `char`:
  
```sql
CREATE TABLE TertColTable  
(Col1 char(15) COLLATE SQL_Latin1_General_Pref_CP437_CI_AS,  
Col2 AS TERTIARY_WEIGHTS(Col1));  
GO   
```  
  
## <a name="see-also"></a>Vedere anche
[Clausola ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)
  
  
