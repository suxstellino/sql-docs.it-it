---
description: CREATE TABLE (Transact-SQL)
title: CREATE TABLE (Transact-SQL)
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILESTREAM_TSQL
- TABLE
- CREATE_TABLE_TSQL
- CREATE TABLE
- FILESTREAM
- TABLE_TSQL
- FILESTREAM_ON
- FILESTREAM_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHECK constraints
- global temporary tables [SQL Server]
- local temporary tables [SQL Server]
- size [SQL Server], tables
- UNIQUE constraints [SQL Server], creating
- constraints [SQL Server], columns
- maximum number of indexes per table
- number of tables per database
- number of indexes per table
- table creation [SQL Server], CREATE TABLE
- temporary tables [SQL Server], creating
- table size [SQL Server]
- nullability [SQL Server]
- partitioned tables [SQL Server], creating
- DEFAULT definition
- null values [SQL Server], columns
- number of rows per table
- maximum number of columns per table
- FOREIGN KEY constraints, creating
- PRIMARY KEY constraints
- number of bytes per row
- CREATE TABLE statement
- number of columns per table
- maximum number of bytes per row
- data retention policy
ms.assetid: 1e068443-b9ea-486a-804f-ce7b6e048e8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4f4dd848c1e0255d9f8fd009a4908a1cc330bf9f
ms.sourcegitcommit: f5de0340c61202e743d9b301e514b95c0660a8e4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106098206"
---
# <a name="create-table-transact-sql"></a>CREATE TABLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea una nuova tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

> [!NOTE]
> Per la sintassi di [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], vedere [CREATE TABLE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/statements/create-table-azure-sql-data-warehouse.md).

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="simple-syntax"></a>Sintassi semplice

```syntaxsql
-- Simple CREATE TABLE Syntax (common if not using options)
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } [ ,...n ] )
[ ; ]
```

## <a name="full-syntax"></a>Sintassi completa

```syntaxsql
-- Disk-Based CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ AS FileTable ]
    ( {   <column_definition>
        | <computed_column_definition>
        | <column_set_definition>
        | [ <table_constraint> ] [ ,... n ]
        | [ <table_index> ] }
          [ ,...n ]
          [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
             , system_end_time_column_name ) ]
      )
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
    [ TEXTIMAGE_ON { filegroup | "default" } ]
    [ FILESTREAM_ON { partition_scheme_name
           | filegroup
           | "default" } ]
    [ WITH ( <table_option> [ ,...n ] ) ]
[ ; ]
  
<column_definition> ::=
column_name <data_type>
    [ FILESTREAM ]
    [ COLLATE collation_name ]
    [ SPARSE ]
    [ MASKED WITH ( FUNCTION = ' mask_function ') ]
    [ [ CONSTRAINT constraint_name ] DEFAULT constant_expression ]
    [ IDENTITY [ ( seed,increment ) ]
    [ NOT FOR REPLICATION ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
    [ ROWGUIDCOL ]
    [ ENCRYPTED WITH
        ( COLUMN_ENCRYPTION_KEY = key_name ,
          ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,
          ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ) ]
    [ <column_constraint> [, ...n ] ]
    [ <column_index> ]
  
<data_type> ::=
[ type_schema_name . ] type_name
    [ ( precision [ , scale ] | max |
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]
  
<column_constraint> ::=
[ CONSTRAINT constraint_name ]
{     { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( < index_option > [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
            | filegroup | "default" } ]
  
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
  
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
}
  
<column_index> ::=
 INDEX index_name [ CLUSTERED | NONCLUSTERED ]
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
<computed_column_definition> ::=
column_name AS computed_column_expression
[ PERSISTED [ NOT NULL ] ]
[
    [ CONSTRAINT constraint_name ]
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
        | filegroup | "default" } ]
  
    | [ FOREIGN KEY ]
        REFERENCES referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE } ]
        [ ON UPDATE { NO ACTION } ]
        [ NOT FOR REPLICATION ]
  
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
]
  
<column_set_definition> ::=
column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
  
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
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )

< table_index > ::=
{  
    {  
      INDEX index_name  [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ]
         (column_name [ ASC | DESC ] [ ,... n ] )
    | INDEX index_name CLUSTERED COLUMNSTORE
    | INDEX index_name [ NONCLUSTERED ] COLUMNSTORE (column_name [ ,... n ] )
    }
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
}

<table_option> ::=
{  
    [DATA_COMPRESSION = { NONE | ROW | PAGE }
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }
      [ , ...n ] ) ]]
    [ FILETABLE_DIRECTORY = <directory_name> ]
    [ FILETABLE_COLLATE_FILENAME = { <collation_name> | database_default } ]
    [ FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] ]
    [ REMOTE_DATA_ARCHIVE =
      {
          ON [ ( <table_stretch_options> [,...n] ) ]
        | OFF ( MIGRATION_STATE = PAUSED )
      }
    ]   
    [ DATA_DELETION = ON  
          {( 
             FILTER_COLUMN = column_name,   
             RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                              | MONTH | MONTHS | YEAR | YEARS }
        )}  
     ]
}
  
<table_stretch_options> ::=
{  
    [ FILTER_PREDICATE = { null | table_predicate_function } , ]
      MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
 }   
  
<index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | COMPRESSION_DELAY= {0 | delay [Minutes]}
  | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }
       [ ON PARTITIONS ( { <partition_number_expression> | <range> }
       [ , ...n ] ) ]
}
<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

## <a name="syntax-for-memory-optimized-table"></a>Sintassi per la tabella con ottimizzazione per la memoria

```syntaxsql
-- Memory optimized CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition>
    | [ <table_constraint> ] [ ,... n ]
    | [ <table_index> ]
      [ ,... n ] }
      [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
        , system_end_time_column_name ) ]
)
    [ WITH ( <table_option> [ ,... n ] ) ]
 [ ; ]

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
[
    [ CONSTRAINT constraint_name ] DEFAULT memory_optimized_constant_expression ]
    | [ IDENTITY [ ( 1, 1 ) ]
]
    [ <column_constraint> ]
    [ <column_index> ]
  
<data_type> ::=
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]

<column_constraint> ::=
 [ CONSTRAINT constraint_name ]
{
  { PRIMARY KEY | UNIQUE }
      {   NONCLUSTERED
        | NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count)
      }
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
  | CHECK ( logical_expression )
}
  
< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   { PRIMARY KEY | UNIQUE }
     {
       NONCLUSTERED (column [ ASC | DESC ] [ ,... n ])
       | NONCLUSTERED HASH (column [ ,... n ] ) WITH ( BUCKET_COUNT = bucket_count )
                    }
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
    | CHECK ( logical_expression )
}

<column_index> ::=
  INDEX index_name
{ [ NONCLUSTERED ] | [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count)}

<table_index> ::=
  INDEX index_name
{   [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
  
}
  
<table_option> ::=
{  
    MEMORY_OPTIMIZED = ON
  | DURABILITY = {SCHEMA_ONLY | SCHEMA_AND_DATA}
  | SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ]
  
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*database_name* è il nome del database in cui è viene creata la tabella. *database_name* deve specificare il nome di un database esistente. Se l'argomento *database_name* non viene specificato, il valore predefinito è il database corrente. L'account di accesso per la connessione corrente deve essere associato a un ID utente esistente nel database specificato da *database_name*. Questo ID utente deve avere le autorizzazioni CREATE TABLE.

*schema_name* è il nome dello schema a cui appartiene la nuova tabella.

*table_name* è il nome della nuova tabella. I nomi delle tabelle devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). *table_name* può essere costituito al massimo da 128 caratteri, ad eccezione dei nomi di tabelle temporanee locali, ovvero i nomi preceduti da un solo simbolo di cancelletto (#), che non possono superare i 116 caratteri.

AS FileTable **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).

Consente di creare la nuova tabella come tabella FileTable. Non è necessario specificare le colonne, perché una tabella FileTable è associata a uno schema fisso. Per altre informazioni, vedere [FileTable](../../relational-databases/blob/filetables-sql-server.md).

*column_name*
*computed_column_expression* è un'espressione che definisce il valore di una colonna calcolata. Una colonna calcolata è una colonna virtuale che non viene archiviata fisicamente nella tabella, a meno che non sia contrassegnata come PERSISTED. Questo tipo di colonna viene calcolata in base a un'espressione che utilizza altre colonne nella stessa tabella. La definizione di una colonna calcolata può ad esempio essere **costo** AS **prezzo**  \* **quantità**. L'espressione può essere un nome di colonna non calcolata, una costante, una funzione, una variabile e qualsiasi combinazione di questi elementi uniti da uno o più operatori. L'espressione non può essere una sottoquery o contenere tipi di dati alias.

È possibile usare le colonne calcolate in elenchi di selezione, clausole WHERE e ORDER BY o in qualsiasi altra posizione in cui è consentito l'utilizzo di espressioni regolari, con le eccezioni seguenti:

- Le colonne calcolate devono essere contrassegnate come PERSISTED per partecipare a un vincolo CHECK o FOREIGN KEY.
- È possibile usare una colonna calcolata come colonna chiave di un indice o come parte di un vincolo PRIMARY KEY o UNIQUE, a condizione che il valore della colonna sia definito da un'espressione deterministica e il tipo di dati del risultato sia supportato nelle colonne dell'indice.

   Se, ad esempio, la tabella include le colonne integer **a** e **b**, è possibile indicizzare la colonna calcolata **a+b**, ma non la colonna calcolata **a+DATEPART(dd, GETDATE())** , poiché in questo caso il valore può cambiare durante le chiamate successive.

- Non è possibile usare una colonna calcolata in un'istruzione INSERT o UPDATE.

> [!NOTE]
> Ogni riga di una tabella può includere valori diversi per le colonne correlate a una colonna calcolata. È pertanto possibile che non sia disponibile lo stesso valore per ciascuna riga della colonna calcolata.

In base alle espressioni utilizzate, il supporto dei valori Null per le colonne calcolate viene determinato automaticamente da [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Si presuppone che il risultato della maggior parte delle espressioni ammetta valori Null anche se sono presenti solo colonne in cui tali valori non sono ammessi, perché anche i possibili underflow oppure overflow generano risultati NULL. Per stabilire se una colonna calcolata in una tabella ammette o meno i valori Null, usare la funzione `COLUMNPROPERTY`con la proprietà **AllowsNull**. Un'espressione che ammette i valori Null può essere convertita in un'espressione in cui tali valori non sono ammessi specificando `ISNULL` con la costante *check_expression*, dove la costante è un valore non Null che sostituisce qualsiasi risultato NULL. Per le colonne calcolate basate su espressioni di tipo CLR (Common Language Runtime) definito dall'utente è richiesta l'autorizzazione REFERENCES per il tipo.

PERSISTED specifica che [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] archivierà fisicamente i valori calcolati nella tabella e li aggiornerà in caso di aggiornamento di eventuali altre colonne da cui dipende la colonna calcolata. Quando si contrassegna una colonna calcolata come `PERSISTED`, è possibile creare un indice deterministico, ma non preciso, in una colonna calcolata. Per altre informazioni, vedere [Indici per le colonne calcolate](../../relational-databases/indexes/indexes-on-computed-columns.md). Le colonne calcolate usate come colonne di partizionamento di una tabella partizionata devono essere contrassegnate come `PERSISTED` in modo esplicito. Se è specificato `PERSISTED`, il valore di *computed_column_expression* deve essere deterministico.

ON { *partition_scheme* | *filegroup* |  **"default"** } Specifica lo schema di partizione o il filegroup in cui la tabella viene archiviata. Se si specifica *partition_scheme*, la tabella deve essere partizionata e le relative partizioni devono essere archiviate in un set di uno o più filegroup specificati in *partition_scheme*. Se si specifica *filegroup*, la tabella viene archiviata nel filegroup indicato. Il filegroup deve essere presente nel database. Se si specifica **"default"** oppure si omette ON, la tabella viene archiviata nel filegroup predefinito. Il meccanismo di archiviazione di una tabella specificato in CREATE TABLE non può essere modificato in seguito.

È possibile specificare ON {*partition_scheme* | *filegroup* |  **"default"** } anche in un vincolo PRIMARY KEY o UNIQUE. Questi vincoli comportano la creazione di indici. Se si specifica *filegroup*, l'indice viene archiviato nel filegroup indicato. Se si specifica **"default"** oppure si omette ON, l'indice viene archiviato nello stesso filegroup della tabella. Se il vincolo `PRIMARY KEY` o `UNIQUE` crea un indice cluster, le pagine di dati della tabella vengono archiviate nello stesso filegroup dell'indice. Se si specifica `CLUSTERED` o se il vincolo crea in altro modo un indice cluster e si specifica un valore *partition_scheme* diverso dal valore *partition_scheme* o *filegroup* della definizione della tabella, o viceversa, verrà rispettata solo la definizione del vincolo e gli altri valori verranno ignorati.

> [!NOTE]
> In questo contesto, *default* non è una parola chiave. Si tratta di un identificatore del filegroup predefinito e deve essere delimitato, ad esempio ON **"default"** o ON **[** default **]** . Se si specifica **"default"** , l'opzione `QUOTED_IDENTIFIER` deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).
>
> Dopo avere creato una tabella partizionata, impostare l'opzione `LOCK_ESCALATION` per la tabella su `AUTO`. In questo modo è possibile migliorare la concorrenza consentendo ai blocchi di eseguire l'escalation a livello di partizione (HoBT) anziché di tabella. Per altre informazioni, vedere [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).

TEXTIMAGE_ON { *filegroup*|  **"default"** } Indica che le colonne di tipo **text**, **ntext**, **image**, **xml**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** e CLR definito dall'utente (inclusi geometry e geography) vengono archiviate nel filegroup specificato.

La parola chiave `TEXTIMAGE_ON` non è consentita se la tabella non include colonne con valori di grandi dimensioni. Non è possibile specificare `TEXTIMAGE_ON` se si specifica *partition_scheme*. Se si specifica **"default"** oppure si omette `TEXTIMAGE_ON`, le colonne con valori di grandi dimensioni vengono archiviate nel filegroup predefinito. Il tipo di archiviazione per i dati di colonne con valori di grandi dimensioni specificato in `CREATE TABLE` non può essere modificato in seguito.

> [!NOTE]
> I valori varchar(max), nvarchar(max), varbinary(max), xml e i valori di grandi dimensioni definiti dall'utente vengono archiviati direttamente nella riga di dati, con un limite massimo di 8.000 byte e a condizione che le dimensioni del record siano sufficienti per contenere il valore. Se le dimensioni del record non sono sufficienti per il valore, all'interno della riga viene archiviato un puntatore e i dati restanti vengono archiviati all'esterno della riga nello spazio di archiviazione LOB. 0 è il valore predefinito e indica che tutti i valori vengono archiviati direttamente nella riga di dati.
>
> `TEXTIMAGE_ON` modifica solo la posizione dello "spazio di archiviazione LOB", non influisce quando i dati sono archiviati nella riga. Usare l'opzione large value types out of row di sp_tableoption per archiviare l'intero valore LOB all'esterno della riga.
>
> In questo contesto, default non è una parola chiave, Si tratta di un identificatore del filegroup predefinito e deve essere delimitato, ad esempio `TEXTIMAGE_ON "default"` o `TEXTIMAGE_ON [default]`. Se si specifica **"default"** , l'opzione `QUOTED_IDENTIFIER` deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

FILESTREAM_ON { *partition_scheme_name* | filegroup | **"** default **"** } **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] e versioni successive). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non supporta `FILESTREAM`.

Specifica il filegroup per i dati FILESTREAM.

Se la tabella è partizionata e contiene dati FILESTREAM, la clausola FILESTREAM_ON deve essere inclusa e deve specificare uno schema di partizione dei filegroup FILESTREAM che utilizzi la stessa funzione e le stesse colonne di partizione dello schema di partizione per la tabella. In caso contrario, verrà generato un errore.

Se la tabella non è partizionata, la colonna FILESTREAM non può essere partizionata. I dati FILESTREAM per la tabella devono essere archiviati in un singolo filegroup, specificato nella clausola FILESTREAM_ON.

Se la tabella non è partizionata e la clausola `FILESTREAM_ON` non è specificata, viene usato il filegroup FILESTREAM per cui è impostata la proprietà `DEFAULT`. Se non è presente alcun filegroup FILESTREAM, viene generato un errore.

Analogamente a ON e a `TEXTIMAGE_ON`, il valore impostato tramite `CREATE TABLE` per `FILESTREAM_ON` non può essere modificato, ad eccezione dei casi seguenti:

- Un'istruzione [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) converte un heap in un indice cluster. In questo caso è possibile specificare un filegroup FILESTREAM diverso, uno schema di partizione o NULL.
- Un'istruzione [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) converte un indice cluster in un heap. In questo caso è possibile specificare un filegroup FILESTREAM diverso, uno schema di partizione o il valore **"default"** .

Per il filegroup nella clausola `FILESTREAM_ON <filegroup>` o per ogni filegroup FILESTREAM denominato nello schema di partizione deve essere presente un file definito per il filegroup. Il file deve essere definito usando un'istruzione [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) o [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). In caso contrario, viene generato un errore.

Per gli argomenti correlati a FILESTREAM, vedere [Dati BLOB (Binary Large Object)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).

[ _type\_schema\_name_ **.** ] *type_name* specifica il tipo di dati della colonna e lo schema a cui appartiene. Per le tabelle basate su disco, il tipo di dati può essere uno dei seguenti:

- Un tipo di dati di sistema
- Tipo di alias basato su un tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per consentirne l'uso in una definizione di tabella, i tipi di dati alias vengono creati con l'istruzione `CREATE TYPE`. L'assegnazione NULL o NOT NULL per un tipo di dati alias può essere ignorata durante l'esecuzione dell'istruzione `CREATE TABLE`. Non è tuttavia possibile modificare l'impostazione della lunghezza. In un'istruzione `CREATE TABLE` non è possibile specificare la lunghezza per un tipo di dati alias.
- Tipo CLR definito dall'utente. I tipi CLR definiti dall'utente devono essere creati con l'istruzione `CREATE TYPE` perché possano essere usati in una definizione di tabella. Per creare una colonna con un tipo CLR definito dall'utente, è richiesta l'autorizzazione REFERENCES per il tipo.

Se *type_schema_name* non viene specificato, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] fa riferimento a *type_name* in base all'ordine seguente:

- Tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Schema predefinito dell'utente corrente nel database corrente.
- Schema **dbo** nel database corrente.

Per le tabelle ottimizzate per la memoria, vedere [Tipi di dati supportati per OLTP in memoria](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md) per un elenco dei tipi di sistema supportati.

*precision* è la precisione del tipo di dati specificato. Per altre informazioni sui valori di precisione validi, vedere [Precisione, scala e lunghezza](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale* è la scala per il tipo di dati specificato. Per altre informazioni sui valori di scala validi, vedere [Precisione, scala e lunghezza](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max** si applica solo ai tipi di dati **varchar**, **nvarchar** e **varbinary** per l'archiviazione di 2^31 byte di dati di tipo carattere e binario e 2^30 byte di dati di tipo Unicode.

CONTENT specifica che ogni istanza del tipo di dati **xml** in *column_name* può contenere più elementi di livello superiore. CONTENT si applica solo al tipo di dati **xml** e può essere specificato solo se viene specificato anche *xml_schema_collection*. Se non specificato, CONTENT rappresenta il comportamento predefinito.

DOCUMENT specifica che ogni istanza del tipo di dati **xml** in *column_name* può contenere solo un elemento di livello superiore. DOCUMENT si applica solo al tipo di dati **xml** e può essere specificato solo se viene specificato anche *xml_schema_collection*.

*xml_schema_collection* si applica solo al tipo di dati **xml** per associare una raccolta di XML Schema al tipo. Per poter digitare una colonna **xml** in uno schema, è necessario creare lo schema nel database usando [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

DEFAULT specifica il valore assegnato alla colonna quando non ne viene specificato uno in modo esplicito durante un inserimento. È possibile applicare le definizioni DEFAULT a qualsiasi colonna, ad eccezione di quelle definite come **timestamp** o con la proprietà `IDENTITY`. Se si specifica un valore predefinito per una colonna di tipo definito dall'utente, il tipo deve supportare la conversione implicita da *constant_expression* nel tipo definito dall'utente. Le definizioni DEFAULT vengono rimosse quando la tabella viene eliminata. È possibile usare come predefinito solo un valore costante, ad esempio una stringa di caratteri, una funzione scalare (una funzione di sistema, definita dall'utente o CLR) oppure un valore NULL. Per garantire la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile assegnare un nome di vincolo a una definizione DEFAULT.

*constant_expression* è una costante, un valore NULL o una funzione di sistema usata come valore predefinito della colonna.

*memory_optimized_constant_expression* è una costante, un valore NULL o una funzione di sistema supportata e usata come valore predefinito della colonna. Deve essere supportata nelle stored procedure compilate in modo nativo. Per altre informazioni sulle funzioni predefinite nelle store procedure compilate in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).

IDENTITY indica che la nuova colonna è una colonna Identity. Quando si aggiunge una nuova riga alla tabella, [!INCLUDE[ssDE](../../includes/ssde-md.md)] assegna un valore univoco e incrementale alla colonna. Le colonne Identity vengono in genere utilizzate in combinazione con vincoli PRIMARY KEY come identificatori di riga univoci per la tabella. La proprietà `IDENTITY` può essere assegnata a colonne **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** o **numeric(p,0)** . Ogni tabella può includere una sola colonna Identity. Non è consentito associare valori predefiniti e vincoli DEFAULT alle colonne Identity. È necessario specificare sia il valore di inizializzazione, sia l'incremento oppure nessuno dei due. In questo secondo caso, il valore predefinito è (1,1).

*seed* è il valore di inizializzazione usato per la prima riga caricata nella tabella.

*increment* è il valore incrementale aggiunto al valore Identity della riga caricata in precedenza.

NOT FOR REPLICATION Nell'istruzione `CREATE TABLE` è possibile specificare la clausola `NOT FOR REPLICATION` per la proprietà IDENTITY, i vincoli FOREIGN KEY e i vincoli CHECK. Se si specifica questa clausola per la proprietà `IDENTITY`, i valori non vengono incrementati nelle colonne Identity per gli inserimenti eseguiti dagli agenti di replica. Se per un vincolo si specifica questa clausola, il vincolo non viene imposto quando gli agenti di replica eseguono le operazioni di inserimento, aggiornamento o eliminazione.

GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] [ NOT NULL ] **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indica che il sistema userà una colonna di tipo `datetime2` specifica per registrare l'ora di inizio o l'ora di fine per cui un record è valido. La colonna deve essere definita come `NOT NULL`. Se si tenta di specificare i valori come `NULL`, il sistema genera un errore. Se non si specifica in modo esplicito NOT NULL per una colonna periodo, il sistema definirà la colonna come `NOT NULL` per impostazione predefinita. Usare questo argomento in combinazione con gli argomenti `PERIOD FOR SYSTEM_TIME` e `WITH SYSTEM_VERSIONING = ON` per abilitare il controllo delle versioni di sistema per una tabella. Per altre informazioni, vedere [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Gli utenti possono contrassegnare una o entrambe le colonne periodo con il flag **HIDDEN** per nascondere in modo implicito le colonne. In questo modo **SELECT \*FROM** _`<table>`_ non restituisce un valore per tali colonne. Per impostazione predefinita, le colonne periodo non vengono nascoste. Per poter essere usate, le colonne nascoste devono essere incluse in modo esplicito in tutte le query che fanno direttamente riferimento alla tabella temporale. Per modificare l'attributo **HIDDEN** per una colonna periodo esistente, è necessario eliminare e ricreare **PERIOD** con un altro flag nascosto.

INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] ) **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica che deve essere creato un indice per la tabella. Può trattarsi di un indice cluster o un indice non cluster. L'indice conterrà le colonne indicate e i dati in ordine crescente o decrescente.

INDEX *index_name* CLUSTERED COLUMNSTORE **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica di archiviare l'intera tabella nel formato a colonne con un indice columnstore cluster. Questo include sempre tutte le colonne nella tabella. I dati non sono riportati in ordine alfabetico o numerico poiché le righe sono organizzate in modo da sfruttare la compressione del columnstore.

INDEX *index_name* [ NONCLUSTERED ] COLUMNSTORE (*column_name* [ ,... *n* ] ) **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica che deve essere creato un indice columnstore non cluster per la tabella. La tabella sottostante può essere un heap rowstore o un indice cluster oppure può essere un indice columnstore cluster. In tutti i casi, la creazione di un indice columnstore non cluster in una tabella consente di archiviare una seconda copia dei dati per le colonne dell'indice.

L'indice columnstore non cluster viene archiviato e gestito come indice columnstore cluster. Viene definito indice columnstore non cluster perché le colonne possono essere limitate e l'indice esiste come indice secondario di una tabella.

ON _partition\_scheme\_name_ **(** _column\_name_ **)** specifica lo schema di partizione che definisce i filegroup a cui verrà eseguito il mapping delle partizioni di un indice partizionato. È necessario che lo schema di partizione sia presente nel database e sia stato creato eseguendo [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* specifica la colonna in base alla quale verrà eseguita la partizione di un indice partizionato. La colonna deve corrispondere all'argomento della funzione di partizione usata da *partition_scheme_name* per tipo di dati, lunghezza e precisione. *column_name* non è limitato alle colonne nella definizione dell'indice. È possibile specificare qualsiasi colonna della tabella di base tranne quando si esegue la partizione di un indice UNIQUE. In questo caso il valore *column_name* deve essere scelto tra quelli usati come chiave univoca. Questa restrizione consente a [!INCLUDE[ssDE](../../includes/ssde-md.md)] di verificare l'univocità dei valori di chiave all'interno di una singola partizione.

> [!NOTE]
> Quando si partiziona un indice cluster non univoco, per impostazione predefinita [!INCLUDE[ssDE](../../includes/ssde-md.md)] aggiunge la colonna di partizionamento all'elenco delle chiavi di indice cluster, se non è già presente. Quando si partiziona un indice non cluster non univoco, [!INCLUDE[ssDE](../../includes/ssde-md.md)] aggiunge la colonna di partizionamento come colonna non chiave (inclusa) dell'indice, se non è già presente.

Se non si specifica *partition_scheme_name* o *filegroup* e la tabella è partizionata, l'indice viene posizionato nello stesso schema di partizione, usando la stessa colonna di partizionamento della tabella sottostante.

> [!NOTE]
> Non è possibile specificare uno schema di partizione per un indice XML. Se la tabella di base è partizionata, l'indice XML utilizzerà lo stesso schema di partizione della tabella.

Per altre informazioni sul partizionamento degli indici, vedere [Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON *filegroup_name* crea l'indice specificato nel filegroup specificato. Se non viene specificata una posizione e la tabella o la vista non è partizionata, l'indice utilizzerà lo stesso filegroup della tabella o della vista sottostante. Il filegroup deve essere già esistente.

ON **"default"** crea l'indice specificato nel filegroup predefinito.

In questo contesto il termine default non rappresenta una parola chiave, Si tratta di un identificatore del filegroup predefinito e deve essere delimitato, ad esempio ON **"default"** o ON **[default]** . Se si specifica "default", l'opzione `QUOTED_IDENTIFIER` deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ] **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] e versioni successive).

Specifica la posizione dei dati FILESTREAM per la tabella quando viene creato un indice cluster. La clausola FILESTREAM_ON consente di spostare i dati FILESTREAM in uno schema di partizione o in un filegroup FILESTREAM diverso.

*filestream_filegroup_name* è il nome di un filegroup FILESTREAM. Nel filegroup deve essere disponibile un file definito usando un'istruzione [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) o [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). In caso contrario, viene generato un errore.

Se la tabella è partizionata, la clausola `FILESTREAM_ON` deve essere inclusa e deve specificare uno schema di partizione dei filegroup FILESTREAM che usi la stessa funzione di partizione e le stesse colonne di partizione dello schema di partizione per la tabella. In caso contrario, viene generato un errore.

Se la tabella non è partizionata, la colonna FILESTREAM non può essere partizionata. I dati FILESTREAM per la tabella devono essere archiviati in un singolo filegroup specificato nella clausola `FILESTREAM_ON`.

È possibile specificare `FILESTREAM_ON NULL` in un'istruzione `CREATE INDEX` se si sta creando un indice cluster e se nella tabella non è contenuta alcuna colonna FILESTREAM.

Per altre informazioni, vedere [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md).

ROWGUIDCOL indica che la nuova colonna e una colonna GUID di riga. È possibile designare come colonna ROWGUIDCOL una sola colonna di tipo **uniqueidentifier** per ogni tabella. L'applicazione della proprietà ROWGUIDCOL consente di fare riferimento alla colonna usando `$ROWGUID`. La proprietà ROWGUIDCOL può essere assegnata solo a una colonna **uniqueidentifier**. Le colonne con tipo di dati definito dall'utente non possono essere designate con ROWGUIDCOL.

La proprietà ROWGUIDCOL non impone l'univocità dei valori archiviati nella colonna e non genera automaticamente valori per le nuove righe inserite nella tabella. Per generare valori univoci per ogni colonna, usare la funzione [NEWID](../../t-sql/functions/newid-transact-sql.md) o [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md) in istruzioni [INSERT](../../t-sql/statements/insert-transact-sql.md) oppure usare tali funzioni come valore predefinito per la colonna.

ENCRYPTED WITH specifica la crittografia di colonne usando la funzionalità [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

COLUMN_ENCRYPTION_KEY = *key_name* specifica la chiave di crittografia delle colonne. Per altre informazioni, vedere [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md).

ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }: la **crittografia deterministica** usa un metodo che genera sempre lo stesso valore crittografato per qualsiasi valore di testo normale specificato. L'uso della crittografia deterministica consente di eseguire operazioni di ricerca usando il confronto di uguaglianza, il raggruppamento e il join di tabelle con join di uguaglianza basati su valori crittografati, ma può anche consentire a utenti non autorizzati di ipotizzare informazioni sui valori crittografati esaminando i criteri nella colonna crittografata. È possibile creare un join di due tabelle nelle colonne crittografate in modo deterministico solo se entrambe le colonne vengono crittografate usando la stessa chiave di crittografia della colonna. La crittografia deterministica deve usare regole di confronto a livello di colonna con un ordinamento binario2 per colonne di tipo carattere.

La **crittografia casuale** usa un metodo di crittografia dei dati meno prevedibile. La crittografia casuale è più sicura, ma impedisce i calcoli e l'indicizzazione delle colonne crittografate, a meno che l'istanza di SQL Server non supporti Always Encrypted con enclave sicuri. Per informazioni dettagliate, vedere [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Se si usa Always Encrypted (senza enclave sicuri), usare la crittografia deterministica per le colonne in cui verrà eseguita la ricerca con parametri o parametri di raggruppamento, ad esempio un numero ID per gli enti pubblici. Usare la crittografia casuale per i dati, ad esempio il numero di carta di credito, che non sono raggruppati con altri record o usati per il join di tabelle e in cui non vengono eseguite ricerche perché si usano altre colonne, ad esempio un numero di transazione, per trovare la riga che contiene la colonna crittografata che interessa.

Se si usa Always Encrypted con enclave sicuri, la crittografia casuale è il tipo di crittografia consigliato.

Le colonne devono essere di un tipo di dati idoneo.

ALGORITHM **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive).

Deve essere **'AEAD_AES_256_CBC_HMAC_SHA_256'** .

Per altre informazioni sui vincoli della funzionalità, vedere [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

SPARSE indica che la colonna è di tipo sparse. L'archiviazione delle colonne di tipo sparse è ottimizzata per valori Null. Non è possibile designare le colonne di tipo sparse come NOT NULL. Per altre restrizioni e informazioni relative alle colonne di tipo sparse, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).

MASKED WITH ( FUNCTION = ' *mask_function* ') **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive).

Specifica una maschera dati dinamica. *mask_function* è il nome della funzione di maschera con i parametri appropriati. Sono disponibili quattro funzioni:

- default()
- email()
- partial()
- random()

Per i parametri di funzione, vedere [Mascheramento dati dinamici](../../relational-databases/security/dynamic-data-masking.md).

FILESTREAM **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] e versioni successive)

Valido solo per le colonne **varbinary (max)** . Specifica l'archiviazione FILESTREAM per i dati BLOB **varbinary(max)** .

Nella tabella deve inoltre essere presente una colonna con tipo di dati **uniqueidentifier** con l'attributo ROWGUIDCOL. Questa colonna non deve consentire valori Null e deve avere un vincolo a colonna singola UNIQUE o PRIMARY KEY. Il valore GUID per la colonna deve essere specificato da un'applicazione al momento dell'inserimento dei dati o da un vincolo DEFAULT che utilizza la funzione NEWID ().

Non è possibile eliminare la colonna ROWGUIDCOL, né modificare i vincoli correlati quando per la tabella è stata definita una colonna FILESTREAM. La colonna ROWGUIDCOL può essere eliminata solo dopo l'eliminazione dell'ultima colonna FILESTREAM.

Quando l'attributo di archiviazione FILESTREAM viene specificato per una colonna, tutti i valori per quella colonna vengono archiviati in un contenitore di dati FILESTREAM nel file system.

COLLATE *collation_name* specifica le regole di confronto per la colonna. È possibile usare nomi di regole di confronto di Windows o SQL. *collation_name* è applicabile solo alle colonne con tipo di dati **char**, **varchar**, **text**, **nchar**,  **nvarchar** e **ntext**. Se non specificato, alla colonna vengono assegnate le regole di confronto del tipo di dati definito dall'utente, se la colonna presenta questo tipo di dati, oppure le regole di confronto predefinite del database.

Per altre informazioni sui nomi delle regole di confronto di Windows e SQL, vedere gli argomenti relativi al [nome delle regole di confronto di Windows](../../t-sql/statements/windows-collation-name-transact-sql.md) e al [nome delle regole di confronto SQL](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Per altre informazioni, vedere [COLLATE](~/t-sql/statements/collations.md).

CONSTRAINT è una parola chiave facoltativa che indica l'inizio di una definizione di vincolo PRIMARY KEY, NOT NULL, UNIQUE, FOREIGN KEY o CHECK.

*constraint_name* è il nome di un vincolo. I nomi di vincolo devono essere univoci nell'ambito dello schema a cui appartiene la tabella.

NULL | NOT NULL indica se i valori NULL sono consentiti nella colonna. L'opzione NULL non è esattamente un vincolo, ma può essere specificata allo stesso modo di NOT NULL. È possibile specificare NOT NULL per le colonne calcolate solo se è specificato PERSISTED.

PRIMARY KEY è il vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco. È possibile creare un solo vincolo PRIMARY KEY per ogni tabella.

UNIQUE è un vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco. Una tabella può includere più vincoli UNIQUE.

CLUSTERED | NONCLUSTERED indica che per il vincolo PRIMARY KEY o UNIQUE viene creato un indice cluster o non cluster. Il valore predefinito per i vincoli PRIMARY KEY è CLUSTERED, mentre per i vincoli UNIQUE è NONCLUSTERED.

In un'istruzione `CREATE TABLE` è possibile specificare CLUSTERED per un solo vincolo. Se si specifica CLUSTERED per un vincolo UNIQUE e si specifica anche un vincolo PRIMARY KEY, il valore predefinito di quest'ultimo è NONCLUSTERED.

FOREIGN KEY REFERENCES è un vincolo che fornisce l'integrità referenziale per i dati di una o più colonne. È necessario che ogni valore delle colonne sia incluso nelle colonne di riferimento corrispondenti della tabella di riferimento. È possibile fare riferimento a vincoli FOREIGN KEY solo nelle colonne che nella tabella con riferimenti corrispondono a vincoli PRIMARY KEY o UNIQUE e nelle colonne a cui viene fatto riferimento in un indice univoco nella tabella di riferimento. È necessario contrassegnare come PERSISTED anche le chiavi esterne nelle colonne calcolate.

[ _schema\_name_ **.** ] *referenced_table_name*] indica i nomi della tabella a cui fa riferimento il vincolo FOREIGN KEY e dello schema a cui appartiene.

**(** *ref_column* [ **,** ... *n* ] **)** è una colonna o un elenco di colonne della tabella a cui fa riferimento il vincolo FOREIGN KEY.

ON DELETE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT } specifica quale azione viene eseguita sulle righe nella tabella creata, se tali righe includono una relazione referenziale e se la riga a cui viene fatto riferimento viene eliminata dalla tabella padre. Il valore predefinito è NO ACTION.

NO ACTION: [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore e viene eseguito il rollback dell'azione di eliminazione della riga nella tabella padre.

CASCADE: le righe corrispondenti vengono eliminate dalla tabella di riferimento se la riga viene eliminata dalla tabella padre.

SET NULL: tutti i valori che compongono la chiave esterna vengono impostati su NULL se viene eliminata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che le colonne chiave esterna ammettano valori Null.

SET DEFAULT: tutti i valori che compongono la chiave esterna vengono impostati sui rispettivi valori predefiniti se viene eliminata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che per tutte le colonne chiave esterna siano definiti valori predefiniti. Se una colonna ammette valori Null e non viene impostato un valore predefinito esplicito, NULL diventa il valore predefinito implicito della colonna.

Non specificare CASCADE se la tabella verrà inclusa in una pubblicazione di tipo merge che utilizza record logici. Per altre informazioni sui record logici, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

Non è possibile definire `ON DELETE CASCADE` se un trigger `INSTEAD OF``ON DELETE` esiste già nella tabella.

Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], ad esempio, la tabella **ProductVendor** ha una relazione referenziale con la tabella **Vendor**. La chiave esterna **ProductVendor.BusinessEntityID** fa riferimento alla chiave primaria **Vendor.BusinessEntityID**.

Se viene eseguita un'istruzione `DELETE` in una riga della tabella **Vendor** e viene specificata un'azione `ON DELETE CASCADE` per **ProductVendor.BusinessEntityID**, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica se esistono una o più righe dipendenti nella tabella **ProductVendor**. Le eventuali righe dipendenti individuate nella tabella **ProductVendor** vengono eliminate insieme alla riga a cui viene fatto riferimento nella tabella **Vendor**.

Viceversa, se si specifica `NO ACTION`, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore ed esegue il rollback dell'azione di eliminazione della riga nella tabella **Vendor** se almeno una riga della tabella **ProductVendor** vi fa riferimento.

ON UPDATE { **NO ACTION** \| CASCADE \ SET NULL \| SET DEFAULT } specifica quale azione viene eseguita sulle righe nella tabella modificata quando tali righe includono una relazione referenziale e la riga a cui viene fatto riferimento viene aggiornata nella tabella padre. Il valore predefinito è NO ACTION.

NO ACTION: [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore e viene eseguito il rollback dell'azione di aggiornamento della riga nella tabella padre.

CASCADE: le righe corrispondenti vengono aggiornate nella tabella di riferimento quando la riga viene aggiornata nella tabella padre.

SET NULL: tutti i valori che compongono la chiave esterna vengono impostati su NULL quando viene aggiornata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che le colonne chiave esterna ammettano valori Null.

SET DEFAULT: tutti i valori che compongono la chiave esterna vengono impostati sui rispettivi valori predefiniti quando viene aggiornata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che per tutte le colonne chiave esterna siano definiti valori predefiniti. Se una colonna ammette valori Null e non viene impostato un valore predefinito esplicito, NULL diventa il valore predefinito implicito della colonna.

Non specificare `CASCADE` se la tabella verrà inclusa in una pubblicazione di tipo merge che usa record logici. Per altre informazioni sui record logici, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

Non è possibile definire `ON UPDATE CASCADE`, `SET NULL` o `SET DEFAULT` se esiste già un trigger `INSTEAD OF``ON UPDATE` nella tabella in corso di modifica.

Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], ad esempio, la tabella **ProductVendor** ha una relazione referenziale con la tabella **Vendor**: La chiave esterna **ProductVendor.BusinessEntity** fa riferimento alla chiave primaria **Vendor.BusinessEntityID**.

Se in una riga della tabella **Vendor** viene eseguita un'istruzione UPDATE e si specifica un'azione ON UPDATE CASCADE per **ProductVendor.BusinessEntityID**, [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica se sono già presenti una o più righe dipendenti nella tabella **ProductVendor**. Le eventuali righe dipendenti individuate nella tabella **ProductVendor** vengono aggiornate insieme alla riga a cui viene fatto riferimento nella tabella **Vendor**.

Viceversa, se si specifica NO ACTION, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore ed esegue il rollback dell'azione di aggiornamento della riga nella tabella **Vendor** se almeno una riga della tabella **ProductVendor** vi fa riferimento.

CHECK è un vincolo che impone l'integrità di dominio tramite la limitazione dei valori che è possibile immettere in una o più colonne. È necessario contrassegnare come PERSISTED anche i vincoli CHECK nelle colonne calcolate.

*logical_expression* è un'espressione logica che restituisce TRUE o FALSE. L'espressione non può includere tipi di dati alias.

*column* è una colonna o un elenco di colonne, tra parentesi, usate nei vincoli di tabella per indicare le colonne specificate nella definizione del vincolo.

[ **ASC** | DESC ] specifica il tipo di ordinamento della colonna o delle colonne che fanno parte dei vincoli di tabella. Il valore predefinito è ASC.

*partition_scheme_name* è il nome dello schema di partizione che definisce i filegroup a cui verrà eseguito il mapping delle partizioni di una tabella partizionata. Lo schema di partizione deve essere presente nel database.

[ _partition\_column\_name_ **.** ] specifica la colonna rispetto alla quale verrà partizionata una tabella partizionata. La colonna deve corrispondere a quella specificata nella funzione di partizione usata da *partition_scheme_name* in termini di tipo di dati, lunghezza e precisione. Una colonna calcolata utilizzata in una funzione di partizione deve essere contrassegnata in modo esplicito come PERSISTED.

> [!IMPORTANT]
> Si consiglia di specificare NOT NULL sulla colonna di partizionamento di tabelle partizionate oppure di tabelle non partizionate che rappresentano origini o destinazioni di operazioni ALTER TABLE...SWITCH. In questo modo, i vincoli CHECK su colonne di partizionamento non devono verificare la presenza di valori Null.

WITH FILLFACTOR **=** _fillfactor_ specifica il livello di riempimento applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] per ogni pagina di indice usata per archiviare i dati dell'indice. I valori per *fillfactor* specificati dall'utente possono essere compresi tra 1 e 100. Se non viene specificato alcun valore, il valore predefinito è 0. I valori 0 e 100 relativi al fattore di riempimento sono equivalenti.

> [!IMPORTANT]
> WITH FILLFACTOR = *fillfactor* è documentata come unica opzione di indice per i vincoli PRIMARY KEY o UNIQUE solo per motivi di compatibilità con le versioni precedenti. Non sarà più documentata in questo senso nelle versioni future.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS è il nome del set di colonne. Un set di colonne è una rappresentazione XML non tipizzata che combina tutte le colonne di tipo sparse di una tabella in un output strutturato. Per altre informazioni sui set di colonne, vedere [Utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md).

PERIOD FOR SYSTEM_TIME (*system_start_time_column_name*, *system_end_time_column_name*) **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica i nomi delle colonne che il sistema userà per registrare il periodo di validità di un record. Usare questo argomento in combinazione con gli argomenti GENERATED ALWAYS AS ROW { START | END } e WITH SYSTEM_VERSIONING = ON per consentire il controllo delle versioni di sistema in una tabella. Per altre informazioni, vedere [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

COMPRESSION_DELAY **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Se è applicata l'ottimizzazione della memoria, il ritardo indica il numero minimo di minuti in cui una riga deve rimanere nella tabella, invariata, prima di essere idonea per la compressione nell'indice columnstore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seleziona le righe specifiche da comprimere in base all'ora dell'ultimo aggiornamento. Se, ad esempio, le righe cambiano frequentemente durante un periodo di due ore, è possibile impostare `COMPRESSION_DELAY = 120 Minutes` per assicurarsi che gli aggiornamenti vengano completati prima che SQL Server comprima la riga.

Per una tabella basata su disco, il ritardo indica il numero minimo di minuti in cui un rowgroup differenziale deve rimanere nello stato CLOSED nel rowgroup differenziale prima che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo comprima nel rowgroup compresso. Poiché le tabelle basate su disco non tengono traccia delle ore di inserimento e aggiornamento per le singole righe, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applica il ritardo ai rowgroup differenziali nello stato CLOSED.

Il valore predefinito è 0 minuti.

Per indicazioni su quando usare `COMPRESSION_DELAY`, vedere [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)

\< table_option> ::= Specifica una o più opzioni per la tabella.

DATA_COMPRESSION specifica l'opzione di compressione dei dati per la tabella, il numero di partizione o l'intervallo di partizioni specificato. descritte di seguito:

NONE: la tabella o le partizioni specificate non vengono compresse.

ROW: la tabella o le partizioni specificate vengono compresse usando la compressione di riga.

PAGE: la tabella o le partizioni specificate vengono compresse usando la compressione di pagina.

COLUMNSTORE

**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Si applica solo agli indici columnstore, inclusi gli indici columnstore cluster e quelli non cluster. COLUMNSTORE indica di usare la compressione del columnstore che offre le prestazioni migliori. Questa è la scelta tipica.

COLUMNSTORE_ARCHIVE **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Si applica solo agli indici columnstore, inclusi gli indici columnstore cluster e quelli non cluster. COLUMNSTORE_ARCHIVE comprimerà ulteriormente la tabella o la partizione a una dimensione inferiore. Può essere utilizzata per l'archiviazione o in altre situazioni in cui sono richieste dimensioni di archiviazione inferiori ed è possibile concedere più tempo per l'archiviazione e il recupero.

Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { `<partition_number_expression>` | [ **,** ...*n* ] **)** specifica le partizioni a cui si applica l'impostazione DATA_COMPRESSION. Se la tabella non è partizionata, l'argomento `ON PARTITIONS` genererà un errore. Se la clausola `ON PARTITIONS` non viene specificata, l'opzione `DATA_COMPRESSION` verrà applicata a tutte le partizioni di una tabella partizionata.

È possibile specificare *partition_number_expression* nei modi seguenti:

- Specificare il numero di una partizione, ad esempio: `ON PARTITIONS (2)`
- Specificare i numeri di partizione per più partizioni singole separati da virgole, ad esempio: `ON PARTITIONS (1, 5)`
- Specificare sia intervalli sia singole partizioni, ad esempio: `ON PARTITIONS (2, 4, 6 TO 8)`

È possibile specificare `<range>` sotto forma di numeri di partizione separati dalla parola TO, ad esempio: `ON PARTITIONS (6 TO 8)`.

Per impostare tipi diversi di compressione dei dati per partizioni diverse, specificare più volte l'opzione `DATA_COMPRESSION`, ad esempio:

```sql
WITH
(
    DATA_COMPRESSION = NONE ON PARTITIONS (1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
    DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
)
```

\<index_option> ::= Specifica una o più opzioni per l'indice. Per una descrizione completa di queste opzioni, vedere [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** }: se si specifica ON, la percentuale di spazio disponibile specificata da FILLFACTOR viene applicata alle pagine di livello intermedio dell'indice. Se si specifica OFF o se non si specifica un valore FILLFACTOR, le pagine di livello intermedio vengono riempite quasi fino alla capacità massima, lasciando spazio sufficiente per almeno una riga delle dimensioni massime consentite per l'indice, considerando il set di chiavi nelle pagine intermedie. Il valore predefinito è OFF.

FILLFACTOR **=** _fillfactor_ specifica una percentuale indicante il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la modifica dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Il valore predefinito è 0. I valori 0 e 100 relativi al fattore di riempimento sono equivalenti.

IGNORE_DUP_KEY = { ON | **OFF** } specifica la risposta all'errore quando un'operazione di inserimento tenta di inserire valori di chiave duplicati in un indice univoco. L'opzione IGNORE_DUP_KEY viene applicata solo alle operazioni di inserimento eseguite dopo la creazione o la ricompilazione dell'indice. L'opzione non ha alcun effetto se si esegue [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md). Il valore predefinito è OFF.

ON: quando vengono inseriti valori di chiave duplicati in un indice univoco, viene visualizzato un messaggio di avviso. Avranno esito negativo solo le righe che violano il vincolo di unicità.

OFF: quando vengono inseriti valori di chiave duplicati in un indice univoco, viene visualizzato un messaggio di errore. Viene eseguito il rollback dell'intera operazione INSERT.

L'opzione `IGNORE_DUP_KEY` non può essere impostata su ON per gli indici creati in una vista, negli indici non univoci, negli indici XML, spaziali e filtrati.

Per visualizzare `IGNORE_DUP_KEY`, usare [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

Nella sintassi compatibile con le versioni precedenti `WITH IGNORE_DUP_KEY` equivale a `WITH IGNORE_DUP_KEY = ON`.

STATISTICS_NORECOMPUTE **=** { ON | **OFF** }: se si specifica ON, le statistiche dell'indice non aggiornate non vengono ricalcolate automaticamente. Se si specifica OFF, viene abilitato l'aggiornamento automatico delle statistiche. Il valore predefinito è OFF.

ALLOW_ROW_LOCKS **=** { **ON** | OFF }: se si specifica ON, sono consentiti blocchi di riga per l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando usare blocchi di riga. Se si specifica OFF, i blocchi a livello di riga non vengono utilizzati. Il valore predefinito è ON.

ALLOW_PAGE_LOCKS **=** { **ON** | OFF }: se si specifica ON, sono consentiti blocchi di pagina per l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando utilizzare blocchi a livello di pagina. Se si specifica OFF, i blocchi a livello di pagina non vengono utilizzati. Il valore predefinito è ON.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** } **si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive. <BR>
Specifica se eseguire o meno l'ottimizzazione per la contesa di inserimento dell'ultima pagina. Il valore predefinito è OFF. Per altre informazioni, vedere le sezione [Chiavi sequenziali](./create-index-transact-sql.md#sequential-keys) della pagina CREATE INDEX.

FILETABLE_DIRECTORY = *directory_name*

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).

Specifica un nome di directory FileTable compatibile con Windows. Questo nome deve essere univoco tra tutti i nomi di directory FileTable nel database. Il confronto di univocità non supporta la distinzione tra maiuscole e minuscole, indipendentemente dalle impostazioni delle regole di confronto. Se questo valore non è specificato, viene utilizzato il nome della tabella FileTable.

FILETABLE_COLLATE_FILENAME = { *collation_name* | database_default }

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non supporta `FILETABLE`.

Specifica il nome delle regole di confronto da applicare alla colonna **Name** della tabella FileTable. Nelle regole di confronto specificate non deve essere applicata la distinzione tra maiuscole e minuscole ai fini della conformità con la semantica di denominazione dei file dei sistemi operativi Windows. Se questo valore non è specificato, vengono utilizzate le regole di confronto predefinite del database. Se nelle regole di confronto predefinite del database viene applicata la distinzione tra maiuscole e minuscole, viene generato un errore e l'operazione CREATE TABLE non riesce.

*collation_name* è il nome delle regole di confronto senza distinzione tra maiuscole e minuscole.

database_default specifica che è necessario usare le regole di confronto predefinite per il database. Nelle regole di confronto non deve essere applicata la distinzione tra maiuscole e minuscole.

FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = *constraint_name*
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).

Viene specificato il nome da usare per il vincolo di chiave primaria che viene creato automaticamente nella tabella FileTable. Se questo valore non è specificato, tramite il sistema viene generato un nome per il vincolo.

FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).

Specifica il nome da usare per il vincolo univoco che viene creato automaticamente nella colonna **stream_id** di FileTable. Se questo valore non è specificato, tramite il sistema viene generato un nome per il vincolo.

FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).

Specifica il nome da usare per il vincolo univoco che viene creato automaticamente per le colonne **parent_path_locator** e **name** di FileTable. Se questo valore non è specificato, tramite il sistema viene generato un nome per il vincolo.

SYSTEM_VERSIONING **=** ON [ ( HISTORY_TABLE **=** *schema_name* .*history_table_name* [, DATA_CONSISTENCY_CHECK **=** { **ON** | OFF } ] ) ] **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

Abilita il controllo delle versioni di sistema della tabella se vengono soddisfatti i requisiti relativi a tipo di dati, vincolo di chiave primaria e vincolo per il supporto dei valori Null. Se non si usa l'argomento `HISTORY_TABLE`, il sistema genera una nuova tabella di cronologia corrispondente allo schema della tabella corrente nello stesso filegroup della tabella corrente, creando un collegamento tra le due tabelle, e consente al sistema di registrare la cronologia di ogni record nella tabella corrente e nella tabella di cronologia. Il nome di questa tabella di cronologia sarà `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Per impostazione predefinita, la tabella di cronologia è **PAGE** compresso. Se si usa l'argomento `HISTORY_TABLE` per creare un collegamento e usare una tabella di cronologia esistente, il collegamento viene creato tra la tabella corrente e la tabella specificata. Se la tabella corrente è partizionata, la tabella di cronologia viene creata nel filegroup predefinito perché la configurazione del partizionamento non viene replicata automaticamente dalla tabella corrente nella tabella di cronologia. Se durante la creazione della tabella di cronologia viene specificato il nome di una tabella di cronologia, è necessario specificare il nome tabella e il nome schema. Quando si crea un collegamento a una tabella di cronologia esistente, è possibile scegliere di eseguire una verifica della coerenza dei dati. La coerenza dei dati viene controllata per garantire che i record esistenti non si sovrappongano. L'impostazione predefinita prevede l'esecuzione della verifica della coerenza dei dati. Usare questo argomento in combinazione con gli argomenti `PERIOD FOR SYSTEM_TIME` e `GENERATED ALWAYS AS ROW { START | END }` per abilitare il controllo delle versioni di sistema per una tabella. Per altre informazioni, vedere [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

REMOTE_DATA_ARCHIVE = { ON [ ( *table_stretch_options* [,...n] ) ] | OFF ( MIGRATION_STATE = PAUSED ) }   
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive).

Crea la nuova tabella con Stretch Database abilitato o disabilitato. Per ulteriori informazioni, vedere [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Abilitazione di Stretch Database per una tabella**

Quando si abilita Stretch per una tabella specificando `ON`, se necessario, è possibile specificare `MIGRATION_STATE = OUTBOUND` per iniziare subito la migrazione dei dati o `MIGRATION_STATE = PAUSED` per posticiparla. Il valore predefinito è `MIGRATION_STATE = OUTBOUND`. Per altre informazioni sull'abilitazione di Stretch per una tabella, vedere [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Prerequisiti**. Prima di abilitare Stretch per una tabella, è necessario abilitare la funzionalità nel server e nel database. Per ulteriori informazioni, vedere [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Autorizzazioni**. L'abilitazione di Stretch per un database o una tabella richiede autorizzazioni db_owner. L'abilitazione di Stretch per una tabella richiede anche autorizzazioni ALTER per la tabella.

[ FILTER_PREDICATE = { null | *predicate* } ] **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive).

Specifica facoltativamente un predicato di filtro per selezionare le righe di cui eseguire la migrazione da una tabella che contiene sia dati cronologici sia dati correnti. Il predicato deve eseguire la chiamata a una funzione inline con valori di tabella. Per altre informazioni, vedere [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) e [Selezionare le righe di cui eseguire la migrazione tramite una funzione di filtro](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Se si specifica un predicato del filtro inefficace, anche la migrazione dei dati risulterà inefficace. Stretch Database applica il predicato del filtro alla tabella usando l'operatore CROSS APPLY.

Se non si specifica un predicato del filtro, viene eseguita la migrazione dell'intera tabella.

Quando si specifica un predicato di filtro, è necessario specificare anche *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED } **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

- Specificare `OUTBOUND` per eseguire la migrazione da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
- Specificare `INBOUND` per copiare i dati remoti per la tabella da [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] di nuovo a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e per disabilitare Stretch per la tabella. Per altre informazioni, vedere [Disabilitare Stretch Database e ripristinare i dati remoti](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

   Questa operazione comporta costi di trasferimento dati e non può essere annullata.

- Specificare `PAUSED` per sospendere o posticipare la migrazione dei dati. Per altre informazioni, vedere [Sospendere e riprendere la migrazione dei dati - Stretch Database](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

[DATA_DELETION = ON  
    {( FILTER_COLUMN = column_name,   
           RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS } )}] **Si applica a:** *solo* SQL Edge di Azure

Abilita la pulizia basata sui criteri di conservazione dei dati non recenti o obsoleti dalle tabelle all'interno di un database. Per altre informazioni, vedere [Abilitare e disabilitare la conservazione dei dati](/azure/azure-sql-edge/data-retention-enable-disable). Per abilitare la conservazione dei dati, è necessario specificare i parametri seguenti. 

- FILTER_COLUMN = { column_name }  
Specifica la colonna che deve essere usata per determinare se le righe nella tabella sono obsolete o meno. Per la colonna di filtro sono consentiti i tipi di dati seguenti.
  - Data
  - Datetime
  - DateTime2
  - SmallDateTime
  - DateTimeOffset

- RETENTION_PERIOD = { INFINITE \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS }}       
  Specifica i criteri del periodo di conservazione per la tabella. Il periodo di conservazione viene specificato come combinazione di un valore intero positivo e dell'unità della parte della data.   

MEMORY_OPTIMIZED **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]). Istanza gestita di SQL di Azure non supporta tabelle ottimizzate per la memoria.

Il valore ON indica che la tabella è ottimizzata per la memoria. Le tabelle ottimizzate per la memoria fanno parte della funzionalità OLTP in memoria, che viene usata ottimizzare le prestazioni dell'elaborazione delle transazioni. Per un'introduzione a OLTP in memoria, vedere [Avvio rapido 1: Tecnologie OLTP in memoria per migliorare le prestazioni di Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md). Per informazioni più dettagliate sulle tabelle ottimizzate per la memoria, vedere [Tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md).

Il valore predefinito OFF indica che la tabella è basata su disco.

DURABILITY **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Il valore di `SCHEMA_AND_DATA` indica che la tabella è durevole, ovvero le modifiche sono persistenti sul disco e restano valide anche dopo il riavvio o il failover. SCHEMA_AND_DATA è il valore predefinito.

Il valore di `SCHEMA_ONLY` indica che la tabella non è durevole. Lo schema delle tabelle è persistente, ma tutti gli aggiornamenti dei dati non sono persistenti al riavvio o failover del database. L'opzione `DURABILITY = SCHEMA_ONLY` è consentita solo con `MEMORY_OPTIMIZED = ON`.

> [!WARNING]
> Quando si crea una tabella con **DURABILITY = SCHEMA_ONLY**, e successivamente si modifica **READ_COMMITTED_SNAPSHOT** usando **ALTER DATABASE**, i dati della tabella andranno perduti.

BUCKET_COUNT **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

Indica il numero di bucket che deve essere creato nell'indice hash. Il valore massimo per BUCKET_COUNT in indici hash è 1.073.741.824. Per altre informazioni sui numeri di bucket, vedere [Indici in tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

Bucket_count è un argomento obbligatorio.

INDEX **si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gli indici di tabella e di colonna possono essere specificati come parte dell'istruzione CREATE TABLE. Per informazioni dettagliate sull'aggiunta e la rimozione degli indici nelle tabelle ottimizzate per la memoria, vedere: [Modifica di tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

HASH **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indica che viene creato un indice HASH.

Gli indici hash sono supportati solo nelle tabelle ottimizzate per la memoria.

## <a name="remarks"></a>Osservazioni

Per altre informazioni sul numero di tabelle, colonne, vincoli e indici consentiti, vedere [Specifiche di capacità massima per SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md).

Lo spazio in tabelle e indici viene generalmente allocato con incrementi di un extent. Quando l'opzione `SET MIXED_PAGE_ALLOCATION` di `ALTER DATABASE` è impostata su TRUE, o sempre prima di [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se si crea una tabella o un indice, vengono allocate pagine da extent misti finché le pagine non sono sufficienti per riempire un extent uniforme. In seguito, viene allocato un altro extent ogni volta che gli extent allocati risultano pieni. Per visualizzare un report sulla quantità di spazio allocato e usato da una tabella, eseguire `sp_spaceused`.

[!INCLUDE[ssDE](../../includes/ssde-md.md)] non prevede l'applicazione di un ordine particolare per l'impostazione dei valori DEFAULT, IDENTITY, ROWGUIDCOL o dei vincoli di colonna in una definizione di colonna.

Quando viene creata una tabella, l'opzione QUOTED IDENTIFIER viene sempre archiviata con l'impostazione ON nei metadati della tabella, anche se l'opzione viene impostata su OFF quando si crea la tabella.

## <a name="temporary-tables"></a>Tabelle temporanee

È possibile creare tabelle temporanee locali e globali. Le tabelle temporanee locali sono visibili solo nella sessione corrente, mentre quelle globali sono visibili in tutte le sessioni. Non è possibile partizionare le tabelle temporanee.

Anteporre ai nomi delle tabelle temporanee locali un simbolo di cancelletto singolo (#*table_name*) e a quelli delle tabelle temporanee globali un simbolo di cancelletto doppio (##*table_name*).

Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] fanno riferimento alla tabella temporanea usando il valore specificato per *table_name* nell'istruzione `CREATE TABLE`, ad esempio:

```sql
CREATE TABLE #MyTempTable (
    col1 INT PRIMARY KEY
);

INSERT INTO #MyTempTable
VALUES (1);
```

Se si creano più tabelle temporanee all'interno di una sola stored procedure o di un singolo batch, è necessario che i nomi delle tabelle siano diversi.

Se si include *schema_name* quando si crea o accede a una tabella temporanea, viene ignorato. Tutte le tabelle temporanee vengono create nello schema dbo.

Se viene creata una tabella temporanea locale in una stored procedure o in un'applicazione che può essere eseguita contemporaneamente da più utenti, [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve essere in grado di distinguere le tabelle create dai vari utenti. A tale scopo, [!INCLUDE[ssDE](../../includes/ssde-md.md)] aggiunge internamente un suffisso numerico a ogni nome di tabella temporanea locale. Il nome completo di una tabella temporanea archiviato nella tabella **sysobjects** in **tempdb** è composto dal nome di tabella specificato nell'istruzione CREATE TABLE e dal suffisso numerico generato dal sistema. Per lasciare spazio per tale suffisso, il valore *table_name* specificato per un nome di tabella temporanea locale non può superare i 116 caratteri.

Le tabelle temporanee vengono eliminate automaticamente quando non sono più comprese nell'ambito, a meno di eliminarle in modo esplicito tramite l'istruzione DROP TABLE:

- Una tabella temporanea locale creata in una stored procedure viene eliminata automaticamente al termine della stored procedure. È possibile fare riferimento alla tabella da qualsiasi stored procedure nidificata eseguita dalla stored procedure con cui è stata creata la tabella. Non è invece possibile fare riferimento alla tabella dal processo che ha chiamato la stored procedure con cui è stata creata la tabella.
- Tutte le altre tabelle temporanee locali vengono eliminate automaticamente alla fine della sessione corrente.
- Le tabelle temporanee globali vengono eliminate automaticamente alla fine della sessione in cui è stata creata la tabella e quando tutte le altre attività non vi fanno più riferimento. L'associazione tra un'attività e una tabella viene mantenuta solo per la durata di una singola istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. Una tabella temporanea globale viene pertanto eliminata dopo il completamento dell'ultima istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che fa attivamente riferimento alla tabella alla fine della sessione di creazione.

Una tabella temporanea locale creata all'interno di una stored procedure o in un trigger può avere lo stesso nome di una tabella temporanea creata prima della chiamata alla stored procedure o al trigger. Se tuttavia una query fa riferimento a una tabella temporanea e sono disponibili due tabelle temporanee con lo stesso nome, non è possibile stabilire in base a quale tabella verrà risolta la query. Anche le stored procedure nidificate possono creare tabelle temporanee con lo stesso nome di una tabella temporanea creata dalla stored procedure chiamante. In questo caso, tuttavia, per fare in modo che le modifiche vengano applicate alla tabella creata nella stored procedure nidificata, è necessario che la struttura della tabella e i nomi di colonna siano identici a quelli della tabella creata nella procedura chiamante, come illustrato nell'esempio seguente.

```sql
CREATE PROCEDURE dbo.Test2
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (2);
    SELECT Test2Col = x FROM #t;
GO

CREATE PROCEDURE dbo.Test1
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (1);
    SELECT Test1Col = x FROM #t;
    EXEC Test2;
GO

CREATE TABLE #t(x INT PRIMARY KEY);
INSERT INTO #t VALUES (99);
GO

EXEC Test1;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
(1 row(s) affected)
Test1Col
-----------
1

(1 row(s) affected)
 Test2Col
 -----------
 2
 ```

Quando si creano tabelle temporanee locali o globali, la sintassi dell'istruzione `CREATE TABLE` supporta le definizioni di vincolo, ad eccezione dei vincoli FOREIGN KEY. Se si specifica un vincolo FOREIGN KEY in una tabella temporanea, l'istruzione restituisce un messaggio di avviso per segnalare che il vincolo è stato ignorato. La tabella viene comunque creata senza i vincoli FOREIGN KEY. Non è possibile fare riferimento a tabelle temporanee nei vincoli FOREIGN KEY.

Se una tabella temporanea viene creata con un vincolo denominato e all'interno dell'ambito di una transazione definita dall'utente, solo un utente alla volta può eseguire l'istruzione che crea la tabella temporanea. Se ad esempio una stored procedure crea una tabella temporanea con un vincolo di chiave primaria denominato, la stored procedure non può essere eseguita simultaneamente dai più utenti.

## <a name="database-scoped-global-temporary-tables-azure-sql-database"></a>Tabelle temporanee globali con ambito database (database SQL di Azure)

Le tabelle temporanee globali per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (avviate con ## table_name) sono archiviate in tempdb e condivise tra le sessioni di tutti gli utenti nell'intera istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per informazioni sui tipi di tabella SQL, vedere la sezione precedente sulla creazione delle tabelle.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] supporta le tabelle temporanee globali archiviate a loro volta in tempdb e con ambito a livello di database. Questo significa che le tabelle temporanee globali vengono condivise per le sessioni di tutti gli utenti all'interno dello stesso [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Le sessioni utente da altri database non possono accedere alle tabelle temporanee globali.

Le tabelle temporanee globali per il [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] seguono la stessa sintassi e la stessa semantica usate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le tabelle temporanee. Analogamente, le stored procedure temporanee globali hanno un ambito limitato a livello di database nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Anche le tabelle temporanee locali (avviate con # table_name) sono supportate per il [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e seguono la stessa sintassi e la stessa semantica usate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vedere la sezione precedente relativa alle [tabelle temporanee](#temporary-tables).  

> [!IMPORTANT]
> Questa funzionalità è disponibile per [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="troubleshooting-global-temporary-tables-for-azure-sql-database"></a>Risoluzione dei problemi delle tabelle temporanee globali per il database SQL di Azure

Per la risoluzione dei problemi relativi a tempdb, vedere [Come monitorare l'uso di tempdb](../../relational-databases/databases/tempdb-database.md#monitoring-tempdb-use).

> [!NOTE]
> Solo un amministratore del server può accedere alle DMV di risoluzione dei problemi in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="permissions"></a>Autorizzazioni

Qualsiasi utente può creare oggetti temporanei globali. Gli utenti possono accedere solo ai propri oggetti, a meno che non ottengano ulteriori autorizzazioni.

## <a name="partitioned-tables"></a>Tabelle partizionate

Prima di creare una tabella partizionata con CREATE TABLE, è necessario creare una funzione di partizione per specificare la modalità di partizionamento della tabella. Una funzione di partizione viene creata usando [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md). In secondo luogo, è necessario creare uno schema di partizione per specificare i filegroup di destinazione delle partizioni indicate dalla funzione di partizione. Uno schema di partizione viene creato usando [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). Per le tabelle partizionate non è possibile posizionare i vincoli PRIMARY KEY o UNIQUE in filegroup diversi. Per ulteriori informazioni, vedere [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

## <a name="primary-key-constraints"></a>Vincoli PRIMARY KEY

- In una tabella è possibile includere un solo vincolo PRIMARY KEY.
- Se l'indice viene generato da un vincolo PRIMARY KEY, nella tabella sarà possibile creare non più di 999 indici non cluster e di 1 indice cluster.
- Nel caso in cui per un vincolo PRIMARY KEY non si specifichi CLUSTERED né NONCLUSTERED, verrà utilizzato automaticamente il valore CLUSTERED se per i vincoli UNIQUE non sono specificati indici cluster.
- Tutte le colonne specificate in un vincolo PRIMARY KEY devono essere definite come NOT NULL. Se non si specifica il supporto di valori Null, per tutte le colonne coinvolte in un vincolo PRIMARY KEY viene impostato NOT NULL.

    > [!NOTE]
    > Per le tabelle ottimizzate per la memoria, è consentita la colonna chiave che ammette i valori Null.

- Se si definisce una chiave primaria in una colonna di tipo CLR definito dall'utente, è necessario che l'implementazione del tipo supporti l'ordinamento binario. Per altre informazioni, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="unique-constraints"></a>Vincoli UNIQUE

- Se per un vincolo UNIQUE non si specifica CLUSTERED né NONCLUSTERED, il valore predefinito è NONCLUSTERED.
- Ogni vincolo UNIQUE genera un indice. Il numero di vincoli UNIQUE non deve generare un numero di indici della tabella maggiore di 999, in caso di indici non cluster, e di 1, in caso di indici cluster.
- Se si definisce un vincolo UNIQUE in una colonna di tipo CLR definito dall'utente, è necessario che l'implementazione del tipo supporti l'ordinamento binario o basato su operatore. Per altre informazioni, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="foreign-key-constraints"></a>Vincoli FOREIGN KEY

- I valori diversi da NULL immessi nella colonna di un vincolo FOREIGN KEY devono essere presenti nella colonna a cui viene fatto riferimento. In caso contrario, viene restituito un messaggio di errore di violazione della chiave esterna.
- I vincoli FOREIGN KEY vengono applicati alla colonna precedente, a meno che non vengano specificate colonne di origine.
- I vincoli FOREIGN KEY possono fare riferimento solo a tabelle di un singolo database nello stesso server. L'integrità referenziale tra database diversi deve essere implementata tramite trigger. Per altre informazioni, vedere [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- I vincoli FOREIGN KEY possono fare riferimento a un'altra colonna nella stessa tabella. Questo tipo di vincolo viene definito autoreferenziale.
- La clausola REFERENCES di un vincolo FOREIGN KEY a livello di colonna può includere una sola colonna di riferimento. Il tipo di dati di tale colonna deve essere uguale al tipo di dati della colonna in cui viene definito il vincolo.
- La clausola REFERENCES di un vincolo FOREIGN KEY a livello di tabella deve includere lo stesso numero di colonne di riferimento di quelle presenti nell'elenco di colonne del vincolo. Il tipo di dati di ogni colonna di riferimento deve inoltre essere uguale a quello della colonna corrispondente nell'elenco di colonne. Le colonne di riferimento devono essere specificate nello stesso ordine usato quando si specificano le colonne del vincolo PRIMARY KEY o UNIQUE nella tabella a cui si fa riferimento.
- Non è possibile specificare CASCADE, SET NULL o SET DEFAULT se una colonna di tipo **timestamp** fa parte della chiave esterna o della chiave a cui si fa riferimento.
- È possibile combinare le azioni CASCADE, SET NULL, SET DEFAULT e NO ACTION in tabelle con relazioni referenziali reciproche. Se [!INCLUDE[ssDE](../../includes/ssde-md.md)] rileva l'azione NO ACTION, l'operazione viene arrestata e viene eseguito il rollback delle azioni CASCADE, SET NULL e SET DEFAULT correlate. Quando un'istruzione DELETE genera una combinazione di azioni CASCADE, SET NULL, SET DEFAULT e NO ACTION, tutte le azioni CASCADE, SET NULL e SET DEFAULT vengono applicate prima che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifichi l'esistenza di azioni NO ACTION.
- Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] non prevede un limite predefinito per il numero di vincoli FOREIGN KEY che possono essere inclusi in una tabella e che fanno riferimento ad altre tabelle o per il numero di vincoli FOREIGN KEY di proprietà di altre tabelle che fanno riferimento a una tabella specifica.

  Il numero effettivo di vincoli FOREIGN KEY che è possibile usare, tuttavia, è limitato dalla configurazione hardware e dalla progettazione del database e dell'applicazione. È consigliabile evitare che una tabella contenga più di 253 vincoli FOREIGN KEY e che più di 253 vincoli FOREIGN KEY facciano riferimento alla tabella stessa. Il limite effettivo potrebbe variare a seconda dell'applicazione e della configurazione hardware. Nella progettazione di database e applicazioni è opportuno valutare i costi correlati all'applicazione dei vincoli FOREIGN KEY.

- I vincoli FOREIGN KEY non vengono applicati nelle tabelle temporanee.
- I vincoli FOREIGN KEY possono fare riferimento solo alle colonne di vincoli PRIMARY KEY o UNIQUE della tabella a cui si fa riferimento o alle colonne in un indice univoco di tale tabella.
- Se si definisce una chiave esterna su una colonna di tipo CLR definito dall'utente, è necessario che l'implementazione del tipo supporti l'ordinamento binario. Per altre informazioni, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Le colonne incluse in una relazione di chiave esterna devono essere definite con la stessa lunghezza e la stessa scala.

## <a name="default-definitions"></a>definizioni DEFAULT

- Una colonna può contenere una sola definizione DEFAULT.
- Una definizione DEFAULT può includere valori costanti, funzioni, funzioni senza parametri standard SQL o valori NULL. Nella tabella seguente sono illustrati le funzioni senza parametri e i valori predefiniti corrispondenti restituiti durante un'istruzione INSERT.

    |Funzione senza parametri SQL-92|Valore restituito|
    |------------------------------|--------------------|
    |CURRENT_TIMESTAMP|Data e ora correnti.|
    |CURRENT_USER|Nome dell'utente che esegue un inserimento.|
    |SESSION_USER|Nome dell'utente che esegue un inserimento.|
    |SYSTEM_USER|Nome dell'utente che esegue un inserimento.|
    |USER|Nome dell'utente che esegue un inserimento.|
- *constant_expression* in una definizione DEFAULT non può fare riferimento a un'altra colonna della tabella o ad altre tabelle, viste o stored procedure.
- Non è possibile creare definizioni DEFAULT per colonne con tipo di dati **timestamp** o colonne con una proprietà IDENTITY.
- Non è possibile creare definizioni DEFAULT per colonne con tipo di dati alias se tale tipo di dati è associato a un oggetto predefinito.

## <a name="check-constraints"></a>Vincoli CHECK

- Una colonna può contenere un numero qualsiasi di vincoli CHECK e la condizione può includere più espressioni logiche unite tramite gli operatori AND e OR. Più vincoli CHECK per una colonna vengono convalidati nell'ordine di creazione.
- La condizione di ricerca deve restituire un'espressione booleana e non può fare riferimento a un'altra tabella.
- Un vincolo CHECK a livello di colonna può fare riferimento solo alla colonna vincolata, mentre un vincolo CHECK a livello di tabella può fare riferimento solo alle colonne della stessa tabella.

  Le regole e i vincoli CHECK svolgono la stessa funzione di convalida dei dati durante l'esecuzione delle istruzioni INSERT e UPDATE.

- Quando per una o più colonne sono definiti una regola e uno o più vincoli CHECK, vengono valutate tutte le restrizioni.
- Non è possibile definire vincoli CHECK per colonne di tipo **text**, **ntext** o **image**.

## <a name="additional-constraint-information"></a>Informazioni aggiuntive sui vincoli

- `DROP INDEX` non consente di eliminare un indice creato per un vincolo. Per eliminare il vincolo, è necessario usare l'istruzione `ALTER TABLE`. Un indice creato per e usato da un vincolo può essere ricompilato tramite `ALTER INDEX ... REBUILD`. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).
- I nomi di vincolo devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md), con l'eccezione che il nome non può iniziare con il simbolo di cancelletto (#). Se *constraint_name* viene omesso, al vincolo viene assegnato un nome generato dal sistema. Il nome del vincolo viene indicato nei messaggi di errore relativi alle violazioni di vincolo.
- Quando in un'istruzione `INSERT`, `UPDATE` o `DELETE` viene violato un vincolo, l'istruzione viene terminata. Con l'impostazione OFF per `SET XACT_ABORT`, tuttavia, la transazione continua a essere elaborata se l'istruzione fa parte di una transazione esplicita. Se l'impostazione di `SET XACT_ABORT` è ON, viene eseguito il rollback dell'intera transazione. È anche possibile usare l'istruzione `ROLLBACK TRANSACTION` con la definizione di transazione eseguendo un controllo con la funzione di sistema `@@ERROR`.
- Quando `ALLOW_ROW_LOCKS = ON` e `ALLOW_PAGE_LOCK = ON`, i blocchi a livello di riga, pagina e tabella sono consentiti quando si accede all'indice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] sceglie il blocco appropriato e può eseguire un'escalation del blocco da un blocco di riga o di pagina a un blocco di tabella. Se `ALLOW_ROW_LOCKS = OFF` e `ALLOW_PAGE_LOCK = OFF`, è consentito solo un blocco a livello di tabella quando si accede all'indice.
- Se una tabella include vincoli FOREIGN KEY o CHECK e trigger, le condizioni di vincolo vengono valutate prima dell'esecuzione del trigger.

Per visualizzare un report per una tabella e le relative colonne, usare `sp_help` o `sp_helpconstraint`. Per rinominare una tabella, usare `sp_rename`. Per visualizzare un report per le viste e le stored procedure che dipendono da una tabella, usare [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) e [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).

## <a name="nullability-rules-within-a-table-definition"></a>Regole per il supporto di valori Null all'interno di una definizione di tabella

L'impostazione del supporto di valori Null per una colonna determina se la colonna ammette o meno valori Null (NULL) come dati. NULL è diverso da zero o non è vuoto ma indica che non è stata immessa alcuna voce oppure che è stato specificato un valore NULL esplicito e in genere implica che il valore è sconosciuto o non applicabile.

Se si usa l'istruzione `CREATE TABLE` o `ALTER TABLE` per creare o modificare una tabella, le impostazioni del database e della sessione influenzano e talvolta sostituiscono il supporto dei valori Null del tipo di dati usato in una definizione di colonna. È consigliabile definire sempre in modo esplicito una colonna come NULL o NOT NULL per le colonne non calcolate oppure, se si utilizza un tipo di dati definito dall'utente, consentire nella colonna l'utilizzo dell'impostazione predefinita relativa al supporto di valori Null per tale tipo di dati. Le colonne di tipo sparse devono consentire sempre valori Null.

Se non si specifica in modo esplicito il supporto di valori Null per una colonna, saranno valide le regole indicate nella tabella seguente.

|Tipo di dati della colonna|Regola|
|----------------------|----------|
|Tipo di dati alias|[!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizza l'impostazione del supporto di valori Null specificata in fase di creazione del tipo di dati. Per determinare l'impostazione predefinita relativa al supporto di valori Null del tipo di dati, usare **sp_help**.|
|Tipo CLR definito dall'utente|Il supporto dei valori Null viene stabilito in base alla definizione della colonna.|
|Tipo di dati fornito dal sistema|Se il tipo di dati fornito dal sistema prevede una sola opzione, questa ha la precedenza. I tipi di dati **timestamp** devono essere NOT NULL. Quando sono presenti impostazioni di sessione specificate su ON tramite SET:<br />Con **ANSI_NULL_DFLT_ON** = ON viene assegnato il valore NULL. <br />Con **ANSI_NULL_DFLT_OFF** = ON viene assegnato il valore NOT NULL.<br /><br /> Quando sono presenti impostazioni del database configurate tramite ALTER DATABASE:<br />Con **ANSI_NULL_DEFAULT_ON** = ON viene assegnato il valore NULL. <br />Con **ANSI_NULL_DEFAULT_OFF** = ON viene assegnato il valore NOT NULL.<br /><br /> Per visualizzare l'impostazione del database per ANSI_NULL_DEFAULT, usare la vista del catalogo **sys.databases**|

Quando per la sessione non è impostata alcuna opzione ANSI_NULL_DFLT e per il database è impostato il valore predefinito (ANSI_NULL_DEFAULT è OFF), viene assegnato il valore predefinito NOT NULL.

Nel caso di una colonna calcolata, l'impostazione del supporto di valori Null viene sempre determinata automaticamente dal [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per individuare l'impostazione relativa al supporto dei valori Null per questo tipo di colonna, usare la funzione `COLUMNPROPERTY` con la proprietà **AllowsNull**.

> [!NOTE]
> Per entrambi i driver ODBC e OLE DB di SQL Server l'impostazione predefinita dell'opzione ANSI_NULL_DFLT_ON è ON. Gli utenti di ODBC e OLE DB possono configurare questa impostazione nelle origini dati ODBC oppure tramite le proprietà o gli attributi di connessione impostati dall'applicazione.

## <a name="data-compression"></a>Compressione dei dati

Le tabelle di sistema non possono essere abilitate per la compressione. Se non specificato diversamente, quando si crea una tabella la compressione dei dati è impostata su NONE. Se si specifica un elenco di partizioni o una partizione non compresa nell'intervallo, verrà generato un errore. Per altre informazioni sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

Per valutare il modo in cui la modifica dello stato di compressione influirà su una tabella, un indice o una partizione, usare la stored procedure [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

## <a name="permissions"></a>Autorizzazioni

Sono necessarie l'autorizzazione `CREATE TABLE` per il database e l'autorizzazione `ALTER` per lo schema in cui viene creata la tabella.

Se una o più colonne nell'istruzione `CREATE TABLE` sono definite con un tipo definito dall'utente, è necessaria l'autorizzazione `REFERENCES` per il tipo definito dall'utente.

Se una o più colonne nell'istruzione `CREATE TABLE` sono definite con un tipo CLR definito dall'utente, è necessario che l'utente sia il proprietario del tipo o abbia l'autorizzazione `REFERENCES`.

Se una o più colonne nell'istruzione `CREATE TABLE` sono associate una raccolta di XML Schema, è necessario che l'utente sia il proprietario della raccolta di XML Schema o abbia l'autorizzazione `REFERENCES`.

Qualsiasi utente può creare tabelle temporanee in tempdb.

## <a name="examples"></a>Esempi

### <a name="a-create-a-primary-key-constraint-on-a-column"></a>R. Creare un vincolo PRIMARY KEY in una colonna

Nell'esempio seguente viene illustrata la definizione di colonna per un vincolo PRIMARY KEY con un indice cluster nella colonna `EmployeeID` della tabella `Employee`. Poiché un nome di vincolo non viene specificato, ne viene fornito uno dal sistema.

```sql
CREATE TABLE dbo.Employee (
    EmployeeID INT PRIMARY KEY CLUSTERED
);
```

### <a name="b-using-foreign-key-constraints"></a>B. Utilizzo di vincoli FOREIGN KEY

Il vincolo FOREIGN KEY viene utilizzato per fare riferimento a un'altra tabella. Le chiavi esterne possono essere chiavi a colonna singola o a più colonne. Nell'esempio seguente viene illustrato un vincolo FOREIGN KEY a colonna singola nella tabella `SalesOrderHeader` che fa riferimento alla tabella `SalesPerson`. Per un vincolo FOREIGN KEY a colonna singola è sufficiente specificare solo la clausola REFERENCES.

```sql
SalesPersonID INT NULL REFERENCES SalesPerson(SalesPersonID)
```

È inoltre possibile usare la clausola FOREIGN KEY in modo esplicito per ridefinire l'attributo di colonna. Si noti che il nome della colonna non deve essere identico in entrambe le tabelle.

```sql
FOREIGN KEY (SalesPersonID) REFERENCES SalesPerson(SalesPersonID)
```

I vincoli con chiavi a più colonne vengono creati come vincoli di tabella. La tabella `SpecialOfferProduct` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] include un vincolo PRIMARY KEY a più colonne. Nell'esempio seguente viene illustrato come fare riferimento a questa chiave da un'altra tabella. Il nome di vincolo esplicito è facoltativo.

```sql
CONSTRAINT FK_SpecialOfferProduct_SalesOrderDetail
    FOREIGN KEY (ProductID, SpecialOfferID)
    REFERENCES SpecialOfferProduct (ProductID, SpecialOfferID)
```

### <a name="c-using-unique-constraints"></a>C. Utilizzo di vincoli UNIQUE

I vincoli UNIQUE vengono utilizzati per imporre l'univocità di colonne chiave non primaria. Nell'esempio seguente viene applicata una restrizione per specificare che la colonna `Name` della tabella `Product` deve essere univoca.

```sql
Name NVARCHAR(100) NOT NULL
UNIQUE NONCLUSTERED
```

### <a name="d-using-default-definitions"></a>D. Utilizzo di definizioni DEFAULT

I valori predefiniti forniscono un valore (tramite le istruzioni INSERT e UPDATE) nel caso in cui non ne viene specificato alcuno. Il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] potrebbe, ad esempio, includere una tabella di ricerca con un elenco dei vari ruoli professionali che possono essere assegnati ai dipendenti della società. Nella colonna destinata alla descrizione di ogni ruolo professionale, si potrebbe usare una stringa di caratteri predefinita per fornire una descrizione nel caso in cui questa non venga immessa in modo esplicito.

```sql
DEFAULT 'New Position - title not formalized yet'
```

Oltre alle costanti, le definizioni DEFAULT possono includere funzioni. Per ottenere la data corrente per una voce, è possibile usare l'esempio seguente:

```sql
DEFAULT (GETDATE())
```

È inoltre possibile migliorare l'integrità dei dati tramite un'analisi di funzioni senza parametri. Per tenere traccia dell'utente che ha inserito una riga, usare la funzione senza parametri per USER. Non racchiudere le funzioni senza parametri tra parentesi.

```sql
DEFAULT USER
```

### <a name="e-using-check-constraints"></a>E. Utilizzo di vincoli CHECK

Nell'esempio seguente viene illustrata l'applicazione di una restrizione ai valori immessi nella colonna `CreditRating` della tabella `Vendor`. Al vincolo non viene assegnato un nome.

```sql
CHECK (CreditRating >= 1 and CreditRating <= 5)
```

Nell'esempio seguente viene illustrato un vincolo denominato con una restrizione basata su modello per i dati di tipo carattere immessi in una colonna di una tabella.

```sql
CONSTRAINT CK_emp_id CHECK (
    emp_id LIKE '[A-Z][A-Z][A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
    OR emp_id LIKE '[A-Z]-[A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
)
```

Nell'esempio seguente viene specificato che i valori devono essere inclusi in un elenco specifico o essere conformi a un modello specificato.

```sql
CHECK (
    emp_id IN ('1389', '0736', '0877', '1622', '1756')
    OR emp_id LIKE '99[0-9][0-9]'
)
```

### <a name="f-showing-the-complete-table-definition"></a>F. Visualizzazione della definizione completa della tabella

Nell'esempio seguente vengono illustrate le definizioni di tabella complete con tutte le definizioni dei vincoli per la tabella `PurchaseOrderDetail` creata nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Per l'esecuzione dell'esempio, lo schema della tabella viene modificato in `dbo`.

```sql
CREATE TABLE dbo.PurchaseOrderDetail
(
    PurchaseOrderID int NOT NULL
        REFERENCES Purchasing.PurchaseOrderHeader(PurchaseOrderID),
    LineNumber smallint NOT NULL,
    ProductID int NULL
        REFERENCES Production.Product(ProductID),
    UnitPrice money NULL,
    OrderQty smallint NULL,
    ReceivedQty float NULL,
    RejectedQty float NULL,
    DueDate datetime NULL,
    rowguid uniqueidentifier ROWGUIDCOL NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_rowguid DEFAULT (NEWID()),
    ModifiedDate datetime NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_ModifiedDate DEFAULT (GETDATE()),
    LineTotal AS ((UnitPrice*OrderQty)),
    StockedQty AS ((ReceivedQty-RejectedQty)),
    CONSTRAINT PK_PurchaseOrderDetail_PurchaseOrderID_LineNumber
               PRIMARY KEY CLUSTERED (PurchaseOrderID, LineNumber)
               WITH (IGNORE_DUP_KEY = OFF)
)
ON PRIMARY;
```

### <a name="g-creating-a-table-with-an-xml-column-typed-to-an-xml-schema-collection"></a>G. Creazione di una tabella con una colonna xml tipizzata in una raccolta di XML Schema

Nell'esempio seguente viene creata una tabella con una colonna `xml` tipizzata nella raccolta di XML Schema `HRResumeSchemaCollection`. La parola chiave `DOCUMENT` specifica che ogni istanza del tipo di dati `xml` in *column_name* può contenere un solo elemento di livello superiore.

```sql
CREATE TABLE HumanResources.EmployeeResumes
(
    LName nvarchar(25),
    FName nvarchar(25),
    Resume xml(DOCUMENT HumanResources.HRResumeSchemaCollection)
);
```

### <a name="h-creating-a-partitioned-table"></a>H. Creazione di una tabella partizionata

Nell'esempio seguente viene creata una funzione di partizione per suddividere una tabella o indice in quattro partizioni. Viene quindi creato uno schema di partizione per specificare i filegroup in cui posizionare ognuna delle quattro partizioni. Infine viene creata una tabella che utilizza tale schema di partizione. Nell'esempio si presuppone che i filegroup esistano già nel database.

```sql
CREATE PARTITION FUNCTION myRangePF1 (int)
    AS RANGE LEFT FOR VALUES (1, 100, 1000);
GO

CREATE PARTITION SCHEME myRangePS1
    AS PARTITION myRangePF1
    TO (test1fg, test2fg, test3fg, test4fg);
GO  
  
CREATE TABLE PartitionTable (col1 int, col2 char(10))
    ON myRangePS1 (col1);
GO
```

Sulla base dei valori della colonna `col1` di `PartitionTable`, le partizioni vengono assegnate nei modi seguenti.

|Filegroup|test1fg|test2fg|test3fg|test4fg|
|---------------|-------------|-------------|-------------|-------------|
|**Partizione**|1|2|3|4|
|**Valori**|col 1 \<= 1|col1 > 1 AND col1 \<= 100|col1 > 100 AND col1 \<= 1,000|col1 > 1000|

### <a name="i-using-the-uniqueidentifier-data-type-in-a-column"></a>I. Utilizzo del tipo di dati uniqueidentifier in una colonna

Nell'esempio seguente viene creata una tabella con una colonna `uniqueidentifier`. Il vincolo PRIMARY KEY viene utilizzato per evitare che gli utenti inseriscano valori duplicati nella tabella, mentre la funzione `NEWSEQUENTIALID()` nel vincolo `DEFAULT` fornisce i valori per le nuove righe. La proprietà ROWGUIDCOL viene applicata alla colonna `uniqueidentifier` in modo che sia possibile farvi riferimento tramite la parola chiave $ROWGUID.

```sql
CREATE TABLE dbo.Globally_Unique_Data
(
    GUID UNIQUEIDENTIFIER
        CONSTRAINT Guid_Default DEFAULT
        NEWSEQUENTIALID() ROWGUIDCOL,
    Employee_Name VARCHAR(60)
    CONSTRAINT Guid_PK PRIMARY KEY (GUID)
);
```

### <a name="j-using-an-expression-for-a-computed-column"></a>J. Utilizzo di un'espressione per una colonna calcolata

Nell'esempio seguente viene illustrato l'utilizzo di un'espressione (`(low + high)/2`) per calcolare la colonna calcolata `myavg`.

```sql
CREATE TABLE dbo.mytable
(
    low INT,
    high INT,
    myavg AS (low + high)/2
);
```

### <a name="k-creating-a-computed-column-based-on-a-user-defined-type-column"></a>K. Creazione di una colonna calcolata basata su una colonna di tipo definito dall'utente

Nell'esempio seguente viene creata una tabella con una colonna di tipo definito dall'utente `utf8string` presupponendo che l'assembly del tipo e il tipo stesso siano già stati creati nel database corrente. Viene definita una seconda colonna basata su `utf8string` e viene usato il metodo `ToString()` di **type(class)** `utf8string` per calcolare un valore per la colonna.

```sql
CREATE TABLE UDTypeTable
(
    u UTF8STRING,
    ustr AS u.ToString() PERSISTED
);
```

### <a name="l-using-the-user_name-function-for-a-computed-column"></a>L. Utilizzo della funzione USER_NAME per una colonna calcolata

Nell'esempio seguente viene utilizzata la funzione `USER_NAME()` nella colonna `myuser_name`.

```sql
CREATE TABLE dbo.mylogintable
(
    date_in DATETIME,
    user_id INT,
    myuser_name AS USER_NAME()
);
```

### <a name="m-creating-a-table-that-has-a-filestream-column"></a>M. Creazione di una tabella con una colonna FILESTREAM

Nell'esempio seguente viene creata una tabella con una colonna `FILESTREAM``Photo`. Se in una tabella sono presenti una o più colonne `FILESTREAM`, tale tabella deve includere anche una colonna `ROWGUIDCOL`.

```sql
CREATE TABLE dbo.EmployeePhoto
(
    EmployeeId INT NOT NULL PRIMARY KEY,
    Photo VARBINARY(MAX) FILESTREAM NULL,
    MyRowGuidColumn UNIQUEIDENTIFIER NOT NULL ROWGUIDCOL UNIQUE DEFAULT NEWID()
);
```

### <a name="n-creating-a-table-that-uses-row-compression"></a>N. Creazione di una tabella che utilizza la compressione di riga

Nell'esempio seguente viene creata una tabella che utilizza la compressione di riga.

```sql
CREATE TABLE dbo.T1
(
    c1 INT,
    c2 NVARCHAR(200)
)
WITH (DATA_COMPRESSION = ROW);
```

Per altri esempi sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

### <a name="o-creating-a-table-that-has-sparse-columns-and-a-column-set"></a>O. Creazione di una tabella con colonne di tipo sparse e un set di colonne

Negli esempi seguenti viene illustrato come creare una tabella con una colonna di tipo sparse e una tabella con due colonne di tipo sparse e un set di colonne. Negli esempi viene utilizzata la sintassi di base. Per esempi più complessi, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md) e [Usare set di colonne](../../relational-databases/tables/use-column-sets.md).

Nell'esempio viene creata una tabella con una colonna di tipo sparse.

```sql
CREATE TABLE dbo.T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL
);
```

In questo esempio viene creata una tabella con due colonne di tipo sparse e un set di colonne denominato `CSet`.

```sql
CREATE TABLE T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL,
    c3 INT SPARSE NULL,
    CSet XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
);
```

### <a name="p-creating-a-system-versioned-disk-based-temporal-table"></a>P. Creazione di una tabella temporale basata su disco con controllo delle versioni di sistema

**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gli esempi seguenti spiegano come creare una tabella temporale collegata a una nuova tabella di cronologia e come creare una tabella temporale collegata a una tabella di cronologia esistente. Si noti che per la tabella temporale deve essere definita una chiave primaria affinché la tabella venga abilitata per il controllo delle versioni di sistema. Per esempi che illustrano come aggiungere o rimuovere il controllo delle versioni di sistema per una tabella esistente, vedere Controllo delle versioni di sistema in [Esempi](../../t-sql/statements/alter-table-transact-sql.md#Example_Top). Per i casi d'uso, vedere [Tabelle temporali](../../relational-databases/tables/temporal-tables.md).

In questo esempio viene creata una nuova tabella temporale collegata a una nuova tabella di cronologia.

```sql
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

In questo esempio viene creata una nuova tabella temporale collegata a una tabella di cronologia già esistente.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON));
```

### <a name="q-creating-a-system-versioned-memory-optimized-temporal-table"></a>Q. Creazione di una tabella temporale ottimizzata per la memoria con controllo delle versioni di sistema

**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

L'esempio seguente illustra come creare una tabella temporale ottimizzata per la memoria con controllo delle versioni di sistema collegata a una nuova tabella di cronologia basata su disco.

In questo esempio viene creata una nuova tabella temporale collegata a una nuova tabella di cronologia.

```sql
CREATE SCHEMA History;
GO

CREATE TABLE dbo.Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY NONCLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    MEMORY_OPTIMIZED = ON,
    DURABILITY = SCHEMA_AND_DATA,
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
```

In questo esempio viene creata una nuova tabella temporale collegata a una tabella di cronologia già esistente.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON)
);
```

### <a name="r-creating-a-table-with-encrypted-columns"></a>R. Creazione di una tabella con colonne crittografate

L'esempio seguente crea una tabella con due colonne crittografate. Per altre informazioni, vedere [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

```sql
CREATE TABLE Customers (
    CustName NVARCHAR(60)
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = RANDOMIZED,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    SSN VARCHAR(11) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = DETERMINISTIC ,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    Age INT NULL
);
```

### <a name="s-create-an-inline-filtered-index"></a>S. Creare un indice filtrato inline

Crea una tabella con un indice filtrato inline.

```sql
CREATE TABLE t1
(
    c1 INT,
    index IX1 (c1) WHERE c1 > 0
);
```

### <a name="t-create-an-inline-index"></a>T. Creare un indice inline

Di seguito viene illustrato come usare NONCLUSTERED inline per tabelle basate su disco:

```sql
CREATE TABLE t1
(
    c1 INT,
    INDEX ix_1 NONCLUSTERED (c1)
);

CREATE TABLE t2
(
    c1 INT,
    c2 INT INDEX ix_1 NONCLUSTERED
);

CREATE TABLE t3
(
    c1 INT,
    c2 INT,
    INDEX ix_1 NONCLUSTERED (c1,c2)
);
```

### <a name="u-create-a-temporary-table-with-an-anonymously-named-compound-primary-key"></a>U. Creare una tabella temporanea con una chiave primaria composta denominata in modo anonimo

Crea una tabella con una chiave primaria composta denominata in modo anonimo. Ciò è utile per evitare conflitti di runtime quando due tabelle temporanee con ambito sessione, ognuna in una sessione separata, usano lo stesso nome per un vincolo.

```sql
CREATE TABLE #tmp
(
    c1 INT,
    c2 INT,
    PRIMARY KEY CLUSTERED ([c1], [c2])
);
GO
```

Se si assegna in modo esplicito il vincolo, la seconda sessione genererà un errore, ad esempio:

```
Msg 2714, Level 16, State 5, Line 1
There is already an object named 'PK_#tmp' in the database.
Msg 1750, Level 16, State 1, Line 1
Could not create constraint or index. See previous errors.
```

Il problema sorge dal fatto che, mentre il nome della tabella temporanea è univoco, i nomi di vincolo non lo sono.

### <a name="v-using-global-temporary-tables-in-azure-sql-database"></a>V. Uso di tabelle temporanee globali nel database SQL di Azure

La sessione A crea una tabella temporanea globale ##test nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 e aggiunge 1 riga

```sql
CREATE TABLE ##test (
    a INT,
    b INT
);

INSERT INTO ##test
VALUES (1, 1);

-- Obtain object ID for temp table ##test
SELECT OBJECT_ID('tempdb.dbo.##test') AS 'Object ID';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
1253579504
```

Ottenere il nome di una tabella temporanea globale per un ID oggetto specifico (1253579504) in tempdb (2)

```sql
SELECT name FROM tempdb.sys.objects WHERE object_id = 1253579504;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
##test
```

La sessione B si connette al [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 e può accedere alla tabella ##test creata dalla sessione A

```sql
SELECT * FROM ##test;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
1, 1
```

La sessione C si connette a un altro database nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb2 e richiede l'accesso alla tabella ##test creata in testdb1. Questa selezione non riesce a causa dell'ambito del database per le tabelle temporanee globali

```sql
SELECT * FROM ##test
```

Viene generato l'errore seguente:

```
Msg 208, Level 16, State 0, Line 1
Invalid object name '##test'
```

Indirizzamento di un oggetto di sistema nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tempdb dal database utente corrente testdb1

```sql
SELECT * FROM tempdb.sys.objects;
SELECT * FROM tempdb.sys.columns;
SELECT * FROM tempdb.sys.database_files;
```   

### <a name="w-enable-data-retention-policy-on-a-table"></a>W. Abilitare i criteri di conservazione dei dati in una tabella

Nell'esempio seguente viene creata una tabella con la conservazione dei dati abilitata e un periodo di conservazione di 1 settimana. Questo esempio si applica solo a **SQL Edge di Azure**.

```sql
CREATE TABLE [dbo].[data_retention_table] 
(
  [dbdatetime2] datetime2(7), 
  [product_code] int, 
  [value] char(10)
) 
WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 WEEKS ))
```

## <a name="next-steps"></a>Passaggi successivi

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)
- [Tipi di dati](../../t-sql/data-types/data-types-transact-sql.md)
- [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)
- [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)
- [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)
- [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [sp_helpconstraint](../../relational-databases/system-stored-procedures/sp-helpconstraint-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
