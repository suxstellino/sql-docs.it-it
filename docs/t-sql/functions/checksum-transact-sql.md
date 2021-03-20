---
description: CHECKSUM (Transact-SQL)
title: CHECKSUM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHECKSUM_TSQL
- CHECKSUM
dev_langs:
- TSQL
helpviewer_keywords:
- hash indexes
- CHECKSUM function
- checksum values
ms.assetid: e26d3339-845c-49c2-9d89-243376874c13
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e0fd5ebe213b0aba9a58e41bd5d685f35be596c1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749971"
---
# <a name="checksum-transact-sql"></a>CHECKSUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

La funzione `CHECKSUM` restituisce il valore di checksum calcolato su una riga di una tabella o su un elenco di espressioni. Usare `CHECKSUM` per compilare indici hash.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CHECKSUM ( * | expression [ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti
\*  
Questo argomento specifica che il calcolo del valore di checksum copre tutte le colonne di tabella. Se una colonna contiene un tipo di dati non confrontabile, `CHECKSUM` restituisce un errore. I tipi di dati non confrontabili includono:

- **cursor**
- **image**
- **ntext**
- **text**
- **XML**

Un altro tipo di dati non confrontabile è **sql_variant** con uno qualsiasi dei tipi di dati precedenti come tipo di base.
  
*expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di qualsiasi tipo, esclusi i tipi di dati non confrontabili.
  
## <a name="return-types"></a>Tipi restituiti
 **int**  
  
## <a name="remarks"></a>Osservazioni  
`CHECKSUM` calcola un valore hash, denominato checksum, sul relativo elenco di argomenti. Usare questo valore hash per la creazione degli indici hash. Se la funzione `CHECKSUM` ha argomenti di colonna e viene compilato un indice sul valore di `CHECKSUM` calcolato, il risultato sarà un indice hash, che può essere utilizzato per eseguire ricerche di uguaglianza sulle colonne.
  
La funzione `CHECKSUM` soddisfa le proprietà di una funzione hash: quando `CHECKSUM` viene applicata su due elenchi di espressioni qualsiasi restituisce lo stesso valore se gli elementi corrispondenti dei due elenchi sono dello stesso tipo di dati e risultano uguali quando vengono confrontati tramite l'operatore di uguaglianza (=). Ai fini della funzione `CHECKSUM`, i valori Null di un tipo specificato vengono considerati uguali ai fini del confronto. Se almeno uno dei valori nell'elenco di espressioni cambia, è probabile che cambi anche il valore di checksum dell'elenco. Questo comportamento non è tuttavia garantito. Per rilevare se i valori sono stati modificati, è pertanto consigliabile usare `CHECKSUM` solo se l'applicazione può tollerare una modifica mancata occasionale. In caso contrario, prendere in considerazione l'uso di `HASHBYTES`. In presenza di un algoritmo hash MD5 specificato, le probabilità che `HASHBYTES` restituisca lo stesso risultato per due diversi input sono notevolmente inferiori rispetto a `CHECKSUM`.
  
L'ordine di espressione influisce sul valore `CHECKSUM` calcolato. L'ordine delle colonne usate per `CHECKSUM(*)` corrisponde all'ordine delle colonne specificato nella definizione della tabella o della vista, incluse le colonne calcolate.
  
Il valore di `CHECKSUM` dipende dalle regole di confronto. Lo stesso valore archiviato con regole di confronto diverse restituirà un valore di `CHECKSUM` diverso.
  
`CHECKSUM ()` non garantisce risultati univoci.

## <a name="examples"></a>Esempi  
Negli esempi seguenti viene illustrato l'uso di `CHECKSUM` per compilare indici hash.
  
Per compilare l'indice hash, il primo esempio aggiunge una colonna checksum calcolata alla tabella che si vuole indicizzare. Compila quindi un indice sulla colonna checksum. 
  
```sql
-- Create a checksum index.  

SET ARITHABORT ON;  
USE AdventureWorks2012;   
GO  
ALTER TABLE Production.Product  
ADD cs_Pname AS CHECKSUM(Name);  
GO  
CREATE INDEX Pname_index ON Production.Product (cs_Pname);  
GO  
```  
  
In questo esempio viene illustrato l'uso di un indice checksum come indice hash. In questo modo si può migliorare la velocità di indicizzazione quando la colonna da indicizzare è una colonna di tipo carattere long. Può inoltre essere utilizzato per l'esecuzione di ricerche di uguaglianza.
  
```sql
/*Use the index in a SELECT query. Add a second search   
condition to catch stray cases where checksums match,   
but the values are not the same.*/  

SELECT *   
FROM Production.Product  
WHERE CHECKSUM(N'Bearing Ball') = cs_Pname  
AND Name = N'Bearing Ball';  
GO  
```  
  
La creazione dell'indice sulla colonna calcolata materializza la colonna checksum, alla quale verranno propagate tutte le modifiche apportate al valore `ProductName`. In alternativa, si potrebbe compilare un indice direttamente nella colonna che si vuole indicizzare. Per i valori di chiave lunghi un normale indice probabilmente non offrirà prestazioni soddisfacenti quanto un indice checksum.
  
## <a name="see-also"></a>Vedere anche
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
  
  
