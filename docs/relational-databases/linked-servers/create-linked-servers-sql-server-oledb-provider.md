---
description: 'Query distribuite di Microsoft SQL Server: Connettività OLE DB'
title: Creare provider di server collegati
ms.date: 07/01/2019
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: MikeRayMSFT
ms.topic: conceptual
author: pmasl
ms.author: pelopes
manager: rothj
ms.custom: seo-dt-2019
ms.openlocfilehash: 75134031d7567dcc5a653d28ecb1c92a26891d23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080412"
---
# <a name="microsoft-sql-server-distributed-queries-ole-db-connectivity"></a>Query distribuite di Microsoft SQL Server: Connettività OLE DB

Questo articolo descrive il modo in cui il processore di query di Microsoft SQL Server interagisce con un provider OLE DB per abilitare query distribuite ed eterogenee. È destinato principalmente agli sviluppatori di provider OLE DB e presuppone una conoscenza approfondita della specifica OLE DB. L'articolo è incentrato sull'interfaccia OLE DB tra il processore di query di SQL Server e il provider OLE DB e non sulla funzionalità delle query distribuite. Per una descrizione completa della funzionalità delle query distribuite, vedere [Server collegati](../../relational-databases/linked-servers/linked-servers-database-engine.md).

## <a name="overview-and-terminology"></a>Panoramica e terminologia

 In Microsoft SQL Server le query distribuite consentono agli utenti SQL Server di accedere ai dati all'esterno di un server basato su SQL Server, all'interno di altri server che eseguono SQL Server o di altre origini dati che espongono un'interfaccia OLE DB. OLE DB consente di accedere in modo uniforme ai dati tabulari da origini dati eterogenee.

Ai fini di questo articolo, una query distribuita è costituita da un'istruzione `SELECT``INSERT`, `UPDATE` o `DELETE` che fa riferimento a tabelle e set di righe da una o più origini dati OLE DB esterne.

Una tabella remota è una tabella archiviata in un'origine dati OLE DB ed esterna al server che esegue SQL Server in cui è in esecuzione la query. Una query distribuita accede a una o più tabelle remote.

### <a name="ole-db-provider-categories"></a>Categorie di provider OLE DB

I provider OLE DB elencati di seguito sono suddivisi in categorie in base alle funzionalità di esecuzione di query distribuite di SQL Server. Per definizione, non si escludono a vicenda. Un provider può appartenere a più di una delle categorie seguenti:

- Provider di comandi SQL

- Provider di indici

- Provider di tabelle semplici

- Provider di comandi non SQL

#### <a name="sql-command-providers"></a>Provider di comandi SQL

I provider che supportano l'oggetto `Command` con un dialetto SQL standard riconosciuto da SQL Server appartengono a questa categoria. I requisiti specifici di un provider OLE DB per essere considerato da SQL Server come provider di comandi SQL sono i seguenti:

- Il provider deve supportare l'oggetto `Command` e tutte le relative interfacce OLE DB obbligatorie: `ICommand`, `ICommandText`, `IColumnsInfo`, `ICommandProperties` e `IAccessor`.

- Il dialetto SQL supportato dal provider deve essere almeno SQL Subminimum. Il dialetto deve essere indicato dal provider tramite la proprietà `DBPROP_SQLSUPPORT`.

Esempi di provider di comandi SQL sono il provider Microsoft OLE DB per SQL Server e il provider Microsoft OLE DB per ODBC.

#### <a name="index-providers"></a>Provider di indici

I provider di indici sono provider che supportano ed espongono gli indici in base a OLE DB e consentono la ricerca basata sull'indice delle tabelle di base. I requisiti specifici di un provider OLE DB per essere considerato da SQL Server come provider di indici sono i seguenti:

- Il provider deve supportare l'interfaccia `IDBSchemaRowset` con i set di righe dello schema TABLES, COLUMNS e INDEXES.

- Il provider deve supportare l'apertura di un set di righe in un indice tramite `IOpenRowset` specificando il nome dell'indice e il nome della tabella di base corrispondente.

- L'oggetto `Index` deve supportare tutte le relative interfacce obbligatorie: `IRowset`, `IRowsetIndex`, `IAccessor`, `IColumnsInfo`, `IRowsetInfo` e `IConvertTypes`.

- I set di righe aperti nella tabella di base indicizzata (tramite `IOpenRowset`) devono supportare l'interfaccia `IRowsetLocate` per il posizionamento in una riga in base a un segnalibro.

Se il provider OLE DB soddisfa i requisiti indicati, gli utenti possono impostare l'opzione di provider `Index As Access Path` per consentire a SQL Server di usare gli indici del provider per la valutazione delle query. Per impostazione predefinita, se l'opzione non è impostata, SQL Server non tenta di usare gli indici del provider.

>[!NOTE]
>SQL Server supporta diverse opzioni che influenzano il modo in cui SQL Server accede a un provider OLE DB. Per impostare queste opzioni è possibile usare la finestra di dialogo `Linked Server Properties` in SQL Server Enterprise Manager.

#### <a name="simple-table-providers"></a>Provider di tabelle semplici

Questi provider espongono l'apertura di un set di righe in una tabella di base attraverso l'interfaccia `IOpenRowset`. Questi provider non sono provider di comandi SQL né provider di indici, ma rappresentano la classe più semplice di provider con cui è possibile usare le query distribuite di SQL Server.

In questi provider SQL Server può eseguire solo scansioni di tabella durante la valutazione delle query distribuite.

#### <a name="non-sql-command-providers"></a>Provider di comandi non SQL

I provider che supportano l'oggetto `Command` e le relative interfacce obbligatorie, ma non supportano un dialetto SQL standard riconosciuto da SQL Server rientrano in questa categoria.

Il provider Microsoft OLE DB per il servizio di indicizzazione e il [provider Microsoft OLE DB per il servizio Microsoft Active Directory](../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-active-directory-service.md) sono due esempi di provider di comandi non SQL.

### <a name="transact-sql-subset"></a>Subset Transact-SQL

Ognuna delle seguenti classi di istruzioni Transact-SQL è supportata per le query distribuite se il provider supporta le interfacce OLE DB obbligatorie.

- Tutte le istruzioni `SELECT` sono consentite ad eccezione delle istruzioni `SELECT` INTO con una tabella remota come tabella di destinazione.

- Le istruzioni `INSERT` sono consentite nelle tabelle remote se il provider supporta le interfacce obbligatorie per INSERT. Per altre informazioni sui requisiti di OLE DB per INSERT, vedere \"Istruzione INSERT\" più avanti in questo articolo.

- Le istruzioni `UPDATE` e DELETE sono consentite nelle tabelle remote se il provider soddisfa i requisiti di interfaccia OLE DB nella tabella specificata. Per i requisiti di interfaccia OLE DB e le condizioni per l'aggiornamento o l'eliminazione di una tabella remota, vedere \"Istruzioni UPDATE e DELETE\" più avanti in questo articolo.

### <a name="cursor-support"></a>Supporto dei cursori

I cursori snapshot e keyset sono supportati nelle query distribuite se il provider supporta la funzionalità OLE DB necessaria. I cursori dinamici non sono supportati nelle query distribuite. Per le richieste dell'utente di un cursore dinamico in una query distribuita viene effettuato il downgrade a un cursore keyset.

I cursori snapshot vengono popolati al momento dell'apertura del cursore e il set di risultati rimane invariato. Aggiornamenti, inserimenti ed eliminazioni nelle tabelle sottostanti non si riflettono nel cursore.

I cursori keyset vengono popolati al momento dell'apertura del cursore e il set di risultati rimane invariato per tutta la durata del cursore. Tuttavia, gli aggiornamenti e le eliminazioni nelle tabelle sottostanti sono visibili nel cursore mentre vengono visitate le righe. Gli inserimenti nelle tabelle sottostanti che potrebbero influire sull'appartenenza ai cursori non sono visibili.

È possibile aggiornare o eliminare una tabella remota tramite un cursore definito in una query distribuita e che fa riferimento alla tabella remota se il provider soddisfa le condizioni per gli aggiornamenti e le eliminazioni nella tabella remota, ad esempio table `UPDATE`\| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`. Per altre informazioni, vedere \"Istruzioni UPDATE e DELETE\" più avanti in questo articolo.

#### <a name="keyset-cursor-support-requirements"></a>Requisiti per il supporto dei cursori keyset

Un cursore keyset è supportato in una query distribuita se vengono soddisfatti tutti i requisiti della sintassi Transact-SQL e si verifica una delle condizioni seguenti:

- Il provider OLE DB supporta i segnalibri riutilizzabili in tutte le tabelle remote della query. I segnalibri riutilizzabili possono essere usati in un set di righe in una tabella specificata e in un set di righe diverso della stessa tabella. Il supporto dei segnalibri riutilizzabili viene indicato tramite il set di righe dello schema TABLES_INFO di `IDBSchemaRowset` impostando la colonna BOOKMARK_DURABILITY su BMK_DURABILITY_INTRANSACTION o su una durabilità superiore.

- Tutte le tabelle remote espongono una chiave univoca tramite il set di righe INDEXES dell'interfaccia `IDBSchemaRowset`. È necessario che sia presente una voce di indice con la colonna UNIQUE impostata su VARIANT_TRUE.

I cursori keyset non sono supportati nelle query distribuite che includono la funzione *OpenQuery*.

#### <a name="updatable-keyset-cursor-requirements"></a>Requisiti per cursori keyset aggiornabili

È possibile aggiornare o eliminare una tabella remota tramite un cursore keyset definito in una query distribuita, ad esempio `UPDATE`\| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`. I cursori aggiornabili sono utilizzabili nelle query distribuite se vengono soddisfatti i requisiti seguenti:

- I cursori aggiornabili sono consentiti se il provider soddisfa anche le condizioni per gli aggiornamenti e le eliminazioni nella tabella remota. Per altre informazioni, vedere \"Istruzioni UPDATE e DELETE\" più avanti in questo articolo.

- Tutte le operazioni dei cursori keyset aggiornabili devono trovarsi in una transazione definita dall'utente con un livello di isolamento di lettura ripetibile o un livello di isolamento superiore. Il provider deve anche supportare le transazioni distribuite con l'interfaccia `ITransactionJoin`.

## <a name="ole-db-provider-interaction-phases"></a>Fasi di interazione del provider OLE DB

 Sei operazioni sono comuni a tutti gli scenari di esecuzione di query distribuite:

- Le operazioni di connessione e recupero delle proprietà indicano la modalità di connessione di SQL Server a un provider OLE DB e le proprietà del provider usate.

- Le operazioni di risoluzione dei nomi di tabella e recupero dei metadati indicano il modo in cui SQL Server risolve il nome della tabella remota (specificato in uno dei due modi seguenti: un nome basato sul server collegato o un nome ad hoc) nell'oggetto dati appropriato del provider. Sono inclusi anche i metadati della tabella che SQL Server recupera dal provider per compilare e ottimizzare una query distribuita.

- Le operazioni di gestione transazioni specificano tutte le interazioni correlate alla transazione con il provider OLE DB.

- Le operazioni di gestione dei tipi di dati indicano il modo in cui SQL Server gestisce i tipi di dati OLE DB quando utilizza o esporta dati in un provider OLE DB durante l'elaborazione di una query distribuita.

- Le operazioni di gestione degli errori indicano il modo in cui SQL Server usa le informazioni estese sugli errori provenienti dal provider.

- Le operazioni di sicurezza specificano il modo in cui la sicurezza di SQL Server interagisce con la sicurezza del provider.

### <a name="connection-establishment-and-property-retrieval"></a>Connessione e recupero delle proprietà

SQL Server supporta due convenzioni di denominazione degli oggetti dati remoti: nomi in quattro parti basati su server collegati e nomi ad hoc con la funzione `OPENROWSET`.

#### <a name="linked-server-based-names"></a>Nomi basati su server collegati

Un server collegato funge da astrazione per un'origine dati OLE DB. Un nome basato su server collegato è un nome in quattro parti nel formato `<linked-server>.<catalog>`. `<schema>.<object>`, dove `<linked-server>` è il nome del server collegato. SQL Server interpreta `<linked-server>` per derivare il provider OLE DB e gli attributi di connessione che identificano l'origine dati per il provider. Le altre tre parti del nome vengono interpretate dall'origine dati OLE DB per identificare la tabella remota specifica. :::

#### <a name="ad-hoc-names"></a>Nomi ad hoc

Un nome ad hoc è un nome basato sulla funzione `OPENROWSET` o `OPENDATASOURCE`. Il nome include tutte le informazioni di connessione, ovvero il provider OLE DB da usare, gli attributi necessari per identificare l'origine dati, l'ID utente e la password, ogni volta che viene fatto riferimento alla tabella remota in una query distribuita.

L'utilizzo di nomi ad hoc non è consentito per impostazione predefinita, ma è consentito solo per i membri del ruolo sysadmin. Per usare nomi ad hoc in un provider OLE DB, l'opzione del provider `DisallowAdhocAccess` deve essere impostata su `0`.

Se viene usato un nome di server collegato, SQL Server estrae dalla definizione del server collegato il nome del provider OLE DB e le proprietà di inizializzazione per il provider. Se viene usato un nome ad hoc, SQL Server estrae le stesse informazioni dagli argomenti della funzione `OPENROWSET`.

Per istruzioni dettagliate sulla configurazione di un server collegato con una sintassi basata su nome in quattro parti e su nome ad hoc, vedere [Creare server collegati](create-linked-servers-sql-server-database-engine.md).

### <a name="connecting-to-an-ole-db-provider"></a>Connessione a un provider OLE DB

Questi sono i passaggi di alto livello che SQL Server esegue quando si connette a un provider OLE DB:

1. SQL Server crea un oggetto di origine dati.

   SQL Server usa `ProgID` del provider per creare un'istanza dell'oggetto di origine dati (DSO). ProgID è specificato come parametro `provider_name` di una configurazione del server collegato o come primo argomento della funzione `OPENROWSET` nel caso di un nome ad hoc.

   SQL Server crea un'istanza dell'oggetto di origine dati (DSO) del provider attraverso l'interfaccia del componente del servizio OLE DB `IDataInitialize`. Ciò consente allo strumento di gestione dei componenti dei servizi di aggregare i servizi, ad esempio lo scorrimento e il supporto aggiornamenti, al di sopra della funzionalità nativa del provider. Inoltre, la creazione di un'istanza del provider tramite `IDataInitialize` consente al componente del servizio OLE DB di aggiungere a un pool le connessioni al provider riducendo il sovraccarico di connessione e inizializzazione.

   Un provider specificato può essere configurato per la creazione di un'istanza nello stesso processo di SQL Server o in un processo specifico. La creazione di un'istanza in un processo separato protegge il processo di SQL Server dagli errori nel provider. Allo stesso tempo, si verifica un sovraccarico delle prestazioni associato al marshalling delle chiamate OLE DB out-of-process da SQL Server. È possibile configurare un provider per la creazione di un'istanza in-process o out-of-process impostando l'opzione del provider `Allow In Process`. Per altre informazioni, vedere l'[impostazione delle opzioni del provider](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md).

   Per altre informazioni sui componenti del servizio OLE DB e sul pool di sessioni, consultare la documentazione OLE DB per i requisiti del provider.

2. L'origine dati viene inizializzata.

   Dopo la creazione dell'oggetto di origine dati (DSO), l'interfaccia `IDBProperties` imposta la proprietà di inizializzazione DBPROP_INIT_TIMEOUT se l'opzione di configurazione del server `remote login timeout` è impostata su un valore maggiore di 0. Questa proprietà è obbligatoria.

   Le proprietà vengono impostate se sono specificate o implicite nella definizione del server collegato o nel secondo argomento della funzione `OPENROWSET`:

   - `DBPROP_INIT_PROVIDERSTRING`

   - `DBPROP_INIT_DATASOURCE`

   - `DBPROP_INIT_LOCATION`

   - `DBPROP_INIT_CATALOG`

   - `DBPROP_AUTH_USERID`

   - `DBPROP_AUTH_PASSWORD`

   Dopo aver impostato queste proprietà, viene chiamato `IDBInitialize::Initialize` per inizializzare l'oggetto di origine dati (DSO) con le proprietà specificate.

3. SQL Server raccoglie le informazioni specifiche del provider.

   SQL Server raccoglie diverse proprietà del provider che vengono usate nella valutazione delle query distribuite. Queste proprietà vengono recuperate tramite una chiamata a `IDBProperties::GetProperties`. Tutte queste proprietà sono facoltative. Tuttavia, il supporto di tutte le proprietà rilevanti consente a SQL Server di sfruttare al meglio le funzionalità del provider. Ad esempio, `DBPROP_SQLSUPPORT` è necessaria per determinare se SQL Server può inviare query al provider. Se questa proprietà non è supportata, SQL Server non usa il provider remoto come provider di comandi SQL, anche nel caso in cui il provider sia un provider di comandi SQL. Nella tabella seguente la colonna Valore predefinito indica il valore usato da SQL Server se il provider non supporta la proprietà.

Proprietà| Valore predefinito| Uso |
|:----|:----|:----|
|`DBPROP_DBMSNAME`|nessuno|Usata per i messaggi di errore.|
|`DBPROP_DBMSVER` |nessuno|Usata per i messaggi di errore.|
|`DBPROP_PROVIDERNAME`|nessuno|Usata per i messaggi di errore.|
|`DBPROP_PROVIDEROLEDBVER1`|1.5|Usata per determinare la disponibilità delle funzionalità 2.0.
|`DBPROP_CONCATNULLBEHAVIOR`|nessuno|Usata per determinare se il comportamento di concatenazione `NULL` del provider corrisponde a quello di SQL Server.|
|`DBPROP_NULLCOLLATION`|nessuno|Consente l'utilizzo dell'ordinamento/indice solo se `NULLCOLLATION` corrisponde al comportamento delle regole di confronto Null dell'istanza di SQL Server.|
|`DBPROP_OLEOBJECTS`|nessuno|Determina se il provider supporta interfacce di archiviazione strutturate per colonne di oggetti dati di grandi dimensioni.|
|`DBPROP_STRUCTUREDSTORAGE`|nessuno|Determina le interfacce di archiviazione strutturate supportate per i tipi di oggetti di grandi dimensioni (tra `ILockBytes`, `Istream`e `ISequentialStream`).|
|`DBPROP_MULTIPLESTORAGEOBJECTS`|Falso|Determina se è possibile aprire più di una colonna di oggetti di grandi dimensioni nello stesso momento.|
|`DBPROP_SQLSUPPORT`|nessuno|Determina se le query SQL possono essere inviate al provider.|
|`DBPROP_CATALOGLOCATION`|`DBPROPVAL_CL_START`|Usata per costruire nomi di tabella multipart.
|`SQLPROP_DYNAMICSQL`|Falso|Proprietà specifica di SQL Server: se restituisce `VARIANT_TRUE`, indica che i marcatori di parametro `?` sono supportati per l'esecuzione di query con parametri.
|`SQLPROP_NESTEDQUERIES`|Falso|Proprietà specifica di SQL Server: se restituisce `VARIANT_TRUE`, indica che il provider supporta le istruzioni `SELECT` nidificate nella clausola `FROM`.
|`SQLPROP_GROUPBY`|Falso|Proprietà specifica di SQL Server: se restituisce `VARIANT_TRUE`, indica che il provider supporta la clausola GROUP BY nell'istruzione `SELECT` come specificato dallo standard SQL-92.
|`SQLPROP_DATELITERALS `|Falso|Proprietà specifica di SQL Server: se restituisce `VARIANT_TRUE`, indica che il provider supporta i valori letterali datetime in base alla sintassi Transact-SQL di SQL Server.
|`SQLPROP_ANSILIKE `|Falso|Proprietà specifica di SQL Server: questa proprietà è adatta ai provider che supportano il livello SQL-Minimum e supporta l'operatore `LIKE` in base al livello SQL-92 (\'%\' e \'_\' come caratteri jolly).
|`SQLPROP_SUBQUERIES `|Falso|Proprietà di SQL Server: Questa proprietà è adatta ai provider che supportano il livello SQL-Minimum. La proprietà indica che il provider supporta le sottoquery in base al livello SQL-92. Sono incluse le sottoquery nell'elenco `SELECT` e nella clausola `WHERE` con supporto per le sottoquery correlate e gli operatori `IN`, `EXISTS`, `ALL` e `ANY`.
|`SQLPROP_INNERJOIN`|Falso|Proprietà specifica di SQL Server: Questa proprietà è adatta ai provider che supportano il livello SQL-Minimum. Questa proprietà indica il supporto per i join con più tabelle nella clausola `FROM`. ------ ---

I tre valori letterali seguenti vengono recuperati da `IDBInfo::GetLiteralInfo`: `DBLITERAL_CATALOG_SEPARATOR`, `DBLITERAL_SCHEMA_SEPARATOR` (per costruire un nome di oggetto completo con le parti del nome catalogo, schema e oggetto) e `DBLITERAL_QUOTE` (per delimitare i nomi degli identificatori in una query SQL inviata al provider).

Se il provider non supporta i valori letterali separatori, SQL Server usa un punto (.) come carattere separatore predefinito. Se il provider supporta solo il carattere separatore del catalogo e non supporta il carattere separatore dello schema, SQL Server usa il carattere separatore del catalogo anche come carattere separatore dello schema. Se il provider non supporta `DBLITERAL_QUOTE`, SQL Server usa una virgoletta singola (`'`) come carattere di delimitazione.

>[!NOTE]
>Se i valori letterali separatore del nome del provider non corrispondono ai valori predefiniti, il provider deve esporli tramite `IDBInfo` per consentire a SQL Server di accedere alle tabelle tramite nomi in quattro parti. Se i valori letterali non vengono esposti, sarà possibile usare solo le query pass-through nel provider.

Per informazioni sull'esposizione delle proprietà `SQLPROP_DYNAMICSQL` e `SQLPROP_NESTEDQUERIES`, vedere [Proprietà specifiche di SQL Server](#appendixc).

### <a name="table-name-resolution-and-meta-data-retrieval"></a>Risoluzione dei nomi di tabella e recupero dei metadati

SQL Server risolve un nome di tabella remota in una query distribuita in una tabella o vista specifica in un'origine dati OLE DB. Sia gli schemi di denominazione basati su server collegati che quelli ad hoc comportano l'interpretazione di un nome in tre parti da parte del provider. Nel caso del nome basato sul server collegato, le ultime tre parti del nome in quattro parti costituiscono i nomi di catalogo, schema e oggetto. Nel caso del nome ad hoc, il terzo argomento della funzione `OPENROWSET` specifica un nome in tre parti che descrive i nomi di catalogo, schema e oggetto. Uno o entrambi i nomi del catalogo e dello schema possono essere vuoti. Un nome in quattro parti con un nome di catalogo e un nome di schema vuoti sarà simile a `<server-name>...<object-name>`. In tal caso, SQL Server usa `NULL` come valore corrispondente da ricercare nelle tabelle dei set di righe dello schema.

Le regole di risoluzione dei nomi e i passaggi di recupero dei metadati usati da SQL Server variano a seconda che il provider supporti l'interfaccia `IDBSchemaRowset` nell'oggetto `Session`.

Se `IDBSchemaRowset` è supportata, vengono usati i set di righe dello schema `TABLES`, `COLUMNS`, `INDEXES` e `TABLES_INFO` dall'interfaccia `IDBSchemaRowset`. Il set di righe dello schema `TABLES_INFO` è definito in OLE DB 2.0. SQL Server limita i set di righe dello schema restituiti dall'interfaccia `IDBSchemaRowset` per cercare le righe dello schema che corrispondono alle parti del nome della tabella remota specificata. Di seguito sono riportate le regole relative alle restrizioni supportate dal provider nei set di righe dello schema ed è illustrato come SQL Server usa le regole per recuperare i metadati di una tabella remota:

- Le restrizioni per le colonne `TABLE_NAME` e `COLUMN_NAME` sono sempre obbligatorie.

- Se il provider supporta una restrizione in `TABLE_CATALOG` (o `TABLE_SCHEMA`), SQL Server usa la restrizione in `TABLE_CATALOG` (o `TABLE_SCHEMA`). Se il nome del catalogo o dello schema non è specificato nel nome della tabella remota, viene usato un valore `NULL` come valore di restrizione corrispondente. Se viene specificato un nome di catalogo o schema, il provider deve supportare la restrizione corrispondente in `TABLE_CATALOG` o `TABLE_SCHEMA`.

- Il provider deve supportare la restrizione nella colonna `TABLE_SCHEMA` in `TABLES` e `COLUMNS` oppure in nessuno dei due. Il provider deve supportare la restrizione del nome di catalogo nei set di righe `TABLES` e `COLUMNS` oppure in nessuno dei due.

- Se sono supportate restrizioni in INDEXES, il provider deve supportare la restrizione dello schema nei set di righe `TABLES` e INDE`XES or support them on neither. The provider must either support catalog name restriction on both `TABLES` and `INDEXES` o in nessuno dei due.

Dal set di righe dello schema `TABLES`, SQL Server recupera le colonne `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `TABLE_TYPE`, `TABLE_GUID` impostando le restrizioni in base alle regole indicate sopra.

Dal set di righe dello schema `COLUMNS`, SQL Server recupera le colonne `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `COLUMN_NAME`, `COLUMN_GUID`, `ORDINAL_POSITION`, `COLUMN_FLAGS`, `IS_NULLABLE`, `DATA_TYPE`, `TYPE_GUID`, `CHARACTER_MAXIMUM_LENGTH`, `NUMERIC_PRECISION` e `NUMERIC_SCALE`. `COLUMN_NAME`, `DATA_TYPE` e `ORDINAL_POSITION` devono restituire valori non Null validi. Se `DATA_TYPE` è `DBTYPE_NUMERIC` o `DBTYPE_DECIMAL`, `NUMERIC_PRECISION` e `NUMERIC_SCALE` corrispondenti devono essere valori non Null validi.

Dal set di righe dello schema `INDEXES` facoltativo, SQL Server ricerca gli indici nella tabella remota specificata impostando le restrizioni in base alle regole precedenti. Dalle voci di indice trovate, SQL Server recupera le colonne `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `INDEX_CATALOG`, `INDEX_SCHEMA`, `INDEX_NAME`, `PRIMARY_KEY`, `UNIQUE`, `CLUSTERED`, `FILL_FACTOR`, `ORDINAL_POSITION`, `COLUMN_NAME`, `COLLATION`, `CARDINALITY` e `PAGES`.

Dal set di righe `TABLES_INFO` facoltativo, SQL Server ricerca informazioni aggiuntive nella tabella remota specificata, ad esempio il supporto dei segnalibri e il tipo e la lunghezza del segnalibro. Vengono usate tutte le colonne, ad eccezione della colonna `DESCRIPTION` del set di righe `TABLES_INFO`. Le informazioni nel set di righe `TABLES_INFO` vengono usate come segue:

- La colonna `BOOKMARK_DURABILITY` viene usata per implementare cursori keyset più efficienti. Se la colonna ha un valore `BMK_DURABILITY_INTRANSACTION` o un valore di durabilità maggiore, SQL Server usa il recupero basato su segnalibro e gli aggiornamenti delle righe della tabella remota per l'implementazione di un cursore keyset.

- Le colonne `BOOKMARK_TYPE`, `BOOKMARK_DATA TYPE` e `BOOKMARK_MAXIMUM_LENGTH` vengono usate per determinare i metadati dei segnalibri in fase di compilazione della query. Se queste colonne non sono supportate, SQL Server apre il set di righe della tabella di base tramite `IOpenRowset` durante la compilazione per ottenere le informazioni dei segnalibri.

Se `IDBSchemaRowset` non è supportato e il nome della tabella remota include un nome di catalogo o schema, SQL Server richiede `IDBSchemaRowset` e restituisce un errore. Tuttavia, se non viene specificato né il nome di catalogo né il nome dello schema, SQL Server apre il set di righe che corrisponde alla tabella remota e recupera i metadati della colonna dall'interfaccia `IColumnsInfo` obbligatoria dell'oggetto set di righe.

SQL Server apre il set di righe corrispondente alla tabella tramite una chiamata a `IOpenRowset::OpenRowset`. Il nome di tabella specificato in `OPENROWSET` viene costruito in base alle parti del nome catalogo, schema e oggetto.

- Ognuna delle parti del nome (`catalog`, `schema`, `object name`) è delimitata con il carattere di delimitazione del provider (`DBLITERAL_QUOTE`) e quindi concatenata con il carattere `DBLITERAL_CATALOG_SEPARATOR` e il carattere `DBLITERAL_SCHEMA_SEPARATOR` incorporati. La costruzione del nome segue le regole OLE DB in `IOpenRowset`.

- I metadati della colonna per la tabella vengono recuperati tramite `IColumnsInfo::GetColumnInfo` dopo l'apertura dell'oggetto set di righe.

Se `IDBSchemaRowset` non è supportato con i set di righe TABLES, COLUMNS, and TABLES_INFO, SQL Server apre due volte il set di righe nella tabella di base: una volta durante la compilazione della query per recuperare i metadati e una volta durante l'esecuzione della query. Questo comportamento deve essere considerato dai provider che riscontrano problemi causati dall'apertura del set di righe, ad esempio l'esecuzione di codice che modifica lo stato di un dispositivo in tempo reale, l'invio di messaggi di posta elettronica, l'esecuzione di codice arbitrario specificato dall'utente.

### <a name="statistics-retrieval"></a>Recupero delle statistiche

Se il provider supporta le statistiche di distribuzione nelle tabelle di base, SQL Server userà queste statistiche. Sono disponibili due tipi di statistiche utili per l'elaborazione di query di SQL Server:

- **Cardinalità di colonne (o tuple)**. Si tratta del numero di valori univoci presenti in una colonna o in una combinazione di colonne di una tabella. Il numero può essere usato per stimare la selettività dei predicati nelle colonne. Un provider che supporta le statistiche di distribuzione deve supportare almeno un tipo di cardinalità.

- **Istogrammi**. Se la distribuzione dei valori non è uniforme, il numero di valori univoci non è sufficiente per stimare con precisione la selettività dei predicati. In questo caso è possibile fornire un istogramma che offre informazioni specifiche sulla distribuzione dei valori di colonna in una tabella.

La disponibilità delle statistiche consente al componente Query Optimizer di SQL Server di stimare meglio le cardinalità delle operazioni intermedie in una query in modo da generare piani di esecuzione migliori.

Il provider di OLE DB deve supportare le statistiche di distribuzione come segue:

- **Obbligatorio**. Supportare le proprietà (1) `DBPROP_TABLESTATISTICS` che indica se sono supportate le cardinalità di colonne o tuple e se sono supportati gli istogrammi e (2) `DBPROP_OPENROWSETSUPPORT` che indica con il bit `DBPROPVAL_ORS_HISTOGRAM` se sono supportati gli istogrammi.

- **Obbligatorio**. Set di righe dello schema `TABLE_STATISTICS`. Il set di righe dello schema `TABLE_STATISTICS` elenca le statistiche disponibili in un determinato database. Include anche le cardinalità di colonne e tuple nel set di righe dello schema e indica se gli istogrammi sono supportati nelle colonne specifiche. Per consentire l'uso delle statistiche da parte di SQL Server, le colonne `TABLE_NAME`, `STATISTICS_NAME`, `STATISTICS_TYPE`, `COLUMN_NAME` e `ORDINAL_POSITION` sono obbligatorie nel set di righe dello schema. Almeno una tra `COLUMN_CARDINALITY` o `TUPLE_CARDINALITY` è obbligatoria. Se gli istogrammi sono supportati, è obbligatorio anche `NO_OF_RANGES`.

- **Facoltativo**. Facoltativamente, se il provider supporta gli istogrammi, deve supportare un miglioramento del metodo `IOpenRowset::OpenRowset` che consente di aprire un set di righe dell'istogramma specificando `DBID` della statistica corrispondente.

Per informazioni complete sulle interfacce delle statistiche, vedere la specifica OLE DB 2.6.

### <a name="constraints"></a>Vincoli

Il componente Query Optimizer di SQL Server usa anche i vincoli `CHECK` definiti nelle tabelle di base in un'origine dati remota, se il provider OLE DB supporta il set di righe dello schema OLE DB 2.6 `CHECK_CONSTRAINTS_BY_TABLE`. La colonna `CHECK_CLAUSE` del set di righe dello schema deve restituire il predicato della clausola `CHECK` nella sintassi conforme a SQL-92. Il componente Query Optimizer usa le informazioni dei vincoli per eliminare o semplificare i predicati sempre false o sempre true a causa della presenza di un vincolo CHECK nella tabella.

### <a name="transaction-management"></a>Gestione transazioni

SQL Server supporta l'accesso basato su transazioni ai dati distribuiti tramite le interfacce OLE DB `ITransactionLocal` (per transazioni locali) e `ITransactionJoin` (per le transazioni distribuite) del provider. Avviando una transazione locale nel provider, SQL Server garantisce operazioni di scrittura atomiche. Usando transazioni distribuite, SQL Server garantisce che una transazione che coinvolge più nodi abbia lo stesso risultato (commit o interruzione) in tutti i nodi. Se il provider non supporta le interfacce correlate alle transazioni OLE DB obbligatorie, le operazioni di aggiornamento nel provider non sono consentite in base al contesto della transazione locale.

Nella tabella seguente viene descritto cosa accade quando l'utente esegue una query distribuita con le capacità del provider e il contesto di transazione locale specificati. Un'operazione di lettura in un provider fa riferimento a un'istruzione `SELECT` o al momento in cui la tabella remota viene letta nel lato input di un'istruzione `SELECT INTO`, `INSERT`, `UPDATE` o `DELETE`. Un'operazione di scrittura in un provider fa riferimento a un'istruzione `INSERT`, `UPDATE` o `DELETE` con una tabella remota come tabella di destinazione.

Risultati di una query distribuita in base alle capacità del provider e al contesto di transazione:

|La query distribuita viene eseguita in|Il provider non supporta `ITransactionLocal`|Il provider supporta `ITransactionLocal` ma non `ITransactionJoin`|Il provider supporta `ITransactionLocal` e `ITransactionJoin`|
|:-----|:-----|:-----|:-----|
| In una transazione (nessuna transazione utente).|Per impostazione predefinita, sono consentite solo le operazioni di lettura. Quando l'opzione di livello del provider `Nontransacted Updates` è abilitata, sono consentite le operazioni di scrittura. Quando questa opzione è abilitata, SQL Server non è in grado di garantire atomicità e coerenza dei dati del provider. Per questa ragione è possibile che gli effetti di un'operazione di scrittura vengano riportati parzialmente nell'origine dati remota senza la possibilità di annullarli.| Nei dati remoti sono consentite tutte le istruzioni. I cursori keyset sono di sola lettura. La transazione locale viene avviata nel provider con il livello di isolamento della sessione di SQL Server corrente e il commit viene eseguito al completamento della valutazione dell'istruzione. Il livello di isolamento predefinito per una sessione di SQL Server è `READ COMMITTED`, a meno che non venga modificato con l'istruzione `SET TRANSACTION ISOLATION LEVEL`. Il provider deve supportare il livello di isolamento richiesto. | Sono consentite tutte le istruzioni. I cursori keyset sono di sola lettura. La transazione locale viene avviata nel provider con il livello di isolamento della sessione di SQL Server corrente e il commit viene eseguito al completamento della valutazione dell'istruzione. |
| In una transazione utente, ovvero tra `BEGIN TRAN` o `BEGIN DISTRIBUTED TRAN` e `COMMIT`. | Se il livello di isolamento della transazione è `READ COMMITTED` (impostazione predefinita) o inferiore, sono consentite le operazioni di lettura. Se il livello di isolamento è superiore, non è consentita alcuna query distribuita. | Sono consentite solo le operazioni di lettura. Le nuove transazioni distribuite vengono avviate nel provider con il livello di isolamento della sessione di SQL Server corrente. |Sono consentite tutte le istruzioni. La nuova transazione distribuita viene avviata nel provider con il livello di isolamento della sessione di SQL Server corrente e il commit viene eseguito al commit della transazione utente. Per le istruzioni di modifica dei dati, per impostazione predefinita SQL Server avvia una transazione nidificata nella transazione distribuita in modo che l'istruzione di modifica dei dati possa essere arrestata in determinate condizioni di errore senza arrestare la transazione circostante. Se l'opzione `XACT_ABORT SET` è attiva, SQL Server non richiede il supporto delle transazioni nidificate e interrompe la transazione circostante in caso di errori durante l'istruzione di modifica dei dati. |
|  |  |  |

### <a name="data-type-handling-in-distributed-queries"></a>Gestione dei tipi di dati nelle query distribuite

I provider OLE DB espongono i dati in termini di tipi di dati definiti da OLE DB (indicati da `DBTYPE` in OLE DB). SQL Server elabora i dati esterni nel server come tipi SQL Server nativi. In questo modo si ottiene un mapping dei tipi di dati OLE DB nei tipi nativi SQL Server e viceversa quando i dati vengono usati rispettivamente da SQL Server o esportati da SQL Server. Il mapping viene eseguito in modo implicito, a meno che non sia specificato diversamente.

I tipi di dati nelle query distribuite vengono gestiti usando uno dei due metodi di mapping seguenti:

- Il mapping lato consumo mappa i tipi da tipi di dati OLE DB a tipi di dati nativi SQL Server sul lato consumo, quando le tabelle remote sono incluse nelle istruzioni `SELECT` e sul lato input delle istruzioni INSERT, UPDATE e DELETE.

- Il mapping lato esportazione esegue il mapping dei tipi dai tipi di dati SQL Server ai tipi di dati OLE DB sul lato esportazione, quando una tabella remota è inclusa come tabella di destinazione di un'istruzione `INSERT` o `UPDATE`.

Tabella di mapping dei tipi di dati SQL Server e OLE DB.

| Tipo OLE DB | `DBCOLUMNFLAG` | Tipo di dati di SQL Server |
|-----|-----|-----|
|`DBTYPE_I1`*| |`numeric(3, 0)`|
|`DBTYPE_I2`| |`smallint`|
|`DBTYPE_I4`| |`int`|
|`DBTYPE_I8`| |`numeric(19,0)`|
|`DBTYPE_UI1`| |`tinyint`|
|`DBTYPE_UI2`*| |`numeric(5,0)`|
|`DBTYPE_UI4`*| |`numeric(10,0)`|
|`DBTYPE_UI8`*| |`numeric(20,0)`|
|`DBTYPE_R4`| |`float`|
|`DBTYPE_R8`| |`real`|
|`DBTYPE_NUMERIC`| |`numeric`|
|`DBTYPE_DECIMAL`| |`decimal`|
|`DBTYPE_CY`| |`money`|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`<br>o<br> Lunghezza massima > 4000 caratteri|ntext|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`|NCHAR|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|NVARCHAR|
|`DBTYPE_IDISPATCH`| |Errore|
|`DBTYPE_ERROR`| |Errore|
|`DBTYPE_BOOL`| |`bit`|
|`DBTYPE_VARIANT`*| |NVARCHAR|
|`DBTYPE_IUNKNOWN`| |Errore|
|`DBTYPE_GUID`| |`uniqueidentifier`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISLONG=true` <br>o<br> Lunghezza massima > 8000|`image`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISROWVER=true`, `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`, Dimensione colonna = 8 <br>o<br> Lunghezza massima non indicata. | `timestamp` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `binary` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varbinary`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `char`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varchar` |
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISLONG=true` <br>o<br> Lunghezza massima > 8000 caratteri <br>o<br>   Lunghezza massima non indicata. | `text`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` |`nchar`|
|`DBTYPE_WSTR` | `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|`nvarchar`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISLONG=true` <br>o<br> Lunghezza massima > 4000 caratteri <br>o<br>   Lunghezza massima non indicata. | `ntext`|
|`DBTYPE_UDT`| |Errore|
|`DBTYPE_DATE`* | | `datetime` |
|`DBTYPE_DBDATE` | | `datetime` (conversione esplicita obbligatoria)|
|`DBTYPE_DBTIME`| | `datetime` (conversione esplicita obbligatoria)|
|`DBTYPE_DBTIMESTAMP`* | | `datetime`|
|`DBTYPE_ARRAY` | |Errore|
|`DBTYPE_BYREF` | | Ignorato |
|`DBTYPE_VECTOR` | |Errore|
|`DBTYPE_RESERVED`| |Errore|

\* Indicare una forma di conversione nella rappresentazione del tipo SQL Server, poiché non esiste un tipo di dati equivalente in SQL Server. Le conversioni potrebbero comportare una perdita di precisione, overflow o underflow. I mapping impliciti predefiniti possono essere modificati in futuro se i tipi di dati corrispondenti sono supportati dalle versioni future di SQL Server.

>[!NOTE]
>`numeric(p,s)` indica il tipo di dati SQL Server `numeric` con precisione `p` e scalabilità `s`. La precisione massima consentita per `DBTYPE_NUMERIC` e `DBTYPE_DECIMAL` è 38. Il provider deve supportare l'associazione alla colonna `DBTYPE_BSTR` come `DBTYPE_WSTR` durante la creazione di una funzione di accesso. Le colonne `DBTYPE_VARIANT` vengono usate come stringhe di caratteri Unicode `nvarchar`. È necessario il supporto per la conversione da `DBTYPE_VARIANT` a `DBTYPE_WSTR` dal provider. Si prevede che il provider implementi questa conversione come definito in OLE DB. Per altre informazioni, vedere i [tipi di dati della specifica OLE DB](#appendixa).

#### <a name="interpreting-data-type-mapping"></a>Interpretazione del mapping dei tipi di dati

Il mapping a un tipo SQL Server è determinato dal tipo di dati OLE DB e dai valori DBCOLUMNFLAGS che descrivono la colonna o il valore scalare. Nel caso del set di righe dello schema `COLUMNS` le colonne `DATA_TYPE` e `COLUMN_FLAGS` rappresentano questi valori. Nel caso dell'interfaccia `IColumnsInfo::GetColumnInfo` i membri `wType` e `dwFlags` della struttura `DBCOLUMNINFO` rappresentano queste informazioni.

Per utilizzare il mapping lato consumo per una determinata colonna con un valore `DBTYPE` e `DBCOLUMNFLAG` specifico, cercare il tipo SQL Server corrispondente nella tabella. Le regole del tipo per le colonne delle tabelle remote nelle espressioni possono essere descritte dalla semplice regola seguente:

>Un valore di colonna remota è valido in un'espressione Transact-SQL se il tipo SQL Server mappato corrispondente nella tabella è valido nello stesso contesto.

La tabella e la regola definiscono:

- Confronti ed espressioni.

In generale, `X <op> <remote-column>` è un'espressione valida se `<op>` è un operatore valido per il tipo di dati `X` e il tipo di dati cui viene mappato `<remote-column>`.

- Conversioni esplicite.

`Convert(X, <remote-column>)` è consentito se `DBTYPE` di `<remote-column>` viene mappato al tipo di dati nativo `Y` (in base alla tabella precedente) ed è consentita la conversione esplicita da `Y` a `X`.

Per convertire i dati remoti in un tipo di dati nativo non predefinito, è necessario che gli utenti usino una conversione esplicita.

Per usare il mapping lato esportazione nel caso di istruzioni `UPDATE` e `INSERT` in tabelle remote, mappare i tipi di dati SQL Server nativi ai tipi di dati OLE DB usando la stessa tabella. Il mapping di un tipo SQL Server `S1` a un tipo OLE DB `T` specifico è consentito solo se si verifica una delle condizioni seguenti:

- Il mapping corrispondente è disponibile direttamente nella tabella di mapping.

- È presente una conversione implicita consentita di `S1` in un altro tipo SQL Server `S2` e `S2` viene mappato al tipo `T` nella tabella di mapping.

#### <a name="large-object-lob-handling"></a>Gestione di Large Object (LOB)

Come indicato nella tabella di mapping, se le colonne di tipo `DBTYPE_STR`, `DBTYPE_WSTR` o `DBTYPE_BSTR` indicano anche `DBCOLUMNFLAGS_ISLONG` o se la lunghezza massima delle colonne è superiore a 4.000 caratteri (o non è indicata alcuna lunghezza massima), SQL Server le considera come colonne `text` o `ntext`, come appropriato. Analogamente, per le colonne `DBTYPE_BYTES`, se è impostato `DBCOLUMNFLAGS_ISLONG` o se la lunghezza massima è superiore a 8.000 byte (o non è indicata la lunghezza massima), le colonne vengono considerate come colonne `image`. Le colonne `Text`, `ntext` e `image` sono chiamate colonne LOB.

SQL Server non espone la funzione di testo completo e immagine negli oggetti LOB di un provider OLE DB. `TEXTPTRS` non sono supportati negli oggetti LOB di un provider OLE DB. Di conseguenza, non è supportata alcuna funzione correlata, ad esempio la funzione di sistema `TEXTPTR` e le istruzioni `READTEXT`, `WRITETEXT` e `UPDATETEXT`. Le istruzioni `SELECT` che recuperano intere colonne LOB sono supportate, così come le istruzioni `UPDATE` e `INSERT` per intere colonne LOB in tabelle remote.

SQL Server usa le interfacce di archiviazione strutturate nelle colonne LOB se supportate dal provider. Le interfacce di archiviazione strutturate in ordine crescente di preferenza e funzionalità sono le seguenti: `ISequentialStream`, `Istream` o `ILockBytes`. Se una o più di queste interfacce sono supportate, il provider deve restituire DBPROPVAL_OO_BLOB come valore della proprietà `DBPROP_OLEOBJECTS` quando viene eseguita una query attraverso l'interfaccia `IDBProperties`. Inoltre, il provider deve indicare il supporto per le interfacce supportate nella proprietà `DBPROP_STRUCTUREDSTORAGE`.

Se il provider non supporta alcuna interfaccia di archiviazione strutturata nelle colonne LOB, SQL Server materializza l'interfaccia e continua ad esporle come colonne `text`, `ntext` o `image`.

#### <a name="accessing-lob-columns"></a>Accesso alle colonne LOB

Se il provider supporta una delle interfacce di archiviazione strutturate, SQL Server esegue i passaggi seguenti per recuperare le colonne LOB durante l'esecuzione della query:

1. Prima di aprire il set di righe tramite `IOpenRowset::OpenRowset`, SQL Server richiede il supporto per una o più interfacce di archiviazione strutturate (`ISequentialStream`, `Istream` e `ILockBytes`) nelle colonne LOB. La prima interfaccia supportata dal provider è obbligatoria. Le interfacce aggiuntive sono richieste con un'impostazione \"economica\" impostando l'elemento *dwOptions* della struttura DBPROP corrispondente su DBPROPOPTIONS_SETIFCHEAP. Ad esempio, se un provider supporta `ISequentialStream` e `ILockBytes`, `ISequentialStream` è obbligatorio, mentre `ILockBytes` è richiesto con un'impostazione \"economica\".

4. Dopo aver aperto il set di righe, SQL Server usa `IRowsetInfo::GetProperties` per identificare le interfacce effettive disponibili nel set di righe. Viene usata l'ultima interfaccia o la più adatta restituita dal provider. Quando SQL Server crea una funzione di accesso nella colonna LOB, la colonna viene associata come DBTYPE_IUNKNOWN con l'elemento *iid* della struttura DBOBJECT nell'associazione impostata sull'interfaccia.

#### <a name="reading-from-lob-columns"></a>Lettura da colonne LOB

Usare il puntatore di interfaccia per l'interfaccia di archiviazione strutturata richiesta restituita nel buffer di riga da `IRowset::GetData` per leggere dalla colonna LOB. Se il provider non supporta più LOB aperti contemporaneamente, ovvero se non supporta `DBPROP_MULTIPLE_STORAGEOBJECTS`, e se la riga contiene più colonne LOB, SQL Server copia le colonne LOB in una tabella di lavoro locale.

#### <a name="update-and-insert-statements-on-lob-columns"></a>Istruzioni `UPDATE` e `INSERT` nelle colonne LOB

SQL Server passa al provider un puntatore a un nuovo oggetto di archiviazione anziché usare l'interfaccia fornita dal provider per modificare l'oggetto di archiviazione. Per ogni colonna LOB, il valore aggiornato o inserito in un oggetto di archiviazione viene creato con l'interfaccia di archiviazione strutturata selezionata. A seconda che si tratti di un'operazione `UPDATE` o `INSERT`, un puntatore all'oggetto di archiviazione viene passato al provider rispettivamente tramite `IRowsetChange::SetData` o `IRowsetChange::InsertRow`.

### <a name="error-handling"></a>Gestione degli errori

Quando una chiamata a un metodo specifico in un provider OLE DB restituisce un codice di errore, SQL Server cerca le informazioni di errore estese del provider prima di restituire all'utente le informazioni sulla condizione di errore.

SQL Server usa l'oggetto errore OLE DB come specificato da OLE DB. Di seguito sono riportati alcuni passaggi di alto livello:

1. Quando una chiamata al metodo restituisce un codice di errore dal provider, SQL Server cerca l'interfaccia `ISupportErrorInfo`. Se l'interfaccia è supportata, SQL Server chiama `ISupportErrorInfo::InterfaceSupportsErrorInfo` per verificare se gli oggetti errore sono supportati dall'interfaccia che ha generato il codice di errore.

<!-- -->

5. Se gli oggetti errore sono supportati dall'interfaccia, SQL Server chiama la funzione `GetErrorInfo` per ottenere un puntatore all'interfaccia `IErrorInfo` nell'oggetto errore corrente.

6. SQL Server usa l'interfaccia `IErrorInfo` per ottenere un puntatore all'interfaccia `IErrorRecords`.

7. SQL Server usa `IErrorRecords` per riprodurre a ciclo continuo tutti i record degli errori nell'oggetto e ottenere il testo del messaggio di errore corrispondente a ogni record.

Per altre informazioni sull'utilizzo dell'oggetto errore del provider, vedere la documentazione OLE DB.

### <a name="security"></a>Sicurezza

Quando un utente si connette a un provider OLE DB, il provider richiede in genere un ID utente e una password, a meno che l'utente non voglia essere autenticato come utente di sicurezza integrato. Nel caso di query distribuite, SQL Server agisce come utente del provider OLE DB per conto dell'account di accesso SQL Server che esegue la query distribuita. SQL Server esegue il mapping dell'account di accesso SQL Server corrente a un ID utente e una password nel server collegato.

Questi mapping possono essere specificati dall'utente per un determinato server collegato e possono essere configurati e gestiti dalle stored procedure di sistema `sp_addlinkedsrvlogin` e `sp_droplinkedsrvlogin`. Impostando le proprietà del gruppo di inizializzazione DBPROP_AUTH_USERID e DBPROP_AUTH_PASSWORD tramite `IDBProperties::SetProperties`, l'ID utente e la password determinati dal mapping vengono passati al provider durante la connessione.

Quando un client si connette a SQL Server tramite l'autenticazione di Windows e se l'account di accesso ha un mapping `self` configurato tramite `sp_addlinkedsrvlogin`, SQL Server tenta di rappresentare il contesto di sicurezza del client e imposta la proprietà `DBPROP_AUTH_INTEGRATED` nel provider durante la connessione. Questo processo viene chiamato *delega*.

Dopo aver determinato il contesto di sicurezza usato per la connessione, l'autenticazione del contesto di sicurezza e il controllo delle autorizzazioni per il contesto negli oggetti dati dell'origine dati sono interamente responsabilità del provider OLE DB.

Per altre informazioni, vedere [`sp_addlinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) e [`sp_droplinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md).

## <a name="query-execution-scenarios"></a>Scenari di esecuzione delle query

 Durante la valutazione di una query distribuita, SQL Server interagisce con il provider OLE DB in uno o più di questi scenari:

- Query remota

- Accesso indicizzato

- Scansioni di tabelle pure

- Istruzioni `UPDATE` e DELETE

- `INSERT` - istruzione

- Query pass-through

### <a name="remote-query"></a>Remote Query

SQL Server genera una query SQL che valuta una parte della query originale che può essere valutata integralmente dal provider. Questo scenario è possibile solo nei provider di comandi SQL. La misura in cui SQL Server effettua il push delle operazioni nel provider generando una query SQL dipende dalla grammatica SQL supportata dal provider. Il provider deve indicare il livello di supporto SQL tramite gli elementi seguenti:

1. Indicando il supporto SQL Minimum, ODBC Core o SQL-92 tramite la proprietà `DBPROP_SQLSUPPORT`. Il livello di sintassi SQL Minimum è un nuovo livello supportato in SQL Server che consente a SQL Server di inviare query remote a provider semplici che supportano un subset di SQL semplice. Questo livello comprende un'istruzione di base `SELECT` che non include sottoquery, più tabelle nella clausola `FROM` (e di conseguenza nessun join) e GROUP BY. Per il subset della grammatica SQL usato da SQL Server per la generazione di query remote nei provider di ognuno di questi livelli di sintassi, vedere [Subset SQL usati per la generazione di query remote](#appendixb).

1. Supportando varie proprietà specifiche di SQL Server per indicare il supporto di singole funzionalità SQL che altrimenti non sono incluse nel livello di sintassi indicato da DBPROP_SQLSUPPORT. L'elenco delle proprietà e il modo in cui vengono usate da SQL Server sono descritti più avanti in questa sezione.

SQL Server usa l'esecuzione di query con parametri con un punto interrogativo (?) come marcatore di parametro nella stringa Transact-SQL. L'esecuzione di query con parametri viene usata nei provider SQL Server, Microsoft Jet e Oracle OLE DB. Negli altri provider viene usata l'esecuzione di query con parametri se il provider supporta `ICommandWithParameters` nell'oggetto `Command` e viene soddisfatta almeno una delle condizioni seguenti:

- Il provider indica il livello di ODBC Core del supporto SQL Server tramite la proprietà `DBPROP_SQLSUPPORT`.

- Il provider indica il supporto per il marcatore di parametro punto interrogativo (?) supportando la proprietà specifica di SQL Server SQLPROP_DYNCMICSQL tramite `IDBPProperties`. Per altre informazioni, vedere la sezione successiva sulle proprietà del provider.

- L'amministratore imposta l'opzione del provider `Dynamic Parameters` nel provider per fare in modo che SQL Server generi query con parametri.

Quando SQL Server genera il testo SQL da eseguire in remoto, i nomi di tabelle e colonne sono delimitati con il carattere di delimitazione del provider come indicato tramite il valore letterale `DBLITERAL_QUOTE` dell'interfaccia `IDBInfo`. Se il valore letterale non è supportato, i nomi di tabelle e colonne non sono delimitati.

Se il provider supporta l'esecuzione di query con parametri, SQL Server usa una strategia di esecuzione di query con parametri per valutare un join di una tabella remota con una tabella locale. La query con parametri viene eseguita ripetutamente per i valori dei parametri generati da ogni riga della tabella locale. Questa strategia consente di ridurre il numero di righe recuperate dal provider ed è utile quando viene creato un join tra una tabella locale con un numero di righe ridotto e una tabella remota con un numero di righe elevato. Questa strategia di join remoto può essere applicata dall'hint di ottimizzazione join `REMOTE`. Per altre informazioni sull'esecuzione di query con parametri, vedere [Procedura: Eseguire query con parametri](../../connect/php/how-to-perform-parameterized-queries.md).

Di seguito sono riportati i passaggi di livello superiore eseguiti nel provider nello scenario di query remota.

1. SQL Server crea un oggetto `Command` dall'oggetto `Session` usando `IDBCreateCommand::CreateCommand`.

9. Se l'opzione di configurazione del server `Remote Query Timeout` è impostata su un valore > 0, SQL Server imposta la proprietà `DBPROP_COMMANDTIMEOUT` per l'oggetto `Command` sullo stesso valore tramite `ICommandProperties::SetProperties`. È necessario chiamare `ICommand::SetCommandText` per impostare il testo del comando sulla stringa Transact-SQL generata.

10. SQL Server chiama `ICommandPrepare::Prepare` per preparare il comando. Se il provider non supporta questa interfaccia, SQL Server continua con il passaggio 4.

11. Se la query generata è con parametri, SQL Server usa `ICommandWithParameters::SetParameterInfo` per descrivere i parametri e `IAccessor::CreateAccessor` per creare funzioni di accesso per i parametri.

12. SQL Server chiama `ICommand::Execute` per eseguire il comando e creare il set di righe.

13. SQL Server usa l'interfaccia `IRowset` per esplorare e usare righe della tabella. Usare `IRowset::GetNextRows` per recuperare le righe, `IRowset::RestartPosition` per tornare all'inizio del set di righe e `IRowset::ReleaseRows` per rilasciare le righe.

#### <a name="provider-properties-of-interest-for-remote-query-execution"></a>Proprietà del provider per l'esecuzione di query remote

Se il provider supporta le funzionalità SQL non coperte dal livello di sintassi indicato in DBPROP_SQLSUPPORT, il provider può indicare le funzionalità usando diverse proprietà specifiche del provider.

- SQLPROP_GROUPBY. Questa proprietà è adatta ai provider che supportano il livello SQL-Minimum. La proprietà indica che il provider supporta le clausole GROUP BY e HAVING nell'istruzione `SELECT`. La proprietà indica anche che il provider supporta le cinque funzioni di aggregazione MIN, MAX, SUM, COUNT e AVG. È possibile che il provider non supporti DISTINCT nell'argomento di queste funzioni di aggregazione.

- SQLPROP_SUBQUERIES. Questa proprietà è adatta ai provider che supportano il livello SQL-Minimum. La proprietà indica che il provider supporta le sottoquery in base al livello SQL-92. Sono incluse le sottoquery nell'elenco `SELECT` e nella clausola `WHERE` con supporto per le sottoquery correlate e gli operatori `IN`, `EXISTS`, `ALL` e `ANY`.

- SQLPROP_DATELITERALS. Questa proprietà è adatta a qualsiasi provider, inclusi quelli che supportano il livello SQL-92. Il supporto per la sintassi dei valori letterali standard per i valori letterali datetime non è incluso nel livello SQL-92. Questa proprietà specifica di SQL Server indica che il provider supporta la sintassi dei valori letterali datetime come specificato dallo standard SQL-92.

- SQLPROP_ANSILIKE. Questa proprietà è adatta ai provider che supportano il livello SQL-Minimum. La proprietà indica che il provider supporta l'operatore `LIKE`in base al livello SQL-92 (\'%\' e \'_\' come caratteri jolly). La proprietà è utile nei provider che supportano il livello SQL-Minimum poiché il livello SQL-Minimum non include il supporto `LIKE`.

- SQLPROP_INNERJOIN. Questa proprietà è adatta ai provider che supportano il livello SQL-Minimum. La proprietà indica il supporto per più tabelle nella clausola `FROM`. La proprietà è utile nei provider che supportano solo il livello SQL-Minimum poiché il livello SQL-Minimum non include il supporto per i join. La proprietà non indica il supporto per le parole chiave JOIN esplicite e non indica il supporto per i join OUTER. Indica solo il supporto di join impliciti tramite un elenco di tabelle nella clausola `FROM`.

- SQLPROP_DYNAMICSQL. Indica il supporto per `?` come marcatore di parametro. Il marcatore di parametro deve essere supportato al posto di un elemento scalare in una clausola `WHERE` o nell'elenco `SELECT`. Il supporto per i marcatori di parametro `?` consente a SQL Server di inviare query con parametri al provider.

- SQLPROP_NESTEDQUERIES. Indica il supporto per SELECT nidificati nella clausola `FROM` (ad esempio, `SELECT * FROM (SELECT * FROM T))`. In molti casi SQL Server usa istruzioni `SELECT` nidificate nella clausola `FROM` di una query quando genera le stringhe di query da eseguire in modalità remota. Poiché il supporto `SELECT` nidificato non è richiesto dal livello SQL-92, SQL Server non delega le query con istruzioni `SELECT` nidificate al provider, a meno che il provider non imposti anche questa proprietà. In alternativa, l'amministratore può impostare anche l'opzione del provider `Nested Queries` per consentire al provider di fare in modo che SQL Server generi query nidificate nel provider.

Il provider può supportare queste proprietà con un set di proprietà specifico di SQL Server denominato `SQLPROPSET_OPTHINTS` e avere valori `PROPID` definiti. Il set di proprietà `SQLPROPSET_OPTHINTS` e le due proprietà vengono definite usando le costanti seguenti:

```
extern const GUID `SQLPROPSET_OPTHINTS` = { 0x2344480c, 0x33a7, 0x11d1, { 0x9b, 0x1a, 0x0, 0x60, 0x8, 0x26, 0x8b, 0x9e } };
enum SQLPROPERTIES {
SQLPROP_NESTEDQUERIES = 0x4,
SQLPROP_DYNAMICSQL = 0x5,
SQLPROP_GROUPBY = 0x6,
SQLPROP_DATELITERALS = 0x7,
SQLPROP_ANSILIKE = 0x8,
SQLPROP_INNERJOIN = 0x9,
SQLPROP_SUBQUERIES = 0x10
};
```

#### <a name="character-set-and-sort-order-implications"></a>Implicazioni per set di caratteri e l'ordinamento

SQL Server supporta la specifica di regole di confronto per i dati di tipo carattere a livello di singola colonna. Le regole di confronto includono la specifica del set di caratteri e dell'ordinamento per i dati di tipo carattere non Unicode (colonne `char` e `varchar`). Per i dati Unicode (colonne `nchar` e `nvarchar`), le regole di confronto specificano solo l'ordinamento.

SQL Server delega i confronti di stringhe al provider solo se il set di caratteri (per i dati non Unicode), l'ordinamento e la semantica di confronto di stringhe usati dal server collegato corrispondono a quelli usati dal server locale.

Nel caso di server collegati SQL Server, SQL Server determina automaticamente la compatibilità delle regole di confronto. Per gli altri provider, l'amministratore deve indicare a SQL Server le regole di confronto dei dati di tipo carattere di un determinato server collegato. In SQL Server è supportata una nuova opzione di server collegato denominata `Collation Name`. Se l'amministratore stabilisce che la semantica delle regole di confronto adottata dal server collegato corrisponde alle regole di confronto SQL Server standard, l'amministratore può impostare l'opzione `Collation Name` sul nome delle regole di confronto. L'opzione `Collation Name` può essere abilitata usando la stored procedure di sistema `sp_serveroption`. L'opzione deve essere impostata solo se vengono soddisfatte entrambe le condizioni seguenti:

- L'ordinamento e il set di caratteri remoti corrispondono alle regole di confronto SQL Server specificate.

- La semantica di confronto di stringhe usata dal provider OLE DB segue le specifiche standard SQL-92 o in modo equivalente la semantica di confronto di SQL Server.

L'opzione Regole di confronto compatibili supportata in SQL Server 7.0 è ancora supportata per ragioni di compatibilità con le versioni precedenti. Impostare l'opzione su true equivale a impostare l'opzione Regole di confronto compatibili sulle regole di confronto predefinite del database master di SQL Server. Le nuove applicazioni devono usare l'opzione Nome regole di confronto anziché l'opzione Regole di confronto compatibili.

### <a name="indexed-access"></a>Accesso indicizzato

SQL Server usa un indice esposto dal provider per valutare determinati predicati della query distribuita. Questo scenario è possibile solo nei provider di indici e quando l'utente imposta l'opzione del provider `Index as Access Path`. Di seguito sono riportati i principali passaggi di livello elevato che SQL Server esegue nel provider quando viene usato un indice per l'esecuzione di una query:

1. Apre il set di righe dell'indice tramite `IOpenRowset::OpenRowset` con il nome di tabella e il nome di indice completi. I nomi di tabella e di indice completi vengono generati come descritto in precedenza nello scenario di query remota.

1. Apre il set di righe della tabella di base tramite `IOpenRowset::OpenRowset` con il nome di tabella completo.

1. Imposta gli intervalli nel set di righe dell'indice in base al predicato della query tramite `IRowsetIndex::SetRange`.

1. Esegue la scansione delle righe del set di righe dell'indice tramite `IRowset` nel set di righe dell'indice.

1. Usa la colonna segnalibro delle righe di indice recuperate per recuperare le righe corrispondenti del set di righe della tabella di base tramite `IRowsetLocate::GetRowsByBookmark`.

Le proprietà del set di righe `DBPROP_IRowsetLocate` e `DBPROP_BOOKMARKS` sono obbligatorie nel set di righe aperto nella tabella di base.

### <a name="pure-table-scans"></a>Scansioni di tabelle pure

SQL Server esegue la scansione dell'intera tabella remota del provider ed esegue la valutazione delle query in locale. Il set di righe corrispondente alla tabella viene aperto tramite una chiamata a `IOpenRowset::OpenRowset`. SQL Server costruisce il nome di tabella specificato in `OPENROWSET` in base alle parti del nome catalogo, schema e oggetto come segue:

1. Ognuna delle parti del nome è delimitata con il carattere di delimitazione del provider (`DBLITERAL_QUOTE`) e quindi concatenata con il carattere `DBLITERAL_CATALOG_SEPARATOR` incorporato.

1. Dopo l'apertura dell'oggetto set di righe, SQL Server usa l'interfaccia `IColumnsInfo` per verificare che i metadati della fase di esecuzione corrispondano ai metadati della fase di compilazione per la tabella.

1. SQL Server usa l'interfaccia `IRowset` per esplorare e usare righe della tabella. Usare `IRowset::GetNextRows` per recuperare le righe, `IRowset::RestartPosition` per tornare all'inizio del set di righe e `IRowset::ReleaseRows` per rilasciare le righe.

### <a name="update-and-delete-statements"></a>Istruzioni `UPDATE` e `DELETE`

Per aggiornare o eliminare una tabella remota da una query distribuita SQL Server, è necessario che siano soddisfatte le condizioni seguenti:

- Il provider deve supportare l'utilizzo di segnalibri nel set di righe aperto tramite `IOpenRowset` nella tabella da aggiornare o eliminare.

- Il provider deve supportare le interfacce `IRowsetLocate` e `IRowsetChange` nel set di righe aperto tramite `IOpenRowset` nella tabella da aggiornare o eliminare.

- L'interfaccia `IRowsetChange` deve supportare i metodi di aggiornamento (`SetData`) ed eliminazione (`DeleteRows`).

- Se il provider non supporta `ITransactionLocal`, le istruzioni `UPDATE` e `DELETE` sono consentite solo se l'opzione `Non-transacted` è impostata per il provider e se l'istruzione non si trova in una transazione utente.

- Se il provider non supporta `ITransactionJoin`, un'istruzione `UPDATE` o `DELETE` è consentita solo se non si trova in una transazione utente.

Nel set di righe aperto nella tabella aggiornata sono obbligatorie le proprietà del set di righe seguenti: `DBPROP_IRowsetLocate`, `DBPROP_IRowsetChange` e `DBPROP_BOOKMARKS`. La proprietà del set di righe `DBPROP_UPDATABILITY` è impostata su `DBPROPVAL_UP_CHANGE` o `DBPROPVAL_UP_DELETE`, a seconda che l'operazione eseguita sia rispettivamente `UPDATE` o `DELETE`.

Nel provider per l'elaborazione di un'operazione `UPDATE` o `DELETE` vengono eseguiti i passaggi di livello elevato seguenti:

1. SQL Server apre il set di righe della tabella di base attraverso l'interfaccia `IOpenRowset`. SQL Server richiede le proprietà sopra indicate nel set di righe.

1. SQL Server determina il set di righe valide da aggiornare o eliminare.

1. SQL Server usa i segnalibri per posizionarsi sulle righe qualificate attraverso l'interfaccia `IRowsetLocate`.

1. Usare `IRowsetChange::SetData` per le operazioni `UPDATE` oppure `IRowsetChange::DeleteRows` per le operazioni di eliminazione per eseguire le modifiche necessarie nelle righe valide.

### <a name="insert-statement"></a>Istruzione `INSERT`

Le condizioni per il supporto di istruzioni `INSERT` in una tabella remota sono meno restrittive di quelle per le istruzioni `UPDATE` e `DELETE`:

- Il provider deve supportare `IRowsetChange::InsertRow` nel set di righe aperto nella tabella di base in cui è inserito.

- Se il provider non supporta `ITransactionLocal`, le istruzioni `INSERT` sono consentite solo se l'opzione `Non-transacted updates` è impostata per il server collegato e se l'istruzione non si trova in una transazione utente.

- Se il provider non supporta `ITransactionJoin`, le istruzioni `INSERT` sono consentite solo se non si trovano in una transazione utente.

SQL Server usa `IOpenRowset::OpenRowset` per aprire un set di righe nella tabella di base e chiama `IRowsetChange::InsertRow` per inserire nuove righe nel set di righe di base.

### <a name="pass-through-queries"></a>Query pass-through

Questo scenario è simile allo scenario nella query remota, ad eccezione del fatto che il testo del comando in `ICommand` è una stringa di comando inviata dall'utente e non viene interpretata da SQL Server. SQL Server usa `DBGUID_DEFAULT` come identificatore del dialetto quando chiama `ICommandText::SetCommandText`. `DBGUID_DEFAULT` indica che il provider deve usare il relativo dialetto predefinito. Se il testo del comando restituisce più di un set di risultati, ad esempio se il comando richiama una stored procedure che restituisce più set di risultati, SQL Server usa solo il primo set di risultati del comando.

Per un elenco di tutte le interfacce OLE DB usate da SQL Server, vedere [Interfacce OLE DB usate da SQL Server](#appendixa).

### <a name="conclusion"></a>Conclusioni

Microsoft SQL Server offre il set di strumenti più affidabile per accedere ai dati da origini dati eterogenee. Grazie alla conoscenza delle interfacce OLE DB esposte da SQL Server, gli sviluppatori possono esercitare un elevato livello di controllo e complessità nelle query distribuite.

## <a name="ole-db-interfaces-consumed-by-sql-server"></a><a name="appendixa"></a> Interfacce OLE DB usate da SQL Server

Nella tabella seguente sono elencate tutte le interfacce OLE DB usate da SQL Server. La colonna Obbligatorio indica se l'interfaccia è inclusa nelle funzionalità OLE DB minime richieste da SQL Server o se è facoltativa. Se un'interfaccia non è contrassegnata come obbligatoria, SQL Server può ancora accedere al provider ma alcune specifiche funzionalità o ottimizzazioni SQL Server non sono possibili nel provider.

Nel caso delle interfacce facoltative, la colonna Scenari indica uno o più dei sei scenari in cui viene usata l'interfaccia specificata. Ad esempio, l'interfaccia `IRowsetChange` nei set di righe della tabella di base è un'interfaccia facoltativa. Questa interfaccia viene usata negli scenari delle istruzioni `UPDATE` e DELETE e dell'istruzione `INSERT`. Se l'interfaccia non è supportata, le istruzioni UPDATE, DELETE e `INSERT` non possono essere supportate nel provider. Per alcune delle altre interfacce facoltative è indicato \"prestazioni\" nella colonna Scenari a indicare che l'interfaccia produce prestazioni generali migliori. Ad esempio, se l'interfaccia `IDBSchemaRowset` non è supportata, SQL Server deve aprire il set di righe due volte: una volta per i metadati e una volta per l'esecuzione della query. Grazie al supporto di `IDBSchemaRowset`, le prestazioni di SQL Server risultano migliorate.

|Oggetto|Interfaccia|Obbligatoria|Commenti|Scenari|
|:-----|:-----|:-----|:-----|:-----|
|Oggetto origine dati|`IDBInitialize`|Sì|Inizializzare e impostare il contesto dei dati e di sicurezza.| |
| |`IDBCreateSession`|Sì|Creare un oggetto sessione database.| |
| |`IDBProperties`|Sì|Ottenere informazioni sulle capacità del provider, impostare le proprietà di inizializzazione, proprietà obbligatoria: DBPROP_INIT_TIMEOUT.| |
| |`IDBInfo`|No|Ottenere valore letterale di delimitazione, catalogo, nome, parte, separatore, carattere e così via.|Query remota.|
|Oggetto sessione database|`IDBSchemaRowset`|No|Ottenere i metadati della tabella/colonna. Set di righe necessari: `TABLES`, `COLUMNS`, `PROVIDER_TYPES`; altri set di righe usati se disponibili: `INDEXES`, `TABLE_STATISTICS`.|Prestazioni, accesso indicizzato.|
| |`IOpenRowset`|Sì|Aprire un set di righe in una tabella, indice o istogramma.| |
| |`IGetDataSource`|Sì|Usare per tornare all'oggetto di origine dati (DSO) da un oggetto sessione di database.| |
| |`IDBCreateCommand`|No|Usare per creare un oggetto comando (query) per i provider che supportano l'esecuzione di query.|Query remota, query pass-through.|
| |`ITransactionLocal`|No|Usare per gli aggiornamenti in transazioni.|Istruzioni `UPDATE`, `DELETE` e `INSERT`.|
| |`ITransactionJoin`|No|Usare per il supporto di transazioni distribuite.|Istruzioni `UPDATE`, `DELETE` e `INSERT` se in una transazione utente.|
|Oggetto set di righe|IRowset|Sì|Eseguire la scansione delle righe.| |
| |`IAccessor`|Sì|Associare alle colonne di un set di righe.| |
| |`IColumnsInfo`|Sì|Ottenere informazioni sulle colonne di un set di righe.| |
| |`IRowsetInfo`|Sì|Ottenere informazioni sulle proprietà di un set di righe.| |
| |`IRowsetLocate`|No|Necessaria per le operazioni `UPDATE`/`DELETE` e per eseguire ricerche basate su indice; usata per ricercare le righe in base a segnalibri.|Accesso indicizzati, istruzioni `UPDATE` e `DELETE`.|
| |`IRowsetChange`|No|Necessaria per `INSERTS`/`UPDATES`/ `DELETES` in un set di righe. I set di righe nelle tabelle di base devono supportare questa interfaccia per le istruzioni `INSERT`, `UPDATE` e `DELETE`.|Istruzioni `UPDATE`, `DELETE` e `INSERT`.|
| |`IConvertType`|Sì|Usare per verificare se il set di righe supporta le conversioni di tipi di dati specifici nelle colonne.| |
|Indice|`IRowset`|Sì|Eseguire la scansione delle righe.|Accesso indicizzato, prestazioni.|
| |`IAccessor`|Sì|Associare alle colonne di un set di righe.|Accesso indicizzato, prestazioni.|
| |`IColumnsInfo`|Sì|Ottenere informazioni sulle colonne di un set di righe.|Accesso indicizzato, prestazioni.|
| |`IRowsetInfo`|Sì|Ottenere informazioni sulle proprietà di un set di righe.|Accesso indicizzato, prestazioni.|
| |`IRowsetIndex`|Sì|Necessaria solo per i set di righe in un indice; usata per la funzionalità di indicizzazione (impostazione dell'intervallo, ricerca).|Accesso indicizzato, prestazioni.|
|Comando|`ICommand`|Sì| |Query remota, query pass-through.|
| |`ICommandText`|Sì|Usare per la definizione del testo delle query.|Query remota, query pass-through.|
| |`IColumnsInfo`|Sì|Usare per ottenere i metadati delle colonne per i risultati della query.|Query remota, query pass-through.|
| |`ICommandProperties`|Sì|Usare per specificare le proprietà obbligatorie nei set di righe restituiti dal comando.|Query remota, query pass-through.|
| |`ICommandWithParameters`|No|Usare per l'esecuzione di query con parametri.|Query remota, prestazioni.|
| |`ICommandPrepare`|No|Usare per preparare un comando per ottenere i metadati (usata nelle query pass-through, se disponibili).|Query remota, prestazioni.|
|Oggetto errore|`IErrorRecords`|Sì|Usare per ottenere un puntatore a un'interfaccia `IErrorInfo` corrispondente a un singolo record di errore.| |
| |`IErrorInfo`|Sì|Usare per ottenere un puntatore a un'interfaccia `IErrorInfo` corrispondente a un singolo record di errore.| |
|Qualsiasi oggetto|`ISupportErrorInfo`|No|Usare per verificare se una determinata interfaccia supporta gli oggetti errore.| |
|  |  |  |  |  |

>[!NOTE]
>L'oggetto `Index`, `Command` e `Error` non sono obbligatori. Tuttavia, se supportate, le interfacce elencate sono obbligatorie come specificato nella colonna Obbligatorio.

## <a name="sql-subset-used-for-generating-remote-queries"></a><a name="appendixb"></a>Subset SQL usato per la generazione di query remote

Il subset SQL che il componente Query Processor di SQL Server genera in un provider di comandi SQL dipende dal livello di sintassi supportato dal provider, come indicato dalla proprietà `DBPROP_SQLSUPPORT`.

Provider di comandi SQL che supportano il livello di base SQL o ODBC Core

SQL Server usa il subset seguente del linguaggio SQL per le query valutate dai provider di comandi SQL che supportano il livello SQL-92 o ODBC Core:

1. Istruzioni `SELECT` con le clausole `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `UNION`, `UNION ALL`, `ORDER BY DESC`, `ASC` e `HAVING`.

1. `UNION` e `UNION ALL` vengono generati solo nei provider che supportano il livello SQL-92, non in quelli che supportano ODBC Core.

1. Clausola `SELECT`:

   - Sottoquery scalari nell'elenco `SELECT`.

   - Alias di colonna senza la parola chiave `AS`.

1. Clausola `FROM`:

   - Le parole chiave di join esplicite non vengono usate. I nomi di tabella con valori delimitati da virgole vengono usati per specificare inner join e gli outer join non vengono specificati nelle query remote.

   - Query annidate nel formato `FROM` ( `<nested query>` ) `<alias>`.

   - Alias di tabella senza la parola chiave AS.

1. La clausola `WHERE` usa sottoquery con `NOT` `EXISTS`, `ANY`, `ALL`.

1. Espressioni:

   - Funzioni di aggregazione usate: `MIN([DISTINCT])`, `MAX([DISTINCT])`, `COUNT([DISTINCT])`, `SUM([DISTINCT])`, `AVG([DISTINCT])` e `COUNT(*)`.

   - Operatori di confronto: `<`, `=`, `<=`, `>`, `<>`, `>=`, `IS NULL` e `IS NOT NULL`.

   - Operatori booleani: `AND`, `OR` e `NOT`.

 - Operatori aritmetici: `+`, `-`, `*` e `/`.

1. Costanti:

- I valori letterali numerici e monetari sono sempre racchiusi tra `( )`.

- I valori letterali carattere sono delimitati da `' '`.

Provider di comandi SQL che supportano il livello SQL Minimum

Nei provider di comandi SQL che supportano il livello SQL Minimum, SQL Server genera SQL usando la grammatica seguente.

Questa grammatica è stata derivata usando la grammatica SQL Minimum descritta in ODBC 3.0. Tutte le differenze rispetto a questa grammatica sono evidenziate. Gli elementi visualizzati in `*bold italics`* sono gli elementi aggiunti alla grammatica SQL Minimum descritta in ODBC 3.0. Gli elementi visualizzati come eliminati in verde sono gli elementi rimossi dalla grammatica.

*select-statement* ::=

`SELECT [ALL | DISTINCT] *select-list* FROM *table-reference-list*[WHERE *search-condition*] [order-by-clause]`

Clausola `SELECT`

select-list ::= `*` | `select-sublist [, select-sublist]...`

select-sublist ::= expression * [`alias`]*

`*alias ::= user-defined-name`*

`FROM clause`

table-reference-list ::= table-reference

table-identifier ::= user-defined-name

table-name ::= table-identifier

table-reference ::= table-name

`WHERE clause`

search-condition ::= boolean-term \[OR search-condition\]

boolean-term ::= boolean-factor \[AND boolean-term\]

boolean-factor ::= \[NOT\] boolean-primary

boolean-primary ::= comparison-predicate \| ( search-condition )

comparison-predicate ::= expression comparison-operator expression

*\| `expression IS \[NOT\] NULL`*

comparison-operator ::= `< \| >` \| `<= \| >`= \| = \| `<>`

`ORDER BY clause`

order-by-clause ::= ORDER BY sort-specification \[, sort-specification\]\..

sort-specification ::= { \| column-name } \[ASC \| DESC\]

`Common syntactic elements`

expression ::= term \| expression {+\|--} term

term ::= factor \| term {\*\|/} factor

factor ::= \[+\|--\] primary

primary ::= column-name

Valore letterale \|

\| ( expression )

column-name ::= \[table-name.\]column-identifier

literal ::= character-string-literal

*\| integer-literal*

*\| exact-numeric-literal*

character-string-literal ::= \'{character}...\'

(carattere è qualsiasi carattere del set di caratteri del driver/origine dati. Per includere un singolo carattere di delimitazione valore letterale (\') in character-string-literal, usare due caratteri di delimitazione valore letterale (\'\').)

`*integer-literal ::=* \[*+ \| -*\] *unsigned-integer`*

`*exact-numeric-literal::=* \[*+ \| -*\] *unsigned-integer* \[*period unsigned-integer*\]`

`*\| period unsigned-integer`*

base-table-name ::= base-table-identifier

base-table-identifier ::= user-defined-name

column-identifier ::= user-defined-name

user-defined-name ::= letter\[digit \| letter \| _\]\..

unsigned-integer ::= {digit}...

digit ::= 0 \| 1 \| 2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8 \| 9

period ::= . 

## <a name="sql-server-specific-properties"></a><a name="appendixc"></a>Proprietà specifiche di SQL Server

```
enum SQLPROPERTIES
       {
       SQLPROP_NOHPNEEDED = 0x1,
       SQLPROP_FREETHREADED = 0x2,
       SQLPROP_UMSENABLED = 0x3,
       SQLPROP_NESTEDQUERIES = 0x4,
       SQLPROP_DYNAMICSQL = 0x5,
       SQLPROP_GROUPBY = 0x6,
       SQLPROP_DATELITERALS = 0x7,
       SQLPROP_ANSILIKE = 0x8,
       SQLPROP_INNERJOIN = 0x9,
       SQLPROP_SUBQUERIES = 0x10, 
       SQLPROP_PARALLELSCAN = 0x11,
       SQLPROP_COLUMNCOLLATION = 0x12,
       SQLPROP_CARDINALITY = 0x13,
       SQLPROP_SIMPLEUPDATES = 0x14,
       SQLPROP_SQLLIKE = 0x15,
       SQLPROP_BITREMOTING = 0x16,
       SQLPROP_UNICODELITERALS = 0x17,
       SQLPROP_USELATESTCOLLATIONVERSION = 0x18
       };

```
