---
description: CREATE COLUMNSTORE INDEX (Transact-SQL)
title: CREATE COLUMNSTORE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE INDEX
- COLUMNSTORE_INDEX_TSQL
- CREATE CLUSTERED COLUMNSTORE INDEX
- COLUMNSTORE_TSQL
- CREATE NONCLUSTERED COLUMNSTORE INDEX
- CREATE_NONCLUSTERED_COLUMNSTORE_INDEX_TSQL
- CREATE COLUMNSTORE INDEX
- CREATE_CLUSTERED_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE
dev_langs:
- TSQL
helpviewer_keywords:
- index creation [SQL Server], columnstore indexes
- columnstore index, creating
- CREATE COLUMNSTORE INDEX statement
- CREATE INDEX statement
ms.assetid: 7e1793b3-5383-4e3d-8cef-027c0c8cb5b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f1cd141fff964f13f4bbbae7bcd71bc1ee31694f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753391"
---
# <a name="create-columnstore-index-transact-sql"></a>CREATE COLUMNSTORE INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Convertire una tabella rowstore in un indice columnstore cluster o creare un indice columnstore non cluster. Usare un indice columnstore per eseguire in modo efficiente un'analisi operativa in tempo reale per un carico di lavoro OLTP o per migliorare la compressione dei dati e le prestazioni delle query per i carichi di lavoro di data warehousing.  
  
> [!NOTE]
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è possibile creare la tabella come indice columnstore cluster.   Non è più necessario creare prima una tabella rowstore e quindi convertirla in un indice columnstore cluster.  

> [!TIP]
> Per informazioni sulle linee guida di progettazione degli indici, vedere [Guida per la progettazione di indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md).

Passare agli esempi:  
-   [Esempi per la conversione di una tabella rowstore in columnstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#convert)  
-   [Esempi per gli indici columnstore non cluster](../../t-sql/statements/create-columnstore-index-transact-sql.md#nonclustered)  
  
Scenari correlati:  
-   [Indici columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
-   [Indici columnstore per il data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
  
Altre informazioni:  
-   [Guida agli indici columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)  
-   [Riepilogo delle funzionalità degli indici columnstore](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create a clustered columnstore index on disk-based table.  
CREATE CLUSTERED COLUMNSTORE INDEX index_name  
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ] 
[ ; ]  
  
--Create a nonclustered columnstore index on a disk-based table.  
CREATE [NONCLUSTERED]  COLUMNSTORE INDEX index_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
        ( column  [ ,...n ] )  
    [ WHERE <filter_expression> [ AND <filter_expression> ] ]
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ]
[ ; ]  
  
<with_option> ::=  
      DROP_EXISTING = { ON | OFF } -- default is OFF  
    | MAXDOP = max_degree_of_parallelism 
    | ONLINE = { ON | OFF } 
    | COMPRESSION_DELAY  = { 0 | delay [ Minutes ] }  
    | DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { partition_number_expression | range } [ ,...n ] ) ]  
  
<on_option>::=  
      partition_scheme_name ( column_name )
    | filegroup_name
    | "default"
  
<filter_expression> ::=  
      column_name IN ( constant [ ,...n ]  
    | column_name { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< } constant  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    [ORDER (column [,...n] ) ]  
    [ WITH ( DROP_EXISTING = { ON | OFF } ) ] --default is OFF  
[;]  

```
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

Alcune opzioni non sono disponibili in tutte le versioni del motore di database. La tabella seguente elenca le versioni in cui le opzioni vengono introdotte negli indici COLUMNSTORE CLUSTERED e COLUMNSTORE NONCLUSTERED:

|Opzione| CLUSTERED | NONCLUSTERED |
|---|---|---|
| COMPRESSION_DELAY | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] |
| DATA_COMPRESSION | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | 
| ONLINE | [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)] | [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] |
| WHERE - clausola | N/D | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] |

Tutte le opzioni sono disponibili nel database SQL di Azure.

### <a name="create-clustered-columnstore-index"></a>CREATE CLUSTERED COLUMNSTORE INDEX

Creare un indice columnstore cluster in cui tutti i dati vengono compressi e archiviati per colonna. L'indice include tutte le colonne della tabella e archivia l'intera tabella. Se la tabella esistente è un heap o un indice cluster, la tabella viene convertita in un indice columnstore cluster. Se la tabella è già archiviata come indice columnstore cluster, l'indice esistente viene eliminato e ricompilato.  
  
*index_name*  
Specifica il nome del nuovo indice.  
  
Se la tabella ha già un indice columnstore cluster, è possibile specificare lo stesso nome dell'indice esistente oppure usare l'opzione DROP_EXISTING per specificare un nuovo nome.  
  
ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*

Specifica il nome composto da una, due o tre parti della tabella da archiviare come indice columnstore cluster. Se la tabella è un heap o un indice cluster, viene convertita da rowstore a columnstore. Se la tabella è già un columnstore, questa istruzione consente di ricompilare l'indice columnstore cluster. Per eseguire la conversione in un indice columnstore cluster ordinato, l'indice esistente deve essere un indice columnstore cluster.
  
#### <a name="with-options"></a>Opzioni WITH

##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON

   `DROP_EXISTING = ON` specifica di eliminare l'indice esistente e di creare un nuovo indice columnstore.  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (DROP_EXISTING = ON);
```
   L'impostazione predefinita, DROP_EXISTING = OFF, prevede che il nome dell'indice sia uguale al nome esistente. Se il nome di indice specificato esiste già, si verifica un errore.  
  
##### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Esegue l'override della configurazione del server del massimo grado di parallelismo per la durata dell'operazione di indice. Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
   I valori per *max_degree_of_parallelism* possono essere:  
   - 1 - Disattiva la generazione di piani paralleli.  
   - \>1 - Limita al valore specificato, o a un valore più basso in base al carico di lavoro corrente del sistema, il numero massimo di processori usati in un'operazione parallela sugli indici. Ad esempio, se MAXDOP = 4, i processori usati sono al massimo 4.  
   - 0 (valore predefinito) - Usa il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (MAXDOP = 2);
```

   Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) e [Configurare operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
 
###### <a name="compression_delay--0--delay--minutes-"></a>COMPRESSION_DELAY = **0** | *delay* [ Minutes ]  
   Per una tabella basata su disco, *delay* specifica il numero minimo di minuti in cui un rowgroup differenziale deve rimanere nello stato CLOSED nel rowgroup differenziale prima che SQL Server lo comprima nel rowgroup compresso. Poiché le tabelle basate su disco non tengono traccia delle ore di inserimento e aggiornamento per le singole righe, SQL Server applica il ritardo ai rowgroup differenziali nello stato CLOSED.  
   Il valore predefinito è 0 minuti.  
   
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( COMPRESSION_DELAY = 10 Minutes );
```

   Per indicazioni su quando usare COMPRESSION_DELAY, vedere l'[introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
##### <a name="data_compression--columnstore--columnstore_archive"></a>DATA_COMPRESSION = COLUMNSTORE | COLUMNSTORE_ARCHIVE  
   Specifica l'opzione di compressione dei dati per la tabella, il numero di partizione o l'intervallo di partizioni specificato. descritte di seguito:   
- `COLUMNSTORE` è l'impostazione predefinita e indica di eseguire la compressione usando il columnstore che offre le prestazioni migliori. Questa è la scelta tipica.  
- `COLUMNSTORE_ARCHIVE` comprime la tabella o la partizione in una dimensione ancora inferiore. Usare questa opzione per situazioni come l'archiviazione in cui sono richieste risorse di archiviazione più piccole e si ha a disposizione più tempo per l'archiviazione e il recupero.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( DATA_COMPRESSION = COLUMNSTORE_ARCHIVE );
```
   Per altre informazioni sulla compressione, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  

###### <a name="online--on--off"></a>ONLINE = [ON | OFF]
- `ON` specifica che l'indice columnstore rimane online e disponibile durante la compilazione della nuova copia dell'indice.
- `OFF` specifica che l'indice non è disponibile per l'uso durante la compilazione della nuova copia.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( ONLINE = ON );
```

#### <a name="on-options"></a>Opzioni ON 
   Con le opzioni ON è possibile specificare le opzioni per l'archiviazione dei dati, ad esempio uno schema di partizione, un filegroup specifico o il filegroup predefinito. Se l'opzione ON non è specificata, l'indice usa le impostazioni della partizione o del filegroup della tabella esistente.  
  
   *partition_scheme_name* **(** _column_name_ **)**  
   Specifica lo schema di partizione per la tabella. Lo schema di partizione deve essere già presente nel database. Per creare lo schema di partizione, vedere [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
 
   *column_name* specifica la colonna in base alla quale viene eseguita la partizione di un indice partizionato. La colonna deve corrispondere all'argomento della funzione di partizione usata da *partition_scheme_name* per tipo di dati, lunghezza e precisione.  

   *filegroup_name*  
   Specifica il filegroup per l'archiviazione dell'indice columnstore cluster. Se non viene specificato alcun percorso e la tabella non è partizionata, l'indice usano lo stesso filegroup della tabella o vista sottostante. Il filegroup deve essere già esistente.  

   **"** default **"**  
   Per creare l'indice per il filegroup predefinito, usare "default" o [ default ].  
  
   Se si specifica "default", l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. QUOTED_IDENTIFIER è ON per impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
### <a name="create-nonclustered-columnstore-index"></a>CREATE [NONCLUSTERED] COLUMNSTORE INDEX  
Creare un indice columnstore non cluster in memoria in una tabella rowstore archiviata come heap o indice cluster. L'indice può avere una condizione filtrata e non è necessario includere tutte le colonne della tabella sottostante. L'indice columnstore richiede spazio sufficiente per archiviare una copia dei dati. È aggiornabile e viene aggiornato ogni volta che si modifica la tabella sottostante. L'indice columnstore non cluster per un indice cluster consente l'esecuzione di analisi in tempo reale.  
  
*index_name*  
   Specifica il nome dell'indice. *index_name* deve essere univoco all'interno della tabella, ma non è necessario che lo sia all'interno del database. Devono essere anche conformi alle regole degli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 **(** _column_  [ **,** ...*n* ] **)**  
    Specifica le colonne da archiviare. Un indice columnstore non cluster è limitato a 1024 colonne.  
   Ogni colonna deve essere di un tipo di dati supportato per gli indici columnstore. Vedere [Limitazioni e restrizioni](../../t-sql/statements/create-columnstore-index-transact-sql.md#LimitRest) per un elenco di tipi di dati supportati.  

ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*  
   Specifica il nome composto da una, due o tre parti della tabella che contiene l'indice.  

#### <a name="with-options"></a>Opzioni WITH
##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON  
   DROP_EXISTING = ON L'indice esistente deve essere eliminato e ricompilato. Il nome di indice specificato deve corrispondere a quello dell'indice esistente, mentre la definizione dell'indice può essere modificata. È possibile, ad esempio, specificare colonne diverse oppure opzioni dell'indice.
  
   DROP_EXISTING = OFF Se il nome di indice specificato esiste già, viene visualizzato un messaggio di errore. Il tipo di indice non può essere modificato tramite l'opzione DROP_EXISTING. Per quanto riguarda la sintassi compatibile con le versioni precedenti, WITH DROP_EXISTING equivale a WITH DROP_EXISTING = ON.  

###### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Esegue l'override dell'[opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) per la durata dell'operazione sugli indici. Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
   I valori per *max_degree_of_parallelism* possono essere:  
   - 1 - Disattiva la generazione di piani paralleli.  
   - \>1 - Limita al valore specificato, o a un valore più basso in base al carico di lavoro corrente del sistema, il numero massimo di processori usati in un'operazione parallela sugli indici. Ad esempio, se MAXDOP = 4, i processori usati sono al massimo 4.  
   - 0 (valore predefinito) - Usa il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
   Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Le operazioni parallele sugli indici non sono disponibili in tutte le edizioni di [!INCLUDE[msC](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
###### <a name="online--on--off"></a>ONLINE = [ON | OFF]   
- `ON` specifica che l'indice columnstore rimane online e disponibile durante la compilazione della nuova copia dell'indice.
- `OFF` specifica che l'indice non è disponibile per l'uso durante la compilazione della nuova copia. Negli indici non cluster la tabella di base rimane disponibile, solo l'indice columnstore non cluster non viene usato per rispondere alle query fino al completamento del nuovo indice. 

```sql
CREATE COLUMNSTORE INDEX ncci ON Sales.OrderLines (StockItemID, Quantity, UnitPrice, TaxRate) WITH ( ONLINE = ON );
```

##### <a name="compression_delay--0--delayminutes"></a>COMPRESSION_DELAY = **0** | \<delay>[Minuti]  
   Specifica un limite inferiore per il periodo di tempo in cui una riga deve rimanere nel rowgroup differenziale prima che sia idonea per la migrazione in un rowgroup compresso. Un cliente ad esempio può dire che se una riga non viene modificata per 120 minuti, diventa idonea per la compressione in formato di archiviazione a colonne. Per l'indice columnstore nelle tabelle basate su disco, non si tiene traccia dell'ora in cui una riga viene inserita o aggiornata, ma si usa l'ora di chiusura del rowgroup differenziale come proxy per la riga. La durata predefinita è 0 minuti. La migrazione di una riga nella risorsa di archiviazione a colonne viene eseguita quando risulta che nel rowgroup differenziale sono state accumulate 1 milione di righe e il rowgroup viene contrassegnato come chiuso.  
  
###### <a name="data_compression"></a>DATA_COMPRESSION  
   Specifica l'opzione di compressione dei dati per la tabella, il numero di partizione o l'intervallo di partizioni specificato. Si applica solo agli indici columnstore, inclusi gli indici columnstore cluster e quelli non cluster. descritte di seguito:
   
- `COLUMNSTORE`: è l'impostazione predefinita e indica di eseguire la compressione usando il columnstore che offre le prestazioni migliori. Questa è la scelta tipica.  
- `COLUMNSTORE_ARCHIVE`: COLUMNSTORE_ARCHIVE comprime ulteriormente la tabella o la partizione a una dimensione inferiore. Può essere utilizzata per l'archiviazione o in altre situazioni in cui sono richieste dimensioni di archiviazione inferiori ed è possibile concedere più tempo per l'archiviazione e il recupero.  
  
 Per altre informazioni sulla compressione, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
##### <a name="where-filter_expression--and-filter_expression-"></a>WHERE \<filter_expression> [ AND \<filter_expression> ]
  
   L'opzione è detta predicato del filtro e consente di specificare le righe da includere nell'indice. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea statistiche filtrate per le righe di dati nell'indice filtrato.  
  
   Il predicato del filtro usa una logica di confronto semplice. I confronti in cui vengono utilizzati valori letterali NULL non sono consentiti con gli operatori di confronto. In alternativa, utilizzare gli operatori IS NULL e IS NOT NULL.  
  
   Di seguito sono riportati alcuni esempi di predicati di filtro per la tabella `Production.BillOfMaterials`:  
   `WHERE StartDate > '20000101' AND EndDate <= '20000630'`    
   `WHERE ComponentID IN (533, 324, 753)`  
   `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
   
   Per le indicazioni relative agli indici filtrati, vedere [Creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="on-options"></a>Opzioni ON  
   Queste opzioni consentono di specificare i filegroup in cui viene creato l'indice.  
  
*partition_scheme_name* **(** _column_name_ **)**  
   Specifica lo schema di partizione che definisce i filegroup a cui vengono mappate le partizioni di un indice partizionato. È necessario includere lo schema di partizione all'interno del database eseguendo [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). 
   *column_name* specifica la colonna in base alla quale viene eseguita la partizione di un indice partizionato. La colonna deve corrispondere all'argomento della funzione di partizione usata da *partition_scheme_name* per tipo di dati, lunghezza e precisione. *column_name* non è limitato alle colonne nella definizione dell'indice. Quando si partiziona un indice columnstore, la colonna di partizionamento viene aggiunta dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] come colonna dell'indice, se non è già stata specificata.  
   Se non si specifica *partition_scheme_name* o *filegroup* e la tabella è partizionata, l'indice viene posizionato nello stesso schema di partizione, usando la stessa colonna di partizionamento della tabella sottostante.  
   Un indice columnstore su una tabella partizionata deve essere allineato con il partizionamento.  
   Per altre informazioni sul partizionamento degli indici, vedere [Tabelle e indici partizionati](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  

*filegroup_name*  
   Specifica il nome di un filegroup in cui creare l'indice. Se *filegroup_name* non viene specificato e la tabella non è partizionata, l'indice usa lo stesso filegroup della tabella sottostante. Il filegroup deve essere già esistente.  
 
**"** default **"**  
Crea l'indice specificato nel filegroup predefinito.  
  
In questo contesto il termine default non rappresenta una parola chiave, È un identificatore per il filegroup predefinito e pertanto deve essere delimitato, ad esempio ON **"** default **"** o ON **[** default **]** . Se si specifica "default", l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="general-remarks"></a><a name="GenRemarks"></a> Osservazioni generali  
È possibile creare un indice columnstore per una tabella temporanea. Quando si elimina la tabella o termina la sessione, viene eliminato anche l'indice.  

È possibile creare un indice columnstore cluster ordinato per colonne con qualsiasi tipo di dati supportato in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], ad eccezione delle colonne stringa.  
 
## <a name="filtered-indexes"></a>Indici filtrati  
Un indice filtrato è un indice non cluster ottimizzato, adatto per le query tramite cui viene selezionata una piccola percentuale di righe da una tabella. Utilizza un predicato del filtro per indicizzare una parte dei dati di una tabella. Un indice filtrato progettato correttamente consente di migliorare le prestazioni di esecuzione delle query e di ridurre i costi di archiviazione e di manutenzione.  
  
### <a name="required-set-options-for-filtered-indexes"></a>Opzioni SET necessarie per gli indici filtrati  
Le opzioni SET nella colonna Valore obbligatorio sono richieste ogni volta che si verifica una qualsiasi delle condizioni seguenti:  
- Viene creato un indice filtrato.  
- I dati di un indice filtrato vengono modificati tramite un'operazione INSERT, UPDATE, DELETE o MERGE.  
- L'indice filtrato viene usato da Query Optimizer per generare il piano di query.  
  
    |Opzioni SET|Valore richiesto|Valore server predefinito|Predefinito<br /><br /> OLE DB e ODBC predefinito|Predefinito<br /><br /> DB-Library predefinito|  
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|  
    |ANSI_NULLS|ON|ON|ON|OFF|  
    |ANSI_PADDING|ON|ON|ON|OFF|  
    |ANSI_WARNINGS*|ON|ON|ON|OFF|  
    |ARITHABORT|ON|ON|OFF|OFF|  
    |CONCAT_NULL_YIELDS_NULL|ON|ON|ON|OFF|  
    |NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|  
    |QUOTED_IDENTIFIER|ON|ON|ON|OFF|   
  
     *Quando il livello di compatibilità del database è impostato su 90 o su un valore maggiore, l'impostazione di ANSI_WARNINGS su ON comporta anche l'impostazione implicita di ARITHABORT su ON. Se il livello di compatibilità del database è impostato su 80 o su un valore inferiore, l'opzione ARITHABORT deve essere impostata su ON in modo esplicito.  
  
 Se le opzioni SET non sono corrette, possono verificarsi le condizioni seguenti:  
  
-   L'indice filtrato non viene creato.  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore ed esegue il rollback delle istruzioni INSERT, UPDATE, DELETE o MERGE tramite le quali vengono modificati i dati dell'indice.  
  
-   Query Optimizer non considera l'indice nel piano di esecuzione per le istruzioni Transact-SQL.  
  
 Per altre informazioni sugli indici filtrati, vedere [Creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md). 
  
##  <a name="limitations-and-restrictions"></a><a name="LimitRest"></a> Limitazioni e restrizioni  

**Ogni colonna di un indice columnstore deve contenere dati di business di uno dei seguenti tipi comuni:** 
-   datetimeoffset [ ( *n* ) ]  
-   datetime2 [ ( *n* ) ]  
-   Datetime  
-   smalldatetime  
-   Data  
-   time [ ( *n* ) ]  
-   float [ ( *n* ) ]  
-   real [ ( *n* ) ]  
-   decimal [ ( *precision* [ *, scale* ] **)** ]
-   numeric [ ( *precision* [ *, scale* ] **)** ]    
-   money  
-   SMALLMONEY  
-   bigint  
-   INT  
-   SMALLINT  
-   TINYINT  
-   bit  
-   nvarchar [ ( *n* ) ] 
-   nvarchar(max)  (Si applica a [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e ai livelli Premium e Standard (S3 e successive) e a tutti i livelli di offerte VCore, solo in indici columnstore cluster)   
-   nchar [ ( *n* ) ]  
-   varchar [ ( *n* ) ]  
-   nvarchar(max)  (Si applica a [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e ai livelli Premium e Standard (S3 e successive) e a tutti i livelli di offerte VCore, solo in indici columnstore cluster)
-   char [ ( *n* ) ]  
-   varbinary [ ( *n* ) ] 
-   varbinary (max)  (Si applica a [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e al database SQL di Azure a livello Premium e Standard (S3 e successive) e a tutti i livelli di offerte VCore, solo in indici columnstore cluster)
-   binary [ ( *n* ) ]  
-   uniqueidentifier (si applica a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive)
  
Se la tabella sottostante ha una colonna con un tipo di dati non supportato per gli indici columnstore, è necessario omettere la colonna dall'indice columnstore non cluster.  
  
**Le colonne che usano uno qualsiasi dei tipi di dati seguenti non possono essere incluse in un indice columnstore:**
-   ntext, testo e immagine  
-   nvarchar(max), varchar(max) e varbinary(max) (si applica a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], alle versioni precedenti e agli indici columnstore non cluster) 
-   rowversion (e timestamp)  
-   sql_variant  
-   Tipi CLR (tipi spaziali e hierarchyid)  
-   Xml  
-   uniqueidentifier (si applica a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  

**Indici columnstore non cluster:**
-   Non può includere più di 1024 colonne.
-   Non può essere creato come indice basato su vincoli. Una tabella con un indice columnstore può includere vincoli univoci, vincoli di chiave primaria o vincoli di chiave esterna. I vincoli vengono applicati sempre con un indice rowstore. I vincoli non possono essere applicati con un indice columnstore (cluster o non cluster).
-   Non può includere una colonna di tipo sparse.  
-   Non può essere modificato usando l'istruzione **ALTER INDEX**. Per modificare l'indice non cluster, è invece necessario eliminare e ricreare l'indice columnstore. È possibile usare **ALTER INDEX** per disabilitare e ricompilare un indice columnstore.  
-   Non può essere creato usando la parola chiave **INCLUDE**.  
-   Impossibile includere le parole chiave **ASC** o **DESC** per l'ordinamento dell'indice. Gli indici columnstore vengono ordinati in base agli algoritmi di compressione. L'ordinamento comporta molti dei vantaggi a livello di prestazioni.  
-   Impossibile includere colonne LOB (Large Object) di tipo nvarchar(max), varchar(max) e varbinary(max) in indici columnstore non cluster. Solo gli indici columnstore cluster supportano i tipi LOB, a partire dalla versione [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e nel database SQL di Azure configurato ai livelli Premium e Standard (S3 e successive) e a tutti i livelli di offerte VCore. Si noti che le versioni precedenti non supportano i tipi LOB negli indici columnstore cluster e non cluster.


> [!NOTE]  
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile creare un indice columnstore non cluster in una vista indicizzata.  


 **Non è possibile combinare gli indici columnstore con le funzionalità seguenti:**  
-   Colonne calcolate. A partire da SQL Server 2017, un indice columnstore cluster può contenere una colonna calcolata non persistente. Tuttavia, in SQL Server 2017 gli indici columnstore cluster non possono contenere colonne calcolate persistenti e non è possibile indici non cluster per le colonne calcolate. 
-   Compressione di riga e di pagina e formato di archiviazione **vardecimal** (un indice columnstore è già compresso in un formato diverso).  
-   Replica  
-   Filestream

Non è possibile usare cursori o trigger in una tabella con un indice columnstore cluster. Questa restrizione non si applica agli indici columnstore non cluster. In una tabella con un indice columnstore non cluster è possibile usare cursori e trigger.

**Limiti specifici di [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**  
Le limitazioni seguenti si applicano solo a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. In questa versione sono stati introdotti gli indici columnstore cluster aggiornabili. Gli indici columnstore non cluster erano ancora di sola lettura.  

-   Change Tracking. Non è possibile usare il rilevamento delle modifiche con gli indici columnstore.  
-   Change Data Capture. Change Data Capture non può essere abilitato per le tabelle con un indice columnstore cluster. A partire da SQL Server 2016, può essere abilitato per le tabelle con un indice columnstore non cluster.  
-   Secondario leggibile. Non è possibile accedere a un indice columnstore cluster (CCI) da una replica secondaria leggibile di un gruppo di disponibilità Always On leggibile.  È possibile accedere a un indice columnstore non cluster (NCCI) da una replica secondaria leggibile.  
-   MARS (Multiple Active Result Sets). [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] usa MARS per le connessioni di sola lettura alle tabelle con un indice columnstore. Tuttavia, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] non supporta MARS per le operazioni simultanee di Data Manipulation Language (DML) su una tabella con indice columnstore. Quando questo si verifica,[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] termina le connessioni e interrompe le transazioni.  
-  Non è possibile creare gli indici columnstore non cluster in una vista o vista indicizzata.
  
 Per informazioni sui vantaggi a livello di prestazioni e sulle limitazioni degli indici columnstore, vedere [Indici columnstore - Panoramica](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
##  <a name="metadata"></a><a name="Metadata"></a> Metadati  
 Tutte le colonne di un indice columnstore vengono archiviate nei metadati come colonne incluse. L'indice columnstore non contiene colonne chiave. Nelle seguenti viste di sistema sono fornite informazioni sugli indici columnstore.  
  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
-   [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)  
-   [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
-   [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
-   [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)  

##  <a name="examples-for-converting-a-rowstore-table-to-columnstore"></a><a name="convert"></a> Esempi per la conversione di una tabella rowstore in columnstore  
  
### <a name="a-convert-a-heap-to-a-clustered-columnstore-index"></a>R. Convertire un heap in un indice columnstore cluster  
 In questo esempio viene creata una tabella come heap, che viene poi convertita in un indice columnstore cluster denominato cci_Simple. In questo modo viene modificata l'archiviazione dell'intera tabella da rowstore a columnstore.  
  
```sql  
CREATE TABLE SimpleTable(  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cci_Simple ON SimpleTable;  
GO  
```  
  
### <a name="b-convert-a-clustered-index-to-a-clustered-columnstore-index-with-the-same-name"></a>B. Convertire un indice cluster in un indice columnstore cluster con lo stesso nome.  
 In questo esempio viene creato una tabella con un indice cluster, quindi viene illustrata la sintassi della conversione dell'indice cluster in un indice columnstore cluster. In questo modo viene modificata l'archiviazione dell'intera tabella da rowstore a columnstore.  
  
```sql  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cl_simple ON SimpleTable  
WITH (DROP_EXISTING = ON);  
GO  
```  
  
### <a name="c-handle-nonclustered-indexes-when-converting-a-rowstore-table-to-a-columnstore-index"></a>C. Gestire gli indici non cluster durante la conversione di una tabella rowstore in un indice columnstore.  
 Questo esempio illustra come gestire gli indici non cluster durante la conversione di una tabella rowstore in un indice columnstore. In realtà, a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] non è richiesta alcuna azione speciale. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definisce e ricompila automaticamente gli indici non cluster nel nuovo indice columnstore cluster.  
  
 Per eliminare gli indici non cluster, usare l'istruzione DROP INDEX prima di creare l'indice columnstore. L'opzione DROP_EXISTING elimina solo l'indice cluster che viene convertito. Non elimina gli indici non cluster.  
  
 In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] non è possibile creare un indice non cluster sulla base di un indice columnstore. Questo esempio illustra come nelle versioni precedenti sia necessario eliminare gli indici non cluster prima di creare l'indice columnstore.  
  
```sql  
--Create the table for use with this example.  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
  
--Create two nonclustered indexes for use with this example  
CREATE INDEX nc1_simple ON SimpleTable (OrderDateKey);  
CREATE INDEX nc2_simple ON SimpleTable (DueDateKey);   
GO  
  
--SQL Server 2012 and SQL Server 2014: you need to drop the nonclustered indexes  
--in order to create the columnstore index.   
  
DROP INDEX SimpleTable.nc1_simple;  
DROP INDEX SimpleTable.nc2_simple;  
  
--Convert the rowstore table to a columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_simple ON SimpleTable;   
GO  
```  
  
### <a name="d-convert-a-large-fact-table-from-rowstore-to-columnstore"></a>D. Convertire una tabella dei fatti di grandi dimensioni da rowstore a columnstore  
 In questo esempio viene illustrato come convertire una tabella dei fatti di grandi dimensioni da una tabella rowstore in una tabella columnstore.  
  
 Per convertire una tabella rowstore in una tabella columnstore.  
  
1.  Innanzitutto, creare una tabella di piccole dimensioni da usare nell'esempio.  
  
    ```sql  
    --Create a rowstore table with a clustered index and a nonclustered index.  
    CREATE TABLE MyFactTable (  
        ProductKey [INT] NOT NULL,  
        OrderDateKey [INT] NOT NULL,  
         DueDateKey [INT] NOT NULL,  
         ShipDateKey [INT] NOT NULL )  
    )  
    WITH (  
        CLUSTERED INDEX ( ProductKey )  
    );  
  
    --Add a nonclustered index.  
    CREATE INDEX my_index ON MyFactTable ( ProductKey, OrderDateKey );  
    ```  
  
2.  Eliminare tutti gli indici non cluster della tabella rowstore.  
  
    ```sql  
    --Drop all nonclustered indexes  
    DROP INDEX my_index ON MyFactTable;  
    ```  
  
3.  Eliminare l'indice cluster.  
  
    -   Eseguire questa operazione solo se si desidera specificare un nuovo nome per l'indice quando viene convertito in un indice columnstore cluster. Se l'indice cluster non viene eliminato, il nuovo indice columnstore cluster ha lo stesso nome.  
  
        > [!NOTE]  
        > Il nome dell'indice è più facile da ricordare se si usano il proprio nome. Tutti gli indici cluster rowstore usano il nome predefinito 'ClusteredIndex_\<GUID>'.  
  
    ```sql  
    --Process for dropping a clustered index.  
    --First, look up the name of the clustered rowstore index.  
    --Clustered rowstore indexes always use the DEFAULT name 'ClusteredIndex_<GUID>'.  
    SELECT i.name   
    FROM sys.indexes i   
    JOIN sys.tables t  
    ON ( i.type_desc = 'CLUSTERED' ) WHERE t.name = 'MyFactTable';  
  
    --Drop the clustered rowstore index.  
    DROP INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09 ON MyDimTable;  
    ```  
  
4.  Convertire la tabella rowstore in una tabella columnstore con un indice columnstore cluster.  
  
    ```sql  
    --Option 1: Convert to columnstore and name the new clustered columnstore index MyCCI.  
    CREATE CLUSTERED COLUMNSTORE INDEX MyCCI ON MyFactTable;  
  
    --Option 2: Convert to columnstore and use the rowstore clustered   
    --index name for the columnstore clustered index name.  
    --First, look up the name of the clustered rowstore index.  
    SELECT i.name   
    FROM sys.indexes i  
    JOIN sys.tables t   
    ON ( i.type_desc = 'CLUSTERED' )  
    WHERE t.name = 'MyFactTable';  
  
    --Second, create the clustered columnstore index and   
    --Replace ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    --with the name of your clustered index.  
    CREATE CLUSTERED COLUMNSTORE INDEX   
    ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
     ON MyFactTable  
    WITH DROP_EXISTING = ON;  
    ```  
  
### <a name="e-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>E. Convertire una tabella columnstore in una tabella rowstore con un indice cluster  
 Per convertire una tabella columnstore in una tabella rowstore con un indice cluster, usare l'istruzione CREATE INDEX con l'opzione DROP_EXISTING.  
  
```sql  
CREATE CLUSTERED INDEX ci_MyTable   
ON MyFactTable  
WITH ( DROP EXISTING = ON );  
```  
  
### <a name="f-convert-a-columnstore-table-to-a-rowstore-heap"></a>F. Convertire una tabella columnstore in un heap rowstore  
 Per convertire una tabella rowstore in un heap rowstore, eliminare l'indice columnstore cluster.  
  
```sql  
DROP INDEX MyCCI   
ON MyFactTable;  
```  
  

### <a name="g-defragment-by-rebuilding-the-entire-clustered-columnstore-index"></a>G. Eseguire la deframmentazione ricompilando l'intero indice columnstore cluster  
   Applicabile a: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 Sono disponibili due modi per ricompilare l'indice columnstore cluster per intero. È possibile usare CREATE CLUSTERED COLUMNSTORE INDEX o [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) e l'opzione REBUILD. Entrambi i metodi raggiungono gli stessi risultati.  
  
> [!NOTE]  
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], usare `ALTER INDEX...REORGANIZE` anziché ricompilare con i metodi descritti in questo esempio.  
  
```sql  
--Determine the Clustered Columnstore Index name of MyDimTable.  
SELECT i.object_id, i.name, t.object_id, t.name   
FROM sys.indexes i   
JOIN sys.tables t  
ON (i.type_desc = 'CLUSTERED COLUMNSTORE')  
WHERE t.name = 'RowstoreDimTable';  
  
--Rebuild the entire index by using CREATE CLUSTERED INDEX.  
CREATE CLUSTERED COLUMNSTORE INDEX my_CCI   
ON MyFactTable  
WITH ( DROP_EXISTING = ON );  
  
--Rebuild the entire index by using ALTER INDEX and the REBUILD option.  
ALTER INDEX my_CCI  
ON MyFactTable  
REBUILD PARTITION = ALL  
WITH ( DROP_EXISTING = ON );  
```  
  
##  <a name="examples-for-nonclustered-columnstore-indexes"></a><a name="nonclustered"></a> Esempi per gli indici columnstore non cluster  
  
### <a name="a-create-a-columnstore-index-as-a-secondary-index-on-a-rowstore-table"></a>R. Creare un indice columnstore come indice secondario per una tabella rowstore  
 Questo esempio consente di creare un indice columnstore non cluster in una tabella rowstore. In questa situazione può essere creato solo un indice columnstore. L'indice columnstore richiede memoria aggiuntiva poiché contiene una copia dei dati nella tabella rowstore. In questo esempio vengono creati una tabella e un indice cluster semplici, quindi viene illustrata la sintassi di creazione di un indice columnstore non cluster.  
  
```sql  
CREATE TABLE SimpleTable  
(ProductKey [INT] NOT NULL,   
OrderDateKey [INT] NOT NULL,   
DueDateKey [INT] NOT NULL,   
ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey);  
GO  
```  
  
### <a name="b-create-a-simple-nonclustered-columnstore-index-using-all-options"></a>B. Creare un indice columnstore non cluster semplice usando tutte le opzioni  
 Nell'esempio seguente viene illustrata la sintassi della creazione di un indice columnstore non cluster usando tutte le opzioni.  
  
```sql  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey)  
WITH (DROP_EXISTING =  ON,   
    MAXDOP = 2)  
ON "default"  
GO  
```  
  
 Per un esempio più complesso con tabelle partizionate, vedere [Indici columnstore - Panoramica](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
### <a name="c-create-a-nonclustered-columnstore-index-with-a-filtered-predicate"></a>C. Creare un indice columnstore non cluster con un predicato filtrato  
 Nell'esempio seguente viene creato un indice columnstore non cluster filtrato per la tabella Production.BillOfMaterials del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Il predicato del filtro può includere colonne che non sono colonne chiave nell'indice filtrato. Il predicato in questo esempio consente di selezionare solo le righe in cui EndDate non ha un valore NULL.  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithEndDate'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX "FIBillOfMaterialsWithEndDate"  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL;  
```  
  
###  <a name="d-change-the-data-in-a-nonclustered-columnstore-index"></a><a name="ncDML"></a> D. Modificare i dati in un indice columnstore non cluster  
   Si applica a: da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].
  
 Dopo aver creato un indice columnstore non cluster in una tabella, non è possibile modificare i dati direttamente nella tabella. Una query eseguita con INSERT, UPDATE, DELETE o MERGE ha esito negativo e restituisce un messaggio di errore. Per aggiungere o modificare i dati nella tabella, è possibile effettuare una delle operazioni seguenti:  
  
-   Disabilitare o eliminare l'indice columnstore. È possibile aggiornare i dati nella tabella. Se si disabilita l'indice columnstore, è possibile ricompilare l'indice columnstore al termine dell'aggiornamento dei dati. Ad esempio,  
  
    ```sql  
    ALTER INDEX mycolumnstoreindex ON mytable DISABLE;  
    -- update mytable --  
    ALTER INDEX mycolumnstoreindex on mytable REBUILD  
    ```  
  
-   Caricare i dati in una tabella di staging che non dispone di un indice columnstore. Compilare un indice columnstore nella tabella di staging. Passare la tabella di staging in una partizione vuota della tabella principale.  
  
-   Passare una partizione della tabella con l'indice columnstore in una tabella di staging vuota. Se è presente un indice columnstore nella tabella di staging, disabilitare l'indice columnstore. Eseguire gli aggiornamenti. Compilare o ricompilare l'indice columnstore. Passare la tabella di staging nuovamente nella partizione (ora vuota) della tabella principale.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-change-a-clustered-index-to-a-clustered-columnstore-index"></a>R. Modificare un indice cluster in un indice columnstore cluster  
 Usando l'istruzione CREATE CLUSTERED COLUMNSTORE INDEX con DROP_EXISTING = ON, è possibile:  
  
-   Trasformare un indice cluster in un indice columnstore cluster.  
  
-   Ricompilare un indice columnstore cluster.  
  
 In questo esempio viene creata la tabella xDimProduct come tabella rowstore con indice cluster e viene usata l'istruzione CREATE CLUSTERED COLUMNSTORE INDEX per modificare la tabella da tabella rowstore a tabella columnstore.  
  
```sql  
-- Uses AdventureWorks  
  
IF EXISTS (SELECT name FROM sys.tables  
    WHERE name = N'xDimProduct'  
    AND object_id = OBJECT_ID (N'xDimProduct'))  
DROP TABLE xDimProduct;  
  
--Create a distributed table with a clustered index.  
CREATE TABLE xDimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey)  
WITH ( DISTRIBUTION = HASH(ProductKey),  
    CLUSTERED INDEX (ProductKey) )  
AS SELECT ProductKey, ProductAlternateKey, ProductSubcategoryKey FROM DimProduct;  
  
--Change the existing clustered index   
--to a clustered columnstore index with the same name.  
--Look up the name of the index before running this statement.  
CREATE CLUSTERED COLUMNSTORE INDEX <index_name>   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="b-rebuild-a-clustered-columnstore-index"></a>B. Ricompilare un indice columnstore cluster  
 Usando come base l'esempio precedente, questo esempio usa CREATE CLUSTERED COLUMNSTORE INDEX per ricompilare l'indice columnstore cluster esistente denominato cci_xDimProduct.  
  
```sql  
--Rebuild the existing clustered columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_xDimProduct   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="c-change-the-name-of-a-clustered-columnstore-index"></a>C. Modificare il nome di un indice columnstore cluster  
 Per modificare il nome di un indice columnstore cluster, eliminare l'indice columnstore cluster esistente e quindi ricreare l'indice con un nuovo nome.  
  
 Si consiglia di eseguire questa operazione solo con una tabella vuota o di piccole dimensioni. La procedura necessaria per eliminare e ricompilare con un altro nome un indice columnstore cluster di grandi dimensioni richiede molto tempo.  
  
 Usando l'indice columnstore cluster cci_xDimProduct dell'esempio precedente, questo esempio elimina l'indice columnstore cluster cci_xDimProduct e quindi ricrea l'indice columnstore cluster con il nome mycci_xDimProduct.  
  
```sql  
--For illustration purposes, drop the clustered columnstore index.   
--The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xDimProduct;  
  
--Create a clustered index with a new name, mycci_xDimProduct.  
CREATE CLUSTERED COLUMNSTORE INDEX mycci_xDimProduct  
ON xdimProduct  
WITH ( DROP_EXISTING = OFF );  
```  
  
### <a name="d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>D. Convertire una tabella columnstore in una tabella rowstore con un indice cluster  
 Si potrebbe verificare una situazione in cui è preferibile eliminare un indice columnstore cluster e creare un indice cluster. In questo modo la tabella viene archiviata in formato rowstore. Questo esempio converte una tabella columnstore in una tabella rowstore con un indice cluster con lo stesso nome. Non vengono persi dati. Tutti i dati vengono inseriti nella tabella rowstore e le colonne indicate diventano le colonne chiave dell'indice cluster.  
  
```sql  
--Drop the clustered columnstore index and create a clustered rowstore index.   
--All of the columns are stored in the rowstore clustered index.   
--The columns listed are the included columns in the index.  
CREATE CLUSTERED INDEX cci_xDimProduct    
ON xdimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey, WeightUnitMeasureCode)  
WITH ( DROP_EXISTING = ON);  
```  
  
### <a name="e-convert-a-columnstore-table-back-to-a-rowstore-heap"></a>E. Convertire una tabella columnstore di nuovo in un heap rowstore  
 Usare [DROP INDEX (SQL Server PDW)](drop-index-transact-sql.md) per eliminare l'indice columnstore cluster e convertire la tabella in un heap rowstore. Questo esempio converte la tabella cci_xDimProduct in un heap rowstore. La tabella continua a essere distribuita, ma viene archiviata come heap.  
  
```sql  
--Drop the clustered columnstore index. The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xdimProduct;  
```  

### <a name="f-create-an-ordered-clustered-columnstore-index-on-a-table-with-no-index"></a>F. Creare un indice columnstore cluster ordinato in una tabella senza indice

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
```

### <a name="g-convert-a-clustered-columnstore-index-to-an-ordered-clustered-columnstore-index"></a>G. Convertire un indice columnstore cluster in un indice columnstore cluster ordinato

```sql  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
WITH (DROP_EXISTING = ON)
```

### <a name="h-add-a-column-to-the-ordering-of-an-ordered-clustered-columnstore-index"></a>H. Aggiungere una colonna all'ordinamento di un indice columnstore cluster ordinato

```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE column only.  Add PRODUCTKEY column to the ordering.
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE, PRODUCTKEY );
WITH (DROP_EXISTING = ON)
```
### <a name="i-change-the-ordinal-of-ordered-columns"></a>I. Modificare l'ordinamento delle colonne ordinate  
```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE, PRODUCTKEY.  Change the ordering to PRODUCTKEY, SHIPDATE.  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( PRODUCTKEY,SHIPDATE );
WITH (DROP_EXISTING = ON)
```