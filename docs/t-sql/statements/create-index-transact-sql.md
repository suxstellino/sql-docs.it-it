---
description: CREATE INDEX (Transact-SQL)
title: CREATE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE INDEX
- INDEX
- INDEX_TSQL
- CREATE_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- PRIMARY XML INDEX statement
- indexes [SQL Server], creating
- online index operations
- index keys [SQL Server]
- PROPERTY INDEX statement
- computed columns, index creation
- clustered indexes, creating
- indexed views [SQL Server], index creation
- partitioned indexes [SQL Server], creating
- VALUE INDEX statement
- index creation [SQL Server], CREATE INDEX statement
- DROP_EXISTING clause
- row locks [SQL Server]
- composite indexes
- MAXDOP index option, CREATE INDEX statement
- filtered indexes [SQL Server], creating
- PATH INDEX statement
- locking [SQL Server], indexes
- unique indexes, creating
- primary indexes [SQL Server]
- CREATE PRIMARY XML INDEX statement
- SET statement, index creation
- index options [SQL Server]
- included columns
- ONLINE option
- nonclustered indexes [SQL Server], creating
- CREATE INDEX statement
- IGNORE_DUP_KEY option
- deterministic functions
- keys [SQL Server], index
- SECONDARY XML INDEX statement
- page locks [SQL Server]
- secondary indexes [SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: d2297805-412b-47b5-aeeb-53388349a5b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d5197659c65c7e984577bc29154350492d2c7aa0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344059"
---
# <a name="create-index-transact-sql"></a>CREATE INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crea un indice relazionale per una tabella o una vista. Viene detto anche indice rowstore, perché è un indice ad albero b-tree sia cluster che non cluster. È possibile creare un indice rowstore prima che siano presenti dati nella tabella. Usare un indice rowstore per migliorare le prestazioni delle query, in particolare quando le query effettuano le selezioni da colonne specifiche o richiedono valori da organizzare in base a un ordine particolare.

> [!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] attualmente non supportano i vincoli UNIQUE. Eventuali esempi che fanno riferimento ai vincoli UNIQUE sono applicabili solo a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> [!TIP]
> Per informazioni sulle linee guida di progettazione degli indici, vedere [Guida per la progettazione di indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

 **Esempi semplici:**

```sql
-- Create a nonclustered index on a table or view
CREATE INDEX i1 ON t1 (col1);

-- Create a clustered index on a table and use a 3-part name for the table
CREATE CLUSTERED INDEX i1 ON d1.s1.t1 (col1);

-- Syntax for SQL Server and Azure SQL Database
-- Create a nonclustered index with a unique constraint
-- on 3 columns and specify the sort order for each column
CREATE UNIQUE INDEX i1 ON t1 (col1 DESC, col2 ASC, col3 DESC);
```

**Scenario chiave:**

A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)], usare un indice non cluster per un indice columnstore per migliorare le prestazioni delle query di data warehousing. Per altre informazioni, vedere [Indici columnstore - Data warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

Per altri tipi di indici, vedere:

- [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)
- [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)
- [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md)

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

### <a name="syntax-for-sql-server-and-azure-sql-database"></a>Sintassi per SQL Server e per il database SQL di Azure

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column [ ASC | DESC ] [ ,...n ] )
    [ INCLUDE ( column_name [ ,...n ] ) ]
    [ WHERE <filter_predicate> ]
    [ WITH ( <relational_index_option> [ ,...n ] ) ]
    [ ON { partition_scheme_name ( column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
[ ; ]
  
<object> ::=
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }

<relational_index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB = { ON | OFF }
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | DROP_EXISTING = { ON | OFF }
  | ONLINE = { ON | OFF }
  | RESUMABLE = { ON | OFF }
  | MAX_DURATION = <time> [MINUTES]
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | MAXDOP = max_degree_of_parallelism
  | DATA_COMPRESSION = { NONE | ROW | PAGE }
     [ ON PARTITIONS ( { <partition_number_expression> | <range> }
     [ , ...n ] ) ]
}

<filter_predicate> ::=
    <conjunct> [ AND <conjunct> ]

<conjunct> ::=
    <disjunct> | <comparison>

<disjunct> ::=
        column_name IN (constant ,...n)

<comparison> ::=
        column_name <comparison_op> constant

<comparison_op> ::=
    { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< }

<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

### <a name="backward-compatible-relational-index"></a>Indice relazionale compatibile con le versioni precedenti

> [!IMPORTANT]
> La struttura della sintassi dell'indice relazionale compatibile con le versioni precedenti verrà rimossa in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
> Evitare di utilizzare questa struttura della sintassi e pianificare la modifica delle applicazioni che ne fanno uso.
> Usare invece la struttura della sintassi specificata in <relational_index_option>.

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column_name [ ASC | DESC ] [ ,...n ] )
    [ WITH <backward_compatible_index_option> [ ,...n ] ]
    [ ON { filegroup_name | "default" } ]

<object> ::=
{
    [ database_name. [ owner_name ] . | owner_name. ]
    table_or_view_name
}

<backward_compatible_index_option> ::=
{
    PAD_INDEX
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB
  | IGNORE_DUP_KEY
  | STATISTICS_NORECOMPUTE
  | DROP_EXISTING
}
```

### <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Sintassi per Azure Synapse Analytics e Parallel Data Warehouse

```syntaxsql

CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
    [ORDER (column[,...n])]
    [WITH ( DROP_EXISTING = { ON | OFF } )]
[;]


CREATE [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
        ( { column [ ASC | DESC ] } [ ,...n ] )
    WITH ( DROP_EXISTING = { ON | OFF } )
[;]


```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

UNIQUE      
Crea un indice univoco per una tabella o una vista. Un indice univoco non consente l'utilizzo di uno stesso valore di chiave di indice per più righe. L'indice cluster di una vista deve essere univoco.

In [!INCLUDE[ssDE](../../includes/ssde-md.md)] non è possibile creare un indice univoco su colonne che includono già valori duplicati, indipendentemente dal fatto che l'opzione IGNORE_DUP_KEY sia impostata o meno su ON. Se si tenta di eseguire questa operazione, [!INCLUDE[ssDE](../../includes/ssde-md.md)] visualizza un messaggio di errore. Prima di poter creare un indice univoco su una o più colonne di questo tipo, è necessario rimuovere i valori duplicati. Le colonne utilizzate in un indice univoco devono essere impostate su NOT NULL, perché più valori Null vengono considerati duplicati in fase di creazione dell'indice.

CLUSTERED      
Crea un indice in cui l'ordine logico dei valori di chiave determina l'ordine fisico delle righe corrispondenti di una tabella. Nel livello inferiore, o foglia, dell'indice cluster sono contenute le righe di dati effettive della tabella. È possibile creare un solo indice cluster alla volta per una tabella o una vista.

Una vista con un indice cluster univoco viene definita vista indicizzata. La creazione di un indice cluster univoco per una vista materializza fisicamente la vista. È necessario creare un indice cluster univoco per una vista prima di poter definire altri indici per la stessa vista. Per altre informazioni, vedere [Creare viste indicizzate](../../relational-databases/views/create-indexed-views.md).

Creare l'indice cluster prima di qualsiasi indice non cluster. Quando si crea un indice cluster, gli indici non cluster esistenti delle tabelle vengono ricompilati.

Se `CLUSTERED` viene omesso, viene creato un indice non cluster.

> [!NOTE]
> Poiché per definizione il livello foglia di un indice cluster corrisponde alle pagine di dati, la creazione di un indice cluster con la clausola ON *partition_scheme_name* o *filegroup_name* comporta lo spostamento di una tabella dal filegroup in cui è stata creata nel nuovo schema di partizione o filegroup. Prima di creare tabelle o indici in filegroup specifici, verificare i filegroup disponibili e controllare che dispongano di spazio sufficiente per l'indice.

In alcuni casi la creazione di un indice cluster può abilitare gli indici precedentemente disabilitati. Per altre informazioni, vedere [Abilitare indici e vincoli](../../relational-databases/indexes/enable-indexes-and-constraints.md) e [Disabilitare indici e vincoli](../../relational-databases/indexes/disable-indexes-and-constraints.md).

NONCLUSTERED      
Crea un indice che specifica l'ordinamento logico di una tabella. Quando si utilizza un indice non cluster, l'ordine fisico delle righe di dati è indipendente dall'ordine delle righe indicizzato.

Per ogni tabella è possibile definire al massimo 999 indici non cluster, indipendentemente dal fatto che vengano creati in modo implicito tramite vincoli PRIMARY KEY e UNIQUE oppure in modo esplicito tramite CREATE INDEX.

Per le viste indicizzate, gli indici non cluster possono essere creati solo se sulla vista è già stato definito un indice cluster univoco.

Se non diversamente specificato, il tipo di indice predefinito è NONCLUSTERED.

*index_name*      
 Nome dell'indice. I nomi di indice devono essere univoci all'interno di una tabella o di una vista, ma non all'interno di un database. Devono essere anche conformi alle regole degli [identificatori](../../relational-databases/databases/database-identifiers.md).

*column*      
 Una o più colonne su cui è basato l'indice. Specificare due o più nomi di colonna per creare un indice composto sui valori combinati delle colonne specificate. Elencare le colonne da includere nell'indice composto, in base alla priorità di ordinamento, tra parentesi dopo *table_or_view_name*.

In una singola chiave di indice composto è possibile combinare al massimo 32 colonne. Tutte le colonne di una chiave di indice composto devono appartenere alla stessa tabella o vista. La dimensione massima consentita dei valori combinati dell'indice è 900 byte per un indice cluster o 1700 per un indice non cluster. I limiti sono 16 colonne e 900 byte per le versioni precedenti a [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].

Le colonne con tipo di dati LOB (Large Object) **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** o **image** non possono essere specificate come colonne chiave di un indice. Inoltre, una definizione di vista non può includere colonne di tipo **ntext**, **text** o **image** anche se non vi viene fatto riferimento nell'istruzione CREATE INDEX.

È possibile creare indici su colonne di tipo CLR definito dall'utente se il tipo supporta l'ordinamento binario. È inoltre possibile creare indici su colonne calcolate definite come chiamate di metodo da una colonna con tipo definito dall'utente, a condizione che i metodi siano contrassegnati come deterministici e non eseguano operazioni di accesso ai dati. Per altre informazioni sull'indicizzazione di colonne di tipo CLR definito dall'utente, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

[ **ASC** | DESC ]      
Determina se il tipo di ordinamento della colonna di indice specificata è crescente o decrescente. Il valore predefinito è ASC.

INCLUDE **(** _column_ [ **,** ... *n* ] **)**       
Specifica le colonne non chiave da aggiungere al livello foglia dell'indice non cluster. L'indice non cluster può essere univoco o non univoco.

I nomi di colonna non possono essere ripetuti nell'elenco INCLUDE e non possono essere utilizzati contemporaneamente per colonne chiave e non chiave. Gli indici non cluster contengono sempre le colonne dell'indice cluster se nella tabella è definito un indice cluster. Per altre informazioni, vedere [Creare indici con colonne incluse](../../relational-databases/indexes/create-indexes-with-included-columns.md).

È possibile usare qualsiasi tipo di dati, ad eccezione di **text**, **ntext** e **image**. L'indice dovrà essere creato o ricompilato offline (ONLINE = OFF) se una delle colonne non chiave specificate è di tipo **varchar(max)** , **nvarchar(max)** o **varbinary(max)** .

Come colonne incluse è possibile utilizzare colonne calcolate che sono deterministiche, sia precise che imprecise. Le colonne calcolate derivate dai tipi di dati **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** e **xml** possono essere incluse in colonne non chiave purché il tipo di dati della colonna calcolata sia consentito come colonna inclusa. Per altre informazioni, vedere [Indici per le colonne calcolate](../../relational-databases/indexes/indexes-on-computed-columns.md).

Per informazioni sulla creazione di un indice XML, vedere [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md).

WHERE \<filter_predicate>      
Crea un indice filtrato specificando le righe da includere nell'indice. L'indice filtrato deve essere un indice non cluster in una tabella. Crea statistiche filtrate per le righe di dati dell'indice filtrato.

Il predicato del filtro utilizza una logica di confronto semplice e non può fare riferimento a una colonna calcolata, a una colonna con tipo definito dall'utente (UDT), a una colonna con tipo di dati spaziale o a una colonna con tipo di dati hierarchyID. I confronti in cui vengono utilizzati valori letterali NULL non sono consentiti con gli operatori di confronto. In alternativa, utilizzare gli operatori IS NULL e IS NOT NULL.

Di seguito sono riportati alcuni esempi di predicati di filtro per la tabella `Production.BillOfMaterials`:

```sql
WHERE StartDate > '20000101' AND EndDate <= '20000630'

WHERE ComponentID IN (533, 324, 753)

WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL
```

Gli indici filtrati non si applicano agli indici XML e full-text. Per gli indici UNIQUE, solo le righe selezionate devono avere valori di indice univoci. Gli indici filtrati non consentono l'opzione IGNORE_DUP_KEY.

ON *partition_scheme_name* **( _column_name_ )**      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica lo schema di partizione che definisce i filegroup a cui verrà eseguito il mapping delle partizioni di un indice partizionato. È necessario che lo schema di partizione sia presente nel database e sia stato creato eseguendo [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* specifica la colonna in base alla quale verrà eseguita la partizione di un indice partizionato. La colonna deve corrispondere all'argomento della funzione di partizione usata da *partition_scheme_name* per tipo di dati, lunghezza e precisione. *column_name* non è limitato alle colonne nella definizione dell'indice. È possibile specificare qualsiasi colonna della tabella di base tranne quando si esegue la partizione di un indice UNIQUE. In questo caso il valore *column_name* deve essere scelto tra quelli usati come chiave univoca. Questa restrizione consente a [!INCLUDE[ssDE](../../includes/ssde-md.md)] di verificare l'univocità dei valori di chiave all'interno di una singola partizione.

> [!NOTE]
> Quando si partiziona un indice cluster non univoco, per impostazione predefinita [!INCLUDE[ssDE](../../includes/ssde-md.md)] aggiunge la colonna di partizionamento all'elenco delle chiavi di indice cluster, se non è già presente. Quando si partiziona un indice non cluster non univoco, [!INCLUDE[ssDE](../../includes/ssde-md.md)] aggiunge la colonna di partizionamento come colonna non chiave (inclusa) dell'indice, se non è già presente.

Se non si specifica _partition_scheme_name_ o _filegroup_ e la tabella è partizionata, l'indice viene posizionato nello stesso schema di partizione, usando la stessa colonna di partizionamento della tabella sottostante.

> [!NOTE]
> Non è possibile specificare uno schema di partizione per un indice XML. Se la tabella di base è partizionata, l'indice XML utilizzerà lo stesso schema di partizione della tabella.

Per altre informazioni sul partizionamento degli indici, vedere [Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON _filegroup_name_      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive)

Crea l'indice specificato nel filegroup specificato. Se non viene specificata una posizione e la tabella o la vista non è partizionata, l'indice utilizzerà lo stesso filegroup della tabella o della vista sottostante. Il filegroup deve essere già esistente.

ON **"** default **"**      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Crea l'indice specificato nello stesso filegroup o schema di partizione della tabella o della vista.

In questo contesto il termine default non rappresenta una parola chiave, È un identificatore per il filegroup predefinito e pertanto deve essere delimitato, ad esempio ON **"** default **"** o ON **[** default **]** . Se si specifica "default", l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

> [!NOTE]
> "default" non indica il filegroup predefinito del database nel contesto di CREATE INDEX, a differenza di CREATE TABLE dove "default" individua la tabella nel filegroup predefinito del database.

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ]      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive)

Specifica la posizione dei dati FILESTREAM per la tabella quando viene creato un indice cluster. La clausola FILESTREAM_ON consente di spostare i dati FILESTREAM in uno schema di partizione o in un filegroup FILESTREAM diverso.

_filestream_filegroup_name_ è il nome di un filegroup FILESTREAM. Nel filegroup deve essere disponibile un file definito usando un'istruzione [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) o [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). In caso contrario, viene generato un errore.

Se la tabella è partizionata, la clausola FILESTREAM_ON deve essere inclusa e deve specificare uno schema di partizione dei filegroup FILESTREAM che utilizzi la stessa funzione di partizione e le stesse colonne di partizione dello schema di partizione per la tabella. In caso contrario, viene generato un errore.

Se la tabella non è partizionata, la colonna FILESTREAM non può essere partizionata. I dati FILESTREAM per la tabella devono essere archiviati in un singolo filegroup specificato nella clausola FILESTREAM_ON.

È possibile specificare `FILESTREAM_ON NULL` in un'istruzione CREATE INDEX se si sta creando un indice cluster e se nella tabella non è presente nessuna colonna FILESTREAM.

Per altre informazioni, vedere [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).

**\<object>::=**      

Oggetto con nome completo o non completo che si desidera indicizzare.

_database_name_      
Nome del database.

*schema_name*      
Nome dello schema a cui appartiene la tabella o la vista.

*table_or_view_name*      
Nome della tabella o della vista che si desidera indicizzare.

Per poter creare un indice per una vista, è necessario che la vista sia definita con l'opzione SCHEMABINDING. Prima di creare qualsiasi indice non cluster per una vista, è necessario creare un indice cluster univoco. Per ulteriori informazioni sulle viste indicizzate, vedere la sezione Osservazioni.

A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] l'oggetto può essere una tabella archiviata con un indice columnstore cluster.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] supporta il formato del nome in tre parti _database_name_.[_schema_name_]._object_name_ quando *database_name* è il database corrente o _database_name_ è `tempdb`e _object_name_ inizia con #.

**\<relational_index_option\>::=**       
Specifica le opzioni da usare quando si crea l'indice.

PAD_INDEX = { ON | **OFF** }      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica il riempimento dell'indice. Il valore predefinito è OFF.

ON      
La percentuale di spazio disponibile specificata da *fillfactor* viene applicata alle pagine di livello intermedio dell'indice.

OFF o _fillfactor_ non è specificato      
Le pagine di livello intermedio vengono riempite poco al di sotto della capacità massima, in modo che lo spazio residuo sia sufficiente per almeno una riga della dimensione massima supportata dall'indice, in base al set di chiavi nelle pagine intermedie.

L'opzione PAD_INDEX risulta utile solo quando si specifica FILLFACTOR, in quanto PAD_INDEX usano la percentuale specificata in FILLFACTOR. Se la percentuale specificata in FILLFACTOR non consente l'inserimento di una riga, [!INCLUDE[ssDE](../../includes/ssde-md.md)] sostituisce internamente tale percentuale in modo da rendere disponibile lo spazio minimo necessario. Il numero di righe di una pagina intermedia dell'indice non è mai minore di due, indipendentemente dal valore di _fillfactor_.

Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH PAD_INDEX equivale a WITH PAD_INDEX = ON.

FILLFACTOR **=** _fillfactor_      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica una percentuale che indica il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la ricompilazione dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Se *fillfactor* è 100, [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea indici con pagine foglia riempite fino alla capacità massima.

L'impostazione di FILLFACTOR viene applicata solo in fase di creazione o di ricompilazione dell'indice. La percentuale specificata di spazio vuoto delle pagine non viene mantenuta in modo dinamico da [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per visualizzare l'impostazione del fattore di riempimento, usare la vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

> [!IMPORTANT]
> La creazione di un indice cluster con un valore FILLFACTOR minore di 100 influisce sulla quantità di spazio di archiviazione occupata dai dati perché i dati vengono ridistribuiti da [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione dell'indice cluster.

Per altre informazioni, vedere [Specificare un fattore di riempimento per un indice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).

SORT_IN_TEMPDB = { ON | **OFF** }      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica se i risultati temporanei dell'ordinamento devono essere archiviati in **tempdb**. Il valore predefinito è OFF, tranne per il database SQL di Azure con servizio Hyperscale. Per tutte le operazioni di compilazione dell'indice in Hyperscale, SORT_IN_TEMPDB è sempre ON, indipendentemente dall'opzione specificata, a meno che non venga usata la ricompilazione dell'indice ripristinabile.

ON      
I risultati intermedi dell'ordinamento usati per la compilazione dell'indice vengono archiviati in **tempdb**. In questo modo si può ridurre il tempo necessario per creare un indice se **tempdb** si trova in un set di dischi diverso rispetto al database utente. La quantità di spazio su disco utilizzata durante la compilazione dell'indice sarà tuttavia maggiore.

OFF      
I risultati intermedi dell'ordinamento vengono archiviati nello stesso database dell'indice.

Oltre allo spazio necessario nel database utente per la creazione dell'indice, in **tempdb** deve essere disponibile una quantità di spazio aggiuntivo pressoché equivalente per l'archiviazione dei risultati intermedi dell'ordinamento. Per altre informazioni, vedere [Opzione SORT_IN_TEMPDB per gli indici](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).

Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH SORT_IN_TEMPDB equivale a WITH SORT_IN_TEMPDB = ON.

IGNORE_DUP_KEY = { ON | **OFF** }      
Specifica l'errore restituito quando un'operazione di inserimento tenta di inserire valori di chiave duplicati in un indice univoco. L'opzione IGNORE_DUP_KEY viene applicata solo alle operazioni di inserimento eseguite dopo la creazione o la ricompilazione dell'indice. L'opzione non ha alcun effetto se si esegue [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md). Il valore predefinito è OFF.

ON      
Viene visualizzato un messaggio di avviso quando i valori di chiave duplicati vengono inseriti in un indice univoco. Avranno esito negativo solo le righe che violano il vincolo di unicità.

OFF      
Viene visualizzato un messaggio di errore quando i valori di chiave duplicati vengono inseriti in un indice univoco. Viene eseguito il rollback dell'intera operazione INSERT.

L'opzione IGNORE_DUP_KEY non può essere impostata su ON per gli indici creati in una vista, negli indici non univoci, negli indici XML, spaziali e filtrati.

Per visualizzare IGNORE_DUP_KEY, usare [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH IGNORE_DUP_KEY equivale a WITH IGNORE_DUP_KEY = ON.

STATISTICS_NORECOMPUTE = { ON | **OFF**}      
Specifica se le statistiche di distribuzione vengono ricalcolate. Il valore predefinito è OFF.

ON      
Le statistiche non aggiornate non vengono ricalcolate automaticamente.

OFF      
Abilita l'aggiornamento automatico delle statistiche.

Per ripristinare l'aggiornamento automatico delle statistiche, impostare l'opzione STATISTICS_NORECOMPUTE su OFF oppure eseguire UPDATE STATISTICS senza la clausola NORECOMPUTE.

> [!IMPORTANT]
> La disabilitazione del ricalcolo automatico delle statistiche di distribuzione può compromettere la selezione di piani di esecuzione ottimali per le query riguardanti la tabella in Query Optimizer.

Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH STATISTICS_NORECOMPUTE equivale a WITH STATISTICS_NORECOMPUTE = ON.

STATISTICS_INCREMENTAL = { ON | **OFF** }     

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Se è specificato **ON**, le statistiche create sono statistiche per partizione. Se è specificato **OFF**, l'albero delle statistiche viene eliminato e le statistiche vengono ricalcolate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il valore predefinito è **OFF**.

Se le statistiche per partizione non sono supportate, l'opzione viene ignorata e viene generato un avviso. Le statistiche incrementali non sono supportate per i seguenti tipi di statistiche:

- Statistiche create con indici che non hanno il partizionamento allineato con la tabella di base.
- Statistiche create per i database secondari leggibili Always On.
- Statistiche create per i database di sola lettura.
- Statistiche create per gli indici filtrati.
- Statistiche create per le viste.
- Statistiche create per le tabelle interne.
- Statistiche create con indici spaziali o indici XML.

DROP_EXISTING = { ON | **OFF** }      
Consente di eliminare e ricompilare l'indice cluster o non cluster esistente con le specifiche colonne modificate e di mantenere lo stesso nome per l'indice. Il valore predefinito è OFF.

ON      
Specifica che l'indice esistente deve essere eliminato e ricompilato e che deve avere lo stesso nome del parametro *index_name*.

OFF      
Specifica che l'indice esistente non deve essere eliminato e ricompilato. Se il nome di indice specificato esiste già, SQL Server visualizza un messaggio di errore.

Con DROP_EXISTING è possibile modificare:

- Un indice rowstore non cluster in un indice rowstore cluster.

Con DROP_EXISTING non è possibile modificare:

- Un indice rowstore cluster in un indice rowstore non cluster.
- Un indice columnstore cluster in qualsiasi tipo di indice rowstore.

Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH DROP_EXISTING equivale a WITH DROP_EXISTING = ON.

ONLINE = { ON | **OFF** }      
Specifica se le tabelle sottostanti e gli indici associati sono disponibili per le query e la modifica dei dati durante l'operazione sugli indici. Il valore predefinito è OFF.

> [!IMPORTANT]
> Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).

ON      
I blocchi di tabella a lungo termine non vengono mantenuti per la durata dell'operazione sugli indici. Durante la fase principale dell'operazione viene mantenuto solo un blocco preventivo condiviso (IS, Intent Shared) sulla tabella di origine. in modo da consentire l'esecuzione di query o l'aggiornamento della tabella sottostante e degli indici. All'inizio dell'operazione viene mantenuto un blocco condiviso (S) sull'oggetto di origine per un periodo molto breve. Al termine dell'operazione, per un breve periodo viene acquisito un blocco condiviso (S) sull'origine, in caso di creazione di un indice non cluster. In caso di creazione o di eliminazione di un indice cluster online o di ricompilazione di un indice cluster o non cluster, viene acquisito un blocco di modifica dello schema (SCH-M). L'opzione ONLINE non può essere impostata su ON quando viene creato un indice per una tabella temporanea locale.

OFF      
I blocchi di tabella vengono applicati per la durata dell'operazione sugli indici. Un'operazione sugli indici offline che crea, ricompila o elimina un indice cluster oppure ricompila o elimina un indice non cluster acquisisce un blocco di modifica dello schema (SCH-M) sulla tabella. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione. Un'operazione sugli indici offline che crea un indice non cluster acquisisce un blocco condiviso (S) sulla tabella. Tale blocco impedisce l'aggiornamento della tabella sottostante ma consente operazioni di lettura, ad esempio l'esecuzione di istruzioni SELECT.

Per altre informazioni, vedere [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  

È possibile creare online tutti gli indici, inclusi quelli di tabelle temporanee globali, ad eccezione dei casi seguenti:

- Indice XML
- Indice di una tabella temporanea locale
- Indice cluster univoco iniziale su una vista
- Indici cluster disabilitati
- Indici columnstore
- Indice cluster, se la tabella sottostante contiene tipi di dati LOB (**image**, **ntext**, **text**) e tipi spaziali
- Le colonne **varchar(max)** e **varbinary(max)** non possono fare parte di un indice. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], quando una tabella contiene le colonne **varchar(max)** o **varbinary(max)** , è possibile compilare o ricompilare un indice cluster contenente altre colonne usando l'opzione **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non consente l'opzione **ONLINE** quando la tabella di base contiene le colonne **varchar(max)** o **varbinary(max)**

Per altre informazioni, vedere [Funzionamento delle operazioni sugli indici online](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}      

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

 Specifica se un'operazione sull'indice online è ripristinabile.

 ON      
L'operazione sull'indice è ripristinabile.

 OFF      
L'operazione sull'indice non è ripristinabile.

MAX_DURATION **=** *time* [**MINUTES**] usato con **RESUMABLE = ON** (richiede **ONLINE = ON**)   

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Indica il tempo (un valore intero specificato in minuti) di esecuzione di un'operazione sull'indice online ripristinabile prima di essere sospesa.

> [!IMPORTANT]
> Per informazioni più dettagliate sulle operazioni di indice eseguibili online, vedere [Linee guida per operazioni di indice online](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Negli indici columnstore non sono supportate le ricompilazioni degli indici online ripristinabili.

ALLOW_ROW_LOCKS = { **ON** | OFF }      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.

ON      
I blocchi di riga sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando usare blocchi di riga.

OFF      
I blocchi di riga non vengono utilizzati.

ALLOW_PAGE_LOCKS = { **ON** | OFF }      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica se sono consentiti blocchi a livello di pagina. Il valore predefinito è ON.

ON      
I blocchi a livello di pagina sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando utilizzare blocchi a livello di pagina.

OFF      
I blocchi a livello di pagina non vengono utilizzati.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica se eseguire o meno l'ottimizzazione per la contesa di inserimento dell'ultima pagina. Il valore predefinito è OFF. Per altre informazioni, vedere le sezione [Chiavi sequenziali](#sequential-keys).

MAXDOP = *max_degree_of_parallelism*      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Sostituisce l'opzione di configurazione **max degree of parallelism** per la durata dell'operazione sull'indice. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.

*max_degree_of_parallelism* può essere:

1      
Disattiva la generazione di piani paralleli.

\>1      
Consente di limitare al valore specificato, o a un valore più basso in base al carico di lavoro corrente del sistema, il numero massimo di processori utilizzati in un'operazione parallela sugli indici.

0 (predefinito)      
Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.

 Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Le operazioni parallele sugli indici non sono disponibili in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) ed [Edizioni e funzionalità supportate per SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

DATA_COMPRESSION      
Specifica l'opzione di compressione dei dati per l'indice, il numero di partizione o l'intervallo di partizioni specificato. descritte di seguito:

NONE      
L'indice o le partizioni specificate non vengono compressi.

ROW      
L'indice o le partizioni specificate vengono compressi utilizzando la compressione di riga.

PAGE      
L'indice o le partizioni specificate vengono compressi utilizzando la compressione di pagina.

Per altre informazioni sulla compressione, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ..._n_ ] **)**       
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Specifica le partizioni alle quali si applica l'impostazione DATA_COMPRESSION. Se l'indice non è partizionato, l'argomento ON PARTITIONS genererà un errore. Se la clausola ON PARTITIONS non viene fornita, l'opzione DATA_COMPRESSION si applica a tutte le partizioni di un indice partizionato.

\<partition_number_expression> può essere specificato nei modi seguenti:

- Specificare il numero di una partizione, ad esempio ON PARTITIONS (2).
- Specificare i numeri di partizione per più partizioni singole separati da virgole, ad esempio ON PARTITIONS (1, 5).
- Specificare sia intervalli, sia singole partizioni, ad esempio ON PARTITIONS (2, 4, 6 TO 8).

È possibile specificare \<range> sotto forma di numeri di partizione separati dalla parola TO, ad esempio: `ON PARTITIONS (6 TO 8)`.

 Per impostare tipi diversi di compressione dei dati per partizioni diverse, specificare più volte l'opzione DATA_COMPRESSION, ad esempio:

```sql
REBUILD WITH
(
  DATA_COMPRESSION = NONE ON PARTITIONS (1),
  DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
  DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
);
```

## <a name="remarks"></a>Osservazioni
L'istruzione CREATE INDEX viene ottimizzata come qualsiasi altra query. Al fine di limitare le operazioni di I/O, è possibile che Query Processor scelga di sottoporre ad analisi un altro indice anziché eseguire un'analisi di tabella. In alcune situazioni è possibile che l'operazione di ordinamento venga eliminata. Nei computer multiprocessore l'istruzione CREATE INDEX può utilizzare più processori per eseguire le operazioni di analisi e ordinamento associate alla creazione dell'indice, in modo identico alle altre query. Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).

L'operazione di creazione dell'indice può essere sottoposta a una registrazione minima se è impostato il modello di recupero del database con registrazione minima o con registrazione minima delle operazioni bulk.

È possibile creare indici per una tabella temporanea. Quando si elimina la tabella o termina la sessione, vengono eliminati anche gli indici associati.

Un indice cluster può essere compilato in base a una variabile di tabella quando viene creata una chiave primaria. Quando la query viene completata o la sessione termina, l'indice viene eliminato.

Gli indici supportano proprietà estese.

## <a name="clustered-indexes"></a>Indici cluster
La creazione di un indice cluster per una tabella (heap) e l'eliminazione e la ricreazione di un indice cluster esistente richiedono la disponibilità di un'area di lavoro aggiuntiva nel database per contenere l'ordinamento dei dati e una copia temporanea della tabella originale o dei dati dell'indice cluster esistenti. Per altre informazioni sugli indici cluster, vedere [Creare indici cluster](../../relational-databases/indexes/create-clustered-indexes.md) e [Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="nonclustered-indexes"></a>Indici non cluster
A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] è possibile creare un indice non cluster per una tabella archiviata come indice columnstore cluster. Se si crea prima un indice non cluster per una tabella archiviata come heap o indice cluster, l'indice verrà conservato se in un secondo tempo la tabella viene convertita in un indice columnstore cluster. Non è inoltre necessario eliminare l'indice non cluster quando si ricompila l'indice columnstore cluster.

Limitazioni e restrizioni:

- L'opzione `FILESTREAM_ON` non è valida quando si crea un indice non cluster per una tabella archiviata come indice columnstore cluster.

## <a name="unique-indexes"></a>Indici univoci
Quando esiste un indice univoco, ogni volta che vengono aggiunti nuovi dati tramite un'operazione di inserimento, [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica l'eventuale presenza di valori duplicati. Le operazioni di inserimento che generano valori di chiave duplicati vengono sottoposte a rollback e in [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene visualizzato un messaggio di errore, anche nel caso in cui l'operazione di inserimento interessi più righe e crei un solo valore duplicato. Se si tenta di immettere dati per i quali è disponibile un indice univoco e la clausola IGNORE_DUP_KEY è impostata su ON, l'operazione ha esito negativo solo per le righe che violano l'indice UNIQUE.

## <a name="partitioned-indexes"></a>Indici partizionati
Gli indici partizionati vengono creati e gestiti in modo analogo alle tabelle partizionate, ma, come gli indici normali, vengono trattati come oggetti di database separati. È possibile creare un indice partizionato per una tabella non partizionata, nonché creare un indice non partizionato per una tabella partizionata.

Se si crea un indice per una tabella partizionata senza specificare un filegroup in cui inserirlo, l'indice verrà partizionato in modo identico alla tabella sottostante, in quanto per impostazione predefinita gli indici vengono inseriti negli stessi filegroup delle tabelle sottostanti e, nel caso di una tabella partizionata, nello stesso schema di partizione con colonne di partizionamento identiche. Se usa lo stesso schema di partizione e la stessa colonna di partizionamento della tabella, l'indice viene *allineato* alla tabella.

> [!WARNING]
> La creazione e la ricompilazione di indici non allineati per una tabella con oltre 1.000 partizioni sono possibili, ma non supportate. Questo tipo di operazioni può causare riduzioni delle prestazioni e un eccessivo consumo della memoria. Quando il numero di partizioni supera 1.000, si consiglia di utilizzare solo indici allineati.

Quando si partiziona un indice cluster non univoco, per impostazione predefinita nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono aggiunte tutte le colonne di partizionamento all'elenco di chiavi di indice cluster, se non sono già presenti.

È possibile creare viste indicizzate per tabelle partizionate in modo analogo agli indici delle tabelle. Per altre informazioni sugli indici partizionati, vedere [Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md) e [Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] le statistiche non vengono create analizzando tutte le righe nella tabella se viene creato o ricompilato un indice partizionato. Query Optimizer utilizza invece l'algoritmo di campionamento predefinito per generare statistiche. Per ottenere statistiche sugli indici partizionati analizzando tutte le righe nella tabella, usare `CREATE STATISTICS` o `UPDATE STATISTICS` con la clausola `FULLSCAN`.

## <a name="filtered-indexes"></a>Indici filtrati
Un indice filtrato è un indice non cluster ottimizzato, adatto per le query tramite cui viene selezionata una piccola percentuale di righe da una tabella. Utilizza un predicato del filtro per indicizzare una parte dei dati di una tabella. Un indice filtrato progettato correttamente consente di migliorare le prestazioni di esecuzione delle query e di ridurre i costi di archiviazione e di manutenzione.

### <a name="required-set-options-for-filtered-indexes"></a>Opzioni SET necessarie per gli indici filtrati

Le opzioni SET nella colonna Valore obbligatorio sono richieste ogni volta che si verifica una qualsiasi delle condizioni seguenti:

- Viene creato un indice filtrato.
- I dati di un indice filtrato vengono modificati tramite un'operazione INSERT, UPDATE, DELETE o MERGE.
- L'indice filtrato viene usato da Query Optimizer per generare il piano di query.

    |Opzioni SET|Valore richiesto|Valore server predefinito|Predefinito<br /><br /> OLE DB e ODBC predefinito|Predefinito<br /><br /> DB-Library predefinito|
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|
    |ANSI_NULLS|ON|ON|ATTIVA|OFF|
    |ANSI_PADDING|ON|ON|ATTIVA|OFF|
    |ANSI_WARNINGS*|ON|ON|ATTIVA|OFF|
    |ARITHABORT|ON|ATTIVA|OFF|OFF|
    |CONCAT_NULL_YIELDS_NULL|ON|ON|ATTIVA|OFF|
    |NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|
    |QUOTED_IDENTIFIER|ON|ON|ATTIVA|OFF|
  
     * Quando il livello di compatibilità del database è impostato su 90 o su un valore maggiore, l'impostazione di ANSI_WARNINGS su ON comporta anche l'impostazione implicita di ARITHABORT su ON. Se il livello di compatibilità del database è impostato su 80 o su un valore inferiore, l'opzione ARITHABORT deve essere impostata su ON in modo esplicito.

Se le opzioni SET non sono corrette, possono verificarsi le condizioni seguenti:

- L'indice filtrato non viene creato.
- [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore ed esegue il rollback delle istruzioni INSERT, UPDATE, DELETE o MERGE tramite le quali vengono modificati i dati dell'indice.
- Query Optimizer non considera l'indice nel piano di esecuzione per le istruzioni Transact-SQL.

 Per altre informazioni sugli indici filtrati, vedere [Creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md) e [Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="spatial-indexes"></a>Indici spaziali
Per informazioni sugli indici spaziali, vedere [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md) e [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).

## <a name="xml-indexes"></a>Indici XML
Per informazioni sugli indici XML, vedere [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md) e [Indici XML (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md).

## <a name="index-key-size"></a>Dimensione della chiave di indice
La dimensione massima per una chiave di indice è 900 byte per un indice cluster e 1700 byte per un indice non cluster. Prima di [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] il limite era sempre 900 byte. È possibile creare indici su colonne **varchar** che superano il limite di byte se i dati esistenti nelle colonne non superano tale limite quando l'indice viene creato. Le successive operazioni di inserimento o aggiornamento nelle colonne che determinano l'aumento della dimensione totale oltre il limite avranno tuttavia esito negativo. La chiave di indice di un indice cluster non può contenere colonne di tipo **varchar** con dati esistenti nell'unità di allocazione ROW_OVERFLOW_DATA. Se viene creato un indice cluster in una colonna **varchar** e i dati esistenti si trovano nell'unità di allocazione IN_ROW_DATA, le azioni di inserimento o aggiornamento successive eseguite nella colonna che comporterebbero lo spostamento dei dati all'esterno delle righe avranno esito negativo.

Negli indici non cluster possono essere incluse colonne non chiave nel relativo livello foglia. Queste colonne non vengono considerate dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante il calcolo della dimensione della chiave di indice. Per altre informazioni, vedere [Creare indici con colonne incluse](../../relational-databases/indexes/create-indexes-with-included-columns.md) e [Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

> [!NOTE]
> Quando le tabelle vengono partizionate, le colonne della chiave di partizionamento vengono aggiunte all'indice dal [!INCLUDE[ssDE](../../includes/ssde-md.md)], se non sono già presenti in un indice cluster non univoco. Le dimensioni combinate delle colonne indicizzate, senza le colonne incluse, più tutte le colonne di partizionamento aggiunte non possono superare 1800 byte in un indice cluster non univoco.

## <a name="computed-columns"></a>Colonne calcolate
Gli indici possono essere creati su colonne calcolate. Per le colonne calcolate è inoltre possibile impostare la proprietà PERSISTED. Questo significa che [!INCLUDE[ssDE](../../includes/ssde-md.md)] archivia i valori calcolati nella tabella e li aggiorna quando vengono aggiornate altre colonne da cui dipende la colonna calcolata. [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizza questi valori persistenti quando crea un indice sulla colonna e quando viene fatto riferimento all'indice all'interno di una query.

Per indicizzare una colonna calcolata, è necessario che tale colonna sia deterministica e precisa. La proprietà PERSISTED consente tuttavia di espandere i tipi di colonne calcolate indicizzabili, includendo i tipi seguenti:

- Colonne calcolate basate su funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e CLR e metodi con tipo CLR definito dall'utente contrassegnati come deterministici dall'utente.
- Colonne calcolate basate su espressioni che sono deterministiche, secondo quanto definito in [!INCLUDE[ssDE](../../includes/ssde-md.md)], ma imprecise.

Le colonne calcolate persistenti richiedono l'impostazione delle seguenti opzioni SET come mostrato nella sezione precedente [Opzioni SET necessarie per gli indici filtrati](#required-set-options-for-filtered-indexes).

Il vincolo UNIQUE o PRIMARY KEY può includere una colonna calcolata a condizione che vengano soddisfatte tutte le condizioni per l'indicizzazione. In particolare, la colonna calcolata deve essere deterministica e precisa oppure deterministica e persistente. Per altre informazioni sul determinismo, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

Le colonne calcolate derivate dai tipi di dati **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** e **xml** possono essere indicizzate come colonne chiave o colonne non chiave incluse purché il tipo di dati della colonna calcolata sia consentito come colonna chiave o colonna non chiave dell'indice. Ad esempio, non è possibile creare un indice XML primario per una colonna **xml** calcolata. Se la dimensione della chiave di indice supera i 900 byte, viene visualizzato un messaggio di avviso.

La creazione di un indice su una colonna calcolata può impedire l'esecuzione di un'operazione di inserimento o di aggiornamento che in precedenza veniva eseguita correttamente. Questo problema si può verificare quando la colonna calcolata genera un errore aritmetico. Nella tabella seguente, ad esempio, nonostante la colonna calcolata `c` generi un errore aritmetico, l'istruzione INSERT viene eseguita correttamente.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
INSERT INTO t1 VALUES (1, 0);
```

Se invece, dopo la creazione della tabella, viene creato un indice sulla colonna calcolata `c`, la stessa istruzione `INSERT` avrà esito negativo.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
CREATE UNIQUE CLUSTERED INDEX Idx1 ON t1(c);
INSERT INTO t1 VALUES (1, 0);
```

Per altre informazioni, vedere [Indici per le colonne calcolate](../../relational-databases/indexes/indexes-on-computed-columns.md).

## <a name="included-columns-in-indexes"></a>Colonne incluse di indici
È possibile aggiungere colonne non chiave, o incluse, al livello foglia di un indice non cluster per migliorare le prestazioni di esecuzione delle query tramite la copertura della query. Questo significa che tutte le colonne a cui la query fa riferimento sono incluse nell'indice come colonne chiave o non chiave. In questo modo, per individuare tutte le informazioni necessarie, in Query Optimizer verrà eseguita un'analisi dell'indice, senza necessità di accedere ai dati della tabella o dell'indice cluster. Per altre informazioni, vedere [Creare indici con colonne incluse](../../relational-databases/indexes/create-indexes-with-included-columns.md) e [Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="specifying-index-options"></a>Impostazione di opzioni per gli indici
In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sono state introdotte nuove opzioni per gli indici ed è stata modificata la modalità di impostazione di tali opzioni. Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH *option_name* equivale a WITH **(** \<option_name> **= ON )** . Quando si impostano opzioni per gli indici, è necessario rispettare le regole seguenti:

- Le nuove opzioni per gli indici possono essere specificate solo usando WITH (**_option\_name_ = ON | OFF**).
- Non è possibile specificare opzioni utilizzando in una stessa istruzione sia la sintassi compatibile con le versioni precedenti, sia la nuova sintassi. Se ad esempio si specifica WITH (**DROP_EXISTING, ONLINE = ON**), l'istruzione avrà esito negativo.
- Quando si crea un indice XML le opzioni devono essere specificate usando WITH (**_option_name_= ON | OFF**).

## <a name="drop_existing-clause"></a>Clausola DROP_EXISTING
È possibile utilizzare la clausola DROP_EXISTING per ricompilare l'indice, aggiungere o eliminare colonne, modificare opzioni, modificare il tipo di ordinamento delle colonne oppure cambiare lo schema di partizione o il filegroup.

Se l'indice applica un vincolo PRIMARY KEY o UNIQUE e la definizione dell'indice non viene modificata in alcun modo, l'indice verrà eliminato e ricreato mantenendo il vincolo esistente. Se invece la definizione dell'indice viene modificata, l'istruzione avrà esito negativo. Per modificare la definizione di un vincolo PRIMARY KEY o UNIQUE, eliminare il vincolo e aggiungere un vincolo con la nuova definizione.

La clausola DROP_EXISTING consente un miglioramento delle prestazioni quando viene ricreato un indice cluster, con un set di chiavi identico oppure diverso, per una tabella che include anche indici non cluster. La clausola DROP_EXISTING sostituisce l'esecuzione di un'istruzione DROP INDEX sull'indice cluster precedente e quindi di un'istruzione CREATE INDEX per il nuovo indice cluster. Gli indici non cluster vengono ricompilati una sola volta e poi solo in caso di modifica della relativa definizione. La clausola DROP_EXISTING non ricompila gli indici non cluster quando la definizione dell'indice contiene gli stessi valori dell'indice originale relativi al nome di indice, alle colonne chiave e di partizione, all'attributo di univocità e al tipo di ordinamento.

Indipendentemente dal fatto che gli indici non cluster vengano ricompilati o meno, rimangono sempre nei filegroup o negli schemi di partizione originali e utilizzano le funzioni di partizione originali. Se un indice cluster viene ricompilato in un filegroup o in uno schema di partizione diverso, gli indici non cluster non vengono spostati in funzione della nuova posizione dell'indice cluster. Pertanto, anche gli indici non cluster in precedenza allineati con l'indice cluster potrebbero non essere più allineati. Per altre informazioni sull'allineamento degli indici partizionati, vedere [Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

La clausola DROP_EXISTING non ripete l'ordinamento dei dati se vengono utilizzate le stesse colonne chiave indice nello stesso ordine e con lo stesso tipo di ordinamento crescente o decrescente, a meno che nell'istruzione dell'indice non venga specificato un indice non cluster e l'opzione ONLINE sia impostata su OFF. Se l'indice cluster è disabilitato, l'operazione CREATE INDEX WITH DROP_EXISTING deve essere eseguita con l'opzione ONLINE impostata su OFF. Se un indice non cluster è disabilitato e non è associato a un indice cluster disabilitato, l'operazione CREATE INDEX WITH DROP_EXISTING può essere eseguita con l'opzione ONLINE impostata su OFF o ON.

> [!NOTE]
> Quando vengono eliminati o ricompilati indici con un numero di extent pari o superiore a 128, tramite il [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono posticipate le effettive deallocazioni delle pagine e i blocchi associati fino al termine del commit della transazione.

## <a name="online-option"></a>Opzione ONLINE
Per l'esecuzione di operazioni sugli indici online, è necessario attenersi alle indicazioni seguenti:

- Non è possibile modificare, troncare o eliminare la tabella sottostante mentre è in corso un'operazione sull'indice online.
- Durante l'operazione sull'indice lo spazio su disco necessario aumenta temporaneamente.
- È possibile eseguire operazioni online su indici partizionati e indici che contengono colonne calcolate persistenti o colonne incluse.

Per altre informazioni, vedere [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a>Operazioni sull'indice ripristinabili
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Per l'esecuzione di operazioni sull'indice ripristinabili, è necessario attenersi alle indicazioni seguenti:

- La creazione dell'indice online viene specificata come ripristinabile usando l'opzione `RESUMABLE = ON`.
- L'opzione RESUMABLE non è persistente nei metadati per un determinato indice e si applica solo alla durata di un'istruzione DDL corrente. Per abilitare la funzione di ripristino, è necessario quindi che la clausola `RESUMABLE = ON` sia specificata in modo esplicito.
- L'opzione MAX_DURATION è supportata solo per l'opzione `RESUMABLE = ON`.
- L'opzione MAX_DURATION per RESUMABLE specifica l'intervallo di tempo per la compilazione di un indice. Trascorso questo tempo, la compilazione dell'indice viene sospesa oppure viene completata. È l'utente a decidere quando riprendere la compilazione di un indice che è stata sospesa. Il valore espresso in minuti in **time** per MAX_DURATION deve essere maggiore di 0 e minore o uguale a una settimana (7 \* 24 \* 60 = 10080 minuti). Una sospensione prolungata di un'operazione sull'indice può compromettere le prestazioni DML su una tabella specifica, nonché la capacità del disco del database poiché entrambi gli indici, quello originale e quello appena creato, richiedono spazio su disco e devono essere aggiornati durante le operazioni DML. Se l'opzione MAX_DURATION viene omessa, l'operazione sull'indice continuerà fino al suo completamento o fino a quando non si verificherà un errore.
- Per sospendere immediatamente l'operazione sull'indice, è possibile arrestare il comando in corso (CTRL+C) oppure eseguire il comando [ALTER INDEX](alter-index-transact-sql.md) PAUSE o il comando `KILL <session_id>`. Quando il comando viene sospeso, è possibile riprenderlo usando il comando [ALTER INDEX](alter-index-transact-sql.md).
- Eseguendo nuovamente l'istruzione CREATE INDEX originale per l'indice ripristinabile, un'operazione di creazione indice sospesa riprende automaticamente.
- L'opzione `SORT_IN_TEMPDB = ON` non è supportata per l'indice ripristinabile.
- Il comando DDL con `RESUMABLE = ON` non può essere eseguito all'interno di una transazione esplicita (non può far parte del blocco BEGIN TRAN ... COMMIT).
- Per riprendere/interrompere la creazione o la ricompilazione di un indice, usare la sintassi T-SQL [ALTER INDEX](alter-index-transact-sql.md)

> [!NOTE]
> Il comando DDL viene eseguito fin tanto che è completato, sospeso o non riuscito. Nel caso in cui il comando sia sospeso, verrà generato un errore a indicare che l'operazione è stata sospesa e che la creazione dell'indice non è stata completata. Altre informazioni sullo stato corrente dell'indice sono disponibili in [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Come in precedenza, in caso di errore verrà generato un errore.

Per indicare che la creazione di un indice viene eseguita come operazione ripristinabile e per verificarne lo stato di esecuzione corrente, vedere [index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md).

#### <a name="resources"></a>Risorse
Sono necessarie le risorse seguenti per un'operazione di creazione dell'indice online ripristinabile:

- Uno spazio aggiuntivo necessario per mantenere l'indice compilato, incluso il tempo di sospensione dell'indice
- Velocità effettiva superiore del log delle transazioni nella fase di ordinamento. L'utilizzo complessivo dello spazio nel log delle transazioni è minore rispetto alla normale creazione di indici online e consente il troncamento del log durante questa operazione.
- Uno stato DDL per impedire eventuali modifiche DDL
- La pulizia fantasma è bloccata nell'indice della compilazione per la durata dell'operazione, sia in pausa che mentre l'operazione è in esecuzione.

#### <a name="current-functional-limitations"></a>Limitazioni funzionali attuali
Le funzionalità seguenti sono disabilitate per le operazioni di creazione dell'indice ripristinabili:

- Quando un'operazione di creazione dell'indice online ripristinabile è sospesa, il valore iniziale di MAXDOP non può essere modificato
- Creare un indice che contenga:

  - Colonne calcolate o colonne TIMESTAMP come colonne chiave
  - Colonna LOB come colonna inclusa per la creazione indice ripristinabile
  - Indice filtrato

## <a name="row-and-page-locks-options"></a>Opzioni per blocchi di riga e di pagina
Se `ALLOW_ROW_LOCKS = ON` e `ALLOW_PAGE_LOCK = ON`, i blocchi a livello di riga, pagina e tabella sono consentiti quando si accede all'indice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] sceglie il blocco appropriato e può eseguire un'escalation del blocco da un blocco di riga o di pagina a un blocco di tabella.

Se `ALLOW_ROW_LOCKS = OFF` e `ALLOW_PAGE_LOCK = OFF`, è consentito solo un blocco a livello di tabella quando si accede all'indice.

## <a name="sequential-keys"></a>Chiavi sequenziali
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La contesa di inserimento dell'ultima pagina è un problema di prestazioni comune che si verifica quando un numero elevato di thread simultanei tenta di inserire righe in un indice con una chiave sequenziale. Un indice viene considerato sequenziale quando la colonna chiave iniziale contiene valori che sono sempre crescenti o decrescenti, ad esempio una colonna Identity o una data che per impostazione predefinita è la data/ora corrente. Poiché le chiavi da inserire sono sequenziali, tutte le nuove righe verranno inserite alla fine della struttura dell'indice (in altre parole, nella stessa pagina). Questo comporta una contesa per la pagina in memoria, che si manifesta come diversi thread in attesa di PAGELATCH_EX per la pagina in questione.

L'attivazione dell'opzione di indice OPTIMIZE_FOR_SEQUENTIAL_KEY abilita un'ottimizzazione all'interno del motore di database che contribuisce a migliorare la velocità effettiva per gli inserimenti nell'indice con un elevato grado di concorrenza. Questa opzione è destinata agli indici che dispongono di una chiave sequenziale e che pertanto sono soggetti a contesa di inserimento dell'ultima pagina, ma può essere utile anche per gli indici contenenti aree sensibili in altre aree della struttura dell'indice albero B.

## <a name="viewing-index-information"></a>Visualizzazione delle informazioni degli indici
Per ottenere informazioni sugli indici, è possibile utilizzare viste del catalogo, funzioni di sistema e stored procedure di sistema.

## <a name="data-compression"></a>Compressione dei dati
La compressione dei dati è descritta nell'argomento [Compressione dei dati](../../relational-databases/data-compression/data-compression.md). Di seguito sono illustrati i punti principali da considerare:

- La compressione può consentire di archiviare più righe in una pagina, ma non di modificare la dimensione massima della riga.
- Alle pagine non foglia di un indice non può essere applicata la compressione a livello di pagina, ma può essere applicata la compressione a livello di riga.
- Ogni indice non cluster dispone di un'impostazione di compressione separata e non eredita l'impostazione di compressione della tabella sottostante.
- Quando un indice cluster viene creato in un heap, tale indice eredita lo stato di compressione dell'heap, a meno che non venga specificato uno stato di compressione alternativo.

Agli indici partizionati vengono applicate le restrizioni seguenti:

- Non è possibile modificare l'impostazione di compressione di una singola partizione se la tabella include indici non allineati.
- La sintassi ALTER INDEX \<index> ... La sintassi REBUILD PARTITION ricompila la partizione specificata dell'indice.
- La sintassi ALTER INDEX \<index> ... La sintassi REBUILD WITH ricompila tutte le partizioni dell'indice.

Per valutare il modo in cui la modifica dello stato di compressione influirà su una tabella, un indice o una partizione, usare la stored procedure [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

## <a name="permissions"></a>Autorizzazioni
È richiesta l'autorizzazione `ALTER` per la tabella o la vista. L'utente deve essere un membro del ruolo predefinito del server `sysadmin` o dei ruoli predefiniti del database `db_ddladmin` e `db_owner`.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni
In [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] non è possibile creare:

- Un indice rowstore cluster o non cluster per una tabella di data warehouse se esiste già un indice columnstore. Questo comportamento è diverso rispetto a SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], che consente la coesistenza di indici rowstore e columnstore nella stessa tabella.
- Non è possibile creare un indice per una vista.

## <a name="metadata"></a>Metadati
Per visualizzare informazioni sugli indici esistenti, è possibile eseguire una query sulla vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

## <a name="version-notes"></a>Note sulla versione

[!INCLUDE[ssSDS](../../includes/sssds-md.md)] non supporta le opzioni filegroup e FileStream.

## <a name="examples-all-versions-uses-the-adventureworks-database"></a>Esempi: tutte le versioni. Usa il database AdventureWorks.

### <a name="a-create-a-simple-nonclustered-rowstore-index"></a>R. Creare un indice rowstore non cluster semplice
Negli esempi che seguono viene creato un indice non cluster per la colonna `VendorID` della tabella `Purchasing.ProductVendor`.

```sql
CREATE INDEX IX_VendorID ON ProductVendor (VendorID);
CREATE INDEX IX_VendorID ON dbo.ProductVendor (VendorID DESC, Name ASC, Address DESC);
CREATE INDEX IX_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="b-create-a-simple-nonclustered-rowstore-composite-index"></a>B. Creare un indice rowstore composto non cluster semplice
Nell'esempio seguente viene creato un indice composto non cluster per le colonne `SalesQuota` e `SalesYTD` della tabella `Sales.SalesPerson`.

```sql
CREATE NONCLUSTERED INDEX IX_SalesPerson_SalesQuota_SalesYTD ON Sales.SalesPerson (SalesQuota, SalesYTD);
```

### <a name="c-create-an-index-on-a-table-in-another-database"></a>C. Creare un indice per una tabella in un altro database
Nell'esempio seguente viene creato un indice cluster per la colonna `VendorID` della tabella `ProductVendor` nel database `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="d-add-a-column-to-an-index"></a>D. Aggiungere una colonna a un indice
Nell'esempio seguente viene creato l'indice IX_FF con due colonne della tabella dbo.FactFinance. L'istruzione successiva ricompila l'indice con un'ulteriore colonna e mantiene il nome esistente.

```sql
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey ASC, DateKey ASC);

-- Rebuild and add the OrganizationKey
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey, DateKey, OrganizationKey DESC)
  WITH (DROP_EXISTING = ON);
```

## <a name="examples-sql-server-azure-sql-database"></a>Esempi: SQL Server, database SQL di Azure

### <a name="e-create-a-unique-nonclustered-index"></a>E. Creare un indice non cluster univoco
Nell'esempio seguente viene creato un indice non cluster univoco sulla colonna `Name` della tabella `Production.UnitMeasure` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Questo indice impone l'univocità dei dati inseriti nella colonna `Name`.

```sql
CREATE UNIQUE INDEX AK_UnitMeasure_Name
  ON Production.UnitMeasure(Name);
```

Nella query seguente viene verificato il vincolo di univocità mediante un tentativo di inserimento di una riga con lo stesso valore di una riga esistente.

```sql
-- Verify the existing value.
SELECT Name FROM Production.UnitMeasure WHERE Name = N'Ounces';
GO

INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name, ModifiedDate)
  VALUES ('OC', 'Ounces', GETDATE());
```

Il messaggio di errore risultante è:

```
Server: Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object 'UnitMeasure' with unique index 'AK_UnitMeasure_Name'. The statement has been terminated.
```

### <a name="f-use-the-ignore_dup_key-option"></a>F. Uso dell'opzione IGNORE_DUP_KEY
Nell'esempio seguente viene illustrato l'effetto dell'opzione `IGNORE_DUP_KEY` tramite l'inserimento di più righe in una tabella temporanea prima con questa opzione impostata su `ON` e quindi con questa opzione impostata su `OFF`. Nella tabella `#Test` viene inserita una singola riga che genererà intenzionalmente un valore duplicato quando verrà eseguita la seconda istruzione `INSERT` su più righe. Il calcolo delle righe della tabella restituisce il numero di righe inserite.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = ON);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Di seguito sono riportati i risultati della seconda istruzione `INSERT`.

```cmd
Server: Msg 3604, Level 16, State 1, Line 5 Duplicate key was ignored.

Number of rows
--------------
38
```

Si noti che le righe della tabella `Production.UnitMeasure` che non violano il vincolo di univocità sono state inserite correttamente. Nonostante sia stato visualizzato un messaggio di avviso e sia stata ignorata la riga duplicata, non è stato eseguito un rollback dell'intera transazione.

A questo punto vengono eseguite nuovamente le stesse istruzioni, ma l'opzione `IGNORE_DUP_KEY` è impostata su `OFF`.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = OFF);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Di seguito sono riportati i risultati della seconda istruzione `INSERT`.

```
Server: Msg 2601, Level 14, State 1, Line 5
Cannot insert duplicate key row in object '#Test' with unique index
'AK_Index'. The statement has been terminated.

Number of rows
--------------
1
```

Si noti che nella tabella non è stata inserita alcuna riga della tabella `Production.UnitMeasure`, sebbene la violazione del vincolo dell'indice `UNIQUE` fosse determinata da una sola riga.

### <a name="g-using-drop_existing-to-drop-and-re-create-an-index"></a>G. Utilizzo di DROP_EXISTING per l'eliminazione e la ricreazione di un indice
Nell'esempio seguente viene eliminato e ricreato un indice esistente nella colonna `ProductID` della tabella `Production.WorkOrder` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] utilizzando l'opzione `DROP_EXISTING`. Vengono inoltre impostate le opzioni `FILLFACTOR` e `PAD_INDEX` .

```sql
CREATE NONCLUSTERED INDEX IX_WorkOrder_ProductID
  ON Production.WorkOrder(ProductID)
    WITH (FILLFACTOR = 80,
      PAD_INDEX = ON,
      DROP_EXISTING = ON);
GO
```

### <a name="h-create-an-index-on-a-view"></a>H. Creare un indice per una vista
Nell'esempio seguente vengono creati una vista e un indice per tale vista, quindi vengono eseguite due query in cui viene usata la vista indicizzata.

```sql
-- Set the options to support indexed views
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
  QUOTED_IDENTIFIER, ANSI_NULLS ON;
GO

-- Create view with schemabinding
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
  DROP VIEW Sales.vOrders;
GO

CREATE VIEW Sales.vOrders
  WITH SCHEMABINDING
AS
  SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Revenue,
    OrderDate, ProductID, COUNT_BIG(*) AS COUNT
  FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
  WHERE od.SalesOrderID = o.SalesOrderID
  GROUP BY OrderDate, ProductID;
GO

-- Create an index on the view
CREATE UNIQUE CLUSTERED INDEX IDX_V1
  ON Sales.vOrders (OrderDate, ProductID);
GO

-- This query can use the indexed view even though the view is
-- not specified in the FROM clause.
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev,
  OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND ProductID BETWEEN 700 AND 800
    AND OrderDate >= CONVERT(DATETIME, '05/01/2002', 101)
GROUP BY OrderDate, ProductID
ORDER BY Rev DESC;
GO

-- This query can use the above indexed view
SELECT OrderDate, SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND DATEPART(mm, OrderDate) = 3
  AND DATEPART(yy, OrderDate) = 2002
GROUP BY OrderDate
ORDER BY OrderDate ASC;
GO
```

### <a name="i-create-an-index-with-included-non-key-columns"></a>I. Creare un indice con colonne incluse (non chiave)
Nell'esempio seguente viene creato un indice non cluster con una colonna chiave (`PostalCode`) e quattro colonne non chiave (`AddressLine1`, `AddressLine2`, `City`, `StateProvinceID`), quindi viene eseguita una query che utilizza tale indice. Per visualizzare l'indice selezionato da Query Optimizer, nel menu **Query** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] selezionare **Includi piano di esecuzione effettivo** prima di eseguire la query.

```sql
CREATE NONCLUSTERED INDEX IX_Address_PostalCode
  ON Person.Address (PostalCode)
  INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);
GO

SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode
FROM Person.Address
WHERE PostalCode BETWEEN N'98000' and N'99999';
GO
```

### <a name="j-create-a-partitioned-index"></a>J. Creare un indice partizionato
Nell'esempio seguente viene creato un indice partizionato non cluster nello schema di partizione esistente `TransactionsPS1` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. In questo esempio si presuppone che sia stato installato l'esempio di indice partizionato.

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
CREATE NONCLUSTERED INDEX IX_TransactionHistory_ReferenceOrderID
  ON Production.TransactionHistory (ReferenceOrderID)
  ON TransactionsPS1 (TransactionDate);
GO
```

### <a name="k-creating-a-filtered-index"></a>K. Creazione di un indice filtrato
Nell'esempio seguente viene creato un indice filtrato nella tabella Production.BillOfMaterials nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Il predicato del filtro può includere colonne che non sono colonne chiave nell'indice filtrato. Il predicato in questo esempio consente di selezionare solo le righe in cui EndDate non ha un valore NULL.

```sql
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithEndDate"
  ON Production.BillOfMaterials (ComponentID, StartDate)
  WHERE EndDate IS NOT NULL;
```

### <a name="l-create-a-compressed-index"></a>L. Creare un indice compresso
Nell'esempio seguente viene creato un indice in una tabella non partizionata utilizzando la compressione di riga.

```sql
CREATE NONCLUSTERED INDEX IX_INDEX_1
  ON T1 (C2)
  WITH (DATA_COMPRESSION = ROW);
GO
```

Nell'esempio seguente viene creato un indice in una tabella partizionata utilizzando la compressione di riga in tutte le partizioni dell'indice.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (DATA_COMPRESSION = ROW);
GO
```

 Nell'esempio seguente viene creato un indice in una tabella partizionata utilizzando la compressione di pagina nella partizione `1` dell'indice e la compressione di riga nelle partizioni da `2` a `4` dell'indice.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (
    DATA_COMPRESSION = PAGE ON PARTITIONS(1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2 TO 4)
  );
GO
```

### <a name="m-create-resume-pause-and-abort-resumable-index-operations"></a>M. Creare, riprendere, sospendere e interrompere operazioni sull'indice ripristinabili
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx1 ON test_table (col1) WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx2 ON test_table (col2) WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx1 ON test_table PAUSE;
ALTER INDEX test_idx2 ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx1 ON test_table RESUME;
ALTER INDEX test_idx2 ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx1 ON test_table ABORT;
ALTER INDEX test_idx2 ON test_table ABORT;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="n-basic-syntax"></a>N. Sintassi di base
Creare, riprendere, sospendere e interrompere operazioni sull'indice ripristinabili       

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx ON test_table ABORT;
```

### <a name="o-create-a-nonclustered-index-on-a-table-in-the-current-database"></a>O. Creare un indice non cluster in una tabella nel database corrente
Nell'esempio seguente viene creato un indice non cluster nella colonna `VendorID` della tabella `ProductVendor`.

```sql
CREATE INDEX IX_ProductVendor_VendorID
  ON ProductVendor (VendorID);
```

### <a name="p-create-a-clustered-index-on-a-table-in-another-database"></a>P. Creare un indice cluster per una tabella in un altro database
Nell'esempio seguente viene creato un indice non cluster nella colonna `VendorID` della tabella `ProductVendor` nel database `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID
  ON Purchasing..ProductVendor (VendorID);
```
### <a name="q-create-an-ordered-clustered-index-on-a-table"></a>Q. Creare un indice cluster ordinato in una tabella  
L'esempio seguente crea un indice cluster ordinato nelle colonne `c1` e `c2` della tabella `T1` nel database `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T1 
ORDER (c1, c2);

```

### <a name="r-convert-a-cci-to-an-ordered-clustered-index-on-a-table"></a>R. Convertire un indice columnstore cluster in un indice cluster ordinato in una tabella  
L'esempio seguente converte l'indice columnstore cluster esistente in un indice columnstore cluster ordinato denominato `MyOrderedCCI` nelle colonne `c1` e `c2` della tabella `T2` del database `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T2
ORDER (c1, c2)
WITH (DROP_EXISTING = ON);

```

## <a name="see-also"></a>Vedere anche
[Architettura e guida per la progettazione degli indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md)  
[Indici e ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)     
[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)     
[CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)     
[CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)     
[CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)      
[CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)     
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)     
[Tipi di dati](../../t-sql/data-types/data-types-transact-sql.md)    
[DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)    
[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)    
[Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)     
[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)     
[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)    
[sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)     
[EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)