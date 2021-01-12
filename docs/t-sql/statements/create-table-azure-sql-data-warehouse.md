---
description: CREATE TABLE (Azure Synapse Analytics)
title: CREATE TABLE (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ea21c73c-40e8-4c54-83d4-46ca36b2cf73
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 733b129c9dd4f598e75baa31f49a4f9811edd5b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102361"
---
# <a name="create-table-azure-synapse-analytics"></a>CREATE TABLE (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crea una nuova tabella in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

Per comprendere le tabelle e il relativo uso, vedere [Tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-overview/).

> [!NOTE]
>  Le discussioni su [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] in questo articolo si applicano sia a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] che a [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] se non diversamente specificato.

 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

<a name="Syntax"></a>

## <a name="syntax"></a>Sintassi
  
```syntaxsql
-- Create a new table.
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( 
      { column_name <data_type>  [ <column_options> ] } [ ,...n ]
    )  
    [ WITH ( <table_option> [ ,...n ] ) ]  
[;]  

<column_options> ::=
    [ COLLATE Windows_collation_name ]  
    [ NULL | NOT NULL ] -- default is NULL  
    [ <column_constraint> ]

<column_constraint>::=
    {
        DEFAULT DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED  NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }

<table_option> ::=
    {
       CLUSTERED COLUMNSTORE INDEX --default for Azure Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column [,...n])  
      | HEAP --default for Parallel Data Warehouse
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) -- default is ASC
    }  
    {
        DISTRIBUTION = HASH ( distribution_column_name )
      | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics
      | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse
    }
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] -- default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) )

<data type> ::=
      datetimeoffset [ ( n ) ]  
    | datetime2 [ ( n ) ]  
    | datetime  
    | smalldatetime  
    | date  
    | time [ ( n ) ]  
    | float [ ( n ) ]  
    | real [ ( n ) ]  
    | decimal [ ( precision [ , scale ] ) ]   
    | numeric [ ( precision [ , scale ] ) ]   
    | money  
    | smallmoney  
    | bigint  
    | int   
    | smallint  
    | tinyint  
    | bit  
    | nvarchar [ ( n | max ) ]  -- max applies only to Azure Synapse Analytics 
    | nchar [ ( n ) ]  
    | varchar [ ( n | max )  ] -- max applies only to Azure Synapse Analytics  
    | char [ ( n ) ]  
    | varbinary [ ( n | max ) ] -- max applies only to Azure Synapse Analytics  
    | binary [ ( n ) ]  
    | uniqueidentifier  
```  

<a name="Arguments"></a>
## <a name="arguments"></a>Argomenti

 *database_name*  
 Nome del database in cui sarà contenuta la nuova tabella. Il valore predefinito è il database attuale.  
  
 *schema_name*  
 Schema della tabella. Specificare lo *schema* è facoltativo. Se si omette, verrà usato lo schema predefinito.  
  
 *table_name*  
 Nome della nuova tabella. Per creare una tabella temporanea locale, anteporre # al nome della tabella.  Per spiegazioni e indicazioni sulle tabelle temporanee, vedere [Tabelle temporanee in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-temporary/). 

 *column_name*  
 Nome di una colonna di tabella.

### <a name="column-options"></a><a name="ColumnOptions"></a> Opzioni delle colonne

 `COLLATE` *Windows_collation_name*  
 Specifica le regole di confronto per l'espressione. È necessario specificare una delle regole di confronto di Windows supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle regole di confronto di Windows supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Regole di confronto di Windows (Transact-SQL)](windows-collation-name-transact-sql.md).  
  
 `NULL` | `NOT NULL`  
 Specifica se i valori `NULL` sono consentiti nella colonna. Il valore predefinito è `NULL`.  
  
 [ `CONSTRAINT` *constraint_name* ] `DEFAULT` *constant_expression*  
 Specifica il valore di colonna predefinito.  
  
 | Argomento | Spiegazione |
 | -------- | ----------- |
 | *constraint_name* | Nome facoltativo per il vincolo. Il nome del vincolo è univoco all'interno del database. Può essere usato nuovamente in altri database. |
 | *constant_expression* | Il valore predefinito per la colonna. L'espressione deve essere un valore letterale o una costante. Ad esempio, queste espressioni costanti sono consentite: `'CA'`, `4`. Queste espressioni costanti non sono consentite: `2+3`, `CURRENT_TIMESTAMP`. |
  
### <a name="table-structure-options"></a><a name="TableOptions"></a> Opzioni della struttura di tabella

Per indicazioni sulla scelta del tipo di tabella, vedere [Indicizzazione di tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index/).
  
 `CLUSTERED COLUMNSTORE INDEX` 
 
Archivia la tabella come indice columnstore cluster. L'indice columnstore cluster viene applicato a tutti i dati della tabella. Questo comportamento è quello predefinito per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
 
 `HEAP` Archivia la tabella come heap. Questo comportamento è quello predefinito per [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 `CLUSTERED INDEX` ( *index_column_name* [ ,...*n* ] )  
 Archivia la tabella come indice cluster con una o più colonne chiave. Questo comportamento archivia i dati per riga. Usare *index_column_name* per specificare il nome di una o più colonne chiave nell'indice.  Per altre informazioni, vedere le tabelle rowstore nella sezione Osservazioni generali.
 
 `LOCATION = USER_DB` Questa opzione è deprecata. Sintatticamente viene accettata, ma non è più richiesta e non influisce sul comportamento.   
  
### <a name="table-distribution-options"></a><a name="TableDistributionOptions"></a> Opzioni di distribuzione della tabella

Per capire come scegliere il metodo di distribuzione migliore e usare le tabelle distribuite, vedere [Distribuzione di tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-distribute/).

`DISTRIBUTION = HASH` ( *distribution_column_name* ) Assegna ogni riga a una distribuzione eseguendo l'hashing del valore archiviato *distribution_column_name*. L'algoritmo è deterministico, ovvero l'hashing viene sempre eseguito sullo stesso valore per la stessa distribuzione.  La colonna di distribuzione deve essere definita come NOT NULL perché tutte le righe con NULL vengono assegnate alla stessa distribuzione.

`DISTRIBUTION = ROUND_ROBIN` Distribuisce le righe in modo uniforme tra tutte le distribuzioni secondo uno schema round robin. Questo comportamento è quello predefinito per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].

`DISTRIBUTION = REPLICATE` Archivia una copia della tabella in ogni nodo di calcolo. Per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] la tabella è archiviata in un database di distribuzione in ogni nodo di calcolo. Per [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] la tabella è archiviata in un filegroup [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che si estende oltre il nodo di calcolo. Questo comportamento è quello predefinito per [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
### <a name="table-partition-options"></a><a name="TablePartitionOptions"></a> Opzioni di partizione della tabella
Per indicazioni sull'uso delle partizioni di tabella, vedere [Partizionamento delle tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/).

 `PARTITION` ( *partition_column_name* `RANGE` [ `LEFT` | `RIGHT` ] `FOR VALUES` ( [ *boundary_value* [,...*n*] ] ))   
Crea una o più partizioni di tabella. Queste partizioni sono porzioni orizzontali della tabella che consentono di applicare operazioni a subset di righe, indipendentemente dal fatto che la tabella sia archiviata come heap, indice cluster o indice columnstore cluster. A differenza della colonna di distribuzione, le partizioni della tabella non determinano la distribuzione in cui viene archiviata ogni riga. Le partizioni della tabella determinano invece il modo in cui le righe vengono raggruppate e archiviate all'interno di ogni distribuzione.  

| Argomento | Spiegazione |
| -------- | ----------- |
|*partition_column_name*| Specifica la colonna che verrà usata da [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] per suddividere le righe in partizioni. Questa colonna può essere qualsiasi tipo di dati. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ordina i valori della colonna di partizione in ordine crescente. L'ordinamento dal basso verso l'alto va da `LEFT` a `RIGHT` nella specifica `RANGE`. |  
| `RANGE LEFT` | Specifica che il valore limite appartiene alla partizione a sinistra (i valori più bassi). Il valore predefinito è LEFT. |
| `RANGE RIGHT` | Specifica che il valore limite appartiene alla partizione a destra (i valori più alti). | 
| `FOR VALUES` ( *boundary_value* [,...*n*] ) | Specifica i valori limite per la partizione. *boundary_value* è un'espressione costante. Non può essere NULL. Deve corrispondere o essere convertibile in modo implicito nel tipo di dati di *partition_column_name*. Non può essere troncata durante la conversione implicita in modo che la dimensione e la scalabilità del valore non corrispondano al tipo di dati di *partition_column_name*<br></br><br></br>Se si specifica la clausola `PARTITION`, ma non si specifica un valore limite, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] crea una tabella partizionata con una sola partizione. Se applicabile, è possibile suddividere la tabella in due partizioni in un secondo momento.<br></br><br></br>Se si specifica un valore limite, la tabella risultante ha due partizioni: una per i valori inferiori al valore limite e una per i valori superiori al valore limite. Se si sposta una partizione in una tabella non partizionata, la tabella non partizionata riceverà i dati, ma non avrà i limiti della partizione nei metadati.| 

 Vedere [Creare una tabella partizionata](#PartitionedTable) nella sezione Esempi.

### <a name="ordered-clustered-columnstore-index-option"></a>Opzione relativa all'indice columnstore cluster ordinato 

L'indice columnstore cluster è l'opzione predefinita per la creazione di tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  I dati in un indice columnstore cluster non vengono ordinati prima di essere compressi in segmenti columnstore.  Quando si crea un indice columnstore cluster con ORDER, i dati vengono ordinati prima di essere aggiunti ai segmenti di indice e le prestazioni delle query possono quindi essere migliori. Per informazioni dettagliate, vedere [Ottimizzazione delle prestazioni con indice columnstore cluster ordinato](/azure/sql-data-warehouse/performance-tuning-ordered-cci?view=azure-sqldw-latest&preserve-view=true).  

È possibile creare un indice columnstore cluster ordinato per colonne con qualsiasi tipo di dati supportato in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], ad eccezione delle colonne stringa.  

Gli utenti possono cercare nella colonna **column_store_order_ordinal** in **sys.index_columns** le colonne in base alle quali una tabella è ordinata e la sequenza nell'ordinamento.  

Per informazioni dettagliate, vedere [Ottimizzazione delle prestazioni con indice columnstore cluster ordinato](/azure/sql-data-warehouse/performance-tuning-ordered-cci).   

### <a name="data-type"></a><a name="DataTypes"></a> Tipo di dati

[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] supporta i tipi di dati di uso più comune. Di seguito è riportato un elenco dei tipi di dati supportati con i relativi dettagli e byte per l'archiviazione. Per comprendere meglio i tipi di dati e il relativo uso, vedere [Tipi di dati per le tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-data-types).

Per una tabella delle conversioni di tipi di dati, vedere la sezione relativa alle conversioni implicite in [CAST e CONVERT (Transact-SQL)](../functions/cast-and-convert-transact-sql.md).

>[!NOTE]
>Per informazioni dettagliate, vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../functions/date-and-time-data-types-and-functions-transact-sql.md).

`datetimeoffset` [ ( *n* ) ]  
 Il valore predefinito di *n* è 7.  
  
 `datetime2` [ ( *n* ) ]  
Come per `datetime`, ad eccezione del fatto che è possibile specificare il numero di secondi frazionari. Il valore predefinito di *n* è `7`.  
  
|Valore *n*|Precision|Scalabilità|  
|--:|--:|-:|  
|`0`|19|0|  
|`1`|21|1|  
|`2`|22|2|  
|`3`|23|3|  
|`4`|24|4|  
|`5`|25|5|  
|`6`|26|6|  
|`7`|27|7|  
  
 `datetime`  
 Archivia data e ora del giorno con 19-23 caratteri in base al calendario gregoriano. La data può contenere anno, mese e giorno. L'ora contiene ora, minuti e secondi. In alternativa, è possibile visualizzare tre cifre per i secondi frazionari. Le dimensioni di archiviazione sono di 8 byte.  
  
 `smalldatetime`  
 Archivia una data e un'ora. La dimensione dello spazio di archiviazione è 4 byte.  
  
 `date`  
 Archivia una data usando un massimo di 10 caratteri per anno, mese e giorno in base al calendario gregoriano. La dimensione dello spazio di archiviazione è 3 byte. La data viene archiviata come numero intero.  
  
 `time` [ ( *n* ) ]  
 Il valore predefinito di *n* è `7`.  
  
 `float` [ ( *n* ) ]  
 Tipo di dati numerici approssimati da usare con dati numerici a virgola mobile. I dati a virgola mobile sono approssimati, ovvero non tutti i valori nell'intervallo del tipo di dati possono essere rappresentati in modo esatto. *n* specifica il numero di bit usati per archiviare la mantissa di `float` in notazione scientifica. *n* determina la precisione e la dimensione dello spazio di archiviazione. Se si specifica *n*, il valore deve essere compreso tra `1` e `53`. Il valore predefinito di *n* è `53`.  
  
| Valore *n* | Precision | Dimensioni dello spazio di archiviazione |  
| --------: | --------: | -----------: |  
| 1-24   | 7 cifre  | 4 byte      |  
| 25-53  | 15 cifre | 8 byte      |  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] interpreta *n* come uno dei due valori possibili. Se `1`<= *n* <= `24`, *n* viene interpretato come `24`. Se `25` <= *n* <= `53`, *n* viene interpretato come `53`.  
  
 Il tipo di dati [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] `float` è conforme allo standard ISO per tutti i valori di *n* da `1` a `53`. Il sinonimo per double precision è `float(53)`.  
  
 `real` [ ( *n* ) ]  
 La definizione di reale è la stessa di float. Il sinonimo di ISO per `real` è `float(24)`.  
  
 `decimal` [ ( *precision* [ *, scale* ] ) ] | `numeric` [ ( *precision* [ *, scale* ] ) ]  
 Archivia numeri con precisione e scala fisse.  
  
 *precisione*  
 Numero massimo totale di cifre decimali che è possibile archiviare, sia a destra che a sinistra del separatore decimale. La precisione deve essere un valore compreso tra `1` e la precisione massima di `38`. La precisione predefinita è `18`.  
  
 *scale*  
 Numero massimo di cifre decimali che è possibile archiviare a destra del separatore decimale. *Scale* deve essere un valore compreso tra `0` e *precision*. È possibile specificare *scale* solo se *precision* è specificato. La scalabilità predefinita è `0` e quindi `0` <= *scale* <= *precision*. Le dimensioni massime di archiviazione variano a seconda della precisione.  
  
| Precision | Byte per l'archiviazione  |  
| ---------: |-------------: |  
|  1-9       |             5 |  
| 10-19      |             9 |  
| 20-28      |            13 |  
| 29-38      |            17 |  
  
 `money` | `smallmoney`  
 Tipi di dati che rappresentano valori di valuta.  
  
| Tipo di dati | Byte per l'archiviazione |  
| --------- | ------------: |  
| `money`|8|  
| `smallmoney` |4|  
  
 `bigint` \| `int` \| `smallint` \| `tinyint`  
 Tipi di dati numerici esatti che utilizzano dati integer. La risorsa di archiviazione è illustrata nella tabella seguente.  
  
| Tipo di dati | Byte per l'archiviazione |  
| --------- | ------------: |  
| `bigint`|8|  
| `int` |4|  
| `smallint` |2|  
| `tinyint` |1|  
  
 `bit`  
 Tipo di dati integer che può accettare un valore di `1`, `0` o NULL. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ottimizza l'archiviazione delle colonne di tipo bit. Se una tabella contiene al massimo 8 colonne di tipo bit, le colonne vengono archiviate come singolo byte. Se la tabella contiene da 9 a 16 colonne di tipo bit, le colonne vengono archiviate come due byte e così via.  
  
 `nvarchar` [ ( *n* | `max` ) ]  -- `max` si applica solo a [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Dati Unicode di tipo carattere a lunghezza variabile. *n* può essere un valore compreso tra 1 e 4000. Tramite `max` viene indicato che la capacità di memorizzazione massima è di 2^31-1 byte (2 GB). Le dimensioni in byte dello spazio di archiviazione sono pari al doppio del numero di caratteri immessi + 2 byte. La lunghezza dei dati immessi può essere uguale a zero caratteri.  
  
 `nchar` [ ( *n* ) ]  
 Dati di tipo carattere Unicode a lunghezza fissa con una lunghezza di *n* caratteri. *n* deve essere un valore compreso tra `1` e `4000`. Le dimensioni di archiviazione, espresse in byte, sono pari al doppio di *n*.  
  
 `varchar` [ ( *n*  | `max` ) ]  -- `max` si applica solo a [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].   
 Dati di tipo carattere non Unicode a lunghezza variabile con una lunghezza di *n* byte. *n* deve essere un valore compreso tra `1` e `8000`. `max` indica che le dimensioni massime dello spazio di archiviazione sono 2^31-1 byte (2 GB). Le dimensioni dello spazio di archiviazione sono la lunghezza effettiva dei dati immessi + 2 byte.  
  
 `char` [ ( *n* ) ]  
 Dati di tipo carattere non Unicode a lunghezza fissa con una lunghezza di *n* byte. *n* deve essere un valore compreso tra `1` e `8000`. Le dimensioni di archiviazione corrispondono a *n* byte. L'impostazione predefinita per *n* è `1`.  
  
 `varbinary` [ ( *n*  | `max` ) ]  -- `max` si applica solo a [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Dati binary a lunghezza variabile. *n* può essere un valore compreso tra `1` e `8000`. Tramite `max` viene indicato che la capacità di memorizzazione massima è di 2^31-1 byte (2 GB). Le dimensioni di archiviazione sono pari all'effettiva lunghezza dei dati immessi + 2 byte. Il valore predefinito di *n* è 7.  
  
 `binary` [ ( *n* ) ]  
 Dati binari a lunghezza fissa con lunghezza di *n* byte. *n* può essere un valore compreso tra `1` e `8000`. Le dimensioni di archiviazione corrispondono a *n* byte. Il valore predefinito di *n* è `7`.  
  
 `uniqueidentifier`  
 GUID a 16 byte.  
   
<a name="Permissions"></a>  
## <a name="permissions"></a>Autorizzazioni  
 La creazione di una tabella richiede l'autorizzazione nel ruolo predefinito del database `db_ddladmin` o:
 - Autorizzazione `CREATE TABLE` per il database
 - Autorizzazione `ALTER SCHEMA` per lo schema che conterrà la tabella. 

La creazione di una tabella partizionata richiede l'autorizzazione nel ruolo predefinito del database `db_ddladmin` o

- l'autorizzazione `ALTER ANY DATASPACE`
  
 L'account di accesso che crea una tabella temporanea locale riceve le autorizzazioni `CONTROL`, `INSERT`, `SELECT` e `UPDATE` per la tabella.  
 
<a name="GeneralRemarks"></a>  
## <a name="general-remarks"></a>Osservazioni generali  
 
Per i limiti minimi e massimi, vedere [Limiti di capacità di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-service-capacity-limits/). 
 
### <a name="determining-the-number-of-table-partitions"></a>Determinazione del numero di partizioni della tabella
Ogni tabella definita dall'utente è suddivisa in tabelle più piccole che vengono archiviate in percorsi separati detti distribuzioni. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] usa 60 distribuzioni. In [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] il numero di distribuzioni dipende dal numero di nodi di calcolo.
 
Ogni distribuzione contiene tutte le partizioni della tabella. Ad esempio, se sono presenti 60 distribuzioni e quattro partizioni di tabella oltre a una partizione vuota, vi saranno 300 partizioni (5 x 60= 300). Se la tabella è un indice columnstore cluster, esisterà un solo indice columnstore per partizione, ovvero saranno disponibili 300 indici columnstore.

Si consiglia di usare un numero inferiore di partizioni di tabella per garantire che ogni indice columnstore abbia righe a sufficienza per poter sfruttare i vantaggi degli indici columnstore. Per altre informazioni, vedere [Partizionamento delle tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/) e [Indicizzazione di tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)  

### <a name="rowstore-table-heap-or-clustered-index"></a>Tabella rowstore (heap o indice cluster)

Una tabella rowstore è una tabella archiviata in ordine riga per riga. Può essere un heap o un indice cluster. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] crea tutte le tabelle rowstore con la compressione della pagina. Questo comportamento non è configurabile dall'utente.

### <a name="columnstore-table-columnstore-index"></a>Tabella columnstore (indice columnstore)

Una tabella columnstore è una tabella archiviata in ordine colonna per colonna. L'indice columnstore è la tecnologia che gestisce i dati archiviati in una tabella columnstore.  L'indice columnstore cluster non influisce sul modo in cui vengono distribuiti i dati, ma su come i dati vengono archiviati all'interno di ogni distribuzione.

Per modificare una tabella rowstore in una tabella columnstore, eliminare tutti gli indici esistenti dalla tabella e creare un indice columnstore cluster. Per un esempio, vedere [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md).

Per altre informazioni, vedere questi articoli:
- [Riepilogo delle funzionalità con versione degli indici columnstore](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)
- [Indicizzazione di tabelle in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)
- [Guida agli indici columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md) 

<a name="LimitationsRestrictions"></a>  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile definire un vincolo DEFAULT per una colonna di distribuzione.  
  
### <a name="partitions"></a>Partizioni
Quando si usano le partizioni, la colonna di partizione non può contenere regole di confronto solo Unicode. Ad esempio, l'istruzione seguente ha esito negativo.  
  
 ```sql
CREATE TABLE t1 ( c1 varchar(20) COLLATE Divehi_90_CI_AS_KS_WS) WITH (PARTITION (c1 RANGE FOR VALUES (N'')))
```  
 
 Se *boundary_value* è un valore letterale che deve essere convertito in modo implicito nel tipo di dati in *partition_column_name*, si verificherà una discrepanza. Il valore letterale viene visualizzato nelle viste del sistema [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], ma il valore convertito viene utilizzato per le operazioni di [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

### <a name="temporary-tables"></a>Tabelle temporanee

 Le tabelle temporanee globali che iniziano con ## non sono supportate.  
  
 Le tabelle temporanee locali presentano le seguenti limitazioni e restrizioni:  
  
-   Sono visibili solo per la sessione corrente. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] le elimina automaticamente alla fine della sessione. Per eliminarle in modo esplicito, usare l'istruzione DROP TABLE.   
-   Non possono essere rinominate. 
-   Non possono avere partizioni o viste.  
-   Non è possibile modificarne le autorizzazioni. Le istruzioni `GRANT`, `DENY` e `REVOKE` non possono essere usate con le tabelle temporanee locali.   
-   I comandi della console del database sono bloccati per le tabelle temporanee.   
-   Se si usa più di una tabella temporanea locale all'interno di un batch, ogni tabella deve avere un nome univoco. Se più sessioni eseguono lo stesso batch e creano la stessa tabella temporanea locale, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] aggiunge internamente un suffisso numerico al nome della tabella temporanea locale per gestire un nome univoco per ogni tabella temporanea locale.  
    
<a name="LockingBehavior"></a>   
## <a name="locking-behavior"></a>Comportamento di blocco  
 Acquisisce un blocco esclusivo per la tabella. Acquisisce un blocco condiviso per gli oggetti DATABASE, SCHEMA e SCHEMARESOLUTION.  
 

<a name="ExamplesColumn"></a>   
## <a name="examples-for-columns"></a>Esempi per le colonne

### <a name="a-specify-a-column-collation"></a><a name="ColumnCollation"></a> A. Specificare le regole di confronto a livello di colonna 
 Nell'esempio seguente la tabella `MyTable` viene creata con due diverse regole di confronto per la colonna. Per impostazione predefinita, la colonna, `mycolumn1`, ha le regole di confronto predefinite Latin1_General_100_CI_AS_KS_WS. La colonna `mycolumn2` ha le regole di confronto Frisian_100_CS_AS.  
  
```sql
CREATE TABLE MyTable   
  (  
    mycolumnnn1 nvarchar,  
    mycolumn2 nvarchar COLLATE Frisian_100_CS_AS )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```  
  
### <a name="b-specify-a-default-constraint-for-a-column"></a><a name="DefaultConstraint"></a> B. Specificare un vincolo DEFAULT per una colonna

 L'esempio seguente illustra la sintassi che consente di specificare un valore predefinito per una colonna. La colonna colA ha un vincolo predefinito denominato constraint_colA e un valore predefinito pari a 0.  
  
```sql
CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```

<a name="ExamplesTemporaryTables"></a> 
## <a name="examples-for-temporary-tables"></a>Esempi per le tabelle temporanee

### <a name="c-create-a-local-temporary-table"></a><a name="TemporaryTable"></a> C. Creare una tabella temporanea locale  
 L'esempio seguente crea una tabella temporanea locale denominata #myTable. La tabella viene specificata con un nome in tre parti, che inizia con #.
  
```sql
CREATE TABLE AdventureWorks.dbo.#myTable
  (  
   id int NOT NULL,  
   lastName varchar(20),  
   zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```

<a name="ExTableStructure"></a>  
## <a name="examples-for-table-structure"></a>Esempi per la struttura della tabella

### <a name="d-create-a-table-with-a-clustered-columnstore-index"></a><a name="ClusteredColumnstoreIndex"></a> D. Creare una tabella con un indice columnstore cluster  
 Nell'esempio seguente viene creata una tabella distribuita con un indice columnstore cluster. Ogni distribuzione verrà archiviata come columnstore.  
  
 L'indice columnstore cluster non influisce sul modo in cui vengono distribuiti i dati. I dati vengono sempre distribuiti per riga. L'indice columnstore cluster influisce sul modo in cui vengono archiviati i dati all'interno di ogni distribuzione.  
  
```sql
  CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH   
  (   
    DISTRIBUTION = HASH ( colB ),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  

### <a name="e-create-an-ordered-clustered-columnstore-index"></a><a name="OrderedClusteredColumnstoreIndex"></a> E. Creare un indice columnstore cluster ordinato

L'esempio seguente mostra come creare un indice columnstore cluster ordinato. L'indice è ordinato in base a SHIPDATE.

```sql
CREATE TABLE Lineitem  
WITH (DISTRIBUTION = ROUND_ROBIN, CLUSTERED COLUMNSTORE INDEX ORDER(SHIPDATE))  
AS  
SELECT * FROM ext_Lineitem
```

<a name="ExTableDistribution"></a> 
## <a name="examples-for-table-distribution"></a>Esempi per la distribuzione della tabella

### <a name="f-create-a-round_robin-table"></a><a name="RoundRobin"></a> F. Creare una tabella ROUND_ROBIN  
 L'esempio seguente crea una tabella ROUND_ROBIN con tre colonne e senza partizioni. I dati vengono diffusi in tutte le distribuzioni. La tabella viene creata con un indice columnstore cluster, che consente una migliore qualità delle prestazioni e della compressione dei dati rispetto a un heap o un indice rowstore cluster.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );  
```  
  
### <a name="g-create-a-hash-distributed-table"></a><a name="HashDistributed"></a> G. Creare una tabella hash distribuita

 Nell'esempio seguente viene creata la stessa tabella dell'esempio precedente. Tuttavia, per questa tabella le righe vengono distribuite (nella colonna `id`) anziché suddivise in modo casuale come una tabella ROUND_ROBIN. La tabella viene creata con un indice columnstore cluster, che consente una migliore qualità delle prestazioni e della compressione dei dati rispetto a un heap o un indice rowstore cluster.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),   
    CLUSTERED COLUMNSTORE INDEX  
  );  
```  
  
### <a name="h-create-a-replicated-table"></a><a name="Replicated"></a> H. Creare una tabella replicata  
 L'esempio seguente crea una tabella replicata simile agli esempi precedenti. Le tabelle replicate vengono copiate completamente in ogni nodo di calcolo. Con questa copia in ogni nodo di calcolo, lo spostamento dei dati viene ridotto per le query. Questo esempio viene creato con CLUSTERED INDEX, che offre una compressione dei dati migliore di quella di un heap. Un heap potrebbe non contenere righe sufficienti per eseguire una compressione CLUSTERED COLUMNSTORE INDEX valida.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = REPLICATE,
    CLUSTERED INDEX (lastName)  
  );  
```  

<a name="ExTablePartitions"></a> 
## <a name="examples-for-table-partitions"></a>Esempi per le partizioni della tabella

###  <a name="i-create-a-partitioned-table"></a><a name="PartitionedTable"></a> I. Creare una tabella partizionata

 L'esempio seguente crea la stessa tabella dell'esempio A, con l'aggiunta del partizionamento RANGE LEFT per la colonna `id`. Specifica quattro valori limite per le partizioni, ottenendo cinque partizioni.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
  (
  
    PARTITION ( id RANGE LEFT FOR VALUES (10, 20, 30, 40 )),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  
  
 In questo esempio i dati vengono ordinati nelle partizioni seguenti:  
  
- Partizione 1: col <= 10
- Partizione 2: 10 < col <= 20
- Partizione 3: 20 < col <= 30
- Partizione 4: 30 < col <= 40
- Partizione 5: 40 < col  
  
 Se questa stessa tabella viene partizionata RANGE RIGHT anziché RANGE LEFT (impostazione predefinita), i dati vengono ordinati nelle partizioni seguenti:  
  
- Partizione 1: col < 10  
- Partizione 2: 10 <= col < 20
- Partizione 3: 20 <= col < 30
- Partizione 4: 30 <= col < 40
- Partizione 5: 40 <= col  
  
### <a name="j-create-a-partitioned-table-with-one-partition"></a><a name="OnePartition"></a> J. Creare una tabella partizionata con una partizione

 L'esempio seguente crea una tabella partizionata con un'unica partizione. Non specifica valori limite e quindi si ottiene una sola partizione.  
  
```sql
CREATE TABLE myTable (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
    (
      PARTITION ( id RANGE LEFT FOR VALUES ( )),  
      CLUSTERED COLUMNSTORE INDEX  
    )  
;  
```  
  
### <a name="k-create-a-table-with-date-partitioning"></a><a name="DatePartition"></a> K. Creare una tabella con partizionamento di data

 L'esempio seguente crea una nuova tabella denominata `myTable`, con partizionamento per una colonna `date`. Usando RANGE RIGHT e le date per i valori limite, inserisce un mese di dati in ogni partizione.  
  
```sql
CREATE TABLE myTable (  
    l_orderkey      bigint,
    l_partkey       bigint,
    l_suppkey       bigint,
    l_linenumber    bigint,
    l_quantity      decimal(15,2),  
    l_extendedprice decimal(15,2),  
    l_discount      decimal(15,2),  
    l_tax           decimal(15,2),  
    l_returnflag    char(1),  
    l_linestatus    char(1),  
    l_shipdate      date,  
    l_commitdate    date,  
    l_receiptdate   date,  
    l_shipinstruct  char(25),  
    l_shipmode      char(10),  
    l_comment       varchar(44))  
WITH
  (
    DISTRIBUTION = HASH (l_orderkey),  
    CLUSTERED COLUMNSTORE INDEX,  
    PARTITION ( l_shipdate  RANGE RIGHT FOR VALUES
      (  
        '1992-01-01','1992-02-01','1992-03-01','1992-04-01','1992-05-01',
        '1992-06-01','1992-07-01','1992-08-01','1992-09-01','1992-10-01',
        '1992-11-01','1992-12-01','1993-01-01','1993-02-01','1993-03-01',
        '1993-04-01','1993-05-01','1993-06-01','1993-07-01','1993-08-01',
        '1993-09-01','1993-10-01','1993-11-01','1993-12-01','1994-01-01',
        '1994-02-01','1994-03-01','1994-04-01','1994-05-01','1994-06-01',
        '1994-07-01','1994-08-01','1994-09-01','1994-10-01','1994-11-01',
        '1994-12-01'  
      ))
  );  
```  
  
<a name="SeeAlso"></a>
## <a name="see-also"></a>Vedere anche
 
[CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) 
