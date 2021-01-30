---
description: CREATE TABLE (grafo SQL)
title: CREATE TABLE (grafo SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_GRAPH_TSQL
- TABLE
- CREATE_TABLE_TSQL
- NODE
- NODE_TSQL
- AS_NODE
- AS_NODE_TSQL
- EDGE
- EDGE_TSQL
- AS_EDGE
- AS_EDGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- graph
- SQL graph
- CREATE TABLE SQL graph
- NODE
- EDGE
- SQL graph, CREATE TABLE statement
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d946a2c834fa910a0a7b76b321a55bfbdd91198
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177897"
---
# <a name="create-table-sql-graph"></a>CREATE TABLE (grafo SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Crea una nuova tabella di grafi SQL come tabella `NODE` o `EDGE`. 
  
> [!NOTE]   
>  Per le istruzioni Transact-SQL standard, vedere [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } 
       | <computed_column_definition>
       | <column_set_definition>
       | [ <table_constraint> ] [ ,... n ]
       | [ <table_index> ] }
          [ ,...n ]
    )   
    AS [ NODE | EDGE ]
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
[ ; ] 

< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CONNECTION
        ( { node_table TO node_table } 
          [ , {node_table TO node_table }]
          [ , ...n ]
        )
        [ ON DELETE { NO ACTION | CASCADE } ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
```  
  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
Questo documento include solo gli argomenti relativi al grafo SQL. Per un elenco completo e una descrizione degli argomenti supportati, vedere [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)

 *database_name*    
 Nome del database in cui è viene creata la tabella. *database_name* deve specificare il nome di un database esistente. Se l'argomento *database_name* non viene specificato, il valore predefinito è il database corrente. L'account di accesso per la connessione corrente deve essere associato a un ID utente esistente nel database specificato da *database_name*. Questo ID utente deve avere le autorizzazioni CREATE TABLE.  
  
 *schema_name*    
 Nome dello schema a cui appartiene la nuova tabella.  
  
 *table_name*      
 Nome della tabella nodi o bordi. I nomi delle tabelle devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). *table_name* può essere costituito al massimo da 128 caratteri, ad eccezione dei nomi di tabelle temporanee locali, ovvero i nomi preceduti da un solo simbolo di cancelletto (#), che non possono superare i 116 caratteri.  
  
 NODE   
 Crea una tabella nodi.

 EDGE  
 Crea una tabella bordi.  
 
 *table_constraint*   
 Specifica le proprietà di un vincolo PRIMARY KEY, UNIQUE, FOREIGN KEY, CONNECTION o CHECK oppure di una definizione DEFAULT aggiunta a una tabella.
 
 > [!NOTE]   
 > Il vincolo CONNECTION si applica solo a un tipo di tabella bordi.
 
 ON { partition_scheme | filegroup | "default" }    
 Specifica lo schema di partizione o il filegroup in cui la tabella viene archiviata. Se si specifica partition_scheme, la tabella deve essere partizionata e le relative partizioni devono essere archiviate in un set di uno o più filegroup specificati in partition_scheme. Se si specifica filegroup, la tabella viene archiviata nel filegroup indicato. Il filegroup deve essere presente nel database. Se si specifica "default" oppure si omette ON, la tabella viene archiviata nel filegroup predefinito. Il meccanismo di archiviazione di una tabella specificato in CREATE TABLE non può essere modificato in seguito.

 ON {partition_scheme | filegroup | "default"}    
 Può essere specificato anche in un vincolo PRIMARY KEY o UNIQUE. Questi vincoli comportano la creazione di indici. Se si specifica filegroup, l'indice viene archiviato nel filegroup indicato. Se si specifica "default" oppure si omette ON, l'indice viene archiviato nello stesso filegroup della tabella. Se il vincolo PRIMARY KEY o UNIQUE crea un indice cluster, le pagine di dati della tabella vengono archiviate nello stesso filegroup dell'indice. Se si specifica CLUSTERED o se il vincolo crea in altro modo un indice cluster e si specifica un valore partition_scheme diverso dal valore partition_scheme o filegroup della definizione della tabella, o viceversa, verrà rispettata solo la definizione del vincolo e gli altri valori verranno ignorati.
  
## <a name="remarks"></a>Osservazioni

La creazione di una tabella temporanea come tabella nodi o bordi non è supportata.  

La creazione di una tabella nodi o bordi come tabella temporanea non è supportata.

Stretch Database non è supportato per la tabella nodi o bordi.

Le tabelle nodi o bordi non possono essere tabelle esterne. PolyBase non supporta le tabelle di grafi. 

Non è possibile modificare una tabella nodi/archi di grafo non partizionata in una tabella nodi/archi di grafo partizionata. 
  
 
## <a name="examples"></a>Esempi  
  
### <a name="a-create-a-node-table"></a>R. Creare una tabella `NODE`
 L'esempio seguente illustra come creare una tabella `NODE`

```sql
 CREATE TABLE Person (
        ID INTEGER PRIMARY KEY, 
        name VARCHAR(100), 
        email VARCHAR(100)
 ) AS NODE;
```

### <a name="b-create-an-edge-table"></a>B. Creare una tabella `EDGE`
L'esempio seguente illustra come creare tabelle `EDGE`

```sql
 CREATE TABLE friends (
    id INTEGER PRIMARY KEY,
    start_date DATe
 ) AS EDGE;
```

```sql
 -- Create a likes edge table, this table does not have any user defined attributes   
 CREATE TABLE likes AS EDGE;
```

Nell'esempio successivo viene modellata una regola secondo cui **solo** le persone possono essere amiche di altre persone, pertanto edge non consente riferimenti a nodi diversi da Person.

```
/* Create friend edge table with CONSTRAINT, restricts for nodes and it direction */
CREATE TABLE dbo.FriendOf(
  CONSTRAINT cnt_Person_FriendOf_Person
    CONNECTION (dbo.Person TO dbo.Person) 
)AS EDGE;
```


## <a name="see-also"></a>Vedere anche 
 [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [INSERT (grafo SQL)](../../t-sql/statements/insert-sql-graph.md)  
 [Graph Processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Elaborazione di grafi con SQL Server 2017)

