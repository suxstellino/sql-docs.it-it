---
description: OPENROWSET (Transact-SQL)
title: OPENROWSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENROWSET_TSQL
- OPENROWSET
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENROWSET function
- remote data access [SQL Server], OPENROWSET
- ad hoc distributed queries
- OPENROWSET function, Transact-SQL
- OPENROWSET statement
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: f47eda43-33aa-454d-840a-bb15a031ca17
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a430643f34c868d99f8e4ed167c5302205027646
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183993"
---
# <a name="openrowset-transact-sql"></a>OPENROWSET (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Include tutte le informazioni di connessione necessarie per l'accesso remoto ai dati da un'origine dati OLE DB. Si tratta di un metodo alternativo per l'accesso alle tabelle di un server collegato e corrisponde a un metodo ad hoc eseguito una sola volta per la connessione e l'accesso ai dati remoti tramite OLE DB. Per ottenere riferimenti più frequenti alle origini dati OLE DB, utilizzare server collegati. Per altre informazioni, vedere [Server collegati &#40;Motore di database&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md). È possibile fare riferimento alla funzione `OPENROWSET` nella clausola FROM di una query come se fosse un nome di tabella. È anche possibile fare riferimento alla funzione `OPENROWSET` come tabella di destinazione di un'istruzione`INSERT`, `UPDATE` o `DELETE`, a seconda delle funzionalità del provider OLE DB. Anche quando la query può restituire più set di risultati, la funzione `OPENROWSET` restituisce solo il primo set.

`OPENROWSET` supporta anche le operazioni bulk tramite un provider BULK predefinito che consente di leggere i dati da un file e restituirli come set di righe.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
OPENROWSET
( { 'provider_name' 
    , { 'datasource' ; 'user_id' ; 'password' | 'provider_string' }
    , {   <table_or_view> | 'query' }
   | BULK 'data_file' ,
       { FORMATFILE = 'format_file_path' [ <bulk_options> ]
       | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB }
} )

<table_or_view> ::= [ catalog. ] [ schema. ] object

<bulk_options> ::=

   [ , DATASOURCE = 'data_source_name' ]

   [ , ERRORFILE = 'file_name' ]
   [ , ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ , MAXERRORS = maximum_errors ]

   [ , FIRSTROW = first_row ]
   [ , LASTROW = last_row ]
   [ , ROWS_PER_BATCH = rows_per_batch ]
   [ , ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) [ UNIQUE ] ]
  
   -- bulk_options related to input file format
   [ , CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ , FORMAT = 'CSV' ]
   [ , FIELDQUOTE = 'quote_characters']
   [ , FORMATFILE = 'format_file_path' ]
   [ , FORMATFILE_DATA_SOURCE = 'data_source_name' ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

### <a name="provider_name"></a>'*provider_name*'
Stringa di caratteri che rappresenta il nome descrittivo (o PROGID) del provider OLE DB specificato nel Registro di sistema. *provider_name* non ha un valore predefinito. Sono esempi di nomi di provider `Microsoft.Jet.OLEDB.4.0`, `SQLNCLI` o `MSDASQL`.

### <a name="datasource"></a>'*datasource*'
Costante stringa che corrisponde a un'origine dati OLE DB specifica. *datasource* è la proprietà DBPROP_INIT_DATASOURCE da passare all'interfaccia IDBProperties del provider per l'inizializzazione di quest'ultimo. In genere questa stringa include il nome del file di database, il nome di un server di database o un nome riconosciuto dal provider per individuare il database o i database.
L'origine dati può essere il percorso di file `C:\SAMPLES\Northwind.mdb'` per il provider `Microsoft.Jet.OLEDB.4.0` o la stringa di connessione `Server=Seattle1;Trusted_Connection=yes;` per il provider `SQLNCLI`.

### <a name="user_id"></a>'*user_id*'
Costante stringa che rappresenta il nome utente passato al provider OLE DB specificato. *user_id* consente di specificare il contesto di sicurezza per la connessione e viene passato come proprietà DBPROP_AUTH_USERID per l'inizializzazione del provider. *user_id* non può essere un ID di accesso di Microsoft Windows.

### <a name="password"></a>'*password*'
Costante stringa che rappresenta la password utente da passare al provider OLE DB. *password* viene passato come proprietà DBPROP_AUTH_PASSWORD durante l'inizializzazione del provider. *password* non può essere una password di Microsoft Windows.

```sql
SELECT a.*
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
                   'C:\SAMPLES\Northwind.mdb';
                   'admin';
                   'password',
                   Customers) AS a;
```

### <a name="provider_string"></a>'*provider_string*'
Stringa di connessione specifica del provider passata come proprietà DBPROP_INIT_PROVIDERSTRING per l'inizializzazione del provider OLE DB. In *provider_string* sono incluse in genere tutte le informazioni di connessione necessarie per inizializzare il provider. Per un elenco di parole chiave riconosciute dal provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, vedere [Proprietà di inizializzazione e di autorizzazione](../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                            Department) AS d;
```

### <a name="table_or_view"></a><table_or_view>
Tabella o vista remota che contiene i dati che devono essere letti da `OPENROWSET`. Può essere un oggetto con nome in tre parti con i componenti seguenti:
- *catalogo* (facoltativo) - Nome del catalogo o del database contenente l'oggetto specificato.
- *schema* (facoltativo) - Nome dello schema o del proprietario dell'oggetto specificato.
- *oggetto* - Nome dell'oggetto che identifica in modo univoco l'oggetto da usare.

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                 AdventureWorks2012.HumanResources.Department) AS d;
```

### <a name="query"></a>'*query*'
Costante stringa inviata al provider ed eseguita da questo. L'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non elabora questa query, ma i risultati della query restituiti dal provider (query pass-through). Le query pass-through risultano utili quando vengono utilizzate in provider che non espongono i dati tabulari tramite i nomi di tabella, ma solo attraverso un linguaggio di comando. Le query pass-through sono supportate nel server remoto, a condizione che il provider di query supporti l'oggetto OLE DB Command e le relative interfacce obbligatorie. Per altre informazioni, vedere [Informazioni di riferimento di SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md).

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT TOP 10 GroupName, Name
     FROM AdventureWorks2012.HumanResources.Department') AS a;
```

### <a name="bulk"></a>BULK
Utilizza il provider BULK per set di righe per OPENROWSET per leggere i dati da un file. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], OPENROWSET è in grado di leggere da un file di dati senza caricare i dati in una tabella di destinazione. Ciò consente di utilizzare OPENROWSET con un'istruzione SELECT semplice.

> [!IMPORTANT]
> Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

Gli argomenti dell'opzione BULK consentono un controllo significativo su dove iniziare e terminare la lettura dei dati, come gestire gli errori e come interpretare i dati. È ad esempio possibile specificare che il file di dati deve essere letto come riga singola, set di righe a colonna singola di tipo **varbinary**, **varchar** o **nvarchar**. Il comportamento predefinito viene illustrato nelle descrizioni degli argomenti seguenti.

 Per informazioni sull'utilizzo dell'opzione BULK, vedere la sezione "Osservazioni" di seguito in questo argomento. Per informazioni sulle autorizzazioni necessarie per l'opzione BULK, vedere la sezione "Autorizzazioni" di seguito in questo argomento.

> [!NOTE]
> Quando utilizzata per importare i dati con il modello di recupero con registrazione completa, OPENROWSET (BULK ...) non ottimizza la registrazione.

Per informazioni sulla preparazione dei dati per le operazioni di importazione bulk, vedere [Preparazione dei dati per l'importazione o l'esportazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

#### <a name="bulk-data_file"></a>BULK '*data_file*'
Percorso completo del file di dati i cui dati devono essere copiati nella tabella di destinazione.

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, data_file può essere presente nell'archivio BLOB di Azure. Per alcuni esempi, vedere [Esempi di accesso bulk ai dati nell'archiviazione BLOB di Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

> [!IMPORTANT]
> Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

#### <a name="bulk-error-handling-options"></a>Opzioni di gestione degli errori BULK

##### <a name="errorfile"></a>ERRORFILE
`ERRORFILE`='*file_name*' specifica il file usato per raccogliere le righe che contengono errori di formattazione e non possono essere convertite in un set di righe OLE DB. Tali righe vengono copiate nel file degli errori dal file di dati così come sono.

Il file di errori viene creato all'inizio dell'esecuzione del comando. Se il file esiste già viene generato un errore. Viene inoltre creato un file di controllo con estensione ERROR.txt. Questo file contiene un riferimento a ogni riga nel file degli errori e fornisce informazioni di diagnostica. Dopo la correzione degli errori, i dati possono essere caricati.
**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], `error_file_path` può essere presente nell'archiviazione BLOB di Azure.

##### <a name="errorfile_data_source_name"></a>ERRORFILE_DATA_SOURCE_NAME
**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Origine dati esterna denominata che punta alla posizione di archiviazione BLOB di Azure del file degli errori rilevati durante l'importazione. L'origine dati esterna deve essere creata tramite l'opzione `TYPE = BLOB_STORAGE` aggiunta in [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

##### <a name="maxerrors"></a>MAXERRORS
`MAXERRORS` =*maximum_errors* specifica il numero massimo di errori di sintassi o righe non conformi, definite nel file di formato, che possono verificarsi prima che OPENROWSET generi un'eccezione. Fino al raggiungimento di MAXERRORS, OPENROWSET ignora ogni riga non conforme, non caricandola, e conteggia la riga non conforme come un errore.

Il valore predefinito per *maximum_errors* è 10.

> [!NOTE]
> `MAX_ERRORS` non si applica ai vincoli CHECK o alla conversione dei tipi di dati **money** e **bigint**.

#### <a name="bulk-data-processing-options"></a>Opzioni di elaborazione dei dati BULK

##### <a name="firstrow"></a>FIRSTROW
`FIRSTROW` =*first_row* specifica il numero della prima riga da caricare. Il valore predefinito è 1. Questo valore indica la prima riga nel file di dati specificato. I numeri di riga sono determinati dal conteggio dei caratteri di terminazione. FIRSTROW è in base 1.

##### <a name="lastrow"></a>LASTROW
`LASTROW` =*last_row* specifica il numero dell'ultima riga da caricare. Il valore predefinito è 0. Questo valore indica l'ultima riga nel file di dati specificato.

##### <a name="rows_per_batch"></a>ROWS_PER_BATCH
`ROWS_PER_BATCH` =*rows_per_batch* specifica il numero approssimativo di righe di dati nel file di dati. Questo valore deve essere dello stesso ordine del numero effettivo di righe.

`OPENROWSET` importa sempre un file di dati come batch singolo. Se tuttavia si specifica *rows_per_batch* con un valore > 0, Query Processor usa il valore di *rows_per_batch* come hint per l'allocazione delle risorse nel piano di query.

Per impostazione predefinita, il valore ROWS_PER_BATCH è sconosciuto. La specifica di ROWS_PER_BATCH = 0 equivale all'omissione di ROWS_PER_BATCH.

##### <a name="order"></a>ORDER
`ORDER` ( { *column* [ ASC | DESC ] } [ ,... *n* ] [ UNIQUE ] ) Suggerimento facoltativo che specifica la modalità di ordinamento dei dati nel file. Per impostazione predefinita, per l'operazione bulk si presume che il file di dati non sia ordinato. Se l'ordine specificato può essere sfruttato da Query Optimizer per generare un piano di query più efficiente, le prestazioni possono migliorare. Di seguito vengono riportati alcuni esempi di situazioni in cui può essere utile specificare l'ordinamento:

- Inserimento di righe in una tabella con un indice cluster, in cui i dati del set di righe sono ordinati in base alla chiave dell'indice cluster.
- Unione del set di righe con un'altra tabella, in cui le colonne di ordinamento e di join corrispondono.
- Aggregazione dei dati del set di righe tramite le colonne dell'ordinamento.
- Utilizzo del set di righe come tabella di origine nella clausola FROM di una query, in cui le colonne di ordinamento e di join corrispondono.

##### <a name="unique"></a>UNIQUE
`UNIQUE` specifica che il file di dati non ha voci duplicate.

Se le righe effettive del file di dati non sono ordinate in base all'ordine specificato o se l'hint UNIQUE viene specificato e sono presenti chiavi duplicate, viene restituito un errore.

Gli alias di colonna sono richiesti quando si utilizza ORDER. L'elenco di alias di colonna deve fare riferimento alla tabella derivata a cui è possibile accedere tramite la clausola BULK. I nomi di colonna specificati nella clausola ORDER si riferiscono a questo elenco di alias di colonna. Non è possibile specificare colonne di tipi valore di grandi dimensioni (**varchar(max)** , **nvarchar(max)** , **varbinary(max)** e **xml**) e di tipi Large Object (LOB) (**text**, **ntext** e **image**).

##### <a name="single_blob"></a>SINGLE_BLOB
Restituisce il contenuto di *data_file* come set di righe a riga singola e a colonna singola di tipo **varbinary(max)** .

> [!IMPORTANT]
> Per l'importazione di dati XML è consigliabile utilizzare solo l'opzione SINGLE_BLOB anziché SINGLE_CLOB e SINGLE_NCLOB, in quanto solo SINGLE_BLOB supporta tutti i tipi di conversione di codifica di Windows.

##### <a name="single_clob"></a>SINGLE_CLOB
Leggendo *data_file* come ASCII, restituisce il contenuto come set di righe a riga singola e colonna singola di tipo **varchar(max)** , usando le regole di confronto del database corrente.

##### <a name="single_nclob"></a>SINGLE_NCLOB
Leggendo *data_file* come UNICODE, restituisce il contenuto come set di righe a riga singola e colonna singola di tipo **varchar(max)** , usando le regole di confronto del database corrente.

```sql
SELECT *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_NCLOB) AS Document;
```

#### <a name="bulk-input-file-format-options"></a>Opzioni di formato del file di input BULK

##### <a name="codepage"></a>CODEPAGE
`CODEPAGE` = { 'ACP' \| 'OEM' \| 'RAW' \| '*tabella_codici*' } Specifica la tabella codici dei dati contenuti nel file di dati. CODEPAGE è pertinente solo se i dati contengono colonne di tipo **char**, **varchar** o **text** con valori carattere maggiori di 127 o minori di 32.

> [!IMPORTANT]
> `CODEPAGE` non è un'opzione supportata in Linux.

> [!NOTE]
> È consigliabile specificare un nome di regole di confronto per ogni colonna in un file di formato tranne quando si vuole assegnare all'opzione 65001 la priorità sulla specifica delle regole di confronto o della tabella codici.

|Valore CODEPAGE|Descrizione|
|--------------------|-----------------|
|ACP|Converte le colonne con tipo di dati **char**, **varchar** o **text** dalla tabella codici ANSI/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) a quella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (predefinito)|Converte le colonne con tipo di dati **char**, **varchar** o **text** dalla tabella codici OEM di sistema a quella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|Non vengono eseguite conversioni tra tabelle codici. Si tratta dell'opzione più rapida.|
|*code_page*|Indica la tabella codici di origine in cui vengono codificati i dati di tipo carattere del file di dati, ad esempio 850.<br /><br /> **Importante** Le versioni precedenti a [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] non supportano la tabella codici 65001 (codifica UTF-8).|

##### <a name="format"></a>FORMAT
`FORMAT` **=** 'CSV' **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Specifica un file di valori separati da virgole conforme allo standard [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

##### <a name="formatfile"></a>FORMATFILE
`FORMATFILE` ='*format_file_path*' Specifica il percorso completo di un file di formato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta due tipi di file di formato: XML e non XML.

Un file di formato è necessario per definire i tipi di colonna nel set di risultati. L'unica eccezione si verifica quando viene specificato SINGLE_CLOB, SINGLE_BLOB o SINGLE_NCLOB. In questo caso, il file di formato non è necessario.

Per informazioni sui file di formato, vedere [Usare un file di formato per l'importazione bulk dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
A partire da [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, format_file_path può essere presente nell'archiviazione BLOB di Azure. Per alcuni esempi, vedere [Esempi di accesso bulk ai dati nell'archiviazione BLOB di Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

##### <a name="fieldquote"></a>FIELDQUOTE
`FIELDQUOTE` **=** 'field_quote' **Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Specifica il carattere da usare come carattere virgolette nel file CSV. Se non viene specificato alcun carattere, viene usato il carattere virgolette (") in base alla definizione dello standard [RFC 4180](https://tools.ietf.org/html/rfc4180).

## <a name="remarks"></a>Osservazioni

È possibile usare `OPENROWSET` per accedere ai dati remoti da origini dati OLE DB solo se l'opzione del Registro di sistema **DisallowAdhocAccess** è impostata esplicitamente su 0 per il provider specificato e l'opzione di configurazione avanzata Ad Hoc Distributed Queries è abilitata. Quando queste opzioni non vengono impostate, il comportamento predefinito non consente l'accesso ad hoc.

Quando si accede alle origini dati OLE DB remote, l'identità dell'account di accesso delle connessioni trusted non viene delegata automaticamente dal server in cui il client è connesso al server su cui viene eseguita la query. È necessario configurare la delega dell'autenticazione.

Se il provider OLE DB supporta più cataloghi e schemi nell'origine dati specificata, è necessario specificare i nomi di catalogo e di schema. I valori per _catalog_ e )_schema_ possono essere omessi se il provider OLE DB non li supporta. Se il provider supporta solo nomi di schema, è necessario specificare un nome composto da due parti nel formato _schema_ **.** _oggetto_. Se il provider supporta solo nomi di catalogo, è necessario specificare un nome composto da tre parti nel formato _catalogo_ **.** _schema_ **.** _oggetto_. È necessario specificare nomi composti da tre parti per le query pass-through che usano il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Per altre informazioni, vedere [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

La funzione `OPENROWSET` non accetta variabili come argomenti.

Qualsiasi chiamata a `OPENDATASOURCE`, `OPENQUERY` r `OPENROWSET` nella clausola `FROM` viene valutata separatamente e indipendentemente da qualsiasi altra chiamata a queste funzioni usate come destinazione dell'aggiornamento, anche se alle due chiamate vengono forniti argomenti identici. In particolare, le condizioni di filtro o join applicate al risultato di una di tali chiamate non hanno effetto sui risultati dell'altra.

## <a name="using-openrowset-with-the-bulk-option"></a>Utilizzo di OPENROWSET con l'opzione BULK

I miglioramenti seguenti apportati a [!INCLUDE[tsql](../../includes/tsql-md.md)] offrono il supporto per la funzione OPENROWSET(BULK…):

- Una clausola FROM usata con `SELECT` può chiamare `OPENROWSET(BULK...)` anziché un nome di tabella. In questo modo, sono disponibili tutte le funzionalità dell'istruzione `SELECT`.

   `OPENROWSET` con l'opzione `BULK` richiede un nome di correlazione, noto anche come alias o variabile di intervallo, nella clausola `FROM`. È possibile specificare alias di colonne. Se non è specificato un elenco di alias di colonna, il file di formato deve contenere nomi di colonna. Se si specificano gli alias di colonna, i nomi di colonna nel file di formato vengono sostituiti, ad esempio:

  - `FROM OPENROWSET(BULK...) AS table_alias`
  - `FROM OPENROWSET(BULK...) AS table_alias(column_alias,...n)`

  > [!IMPORTANT]
  > Se non si aggiunge `AS <table_alias>` viene generato l'errore: Messaggio 491, Livello 16, Stato 1, Riga 20: specificare il nome della correlazione per il set di righe con lettura bulk nella clausola FROM.

- Un'istruzione `SELECT...FROM OPENROWSET(BULK...)` consente di eseguire query direttamente sui dati in un file, senza importare i dati in una tabella. Le istruzioni `SELECT...FROM OPENROWSET(BULK...)` consentono anche di elencare alias di colonna bulk usando un file di formato per specificare nomi di colonna e tipi di dati.
- L'uso di `OPENROWSET(BULK...)` come tabella di origine in un'istruzione `INSERT` o `MERGE` consente di eseguire l'importazione bulk di dati da un file di dati in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Importare dati per operazioni bulk con BULK INSERT o OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).
- Quando l'opzione `OPENROWSET BULK` viene usata con un'istruzione `INSERT`, la clausola BULK supporta gli hint di tabella. Oltre agli hint di tabella normali, ad esempio `TABLOCK`, la clausola `BULK` può accettare gli hint di tabella specializzati seguenti: `IGNORE_CONSTRAINTS` (ignora solo i vincoli `CHECK` e `FOREIGN KEY`), `IGNORE_TRIGGERS`, `KEEPDEFAULTS` e `KEEPIDENTITY`. Per altre informazioni, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).

  Per informazioni su come usare le istruzioni `INSERT...SELECT * FROM OPENROWSET(BULK...)`, vedere [Importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md). Per informazioni sui casi in cui le operazioni di inserimento di righe eseguite durante l'importazione in blocco vengono registrate nel log delle transazioni, vedere [Prerequisiti per la registrazione minima nell'importazione in blocco](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

> [!NOTE]
> Quando si usa `OPENROWSET`, è importante comprendere il modo in cui la rappresentazione viene gestita da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per considerazioni sulla sicurezza, vedere [Importazione di dati per operazioni bulk con BULK INSERT o OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

### <a name="bulk-importing-sqlchar-sqlnchar-or-sqlbinary-data"></a>Importazione bulk di dati SQLCHAR, SQLNCHAR o SQLBINARY

OPENROWSET(BULK...) presuppone che, se non è specificata, la lunghezza massima dei dati SQLCHAR, SQLNCHAR o SQLBINARY non supera 8000 byte. Se i dati in corso di importazione sono in un campo dati di tipo LOB contenente un oggetto **varchar(max)** , **nvarchar(max)** o **varbinary(max)** che supera 8000 byte, è necessario usare un file di formato XML che definisca la lunghezza massima per il campo dati. Per specificare la lunghezza massima, modificare il file di formato dichiarando l'attributo MAX_LENGTH.

> [!NOTE]
> Un file di formato generato automaticamente non specifica la lunghezza o la lunghezza massima per un campo di tipo LOB. Tuttavia, è possibile modificare un file di formato e specificare la lunghezza o la lunghezza massima manualmente.

### <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Esportazione o importazione bulk di documenti SQLXML

Per l'esportazione o l'importazione bulk di dati SQLXML, utilizzare uno dei tipi di dati seguenti nel file di formato.

|Tipo di dati|Effetto|
|---------------|------------|
|SQLCHAR o SQLVARYCHAR|I dati vengono inviati nella tabella codici del client o nella tabella codici implicita delle regole di confronto.|
|SQLNCHAR o SQLNVARCHAR|I dati vengono inviati in formato Unicode.|
|SQLBINARY o SQLVARYBIN|I dati vengono inviati senza conversione.|

## <a name="permissions"></a>Autorizzazioni

Le autorizzazioni `OPENROWSET` sono determinate dalle autorizzazioni del nome utente che viene passato al provider OLE DB. L'uso dell'opzione `BULK` richiede l'autorizzazione `ADMINISTER BULK OPERATIONS` o `ADMINISTER DATABASE BULK OPERATIONS`.

## <a name="examples"></a>Esempi

### <a name="a-using-openrowset-with-select-and-the-sql-server-native-client-ole-db-provider"></a>R. Utilizzo di OPENROWSET con SELECT e il provider OLE DB di SQL Server Native Client

L'esempio seguente usa il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per accedere alla tabella `HumanResources.Department` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] nel server remoto `Seattle1`. (L'utilizzo di SQLNCLI e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reindirizza alla versione più recente del provider OLE DB per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.) Viene usata un'istruzione `SELECT` per definire il set di righe restituito. La stringa del provider contiene le parole chiave `Server` e `Trusted_Connection`. Queste parole chiave sono riconosciute dal provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT GroupName, Name, DepartmentID
      FROM AdventureWorks2012.HumanResources.Department
      ORDER BY GroupName, Name') AS a;
```

### <a name="b-using-the-microsoft-ole-db-provider-for-jet"></a>B. Utilizzo del provider Microsoft OLE DB per Jet

Nell'esempio seguente viene ottenuto l'accesso alla tabella `Customers` del database `Northwind` di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access tramite il provider [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB per Jet.

> [!NOTE]
> Nell'esempio si presuppone che Access sia installato. Per eseguire questo esempio, è necessario installare il database Northwind.

```sql
SELECT CustomerID, CompanyName
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
      'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';
      'admin';'',Customers);
```

> [!IMPORTANT]
> Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="c-using-openrowset-and-another-table-in-an-inner-join"></a>C. Utilizzo di OPENROWSET e di un'altra tabella in un INNER JOIN

Nell'esempio seguente vengono selezionati tutti i dati della tabella `Customers` dell'istanza locale del database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Northwind` e della tabella `Orders` del database `Northwind` di Access archiviato nello stesso computer.

> [!NOTE]
> Nell'esempio si presuppone che Access sia installato. Per eseguire questo esempio, è necessario installare il database Northwind.

```sql
USE Northwind;
GO
SELECT c.*, o.*
FROM Northwind.dbo.Customers AS c
   INNER JOIN OPENROWSET('Microsoft.Jet.OLEDB.4.0',
   'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';'admin';'', Orders)
   AS o
   ON c.CustomerID = o.CustomerID ;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="d-using-openrowset-to-bulk-insert-file-data-into-a-varbinarymax-column"></a>D. Utilizzo di OPENROWSET per eseguire un inserimento bulk dei dati del file in una colonna varbinary(max).

 Nell'esempio seguente viene creata una tabella di piccole dimensioni a titolo dimostrativo e vengono quindi inseriti i dati di un file denominato `Text1.txt` archiviato nella directory radice `C:` in una colonna `varbinary(max)`.

```sql
CREATE TABLE myTable(FileName NVARCHAR(60),
  FileType NVARCHAR(60), Document VARBINARY(max));
GO

INSERT INTO myTable(FileName, FileType, Document)
   SELECT
      'Text1.txt' AS FileName
      , '.txt' AS FileType
      , *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_BLOB) AS Document;
GO
```

> [!IMPORTANT]
> Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="e-using-the-openrowset-bulk-provider-with-a-format-file-to-retrieve-rows-from-a-text-file"></a>E. Utilizzo del provider OPENROWSET BULK con un file di formato per recuperare le righe da un file di testo

Nell'esempio seguente viene utilizzato un file di formato per recuperare le righe da un file di testo delimitato da tabulazioni, `values.txt` contenente i dati seguenti:

```
1     Data Item 1
2     Data Item 2
3     Data Item 3
```

Il file di formato, `values.fmt`, descrive le colonne in `values.txt`:

```
9.0
2  
1  SQLCHAR  0  10 "\t"        1  ID                      SQL_Latin1_General_Cp437_BIN
2  SQLCHAR  0  40 "\r\n"      2  Description        SQL_Latin1_General_Cp437_BIN
```

Di seguito è illustrata la query che recupera tali dati:

```sql
SELECT a.* FROM OPENROWSET( BULK 'c:\test\values.txt',
   FORMATFILE = 'c:\test\values.fmt') AS a;
```

> [!IMPORTANT]
> Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="f-specifying-a-format-file-and-code-page"></a>F. Indicazione di un file di formato e di una tabella codici

L'esempio seguente illustra come usare sia il file di formato che la tabella codici contemporaneamente.

```sql
INSERT INTO MyTable SELECT a.* FROM
OPENROWSET (BULK N'D:\data.csv', FORMATFILE =
    'D:\format_no_collation.txt', CODEPAGE = '65001') AS a;
```

### <a name="g-accessing-data-from-a-csv-file-with-a-format-file"></a>G. Accesso ai dati da un file CSV con un file di formato

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="h-accessing-data-from-a-csv-file-without-a-format-file"></a>H. Accesso ai dati da un file CSV senza un file di formato

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\Program Files\Microsoft SQL Server\MSSQL14.CTP1_1\MSSQL\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

```sql
SELECT *
FROM OPENROWSET
   (  'MSDASQL'
     ,'Driver={Microsoft Access Text Driver (*.txt, *.csv)}'
     ,'select * from E:\Tlog\TerritoryData.csv')
;
```

> [!IMPORTANT]
>
> - Il driver ODBC deve essere a 64 bit. Per verificare che questo requisito sia soddisfatto, aprire la scheda **Driver** dell'applicazione [Origini dati ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md) in Windows. È presente un `Microsoft Text Driver (*.txt, *.csv)` a 32 bit che non funziona con una versione a 64 bit di sqlservr.exe.
> - Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure.

### <a name="i-accessing-data-from-a-file-stored-on-azure-blob-storage"></a>I. Accesso ai dati da un file archiviato nell'archiviazione BLOB di Azure

**Si applica a:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
L'esempio seguente usa un'origine dati esterna che punta a un contenitore in un account di archiviazione di Azure e credenziali con ambito database create per una firma di accesso condiviso.

```sql
SELECT * FROM OPENROWSET(
   BULK 'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   SINGLE_CLOB) AS DataFile;
```

Per esempi di `OPENROWSET` completi che includono la configurazione di credenziali e di un'origine dati esterna, vedere [Esempi di accesso bulk ai dati nell'archiviazione BLOB di Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage"></a>J. Importazione in una tabella da un file archiviato in Archiviazione BLOB di Azure

L'esempio seguente illustra come usare il comando OPENROWSET per caricare dati da un file CSV in una posizione di Archiviazione BLOB di Azure in cui è stata creata una chiave di firma di accesso condiviso. La posizione di Archiviazione BLOB di Azure è configurata come origine dati esterna. Questa operazione richiede credenziali con ambito database che usano una firma di accesso condiviso crittografata con una chiave master nel database utente.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/curriculum'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

INSERT INTO achievements with (TABLOCK) (id, description)
SELECT * FROM OPENROWSET(
   BULK  'csv/achievements.csv',
   DATA_SOURCE = 'MyAzureBlobStorage',
   FORMAT ='CSV',
   FORMATFILE='csv/achievements-c.xml',
   FORMATFILE_DATA_SOURCE = 'MyAzureBlobStorage'
    ) AS DataFile;
```

> [!IMPORTANT]
> Il database SQL di Azure supporta solo la lettura da Archiviazione BLOB di Azure tramite un token di firma di accesso condiviso.

### <a name="additional-examples"></a>Esempi aggiuntivi

Per esempi aggiuntivi relativi all'uso di `INSERT...SELECT * FROM OPENROWSET(BULK...)`, vedere gli argomenti seguenti:

- [Esempi di importazione ed esportazione bulk di documenti XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Mantenere i valori Identity durante l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Mantenimento dei valori Null o uso dei valori predefiniti durante un'importazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Vedere anche

- [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
- [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)
- [Informazioni sull'importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)
- [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)
- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)
- [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
- [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)
