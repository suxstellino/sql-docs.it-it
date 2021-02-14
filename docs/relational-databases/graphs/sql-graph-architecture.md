---
description: Architettura di SQL Graph
title: Architettura di SQL Graph | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, architecture
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab08692b1e221e589cc6283e2800a695044f79b7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351408"
---
# <a name="sql-graph-architecture"></a>Architettura di SQL Graph  
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

Informazioni sull'architettura di SQL Graph. Conoscere le nozioni di base renderà più semplice la comprensione di altri articoli di SQL Graph.
 
## <a name="sql-graph-database"></a>Database di SQL Graph
Gli utenti possono creare un grafico per ogni database. Un grafico è una raccolta di tabelle nodi e bordi. È possibile creare tabelle node o Edge in qualsiasi schema del database, ma tutte appartengono a un solo grafo logico. Una tabella Node è una raccolta di tipi di nodi simili. Una tabella dei nodi person, ad esempio, include tutti i nodi Person che appartengono a un grafo. Analogamente, una tabella Edge è una raccolta di un tipo di bordi simile. Ad esempio, una tabella Edge friends include tutti i bordi che connettono una persona a un'altra persona. Poiché i nodi e i bordi sono archiviati in tabelle, la maggior parte delle operazioni supportate nelle tabelle regolari sono supportate nelle tabelle node o Edge. 
 
 
![Diagramma che illustra l'architettura del database SQL Graph.](../../relational-databases/graphs/media/sql-graph-architecture.png "Architettura del database SQL Graph")   

Figura 1: architettura del database Graph di SQL
 
## <a name="node-table"></a>Tabella Node
Una tabella node rappresenta un'entità in uno schema Graph. Ogni volta che viene creata una tabella dei nodi, insieme alle colonne definite dall'utente, `$node_id` viene creata una colonna implicita, che identifica in modo univoco un determinato nodo nel database. I valori in `$node_id` vengono generati automaticamente e sono una combinazione di `object_id` tale tabella Node e un valore bigint generato internamente. Tuttavia, quando la `$node_id` colonna è selezionata, viene visualizzato un valore calcolato sotto forma di stringa JSON. Inoltre, `$node_id` è una pseudo-colonna che esegue il mapping a un nome interno con stringa esadecimale. Quando si seleziona `$node_id` dalla tabella, il nome della colonna verrà visualizzato come `$node_id_<hex_string>` . L'utilizzo di nomi di pseudo-colonna nelle query è il metodo consigliato per eseguire query sulla `$node_id` colonna interna e utilizzare il nome interno con la stringa esadecimale da evitare.

Si consiglia agli utenti di creare un vincolo o un indice univoco nella `$node_id` colonna al momento della creazione della tabella dei nodi, ma se non ne viene creato uno, viene creato automaticamente un indice univoco non cluster predefinito. Tuttavia, qualsiasi indice di una pseudo colonna Graph viene creato sulle colonne interne sottostanti. Ovvero, un indice creato sulla `$node_id` colonna verrà visualizzato nella `graph_id_<hex_string>` colonna interna.   


## <a name="edge-table"></a>Tabella Edge
Una tabella Edge rappresenta una relazione in un grafico. I bordi sono sempre diretti e connettono due nodi. Una tabella Edge consente agli utenti di modellare relazioni molti-a-molti nel grafico. Una tabella Edge può contenere o meno attributi definiti dall'utente. Ogni volta che viene creata una tabella Edge insieme agli attributi definiti dall'utente, nella tabella Edge vengono create tre colonne implicite:

|Nome colonna    |Descrizione  |
|---   |---  |
|`$edge_id`   |Identifica in modo univoco un determinato bordo nel database. Si tratta di una colonna generata e il valore è una combinazione di object_id della tabella Edge e di un valore bigint generato internamente. Tuttavia, quando la `$edge_id` colonna è selezionata, viene visualizzato un valore calcolato sotto forma di stringa JSON. `$edge_id` è una pseudo-colonna che esegue il mapping a un nome interno con stringa esadecimale. Quando si seleziona `$edge_id` dalla tabella, il nome della colonna verrà visualizzato come `$edge_id_\<hex_string>` . L'utilizzo di nomi di pseudo-colonna nelle query è il metodo consigliato per eseguire query sulla `$edge_id` colonna interna e utilizzare il nome interno con la stringa esadecimale da evitare. |
|`$from_id`   |Archivia l'oggetto `$node_id` del nodo, da cui ha origine il bordo.  |
|`$to_id`   |Archivia l'oggetto `$node_id` del nodo, in corrispondenza del quale termina il bordo. |

I nodi a cui un determinato bordo possono connettersi sono regolati dai dati inseriti nelle `$from_id` colonne e `$to_id` . Nella prima versione non è possibile definire vincoli sulla tabella Edge per limitare la connessione di due tipi di nodi. In altre termini, un Edge può connettere due nodi qualsiasi nel grafico, indipendentemente dai tipi.

Analogamente alla `$node_id` colonna, è consigliabile che gli utenti creino un indice o un vincolo univoco nella `$edge_id` colonna al momento della creazione della tabella Edge, ma se non ne viene creato uno, viene creato automaticamente un indice univoco non cluster predefinito in questa colonna. Tuttavia, qualsiasi indice di una pseudo colonna Graph viene creato sulle colonne interne sottostanti. Ovvero, un indice creato sulla `$edge_id` colonna verrà visualizzato nella `graph_id_<hex_string>` colonna interna. È inoltre consigliabile, per gli scenari OLTP, che gli utenti creino un indice `$from_id` in `$to_id` colonne (,) per le ricerche più veloci nella direzione del perimetro.  

Nella figura 2 viene illustrata la modalità di archiviazione delle tabelle dei nodi e dei bordi nel database. 

![Diagramma che mostra la rappresentazione della tabella dei nodi e dei bordi.](../../relational-databases/graphs/media/person-friends-tables.png "Tabelle Edge nodo persona e amici")   

Figura 2: rappresentazione della tabella Node e Edge



## <a name="metadata"></a>Metadati
Usare queste viste dei metadati per visualizzare gli attributi di una tabella nodi o bordi.
 
### <a name="systables"></a>sys.tables
Verranno aggiunte a SYS le colonne seguenti: nuovo tipo di bit. Tabelle. Se `is_node` è impostato su 1, indica che la tabella è una tabella nodi e se `is_edge` è impostato su 1, che indica che la tabella è una tabella Edge.
 
|Nome colonna |Tipo di dati |Descrizione |
|--- |---|--- |
|is_node |bit |1 = tabella Node |
|is_edge |bit |1 = tabella Edge |
 
### <a name="syscolumns"></a>sys.columns
La `sys.columns` vista contiene colonne aggiuntive `graph_type` e `graph_type_desc` , che indicano il tipo della colonna nelle tabelle node e Edge.
 
|Nome colonna |Tipo di dati |Descrizione |
|--- |---|--- |
|graph_type |INT |Colonna interna con un set di valori. I valori sono compresi tra 1-8 per le colonne Graph e NULL per altri.  |
|graph_type_desc |nvarchar(60)  |colonna interna con un set di valori |
 
Nella tabella seguente sono elencati i valori validi per la `graph_type` colonna

|Valore colonna  |Descrizione  |
|---   |---   |
|1  |GRAPH_ID  |
|2  |GRAPH_ID_COMPUTED  |
|3  |GRAPH_FROM_ID  |
|4  |GRAPH_FROM_OBJ_ID  |
|5  |GRAPH_FROM_ID_COMPUTED  |
|6  |GRAPH_TO_ID  |
|7  |GRAPH_TO_OBJ_ID  |
|8  |GRAPH_TO_ID_COMPUTED  |


`sys.columns` archivia inoltre le informazioni sulle colonne implicite create nelle tabelle node o Edge. Le informazioni seguenti possono essere recuperate da sys. Columns, tuttavia gli utenti non possono selezionare queste colonne da un nodo o da una tabella Edge. 

Colonne implicite in una tabella nodi

|Nome colonna    |Tipo di dati  |is_hidden  |Commento  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |`graph_id`colonna interna  |
|$node _id_\<hex_string> |NVARCHAR   |0  |Colonna nodo esterno `node_id`  |

Colonne implicite in una tabella Edge

|Nome colonna    |Tipo di dati  |is_hidden  |Commento  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |`graph_id`colonna interna  |
|$edge _id_\<hex_string> |NVARCHAR   |0  |`edge_id`colonna esterna  |
|from_obj_id_\<hex_string>  |INT    |1  |interno da nodo `object_id`  |
|from_id_\<hex_string>  |bigint |1  |Interno da nodo `graph_id`  |
|$from _id_\<hex_string> |NVARCHAR   |0  |esterno da nodo `node_id`  |
|to_obj_id_\<hex_string>    |INT    |1  |da interno a nodo `object_id`  |
|to_id_\<hex_string>    |bigint |1  |Da interno a nodo `graph_id`  |
|$to _id_\<hex_string>   |NVARCHAR   |0  |da esterno a nodo `node_id`  |
 
### <a name="system-functions"></a>Funzioni di sistema
Vengono aggiunte le funzioni predefinite seguenti. Che consente agli utenti di estrarre informazioni dalle colonne generate. Si noti che questi metodi non convalideranno l'input dell'utente. Se l'utente specifica un valore non valido `sys.node_id` , il metodo estrae la parte appropriata e la restituisce. Ad esempio, OBJECT_ID_FROM_NODE_ID accetta `$node_id` come input e restituirà l'object_id della tabella a cui appartiene questo nodo. 
 
|Predefinito   |Descrizione  |
|---  |---  |
|OBJECT_ID_FROM_NODE_ID |Estrae il object_id da un `node_id`  |
|GRAPH_ID_FROM_NODE_ID  |Estrae il graph_id da un `node_id`  |
|NODE_ID_FROM_PARTS |Costruire un node_id da un oggetto `object_id` e un oggetto `graph_id`  |
|OBJECT_ID_FROM_EDGE_ID |Estrai `object_id` da `edge_id`  |
|GRAPH_ID_FROM_EDGE_ID  |Estrai identità da `edge_id`  |
|EDGE_ID_FROM_PARTS |Costrutto `edge_id` da `object_id` e identità  |



## <a name="transact-sql-reference"></a>Guida di riferimento a Transact-SQL 
Informazioni sulle [!INCLUDE[tsql-md](../../includes/tsql-md.md)] estensioni introdotte in SQL Server e nel database SQL di Azure, che consentono di creare ed eseguire query sugli oggetti Graph. Le estensioni del linguaggio di query consentono di eseguire query e attraversare il grafo utilizzando la sintassi Art ASCII.
 
### <a name="data-definition-language-ddl-statements"></a>Istruzioni DDL (Data Definition Language)

|Attività   |Articolo correlato  |Note
|---  |---  |---  |
|CREA TABELLA |[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-sql-graph.md)|`CREATE TABLE` è ora esteso per supportare la creazione di una tabella come nodo o come EDGE. Si noti che una tabella Edge può contenere o meno attributi definiti dall'utente.  |
|MODIFICA TABELLA    |[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)|Le tabelle nodi e bordi possono essere modificate allo stesso modo di una tabella relazionale, usando `ALTER TABLE` . Gli utenti possono aggiungere o modificare colonne, indici o vincoli definiti dall'utente. Tuttavia, se si modificano le colonne del grafo interne, `$node_id` ad esempio o `$edge_id` , verrà generato un errore.  |
|CREATE INDEX   |[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  |Gli utenti possono creare indici su pseudo-colonne e colonne definite dall'utente nelle tabelle node e Edge. Sono supportati tutti i tipi di indice, inclusi gli indici columnstore cluster e non cluster.  |
|CREARE VINCOLI EDGE    |[VINCOLI EDGE &#40;&#41;Transact-SQL ](../../relational-databases/tables/graph-edge-constraints.md)  |Gli utenti possono ora creare vincoli Edge sulle tabelle Edge per applicare una semantica specifica e mantenere anche l'integrità dei dati  |
|DROP TABLE |[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)  |Le tabelle nodi e bordi possono essere eliminate allo stesso modo di una tabella relazionale, usando `DROP TABLE` . In questa versione, tuttavia, non sono previsti vincoli per garantire che nessun bordo punti a un nodo eliminato e l'eliminazione a catena di bordi, dopo l'eliminazione di una tabella nodi o nodi non è supportata. Se viene eliminata una tabella dei nodi, è consigliabile eliminare manualmente tutti i bordi collegati ai nodi della tabella nodo per mantenere l'integrità del grafo.  |


### <a name="data-manipulation-language-dml-statements"></a>Istruzioni DML (Data Manipulation Language)

|Attività   |Articolo correlato  |Note
|---  |---  |---  |
|INSERT |[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-sql-graph.md)|L'inserimento in una tabella nodi non è diverso dall'inserimento in una tabella relazionale. I valori per la `$node_id` colonna vengono generati automaticamente. Se si tenta di inserire un valore in una `$node_id` `$edge_id` colonna o, verrà generato un errore. Gli utenti devono fornire valori per `$from_id` le `$to_id` colonne e durante l'inserimento in una tabella Edge. `$from_id` e `$to_id` sono i `$node_id` valori dei nodi a cui un determinato bordo si connette.  |
|DELETE | [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|I dati delle tabelle nodi o bordi possono essere eliminati nello stesso modo in cui vengono eliminati dalle tabelle relazionali. In questa versione, tuttavia, non sono previsti vincoli per garantire che nessun bordo punti a un nodo eliminato e l'eliminazione a catena di bordi, quando l'eliminazione di un nodo non è supportata. Quando un nodo viene eliminato, vengono eliminati anche tutti i bordi di connessione al nodo, in modo da mantenere l'integrità del grafo.  |
|UPDATE |[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  |I valori nelle colonne definite dall'utente possono essere aggiornati utilizzando l'istruzione UPDATE. L'aggiornamento delle colonne grafiche interne,, `$node_id` `$edge_id` `$from_id` e `$to_id` non è consentito.  |
|MERGE |[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  |`MERGE` l'istruzione è supportata in una tabella nodi o bordi.  |


### <a name="query-statements"></a>Istruzioni di query

|Attività   |Articolo correlato  |Note
|---  |---  |---  |
|SELECT |[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)|I nodi e i bordi vengono archiviati come tabelle internamente, quindi la maggior parte delle operazioni supportate in una tabella SQL Server o nel database SQL di Azure sono supportate nelle tabelle nodi e bordi  |
|MATCH  | [CORRISPONDENZA &#40;&#41;Transact-SQL ](../../t-sql/queries/match-sql-graph.md)|Viene introdotta la corrispondenza predefinita per supportare i criteri di ricerca e l'attraversamento del grafo.  |



## <a name="limitations-and-known-issues"></a>Limitazioni e problemi noti  
In questa versione sono presenti alcune limitazioni sulle tabelle node e Edge:
* Le tabelle temporanee locali o globali non possono essere tabelle node o Edge.
* I tipi di tabella e le variabili di tabella non possono essere dichiarati come una tabella nodi o bordi. 
* Non è possibile creare tabelle nodi e bordi come tabelle temporali con controllo delle versioni di sistema.   
* Le tabelle nodi e bordi non possono essere tabelle con ottimizzazione per la memoria.  
* Gli utenti non possono aggiornare le `$from_id` `$to_id` colonne e di un bordo usando l'istruzione Update. Per aggiornare i nodi a cui si connette un bordo, gli utenti dovranno inserire il nuovo bordo che punta ai nuovi nodi ed eliminare quello precedente.
* Le query tra database su oggetti Graph non sono supportate. 


## <a name="next-steps"></a>Passaggi successivi
Per iniziare a usare la nuova sintassi, vedere [database di SQL Graph-esempio](./sql-graph-sample.md)
 

