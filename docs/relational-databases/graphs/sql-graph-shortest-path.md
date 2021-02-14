---
description: SHORTEST_PATH (Transact-SQL)
title: PERCORSO più breve (grafo SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
f1_keywords:
- SHORTEST PATH
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 9e5a6f6506e4ce40b3b89f32dd58329eb4667760
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351402"
---
# <a name="shortest_path-transact-sql"></a>SHORTEST_PATH (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

  Specifica una condizione di ricerca per un grafo, in cui viene eseguita la ricerca in modo ricorsivo o ripetitivo. Nell'istruzione SELECT è possibile usare SHORTEST_PATH per la corrispondenza con il nodo del grafico e le tabelle Edge. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="shortest-path"></a>Percorso più breve
La funzione SHORTEST_PATH consente di trovare:    
* Un percorso più breve tra due nodi/entità specificati
* Percorsi più brevi di origine singola.
* Percorso più breve da più nodi di origine a più nodi di destinazione.

Accetta un modello di lunghezza arbitrario come input e restituisce un percorso più breve esistente tra due nodi. Questa funzione può essere usata solo all'interno di MATCH. La funzione restituisce un solo percorso più breve tra due nodi specificati. Se esiste, due o più percorsi più brevi della stessa lunghezza tra qualsiasi coppia di nodi di origine e di destinazione, la funzione restituisce un solo percorso trovato per primo durante l'attraversamento. Si noti che un modello di lunghezza arbitraria può essere specificato solo all'interno di una funzione SHORTEST_PATH. 

Per la sintassi, fare riferimento alla [corrispondenza (grafo SQL)](../../t-sql/queries/match-sql-graph.md) . 

## <a name="for-path"></a>PER IL PERCORSO
PER PATH deve essere usato con qualsiasi nome di tabella Node o Edge nella clausola FROM, che parteciperà a un modello di lunghezza arbitraria. PER PATH indica al motore che la tabella Node o Edge restituirà una raccolta ordinata che rappresenta l'elenco di nodi o bordi trovati lungo il percorso attraversato. Gli attributi di queste tabelle non possono essere proiettati direttamente nella clausola SELECT. Per proiettare gli attributi da queste tabelle, è necessario utilizzare le funzioni di aggregazione del percorso grafico.  

## <a name="arbitrary-length-pattern"></a>Modello di lunghezza arbitraria
Questo modello include i nodi e i bordi che devono essere attraversati ripetutamente fino a raggiungere il nodo desiderato o fino a quando non viene soddisfatto il numero massimo di iterazioni specificato nel criterio. Ogni volta che viene eseguita la query, il risultato dell'esecuzione di questo modello sarà una raccolta ordinata dei nodi e dei bordi attraversati lungo il percorso dal nodo iniziale al nodo finale. Si tratta di un modello di sintassi di tipo espressione regolare e sono supportati i due quantificatori di criteri seguenti:

* **' +'**: Ripetere il modello 1 o più volte. Terminare non appena viene trovato un percorso più breve.
* **{1,n}** : Ripetere il criterio da una a "n" volte. Termina non appena viene rilevata una più breve.

## <a name="last_node"></a>LAST_NODE
La funzione LAST_NODE () consente il concatenamento di due modelli di attraversamento di lunghezza arbitraria. Può essere usato in scenari in cui:    
* In una query vengono usati più modelli di percorso più brevi e un modello inizia in corrispondenza dell'ultimo nodo del modello precedente.
* Due modelli di percorso più brevi si uniscono nello stesso LAST_NODE ().

## <a name="graph-path-order"></a>Ordine percorso grafico
L'ordine del percorso del grafo si riferisce all'ordine dei dati nel percorso di output. L'ordine del percorso di output inizia sempre in corrispondenza della parte non ricorsiva del modello seguito dai nodi/bordi visualizzati nella parte ricorsiva. L'ordine in cui il grafico viene attraversato durante l'ottimizzazione o l'esecuzione della query non ha nulla a che fare con l'ordine stampato nell'output. Analogamente, anche la direzione della freccia nel modello ricorsivo non influisce sull'ordine del tracciato del grafico. 

## <a name="graph-path-aggregate-functions"></a>Funzioni di aggregazione percorso grafico
Poiché i nodi e i bordi che coinvolgono il modello di lunghezza arbitraria restituiscono una raccolta (dei nodi e dei bordi attraversati in tale percorso), gli utenti non possono proiettare gli attributi direttamente usando la sintassi tablename. AttributeName convenzionale. Per le query in cui è necessario per proiettare i valori di attributo dalle tabelle dei nodi intermedi o dei bordi, nel percorso attraversato usare le funzioni di aggregazione del percorso grafico seguenti: STRING_AGG, LAST_VALUE, SUM, AVG, MIN, MAX e COUNT. La sintassi generale per utilizzare queste funzioni di aggregazione nella clausola SELECT è la seguente:

```syntaxsql
<GRAPH_PATH_AGGREGATE_FUNCTION>(<expression> , <separator>)  <order_clause>

    <order_clause> ::=
        { WITHIN GROUP (GRAPH PATH) }

    <GRAPH_PATH_AGGREGATE_FUNCTION> ::=
          STRING_AGG
        | LAST_VALUE
        | SUM
        | COUNT
        | AVG
        | MIN
        | MAX

```

### <a name="string_agg"></a>STRING_AGG
La funzione STRING_AGG accetta un'espressione e un separatore come input e restituisce una stringa. Gli utenti possono utilizzare questa funzione nella clausola SELECT per proiettare gli attributi dai nodi intermedi o dai bordi del percorso attraversato. 

### <a name="last_value"></a>LAST_VALUE
Per proiettare gli attributi dall'ultimo nodo attraversato, è possibile usare LAST_VALUE funzione di aggregazione. Non è possibile specificare alias della tabella Edge come input per questa funzione. è possibile usare solo nomi o alias di tabella del nodo.

**Ultimo nodo**: l'ultimo nodo fa riferimento al nodo che viene visualizzato per ultimo nel percorso attraversato, indipendentemente dalla direzione della freccia nel predicato di corrispondenza. Ad esempio: `MATCH(SHORTEST_PATH(n(-(e)->p)+) )`. Qui l'ultimo nodo del percorso sarà l'ultimo nodo P visitato. 

Mentre l'ultimo nodo è l'ultimo ennesimo nodo nel percorso del grafico di output per questo modello: `MATCH(SHORTEST_PATH((n<-(e)-)+p))`    

### <a name="sum"></a>SUM
Questa funzione restituisce la somma dei valori dell'attributo node/Edge forniti o dell'espressione che è stata visualizzata nel percorso attraversato.

### <a name="count"></a>COUNT
Questa funzione restituisce il numero di valori non null dell'attributo node/Edge desiderato nel percorso. La funzione COUNT supporta l' \* operatore '' con un alias di tabella Node o Edge. Senza l'alias della tabella Node o Edge, l'utilizzo di \* è ambiguo e verrà generato un errore.

```syntaxsql
{  COUNT( <expression> | <node_or_edge_alias>.* )  <order_clause>  }
```

### <a name="avg"></a>MEDIA
Restituisce la media dei valori dell'attributo node/Edge forniti o dell'espressione che è stata visualizzata nel percorso attraversato.

### <a name="min"></a>MIN
Restituisce il valore minimo dall'espressione o dai valori di attributo node/Edge forniti visualizzati nel percorso attraversato.

### <a name="max"></a>MAX
Restituisce il valore massimo dai valori o dall'espressione dell'attributo node/Edge fornito visualizzato nel percorso attraversato.

## <a name="remarks"></a>Commenti  
shortest_path funzione può essere usata solo all'interno di MATCH.     
LAST_NODE è supportato solo all'interno shortest_path.     
La ricerca del percorso più breve ponderato, di tutti i percorsi o di tutti i percorsi più brevi non è supportata.         
In alcuni casi, è possibile che vengano generati piani danneggiati per le query con un numero maggiore di hop, il che comporta tempi di esecuzione delle query più elevati. L'uso di un hint di hash join può essere utile.    


## <a name="examples"></a>Esempio 
Per le query di esempio illustrate di seguito, verrà usato il nodo e le tabelle Edge create in [SQL Graph Sample](./sql-graph-sample.md)

### <a name="a--find-shortest-path-between-2-people"></a>R.  Trova il percorso più breve tra due persone
 Nell'esempio seguente viene trovato il percorso più breve tra Jacob e Alice. Sono necessari il nodo Person e amica Edge creati dallo script di esempio Graph. 

```sql
SELECT PersonName, Friends
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

 ### <a name="b--find-shortest-path-from-a-given-node-to-all-other-nodes-in-the-graph"></a>B.  Trova il percorso più breve da un nodo specificato a tutti gli altri nodi del grafico. 
 Nell'esempio seguente vengono individuate tutte le persone a cui Giacobbe è connesso nel grafico e il percorso più breve che inizia da Jacob a tutti gli utenti. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
AND Person1.name = 'Jacob'
```

### <a name="c--count-the-number-of-hopslevels-traversed-to-go-from-one-person-to-another-in-the-graph"></a>C.  Contare il numero di hop/livelli attraversati per passare da una persona a un'altra nel grafico.
 Nell'esempio seguente viene individuato il percorso più breve tra Jacob e Alice e viene stampato il numero di hop necessari per passare da Jacob ad Alice. 

```sql
 SELECT PersonName, Friends, levels
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

### <a name="d-find-people-1-3-hops-away-from-a-given-person"></a>D. Trova persone 1-3 hop lontani da un determinato utente
Nell'esempio seguente viene individuato il percorso più breve tra Giacobbe e tutte le persone a cui è connesso nel grafico 1-3 hop a distanza. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
AND Person1.name = 'Jacob'
```

### <a name="e-find-people-exactly-2-hops-away-from-a-given-person"></a>E. Trovare persone esattamente 2 hop da una determinata persona
Nell'esempio seguente viene individuato il percorso più breve tra Giacobbe e le persone che sono esattamente 2 hop a distanza dal grafico. 

```sql
SELECT PersonName, Friends
FROM (
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
    AND Person1.name = 'Jacob'
) Q
WHERE Q.levels = 2
```

## <a name="see-also"></a>Vedere anche  
 [MATCH (grafo SQL)](../../t-sql/queries/match-sql-graph.md)    
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (grafo SQL)](../../t-sql/statements/insert-sql-graph.md)  
 [Graph Processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Elaborazione di grafi con SQL Server 2017)     
 
