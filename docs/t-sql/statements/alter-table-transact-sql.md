---
description: ALTER TABLE (Transact-SQL)
title: ALTER TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WAIT_AT_LOW_PRIORITY
- ABORT_AFTER_WAIT
- ABORT_AFTER_WAIT_TSQL
- ALTER_TABLE_TSQL
- ALTER TABLE
- WAIT_AT_LOW_PRIORITY_TSQL
- ALTER_COLUMN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- columns [SQL Server], resizing
- changing column size
- MAXDOP index option, ALTER TABLE statement
- table modifications [SQL Server], ALTER TABLE
- ALTER TABLE statement
- modifying tables
- partitioned tables [SQL Server], lock escalation
- resizing columns
- removing columns
- switching partitions
- reassigning partitions
- removing constraints
- triggers [SQL Server], disabling
- columns [SQL Server], adding
- LOCK_ESCALATION option of ALTER TABLE
- constraints [SQL Server], deleting
- constraints [SQL Server], disabling
- triggers [SQL Server], enabling
- re-enabling constraints
- index modifications [SQL Server]
- disabling constraints
- columns [SQL Server], removing
- max degree of parallelism option
- locking [SQL Server], tables
- ONLINE option
- disabling triggers
- constraints [SQL Server], adding
- deleting constraints
- adding constraints
- adding columns
- SWITCH partitions
- partitioned tables [SQL Server], switching
- lock escalation [SQL Server], option of ALTER TABLE
- constraints [SQL Server], enabling
- dropping constraints
- dropping columns
- data retention policy
- table changes [SQL Server]
ms.assetid: f1745145-182d-4301-a334-18f799d361d1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 155539807ce2a470d0a54b51db664425045c6954
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076383"
---
# <a name="alter-table-transact-sql"></a>ALTER TABLE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Modifica una definizione di tabella tramite la modifica, l'aggiunta o l'eliminazione di colonne e vincoli. ALTER TABLE consente inoltre di riassegnare e ricompilare partizioni, oltre a disabilitare e abilitare vincoli e trigger.

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!IMPORTANT]
> La sintassi di ALTER TABLE è diversa per le tabelle basate su disco e le tabelle ottimizzate per la memoria. Usare i collegamenti seguenti per passare direttamente al blocco di sintassi appropriata per i tipi di tabella e agli esempi di sintassi appropriata:
>
> - Tabelle basate su disco:
>
>   - [Sintassi](#syntax-for-disk-based-tables)
>   - [esempi](#Example_Top)
> - Tabelle ottimizzate per la memoria
>
>   - [Sintassi](#syntax-for-memory-optimized-tables)
>   - [esempi](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

## <a name="syntax-for-disk-based-tables"></a>Sintassi per le tabelle basate su disco

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                 | max
                 | xml_schema_collection
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ] [ SPARSE ]
      | { ADD | DROP }
          { ROWGUIDCOL | PERSISTED | NOT FOR REPLICATION | SPARSE | HIDDEN }
      | { ADD | DROP } MASKED [ WITH ( FUNCTION = ' mask_function ') ]
    }
    [ WITH ( ONLINE = ON | OFF ) ]
    | [ WITH { CHECK | NOCHECK } ]
  
    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <column_set_definition>
    } [ ,...n ]
      | [ system_start_time_column_name datetime2 GENERATED ALWAYS AS ROW START
                [ HIDDEN ] [ NOT NULL ] [ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
                system_end_time_column_name datetime2 GENERATED ALWAYS AS ROW END
                   [ HIDDEN ] [ NOT NULL ][ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
        ]
       PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )
    | DROP
     [ {
         [ CONSTRAINT ][ IF EXISTS ]
         {
              constraint_name
              [ WITH
               ( <drop_clustered_constraint_option> [ ,...n ] )
              ]
          } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }
  
    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | { ENABLE | DISABLE } CHANGE_TRACKING
        [ WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } ) ]
  
    | SWITCH [ PARTITION source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]

    | SET
        (
            [ FILESTREAM_ON =
                { partition_scheme_name | filegroup | "default" | "NULL" } ]
            | SYSTEM_VERSIONING =
                  {
                      OFF
                  | ON
                      [ ( HISTORY_TABLE = schema_name . history_table_name
                          [, DATA_CONSISTENCY_CHECK = { ON | OFF } ]
                          [, HISTORY_RETENTION_PERIOD =
                          {
                              INFINITE | number {DAY | DAYS | WEEK | WEEKS
                  | MONTH | MONTHS | YEAR | YEARS }
                          }
                          ]
                        )
                      ]
                  }
            | DATA_DELETION =  
                {
                      OFF 
                    | ON  
                        [(  [ FILTER_COLUMN = column_name ]   
                            [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                                    | MONTH | MONTHS | YEAR | YEARS }}]   
                        )]
                   }
    | REBUILD
      [ [PARTITION = ALL]
        [ WITH ( <rebuild_option> [ ,...n ] ) ]
      | [ PARTITION = partition_number
           [ WITH ( <single_partition_rebuild_option> [ ,...n ] ) ]
        ]
      ]
  
    | <table_option>
    | <filetable_option>
    | <stretch_configuration>
}
[ ; ]
  
-- ALTER TABLE options
  
<column_set_definition> ::=
    column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS

<drop_clustered_constraint_option> ::=
    {
        MAXDOP = max_degree_of_parallelism
      | ONLINE = { ON | OFF }
      | MOVE TO
         { partition_scheme_name ( column_name ) | filegroup | "default" }
    }
<table_option> ::=
    {
        SET ( LOCK_ESCALATION = { AUTO | TABLE | DISABLE } )
    }
  
<filetable_option> ::=
    {
       [ { ENABLE | DISABLE } FILETABLE_NAMESPACE ]
       [ SET ( FILETABLE_DIRECTORY = directory_name ) ]
    }
  
<stretch_configuration> ::=
    {
      SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options>)
          | = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED )
          | ( <table_stretch_options> [, ...n] )
        }
            )
    }
  
<table_stretch_options> ::=
    {
     [ FILTER_PREDICATE = { null | table_predicate_function } , ]
       MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
    }
  
<single_partition_rebuild__option> ::=
{
      SORT_IN_TEMPDB = { ON | OFF }
    | MAXDOP = max_degree_of_parallelism
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE} }
    | ONLINE = { ON [( <low_priority_lock_wait> ) ] | OFF }
}
  
<low_priority_lock_wait>::=
{
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ],
        ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )
}
```

> [!NOTE]
> Per altre informazioni, vedere:
>
> - [ALTER TABLE column_constraint](alter-table-column-constraint-transact-sql.md)
> - [ALTER TABLE column_definition](alter-table-column-definition-transact-sql.md)
> - [ALTER TABLE computed_column_definition](alter-table-computed-column-definition-transact-sql.md)
> - [ALTER TABLE index_option](alter-table-index-option-transact-sql.md)
> - [ALTER TABLE table_constraints](alter-table-table-constraint-transact-sql.md)

## <a name="syntax-for-memory-optimized-tables"></a>Sintassi per le tabelle con ottimizzazione per la memoria

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ]
    }

    | ALTER INDEX index_name
    {
        [ type_schema_name. ] type_name
        REBUILD
        [ [ NONCLUSTERED ] WITH ( BUCKET_COUNT = bucket_count )
        ]
    }

    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <table_index>
      | <column_index>
    } [ ,...n ]
  
    | DROP
     [ {
         CONSTRAINT [ IF EXISTS ]
         {
              constraint_name
          } [ ,...n ]
        | INDEX [ IF EXISTS ]
      {
         index_name
       } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }

    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | SWITCH [ [ PARTITION ] source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]
    
}
[ ; ]

-- ALTER TABLE options

< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   {PRIMARY KEY | UNIQUE }
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
{[ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
}

```

## <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Sintassi per Azure Synapse Analytics e Parallel Data Warehouse

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

ALTER TABLE { database_name.schema_name.source_table_name | schema_name.source_table_name | source_table_name }
{
    ALTER COLUMN column_name
        {
            type_name [ ( precision [ , scale ] ) ]
            [ COLLATE Windows_collation_name ]
            [ NULL | NOT NULL ]
        }
    | ADD { <column_definition> | <column_constraint> FOR column_name} [ ,...n ]
    | DROP { COLUMN column_name | [CONSTRAINT] constraint_name } [ ,...n ]
    | REBUILD {
            [ PARTITION = ALL [ WITH ( <rebuild_option> ) ] ]
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_option> ] ]
      }
    | { SPLIT | MERGE } RANGE (boundary_value)
    | SWITCH [ PARTITION source_partition_number
        TO target_table_name [ PARTITION target_partition_number ] [ WITH ( TRUNCATE_TARGET = ON | OFF )
}
[;]

<column_definition>::=
{
    column_name
    type_name [ ( precision [ , scale ] ) ]
    [ <column_constraint> ]
    [ COLLATE Windows_collation_name ]
    [ NULL | NOT NULL ]
}

<column_constraint>::=
    [ CONSTRAINT constraint_name ] 
    {
        DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }
<rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]
}

<single_partition_rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*database_name*  
Nome del database in cui è stata creata la tabella.

*schema_name*  
Nome del processo a cui appartiene la tabella.

*table_name*  
Nome della tabella da modificare. Se la tabella non è inclusa nel database corrente o nello schema di proprietà dell'utente corrente, è necessario specificare in modo esplicito il database e lo schema.

ALTER COLUMN  
Specifica che la colonna denominata deve essere cambiata o modificata.

Non è consentita la modifica di queste colonne:

- Colonna con tipo di dati **timestamp**.
- Colonna ROWGUIDCOL della tabella.
- Colonne calcolate o utilizzate in una colonna calcolata.
- Colonne usate in statistiche generate dall'istruzione CREATE STATISTICS. Gli utenti devono eseguire DROP STATISTICS per eliminare le statistiche prima di poter completare ALTER COLUMN con esito positivo.  Eseguire questa query per ottenere tutte le statistiche e le colonne delle statistiche create dall'utente per una tabella.

``` sql

SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<table_name>'); 

```
   > [!NOTE]
   > Le statistiche generate in modo automatico da Query Optimizer vengono eliminate automaticamente da ALTER COLUMN.

- Colonne utilizzate in un vincolo PRIMARY KEY o [FOREIGN KEY] REFERENCES.
- Colonne utilizzate in un vincolo CHECK o UNIQUE. È in ogni caso possibile modificare la lunghezza di una colonna a lunghezza variabile usata in un vincolo CHECK o UNIQUE.
- Colonne associate a una definizione DEFAULT. Se il tipo di dati non viene modificato, è tuttavia possibile modificare la lunghezza, la precisione o la scala di una colonna.

Il tipo di dati di colonne **text**, **ntext** e **image** può essere modificato solo nei modi seguenti:

- Da **text** a **varchar(max)** , **nvarchar(max)** o **xml**
- Da **ntext** a **varchar(max)** , **nvarchar(max)** o **xml**
- Da **image** a **varbinary(max)**

Alcune modifiche del tipo di dati possono comportare la modifica dei dati. Ad esempio, la modifica di una colonna **nchar** o **nvarchar** in **char** o **varchar** potrebbe causare la conversione di caratteri estesi. Per altre informazioni, vedere [CAST e CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). La riduzione della precisione o della scala di una colonna può causare il troncamento dei dati.

> [!NOTE]
> Non è possibile modificare il tipo di dati di una colonna di una tabella partizionata.
>
> Il tipo di dati delle colonne in un indice non può essere modificato. Fanno eccezione le colonne per cui il tipo di dati è **varchar**, **nvarchar** o **varbinary** e le nuove dimensioni sono uguali o maggiori di quelle precedenti.
>
> Le colonne incluse in un vincolo di chiave primaria non possono essere modificate da **NOT NULL** a **NULL**.

Quando si usa Always Encrypted (senza enclave sicuri), se la colonna da modificare è crittografata con "ENCRYPTED WITH", è possibile modificare il tipo di dati in un tipo di dati compatibile, ad esempio da INT a BIGINT, ma non è possibile modificarne le impostazioni di crittografia.

Quando si usa Always Encrypted con enclave sicuri, è possibile modificare qualsiasi impostazione di crittografia, se la chiave di crittografia che protegge la colonna (e la nuova chiave di crittografia della colonna, se si modifica la chiave) supporta i calcoli dell'enclave (crittografati con le chiavi master della colonna abilitate per le enclave). Per informazioni dettagliate, vedere [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*column_name*  
Nome della colonna da modificare, aggiungere o eliminare. *column_name* può essere composto da un massimo di 128 caratteri. Nel caso di nuove colonne, è possibile omettere *column_name* per le colonne che sono state create con il tipo di dati **timestamp**. Viene usato il nome **timestamp** se non si specifica il nome *column_name* per una colonna con il tipo di dati **timestamp**.

> [!NOTE]
> Le nuove colonne vengono aggiunte dopo la modifica di tutte le colonne esistenti nella tabella.

[ _type\_schema\_name_ **.** ] _type\_name_  
Nuovo tipo di dati per la colonna modificata o tipo di dati per la colonna aggiunta. Non è possibile specificare *type_name* per colonne esistenti di tabelle partizionate. *type_name* può essere uno dei tipi seguenti:

- Tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Tipo di dati alias basato su un tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per consentirne l'uso in una definizione di tabella, si creano tipi di dati alias con l'istruzione CREATE TYPE.
- Tipo definito dall'utente (UDT) di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] e lo schema a cui appartiene. Per consentirne l'uso in una definizione di tabella, si creano tipi definiti dall'utente con l'istruzione CREATE TYPE.

Di seguito sono riportati i criteri per *type_name* di una colonna modificata:

- Il tipo di dati precedente deve supportare la conversione implicita nel nuovo tipo di dati.
- *type_name* non può essere **timestamp**.
- I valori predefiniti di ANSI_NULL sono sempre attivi per ALTER COLUMN. Se non diversamente specificato, la colonna ammette i valori Null.
- Il riempimento con ANSI_PADDING è sempre attivo per ALTER COLUMN.
- Se la colonna modificata è una colonna Identity, il tipo di dati di *new_data_type* deve supportare la proprietà Identity.
- L'impostazione corrente di SET ARITHABORT viene ignorata. Il funzionamento di ALTER TABLE presume l'impostazione di ARITHABORT su ON.

> [!NOTE]
> Se la clausola COLLATE è omessa, la modifica del tipo di dati di una colonna causa la modifica delle regole di confronto predefinite del database.

*precisione*  
Precisione del tipo di dati specificato. Per altre informazioni sui valori di precisione validi, vedere [Precisione, scala e lunghezza](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale*  
Scala per il tipo di dati specificato. Per altre informazioni sui valori di scala validi, vedere [Precisione, scala e lunghezza](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max**  
Viene applicato solo ai tipi di dati **varchar**, **nvarchar** e **varbinary** per l'archiviazione di 2^31-1 byte di dati di tipo carattere, binario e Unicode.

*xml_schema_collection*  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Viene applicato solo al tipo di dati **xml** per l'associazione di uno XML Schema al tipo. Prima di tipizzare una colonna **xml** in una raccolta di schemi, si crea la raccolta nel database usando [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

COLLATE \< *collation_name* >  
Specifica le nuove regole di confronto per la colonna modificata. Se viene omesso, alla colonna vengono assegnate le regole di confronto predefinite del database. È possibile usare nomi di regole di confronto di Windows o SQL. Per un elenco e altre informazioni, vedere [Nome delle regole di confronto di Windows](../../t-sql/statements/windows-collation-name-transact-sql.md) e [Nome delle regole di confronto di SQL Server](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

La clausola COLLATE modifica le regole di confronto solo per le colonne con tipo di dati **char**, **varchar**, **nchar** e **nvarchar**. Per modificare le regole di confronto di una colonna con un tipo di dati alias definito dall'utente, usare istruzioni ALTER TABLE separate in modo da modificare il tipo di dati della colonna in un tipo di dati di sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Modificare quindi le regole di confronto e ripristinare un tipo di dati alias per la colonna.

Non è possibile specificare una modifica delle regole di confronto per ALTER COLUMN se si verifica una delle condizioni seguenti:

- Un vincolo CHECK o FOREIGN KEY o una colonna calcolata fa riferimento alla colonna modificata.
- Nella colonna viene creato un indice, un indice full-text o una serie di statistiche. Le statistiche create automaticamente nella colonna modificata vengono eliminate se si modificano le regole di confronto della colonna.
- Una funzione o una vista associata allo schema fa riferimento alla colonna.

Per altre informazioni, vedere [COLLATE](~/t-sql/statements/collations.md).

NULL | NOT NULL  
Specifica se la colonna consente valori Null. L'istruzione ALTER TABLE consente di aggiungere colonne che non consentono valori Null solo se alle colonne è associato un valore predefinito oppure se la tabella è vuota. È possibile specificare NOT NULL per le colonne calcolate solo se è specificato anche PERSISTED. Le nuove colonne che consentono valori Null ma a cui non è associato alcun valore predefinito contengono un valore Null per ogni riga della tabella. Se a una nuova colonna che consente valori Null si aggiunge una definizione DEFAULT, è possibile usare WITH VALUES per l'archiviazione del valore predefinito nella nuova colonna per ogni riga della tabella.

Se la nuova colonna non consente valori Null e la tabella non è vuota, è necessario aggiungervi una definizione DEFAULT. Il valore predefinito viene quindi caricato automaticamente in ogni riga esistente delle nuove colonne.

È possibile specificare NULL in ALTER COLUMN per forzare l'uso di valori Null nelle colonne NOT NULL, fatta eccezione per le colonne nei vincoli PRIMARY KEY. È possibile specificare NOT NULL in ALTER COLUMN solo se la colonna non contiene valori Null. Per utilizzare ALTER COLUMN NOT NULL, è necessario aggiornare i valori Null con un valore specifico, ad esempio:

```sql
UPDATE MyTable SET NullCol = N'some_value' WHERE NullCol IS NULL;
ALTER TABLE MyTable ALTER COLUMN NullCOl NVARCHAR(20) NOT NULL;
```

Quando si crea o si modifica una tabella mediante un'istruzione CREATE TABLE o ALTER TABLE, le impostazioni del database e della sessione influiscono sull'impostazione che consente l'uso dei valori Null del tipo di dati usato in una definizione di colonna. In questo caso, tale impostazione può essere sostituita. Nel caso di colonne non calcolate, assicurarsi di definire sempre in modo esplicito una colonna come NULL o NOT NULL.

Se si aggiunge una colonna con un tipo di dati definito dall'utente, assicurarsi di definire per la colonna la stessa impostazione relativa al supporto dei valori Null del tipo di dati definito dall'utente. Specificare quindi un valore predefinito per la colonna. Per altre informazioni, vedere [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).

> [!NOTE]
> Se si specifica NULL o NOT NULL con ALTER COLUMN, è necessario specificare anche *new_data_type* [(*precision* [, *scale* ])]. Se il tipo di dati, la precisione e la scala non vengono modificati, specificare i valori correnti della colonna.

[ {ADD | DROP} ROWGUIDCOL ]  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica l'aggiunta o l'eliminazione della proprietà ROWGUIDCOL nella colonna specificata. ROWGUIDCOL indica che la colonna è di tipo rowguid. È possibile impostare come colonna ROWGUIDCOL una sola colonna di tipo **uniqueidentifier** per ogni tabella. È inoltre possibile assegnare la proprietà ROWGUIDCOL solo a una colonna **uniqueidentifier**. Non è possibile assegnare ROWGUIDCOL a una colonna con un tipo di dati definito dall'utente.

ROWGUIDCOL non impone l'unicità dei valori archiviati nella colonna e non genera automaticamente valori per le nuove righe inserite nella tabella. Per generare valori univoci per ogni colonna, usare la funzione NEWID o NEWSEQUENTIALID nelle istruzioni INSERT. In alternativa, specificare la funzione NEWID o NEWSEQUENTIALID come valore predefinito per la colonna.

[ {ADD | DROP} PERSISTED ]  
Specifica l'aggiunta o l'eliminazione della proprietà PERSISTED nella colonna specificata. La colonna interessata deve essere una colonna calcolata definita con un'espressione deterministica. Per le colonne specificate come PERSISTED, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] sono archiviati fisicamente i valori calcolati nella tabella e aggiornati i valori durante l'aggiornamento delle altre colonne da cui le colonne calcolate dipendono. Se si contrassegna una colonna calcolata come PERSISTED, è possibile creare indici in colonne calcolate definite in base a espressioni deterministiche ma imprecise. Per altre informazioni, vedere [Indici per le colonne calcolate](../../relational-databases/indexes/indexes-on-computed-columns.md).

Tutte le colonne calcolate usate come colonne di partizionamento di tabelle partizionate devono essere contrassegnate come PERSISTED in modo esplicito.

DROP NOT FOR REPLICATION  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica che i valori vengono incrementati nelle colonne Identity quando gli agenti di replica eseguono operazioni di inserimento. È possibile specificare questa clausola solo se *column_name* corrisponde a una colonna Identity.

SPARSE  
Indica che la colonna è di tipo sparse. L'archiviazione delle colonne di tipo sparse è ottimizzata per valori Null. Non è possibile impostare le colonne di tipo sparse come NOT NULL. La conversione di una colonna di tipo sparse in una non di tipo sparse o viceversa provoca il blocco della tabella per la durata dell'esecuzione del comando. Potrebbe essere necessario utilizzare la clausola REBUILD per recuperare spazio. Per altre restrizioni e informazioni relative alle colonne di tipo sparse, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).

ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica una maschera dati dinamica. *mask_function* è il nome della funzione di maschera con i parametri appropriati. Sono disponibili tre funzioni:

- default()
- email()
- partial()
- random()

Per eliminare una maschera, usare `DROP MASKED`. Per i parametri di funzione, vedere [Mascheramento dati dinamici](../../relational-databases/security/dynamic-data-masking.md).

WITH ( ONLINE = ON | OFF) \<as applies to altering a column>  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Consente l'esecuzione di molte azioni di modifica colonna mentre la tabella rimane disponibile. L'impostazione predefinita è OFF. È possibile eseguire l'operazione di modifica colonna online per le modifiche relative al tipo di dati, alla lunghezza o precisione della colonna, al supporto dei valori Null, all'impostazione del tipo sparse e alle regole di confronto.

L'operazione di modifica colonna online consente alle statistiche automatiche o create dall'utente di fare riferimento alla colonna modificata per la durata dell'operazione ALTER COLUMN, il che consente di eseguire le query come al solito. Al termine dell'operazione, le statistiche automatiche che fanno riferimento alla colonna vengono eliminate e le statistiche create dall'utente vengono invalidate. L'utente deve aggiornare manualmente le statistiche generate dall'utente dopo il completamento dell'operazione. Se la colonna fa parte di un'espressione filtro per statistiche o indici, non è possibile eseguire un'operazione di modifica colonna.

- Durante l'esecuzione di un'operazione di modifica colonna online, tutte le operazioni che potrebbero dipendere dalla colonna (indici, viste e così via) vengono bloccate o hanno esito negativo generando un errore appropriato. Questo comportamento garantisce che l'operazione di modifica colonna online non avrà esito negativo a causa delle dipendenze introdotte durante l'esecuzione dell'operazione.
- La modifica di una colonna da NOT NULL a NULL non è supportata come operazione online quando gli indici non cluster fanno riferimento alla colonna modificata.
- La modifica online non è supportata quando un vincolo CHECK fa riferimento alla colonna e l'operazione di modifica limita la precisione della colonna (valori numerici o datetime).
- L'opzione `WAIT_AT_LOW_PRIORITY` non può essere usata con l'operazione di modifica colonna online.
- `ALTER COLUMN ... ADD/DROP PERSISTED` non è supportato per l'operazione di modifica colonna online.
- `ALTER COLUMN ... ADD/DROP ROWGUIDCOL/NOT FOR REPLICATION` non è interessato dall'operazione di modifica colonna online.
- L'operazione di modifica colonna online non supporta la modifica di una tabella in cui è abilitato il rilevamento modifiche o che è una tabella di pubblicazione della replica di tipo merge.
- L'operazione di modifica colonna online non supporta la modifica dai tipi di dati CLR o viceversa.
- L'operazione di modifica colonna online non supporta la modifica in un tipo di dati XML con una raccolta di schemi diversa dalla raccolta di schemi corrente.
- L'operazione di modifica colonna online non consente di ridurre le restrizioni che stabiliscono quando una colonna può essere modificata. Riferimenti da indici/statistiche e così via potrebbero causare l'esito negativo dell'operazione di modifica.
- L'operazione di modifica colonna online non supporta la modifica di più colonne contemporaneamente.
- L'operazione di modifica colonna online non influisce in caso di una tabella temporale con controllo delle versioni di sistema. La colonna ALTER non viene eseguita online indipendentemente dal valore che è stato specificato per l'opzione ONLINE.

L'operazione di modifica colonna online prevede requisiti, restrizioni e funzionalità simili a quelli dell'operazione di ricompilazione indice online, ad esempio:

- L'operazione di ricompilazione indice online non è supportata quando la tabella contiene colonne LOB o filestream legacy oppure quando la tabella include un indice columnstore. Le stesse limitazioni si applicano all'operazione di modifica colonna online.
- La modifica di una colonna esistente richiede un'allocazione dello spazio doppia: per la colonna originale e per la colonna nascosta appena creata.
- La strategia di blocco durante un'operazione di modifica colonna online segue lo stesso criterio di blocco usato per l'operazione di compilazione indice online.

WITH CHECK | WITH NOCHECK  
Specifica se i dati nella tabella vengono convalidati in base a un vincolo FOREIGN KEY o CHECK nuovo o riabilitato. Se non lo si specifica, viene usata la clausola WITH CHECK per nuovi vincoli e WITH NOCHECK per vincoli riabilitati.

Se non si vogliono verificare nuovi vincoli CHECK o FOREIGN KEY in base ai dati esistenti, usare WITH NOCHECK. È tuttavia consigliabile effettuare questa scelta solo in casi rari. Il nuovo vincolo viene valutato in tutti gli aggiornamenti successivi dei dati. Le eventuali violazioni del vincolo soppresse da WITH NOCHECK quando si aggiunge il vincolo possono causare il mancato completamento dei successivi aggiornamenti di righe contenenti dati che non seguono il vincolo.

> [!NOTE]
> Query Optimizer non considera i vincoli definiti con WITH NOCHECK, i quali vengono ignorati finché non vengono riabilitati mediante `ALTER TABLE table WITH CHECK CHECK CONSTRAINT ALL`.

ALTER INDEX *index_name*  
Specifica che il numero di bucket per *index_name* deve essere cambiato o modificato.

La sintassi ALTER TABLE … ADD/DROP/ALTER INDEX è supportata solo per le tabelle ottimizzate per la memoria.

> [!IMPORTANT]
> Se non si usa l'istruzione ALTER TABLE, le istruzioni [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) e [PAD_INDEX](alter-table-index-option-transact-sql.md) non sono supportate per gli indici nelle tabelle ottimizzate per la memoria.

ADD  
Specifica l'aggiunta di una o più definizioni di colonna, definizioni di colonna calcolata o vincoli di tabella. In alternativa, specifica l'aggiunta delle colonne che il sistema usa per il controllo delle versioni di sistema. Per le tabelle ottimizzate per la memoria, è possibile aggiungere un indice.

> [!NOTE]
> Le nuove colonne vengono aggiunte dopo la modifica di tutte le colonne esistenti nella tabella.

> [!IMPORTANT]
> Se non si usa l'istruzione ALTER TABLE, le istruzioni [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) e [PAD_INDEX](alter-table-index-option-transact-sql.md) non sono supportate per gli indici nelle tabelle ottimizzate per la memoria.

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica i nomi delle colonne che il sistema usa per registrare il periodo di validità di un record. È possibile specificare le colonne esistenti o creare nuove colonne come parte dell'argomento ADD PERIOD FOR SYSTEM_TIME. Configurare le colonne con il tipo di dati datetime2 e definirle come NOT NULL. Se si definisce una colonna periodo NULL, verrà generato un errore. È possibile definire un oggetto [column_constraint](../../t-sql/statements/alter-table-column-constraint-transact-sql.md) e/o [specificare i valori predefiniti](../../relational-databases/tables/specify-default-values-for-columns.md) per le colonne system_start_time e system_end_time. Vedere l'esempio A negli esempi di [Controllo delle versioni di sistema](#system_versioning) seguenti, che illustrano l'uso di un valore predefinito per la colonna system_end_time.

Usare questo argomento con l'argomento SET SYSTEM_VERSIONING per abilitare il controllo delle versioni di sistema in una tabella esistente. Per altre informazioni, vedere [Tabelle temporali](../../relational-databases/tables/temporal-tables.md) e [Introduzione alle tabelle temporali nel database SQL di Azure](/azure/azure-sql/temporal-tables).

A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], gli utenti possono contrassegnare una o entrambe le colonne periodo con il flag **HIDDEN** per nascondere in modo implicito tali colonne. In questo modo, **SELECT \* FROM \<table_name>** non restituirà un valore per le colonne. Per impostazione predefinita, le colonne periodo non vengono nascoste. Per poter essere usate, le colonne nascoste devono essere incluse in modo esplicito in tutte le query che fanno direttamente riferimento alla tabella temporale.

DROP  
Specifica la rimozione di una o più definizioni di colonna, definizioni di colonna calcolata o vincoli di tabella o l'eliminazione della specifica per le colonne che il sistema usa per il controllo delle versioni di sistema.

CONSTRAINT *constraint_name*  
Specifica che *constraint_name* viene rimosso dalla tabella. È possibile elencare più vincoli.

È possibile determinare il nome del vincolo definito dall'utente o fornito dal sistema tramite l'esecuzione di una query sulle viste del catalogo **sys.check_constraint**, **sys.default_constraints**, **sys.key_constraints** e **sys.foreign_keys**.

Se nella tabella è presente un indice XML, non è possibile eliminare un vincolo PRIMARY KEY.

INDEX *index_name*  
Specifica che *index_name* viene rimosso dalla tabella.

La sintassi ALTER TABLE … ADD/DROP/ALTER INDEX è supportata solo per le tabelle ottimizzate per la memoria.

> [!IMPORTANT]
> Se non si usa l'istruzione ALTER TABLE, le istruzioni [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) e [PAD_INDEX](alter-table-index-option-transact-sql.md) non sono supportate per gli indici nelle tabelle ottimizzate per la memoria.

COLUMN *column_name*  
Specifica che *constraint_name* o *column_name* viene rimosso dalla tabella. È possibile elencare più colonne.

 Non è possibile eliminare una colonna se:

- È usata in un indice, come colonna chiave o come INCLUDE
- Viene utilizzata in un vincolo CHECK, FOREIGN KEY, UNIQUE o PRIMARY KEY.
- È associata a un valore predefinito creato con la parola chiave DEFAULT o a un oggetto predefinito.
- È associata a una regola.

> [!NOTE]
> L'eliminazione di una colonna non consente di recuperare lo spazio su disco corrispondente. Può essere necessario recuperare lo spazio su disco di una colonna rimossa quando le dimensioni delle righe della tabella sono prossime al limite o lo hanno superato. Per recuperare spazio, creare un indice cluster nella tabella o ricompilare un indice cluster esistente usando [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md). Per informazioni sull'impatto dell'eliminazione dei tipi di dati LOB, vedere questo [intervento sul blog di CSS](/archive/blogs/psssql/how-it-works-gotcha-varcharmax-caused-my-queries-to-be-slower).

PERIOD FOR SYSTEM_TIME  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Elimina la specifica per le colonne che il sistema userà per il controllo delle versioni di sistema.

WITH \<drop_clustered_constraint_option>  
Specifica l'impostazione di una o più opzioni di eliminazione dei vincoli cluster.

MAXDOP = *max_degree_of_parallelism*  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Esegue l'override dell'opzione di configurazione **max degree of parallelism** solo per la durata dell'operazione. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

L'opzione MAXDOP consente di limitare il numero di processori utilizzati per l'esecuzione di piani paralleli. Il valore massimo è 64 processori.

*max_degree_of_parallelism* può essere uno dei valori seguenti:

1  
Disattiva la generazione di piani paralleli.

\>1  
Limita il numero massimo di processori utilizzati in un'operazione parallela sull'indice in base al numero specificato.

0 (predefinito)  
Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.

Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Le operazioni parallele sugli indici non sono disponibili in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) e [Edizioni e funzionalità supportate per SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

ONLINE **=** { ON | **OFF** } \<as applies to drop_clustered_constraint_option>  
Specifica se le tabelle sottostanti e gli indici associati sono disponibili per le query e la modifica dei dati durante l'operazione sugli indici. Il valore predefinito è OFF. È possibile eseguire REBUILD come operazione ONLINE.

ON  
I blocchi di tabella a lungo termine non vengono mantenuti per la durata dell'operazione sugli indici. Durante la fase principale dell'operazione viene mantenuto solo un blocco preventivo condiviso (IS, Intent Shared) sulla tabella di origine. Questo comportamento consente l'esecuzione di query o l'aggiornamento della tabella sottostante e degli indici. All'inizio dell'operazione viene mantenuto un blocco condiviso (S) sull'oggetto di origine per un breve periodo. Al termine dell'operazione, per un breve periodo viene acquisito un blocco condiviso (S) sull'origine, se viene creato un indice non cluster. In alternativa, viene acquisito un blocco di modifica dello schema (SCH-M) quando un indice cluster viene creato o eliminato online e quando un indice cluster o non cluster viene ricompilato. L'opzione ONLINE non può essere impostata su ON quando viene creato un indice per una tabella temporanea locale. È consentita solo l'operazione di ricompilazione dell'heap a thread singolo.

Per eseguire l'istruzione DDL per un'operazione **SWITCH** o la ricompilazione dell'indice online, è necessario completare tutte le transazioni bloccanti attive in esecuzione in una specifica tabella. Durante l'esecuzione, l'operazione **SWITCH** o di ricompilazione impedisce l'avvio di nuove transazioni e può influire in modo significativo sulla velocità effettiva del carico di lavoro e ritardare temporaneamente l'accesso alla tabella sottostante.

OFF  
I blocchi di tabella si applicano per la durata dell'operazione sugli indici. Un'operazione sugli indici offline che crea, ricompila o elimina un indice cluster oppure ricompila o elimina un indice non cluster acquisisce un blocco di modifica dello schema (SCH-M) sulla tabella. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione. Un'operazione sugli indici offline che crea un indice non cluster acquisisce un blocco condiviso (S) sulla tabella. Il blocco impedisce l'aggiornamento della tabella sottostante ma consente operazioni di lettura, ad esempio l'esecuzione di istruzioni SELECT. Sono consentite operazioni di ricompilazione dell'heap multithread.

Per altre informazioni, vedere [Funzionamento delle operazioni sugli indici online](../../relational-databases/indexes/how-online-index-operations-work.md).

> [!NOTE]
> Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) e [Edizioni e funzionalità supportate per SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ [ 1 **,** ... *n*] **)**  | *filegroup* |  **"** default **"** }  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica una posizione in cui spostare le righe di dati attualmente presenti a livello foglia nell'indice cluster. La tabella viene spostata nella nuova posizione. Questa opzione è valida solo per i vincoli che creano un indice cluster.

> [!NOTE]
> In questo contesto default non è una parola chiave, ma un identificatore per il filegroup predefinito e deve essere delimitato, come in MOVE TO **"** default **"** o MOVE TO **[** default **]** . Se si specifica " **"** default **"** , l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

{ CHECK | NOCHECK } CONSTRAINT  
Specifica che *constraint_name* è abilitato o disabilitato. È possibile utilizzare questa opzione solo con vincoli FOREIGN KEY e CHECK. Quando si specifica NOCHECK, il vincolo viene disabilitato e gli inserimenti o gli aggiornamenti successivi della colonna non vengono convalidati in base alle condizioni del vincolo. I vincoli DEFAULT, PRIMARY KEY e UNIQUE non possono essere disabilitati.

ALL  
Specifica che tutti i vincoli sono disabilitati con l'opzione NOCHECK o abilitati con l'opzione CHECK.

{ ENABLE | DISABLE } TRIGGER  
Specifica che *trigger_name* è abilitato o disabilitato. Quando un trigger è disabilitato, è comunque definito per la tabella. Tuttavia, quando si esegue un'istruzione INSERT, UPDATE o DELETE sulla tabella, le azioni nel trigger vengono eseguite solo dopo che il trigger è stato abilitato nuovamente.

ALL  
Specifica l'abilitazione o la disabilitazione di tutti i trigger della tabella.

*trigger_name*  
Specifica il nome del trigger da abilitare o disabilitare.

{ ENABLE | DISABLE } CHANGE_TRACKING  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica se il rilevamento delle modifiche è abilitato o disabilitato per la tabella. Per impostazione predefinita, il rilevamento delle modifiche è disabilitato.

Questa opzione è disponibile solo quando il rilevamento delle modifiche è abilitato per il database. Per altre informazioni, vedere [Opzioni ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md).

Per abilitare il rilevamento delle modifiche, nella tabella deve essere presente una chiave primaria.

WITH **(** TRACK_COLUMNS_UPDATED **=** { ON | **OFF** } **)**  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica se nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene tenuta traccia delle colonne con rilevamento delle modifiche abilitato che sono state aggiornate. Il valore predefinito è OFF.

SWITCH [ PARTITION *source_partition_number_expression* ] TO [ _schema\_name_ **.** ] *target_table* [ PARTITION *target_partition_number_expression* ]  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Trasferisce un blocco di dati in uno dei modi seguenti:

- Riassegna tutti i dati di una tabella come partizione a una tabella partizionata già esistente.
- Sposta una partizione da una tabella partizionata a un'altra.
- Riassegna tutti i dati in una partizione di una tabella partizionata a una tabella non partizionata esistente.

Se *table* è una tabella partizionata, è necessario specificare *source_partition_number_expression*. Se *target_table* è partizionata, è necessario specificare *target_partition_number_expression*. Quando si riassegnano i dati di una tabella come partizione a una tabella esistente già partizionata o se si sposta una partizione da una tabella partizionata a un'altra, la partizione di destinazione deve essere già esistente e vuota.

Quando si riassegnano i dati di una partizione per formare un'unica tabella, la tabella di destinazione deve esistere già ed essere vuota. Sia la tabella o la partizione di origine che la tabella o la partizione di destinazione devono trovarsi nello stesso filegroup. È inoltre necessario che gli indici o le partizioni degli indici corrispondenti si trovino nello stesso filegroup. Al trasferimento di partizioni vengono applicate molte ulteriori restrizioni. *table* e *target_table* non possono essere la stessa tabella. *target_table* può essere un identificatore in più parti.

*source_partition_number_expression* e *target_partition_number_expression* sono espressioni costanti che possono fare riferimento a variabili e funzioni. incluse variabili con tipo definito dall'utente (UDT) e funzioni definite dall'utente, ma che non possono fare riferimento a espressioni [!INCLUDE[tsql](../../includes/tsql-md.md)].

Una tabella partizionata con un indice columstore cluster ha lo stesso comportamento di un heap partizionato:

- La chiave primaria deve includere la chiave di partizione.
- La chiave di partizione deve essere inclusa in indice univoco. Ma l'inclusione della chiave di partizione con un indice univoco esistente può modificarne l'univocità.
- Per cambiare le partizioni, tutti gli indici non cluster devono includere la chiave di partizione.

Per la restrizione **SWITCH** durante la replica, vedere [Replicare tabelle e indici partizionati](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).

Gli indici columnstore non cluster compilati per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 CTP1 e per il database SQL prima della versione V12 sono in un formato di sola lettura. Prima di poter eseguire un'operazione PARTITION, è necessario ricompilare gli indici columnstore non cluster nel formato corrente, vale a dire in un formato aggiornabile.

SET **(** FILESTREAM_ON = { *partition_scheme_name* \| *filestream_filegroup_name* \| **"** default **"** \| **"** NULL **"** } **)**  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non supporta `FILESTREAM`.

Specifica dove vengono archiviati i dati FILESTREAM.

L'istruzione ALTER TABLE con la clausola SET FILESTREAM_ON viene eseguita in modo corretto solo se nella tabella non sono presenti colonne FILESTREAM. È possibile aggiungere colonne FILESTREAM tramite una seconda istruzione ALTER TABLE.

Se si specifica *partition_scheme_name*, vengono applicate le regole per [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md). Assicurarsi che la tabella sia già partizionata per i dati delle righe e che il relativo schema di partizione usi la stessa funzione e le stesse colonne di partizione dello schema di partizione FILESTREAM.

*filestream_filegroup_name* specifica il nome di un filegroup FILESTREAM. È necessario che per il filegroup sia definito un file tramite un'istruzione [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) o [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). In caso contrario, viene generato un errore.

**"** default **"** specifica il filegroup FILESTREAM con il set di proprietà DEFAULT. Se non è presente alcun filegroup FILESTREAM, viene generato un errore.

**"** NULL **"** specifica che tutti i riferimenti al filegroup FILESTREAM per la tabella vengono rimossi. È necessario eliminare innanzitutto tutte le colonne FILESTREAM. Usare SET FILESTREAM_ON="**NULL**" per eliminare tutti i dati FILESTREAM associati a una tabella.

SET **(** SYSTEM_VERSIONING **=** { OFF | ON [ ( HISTORY_TABLE = schema_name . history_table_name [ , DATA_CONSISTENCY_CHECK = { **ON** | OFF } ] ) ] } **)**  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Disabilita o abilita il controllo delle versioni di sistema di una tabella. Per abilitare il controllo delle versioni di sistema di una tabella, il sistema verifica che il tipo di dati, il vincolo per il supporto dei valori Null e i requisiti di vincolo della chiave primaria per il controllo delle versioni di sistema siano soddisfatti. Se non si usa l'argomento HISTORY_TABLE, il sistema genera una nuova tabella di cronologia corrispondente allo schema della tabella corrente, crea un collegamento tra le due tabelle e consente al sistema di registrare la cronologia di ogni record nella tabella corrente della tabella di cronologia. Il nome di questa tabella di cronologia sarà `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Se si usa l'argomento HISTORY_TABLE per creare un collegamento e usare una tabella di cronologia esistente, il sistema crea un collegamento tra la tabella corrente e la tabella specificata. Quando si crea un collegamento a una tabella di cronologia esistente, è possibile scegliere di eseguire una verifica coerenza dei dati. Questa verifica coerenza dei dati garantisce che i record esistenti non si sovrappongano. L'impostazione predefinita prevede l'esecuzione della verifica coerenza dei dati. Per altre informazioni, vedere [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

HISTORY_RETENTION_PERIOD = { **INFINITE** \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS} }  
**Si applica a**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica il periodo di conservazione finito o infinito per i dati cronologici in una tabella temporale. Se è omesso, si applicherà il periodo di conservazione infinito.

SET (DATA_DELETION =  
      { OFF | ON  
                [(  [ FILTER_COLUMN = column_name ]   
                    [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS }}]   
                )] }   
**Si applica a:** *solo* SQL Edge di Azure

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

SET **(** LOCK_ESCALATION = { AUTO \| TABLE \| DISABLE } **)**  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica i metodi consentiti di escalation blocchi per una tabella.

AUTO  
Questa opzione consente al [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di selezionare la granularità dell'escalation blocchi appropriata per lo schema della tabella.

- Se la tabella è partizionata, sarà consentita l'escalation dei blocchi nella granularità a livello di heap o albero B (HoBT). In altre parole, l'escalation sarà consentita al livello di partizione. Una volta eseguita l'escalation del blocco nel livello HoBT, non verrà eseguita alcuna successiva escalation del blocco nella granularità TABLE.
- Se la tabella non è partizionata, l'escalation blocchi viene eseguita nella granularità TABLE.

TABLE  
L'escalation blocchi viene eseguita con una granularità a livello di tabella, indipendentemente dal partizionamento o meno della tabella. TABLE rappresenta il valore predefinito.

DISABLE  
Evita che venga eseguita l'escalation blocchi nella maggior parte dei casi. I blocchi a livello di tabella non vengono completamente disattivati. Ad esempio, quando si esegue l'analisi di una tabella in cui non è presente alcun indice cluster a livello di isolamento serializzabile, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve acquisire un blocco di tabella per proteggere l'integrità dei dati.

REBUILD  
Utilizzare la sintassi REBUILD WITH per ricompilare un'intera tabella che include tutte le partizioni in una tabella partizionata. Se nella tabella è presente un indice cluster, l'opzione REBUILD consente di ricompilare l'indice stesso. L'opzione REBUILD può essere eseguita come operazione ONLINE.

Utilizzare la sintassi REBUILD PARTITION per ricompilare un'unica partizione in una tabella partizionata.

PARTITION = ALL  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Ricompila tutte le partizioni in caso di modifica delle impostazioni di compressione della partizione.

REBUILD WITH ( \<rebuild_option> )  
Tutte le opzioni vengono applicate a una tabella con un indice cluster. Se nella tabella non è presente un indice cluster, sulla struttura di heap influiranno solo alcune opzioni.

Se con l'operazione REBUILD non viene indicata un'impostazione di compressione specifica, verrà usata l'impostazione di compressione corrente per la partizione. Per restituire l'impostazione corrente, eseguire una query sulla colonna **data_compression** nella vista del catalogo **sys.partitions**.

Per una descrizione completa delle opzioni di ricompilazione, vedere [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md).

DATA_COMPRESSION  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica l'opzione di compressione dei dati per la tabella, il numero di partizione o l'intervallo di partizioni specificato. descritte di seguito:

NONE: la tabella o le partizioni specificate non vengono compresse. Questa opzione non si applica alle tabelle columnstore.

ROW: la tabella o le partizioni specificate vengono compresse usando la compressione di riga. Questa opzione non si applica alle tabelle columnstore.

PAGE: la tabella o le partizioni specificate vengono compresse usando la compressione di pagina. Questa opzione non si applica alle tabelle columnstore.

COLUMNSTORE  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Si applica solo alle tabelle columnstore. Con COLUMNSTORE si specifica di decomprimere una partizione compressa con l'opzione COLUMNSTORE_ARCHIVE. Quando i dati vengono ripristinati, continuano a essere compressi con la compressione columnstore usata per tutte le tabelle columnstore.

COLUMNSTORE_ARCHIVE  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Si applica solo alle tabelle columnstore, ovvero tabelle archiviate con un indice columnstore cluster. COLUMNSTORE_ARCHIVE comprimerà ulteriormente la partizione specificata a una dimensione inferiore. Usare questa opzione per l'archiviazione o altre situazioni in cui sono richieste dimensioni di archiviazione inferiori ed è possibile concedere più tempo per l'archiviazione e il recupero.

Per ricompilare contemporaneamente più partizioni, vedere [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md). Se la tabella non dispone di un indice cluster, la modifica della compressione dei dati comporta la ricompilazione dell'heap e degli indici non cluster. Per altre informazioni sulla compressione, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

ONLINE **=** { ON | **OFF** } \<as applies to single_partition_rebuild_option>  
Specifica se una singola partizione delle tabelle sottostanti e degli indici associati è disponibile per le query e le modifiche dei dati durante l'operazione sull'indice. Il valore predefinito è OFF. È possibile eseguire REBUILD come operazione ONLINE.

ON  
I blocchi di tabella a lungo termine non vengono mantenuti per la durata dell'operazione sugli indici. È necessario un blocco condiviso (S) sulla tabella all'inizio della ricompilazione dell'indice e un blocco di modifica schema (Sch-M) sulla tabella alla fine della ricompilazione dell'indice online. Sebbene entrambi i blocchi siano blocchi di metadati brevi, il blocco Sch-M deve attendere il completamento di tutte le transazioni bloccanti. Durante il tempo di attesa, il blocco Sch-M impedisce tutte le altre transazioni in attesa dietro il blocco stesso per l'accesso alla stessa tabella.

> [!NOTE]
> Con la ricompilazione dell'indice online è possibile impostare le opzioni *low_priority_lock_wait* descritte più avanti in questa sezione.

OFF  
I blocchi di tabella vengono applicati per la durata dell'operazione sugli indici. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Nome del set di colonne. Un set di colonne è una rappresentazione XML non tipizzata che combina tutte le colonne di tipo sparse di una tabella in un output strutturato. Un set di colonne non può essere aggiunto a una tabella che contiene colonne di tipo sparse. Per altre informazioni sui set di colonne, vedere [Utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md).

{ ENABLE | DISABLE } FILETABLE_NAMESPACE  
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).

Consente di abilitare o disabilitare i vincoli definiti dal sistema su una tabella FileTable. Può essere utilizzato solo con una tabella FileTable.

SET ( FILETABLE_DIRECTORY = *directory_name* )  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non supporta `FILETABLE`.

Specifica un nome di directory FileTable compatibile con Windows. Questo nome deve essere univoco tra tutti i nomi di directory FileTable nel database. Il confronto di univocità non supporta la distinzione tra maiuscole e minuscole, nonostante le impostazioni delle regole di confronto SQL. Può essere utilizzato solo con una tabella FileTable.

```syntaxsql
 SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options> )
          | = OFF_WITHOUT_DATA_RECOVERY
          ( MIGRATION_STATE = PAUSED ) | ( <table_stretch_options> [, ...n] )
        } )
```

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e versioni successive).

Consente di abilitare o disabilitare Stretch Database per una tabella. Per altre informazioni, vedere [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Abilitazione di Stretch Database per una tabella**

Quando si specifica `ON` per abilitare Stretch per una tabella, è necessario specificare anche `MIGRATION_STATE = OUTBOUND` per iniziare subito la migrazione dei dati o `MIGRATION_STATE = PAUSED` per posticiparla. Il valore predefinito è `MIGRATION_STATE = OUTBOUND`. Per altre informazioni sull'abilitazione di Stretch per una tabella, vedere [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Prerequisiti**. Prima di abilitare Stretch per una tabella, è necessario abilitare la funzionalità nel server e nel database. Per altre informazioni, vedere [Abilitare Stretch Database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Autorizzazioni**. L'abilitazione di Stretch per un database o una tabella richiede autorizzazioni db_owner. L'abilitazione di Stretch per una tabella richiede anche autorizzazioni ALTER per la tabella.

**Disabilitazione di Stretch Database per una tabella**

Quando si disabilita Stretch per una tabella, esistono due opzioni disponibili per i dati remoti che sono già stati migrati in Azure. Per altre informazioni, vedere [Disabilitare Stretch Database e ripristinare i dati remoti](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

- Per disabilitare l'estensione per una tabella e copiare i dati remoti per la tabella da Azure a SQL Server, eseguire il comando seguente. Questo comando non può essere annullato.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    ```

Questa operazione comporta costi di trasferimento dati e non può essere annullata. Per altre informazioni, vedere i [dettagli sui prezzi dei trasferimenti di dati](https://azure.microsoft.com/pricing/details/data-transfers/).

Dopo aver copiato tutti i dati remoti da Azure a SQL Server, l'estensione viene disabilitata per la tabella.

- Per disabilitare l'estensione per una tabella e abbandonare i dati remoti, eseguire il comando seguente.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

Dopo aver disabilitato Stretch Database per una tabella, la migrazione di dati viene interrotta e i risultati della query non includono più i risultati della tabella remota.

Se si disabilita Stretch, la tabella remota non viene rimossa. Se si vuole eliminare la tabella remota, eseguire l'operazione tramite il portale di Azure.

[ FILTER_PREDICATE = { null | *predicate* } ]  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e versioni successive).

Specifica facoltativamente un predicato di filtro per selezionare le righe di cui eseguire la migrazione da una tabella che contiene sia dati cronologici sia dati correnti. Il predicato deve eseguire la chiamata a una funzione inline con valori di tabella. Per altre informazioni, vedere [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) e [Selezionare le righe di cui eseguire la migrazione tramite una funzione di filtro](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Se si specifica un predicato del filtro inefficace, anche la migrazione dei dati risulterà inefficace. Stretch Database applica il predicato del filtro alla tabella usando l'operatore CROSS APPLY.

Se non si specifica un predicato del filtro, viene eseguita la migrazione dell'intera tabella.

Quando si specifica un predicato di filtro, è necessario specificare anche *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e versioni successive).

- Specificare `OUTBOUND` per eseguire la migrazione dei dati da SQL Server ad Azure.
- Specificare `INBOUND` per copiare i dati remoti per la tabella da Azure a SQL Server e disabilitare Stretch per la tabella. Per altre informazioni, vedere [Disabilitare Stretch Database e ripristinare i dati remoti](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

    Questa operazione comporta costi di trasferimento dati e non può essere annullata.

- Specificare `PAUSED` per sospendere o posticipare la migrazione dei dati. Per altre informazioni, vedere [Sospendere e riprendere la migrazione dei dati](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

WAIT_AT_LOW_PRIORITY  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Per una ricompilazione di indice online è necessario attendere il blocco delle operazioni su questa tabella. **WAIT_AT_LOW_PRIORITY** indica che l'operazione di ricompilazione dell'indice online rimane in attesa di blocchi a priorità bassa, consentendo alle altre operazioni di proseguire mentre la compilazione dell'indice online è in attesa. L'omissione dell'opzione **WAIT AT LOW PRIORITY** equivale a `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.

MAX_DURATION = *time* [**MINUTES** ]  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Tempo di attesa, espresso con un valore intero specificato in minuti, dell'operazione **SWITCH** o dei blocchi di ricompilazione dell'indice online a priorità bassa durante l'esecuzione del comando DDL. Se l'operazione viene bloccata per il tempo specificato in **MAX_DURATION**, una delle azioni **ABORT_AFTER_WAIT** verrà eseguita. Il tempo **MAX_DURATION** è sempre espresso in minuti ed è possibile omettere la parola **MINUTES**.

ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

NONE  
Continuare ad attendere il blocco con priorità normale (regolare).

SELF  
Esce dall'operazione **SWITCH** o DDL di ricompilazione dell'indice online attualmente in esecuzione senza eseguire alcuna azione.

BLOCKERS  
Termina tutte le transazioni utente che attualmente bloccano l'operazione **SWITCH** o DDL di ricompilazione dell'indice online in modo da poter continuare l'operazione.

È necessaria l'autorizzazione **ALTER ANY CONNECTION**.

IF EXISTS  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Elimina in modo condizionale la colonna o il vincolo solo se è già esistente.

## <a name="remarks"></a>Osservazioni

Per aggiungere nuove righe di dati, usare [INSERT](../../t-sql/statements/insert-transact-sql.md). Per rimuovere righe di dati, usare [DELETE](../../t-sql/statements/delete-transact-sql.md) o [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md). Per modificare i valori nelle righe esistenti, usare [UPDATE](../../t-sql/queries/update-transact-sql.md).

Se la cache delle procedure include piani di esecuzione che fanno riferimento alla tabella, l'istruzione ALTER TABLE li contrassegna per la ricompilazione durante l'esecuzione successiva.

## <a name="changing-the-size-of-a-column"></a>Modifica delle dimensioni di una colonna

È possibile modificare la lunghezza, la precisione o la scala di una colonna specificando nuove dimensioni per il tipo di dati della colonna. Usare la clausola ALTER COLUMN. Se nella colonna sono presenti dati, le nuove dimensioni non possono essere minori delle dimensioni massime dei dati. Inoltre, non è possibile definire la colonna in un indice, tranne nel caso in cui il tipo di dati della colonna sia **varchar**, **nvarchar** o **varbinary** e l'indice sia diverso dal risultato di un vincolo PRIMARY KEY. Vedere l'esempio nella sezione breve intitolata [Modifica di una definizione di colonna](#alter_column).

## <a name="locks-and-alter-table"></a>Blocchi e ALTER TABLE

Le modifiche specificate in ALTER TABLE vengono implementate immediatamente. Se le modifiche richiedono l'alterazione delle righe nella tabella, le righe vengono aggiornate tramite ALTER TABLE. ALTER TABLE acquisisce un blocco di modifica dello schema (SCH-M) sulla tabella per verificare che durante la modifica nessun'altra connessione faccia riferimento ai dati o ai metadati della tabella, ad eccezione delle operazioni sugli indici online al termine delle quali è richiesto un breve blocco SCH-M. In un'operazione `ALTER TABLE...SWITCH` il blocco viene acquisito sia sulle tabelle di origine sia in quelle di destinazione. Le modifiche apportate alla tabella vengono registrate e possono essere recuperate completamente. Le modifiche che influiscono su tutte le righe di tabelle di grandi dimensioni, ad esempio l'eliminazione di una colonna o, in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'aggiunta di una colonna NOT NULL con un valore predefinito, possono richiedere molto tempo e generare un elevato numero di record del log. Eseguire queste istruzioni ALTER TABLE con la stessa attenzione dedicata alle istruzioni INSERT, UPDATE o DELETE che influiscono su molte righe.

### <a name="adding-not-null-columns-as-an-online-operation"></a>Aggiunta di colonne NOT NULL come operazione online

A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise Edition, l'aggiunta di una colonna NOT NULL con un valore predefinito è un'operazione online quando il valore predefinito è una *costante di runtime*. L'operazione viene pertanto completata quasi istantaneamente nonostante il numero di righe nella tabella, in quanto le righe esistenti nella tabella non vengono aggiornate durante l'operazione, ma il valore predefinito viene archiviato solo nei metadati della tabella e il valore viene cercato in base alle necessità nelle query che accedono a tali righe. Questo comportamento è automatico. Oltre alla sintassi ADD COLUMN, non è necessaria alcuna sintassi aggiuntiva per implementare l'operazione online. Una costante di runtime è un'espressione che produce lo stesso valore durante il runtime per ogni riga nella tabella nonostante il relativo determinismo. Esempi di costanti di runtime sono l'espressione costante "Dati temporanei personali" o la funzione di sistema GETUTCDATETIME(). Al contrario, le funzioni `NEWID()` e `NEWSEQUENTIALID()` non sono costanti di runtime perché viene generato un valore univoco per ogni riga della tabella. L'aggiunta di una colonna NOT NULL con un valore predefinito che non è una costante di runtime viene eseguita sempre offline e per tutta la durata dell'operazione viene acquisito un blocco esclusivo (SCH-M).

Mentre le righe esistenti fanno riferimento al valore archiviato nei metadati, il valore predefinito viene archiviato nella riga per tutte le nuove righe inserite e che non specificano un altro valore per la colonna. Il valore predefinito archiviato nei metadati viene spostato in una riga esistente quando la riga viene aggiornata, anche se la colonna effettiva non viene specificata nell'istruzione UPDATE, o se la tabella o l'indice cluster viene ricompilato.

Le colonne di tipo **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml**, **text**, **ntext**, **image**, **hierarchyid**, **geometry**, **geography** o CLR UDTS non possono essere aggiunte in un'operazione online. Non è possibile aggiungere una colonna online se con tale operazione le dimensioni massime possibili per la riga superano il limite di 8.060 byte. In tal caso, la colonna viene aggiunta come operazione offline.

## <a name="parallel-plan-execution"></a>Esecuzione di piani paralleli

In [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] e versioni successive il numero di processori impiegati per eseguire un'unica istruzione ALTER TABLE ADD (basata su indici) CONSTRAINT o DROP (indice cluster) CONSTRAINT è determinato dall'opzione di configurazione **max degree of parallelism** e dal carico di lavoro corrente. Se [!INCLUDE[ssDE](../../includes/ssde-md.md)] rileva che il sistema è occupato, il grado di parallelismo dell'operazione viene ridotto automaticamente prima dell'avvio dell'esecuzione dell'istruzione. È possibile configurare manualmente il numero di processori utilizzati per eseguire l'istruzione mediante l'opzione MAXDOP. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

## <a name="partitioned-tables"></a>Tabelle partizionate

Oltre all'esecuzione di operazioni SWITCH che interessano tabelle partizionate, usare ALTER TABLE per modificare lo stato di colonne, vincoli e trigger di tali tabelle così come per le tabelle non partizionate. Non è tuttavia possibile usare questa istruzione per modificare il modo in cui la tabella stessa è partizionata. Per la ripartizione di una tabella partizionata, usare [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) e [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md). Non è inoltre possibile modificare il tipo di dati di una colonna di una tabella partizionata.

## <a name="restrictions-on-tables-with-schema-bound-views"></a>Restrizioni per le tabelle con viste associate a schema

Le restrizioni che si applicano a istruzioni ALTER TABLE eseguite su tabelle con viste associate a schema sono le stesse che vengono applicate alla modifica di tabelle con un indice semplice. È possibile aggiungere una colonna mentre non è consentito rimuovere o modificare una colonna che fa parte di una vista associata a uno schema. Se l'istruzione ALTER TABLE richiede la modifica di una colonna utilizzata in una vista associata allo schema, ALTER TABLE ha esito negativo e [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un messaggio di errore. Per altre informazioni sull'associazione allo schema e sulle viste indicizzate, vedere [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md).

La creazione di una vista associata a uno schema che fa riferimento a tabelle di base non influisce sull'aggiunta o sulla rimozione di trigger in tali tabelle.

## <a name="indexes-and-alter-table"></a>Indici e ALTER TABLE

Gli indici creati nell'ambito di un vincolo vengono eliminati con l'eliminazione del vincolo. Gli indici creati mediante CREATE INDEX devono essere eliminati mediante DROP INDEX. Usare l'istruzione ALTER INDEX per ricompilare un indice che costituisce una parte di una definizione di vincolo. Non è necessario eliminare e quindi aggiungere nuovamente il vincolo con ALTER TABLE.

Tutti gli indici e i vincoli basati su una colonna devono essere rimossi prima della rimozione della colonna.

Quando si elimina un vincolo con cui è stato creato un indice cluster, le righe di dati archiviate a livello foglia nell'indice cluster vengono archiviate in una tabella non cluster. È possibile eliminare l'indice cluster e spostare la tabella risultante in un altro filegroup o schema di partizione in una singola transazione specificando l'opzione MOVE TO. Per l'opzione MOVE TO vengono applicate le seguenti restrizioni:

- MOVE TO non può essere usata per viste indicizzate o indici non cluster.
- Lo schema di partizione o il filegroup deve essere già esistente.
- Se non si specifica MOVE TO, la tabella viene inserita nello stesso schema di partizione o nello stesso filegroup definito per l'indice cluster.

Quando si elimina un indice cluster, specificare l'opzione ONLINE **=** ON per evitare che la transazione DROP INDEX blocchi l'esecuzione di query e le modifiche nei dati sottostanti e negli indici non cluster associati.

Per l'opzione ONLINE **=** ON vengono applicate le restrizioni seguenti:

- ONLINE **=** ON non è valida per gli indici cluster che sono anche disabilitati. Per eliminare gli indici disabilitati è necessario usare l'opzione ONLINE **=** OFF.
- È possibile eliminare un solo indice alla volta.
- ONLINE **=** ON non è valida per viste indicizzate, indici non cluster o indici su tabelle temporanee locali.
- ONLINE **=** ON non è valida per gli indici columnstore.

Per l'eliminazione di un indice cluster, lo spazio su disco temporaneo deve essere uguale alle dimensioni dell'indice cluster esistente. Questo spazio aggiuntivo viene rilasciato al termine dell'operazione.

> [!NOTE]
> Le opzioni elencate in *\<drop_clustered_constraint_option>* si applicano a indici cluster su tabelle e non possono essere applicate a indici cluster su viste o a indici non cluster.

## <a name="replicating-schema-changes"></a>Replica delle modifiche dello schema

Per impostazione predefinita, quando si esegue ALTER TABLE su una tabella pubblicata in un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tale modifica si propaga a tutti i Sottoscrittori [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa funzionalità presenta alcune restrizioni e può essere disabilitata. Per altre informazioni, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).

## <a name="data-compression"></a>Compressione dei dati

Le tabelle di sistema non possono essere abilitate per la compressione. Se la tabella è un heap, l'operazione di ricompilazione per la modalità ONLINE sarà a thread singolo. Utilizzare la modalità OFFLINE per un'operazione di ricompilazione di heap multithread. Per altre informazioni sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

Per valutare il modo in cui la modifica dello stato di compressione influirà su una tabella, un indice o una partizione, usare la stored procedure [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

Alle tabelle partizionate vengono applicate le restrizioni seguenti:

- Non è possibile modificare l'impostazione di compressione di una singola partizione se la tabella include indici non allineati.
- La sintassi ALTER TABLE \<table> REBUILD PARTITION ... ricompila la partizione specificata.
- La sintassi ALTER TABLE \<table> REBUILD WITH ... ricompila tutte le partizioni.

## <a name="dropping-ntext-columns"></a>Eliminazione di colonne NTEXT

Quando si eliminano le colonne NTEXT, la pulizia dei dati eliminati viene eseguita come operazione serializzata per tutte le righe. L'operazione di pulizia può richiedere una grande quantità di tempo. Per eliminare una colonna NTEXT in una tabella con molte righe, aggiornare la colonna NTEXT su un valore NULL, quindi eliminare la colonna. È possibile eseguire questa opzione con operazioni parallele rendendola molto più rapida.

## <a name="online-index-rebuild"></a>Ricompilazione di indici online

Per eseguire l'istruzione DDL per una ricompilazione dell'indice online, è necessario completare tutte le transazioni bloccanti attive in esecuzione in una specifica tabella. Quando la ricompilazione dell'indice online viene avviata, blocca tutte le nuove transazioni pronte per l'esecuzione in questa tabella. Sebbene la durata del blocco della ricompilazione dell'indice online sia breve, l'attesa del completamento di tutte le transazioni aperte in una tabella specificata e il blocco dell'avvio di nuove transazioni potrebbero influire in modo significativo sulla velocità effettiva. Ciò può provocare un rallentamento o un timeout del carico di lavoro e limitare notevolmente l'accesso alla tabella sottostante. L'opzione **WAIT_AT_LOW_PRIORITY** consente agli amministratori di database di gestire i blocchi S e Sch-M necessari per le ricompilazioni degli indici online, nonché di selezionare una delle tre opzioni disponibili. In tutti e tre i casi se durante il tempo di attesa, ( `(MAX_DURATION =n [minutes])` ), non sono presenti attività di blocco, la ricompilazione dell'indice online viene eseguita immediatamente senza attendere il completamento dell'istruzione DDL.

## <a name="compatibility-support"></a>Informazioni sulla compatibilità

L'istruzione ALTER TABLE supporta unicamente nomi di tabella in due parti (schema.oggetto). In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] l'utilizzo di un nome di tabella basato sui formati seguenti comporta la generazione dell'errore 117 in fase di compilazione.

- server.database.schema.tabella
- .database.schema.tabella
- ..schema.tabella

Nelle versioni precedenti l'uso del formato server.database.schema.tabella genera l'errore 4902. L'uso del formato .database.schema.tabella o ..schema.tabella è supportato.

Per risolvere il problema, rimuovere l'uso di un prefisso in quattro parti.

## <a name="permissions"></a>Autorizzazioni

È necessario disporre dell'autorizzazione ALTER per la tabella.

Le autorizzazioni ALTER TABLE si applicano a entrambe le tabelle coinvolte in un'istruzione ALTER TABLE SWITCH. Tutti i dati trasferiti ereditano la sicurezza della tabella di destinazione.

Se nell'istruzione ALTER TABLE sono state definite colonne di tipo Common Language Runtime (CLR) definito dall'utente (UDT) o di tipo di dati alias, è necessaria l'autorizzazione REFERENCES per il tipo desiderato.

Per aggiungere o modificare una colonna con la quale vengono aggiornate le righe della tabella è necessaria l'autorizzazione **UPDATE** per la tabella. ad esempio per aggiungere una colonna **NOT NULL** con un valore predefinito o una colonna Identity quando la tabella non è vuota.

## <a name="examples"></a><a name="Example_Top"></a> Esempi

|Category|Elementi di sintassi inclusi|
|--------------|------------------------------|
|[Aggiunta di colonne e vincoli](#add)|ADD * PRIMARY KEY con opzioni per gli indici * colonne di tipo sparse e set di colonne *|
|[Eliminazione di colonne e vincoli](#Drop)|DROP|
|[Modifica della definizione di colonna](#alter_column)|cambiare tipo di dati * cambiare dimensioni delle colonna * regole di confronto|
|[Modifica della definizione di una tabella](#alter_table)|DATA_COMPRESSION * SWITCH PARTITION OF * LOCK ESCALATION * rilevamento delle modifiche|
|[Disabilitazione e abilitazione di vincoli e trigger](#disable_enable)|CHECK * NO CHECK * ENABLE TRIGGER * DISABLE TRIGGER|
| &nbsp; | &nbsp; |

### <a name="adding-columns-and-constraints"></a><a name="add"></a>Aggiunta di colonne e vincoli

Negli esempi di questa sezione viene illustrata l'aggiunta di colonne e vincoli a una tabella.

#### <a name="a-adding-a-new-column"></a>R. Aggiunta di una nuova colonna

Nell'esempio seguente viene aggiunta una colonna che consente valori Null e alla quale non sono forniti valori mediante una definizione DEFAULT. In ogni riga della nuova colonna sarà indicato `NULL`.

```sql
CREATE TABLE dbo.doc_exa (column_a INT) ;
GO
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL ;
GO
```

#### <a name="b-adding-a-column-with-a-constraint"></a>B. Aggiunta di una colonna con un vincolo

Nell'esempio seguente viene aggiunta una nuova colonna con un vincolo `UNIQUE`.

```sql
CREATE TABLE dbo.doc_exc (column_a INT) ;
GO
ALTER TABLE dbo.doc_exc ADD column_b VARCHAR(20) NULL
    CONSTRAINT exb_unique UNIQUE ;
GO
EXEC sp_help doc_exc ;
GO
DROP TABLE dbo.doc_exc ;
GO
```

#### <a name="c-adding-an-unverified-check-constraint-to-an-existing-column"></a>C. Aggiunta di un vincolo CHECK non verificato a una colonna esistente

Nell'esempio seguente viene aggiunto un vincolo a una colonna esistente nella tabella. Nella colonna è presente un valore che viola il vincolo. Pertanto, viene utilizzato `WITH NOCHECK` per evitare che il vincolo venga convalidato in base alle righe esistenti e consentire l'aggiunta del vincolo.

```sql
CREATE TABLE dbo.doc_exd (column_a INT) ;
GO
INSERT INTO dbo.doc_exd VALUES (-1) ;
GO
ALTER TABLE dbo.doc_exd WITH NOCHECK
ADD CONSTRAINT exd_check CHECK (column_a > 1) ;
GO
EXEC sp_help doc_exd ;
GO
DROP TABLE dbo.doc_exd ;
GO
```

#### <a name="d-adding-a-default-constraint-to-an-existing-column"></a>D. Aggiunta di un vincolo DEFAULT a una colonna esistente

Nell'esempio seguente viene creata una tabella con due colonne e viene inserito un valore nella prima colonna mentre i valori nell'altra colonna rimangono NULL. Viene quindi aggiunto un vincolo `DEFAULT` alla seconda colonna. Per verificare l'applicazione del vincolo, viene inserito un altro valore nella prima colonna e viene eseguita una query sulla tabella.

```sql
CREATE TABLE dbo.doc_exz (column_a INT, column_b INT) ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (7) ;
GO
ALTER TABLE dbo.doc_exz
  ADD CONSTRAINT col_b_def
  DEFAULT 50 FOR column_b ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (10) ;
GO
SELECT * FROM dbo.doc_exz ;
GO
DROP TABLE dbo.doc_exz ;
GO
```

#### <a name="e-adding-several-columns-with-constraints"></a>E. Aggiunta di più colonne con vincoli

Nell'esempio seguente vengono aggiunte più colonne con vincoli. I vincoli vengono definiti con la nuova colonna. Alla prima colonna è associata la proprietà `IDENTITY`. Nella colonna Identity di ogni riga della tabella sono presenti nuovi valori incrementali.

```sql
CREATE TABLE dbo.doc_exe (column_a INT CONSTRAINT column_a_un UNIQUE) ;
GO
ALTER TABLE dbo.doc_exe ADD

-- Add a PRIMARY KEY identity column.
column_b INT IDENTITY
CONSTRAINT column_b_pk PRIMARY KEY,

-- Add a column that references another column in the same table.
column_c INT NULL
CONSTRAINT column_c_fk
REFERENCES doc_exe(column_a),

-- Add a column with a constraint to enforce that
-- nonnull data is in a valid telephone number format.
column_d VARCHAR(16) NULL
CONSTRAINT column_d_chk
CHECK
(column_d LIKE '[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]' OR
column_d LIKE
'([0-9][0-9][0-9]) [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'),

-- Add a nonnull column with a default.
column_e DECIMAL(3,3)
CONSTRAINT column_e_default
DEFAULT .081 ;
GO
EXEC sp_help doc_exe ;
GO
DROP TABLE dbo.doc_exe ;
GO
```

#### <a name="f-adding-a-nullable-column-with-default-values"></a>F. Aggiunta di una colonna che ammette i valori Null con valori predefiniti

Nell'esempio seguente viene aggiunta una colonna che ammette i valori Null con una definizione `DEFAULT` e viene specificato `WITH VALUES` per l'assegnazione di valori a ogni riga della tabella. Se non si usa WITH VALUES, a ogni riga della nuova colonna viene associato il valore NULL.

```sql
CREATE TABLE dbo.doc_exf (column_a INT) ;
GO
INSERT INTO dbo.doc_exf VALUES (1) ;
GO
ALTER TABLE dbo.doc_exf
ADD AddDate smalldatetime NULL
CONSTRAINT AddDateDflt
DEFAULT GETDATE() WITH VALUES ;
GO
DROP TABLE dbo.doc_exf ;
GO
```

#### <a name="g-creating-a-primary-key-constraint-with-index-or-data-compression-options"></a>G. Creazione di un vincolo PRIMARY KEY con opzioni per gli indici o la compressione dei dati

Nell'esempio seguente viene creato il vincolo PRIMARY KEY `PK_TransactionHistoryArchive_TransactionID` e vengono impostate le opzioni `FILLFACTOR`, `ONLINE` e `PAD_INDEX`. All'indice cluster risultante sarà assegnato lo stesso nome del vincolo.

**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (FILLFACTOR = 75, ONLINE = ON, PAD_INDEX = ON);
GO
```

In questo esempio simile viene applicata la compressione di pagina durante l'applicazione della chiave primaria in cluster.

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (DATA_COMPRESSION = PAGE);
GO
```

#### <a name="h-adding-a-sparse-column"></a>H. Aggiunta di una colonna di tipo sparse

Negli esempi seguenti si illustrano l'aggiunta e la modifica di colonne di tipo sparse nella tabella T1. Il codice per creare la tabella `T1` è il seguente:

```sql
CREATE TABLE T1 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) SPARSE NULL,
  C3 INT SPARSE NULL,
  C4 INT) ;
GO
```

Per aggiungere una colonna di tipo sparse aggiuntiva `C5`, eseguire l'istruzione riportata di seguito.

```sql
ALTER TABLE T1
ADD C5 CHAR(100) SPARSE NULL ;
GO
```

Per convertire la colonna non di tipo sparse `C4` in una colonna di tipo sparse, eseguire l'istruzione riportata di seguito.

```sql
ALTER TABLE T1
ALTER COLUMN C4 ADD SPARSE ;
GO
```

Per convertire la colonna di tipo sparse `C4` in una colonna non di tipo sparse, eseguire l'istruzione riportata di seguito.

```sql
ALTER TABLE T1
ALTER COLUMN C4 DROP SPARSE ;
GO
```

#### <a name="i-adding-a-column-set"></a>I. Aggiunta di un set di colonne

Negli esempi seguenti viene illustrata l'aggiunta di una colonna alla tabella `T2`. Un set di colonne non può essere aggiunto a una tabella che contiene già colonne di tipo sparse. Il codice per creare la tabella `T2` è il seguente:

```sql
CREATE TABLE T2 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Le tre istruzioni seguenti aggiungono un set di colonne denominato `CS`, quindi modificano le colonne `C2` e `C3` in `SPARSE`.

```sql
ALTER TABLE T2
ADD CS XML COLUMN_SET FOR ALL_SPARSE_COLUMNS ;
GO

ALTER TABLE T2
ALTER COLUMN C2 ADD SPARSE ;
GO

ALTER TABLE T2
ALTER COLUMN C3 ADD SPARSE ;
GO
```

#### <a name="j-adding-an-encrypted-column"></a>J. Aggiunta di una colonna crittografata

L'istruzione seguente aggiunge una colonna crittografata denominata `PromotionCode`.

```sql
ALTER TABLE Customers ADD
    PromotionCode nvarchar(100)
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,
    ENCRYPTION_TYPE = RANDOMIZED,
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') ;
```

### <a name="dropping-columns-and-constraints"></a><a name="Drop"></a>Eliminazione di colonne e vincoli

Negli esempi di questa sezione viene illustrata l'eliminazione di colonne e vincoli.

#### <a name="a-dropping-a-column-or-columns"></a>R. Eliminazione di una o più colonne

Nel primo esempio viene modificata una tabella per rimuovere una colonna. Nel secondo esempio vengono rimosse più colonne.

```sql
CREATE TABLE dbo.doc_exb (
     column_a INT,
     column_b VARCHAR(20) NULL,
     column_c DATETIME,
     column_d INT) ;
GO  
-- Remove a single column.
ALTER TABLE dbo.doc_exb DROP COLUMN column_b ;
GO
-- Remove multiple columns.
ALTER TABLE dbo.doc_exb DROP COLUMN column_c, column_d;
```

#### <a name="b-dropping-constraints-and-columns"></a>B. Eliminazione di vincoli e colonne

Nel primo esempio viene rimosso un vincolo `UNIQUE` da una tabella. Nel secondo esempio vengono rimossi due vincoli e una singola colonna.

```sql
CREATE TABLE dbo.doc_exc (column_a INT NOT NULL CONSTRAINT my_constraint UNIQUE) ;
GO

-- Example 1. Remove a single constraint.
ALTER TABLE dbo.doc_exc DROP my_constraint ;
GO

DROP TABLE dbo.doc_exc;
GO

CREATE TABLE dbo.doc_exc ( column_a INT
                          NOT NULL CONSTRAINT my_constraint UNIQUE
                          ,column_b INT
                          NOT NULL CONSTRAINT my_pk_constraint PRIMARY KEY) ;
GO

-- Example 2. Remove two constraints and one column
-- The keyword CONSTRAINT is optional. The keyword COLUMN is required.
ALTER TABLE dbo.doc_exc
DROP CONSTRAINT my_constraint, my_pk_constraint, COLUMN column_b ;
GO
```

#### <a name="c-dropping-a-primary-key-constraint-in-the-online-mode"></a>C. Eliminazione di un vincolo PRIMARY KEY nella modalità ONLINE

Nell'esempio seguente viene eliminato un vincolo PRIMARY KEY con l'opzione `ONLINE` impostata su `ON`.

```sql
ALTER TABLE Production.TransactionHistoryArchive
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID
WITH (ONLINE = ON) ;
GO
```

#### <a name="d-adding-and-dropping-a-foreign-key-constraint"></a>D. Aggiunta e rimozione di un vincolo FOREIGN KEY

Nell'esempio seguente viene creata la tabella `ContactBackup`, successivamente modificata con l'aggiunta di un vincolo `FOREIGN KEY` che fa riferimento alla tabella `Person.Person`. Il vincolo `FOREIGN KEY` viene quindi rimosso.

```sql
CREATE TABLE Person.ContactBackup
    (ContactID INT) ;
GO

ALTER TABLE Person.ContactBackup
ADD CONSTRAINT FK_ContactBackup_Contact FOREIGN KEY (ContactID)
    REFERENCES Person.Person (BusinessEntityID) ;
GO

ALTER TABLE Person.ContactBackup
DROP CONSTRAINT FK_ContactBackup_Contact ;
GO

DROP TABLE Person.ContactBackup ;
```

![Icona freccia usata con il collegamento Torna all'inizio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icona freccia usata con il collegamento Torna all'inizio") [Esempi](#Example_Top)

### <a name="altering-a-column-definition"></a><a name="alter_column"></a> Modifica della definizione di una colonna

#### <a name="a-changing-the-data-type-of-a-column"></a>R. Modifica del tipo di dati di una colonna

Nell'esempio seguente la colonna di una tabella viene modificata da `INT` a `DECIMAL`.

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;
GO
DROP TABLE dbo.doc_exy ;
GO
```

#### <a name="b-changing-the-size-of-a-column"></a>B. Modifica delle dimensioni di una colonna

Nell'esempio seguente vengono aumentate le dimensioni di una colonna **varchar** e la precisione e la scala di una colonna **decimal**. Poiché le colonne contengono dati, le relative dimensioni possono solo essere aumentate. Si noti inoltre che `col_a` è definito in un indice univoco. Le dimensioni di `col_a` possono ancora essere aumentate poiché il tipo di dati è **varchar** e l'indice non è il risultato di un vincolo PRIMARY KEY.

```sql
-- Create a two-column table with a unique index on the varchar column.
CREATE TABLE dbo.doc_exy (col_a varchar(5) UNIQUE NOT NULL, col_b decimal (4,2)) ;
GO
INSERT INTO dbo.doc_exy VALUES ('Test', 99.99) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
GO
-- Increase the size of the varchar column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_a varchar(25) ;
GO
-- Increase the scale and precision of the decimal column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_b decimal (10,4) ;
GO
-- Insert a new row.
INSERT INTO dbo.doc_exy VALUES ('MyNewColumnSize', 99999.9999) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
```

#### <a name="c-changing-column-collation"></a>C. Modifica delle regole di confronto di una colonna

Nell'esempio seguente si illustra come modificare le regole di confronto di una colonna. Innanzitutto, viene creata una tabella con le regole di confronto predefinite dell'utente.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

In seguito le regole di confronto della colonna `C2` vengono impostate su Latin1_General_BIN. Il tipo di dati è richiesto, anche se non è modificato.

```sql
ALTER TABLE T3
ALTER COLUMN C2 varchar(50) COLLATE Latin1_General_BIN ;
GO
```

#### <a name="d-encrypting-a-column"></a>D. Crittografia di una colonna

L'esempio seguente illustra come crittografare una colonna usando [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

In primo luogo, viene creata una tabella senza colonne crittografate.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Successivamente, la colonna "C2" viene crittografata con una chiave di crittografia, denominata CEK1, e la crittografia casuale. Perché l'istruzione seguente abbia esito positivo:

- La chiave di crittografia della colonna deve essere abilitata per l'enclave, vale a dire che deve essere crittografata con una chiave master della colonna che consente i calcoli dell'enclave.
- L'istanza di SQL Server di destinazione deve supportare Always Encrypted con enclave sicuri.
- L'istruzione deve essere eseguita tramite una connessione configurata per Always Encrypted con enclave sicuri e usando un driver client supportato.
- L'applicazione chiamante deve avere accesso alla chiave master della colonna, CEK1 di protezione.

```sql
ALTER TABLE T3
ALTER COLUMN C2 VARCHAR(50) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL;
GO
```

### <a name="altering-a-table-definition"></a><a name="alter_table"></a> Modifica della definizione di una tabella

Negli esempi di questa sezione viene illustrato come modificare la definizione di una tabella.

#### <a name="a-modifying-a-table-to-change-the-compression"></a>R. Modifica di una tabella per cambiare la compressione

Nell'esempio seguente viene modificata la compressione di una tabella non partizionata. L'heap o l'indice cluster verrà ricompilato. Se la tabella è un heap, tutti gli indici non cluster verranno ricompilati.

```sql
ALTER TABLE T1
REBUILD WITH (DATA_COMPRESSION = PAGE) ;
```

Nell'esempio seguente viene modificata la compressione di una tabella partizionata. La sintassi `REBUILD PARTITION = 1` consente di ricompilare solo il numero di partizione `1`.

**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =NONE) ;
GO
```

Se per la stessa operazione viene utilizzata la sintassi alternativa seguente, vengono ricompilate tutte le partizioni della tabella.

**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = ALL
WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)) ;
```

Per altri esempi sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

#### <a name="b-modifying-a-columnstore-table-to-change-archival-compression"></a>B. Modifica di una tabella columnstore per modificare la compressione dell'archivio

Nell'esempio seguente viene compressa una partizione di tabella columnstore applicando un algoritmo di compressione aggiuntivo. Questa compressione riduce le dimensioni della tabella, ma aumenta il tempo necessario per l'archiviazione e il recupero. È utile per l'archiviazione o in situazioni in cui è richiesto uno spazio inferiore ed è possibile concedere più tempo per l'archiviazione e il recupero.

**SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE) ;
GO
```

Nell'esempio seguente viene decompressa una partizione di tabella columnstore compressa con l'opzione COLUMNSTORE_ARCHIVE. Quando i dati vengono ripristinati, continueranno a essere compressi con la compressione columnstore usata per tutte le tabelle columnstore.

**SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE) ;
GO
```

#### <a name="c-switching-partitions-between-tables"></a>C. Trasferimento di partizioni tra tabelle

Nell'esempio seguente viene creata una tabella partizionata, presupponendo che nel database sia già stato creato lo schema di partizione `myRangePS1`. Verrà quindi creata una tabella non partizionata con la stessa struttura della tabella partizionata e nello stesso filegroup di `PARTITION 2` della tabella `PartitionTable`. I dati di `PARTITION 2` della tabella `PartitionTable` vengono quindi trasferiti nella tabella `NonPartitionTable`.

```sql
CREATE TABLE PartitionTable (col1 INT, col2 CHAR(10))
ON myRangePS1 (col1) ;
GO
CREATE TABLE NonPartitionTable (col1 INT, col2 CHAR(10))
ON test2fg ;
GO
ALTER TABLE PartitionTable SWITCH PARTITION 2 TO NonPartitionTable ;
GO
```

#### <a name="d-allowing-lock-escalation-on-partitioned-tables"></a>D. Consentire l'escalation blocchi nelle tabelle partizionate

Nell'esempio seguente viene abilitata l'escalation blocchi a livello di partizione in una tabella partizionata. Se la tabella non è partizionata, l'escalation blocchi viene impostata a livello TABLE.

**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE dbo.T1 SET (LOCK_ESCALATION = AUTO) ;
GO
```

#### <a name="e-configuring-change-tracking-on-a-table"></a>E. Configurazione del rilevamento delle modifiche in una tabella

Nell'esempio seguente viene abilitato il rilevamento delle modifiche per la tabella `Person.Person`.

**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING ;
```

Nell'esempio seguente viene abilitato il rilevamento delle modifiche e il rilevamento delle colonne aggiornate durante una modifica.

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON)
```

Nell'esempio seguente viene disabilitato il rilevamento delle modifiche per la tabella `Person.Person`.

**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
DISABLE CHANGE_TRACKING ;
```

### <a name="disabling-and-enabling-constraints-and-triggers"></a><a name="disable_enable"></a> Disabilitazione e abilitazione di vincoli e trigger

#### <a name="a-disabling-and-re-enabling-a-constraint"></a>R. Disabilitazione e riabilitazione di un vincolo

Nell'esempio seguente viene disabilitato un vincolo che limita i dati relativi agli stipendi accettabili. `NOCHECK CONSTRAINT` viene utilizzata con `ALTER TABLE` per disabilitare il vincolo e consentire un inserimento che in genere violerebbe il vincolo. `CHECK CONSTRAINT` abilita nuovamente il vincolo.

```sql
CREATE TABLE dbo.cnst_example (
  id INT NOT NULL,
  name VARCHAR(10) NOT NULL,
  salary MONEY NOT NULL
  CONSTRAINT salary_cap CHECK (salary < 100000)) ;

-- Valid inserts
INSERT INTO dbo.cnst_example VALUES (1,'Joe Brown',65000) ;
INSERT INTO dbo.cnst_example VALUES (2,'Mary Smith',75000) ;

-- This insert violates the constraint.
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Disable the constraint and try again.
ALTER TABLE dbo.cnst_example NOCHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Re-enable the constraint and try another insert; this will fail.
ALTER TABLE dbo.cnst_example CHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (4,'Eric James',110000) ;
```

#### <a name="b-disabling-and-re-enabling-a-trigger"></a>B. Disabilitazione e riabilitazione di un trigger

Nell'esempio seguente viene utilizzata l'opzione `DISABLE TRIGGER` di `ALTER TABLE` per disabilitare il trigger e consentire un inserimento che altrimenti violerebbe il trigger. `ENABLE TRIGGER` viene quindi utilizzato per abilitare nuovamente il trigger.

```sql
CREATE TABLE dbo.trig_example (
  id INT,
  name VARCHAR(12),
  salary MONEY) ;
GO
-- Create the trigger.
CREATE TRIGGER dbo.trig1 ON dbo.trig_example FOR INSERT
AS
IF (SELECT COUNT(*) FROM INSERTED
WHERE salary > 100000) > 0
BEGIN
    print 'TRIG1 Error: you attempted to insert a salary > $100,000'
    ROLLBACK TRANSACTION
END ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (1,'Pat Smith',100001) ;
GO
-- Disable the trigger.
ALTER TABLE dbo.trig_example DISABLE TRIGGER trig1 ;
GO
-- Try an insert that would typically violate the trigger.
INSERT INTO dbo.trig_example VALUES (2,'Chuck Jones',100001) ;
GO
-- Re-enable the trigger.
ALTER TABLE dbo.trig_example ENABLE TRIGGER trig1 ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (3,'Mary Booth',100001) ;
GO
```

### <a name="online-operations"></a><a name="online"></a>Operazioni online

#### <a name="a-online-index-rebuild-using-low-priority-wait-options"></a>R. Ricompilazione dell'indice online usando le opzioni di attesa con priorità bassa

L'esempio seguente illustra come eseguire la ricompilazione di un indice online specificando le opzioni di attesa con priorità bassa.

**SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE T1
REBUILD WITH
(
    PAD_INDEX = ON,
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES,
                                         ABORT_AFTER_WAIT = BLOCKERS ) )
) ;
```

#### <a name="b-online-alter-column"></a>B. Modifica colonna online

L'esempio seguente illustra come eseguire un'operazione di modifica colonna con l'opzione ONLINE.

**SI APPLICA A**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy
    ALTER COLUMN column_a DECIMAL (5, 2) WITH (ONLINE = ON) ;
GO
sp_help doc_exy;
DROP TABLE dbo.doc_exy ;
GO
```

### <a name="system-versioning"></a><a name="system_versioning"></a> Controllo delle versioni di sistema

I quattro esempi riportati di seguito consentono di acquisire familiarità con la sintassi per l'uso del controllo delle versioni di sistema. Per altre istruzioni, vedere [Introduzione alle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md).

**SI APPLICA A**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

#### <a name="a-add-system-versioning-to-existing-tables"></a>R. Aggiungere il controllo delle versioni di sistema a tabelle esistenti

Nell'esempio seguente viene illustrato come aggiungere il controllo delle versioni di sistema a una tabella esistente e creare una tabella di cronologia futura. In questo esempio si presuppone l'esistenza di una tabella denominata `InsurancePolicy` con una chiave primaria definita. In questo esempio si popolano le colonne periodo appena create per il controllo delle versioni del sistema usando valori predefiniti per l'ora di inizio e di fine in quanto questi valori non possono essere Null. In questo esempio viene usata la clausola HIDDEN per evitare impatti sulle applicazioni esistenti che interagiscono con la tabella corrente. Viene anche usato HISTORY_RETENTION_PERIOD disponibile solo nel [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

```sql
--Alter non-temporal table to define periods for system versioning
ALTER TABLE InsurancePolicy
ADD PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime),
SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
    DEFAULT SYSUTCDATETIME(),
SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
    DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.99999999') ;

--Enable system versioning with 1 year retention for historical data
ALTER TABLE InsurancePolicy
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 1 YEAR)) ;
```

#### <a name="b-migrate-an-existing-solution-to-use-system-versioning"></a>B. Eseguire la migrazione di una soluzione esistente per usare il controllo delle versioni di sistema

Nell'esempio seguente viene illustrato come eseguire la migrazione per il controllo delle versioni di sistema da una soluzione che usa i trigger per simulare il supporto temporale. In questo esempio si presuppone l'esistenza di una soluzione in cui siano usate una tabella `ProjectTask` e una tabella `ProjectTaskHistory` per la soluzione esistente, vale a dire le colonne `Changed Date` e `Revised Date` per i periodi. Tali colonne di periodo non devono usare il tipo di dati `datetime2` e la tabella `ProjectTask` deve avere una chiave primaria definita.

```sql
-- Drop existing trigger
DROP TRIGGER ProjectTask_HistoryTrigger;

-- Adjust the schema for current and history table
-- Change data types for existing period columns
ALTER TABLE ProjectTask ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTask ALTER COLUMN [Revised Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Revised Date] datetime2 NOT NULL ;

-- Add SYSTEM_TIME period and set system versioning with linking two existing tables
-- (a certain set of data checks happen in the background)
ALTER TABLE ProjectTask
ADD PERIOD FOR SYSTEM_TIME ([Changed Date], [Revised Date])

ALTER TABLE ProjectTask
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))
```

#### <a name="c-disabling-and-re-enabling-system-versioning-to-change-table-schema"></a>C. Disabilitazione e riabilitazione del controllo delle versioni di sistema per modificare lo schema tabella

In questo esempio viene illustrato come disabilitare il controllo delle versioni di sistema nella tabella `Department`, aggiungere una colonna e riabilitare il controllo delle versioni di sistema. Per modificare lo schema tabella, è necessario disabilitare il controllo delle versioni di sistema. Eseguire questi passaggi all'interno di una transazione per impedire l'aggiornamento di entrambe le tabelle durante l'aggiornamento dello schema tabella. In questo modo, gli amministratori di database non dovranno verificare la coerenza dei dati quando il controllo delle versioni di sistema viene riabilitato e si garantiscono prestazioni migliori. Per altre attività come la creazione delle statistiche, il cambio delle partizioni o l'applicazione della compressione a una o entrambe le tabelle, non è necessaria la disabilitazione del controllo delle versioni di sistema.

```sql
BEGIN TRAN
/* Takes schema lock on both tables */
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
/* expand table schema for temporal table */
ALTER TABLE Department  
     ADD Col5 int NOT NULL DEFAULT 0 ;
/* Expand table schema for history table */
ALTER TABLE DepartmentHistory
    ADD Col5 int NOT NULL DEFAULT 0 ;
/* Re-establish versioning again*/
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE=dbo.DepartmentHistory,
                                 DATA_CONSISTENCY_CHECK = OFF)) ;
COMMIT
```

#### <a name="d-removing-system-versioning"></a>D. Rimozione del controllo delle versioni di sistema

In questo esempio viene illustrato come rimuovere completamente il controllo delle versioni di sistema della tabella Department ed eliminare la tabella `DepartmentHistory`. Facoltativamente, è anche possibile eliminare le colonne periodo usate dal sistema per registrare informazioni sul controllo delle versioni di sistema. Non è possibile eliminare le tabelle `Department` o `DepartmentHistory` mentre il controllo delle versioni di sistema è abilitato.

```sql
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
ALTER TABLE Department
DROP PERIOD FOR SYSTEM_TIME ;
DROP TABLE DepartmentHistory ;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Negli esempi da A a C riportati di seguito viene usata la tabella `FactResellerSales` nel database [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].

### <a name="a-determining-if-a-table-is-partitioned"></a>R. Determinazione di una tabella partizionata

Tramite la query seguente vengono restituite una o più righe se la tabella `FactResellerSales` è partizionata. Se la tabella non è partizionata, non viene restituita alcuna riga.

```sql
SELECT * FROM sys.partitions AS p
JOIN sys.tables AS t
    ON p.object_id = t.object_id
WHERE p.partition_id IS NOT NULL
    AND t.name = 'FactResellerSales' ;
```

### <a name="b-determining-boundary-values-for-a-partitioned-table"></a>B. Determinazione dei valori limite per una tabella partizionata

Tramite la query seguente vengono restituiti i valori limite per ogni partizione nella tabella `FactResellerSales` .

```sql
SELECT t.name AS TableName, i.name AS IndexName, p.partition_number,
    p.partition_id, i.data_space_id, f.function_id, f.type_desc,
    r.boundary_id, r.value AS BoundaryValue
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN  sys.partition_schemes AS s
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'FactResellerSales' AND i.type <= 1
ORDER BY p.partition_number ;
```

### <a name="c-determining-the-partition-column-for-a-partitioned-table"></a>C. Determinazione della colonna di partizione per una tabella partizionata

Tramite la query seguente viene restituito il nome della colonna di partizionamento per la tabella. `FactResellerSales`.

```sql
SELECT t.object_id AS Object_ID, t.name AS TableName,
    ic.column_id as PartitioningColumnID, c.name AS PartitioningColumnName
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.columns AS c
    ON t.object_id = c.object_id
JOIN sys.partition_schemes AS ps
    ON ps.data_space_id = i.data_space_id
JOIN sys.index_columns AS ic
    ON ic.object_id = i.object_id
    AND ic.index_id = i.index_id AND ic.partition_ordinal > 0
WHERE t.name = 'FactResellerSales'
AND i.type <= 1
AND c.column_id = ic.column_id ;
```

### <a name="d-merging-two-partitions"></a>D. Unione di due partizioni

Nell'esempio seguente si uniscono due partizioni in una tabella.

La tabella `Customer` presenta la definizione seguente:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100))) ;
```

Il comando seguente unisce i limiti delle partizioni 10 e 25.

```sql
ALTER TABLE Customer MERGE RANGE (10);
```

La nuova istruzione DDL per la tabella è la seguente:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 25, 50, 100))) ;
```

### <a name="e-splitting-a-partition"></a>E. Suddivisione di una partizione

Nell'esempio seguente si suddivide una partizione in una tabella.

La tabella `Customer` presenta l'istruzione DDL seguente:

```sql
DROP TABLE Customer;

CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100 ))) ;
```

Il comando seguente consente di creare una nuova partizione associata al valore 75, compresa tra 50 e 100.

```sql
ALTER TABLE Customer SPLIT RANGE (75);
```

La nuova istruzione DDL per la tabella è la seguente:

```sql
CREATE TABLE Customer (
   id INT NOT NULL,
   lastName VARCHAR(20),
   orderCount INT,
   orderDate DATE)
   WITH DISTRIBUTION = HASH(id),
   PARTITION ( orderCount (RANGE LEFT
      FOR VALUES (1, 5, 10, 25, 50, 75, 100))) ;
```

### <a name="f-using-switch-to-move-a-partition-to-a-history-table"></a>F. Uso di SWITCH per spostare una partizione in una tabella di cronologia

Nell'esempio seguente si spostano i dati in una partizione della tabella `Orders` in una partizione della tabella `OrdersHistory`.

La tabella `Orders` presenta l'istruzione DDL seguente:

```sql
CREATE TABLE Orders (
    id INT,
    city VARCHAR (25),
    lastUpdateDate DATE,
    orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01', '2005-01-01', '2006-01-01', '2007-01-01'))) ;
```

In questo esempio la tabella `Orders` contiene le partizioni seguenti. Ogni partizione contiene dati.

|Partition|Contiene dati?|Intervallo limite|
|---------------|---------------|--------------------|
|1|Sì|OrderDate < '2004-01-01'|
|2|Sì|'2004-01-01' <= OrderDate < '2005-01-01'|
|3|Sì|'2005-01-01' <= OrderDate< '2006-01-01'|
|4|Sì|'2006-01-01'<= OrderDate < '2007-01-01'|
|5|Sì|'2007-01-01' <= OrderDate|
| &nbsp; | &nbsp; | &nbsp; |

- Partizione 1 (contiene dati): OrderDate < '2004-01-01'
- Partizione 2 (contiene dati): '2004-01-01' <= OrderDate < '2005-01-01'
- Partizione 3 (contiene dati): '2005-01-01' <= OrderDate< '2006-01-01'
- Partizione 4 (contiene dati): '2006-01-01'<= OrderDate < '2007-01-01'
- Partizione 5 (contiene dati): '2007-01-01' <= OrderDate

La tabella `OrdersHistory` contiene l'istruzione DDL seguente, con colonne identiche e gli stessi nomi di colonna della tabella `Orders`. Sono entrambe tabelle hash distribuite nella colonna `id`.

```sql
CREATE TABLE OrdersHistory (
   id INT,
   city VARCHAR (25),
   lastUpdateDate DATE,
   orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01'))) ;
```

Mentre le colonne e i nomi di colonna devono essere gli stessi, non è necessario che i limiti delle partizioni siano uguali. In questo esempio la tabella `OrdersHistory` contiene le due partizioni seguenti. Entrambe sono vuote:

- Partizione 1 (non contiene dati): OrderDate < '2004-01-01'
- Partizione 2 (vuota): '2004-01-01' <= OrderDate

Per le due tabelle precedenti, il comando seguente sposta tutte le righe con `OrderDate < '2004-01-01'` dalla tabella `Orders` alla tabella `OrdersHistory`.

```sql
ALTER TABLE Orders SWITCH PARTITION 1 TO OrdersHistory PARTITION 1;
```

Di conseguenza, la prima partizione in `Orders` è vuota e la prima partizione in `OrdersHistory` contiene dati. Le tabelle vengono visualizzate come indicato di seguito:

 Tabella `Orders`

- Partizione 1 (vuota): OrderDate < '2004-01-01'
- Partizione 2 (contiene dati): '2004-01-01' <= OrderDate < '2005-01-01'
- Partizione 3 (contiene dati): '2005-01-01' <= OrderDate< '2006-01-01'
- Partizione 4 (contiene dati): '2006-01-01'<= OrderDate < '2007-01-01'
- Partizione 5 (contiene dati): '2007-01-01' <= OrderDate

Tabella `OrdersHistory`

- Partizione 1 (contiene dati): OrderDate < '2004-01-01'
- Partizione 2 (vuota): '2004-01-01' <= OrderDate

Per pulire la tabella `Orders`, è possibile rimuovere la partizione vuota unendo le partizioni 1 e 2 come indicato di seguito:

```sql
ALTER TABLE Orders MERGE RANGE ('2004-01-01');
```

Dopo l'unione, la tabella `Orders` conterrà le partizioni seguenti:

Tabella `Orders`

- Partizione 1 (contiene dati): OrderDate < '2005-01-01'
- Partizione 2 (contiene dati): '2005-01-01' <= OrderDate< '2006-01-01'
- Partizione 3 (contiene dati): '2006-01-01'<= OrderDate < '2007-01-01'
- Partizione 4 (contiene dati): '2007-01-01' <= OrderDate

Si supponga che sia passato un anno e che si voglia archiviare l'anno 2005. È possibile allocare una partizione vuota per l'anno 2005 nella tabella `OrdersHistory` suddividendo la partizione vuota come indicato di seguito:

```sql
ALTER TABLE OrdersHistory SPLIT RANGE ('2005-01-01');
```

Dopo la suddivisione, la tabella `OrdersHistory` conterrà le partizioni seguenti:

 Tabella `OrdersHistory`

- Partizione 1 (contiene dati): OrderDate < '2004-01-01'
- Partizione 2 (vuota): '2004-01-01' < '2005-01-01'
- Partizione 3 (vuota): '2005-01-01' <= OrderDate

## <a name="see-also"></a>Vedere anche

- [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)
- [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
