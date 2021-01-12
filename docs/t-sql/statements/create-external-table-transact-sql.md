---
description: CREATE EXTERNAL TABLE (Transact-SQL)
title: CREATE EXTERNAL TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_EXTERNAL_TABLE
- CREATE EXTERNAL TABLE
- PolyBase, T-SQL
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, table create
- PolyBase, external table
ms.assetid: 6a6fd8fe-73f5-4639-9908-2279031abdec
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2595db017ce9572f6ceb7a4c7eca16f9db9e5abc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095796"
---
# <a name="create-external-table-transact-sql"></a>CREATE EXTERNAL TABLE (Transact-SQL)

Crea una tabella esterna

Questo articolo fornisce la sintassi, gli argomenti, la sezione Osservazioni, le autorizzazioni ed esempi per qualsiasi prodotto SQL scelto.

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Database SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Panoramica: SQL Server

Questo comando crea una tabella esterna per l'accesso di PolyBase ai dati archiviati in un cluster Hadoop o in una tabella esterna PolyBase di Archiviazione BLOB di Azure che fa riferimento ai dati archiviati in un cluster Hadoop o in Archiviazione BLOB di Azure.

**SI APPLICA A**: SQL Server 2016 (o versione successiva)

Usa una tabella esterna con un'origine dati esterna per le query PolyBase. Le origini dati esterne vengono usate per stabilire la connettività e supportano questi casi d'uso principali:

- Virtualizzazione dati e caricamento dati con [PolyBase](../../relational-databases/polybase/polybase-guide.md)
- Operazioni di caricamento bulk con SQL Server o database SQL con `BULK INSERT` o `OPENROWSET`

Vedere anche [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) e [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Create a new external table
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )
    WITH (
        LOCATION = 'folder_or_filepath',
        DATA_SOURCE = external_data_source_name,
        FILE_FORMAT = external_file_format_name
        [ , <reject_options> [ ,...n ] ]
    )
[;]

<reject_options> ::=
{
    | REJECT_TYPE = value | percentage
    | REJECT_VALUE = reject_value
    | REJECT_SAMPLE_VALUE = reject_sample_value
}

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
```

## <a name="arguments"></a>Argomenti

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* è il nome della tabella da creare, composto da una, due o tre parti. Per una tabella esterna, solo i metadati della tabella vengono archiviati in SQL insieme alle statistiche di base relative al file o alla cartella a cui viene fatto riferimento in Hadoop o nell'archiviazione BLOB di Azure. I dati effettivi non vengono spostati o archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!IMPORTANT]
> Per ottenere prestazioni ottimali, se il driver dell'origine dati esterna supporta un nome in tre parti, è consigliabile specificare il nome in tre parti.  

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE supporta la possibilità di configurare il nome di colonna, il tipo di dati, il supporto dei valori Null e le regole di confronto. Non è possibile usare DEFAULT CONSTRAINT nelle tabelle esterne.

Le definizioni di colonna, inclusi i tipi di dati e il numero di colonne, devono corrispondere ai dati nei file esterni. In caso di mancata corrispondenza, le righe di file verranno rifiutate quando si eseguono query sui dati effettivi.

LOCATION = '*folder_or_filepath*' specifica la cartella o il percorso e il nome file per i dati effettivi in Hadoop o Archiviazione BLOB di Azure. Il percorso inizia dalla cartella radice. La cartella radice è il percorso dei dati specificato nell'origine dati esterna.

In SQL Server l'istruzione CREATE EXTERNAL TABLE crea il percorso e la cartella, se non esiste già. Quindi è possibile usare INSERT INTO per esportare i dati da una tabella di SQL Server locale a un'origine dati esterna. Per altre informazioni, vedere l'articolo relativo alle [query di PolyBase](../../relational-databases/polybase/polybase-queries.md).

Se si specifica che LOCATION deve essere una cartella, una query PolyBase che effettua selezioni dalla tabella esterna recupererà i file dalla cartella e da tutte le relative sottocartelle. Proprio come Hadoop, PolyBase non restituisce le cartelle nascoste. Inoltre, non restituisce i file il cui nome file inizia con un carattere di sottolineatura (_) o un punto (.).

In questo esempio, se LOCATION='/webdata/', una query PolyBase restituisce le righe da mydata.txt e mydata2.txt. Non restituirà mydata3.txt perché è un file in una cartella nascosta. Non restituirà _hidden.txt perché è un file nascosto.

![Dati ricorsivi per tabelle esterne](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Dati ricorsivi per tabelle esterne")

Per modificare l'impostazione predefinita e leggere solo dalla directory radice, impostare l'attributo \<polybase.recursive.traversal> su 'false' nel file di configurazione core-site.xml. Questo file si trova in `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Ad esempio: `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name* specifica il nome dell'origine dati esterna che contiene il percorso dei dati esterni. Questo percorso è un file system Hadoop (HDFS), un contenitore BLOB del servizio di archiviazione di Azure o Azure Data Lake Store. Per creare un'origine dati esterna, usare [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name* specifica il nome dell'oggetto formato di file esterno che contiene il tipo di file e il metodo di compressione per i dati esterni. Per creare un formato di file esterno, usare [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Le opzioni REJECT consentono di specificare i parametri di rifiuto che determinano il modo in cui PolyBase gestirà i record *dirty* recuperati dall'origine dati esterna. Un record di dati è considerato "dirty" se i tipi di dati effettivi o il numero di colonne non corrispondono alle definizioni di colonna della tabella esterna.

Se non si specificano o si modificano i valori di rifiuto, PolyBase usa i valori predefiniti. Queste informazioni sui parametri di rifiuto vengono archiviate come metadati aggiuntivi quando si crea una tabella esterna con l'istruzione CREATE EXTERNAL TABLE. Quando un'istruzione SELECT o SELECT INTO SELECT futura seleziona i dati dalla tabella esterna, PolyBase usa le opzioni di rifiuto per determinare il numero o la percentuale di righe che possono essere rifiutate prima che la query effettiva abbia esito negativo. La query restituirà risultati (parziali) finché non viene superata la soglia di rifiuto, quindi ha esito negativo con il messaggio di errore appropriato.

REJECT_TYPE = **value** | percentage indica se l'opzione REJECT_VALUE è specificata come valore letterale o percentuale.

L'opzione REJECT_VALUE di tipo value indica un valore letterale, non una percentuale. La query PolyBase avrà esito negativo se il numero di righe rifiutate supera *reject_value*.

Ad esempio, se REJECT_VALUE = 5 e REJECT_TYPE = value, la query SELECT di PolyBase avrà esito negativo dopo che sono state rifiutate cinque righe.

L'opzione REJECT_VALUE di tipo percentage indica una percentuale, non un valore letterale. La query PolyBase avrà esito negativo se la *percentuale* di righe non eseguite supera il valore *reject_value*. La percentuale di righe con esito negativo viene calcolata a intervalli.

REJECT_VALUE = *reject_value* specifica il valore o la percentuale di righe che possono essere rifiutate prima che la query abbia esito negativo.

Per REJECT_TYPE = value, *reject_value* deve essere un numero intero compreso tra 0 e 2.147.483.647.

Per REJECT_TYPE = percentage, *reject_value* deve essere un valore float compreso tra 0 e 100.

REJECT_SAMPLE_VALUE = *reject_sample_value* è un attributo obbligatorio quando si specifica REJECT_TYPE = percentage. Determina il numero di righe che si deve tentare di recuperare prima che PolyBase ricalcoli la percentuale di righe rifiutate.

Il parametro *reject_sample_value* deve essere un numero intero compreso tra 0 e 2.147.483.647.

Ad esempio, se REJECT_SAMPLE_VALUE = 1000, PolyBase calcola la percentuale di righe con esito negativo dopo che ha tentato di importare 1000 righe dal file di dati esterno. Se la percentuale di righe con esito negativo è inferiore al valore *reject_value*, PolyBase tenterà di recuperare altre 1000 righe. Continua a ricalcolare la percentuale di righe con esito negativo dopo aver tentato di importare ognuna delle 1000 righe aggiuntive.

> [!NOTE]
> Poiché PolyBase calcola la percentuale di righe con esito negativo a intervalli, la percentuale effettiva di tali righe può superare *reject_value*.

Esempio:

Questo esempio illustra come le tre opzioni REJECT interagiscono tra loro. Ad esempio, se REJECT_TYPE = percentage, REJECT_VALUE = 30 e REJECT_SAMPLE_VALUE = 100, potrebbe verificarsi il seguente scenario:

- PolyBase tenta di recuperare le prime 100 righe di cui 25 avranno esito negativo e 75 esito positivo.
- La percentuale di righe con esito negativo viene calcolata come 25%, che è minore del valore di rifiuto pari al 30%. Di conseguenza, PolyBase continuerà a recuperare i dati dall'origine dati esterna.
- PolyBase tenta di caricare le 100 righe successive: questa volta 25 righe hanno esito positivo e 75 righe hanno esito negativo.
- Percentuale di righe con esito negativo viene ricalcolata come 50%. La percentuale di righe con esito negativo ha superato il valore di rifiuto del 30%.
- La query PolyBase ha esito negativo con il 50% di righe rifiutate dopo aver tentato di restituire le prime 200 righe. Si noti che le righe corrispondenti vengono restituite prima che la query PolyBase rilevi che è stata superata la soglia di rifiuto.

SCHEMA_NAME La clausola SCHEMA_NAME consente di eseguire il mapping della definizione di tabella esterna a una tabella in uno schema diverso nel database remoto. Usare questa clausola per evitare ambiguità tra gli schemi che esistono sia nei database locali che in quelli remoti.

OBJECT_NAME La clausola OBJECT_NAME consente di eseguire il mapping della definizione di tabella esterna a una tabella con un nome diverso nel database remoto. Usare questa clausola per evitare ambiguità tra i nomi oggetto che esistono sia nei database locali che in quelli remoti.

DISTRIBUTION Facoltativo. Questo argomento è obbligatorio solo per i database di tipo SHARD_MAP_MANAGER. Questo argomento verifica se una tabella viene trattata come una tabella partizionata o una tabella replicata. Con le tabelle **SHARDED** (*nome colonna*), i dati provenienti da tabelle diverse non si sovrappongono. **REPLICATED** specifica che le tabelle devono avere gli stessi dati in ogni partizione. **ROUND_ROBIN** indica che viene usato un metodo specifico di un'applicazione per distribuire i dati.

## <a name="permissions"></a>Autorizzazioni

Richiede queste autorizzazioni utente:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Si noti che l'account di accesso che crea l'origine dati esterna deve avere le autorizzazioni necessarie per leggere e scrivere nell'origine dati esterna, che si trova in Hadoop o nell'archiviazione BLOB di Azure.

> [!IMPORTANT]
> L'autorizzazione ALTER ANY EXTERNAL DATA SOURCE concede a qualsiasi entità di sicurezza la possibilità di creare e modificare qualsiasi oggetto origine dati esterna e, di conseguenza, la possibilità di accedere a tutte le credenziali con ambito database nel database. Questa autorizzazione deve essere considerata con privilegi elevati e quindi essere concessa solo a entità attendibili nel sistema.

## <a name="error-handling"></a>Gestione degli errori

Durante l'esecuzione dell'istruzione CREATE EXTERNAL TABLE, PolyBase tenta di connettersi all'origine dati esterna. Se il tentativo di connessione non riesce, l'istruzione ha esito negativo e la tabella esterna non viene creata. La conferma dell'esito negativo del comando può richiedere almeno un minuto perché PolyBase ritenta la connessione prima di stabilire che la query non riesce.

## <a name="general-remarks"></a>Osservazioni generali

Negli scenari di query ad hoc, ad esempio SELECT FROM EXTERNAL TABLE, PolyBase archivia le righe recuperate dall'origine dati esterna in una tabella temporanea. Dopo il completamento della query, PolyBase rimuove ed elimina la tabella temporanea. Nessun dato permanente viene archiviato nelle tabelle SQL.

Al contrario, nello scenario di importazione, ad esempio SELECT INTO FROM EXTERNAL TABLE, PolyBase archivia le righe recuperate dall'origine dati esterna come dati permanenti nella tabella SQL. La nuova tabella viene creata durante l'esecuzione della query quando PolyBase recupera i dati esterni.

PolyBase può eseguire il push di parte del calcolo della query in Hadoop per migliorare le prestazioni della query. Questa operazione è chiamata distribuzione del predicato. Per abilitarla, specificare l'opzione del percorso della gestione risorse di Hadoop in [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

È possibile creare numerose tabelle esterne che fanno riferimento alle stesse o ad altre origini dati esterne.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

Non essendo sotto il controllo di gestione diretto di SQL Server, i dati per una tabella possono essere modificati o rimossi in qualsiasi momento da un processo esterno. Per questo motivo, non si garantisce che i risultati delle query in una tabella esterna siano deterministici. La stessa query può restituire risultati diversi ogni volta che viene eseguita su una tabella esterna. Analogamente, una query può non riuscire se i dati esterni vengono spostati o rimossi.

È possibile creare più tabelle esterne che fanno tutte riferimento a origini dati esterne differenti. Se si eseguono contemporaneamente query in diverse origini dati Hadoop, ogni origine Hadoop deve usare la stessa impostazione di configurazione del server di "connettività Hadoop". Ad esempio, non è possibile eseguire contemporaneamente una query su un cluster Cloudera Hadoop e un cluster Hortonworks Hadoop poiché usano impostazioni di configurazione diverse. Per le impostazioni di configurazione e le combinazioni supportate, vedere [Configurazione della connettività di PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Solo queste istruzioni Data Definition Language (DDL) sono consentite per le tabelle esterne:

- CREATE TABLE e DROP TABLE
- CREATE STATISTICS e DROP STATISTICS
- CREATE VIEW e DROP VIEW

Costrutti e operazioni non supportati:

- Il vincolo DEFAULT per le colonne di tabelle esterne
- Operazioni di eliminazione, inserimento e aggiornamento di Data Manipulation Language (DML)

### <a name="query-limitations"></a>Limitazioni delle query

PolyBase può utilizzare al massimo 33.000 file per cartella durante l'esecuzione di 32 query PolyBase simultanee. Questo numero massimo include i file e le sottocartelle presenti in ogni cartella HDFS. Se il livello di concorrenza è inferiore a 32, un utente può eseguire le query PolyBase sulle cartelle in HDFS che contengono più di 33.000 file. È consigliabile usare percorsi brevi per i file esterni e non più di 30.000 file per ogni cartella HDFS. Quando si fa riferimento a troppi file, potrebbe verificarsi un'eccezione di memoria insufficiente in Java Virtual Machine (JVM).

### <a name="table-width-limitations"></a>Limitazioni della larghezza della tabella

PolyBase in SQL Server 2016 ha un limite di larghezza di riga di 32 KB, in base alla dimensione massima di una singola riga valida secondo la definizione della tabella. Se la somma dello schema di colonne è maggiore di 32 KB, PolyBase non sarà in grado di eseguire query sui dati.

### <a name="data-type-limitations"></a>Limitazioni dei tipi di dati

I tipi di dati seguenti non possono essere usati in tabelle esterne di PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Qualsiasi tipo definito dall'utente

## <a name="locking"></a>Blocco

Blocco condiviso per l'oggetto SCHEMARESOLUTION.

## <a name="security"></a>Sicurezza

I file di dati per una tabella esterna vengono archiviati in Hadoop o nell'archiviazione BLOB di Azure. Questi file di dati vengono creati e gestiti dai processi dell'utente, che sarà responsabile della gestione della sicurezza dei dati esterni.

## <a name="examples"></a>Esempi

### <a name="a-create-an-external-table-with-data-in-text-delimited-format"></a>R. Creare una tabella esterna con dati in formato di testo delimitato

Questo esempio illustra tutti i passaggi necessari per creare una tabella esterna i cui dati sono formattati in file di testo delimitato. Definisce un'origine dati esterna *mydatasource* e un formato di file esterno *myfileformat*. A questi oggetti a livello di database viene fatto riferimento nell'istruzione CREATE EXTERNAL TABLE. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) e [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (FIELD_TERMINATOR ='|')
);

CREATE EXTERNAL TABLE ClickStream (
    url varchar(50),
    event_date date,
    user_IP varchar(50)
)
WITH (
        LOCATION='/webdata/employee.tbl',
        DATA_SOURCE = mydatasource,
        FILE_FORMAT = myfileformat
    )
;
```

### <a name="b-create-an-external-table-with-data-in-rcfile-format"></a>B. Creare una tabella esterna con dati in formato RCFILE

Questo esempio illustra tutti i passaggi necessari per creare una tabella esterna i cui dati sono formattati come RCFILE. Definisce un'origine dati esterna *mydatasource_rc* e un formato di file esterno *myfileformat_rc*. A questi oggetti a livello di database viene fatto riferimento nell'istruzione CREATE EXTERNAL TABLE. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) e [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_rc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_rc
WITH (
    FORMAT_TYPE = RCFILE,
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'
)
;

CREATE EXTERNAL TABLE ClickStream_rc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/employee_rc.tbl',
        DATA_SOURCE = mydatasource_rc,
        FILE_FORMAT = myfileformat_rc
    )
;
```

### <a name="c-create-an-external-table-with-data-in-orc-format"></a>C. Creare una tabella esterna con dati in formato ORC

Questo esempio illustra tutti i passaggi necessari per creare una tabella esterna i cui dati sono formattati come file ORC. Definisce un'origine dati esterna mydatasource_orc e un formato di file esterno myfileformat_orc. A questi oggetti a livello di database viene fatto riferimento nell'istruzione CREATE EXTERNAL TABLE. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) e [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_orc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_orc
WITH (
    FORMAT = ORC,
    COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
;

CREATE EXTERNAL TABLE ClickStream_orc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/',
        DATA_SOURCE = mydatasource_orc,
        FILE_FORMAT = myfileformat_orc
    )
;
```

### <a name="d-querying-hadoop-data"></a>D. Eseguire query sui dati Hadoop

Clickstream è una tabella esterna che si connette al file di testo delimitato employee.tbl in un cluster Hadoop. La query seguente è simile a una query eseguita su una tabella standard. Tuttavia, questa query recupera i dati da Hadoop e quindi calcola i risultati.

```sql
SELECT TOP 10 (url) FROM ClickStream WHERE user_ip = 'xxx.xxx.xxx.xxx'
;
```

### <a name="e-join-hadoop-data-with-sql-data"></a>E. Unire i dati Hadoop con i dati SQL

Questa query è simile a un JOIN standard in due tabelle SQL. La differenza è che PolyBase recupera i dati Clickstream da Hadoop e li aggiunge alla tabella UrlDescription. Una tabella è una tabella esterna e l'altra è una tabella SQL standard.

```sql
SELECT url.description
FROM ClickStream cs
JOIN UrlDescription url ON cs.url = url.name
WHERE cs.url = 'msdn.microsoft.com'
;
```

### <a name="f-import-data-from-hadoop-into-a-sql-table"></a>F. Importare dati da Hadoop in una tabella SQL

In questo esempio viene creata una nuova tabella SQL ms_user che archivia in modo permanente il risultato di un join tra la tabella SQL standard *user* e la tabella esterna *ClickStream*.

```sql
SELECT DISTINCT user.FirstName, user.LastName
INTO ms_user
FROM user INNER JOIN (
    SELECT * FROM ClickStream WHERE cs.url = 'www.microsoft.com'
    ) AS ms
ON user.user_ip = ms.user_ip
;
```

### <a name="g-create-an-external-table-for-a-sharded-data-source"></a>G. Creare una tabella esterna per un'origine dati partizionata

In questo esempio viene rieseguito il mapping di una DMV remota a una tabella esterna usando le clausole SCHEMA_NAME e OBJECT_NAME.

```sql
CREATE EXTERNAL TABLE [dbo].[all_dm_exec_requests]([session_id] smallint NOT NULL,
  [request_id] int NOT NULL,
  [start_time] datetime NOT NULL,
  [status] nvarchar(30) NOT NULL,
  [command] nvarchar(32) NOT NULL,
  [sql_handle] varbinary(64),
  [statement_start_offset] int,
  [statement_end_offset] int,
  [cpu_time] int NOT NULL)
WITH
(
  DATA_SOURCE = MyExtSrc,
  SCHEMA_NAME = 'sys',
  OBJECT_NAME = 'dm_exec_requests',
  DISTRIBUTION=ROUND_ROBIN
);
```

### <a name="h-create-an-external-table-for-sql-server"></a>H. Creare una tabella esterna per SQL Server

```sql
     -- Create a Master Key
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';
    GO
     /*  specify credentials to external data source
     *  IDENTITY: user name for external source.
     *  SECRET: password for external source.
     */
     CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
     WITH IDENTITY = 'username', Secret = 'password';
    GO

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
    WITH (
    LOCATION = 'sqlserver://SqlServer',
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = SQLServerCredentials
    );
    GO

    CREATE SCHEMA sqlserver;
    GO

     /* LOCATION: sql server table/view in 'database_name.schema_name.object_name' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE sqlserver.customer(
     C_CUSTKEY INT NOT NULL,
     C_NAME VARCHAR(25) NOT NULL,
     C_ADDRESS VARCHAR(40) NOT NULL,
     C_NATIONKEY INT NOT NULL,
     C_PHONE CHAR(15) NOT NULL,
     C_ACCTBAL DECIMAL(15,2) NOT NULL,
     C_MKTSEGMENT CHAR(10) NOT NULL,
     C_COMMENT VARCHAR(117) NOT NULL
      )
      WITH (
      LOCATION='tpch_10.dbo.customer',
      DATA_SOURCE=SqlServerInstance
     );
```

### <a name="i-create-an-external-table-for-oracle"></a>I. Creare una tabella esterna per Oracle

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

   /*
   * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
   * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
   * CONNECTION_OPTIONS: Specify driver location
   * CREDENTIAL: the database scoped credential, created above.
   */
   CREATE EXTERNAL DATA SOURCE external_data_source_name
   WITH (
     LOCATION = 'oracle://<server address>[:<port>]',
     -- PUSHDOWN = ON | OFF,
     CREDENTIAL = credential_name)

   /*
   * LOCATION: Oracle table/view in '.<schema_name>.<object_name>' format
   * DATA_SOURCE: the external data source, created above.
   */
   CREATE EXTERNAL TABLE customers(
   [O_ORDERKEY] DECIMAL(38) NOT NULL,
   [O_CUSTKEY] DECIMAL(38) NOT NULL,
   [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
   [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
   [O_ORDERDATE] DATETIME2(0) NOT NULL,
   [O_ORDERPRIORITY] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_CLERK] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_SHIPPRIORITY] DECIMAL(38) NOT NULL,
   [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
   )
   WITH (
    LOCATION='.mySchema.customer',
    DATA_SOURCE= external_data_source_name
   );
```

### <a name="j-create-an-external-table-for-teradata"></a>J. Creare una tabella esterna per Teradata

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = teradata://<server address>[:<port>],
   -- PUSHDOWN = ON | OFF,
    CREDENTIAL =credential_name
    );


     /* LOCATION: Teradata table/view in '<database_name>.<object_name>' format
      * DATA_SOURCE: the external data source, created above.
      */
     CREATE EXTERNAL TABLE customer(
      L_ORDERKEY INT NOT NULL,
      L_PARTKEY INT NOT NULL,
     L_SUPPKEY INT NOT NULL,
     L_LINENUMBER INT NOT NULL,
     L_QUANTITY DECIMAL(15,2) NOT NULL,
     L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
     L_DISCOUNT DECIMAL(15,2) NOT NULL,
     L_TAX DECIMAL(15,2) NOT NULL,
     L_RETURNFLAG CHAR NOT NULL,
     L_LINESTATUS CHAR NOT NULL,
     L_SHIPDATE DATE NOT NULL,
     L_COMMITDATE DATE NOT NULL,
     L_RECEIPTDATE DATE NOT NULL,
     L_SHIPINSTRUCT CHAR(25) NOT NULL,
     L_SHIPMODE CHAR(10) NOT NULL,
     L_COMMENT VARCHAR(44) NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

### <a name="k-create-an-external-table-for-mongodb"></a>K. Creare una tabella esterna per MongoDB

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

     /* LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = mongodb://<server>[:<port>],
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = credential_name
    );

     /* LOCATION: MongoDB table/view in '<database_name>.<schema_name>.<object_name>' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE customers(
     [O_ORDERKEY] DECIMAL(38) NOT NULL,
     [O_CUSTKEY] DECIMAL(38) NOT NULL,
     [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
     [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
     [O_ORDERDATE] DATETIME2(0) NOT NULL,
     [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

## <a name="see-also"></a>Vedere anche

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Database SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Panoramica: database SQL di Azure

Nel database SQL di Azure crea una tabella esterna per [query elastiche](/azure/sql-database/sql-database-elastic-query-overview/) (in anteprima).


Vedere anche [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Create a table for use with elastic query  
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```

## <a name="arguments"></a>Argomenti

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* è il nome della tabella da creare, composto da una, due o tre parti. Per una tabella esterna, solo i metadati della tabella vengono archiviati in SQL insieme alle statistiche di base relative al file o alla cartella a cui viene fatto riferimento nel database SQL di Azure. I dati effettivi non vengono spostati o archiviati nel database SQL di Azure.

> [!IMPORTANT]
> Per ottenere prestazioni ottimali, se il driver dell'origine dati esterna supporta un nome in tre parti, è consigliabile specificare il nome in tre parti.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE supporta la possibilità di configurare il nome di colonna, il tipo di dati, il supporto dei valori Null e le regole di confronto. Non è possibile usare DEFAULT CONSTRAINT nelle tabelle esterne.

> [!NOTE]
> `Text`, `nText` e `XML` non sono tipi di dati supportati per le colonne nelle tabelle esterne per il database SQL di Azure.

Le definizioni di colonna, inclusi i tipi di dati e il numero di colonne, devono corrispondere ai dati nei file esterni. In caso di mancata corrispondenza, le righe di file verranno rifiutate quando si eseguono query sui dati effettivi.

Opzioni per la tabella esterna partizionata

Specifica l'origine dati esterna (un'origine dati non SQL Server) e un metodo di distribuzione per la [query elastica](/azure/azure-sql/database/elastic-query-overview).

DATA_SOURCE La clausola DATA_SOURCE definisce l'origine dati esterna (una mappa partizioni) usata per la tabella esterna. Per un esempio, vedere [Creare tabelle esterne](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

SCHEMA_NAME e OBJECT_NAME Le clausole SCHEMA_NAME e OBJECT_NAME eseguono il mapping della definizione della tabella esterna a una tabella in uno schema diverso. Se queste clausole vengono omesse, si presuppone che lo schema dell'oggetto remoto sia "dbo" e che il relativo nome sia identico al nome della tabella esterna in fase di definizione. Questo è utile se il nome della tabella remota è già in uso nel database in cui si vuole creare la tabella esterna. Ad esempio, si vuole definire una tabella esterna per ottenere una visualizzazione aggregata delle viste del catalogo o delle viste a gestione dinamica (DMV) nel livello dati con scalabilità orizzontale. Poiché le viste del catalogo e le DMV esistono già localmente, non sarà possibile usare i rispettivi nomi per la definizione della tabella esterna. Usare invece un nome diverso e usare il nome della vista del catalogo o della vista DMV nelle clausole SCHEMA_NAME e/o OBJECT_NAME. Per un esempio, vedere [Creare tabelle esterne](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

DISTRIBUTION La clausola DISTRIBUTION specifica la distribuzione dei dati usata per questa tabella. Query Processor utilizza le informazioni fornite nella clausola DISTRIBUTION per generare i piani di query più efficienti.

- SHARDED indica che i dati sono partizionati orizzontalmente tra i database. La chiave di partizionamento per la distribuzione dei dati è il parametro <sharding_column_name>.
- REPLICATED indica che in ogni database sono presenti copie identiche della tabella. Sarà quindi necessario assicurarsi che le repliche siano identiche in tutti i database.
- ROUND_ROBIN indica che la tabella è partizionata orizzontalmente con un metodo di distribuzione dipendente dall'applicazione.

## <a name="permissions"></a>Autorizzazioni

Gli utenti con accesso alla tabella esterna ottengono automaticamente l'accesso alle tabelle remote sottostanti con le credenziali specificate nella definizione dell'origine dati esterna. Evitare l'elevazione dei privilegi indesiderata mediante le credenziali dell'origine dati esterna. Usare GRANT o REVOKE per la tabella esterna, come se fosse una tabella comune. Dopo aver definito l'origine dati esterna e le tabelle esterne, è ora possibile usare la sintassi T-SQL completa sulle tabelle esterne.

## <a name="error-handling"></a>Gestione degli errori

Durante l'esecuzione dell'istruzione CREATE EXTERNAL TABLE, se il tentativo di connessione non riesce, l'istruzione ha esito negativo e la tabella esterna non viene creata. La conferma dell'esito negativo del comando può richiedere almeno un minuto perché il database SQL ritenta la connessione prima di determinare l'esito negativo della query.

## <a name="general-remarks"></a>Osservazioni generali

Negli scenari di query ad hoc, ad esempio SELECT FROM EXTERNAL TABLE, il database SQL archivia le righe recuperate dall'origine dati esterna in una tabella temporanea. Dopo il completamento della query, il database SQL rimuove ed elimina la tabella temporanea. Nessun dato permanente viene archiviato nelle tabelle SQL.

Al contrario, nello scenario di importazione, ad esempio SELECT INTO FROM EXTERNAL TABLE, il database SQL archivia le righe recuperate dall'origine dati esterna come dati permanenti nella tabella SQL. La nuova tabella viene creata durante l'esecuzione della query quando il database SQL recupera i dati esterni.

È possibile creare numerose tabelle esterne che fanno riferimento alle stesse o ad altre origini dati esterne.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

L'accesso ai dati tramite una tabella esterna non rispetta la semantica di isolamento all'interno di SQL Server. Ciò significa che l'esecuzione di query su una tabella esterna non impone alcun blocco o isolamento dello snapshot. I dati restituiti possono quindi cambiare se i dati nell'origine dati esterna cambiano.  La stessa query può restituire risultati diversi ogni volta che viene eseguita su una tabella esterna. Analogamente, una query può non riuscire se i dati esterni vengono spostati o rimossi.

È possibile creare più tabelle esterne che fanno tutte riferimento a origini dati esterne differenti.

Solo queste istruzioni Data Definition Language (DDL) sono consentite per le tabelle esterne:

- CREATE TABLE e DROP TABLE
- CREATE VIEW e DROP VIEW

Costrutti e operazioni non supportati:

- Il vincolo DEFAULT per le colonne di tabelle esterne
- Operazioni di eliminazione, inserimento e aggiornamento di Data Manipulation Language (DML)

Solo i predicati letterali definiti in una query possono essere propagati nell'origine dati esterna. Questo rappresenta una differenza rispetto ai server collegati e all'accesso a posizioni in cui è possibile usare predicati determinati durante l'esecuzione di query, ad esempio in combinazione con un ciclo annidato in un piano di query. Ciò porta spesso alla copia in locale dell'intera tabella esterna e quindi al join della tabella stessa.

```sql
  \\ Assuming External.Orders is an external table and Customer is a local table.
  \\ This query  will copy the whole of the external locally as the predicate needed
  \\ to filter isn't known at compile time. Its only known during execution of the query
  
  SELECT Orders.OrderId, Orders.OrderTotal
    FROM External.Orders
   WHERE CustomerId in (SELECT TOP 1 CustomerId
                          FROM Customer
                          WHERE CustomerName = 'MyCompany')
```

L'uso di tabelle esterne impedisce l'uso del parallelismo nel piano di query.

Le tabelle esterne vengono implementate come query remote e, di conseguenza, il numero stimato di righe restituite è in genere pari a 1000. Esistono altre regole basate sul tipo di predicato usato per filtrare la tabella esterna. Si tratta di stime basate su regole anziché di stime basate sui dati effettivi della tabella esterna. Lo strumento di ottimizzazione non accede all'origine dati remota per ottenere una stima più accurata.

### <a name="data-type-limitations"></a>Limitazioni dei tipi di dati

I tipi di dati seguenti non possono essere usati in tabelle esterne di PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Qualsiasi tipo definito dall'utente

## <a name="locking"></a>Blocco

Blocco condiviso per l'oggetto SCHEMARESOLUTION.

## <a name="examples"></a>Esempi

### <a name="a-create-external-table-for-azure-sql-database"></a>R. Creare una tabella esterna per il database SQL di Azure

```sql
CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
( [CustomerID] [int] NOT NULL,
  [CustomerName] [varchar](50) NOT NULL,
  [Company] [varchar](50) NOT NULL)
WITH
( DATA_SOURCE = MyElasticDBQueryDataSrc)
```

## <a name="see-also"></a>Vedere anche

- [Panoramica delle query elastiche del database SQL di Azure](/azure/sql-database/sql-database-elastic-query-overview)
- [Creazione di report tra database cloud con scalabilità orizzontale](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning)
- [Introduzione alle query tra database (partizionamento verticale)](/azure/sql-database/sql-database-elastic-query-getting-started-vertical)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Panoramica: Azure Synapse Analytics

Usare una tabella esterna per:

- Eseguire query sui dati di Hadoop o dell'archiviazione BLOB di Azure con istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Importare e archiviare i dati da Hadoop o dall'archiviazione BLOB di Azure.
- Importare e archiviare dati da Azure Data Lake Store.

Vedere anche [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) e [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).  

## <a name="syntax"></a>Sintassi

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
    | REJECTED_ROW_LOCATION = '/REJECT_Directory'
}  
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    ( <column_definition> [ ,...n ] )   
    WITH ( 
        LOCATION = 'folder_or_filepath',   
        DATA_SOURCE = external_data_source_name,   
        FILE_FORMAT = external_file_format_name   
    )   
[;]   
<column_definition> ::= 
column_name <data_type> 
    [ COLLATE collation_name ] 
```
---

## <a name="arguments"></a>Argomenti

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* è il nome della tabella da creare, composto da una, due o tre parti. Per una tabella esterna, solo i metadati della tabella, insieme alle statistiche di base relative al file o alla cartella a cui viene fatto riferimento in Azure Data Lake, in Hadoop o nell'archiviazione BLOB di Azure. Quando vengono create tabelle esterne, i dati effettivi non vengono spostati né archiviati.

> [!IMPORTANT]
> Per ottenere prestazioni ottimali, se il driver dell'origine dati esterna supporta un nome in tre parti, è consigliabile specificare il nome in tre parti.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE supporta la possibilità di configurare il nome di colonna, il tipo di dati, il supporto dei valori Null e le regole di confronto. Non è possibile usare DEFAULT CONSTRAINT nelle tabelle esterne.

> [!NOTE]
> `Text`, `nText` e `XML` non sono tipi di dati supportati per le colonne nelle tabelle esterne per Azure SQL Data Warehouse.

Le definizioni di colonna, inclusi i tipi di dati e il numero di colonne, devono corrispondere ai dati nei file esterni. In caso di mancata corrispondenza, le righe di file verranno rifiutate quando si eseguono query sui dati effettivi.

LOCATION = '*folder_or_filepath*' specifica la cartella o il percorso e il nome file per i dati effettivi in Azure Data Lake, Hadoop o Archiviazione BLOB di Azure. Il percorso inizia dalla cartella radice. La cartella radice è il percorso dei dati specificato nell'origine dati esterna. L'istruzione [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crea il percorso e la cartella, se non esistono. `CREATE EXTERNAL TABLE` non crea il percorso e la cartella.

Se si specifica che LOCATION deve essere una cartella, una query PolyBase che effettua selezioni dalla tabella esterna recupererà i file dalla cartella e da tutte le relative sottocartelle. Proprio come Hadoop, PolyBase non restituisce le cartelle nascoste. Inoltre, non restituisce i file il cui nome file inizia con un carattere di sottolineatura (_) o un punto (.).

In questo esempio, se LOCATION='/webdata/', una query PolyBase restituisce le righe da mydata.txt e mydata2.txt. Non restituirà mydata3.txt perché è una sottocartella di una cartella nascosta. Non restituirà _hidden.txt perché è un file nascosto.

![Dati ricorsivi per tabelle esterne](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Dati ricorsivi per tabelle esterne")

Per modificare l'impostazione predefinita e leggere solo dalla directory radice, impostare l'attributo \<polybase.recursive.traversal> su 'false' nel file di configurazione core-site.xml. Questo file si trova in `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Ad esempio: `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name* specifica il nome dell'origine dati esterna che contiene il percorso dei dati esterni. Questo percorso è in Azure Data Lake. Per creare un'origine dati esterna, usare [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name* specifica il nome dell'oggetto formato di file esterno che contiene il tipo di file e il metodo di compressione per i dati esterni. Per creare un formato di file esterno, usare [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Le opzioni REJECT consentono di specificare i parametri di rifiuto che determinano il modo in cui PolyBase gestirà i record *dirty* recuperati dall'origine dati esterna. Un record di dati è considerato "dirty" se i tipi di dati effettivi o il numero di colonne non corrispondono alle definizioni di colonna della tabella esterna.

Se non si specificano o si modificano i valori di rifiuto, PolyBase usa i valori predefiniti. Queste informazioni sui parametri di rifiuto vengono archiviate come metadati aggiuntivi quando si crea una tabella esterna con l'istruzione CREATE EXTERNAL TABLE. Quando un'istruzione SELECT o SELECT INTO SELECT futura seleziona i dati dalla tabella esterna, PolyBase usa le opzioni di rifiuto per determinare il numero o la percentuale di righe che possono essere rifiutate prima che la query effettiva abbia esito negativo. La query restituirà risultati (parziali) finché non viene superata la soglia di rifiuto, quindi ha esito negativo con il messaggio di errore appropriato.

REJECT_TYPE = **value** | percentage indica se l'opzione REJECT_VALUE è specificata come valore letterale o percentuale.

L'opzione REJECT_VALUE di tipo value indica un valore letterale, non una percentuale. La query PolyBase avrà esito negativo se il numero di righe rifiutate supera *reject_value*.

Ad esempio, se REJECT_VALUE = 5 e REJECT_TYPE = value, la query SELECT di PolyBase avrà esito negativo dopo che sono state rifiutate cinque righe.

L'opzione REJECT_VALUE di tipo percentage indica una percentuale, non un valore letterale. La query PolyBase avrà esito negativo se la *percentuale* di righe non eseguite supera il valore *reject_value*. La percentuale di righe con esito negativo viene calcolata a intervalli.

REJECT_VALUE = *reject_value* specifica il valore o la percentuale di righe che possono essere rifiutate prima che la query abbia esito negativo.

Per REJECT_TYPE = value, *reject_value* deve essere un numero intero compreso tra 0 e 2.147.483.647.

Per REJECT_TYPE = percentage, *reject_value* deve essere un valore float compreso tra 0 e 100.

REJECT_SAMPLE_VALUE = *reject_sample_value* è un attributo obbligatorio quando si specifica REJECT_TYPE = percentage. Determina il numero di righe che si deve tentare di recuperare prima che PolyBase ricalcoli la percentuale di righe rifiutate.

Il parametro *reject_sample_value* deve essere un numero intero compreso tra 0 e 2.147.483.647.

Ad esempio, se REJECT_SAMPLE_VALUE = 1000, PolyBase calcola la percentuale di righe con esito negativo dopo che ha tentato di importare 1000 righe dal file di dati esterno. Se la percentuale di righe con esito negativo è inferiore al valore *reject_value*, PolyBase tenterà di recuperare altre 1000 righe. Continua a ricalcolare la percentuale di righe con esito negativo dopo aver tentato di importare ognuna delle 1000 righe aggiuntive.

> [!NOTE]
> Poiché PolyBase calcola la percentuale di righe con esito negativo a intervalli, la percentuale effettiva di tali righe può superare *reject_value*.

Esempio:

Questo esempio illustra come le tre opzioni REJECT interagiscono tra loro. Ad esempio, se REJECT_TYPE = percentage, REJECT_VALUE = 30 e REJECT_SAMPLE_VALUE = 100, potrebbe verificarsi il seguente scenario:

- PolyBase tenta di recuperare le prime 100 righe di cui 25 avranno esito negativo e 75 esito positivo.
- La percentuale di righe con esito negativo viene calcolata come 25%, che è minore del valore di rifiuto pari al 30%. Di conseguenza, PolyBase continuerà a recuperare i dati dall'origine dati esterna.
- PolyBase tenta di caricare le 100 righe successive: questa volta 25 righe hanno esito positivo e 75 righe hanno esito negativo.
- Percentuale di righe con esito negativo viene ricalcolata come 50%. La percentuale di righe con esito negativo ha superato il valore di rifiuto del 30%.
- La query PolyBase ha esito negativo con il 50% di righe rifiutate dopo aver tentato di restituire le prime 200 righe. Si noti che le righe corrispondenti vengono restituite prima che la query PolyBase rilevi che è stata superata la soglia di rifiuto.

REJECTED_ROW_LOCATION = *posizione della directory*

Specifica la directory all'interno dell'origine dati esterna in cui vengono scritte le righe rifiutate e il file di errori corrispondente.
Se il percorso specificato non esiste, PolyBase ne crea uno automaticamente. Viene creata una directory figlio con nome "\_rejectedrows". Il carattere "\_" assicura che la directory venga ignorata da altre attività di elaborazione dati, salvo se indicata in modo esplicito nel parametro del percorso. Questa directory include una cartella creata in base all'ora di inoltro del carico, con il formato AnnoMeseGiorno - OraMinutoSecondo (ad esempio 20180330-173205). In questa cartella vengono scritte due tipi di file, i file _reason (file del motivo) e i file di dati.

Sia i file del motivo che i file di dati hanno il queryID associato all'istruzione CTAS. Poiché i dati e il motivo si trovano in file distinti, i file corrispondenti hanno un suffisso corrispondente.

## <a name="permissions"></a>Autorizzazioni

Richiede queste autorizzazioni utente:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**

> [!NOTE]
> Le autorizzazioni CONTROL DATABASE sono necessarie per creare solo MASTER KEY, DATABASE SCOPED CREDENTIAL ed EXTERNAL DATA SOURCE

Si noti che l'account di accesso che crea l'origine dati esterna deve avere le autorizzazioni necessarie per leggere e scrivere nell'origine dati esterna, che si trova in Hadoop o nell'archiviazione BLOB di Azure.

> [!IMPORTANT]
> L'autorizzazione ALTER ANY EXTERNAL DATA SOURCE concede a qualsiasi entità di sicurezza la possibilità di creare e modificare qualsiasi oggetto origine dati esterna e, di conseguenza, la possibilità di accedere a tutte le credenziali con ambito database nel database. Questa autorizzazione deve essere considerata con privilegi elevati e quindi essere concessa solo a entità attendibili nel sistema.

## <a name="error-handling"></a>Gestione degli errori

Durante l'esecuzione dell'istruzione CREATE EXTERNAL TABLE, PolyBase tenta di connettersi all'origine dati esterna. Se il tentativo di connessione non riesce, l'istruzione ha esito negativo e la tabella esterna non viene creata. La conferma dell'esito negativo del comando può richiedere almeno un minuto perché PolyBase ritenta la connessione prima di stabilire che la query non riesce.

## <a name="general-remarks"></a>Osservazioni generali

Negli scenari di query ad hoc, ad esempio SELECT FROM EXTERNAL TABLE, PolyBase archivia le righe recuperate dall'origine dati esterna in una tabella temporanea. Dopo il completamento della query, PolyBase rimuove ed elimina la tabella temporanea. Nessun dato permanente viene archiviato nelle tabelle SQL.

Al contrario, nello scenario di importazione, ad esempio SELECT INTO FROM EXTERNAL TABLE, PolyBase archivia le righe recuperate dall'origine dati esterna come dati permanenti nella tabella SQL. La nuova tabella viene creata durante l'esecuzione della query quando PolyBase recupera i dati esterni.

PolyBase può eseguire il push di parte del calcolo della query in Hadoop per migliorare le prestazioni della query. Questa operazione è chiamata distribuzione del predicato. Per abilitarla, specificare l'opzione del percorso della gestione risorse di Hadoop in [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

È possibile creare numerose tabelle esterne che fanno riferimento alle stesse o ad altre origini dati esterne.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

Poiché non si trovano sotto il controllo di gestione diretto di Azure Synapse, i dati per una tabella possono essere modificati o rimossi in qualsiasi momento da un processo esterno. Per questo motivo, non si garantisce che i risultati delle query in una tabella esterna siano deterministici. La stessa query può restituire risultati diversi ogni volta che viene eseguita su una tabella esterna. Analogamente, una query può non riuscire se i dati esterni vengono spostati o rimossi.

È possibile creare più tabelle esterne che fanno tutte riferimento a origini dati esterne differenti.

Solo queste istruzioni Data Definition Language (DDL) sono consentite per le tabelle esterne:

- CREATE TABLE e DROP TABLE
- CREATE STATISTICS e DROP STATISTICS
- CREATE VIEW e DROP VIEW

Costrutti e operazioni non supportati:

- Il vincolo DEFAULT per le colonne di tabelle esterne
- Operazioni di eliminazione, inserimento e aggiornamento di Data Manipulation Language (DML)

### <a name="query-limitations"></a>Limitazioni delle query

Si consiglia di non fare riferimento a più di 30.000 file per cartella. Quando si fa riferimento a troppi file, potrebbe verificarsi un'eccezione di memoria insufficiente in Java Virtual Machine (JVM) o un peggioramento delle prestazioni.

### <a name="table-width-limitations"></a>Limitazioni della larghezza della tabella

PolyBase in Azure Data Warehouse ha un limite di larghezza di riga di 1 MB, in base alla dimensione massima di una singola riga valida secondo la definizione della tabella. Se la somma dello schema di colonne è maggiore di 1 MB, PolyBase non sarà in grado di eseguire query sui dati.

### <a name="data-type-limitations"></a>Limitazioni dei tipi di dati

I tipi di dati seguenti non possono essere usati in tabelle esterne di PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Qualsiasi tipo definito dall'utente

## <a name="locking"></a>Blocco

Blocco condiviso per l'oggetto SCHEMARESOLUTION.

## <a name="examples"></a>Esempi

### <a name="a-importing-data-from-adls-gen-2-into-azure-ssdw"></a>R. Importazione di dati da Azure Data Lake Storage Gen 2 in Azure [!INCLUDE[ssDW](../../includes/ssdw-md.md)]. 

Per esempi per Azure Data Lake Storage Gen 1, vedere [Creare un'origine dati esterna](create-external-data-source-transact-sql.md).

```sql

-- These values come from your Azure Active Directory Application used to authenticate to ADLS Gen 2. 
CREATE DATABASE SCOPED CREDENTIAL ADLUser
WITH IDENTITY = '<clientID>@\<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = 'abfss://data@pbasetr.azuredatalakestore.net'
)

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(
    FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
       , STRING_DELIMITER = ''
      , DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'
      , USE_TYPE_DEFAULT = FALSE
      )
)

CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( [ProductKey] [int] NOT NULL,
  [ProductLabel] nvarchar NULL,
  [ProductName] nvarchar NULL )
WITH
(
    LOCATION='/DimProduct/' ,
    DATA_SOURCE = AzureDataLakeStore ,
    FILE_FORMAT = TextFileFormat ,
    REJECT_TYPE = VALUE ,
    REJECT_VALUE = 0
) ;

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey] ) )
AS SELECT * FROM
[dbo].[DimProduct_external] ;
```

## <a name="see-also"></a>Vedere anche

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Piattaforma di strumenti<br />analitici (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Panoramica: Sistema della piattaforma di analisi

Usare una tabella esterna per:

- Eseguire query sui dati di Hadoop o dell'archiviazione BLOB di Azure con istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Importare e archiviare dati da Hadoop o Archiviazione BLOB di Azure nella piattaforma di strumenti analitici.

Vedere anche [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) e [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]

<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
  
}  
```

## <a name="arguments"></a>Argomenti

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* è il nome della tabella da creare, composto da una, due o tre parti. Per una tabella esterna, solo i metadati della tabella vengono archiviati nella piattaforma di strumenti analitici insieme alle statistiche di base relative al file o alla cartella a cui viene fatto riferimento in Hadoop o Archiviazione BLOB di Azure. I dati effettivi non vengono spostati o archiviati nella piattaforma di strumenti analitici.

> [!IMPORTANT]
> Per ottenere prestazioni ottimali, se il driver dell'origine dati esterna supporta un nome in tre parti, è consigliabile specificare il nome in tre parti.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE supporta la possibilità di configurare il nome di colonna, il tipo di dati, il supporto dei valori Null e le regole di confronto. Non è possibile usare DEFAULT CONSTRAINT nelle tabelle esterne.

Le definizioni di colonna, inclusi i tipi di dati e il numero di colonne, devono corrispondere ai dati nei file esterni. In caso di mancata corrispondenza, le righe di file verranno rifiutate quando si eseguono query sui dati effettivi.

LOCATION = '*folder_or_filepath*' specifica la cartella o il percorso e il nome file per i dati effettivi in Hadoop o Archiviazione BLOB di Azure. Il percorso inizia dalla cartella radice. La cartella radice è il percorso dei dati specificato nell'origine dati esterna.

Nella piattaforma di strumenti analitici l'istruzione [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crea il percorso e la cartella, se non esistono. `CREATE EXTERNAL TABLE` non crea il percorso e la cartella.

Se si specifica che LOCATION deve essere una cartella, una query PolyBase che effettua selezioni dalla tabella esterna recupererà i file dalla cartella e da tutte le relative sottocartelle. Proprio come Hadoop, PolyBase non restituisce le cartelle nascoste. Inoltre, non restituisce i file il cui nome file inizia con un carattere di sottolineatura (_) o un punto (.).

In questo esempio, se LOCATION='/webdata/', una query PolyBase restituisce le righe da mydata.txt e mydata2.txt. Non restituirà mydata3.txt perché è una sottocartella di una cartella nascosta. Non restituirà _hidden.txt perché è un file nascosto.

![Dati ricorsivi per tabelle esterne](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Dati ricorsivi per tabelle esterne")

Per modificare l'impostazione predefinita e leggere solo dalla directory radice, impostare l'attributo \<polybase.recursive.traversal> su 'false' nel file di configurazione core-site.xml. Questo file si trova in `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Ad esempio: `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name* specifica il nome dell'origine dati esterna che contiene il percorso dei dati esterni. Questo percorso è un cluster Hadoop o un'archiviazione BLOB di Azure. Per creare un'origine dati esterna, usare [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name* specifica il nome dell'oggetto formato di file esterno che contiene il tipo di file e il metodo di compressione per i dati esterni. Per creare un formato di file esterno, usare [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Le opzioni REJECT consentono di specificare i parametri di rifiuto che determinano il modo in cui PolyBase gestirà i record *dirty* recuperati dall'origine dati esterna. Un record di dati è considerato "dirty" se i tipi di dati effettivi o il numero di colonne non corrispondono alle definizioni di colonna della tabella esterna.

Se non si specificano o si modificano i valori di rifiuto, PolyBase usa i valori predefiniti. Queste informazioni sui parametri di rifiuto vengono archiviate come metadati aggiuntivi quando si crea una tabella esterna con l'istruzione CREATE EXTERNAL TABLE. Quando un'istruzione SELECT o SELECT INTO SELECT futura seleziona i dati dalla tabella esterna, PolyBase usa le opzioni di rifiuto per determinare il numero o la percentuale di righe che possono essere rifiutate prima che la query effettiva abbia esito negativo. La query restituirà risultati (parziali) finché non viene superata la soglia di rifiuto, quindi ha esito negativo con il messaggio di errore appropriato.

REJECT_TYPE = **value** | percentage indica se l'opzione REJECT_VALUE è specificata come valore letterale o percentuale.

L'opzione REJECT_VALUE di tipo value indica un valore letterale, non una percentuale. La query PolyBase avrà esito negativo se il numero di righe rifiutate supera *reject_value*.

Ad esempio, se REJECT_VALUE = 5 e REJECT_TYPE = value, la query SELECT di PolyBase avrà esito negativo dopo che sono state rifiutate cinque righe.

L'opzione REJECT_VALUE di tipo percentage indica una percentuale, non un valore letterale. La query PolyBase avrà esito negativo se la *percentuale* di righe non eseguite supera il valore *reject_value*. La percentuale di righe con esito negativo viene calcolata a intervalli.

REJECT_VALUE = *reject_value* specifica il valore o la percentuale di righe che possono essere rifiutate prima che la query abbia esito negativo.

Per REJECT_TYPE = value, *reject_value* deve essere un numero intero compreso tra 0 e 2.147.483.647.

Per REJECT_TYPE = percentage, *reject_value* deve essere un valore float compreso tra 0 e 100.

REJECT_SAMPLE_VALUE = *reject_sample_value* è un attributo obbligatorio quando si specifica REJECT_TYPE = percentage. Determina il numero di righe che si deve tentare di recuperare prima che PolyBase ricalcoli la percentuale di righe rifiutate.

Il parametro *reject_sample_value* deve essere un numero intero compreso tra 0 e 2.147.483.647.

Ad esempio, se REJECT_SAMPLE_VALUE = 1000, PolyBase calcola la percentuale di righe con esito negativo dopo che ha tentato di importare 1000 righe dal file di dati esterno. Se la percentuale di righe con esito negativo è inferiore al valore *reject_value*, PolyBase tenterà di recuperare altre 1000 righe. Continua a ricalcolare la percentuale di righe con esito negativo dopo aver tentato di importare ognuna delle 1000 righe aggiuntive.

> [!NOTE]
> Poiché PolyBase calcola la percentuale di righe con esito negativo a intervalli, la percentuale effettiva di tali righe può superare *reject_value*.

Esempio:

Questo esempio illustra come le tre opzioni REJECT interagiscono tra loro. Ad esempio, se REJECT_TYPE = percentage, REJECT_VALUE = 30 e REJECT_SAMPLE_VALUE = 100, potrebbe verificarsi il seguente scenario:

- PolyBase tenta di recuperare le prime 100 righe di cui 25 avranno esito negativo e 75 esito positivo.
- La percentuale di righe con esito negativo viene calcolata come 25%, che è minore del valore di rifiuto pari al 30%. Di conseguenza, PolyBase continuerà a recuperare i dati dall'origine dati esterna.
- PolyBase tenta di caricare le 100 righe successive: questa volta 25 righe hanno esito positivo e 75 righe hanno esito negativo.
- Percentuale di righe con esito negativo viene ricalcolata come 50%. La percentuale di righe con esito negativo ha superato il valore di rifiuto del 30%.
- La query PolyBase ha esito negativo con il 50% di righe rifiutate dopo aver tentato di restituire le prime 200 righe. Si noti che le righe corrispondenti vengono restituite prima che la query PolyBase rilevi che è stata superata la soglia di rifiuto.

## <a name="permissions"></a>Autorizzazioni

Richiede queste autorizzazioni utente:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Si noti che l'account di accesso che crea l'origine dati esterna deve avere le autorizzazioni necessarie per leggere e scrivere nell'origine dati esterna, che si trova in Hadoop o nell'archiviazione BLOB di Azure.

> [!IMPORTANT]
> L'autorizzazione ALTER ANY EXTERNAL DATA SOURCE concede a qualsiasi entità di sicurezza la possibilità di creare e modificare qualsiasi oggetto origine dati esterna e, di conseguenza, la possibilità di accedere a tutte le credenziali con ambito database nel database. Questa autorizzazione deve essere considerata con privilegi elevati e quindi essere concessa solo a entità attendibili nel sistema.

## <a name="error-handling"></a>Gestione degli errori

Durante l'esecuzione dell'istruzione CREATE EXTERNAL TABLE, PolyBase tenta di connettersi all'origine dati esterna. Se il tentativo di connessione non riesce, l'istruzione ha esito negativo e la tabella esterna non viene creata. La conferma dell'esito negativo del comando può richiedere almeno un minuto perché PolyBase ritenta la connessione prima di stabilire che la query non riesce.

## <a name="general-remarks"></a>Osservazioni generali

Negli scenari di query ad hoc, ad esempio SELECT FROM EXTERNAL TABLE, PolyBase archivia le righe recuperate dall'origine dati esterna in una tabella temporanea. Dopo il completamento della query, PolyBase rimuove ed elimina la tabella temporanea. Nessun dato permanente viene archiviato nelle tabelle SQL.

Al contrario, nello scenario di importazione, ad esempio SELECT INTO FROM EXTERNAL TABLE, PolyBase archivia le righe recuperate dall'origine dati esterna come dati permanenti nella tabella SQL. La nuova tabella viene creata durante l'esecuzione della query quando PolyBase recupera i dati esterni.

PolyBase può eseguire il push di parte del calcolo della query in Hadoop per migliorare le prestazioni della query. Questa operazione è chiamata distribuzione del predicato. Per abilitarla, specificare l'opzione del percorso della gestione risorse di Hadoop in [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

È possibile creare numerose tabelle esterne che fanno riferimento alle stesse o ad altre origini dati esterne.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

Poiché non si trovano sotto il controllo di gestione diretto dell'appliance, i dati per una tabella possono essere modificati o rimossi in qualsiasi momento da un processo esterno. Per questo motivo, non si garantisce che i risultati delle query in una tabella esterna siano deterministici. La stessa query può restituire risultati diversi ogni volta che viene eseguita su una tabella esterna. Analogamente, una query può non riuscire se i dati esterni vengono spostati o rimossi.

È possibile creare più tabelle esterne che fanno tutte riferimento a origini dati esterne differenti. Se si eseguono contemporaneamente query in diverse origini dati Hadoop, ogni origine Hadoop deve usare la stessa impostazione di configurazione del server di "connettività Hadoop". Ad esempio, non è possibile eseguire contemporaneamente una query su un cluster Cloudera Hadoop e un cluster Hortonworks Hadoop poiché usano impostazioni di configurazione diverse. Per le impostazioni di configurazione e le combinazioni supportate, vedere [Configurazione della connettività di PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Solo queste istruzioni Data Definition Language (DDL) sono consentite per le tabelle esterne:

- CREATE TABLE e DROP TABLE
- CREATE STATISTICS e DROP STATISTICS
- CREATE VIEW e DROP VIEW

Costrutti e operazioni non supportati:

- Il vincolo DEFAULT per le colonne di tabelle esterne
- Operazioni di eliminazione, inserimento e aggiornamento di Data Manipulation Language (DML)

### <a name="query-limitations"></a>Limitazioni delle query

PolyBase può utilizzare al massimo 33.000 file per cartella durante l'esecuzione di 32 query PolyBase simultanee. Questo numero massimo include i file e le sottocartelle presenti in ogni cartella HDFS. Se il livello di concorrenza è inferiore a 32, un utente può eseguire le query PolyBase sulle cartelle in HDFS che contengono più di 33.000 file. È consigliabile usare percorsi brevi per i file esterni e non più di 30.000 file per ogni cartella HDFS. Quando si fa riferimento a troppi file, potrebbe verificarsi un'eccezione di memoria insufficiente in Java Virtual Machine (JVM).

### <a name="table-width-limitations"></a>Limitazioni della larghezza della tabella

PolyBase in SQL Server 2016 ha un limite di larghezza di riga di 32 KB, in base alla dimensione massima di una singola riga valida secondo la definizione della tabella. Se la somma dello schema di colonne è maggiore di 32 KB, PolyBase non sarà in grado di eseguire query sui dati.

In Azure Synapse Analytics questa limitazione è stata aumentata a 1 MB.

### <a name="data-type-limitations"></a>Limitazioni dei tipi di dati

I tipi di dati seguenti non possono essere usati in tabelle esterne di PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Qualsiasi tipo definito dall'utente

## <a name="locking"></a>Blocco

Blocco condiviso per l'oggetto SCHEMARESOLUTION.

## <a name="security"></a>Sicurezza

I file di dati per una tabella esterna vengono archiviati in Hadoop o nell'archiviazione BLOB di Azure. Questi file di dati vengono creati e gestiti dai processi dell'utente, che sarà responsabile della gestione della sicurezza dei dati esterni.

## <a name="examples"></a>Esempi

### <a name="a-join-hdfs-data-with-analytics-platform-system-data"></a>R. Unire i dati HDFS ai dati della piattaforma di strumenti analitici

```sql
SELECT cs.user_ip FROM ClickStream cs
JOIN User u ON cs.user_ip = u.user_ip
WHERE cs.url = 'www.microsoft.com'
;
```

### <a name="b-import-row-data-from-hdfs-into-a-distributed-analytics-platform-system-table"></a>B. Importare i dati delle righe da HDFS in una tabella della piattaforma di strumenti analitici distribuita

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = HASH (url) )
AS SELECT url, event_date, user_ip FROM ClickStream
;
```

### <a name="c-import-row-data-from-hdfs-into-a-replicated-analytics-platform-system-table"></a>C. Importare i dati delle righe da HDFS in una tabella della piattaforma di strumenti analitici replicata

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = REPLICATE )
AS SELECT url, event_date, user_ip
FROM ClickStream
;
```

## <a name="see-also"></a>Vedere anche

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
