---
description: COUNT_BIG (Transact-SQL)
title: COUNT_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COUNT_BIG_TSQL
- COUNT_BIG
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT_BIG function
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT_BIG function
ms.assetid: f2e3601f-487e-4917-bb01-47b1047908cd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 639ee47ccf91d456c4f5a34b4b53a4cfb85ed0dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184160"
---
# <a name="count_big-transact-sql"></a>COUNT_BIG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il numero di elementi presenti in un gruppo. Il funzionamento di `COUNT_BIG` è analogo a quello della funzione [COUNT](../../t-sql/functions/count-transact-sql.md). Queste funzioni differiscono solo per i tipi di dati dei valori restituiti. `COUNT_BIG` restituisce sempre un valore con tipo di dati **bigint**. `COUNT` restituisce sempre un valore con tipo di dati **int**.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT_BIG ( { [ [ ALL | DISTINCT ] expression ] | * } )  
  
-- Analytic Function Syntax  
COUNT_BIG ( [ ALL ] { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
ALL  
Applica la funzione di aggregazione a tutti i valori. ALL funge da valore predefinito.
  
DISTINCT  
Specifica che `COUNT_BIG` restituisce il numero di valori univoci non Null.
  
*expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di qualsiasi tipo. `COUNT_BIG` non supporta le funzioni di aggregazione o le sottoquery in un'espressione.
  
*\**  
Specifica che `COUNT_BIG` deve conteggiare tutte le righe per determinare il numero totale delle righe della tabella da restituire. `COUNT_BIG(*)` non accetta parametri e non supporta l'uso di DISTINCT. `COUNT_BIG(*)` non richiede un parametro *expression* perché per definizione non usa informazioni relative a colonne particolari. `COUNT_BIG(*)` restituisce il numero di righe di una tabella specificata e mantiene le righe duplicate. Conteggia ogni riga separatamente, incluse le righe che contengono valori Null.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] **)**  
*partition_by_clause* divide il set di risultati generato dalla clausola `FROM` in partizioni alle quali viene applicata la funzione `COUNT_BIG`. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo. *order_by_clause* determina l'ordine logico dell'operazione. Per altre informazioni, vedere [Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Tipi restituiti
**bigint**
  
## <a name="remarks"></a>Osservazioni  
COUNT_BIG(\*) restituisce il numero di elementi in un gruppo, inclusi valori NULL e duplicati.
  
COUNT_BIG (ALL *expression*) valuta *expression* per ogni riga in un gruppo e restituisce il numero di valori non Null.
  
COUNT_BIG (DISTINCT *expression*) valuta *expression* per ogni riga in un gruppo e restituisce il numero di valori univoci non Null.
  
COUNT_BIG è una funzione deterministica quando viene usata **_senza_** le clausole ORDER BY e OVER. COUNT_BIG non è deterministica quando viene usata **_con_** le clausole ORDER BY e OVER. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Esempi  
Per esempi, vedere [COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md).
  
## <a name="see-also"></a>Vedere anche
[Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md)  
[int, bigint, smallint e tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
[Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
