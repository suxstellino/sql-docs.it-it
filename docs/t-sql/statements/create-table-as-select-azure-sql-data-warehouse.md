---
description: CREATE TABLE AS SELECT (Azure Synapse Analytics)
title: CREATE TABLE AS SELECT (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 10/07/2016
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: d1e08f88-64ef-4001-8a66-372249df2533
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9597182b2266ef67878a5d55b96ce731278d9d36
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204914"
---
# <a name="create-table-as-select-azure-synapse-analytics"></a>CREATE TABLE AS SELECT (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

CREATE TABLE AS SELECT (CTAS) è una delle più importanti funzionalità T-SQL disponibili. È un'operazione eseguita completamente in parallelo, che crea una nuova tabella basata sull'output di un'istruzione SELECT. CTAS è il modo più semplice e rapido per creare una copia di una tabella.   
 
 Ad esempio, usare CTAS per:  
  
-   Ricreare una tabella con una colonna di distribuzione hash diversa.
-   Ricreare una tabella come replicata.   
-   Crea un indice columnstore solo per alcune colonne della tabella.  
-   Eseguire query o importare dati esterni.  

> [!NOTE]  
> Poiché CTAS fa parte delle funzionalità di creazione di una tabella, questo argomento non ripropone i contenuti dell'argomento CREATE TABLE. Descrive invece le differenze tra le istruzioni CTAS e CREATE TABLE. Per i dettagli su CREATE TABLE, vedere l'istruzione [CREATE TABLE (Azure Synapse Analytics)](./create-table-azure-sql-data-warehouse.md). 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
<a name="syntax-bk"></a>

## <a name="syntax"></a>Sintassi   

```syntaxsql
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    WITH ( 
      <distribution_option> -- required
      [ , <table_option> [ ,...n ] ]    
    )  
    AS <select_statement>  
    OPTION <query_hint> 
[;]  

<distribution_option> ::=
    { 
        DISTRIBUTION = HASH ( distribution_column_name ) 
      | DISTRIBUTION = ROUND_ROBIN 
      | DISTRIBUTION = REPLICATE
    }   

<table_option> ::= 
    {   
        CLUSTERED COLUMNSTORE INDEX --default for Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column[,...n])
      | HEAP --default for Parallel Data Warehouse   
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) --default is ASC 
    }  
      | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] --default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) ) 
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT select_criteria  

<query_hint> ::=
    {
        MAXDOP 
    }
```  

<a name="arguments-bk"></a>
  
## <a name="arguments"></a>Argomenti  
Per informazioni dettagliate, vedere la [sezione degli argomenti](./create-table-azure-sql-data-warehouse.md#Arguments) in CREATE TABLE.  

<a name="column-options-bk"></a>

### <a name="column-options"></a>Opzioni colonne
`column_name` [ ,...`n` ]   
 I nomi di colonna non consentono le [opzioni delle colonne](./create-table-azure-sql-data-warehouse.md#ColumnOptions) menzionate in CREATE TABLE.  In alternativa, è possibile specificare un elenco facoltativo di uno o più nomi di colonna per la nuova tabella. Le colonne della nuova tabella useranno i nomi specificati. Quando si specificano nomi di colonna, il numero di colonne nell'elenco di colonne deve corrispondere al numero di colonne nei risultati di SELECT. Se non si specificano nomi di colonna, la nuova tabella di destinazione userà i nomi di colonna indicati nei risultati dell'istruzione SELECT. 
  
 Non è possibile specificare altre opzioni delle colonne, ad esempio tipi di dati, regole di confronto o supporto dei valori Null. Ognuno di questi attributi è derivato dai risultati dell'istruzione `SELECT`. Tuttavia, è possibile usare l'istruzione SELECT per modificare gli attributi. Per un esempio, vedere le indicazioni sull'uso di [CTAS per modificare gli attributi di colonna](#ctas-change-column-attributes-bk).   

<a name="table-distribution-options-bk"></a>

### <a name="table-distribution-options"></a>Opzioni di distribuzione della tabella

`DISTRIBUTION` = `HASH` ( *distribution_column_name* ) | ROUND_ROBIN | REPLICATE      
L'istruzione CTAS richiede un'opzione di distribuzione e non ha valori predefiniti. In questo differisce da CREATE TABLE, che ha valori predefiniti. 

Per maggiori dettagli e per comprendere come scegliere la colonna di distribuzione migliore, vedere la sezione [Opzioni di distribuzione della tabella](./create-table-azure-sql-data-warehouse.md#TableDistributionOptions) in CREATE TABLE. 

<a name="table-partition-options-bk"></a>

### <a name="table-partition-options"></a>Opzioni di partizione della tabella
L'istruzione CTAS crea una tabella non partizionata per impostazione predefinita, anche se la tabella di origine è partizionata. Per creare una tabella partizionata con l'istruzione CTAS, è necessario specificare l'opzione di partizione. 

Per informazioni dettagliate, vedere la sezione [Opzioni di partizione della tabella](./create-table-azure-sql-data-warehouse.md#TablePartitionOptions) in CREATE TABLE.

<a name="select-options-bk"></a>

### <a name="select-statement"></a>istruzione SELECT
L'istruzione SELECT è la differenza fondamentale tra CTAS e CREATE TABLE.  

 `WITH` *common_table_expression*  
 Indica un set di risultati denominato temporaneo, noto come espressione di tabella comune (CTE). Per altre informazioni, vedere [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 `SELECT` *select_criteria*  
 Popola la nuova tabella con i risultati di un'istruzione SELECT. *select_criteria* è il corpo dell'istruzione SELECT che determina i dati da copiare nella nuova tabella. Per informazioni sulle istruzioni SELECT, vedere [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
 
### <a name="query-hint"></a>Hint per la query
Gli utenti possono impostare MAXDOP su un valore integer per controllare il grado massimo di parallelismo.  Quando MAXDOP è impostato su 1, la query viene eseguita da un singolo thread.

 
<a name="permissions-bk"></a>  
  
## <a name="permissions"></a>Autorizzazioni  
CTAS richiede l'autorizzazione `SELECT` per tutti gli oggetti a cui si fa riferimento in *select_criteria*.

Per le autorizzazioni per la creazione di una tabella, vedere [Autorizzazioni](./create-table-azure-sql-data-warehouse.md#Permissions) in CREATE TABLE. 
  
<a name="general-remarks-bk"></a>
  
## <a name="general-remarks"></a>Osservazioni generali
Per informazioni dettagliate, vedere [Osservazioni generali](./create-table-azure-sql-data-warehouse.md#GeneralRemarks) in CREATE TABLE.

<a name="limitations-bk"></a>

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  

È possibile creare un indice columnstore cluster ordinato per colonne con qualsiasi tipo di dati supportato in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], ad eccezione delle colonne stringa.  

[SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) non influisce su CTAS. Per ottenere un comportamento simile, usare [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
 
Per informazioni dettagliate, vedere [Limitazioni e restrizioni](./create-table-azure-sql-data-warehouse.md#LimitationsRestrictions) in CREATE TABLE.

<a name="locking-behavior-bk"></a>
  
## <a name="locking-behavior"></a>Comportamento di blocco  
 Per informazioni dettagliate, vedere [Comportamento di blocco](./create-table-azure-sql-data-warehouse.md#LockingBehavior) in CREATE TABLE.
 
<a name="performance-bk"></a>
 
 ## <a name="performance"></a>Prestazioni 

Per una tabella hash distribuita, è possibile usare CTAS e scegliere una colonna di distribuzione diversa per ottenere prestazioni migliori di join e aggregazioni. Se non si prevede di scegliere un'altra colonna di distribuzione, si otterranno prestazioni ottimali di CTAS se si specifica la stessa colonna di distribuzione, poiché in questo modo si evita di distribuire di nuovo le righe. 

Se si usa CTAS per creare una tabella e le prestazioni non sono un aspetto essenziale, è possibile specificare `ROUND_ROBIN` per evitare di dover decidere quale colonna di distribuzione usare.

Per evitare spostamenti di dati nelle query successive, è possibile specificare `REPLICATE`, aumentando in questo modo l'archiviazione, per caricare una copia completa della tabella in ogni nodo di calcolo.  


<a name="examples-copy-table-bk"></a>

## <a name="examples-for-copying-a-table"></a>Esempi per la copia di una tabella

<a name="ctas-copy-table-bk"></a>

### <a name="a-use-ctas-to-copy-a-table"></a>R. Usare CTAS per copiare una tabella 
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Forse uno degli usi più comuni di `CTAS` è creare una copia di una tabella in modo che sia possibile modificare l'istruzione DDL. Se ad esempio originariamente la tabella è stata creata come `ROUND_ROBIN` e si vuole trasformarla in una tabella distribuita per una colonna, `CTAS` è come deve essere cambiata la colonna di distribuzione. Si può inoltre usare `CTAS` per modificare il partizionamento, l'indicizzazione o i tipi di colonna.

Si supponga che questa tabella sia stata creata usando il tipo di distribuzione predefinito di `ROUND_ROBIN` distribuita poiché non è stata specificata alcuna colonna di distribuzione in `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey INT NOT NULL,
    OrderDateKey INT NOT NULL,
    DueDateKey INT NOT NULL,
    ShipDateKey INT NOT NULL,
    CustomerKey INT NOT NULL,
    PromotionKey INT NOT NULL,
    CurrencyKey INT NOT NULL,
    SalesTerritoryKey INT NOT NULL,
    SalesOrderNumber NVARCHAR(20) NOT NULL,
    SalesOrderLineNumber TINYINT NOT NULL,
    RevisionNumber TINYINT NOT NULL,
    OrderQuantity SMALLINT NOT NULL,
    UnitPrice MONEY NOT NULL,
    ExtendedAmount MONEY NOT NULL,
    UnitPriceDiscountPct FLOAT NOT NULL,
    DiscountAmount FLOAT NOT NULL,
    ProductStandardCost MONEY NOT NULL,
    TotalProductCost MONEY NOT NULL,
    SalesAmount MONEY NOT NULL,
    TaxAmt MONEY NOT NULL,
    Freight MONEY NOT NULL,
    CarrierTrackingNumber NVARCHAR(25),
    CustomerPONumber NVARCHAR(25)
);
```

Ora si vuole creare una nuova copia di questa tabella con un indice columnstore cluster in modo che sia possibile sfruttare le prestazioni delle tabelle columnstore cluster. Può anche essere opportuno distribuire la tabella in ProductKey poiché si prevede di usare join per questa colonna e si vogliono evitare spostamenti dei dati durante i join in ProductKey. Infine, si vuole aggiungere il partizionamento in OrderDateKey in modo da poter eliminare rapidamente i dati precedenti eliminando le partizioni più datate. Di seguito è riportata l'istruzione CTAS che consente di copiare la vecchia tabella in una nuova tabella.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Infine è possibile rinominare le tabelle per passare alla nuova tabella e quindi eliminare quella precedente.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

<a name="examples-column-bk"></a>

## <a name="examples-for-column-options"></a>Esempi per le opzioni delle colonne

<a name="ctas-change-column-attributes-bk"></a>

### <a name="b-use-ctas-to-change-column-attributes"></a>B. Usare CTAS per modificare gli attributi di colonna 
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

In questo esempio viene usata un'istruzione CTAS per modificare i tipi di dati, il supporto dei valori Null e le regole di confronto per diverse colonne della tabella DimCustomer2.  
  
```sql  
-- Original table 
CREATE TABLE [dbo].[DimCustomer2] (  
    [CustomerKey] INT NOT NULL,  
    [GeographyKey] INT NULL,  
    [CustomerAlternateKey] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL  
)  
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerKey]));  
  
-- CTAS example to change data types, nullability, and column collations  
CREATE TABLE test  
WITH (HEAP, DISTRIBUTION = ROUND_ROBIN)  
AS  
SELECT  
    CustomerKey AS CustomerKeyNoChange,  
    CustomerKey*1 AS CustomerKeyChangeNullable,  
    CAST(CustomerKey AS DECIMAL(10,2)) AS CustomerKeyChangeDataTypeNullable,  
    ISNULL(CAST(CustomerKey AS DECIMAL(10,2)),0) AS CustomerKeyChangeDataTypeNotNullable,  
    GeographyKey AS GeographyKeyNoChange,  
    ISNULL(GeographyKey,0) AS GeographyKeyChangeNotNullable,  
    CustomerAlternateKey AS CustomerAlternateKeyNoChange,  
    CASE WHEN CustomerAlternateKey = CustomerAlternateKey 
        THEN CustomerAlternateKey END AS CustomerAlternateKeyNullable,  
    CustomerAlternateKey COLLATE Latin1_General_CS_AS_KS_WS AS CustomerAlternateKeyChangeCollation  
FROM [dbo].[DimCustomer2]  
  
-- Resulting table 
CREATE TABLE [dbo].[test] (
    [CustomerKeyNoChange] INT NOT NULL, 
    [CustomerKeyChangeNullable] INT NULL, 
    [CustomerKeyChangeDataTypeNullable] DECIMAL(10, 2) NULL, 
    [CustomerKeyChangeDataTypeNotNullable] DECIMAL(10, 2) NOT NULL, 
    [GeographyKeyNoChange] INT NULL, 
    [GeographyKeyChangeNotNullable] INT NOT NULL, 
    [CustomerAlternateKeyNoChange] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL, 
    [CustomerAlternateKeyNullable] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [CustomerAlternateKeyChangeCollation] NVARCHAR(15) COLLATE Latin1_General_CS_AS_KS_WS NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN);
```
 
Come passaggio finale, è possibile usare [RENAME &#40;Transact-SQL&#41;](../../t-sql/statements/rename-transact-sql.md) per cambiare i nomi delle tabelle. In questo modo DimCustomer2 diventa la nuova tabella.

```sql
RENAME OBJECT DimCustomer2 TO DimCustomer2_old;
RENAME OBJECT test TO DimCustomer2;

DROP TABLE DimCustomer2_old;
```

<a name="examples-table-distribution-bk"></a>

## <a name="examples-for-table-distribution"></a>Esempi per la distribuzione della tabella

<a name="ctas-change-distribution-method-bk"></a>

### <a name="c-use-ctas-to-change-the-distribution-method-for-a-table"></a>C. Usare CTAS per modificare il metodo di distribuzione per una tabella
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Questo semplice esempio illustra come modificare il metodo di distribuzione per una tabella. Evidenzia il modo in cui una tabella hash distribuita viene trasformata in round robin e la tabella round robin viene di nuovo trasformata in tabella hash distribuita. La tabella finale corrisponde alla tabella originale. 

Nella maggior parte dei casi non è necessario modificare una tabella hash distribuita in una tabella round robin. Più spesso può essere necessario modificare una tabella round robin in una tabella hash distribuita. Si può ad esempio caricare inizialmente una nuova tabella come round robin e successivamente trasformarla in una tabella hash distribuita per ottenere migliori prestazioni di join.

In questo esempio viene usato il database AdventureWorksDW. Per caricare la versione di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], vedere [Caricare i dati di esempio in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-sample-databases/)
 
```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a round-robin table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  
Successivamente, trasformarla di nuovo in tabella hash distribuita.

```sql
-- You just made DimSalesTerritory a round-robin table.
-- Change it back to the original hash-distributed table. 
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH(SalesTerritoryKey) 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
<a name="ctas-change-to-replicated-bk"></a>

### <a name="d-use-ctas-to-convert-a-table-to-a-replicated-table"></a>D. Usare CTAS per convertire una tabella in una tabella replicata  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Questo esempio è valido per la conversione di una tabella round robin o hash distribuita in una tabella replicata. Questo particolare esempio porta a un livello successivo il metodo di modifica del tipo di distribuzione visto in precedenza.  Poiché DimSalesTerritory è una dimensione e probabilmente una tabella più piccola, è possibile scegliere di ricreare la tabella come replicata per evitare spostamenti di dati quando si esegue il join con altre tabelle. 

```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a replicated table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
### <a name="e-use-ctas-to-create-a-table-with-fewer-columns"></a>E. Usare CTAS per creare una tabella con meno colonne
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Nell'esempio seguente viene creata una tabella round robin distribuita denominata `myTable (c, ln)`. La nuova tabella ha solo due colonne. Usa gli alias di colonna nell'istruzione SELECT per i nomi delle colonne.  
  
```sql  
CREATE TABLE myTable  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT CustomerKey AS c, LastName AS ln  
    FROM dimCustomer; 
```  

<a name="examples-query-hints-bk"></a>

## <a name="examples-for-query-hints"></a>Esempi per gli hint per la query

<a name="ctas-query-hint-bk"></a>

### <a name="f-use-a-query-hint-with-create-table-as-select-ctas"></a>F. Usare un hint per la query con CREATE TABLE AS SELECT (CTAS)  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
Questa query illustra la sintassi di base per l'uso di un hint di join per la query con l'istruzione CTAS. Dopo l'invio della query, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] applica la strategia di hash join quando genera il piano di query per ogni singola distribuzione. Per altre informazioni sull'hint per la query di hash join, vedere [Clausola OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql  
CREATE TABLE dbo.FactInternetSalesNew  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN   
  )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```  

<a name="examples-external-tables"></a>

## <a name="examples-for-external-tables"></a>Esempi per le tabelle esterne

<a name="ctas-azure-blob-storage-bk"></a>

### <a name="g-use-ctas-to-import-data-from-azure-blob-storage"></a>G. Usare CTAS per importare dati dall'archivio BLOB di Azure  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Per importare dati da una tabella esterna, usare CREATE TABLE AS SELECT per selezionare i dati dalla tabella esterna. La sintassi per selezionare i dati da una tabella esterna e inserirli in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] è la stessa usata per selezionare i dati da una normale tabella.  
  
 L'esempio seguente definisce una tabella esterna per i dati di un account di archiviazione BLOB di Azure. Usa quindi CREATE TABLE AS SELECT per selezionare dati dalla tabella esterna. I dati vengono importati dai file di testo delimitato dell'archiviazione BLOB di Azure e archiviati in una nuova tabella [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
```sql  
--Use your own processes to create the text-delimited files on Azure blob storage.  
--Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION='/logs/clickstream/2015/',  
    DATA_SOURCE = MyAzureStorage,  
    FILE_FORMAT = TextFileFormat)  
;  
  
--Use CREATE TABLE AS SELECT to import the Azure blob storage data into a new   
--Synapse Analytics table called ClickStreamData  
CREATE TABLE ClickStreamData   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;  
```  

<a name="ctas-import-Hadoop-bk"></a>
  
### <a name="h-use-ctas-to-import-hadoop-data-from-an-external-table"></a>H. Usare CTAS per importare dati Hadoop da una tabella esterna  
Applicabile a: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
Per importare dati da una tabella esterna, usare CREATE TABLE AS SELECT per selezionare i dati dalla tabella esterna. La sintassi per selezionare i dati da una tabella esterna e inserirli in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] è la stessa usata per selezionare i dati da una normale tabella.  
  
 L'esempio seguente definisce una tabella esterna per un cluster Hadoop. Usa quindi CREATE TABLE AS SELECT per selezionare dati dalla tabella esterna. I dati vengono importati dai file di testo delimitato di Hadoop e archiviati in una nuova tabella [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
```sql  
-- Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION = 'hdfs://MyHadoop:5000/tpch1GB/employee.tbl',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|')  
)  
;  
  
-- Use your own processes to create the Hadoop text-delimited files 
-- on the Hadoop Cluster.  
  
-- Use CREATE TABLE AS SELECT to import the Hadoop data into a new 
-- table called ClickStreamPDW  
CREATE TABLE ClickStreamPDW   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;   
```  
 
<a name="examples-workarounds-bk"></a>
 
## <a name="examples-using-ctas-to-replace-sql-server-code"></a>Esempi di uso di CTAS per sostituire il codice di SQL Server

Usare CTAS come soluzione alternativa se alcune funzionalità non sono supportate. Oltre a consentire l'esecuzione del codice nel data warehouse, la riscrittura del codice esistente per l'uso di CTAS in genere migliora le prestazioni. Questo è il risultato di una progettazione eseguita completamente in parallelo. 

> [!NOTE]
> Provare a considerare CTAS come prima soluzione. Se si ritiene che sia possibile risolvere un problema usando `CTAS`, in genere questo è il modo migliore per affrontarlo, anche se si devono scrivere più dati.
>

<a name="ctas-replace-select-into-bk"></a>

### <a name="i-use-ctas-instead-of-selectinto"></a>I. Usare CTAS anziché SELECT...INTO  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Il codice di SQL Server in genere usa SELECT...INTO per popolare una tabella con i risultati di un'istruzione SELECT. Questo è un esempio di istruzione SELECT..INTO di SQL Server.

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Questa sintassi non è supportata in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e Parallel Data Warehouse. Questo esempio illustra come riscrivere l'istruzione SELECT...INTO precedente come istruzione CTAS. È possibile scegliere una qualsiasi delle opzioni di distribuzione descritte nella sintassi CTAS. In questo esempio viene usato il metodo di distribuzione ROUND_ROBIN.

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<a name="ctas-replace-implicit-joins-bk"></a>

### <a name="j-use-ctas-and-implicit-joins-to-replace-ansi-joins-in-the-from-clause-of-an-update-statement"></a>J. Usare CTAS e join impliciti per sostituire i join ANSI nella clausola `FROM` di un'istruzione `UPDATE`  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

È possibile che sia necessario gestire un aggiornamento complesso che unisce più di due tabelle usando la sintassi di join ANSI per eseguire azioni UPDATE o DELETE.

Si supponga di dover aggiornare questa tabella:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]          SMALLINT    NOT NULL
,   [TotalSalesAmount]      MONEY       NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

L'aspetto della query originale potrebbe essere simile al seguente:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Poiché [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] non supporta i join ANSI nella clausola `FROM` di un'istruzione `UPDATE`, non è possibile usare questo codice di SQL Server senza modificarlo leggermente.

È possibile usare una combinazione di `CTAS` e un join implicito per sostituire questo codice:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

<a name="ctas-replace-ansi-joins-bk"></a>

### <a name="k-use-ctas-to-specify-which-data-to-keep-instead-of-using-ansi-joins-in-the-from-clause-of-a-delete-statement"></a>K. Usare CTAS per specificare i dati da mantenere anziché usare join ANSI nella clausola FROM di un'istruzione DELETE  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

In alcuni casi l'approccio migliore per l'eliminazione dei dati è usare `CTAS`. Anziché eliminare i dati, è sufficiente selezionare i dati da mantenere. Questa operazione è particolarmente utile per le istruzioni `DELETE` che usano la sintassi di join ANSI poiché [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] non supporta i join ANSI nella clausola `FROM` di un'istruzione `DELETE`.

Un esempio di istruzione DELETE convertita è disponibile di seguito:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

<a name="ctas-simplify-merge-bk"></a>

### <a name="l-use-ctas-to-simplify-merge-statements"></a>L. Usare CTAS per semplificare le istruzioni MERGE  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Le istruzioni MERGE possono essere sostituite, almeno in parte, usando `CTAS`. È possibile consolidare `INSERT` e `UPDATE` in una singola istruzione. Eventuali record eliminati dovranno essere isolati in una seconda istruzione.

Un esempio di `UPSERT` è riportato di seguito:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];

```

<a name="ctas-data-type-and-nullability-bk"></a>

### <a name="m-explicitly-state-data-type-and-nullability-of-output"></a>M. dichiarare in modo esplicito il tipo di dati e il supporto dei valori Null di output  
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Durante la migrazione del codice di SQL Server a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] è possibile incontrare il tipo di modello di codifica seguente:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Istintivamente si può pensare, con ragione, che è necessario eseguire la migrazione del codice in un'istruzione CTAS. Questa operazione però nasconde un problema.

Il codice seguente NON produce lo stesso risultato:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Si noti che la colonna del risultato riporta i valori relativi a tipo di dati e supporto dei valori Null dell'espressione. Questo può causare lievi variazioni nei valori se non si presta attenzione.

Provare quanto segue a titolo di esempio:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Il valore archiviato per il risultato è diverso. Poiché il valore persistente nella colonna del risultato viene usato in altre espressioni, l'errore diventa ancora più significativo.

![Risultati di CREATE TABLE AS SELECT](../../t-sql/statements/media/create-table-as-select-results.png)

Questo è particolarmente importante per le migrazioni di dati. Anche se la seconda query probabilmente è più precisa, esiste un problema. I dati risulterebbero diversi rispetto al sistema di origine e ciò comporta problemi di integrità della migrazione. Questo è uno dei rari casi in cui la risposta "sbagliata" in effetti è quella giusta.

Il motivo per cui si nota questa disparità tra i due risultati è il cast dei tipi eseguito in modo implicito. Nel primo esempio la tabella definisce la definizione di colonna. Quando viene inserita la riga si verifica una conversione implicita del tipo. Nel secondo esempio non vi è alcuna conversione implicita del tipo, poiché l'espressione definisce il tipo di dati della colonna. Si noti inoltre che, a differenza del primo esempio, la colonna del secondo esempio è stata definita come colonna che ammette valori Null. Durante la creazione della tabella del primo esempio il supporto dei valori Null da parte della colonna è stato definito in modo esplicito. Nel secondo esempio è stato gestito dall'espressione e per impostazione predefinita si è ottenuta una definizione NULL.  

Per risolvere questi problemi, è necessario impostare in modo esplicito la conversione del tipo e il supporto dei valori Null nella parte `SELECT` dell'istruzione `CTAS`. Non è possibile impostare queste proprietà nella parte CREATE TABLE.

L'esempio seguente illustra come correggere il codice:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Tenere presente quanto segue:
- CAST o CONVERT avrebbero potuto essere usati
- ISNULL viene usato per non unire il supporto dei valori Null
- ISNULL è la funzione più esterna
- La seconda parte di ISNULL è una costante, ad esempio 0

> [!NOTE]
> Per impostare in modo corretto il supporto dei valori Null, è essenziale usare `ISNULL` e non `COALESCE`. `COALESCE` non è una funzione deterministica e quindi il risultato dell'espressione ammette sempre i valori Null. `ISNULL` è differente. È deterministica. Quindi, se la seconda parte della funzione `ISNULL` è una costante o un valore letterale, il valore risultante sarà NOT NULL.

Questo suggerimento non è utile solo per garantire l'integrità dei calcoli. È importante anche per la commutazione delle partizioni della tabella. Si supponga che questa tabella sia definita come fact:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Tuttavia, il campo del valore è un'espressione calcolata e non fa parte dell'origine dati.

Per creare il set di dati partizionato, eseguire queste operazioni:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

La query verrebbe eseguita in modo corretto. Il problema si verifica quando si tenta di eseguire la commutazione delle partizioni. Le definizioni di tabella non corrispondono. Per fare in modo che le definizioni di tabella corrispondano, è necessario modificare l'istruzione CTAS.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

È quindi possibile vedere come la coerenza dei tipi e la gestione delle proprietà del supporto dei valori Null in CTAS siano importanti ai fini della progettazione. In questo modo si preserva l'integrità dei calcoli e si garantisce che la commutazione delle partizioni sia possibile.

### <a name="n-create-an-ordered-clustered-columnstore-index-with-maxdop-1"></a>N. Creare un indice columnstore cluster ordinato con MAXDOP 1  
```sql
CREATE TABLE Table1 WITH (DISTRIBUTION = HASH(c1), CLUSTERED COLUMNSTORE INDEX ORDER(c1) )
AS SELECT * FROM ExampleTable
OPTION (MAXDOP 1);
```

 
## <a name="see-also"></a>Vedere anche  
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [DROP EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER EXTERNAL TABLE &#40;Transact-SQL&#41;](./create-external-table-transact-sql.md)  
