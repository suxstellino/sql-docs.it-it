---
description: MATCH (Transact-SQL)
title: MATCH (grafo SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MATCH
- MATCH_TSQL
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
- Shortest Path, shortest_path
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d55511e11750d29184124aa25b00a952eaf20ae7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270518"
---
# <a name="match-transact-sql"></a>MATCH (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

  Specifica una condizione di ricerca per un grafo. Il criterio MATCH può essere usato solo con il nodo del grafico e le tabelle bordi, nell'istruzione SELECT come parte della clausola WHERE. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
MATCH (<graph_search_pattern>)

<graph_search_pattern>::=
  {  
      <simple_match_pattern> 
    | <arbitrary_length_match_pattern>  
    | <arbitrary_length_match_last_node_predicate> 
  }

<simple_match_pattern>::=
  {
      LAST_NODE(<node_alias>) | <node_alias>   { 
          { <-( <edge_alias> )- } 
        | { -( <edge_alias> )-> }
        <node_alias> | LAST_NODE(<node_alias>)
        } 
  }
  [ { AND } { ( <simple_match_pattern> ) } ]
  [ ,...n ]

<node_alias> ::=
  node_table_name | node_table_alias 

<edge_alias> ::=
  edge_table_name | edge_table_alias


<arbitrary_length_match_pattern>  ::=
  { 
    SHORTEST_PATH( 
      <arbitrary_length_pattern> 
      [ { AND } { <arbitrary_length_pattern> } ] 
      [ ,…n] 
    )
  } 

<arbitrary_length_match_last_node_predicate> ::=
  {  LAST_NODE( <node_alias> ) = LAST_NODE( <node_alias> ) }


<arbitrary_length_pattern> ::=
    {  LAST_NODE( <node_alias> )   | <node_alias>
     ( <edge_first_al_pattern> [<edge_first_al_pattern>…,n] )
     <al_pattern_quantifier> 
  }
    |  ( {<node_first_al_pattern> [<node_first_al_pattern> …,n] )
        <al_pattern_quantifier> 
        LAST_NODE( <node_alias> ) | <node_alias> 
 }
    
<edge_first_al_pattern> ::=
  { (  
        { -( <edge_alias> )->   } 
      | { <-( <edge_alias> )- } 
      <node_alias>
      ) 
  } 

<node_first_al_pattern> ::=
  { ( 
      <node_alias> 
        { <-( <edge_alias> )- } 
      | { -( <edge_alias> )-> }
      ) 
  } 


<al_pattern_quantifier> ::=
  {
        +
      | { 1 , n }
  }

n -  positive integer only.
 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*graph_search_pattern*  
Specifica il criterio di ricerca o il percorso da attraversare nel grafico. Questo criterio usa la sintassi grafica ASCII per attraversare un percorso nel grafico. Il criterio passa da un nodo a un altro tramite un bordo, nella direzione della freccia indicata. I nomi o alias dei bordi vengono specificati all'interno di parentesi. I nomi o gli alias dei nodi vengono visualizzati alle due estremità della freccia. La freccia può andare in entrambe le direzioni nel modello.

*node_alias*  
Nome o alias di una tabella nodi specificata nella clausola FROM.

*edge_alias*  
Nome o alias di una tabella bordi specificata nella clausola FROM.

*SHORTEST_PATH*   
Questa funzione viene usata per individuare il percorso più breve tra due nodi specificati in un grafo o tra un determinato nodo e tutti gli altri nodi in un grafo. Accetta come input un criterio di lunghezza arbitraria, di cui viene eseguita ripetutamente la ricerca in un grafo. 

*arbitrary_length_match_pattern*  
Specifica i nodi e i bordi che devono essere attraversati ripetutamente finché non viene raggiunto il nodo desiderato o fino al numero massimo di iterazioni specificato nel criterio. 

*al_pattern_quantifier*   
Il criterio di lunghezza arbitraria accetta quantificatori di criteri nello stile delle espressioni regolari per specificare il numero di volte per cui viene ripetuto un criterio di ricerca specificato. I quantificatori dei criteri di ricerca supportati sono:   
* **+** : Ripetere il criterio una o più volte. Terminare non appena viene trovato un percorso più breve.    
* **{1,n}** : Ripetere il criterio da una a "n" volte. Terminare non appena viene trovato un percorso più breve.     

## <a name="remarks"></a>Osservazioni  
I nomi dei nodi all'interno del criterio MATCH possono essere ripetuti.  In altre parole, un nodo può essere attraversato un numero arbitrario di volte nella stessa query.  
Il nome di un bordo non può essere ripetuto all'interno del criterio MATCH.  
Un bordo può puntare in entrambe le direzioni, ma deve avere una direzione esplicita.  
Gli operatori OR e NOT non sono supportati nel criterio MATCH. Il criterio MATCH può essere combinato con altre espressioni tramite l'operatore AND nella clausola WHERE. Non può tuttavia essere combinato con altre espressioni tramite l'operatore OR o NOT. 

## <a name="examples"></a>Esempi  
### <a name="a--find-a-friend"></a>R.  Trovare un amico 
 Nell'esempio seguente viene creata una tabella nodi di persone e una tabella bordi di amici, vengono inseriti alcuni dati e quindi viene usato il criterio MATCH per trovare gli amici di Alice, una persona nel grafo.

 ```sql
 -- Create person node table
 CREATE TABLE dbo.Person (ID INTEGER PRIMARY KEY, name VARCHAR(50)) AS NODE;
 CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

 -- Insert into node table
 INSERT INTO dbo.Person VALUES (1, 'Alice');
 INSERT INTO dbo.Person VALUES (2,'John');
 INSERT INTO dbo.Person VALUES (3, 'Jacob');

-- Insert into edge table
INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'John'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2012');

-- use MATCH in SELECT to find friends of Alice
SELECT Person2.name AS FriendName
FROM Person Person1, friend, Person Person2
WHERE MATCH(Person1-(friend)->Person2)
AND Person1.name = 'Alice';
 ```

 ### <a name="b--find-friend-of-a-friend"></a>B.  Trovare un amico di un amico
 Nell'esempio seguente si tenta di trovare l'amico di un amico di Alice. 

 ```sql
SELECT Person3.name AS FriendName 
FROM Person Person1, friend, Person Person2, friend friend2, Person Person3
WHERE MATCH(Person1-(friend)->Person2-(friend2)->Person3)
AND Person1.name = 'Alice';
 ```

### <a name="c--more-match-patterns"></a>C.  Altri criteri `MATCH`
 Di seguito sono riportati alcuni altri modi in cui è possibile specificare un criterio all'interno di MATCH.

 ```sql
 -- Find a friend
    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2
    WHERE MATCH(Person1-(friend)->Person2);
    
-- The pattern can also be expressed as below

    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2 
    WHERE MATCH(Person2<-(friend)-Person1);


-- Find 2 people who are both friends with same person
    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0<-(friend2)-Person2);
    
-- this pattern can also be expressed as below

    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0 AND Person2-(friend2)->Person0);
 ```
 

## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (grafo SQL)](../../t-sql/statements/insert-sql-graph.md)  
 [Graph Processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Elaborazione di grafi con SQL Server 2017)  
