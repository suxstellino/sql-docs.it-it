---
description: Vincoli di arco del grafo
title: Vincoli di arco del grafo | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- edge constraints [SQL Server]
- CONNECTION constraints
- edge constraints [Azure SQL Database]
- graph edge constraints
- SQL Graph
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 972e0a1cf391f321426398e94997740712580305
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346713"
---
# <a name="edge-constraints"></a>Vincoli di arco

[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

I vincoli di arco possono essere usati per imporre l'integrità dei dati e semantica specifica per le tabelle archi nel database a grafo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="edge-constraints"></a><a name="Connection"></a> Vincoli di arco

Nella prima versione delle funzionalità relative ai grafi, le tabelle archi non imponevano alcun vincolo per le estremità dell'arco. Questo significa che un arco in un database a grafo poteva connettere qualsiasi nodo a qualsiasi altro nodo, indipendentemente dal tipo.

In questa versione sono stati introdotti i vincoli di arco, che consentono agli utenti di aggiungere vincoli alle tabelle archi, per imporre una semantica specifica e mantenere l'integrità dei dati. Quando si aggiunge un nuovo arco a una tabella archi con vincoli di arco, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] impone che i nodi a cui tenta di connettersi l'arco esistano nelle tabelle nodi appropriate. Questi vincoli possono anche assicurare che un nodo non possa essere eliminato se un arco vi fa ancora riferimento.

### <a name="edge-constraint-clauses"></a>Clausole dei vincoli di arco

Ogni vincolo di arco è costituito da una o più clausole. Una clausola del vincolo di arco corrisponde alla coppia di nodi DA e A che l'arco specificato potrebbe connettere.

Si supponga che il grafico contenga i nodi `Product` e `Customer` e di usare l'arco `Bought` per la connessione di questi nodi. La clausola del vincolo di arco specifica la coppia di nodi DA e A e la direzione dell'arco. In questo caso la clausola del vincolo di arco sarà DA `Customer` A `Product`. Ovvero sarà consentito l'inserimento di un arco `Bought` che va da `Customer` a `Product`. I tentativi di inserire un arco che va da `Product` a `Customer` avranno esito negativo.

- Una clausola del vincolo di arco contiene una coppia di tabelle nodi DA e A a cui viene applicato il vincolo di arco.
- Gli utenti possono specificare più clausole per ogni vincolo di arco, che verranno applicate in forma disgiunta.
- Se vengono creati più vincoli di arco vengono per una singola tabella archi, gli archi devono soddisfare TUTTI i vincoli per essere consentiti.

### <a name="indexes-on-edge-constraints"></a>Indici sui vincoli di arco

La creazione di un vincolo di arco non determina automaticamente la creazione di un indice corrispondente sulle colonne `$from_id` e `$to_id` nella tabella archi. La creazione manuale di un indice per una coppia `$from_id`, `$to_id` è consigliata in presenza di query di ricerca di punti o carichi di lavoro OLTP.

### <a name="on-delete-referential-actions-on-edge-constraints"></a>Azioni referenziali ON DELETE sui vincoli di arco
Le azioni di propagazione su un vincolo di arco consentono agli utenti di definire la azioni eseguite dal motore di database quando un utente elimina il nodo o i nodi a cui si connette l'arco specificato. È possibile definire le azioni referenziali seguenti:  
*NO ACTION*   
Il motore di database genera un errore quando si tenta di eliminare un nodo con archi connessi.  

*CASCADE*   
Quando si elimina un nodo dal database, vengono eliminati gli archi connessi.  

## <a name="working-with-edge-constraints"></a>Utilizzo di vincoli di arco

È possibile definire un vincolo di arco in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../includes/tsql-md.md)]. È possibile definire un vincolo di arco solo in una tabella archi del grafo. Per creare, eliminare o modificare un vincolo di arco, è necessario avere l'autorizzazione **ALTER** per la tabella.

### <a name="create-edge-constraints"></a>Creare vincoli di arco

Gli esempi seguenti mostrano come creare vincoli di arco in tabelle nuove o esistenti

#### <a name="to-create-an-edge-constraint-on-a-new-edge-table"></a>Per creare un vincolo di arco in una nuova tabella archi

L'esempio seguente crea un vincolo di arco nella tabella archi **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE NO ACTION
   )
   AS EDGE;
   ```

#### <a name="defining-referential-actions-on-a-new-edge-table"></a>Definizione di azioni referenziali in una nuova tabella archi 

Nell'esempio seguente viene creato un vincolo di arco nella tabella archi **bought** e viene definita l'azione referenziale ON DELETE CASCADE. 

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE CASCADE
   )
   AS EDGE;
   ```

#### <a name="to-add-edge-constraint-to-an-existing-edge-table"></a>Per aggiungere un vincolo di arco a una tabella archi esistente

L'esempio seguente usa ALTER TABLE per aggiungere un vincolo di arco nella tabella archi **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY,
      , CustomerName VARCHAR(100)
   )
   AS NODE;
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product);
```  

#### <a name="create-a-new-edge-constraint-on-existing-edge-table-with-additional-edge-constraint-clauses"></a>Creare un nuovo vincolo di arco in una tabella archi esistente, con clausole aggiuntive per il vincolo di arco

L'esempio seguente usa il comando `ALTER TABLE` per aggiungere un nuovo vincolo di arco con clausole aggiuntive per il vincolo di arco nella tabella archi **bought**.
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
-- User ALTER TABLE to create a new edge constraint.
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product, Supplier TO Product);
```  

Nell'esempio precedente sono presenti due clausole per il vincolo di arco nel vincolo *EC_BOUGHT1*, una che connette **Customer** a **Product** e l'altra che connette **Supplier** a **Product**. Entrambe queste clausole vengono applicate in disgiunzione. Vale a dire, un determinato arco deve soddisfare una delle due clausole per essere consentito nella tabella archi.

#### <a name="creating-a-new-edge-constraint-on-existing-edge-table-with-new-edge-constraint-clause"></a>Creazione di un nuovo vincolo di arco in una tabella archi esistente, con una nuova clausola per il vincolo di arco

L'esempio seguente usa il comando `ALTER TABLE` per aggiungere un nuovo vincolo di arco con una nuova clausola per il vincolo di arco nella tabella archi **bought**.
  
 ```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT,
         CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Supplier TO Product);
 ```  

Nell'esempio precedente sono stati creati due vincoli di arco separati per la tabella archi **bought**: *EC_BOUGHT* e *EC_BOUGHT1*. Entrambi questi vincoli di arco hanno clausole diverse per il vincolo di arco. Se una tabella archi ha più di un vincolo di arco, un determinato arco deve soddisfare **TUTTI** i vincoli di arco per essere consentito nella tabella archi. Dato che nessun arco potrà soddisfare entrambi i vincoli *EC_BOUGHT* e *EC_BOUGHT1* in questo esempio, la tabella archi **bought** deve rimanere vuota.

Affinché gli inserimenti abbiano esito positivo in questa tabella archi, è necessario eliminare uno dei vincoli di arco oppure devono essere eliminati entrambi e occorre creare un nuovo vincolo di arco che include entrambe le clausole.

```sql
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT1;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT_NEW CONNECTION (Customer TO Product, Supplier TO Product);
GO
```  

Per essere consentito nella tabella archi **bought**, un determinato arco deve soddisfare una delle clausole nel vincolo *EC_BOUGHT_NEW*. Sarà quindi consentito qualsiasi arco che tenta di connettere un nodo **Customer** valido al nodo **Product** o un nodo **Supplier** al nodo **Product**.

### <a name="delete-edge-constraints"></a>Eliminare vincoli di arco

L'esempio seguente identifica prima di tutto il nome del vincolo di arco e quindi questo viene eliminato.  
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   ) AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
    )
    AS EDGE;
GO
-- Return the name of edge constraint.
SELECT name  
   FROM sys.edge_constraints  
   WHERE type = 'EC' AND parent_object_id = OBJECT_ID('bought');  
GO  
-- Delete the primary key constraint.  
ALTER TABLE bought
DROP CONSTRAINT EC_BOUGHT;
```  

### <a name="modify-edge-constraints"></a>Modificare vincoli di arco

Per modificare un vincolo di arco usando Transact-SQL, è prima necessario eliminare il vincolo di arco esistente e quindi ricrearlo con la nuova definizione.


### <a name="view-edge-constraints"></a>Visualizzare vincoli di arco

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

Nell'esempio vengono restituiti tutti i vincoli di arco e le relative proprietà per la tabella archi `bought` nel database tempdb.  

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
   GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;

-- CREATE edge table with edge constraints.
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product, Supplier TO Product)
   )
   AS EDGE;

-- Query sys.edge_constraints and sys.edge_constraint_clauses to view
-- edge constraint properties.
SELECT
   EC.name AS edge_constraint_name
   , OBJECT_NAME(EC.parent_object_id) AS edge_table_name
   , OBJECT_NAME(ECC.from_object_id) AS from_node_table_name
   , OBJECT_NAME(ECC.to_object_id) AS to_node_table_name
   , is_disabled
   , is_not_trusted
FROM sys.edge_constraints EC
   INNER JOIN sys.edge_constraint_clauses ECC
   ON EC.object_id = ECC.object_id
WHERE EC.parent_object_id = object_id('bought');
```  

## <a name="related-tasks"></a>Attività correlate

[CREATE TABLE (grafo SQL)](../../t-sql/statements/create-table-sql-graph.md)  
[ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  

Per informazioni sulla tecnologia dei grafi in SQL Server, vedere [Elaborazione di grafi con SQL Server e il database SQL di Azure](../graphs/sql-graph-overview.md).
