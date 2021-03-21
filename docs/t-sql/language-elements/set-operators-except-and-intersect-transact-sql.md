---
description: Operatori sui set - EXCEPT e INTERSECT (Transact-SQL)
title: EXCEPT e INTERSECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INTERSECT_TSQL
- EXCEPT_TSQL
- INTERSECT
- EXCEPT
dev_langs:
- TSQL
helpviewer_keywords:
- EXCEPT operator [Transact-SQL]
- queries [SQL Server], comparing
- comparing queries
- INTERSECT operator
ms.assetid: b1019300-171a-4a1a-854f-e1e751de3565
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b0e3da3f5c659d4b6a5fc5a8240fbcd03be69e5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753931"
---
# <a name="set-operators---except-and-intersect-transact-sql"></a>Operatori sui set - EXCEPT e INTERSECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituiscono righe distinte eseguendo un confronto dei risultati di due query.  
  
EXCEPT restituisce righe distinte dalla query di input sinistra non generate dalla query di input destra.  
 
INTERSECT restituisce righe distinte generate dall'operatore query di input sinistro e destro.  
  
Le regole di base per la combinazione dei set di risultati di due query che usano EXCEPT o INTERSECT sono le seguenti:  
  
-   Tutte le query devono includere lo stesso numero di colonne nello stesso ordine.  
  
-   I tipi di dati devono essere compatibili.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ EXCEPT | INTERSECT }  
{ <query_specification> | ( <query_expression> ) }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
\<_query\_specification_> | ( \<_query\_expression_> )  
Specifica di query o espressione di query che restituisce dati da confrontare con i dati di un'altra specifica o espressione di query. Le definizioni delle colonne incluse in un'operazione EXCEPT o INTERSECT non devono essere necessariamente identiche. Devono tuttavia essere confrontabili tramite una conversione implicita. Se i tipi di dati sono diversi, le regole per la [precedenza dei tipi di dati](../../t-sql/data-types/data-type-precedence-transact-sql.md) determinano il tipo di dati eseguito per il confronto.  
  
Il risultato viene determinato in base alle stesse regole previste per la combinazione di espressioni quando i tipi sono gli stessi ma differiscono per precisione, scala o lunghezza. Per altre informazioni, vedere [Precisione, scala e lunghezza &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
La specifica o l'espressione di query non può restituire colonne di tipo **xml**, **text**, **ntext**, **image** o con un tipo di dati CLR non binario definito dall'utente, perché questi tipi di dati non sono confrontabili.  
  
EXCEPT  
Restituisce tutti i valori distinti della query a sinistra dell'operatore EXCEPT. Tali valori vengono restituiti a condizione che non siano restituiti anche dalla query a destra.  
  
INTERSECT  
Restituisce tutti i valori distinti restituiti da entrambe le query sul lato sinistro e destro dell'operatore INTERSECT.  
  
## <a name="remarks"></a>Osservazioni  
I tipi di dati delle colonne confrontabili vengono restituiti dalle query a sinistra e a destra dell'operatore EXCEPT o INTERSECT. Questi tipi di dati possono includere tipi di dati Char con regole di confronto diverse. In tal caso, il confronto richiesto viene eseguito in base alle regole di [precedenza delle regole di confronto](../../t-sql/statements/collation-precedence-transact-sql.md). Se non è possibile eseguire questa conversione, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] restituisce un errore.  
  
Durante il confronto dei valori di colonna per la determinazione delle righe DISTINCT, due valori NULL vengono considerati uguali.  
  
EXCEPT e INTERSECT restituiscono i nomi di colonna del set di risultati che corrispondono ai nomi di colonna restituiti dalla query a sinistra dell'operatore.  
  
I nomi o gli alias di colonna in clausole ORDER BY devono fare riferimento a nomi di colonna restituiti dalla query a sinistra dell'operatore.  
  
Il supporto dei valori Null in qualsiasi colonna nel set di risultati restituito da EXCEPT o INTERSECT equivale a quello della colonna corrispondente restituita dalla query a sinistra dell'operatore.  
  
Se si usa EXCEPT o INTERSECT insieme ad altri operatori in un'espressione, la valutazione avviene in base all'ordine di precedenza seguente:  
  
1.  Espressioni tra parentesi  
  
2.  Operatore INTERSECT  
  
3.  EXCEPT e UNION, valutati da sinistra verso destra in base alla relativa posizione nell'espressione  
  
È possibile usare EXCEPT o INTERSECT per confrontare più di due set di query. In tal caso, la conversione dei tipi di dati viene determinata tramite il confronto di due query alla volta e in base alle regole sopra indicate per la valutazione delle espressioni.  
  
Non è possibile usare EXCEPT e INTERSECT nelle definizioni di viste partizionate distribuite e nelle notifiche di query.  
 
EXCEPT e INTERSECT possono essere utilizzati in query distribuite, ma vengono eseguiti solo nel server locale e non è possibile eseguirne il push nel server collegato. Pertanto, l'uso di EXCEPT e INTERSECT nelle query distribuite può influire sulle prestazioni.  
  
È possibile usare i cursori fast forward-only e statici nel set di risultati quando vengono usati con un'operazione EXCEPT o INTERSECT. È anche possibile usare un cursore gestito da keyset o dinamico insieme a un'operazione EXCEPT o INTERSECT. In tal caso, il cursore del set di risultati dell'operazione viene convertito in un cursore statico.  
  
Quando un'operazione EXCEPT viene visualizzata tramite la funzionalità Showplan grafico di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], viene indicata come [left anti semi join](../../relational-databases/showplan-logical-and-physical-operators-reference.md), mentre un'operazione INTERSECT viene indicata come [left semi join](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="examples"></a>Esempi  
Negli esempi seguenti viene illustrato l'uso degli operatori `INTERSECT` e `EXCEPT`. La prima query restituisce tutti i valori della tabella `Production.Product` per il confronto con i risultati ottenuti con `INTERSECT` e `EXCEPT`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product ;  
--Result: 504 Rows  
```  
  
La query seguente restituisce tutti i valori distinti restituiti da entrambe le query a sinistra e a destra dell'operatore `INTERSECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
INTERSECT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 238 Rows (products that have work orders)  
```  
  
La query seguente restituisce tutti i valori distinti della query a sinistra dell'operatore `EXCEPT` non presenti nella query a destra.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
EXCEPT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 266 Rows (products without work orders)  
```  
  
La query seguente restituisce tutti i valori distinti della query a sinistra dell'operatore `EXCEPT` non presenti nella query a destra. Vengono utilizzate tabelle invertite rispetto a quelle dell'esempio precedente.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.WorkOrder  
EXCEPT  
SELECT ProductID   
FROM Production.Product ;  
--Result: 0 Rows (work orders without products)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Gli esempi seguenti mostrano come usare gli operatori `INTERSECT` e `EXCEPT`. La prima query restituisce tutti i valori della tabella `FactInternetSales` per il confronto con i risultati ottenuti con `INTERSECT` e `EXCEPT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales;  
--Result: 60398 Rows  
```  
  
La query seguente restituisce tutti i valori distinti restituiti da entrambe le query a sinistra e a destra dell'operatore `INTERSECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
INTERSECT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9133 Rows (Sales to customers that are female.)  
```  
  
La query seguente restituisce tutti i valori distinti della query a sinistra dell'operatore `EXCEPT` non presenti nella query a destra.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
EXCEPT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9351 Rows (Sales to customers that are not female.)  
```  
