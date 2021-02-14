---
title: Indici per tabelle con ottimizzazione per la memoria | Microsoft Docs
description: Informazioni sulle differenze tra un indice in una tabella ottimizzata per la memoria e un indice tradizionale in una tabella basata su disco in SQL Server e nel database SQL di Azure.
ms.custom: ''
ms.date: 09/16/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: eecc5821-152b-4ed5-888f-7c0e6beffed9
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 15cabb6877e249a6f0be9d38449c85c13bbda78b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351238"
---
# <a name="indexes-on-memory-optimized-tables"></a>Indici in tabelle con ottimizzazione per la memoria

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Tutte le tabelle ottimizzate per la memoria devono contenere almeno un indice in quanto gli indici consentono l'interconnessione delle righe. In una tabella con ottimizzazione per la memoria, ogni indice è anche ottimizzato per la memoria. Le differenze tra un indice in una tabella ottimizzata per la memoria e un indice tradizionale in una tabella basata su disco sono molte:  

- Le righe di dati non vengono archiviate in pagine. Non è pertanto possibile fare riferimento a una raccolta di pagine o extent, né a partizioni o unità di allocazione per ottenere tutte le pagine di una tabella. Il concetto di pagine di indice per uno dei tipi di indici disponibili è presente, ma sono archiviati in modo diverso rispetto agli indici per le tabelle basate su disco. Non presentano il tipo tradizionale di frammentazione all'interno di una pagina e non usano quindi il fattore di riempimento.
- Le modifiche apportate agli indici nelle tabelle ottimizzate per la memoria durante la manipolazione dei dati non vengono mai scritte su disco. Solo le righe di dati e le modifiche apportate ai dati vengono scritte nel log delle transazioni. 
- Quando il database torna online, gli indici con ottimizzazione per la memoria vengono ricompilati. 

Tutti gli indici nelle tabelle ottimizzate per la memoria vengono creati in base alle definizioni degli indici durante il recupero del database.

Il tipo di indice deve essere uno dei seguenti:  
  
- Indice hash  
- Indice non cluster ottimizzato per la memoria (struttura interna predefinita di un albero B) 
  
Gli indici *hash* sono illustrati in dettaglio in [Indici hash per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index).  
Gli indici *non cluster* sono illustrati in dettaglio in [Indice non cluster per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index).  
Gli indici *columnstore* sono illustrati in [un altro articolo](../../relational-databases/indexes/columnstore-indexes-overview.md).  

## <a name="syntax-for-memory-optimized-indexes"></a>Sintassi per gli indici ottimizzati per la memoria  
  
Ogni istruzione CREATE TABLE per una tabella ottimizzata per la memoria deve includere un indice, in modo esplicito tramite un INDEX o in modo implicito tramite un vincolo PRIMAY KEY o UNIQUE.
  
Per essere dichiarata con la clausola DURABILITY = SCHEMA\_AND_DATA predefinita, la tabella ottimizzata per la memoria deve contenere una chiave primaria. La clausola PRIMARY KEY NONCLUSTERED nell'istruzione CREATE TABLE seguente soddisfa due requisiti:  
  
- Fornisce un indice per soddisfare il requisito minimo di un indice nell'istruzione CREATE TABLE.  
- Fornisce la chiave primaria necessaria per la clausola SCHEMA\_AND_DATA.  

    ```sql
    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int NOT NULL  
            PRIMARY KEY NONCLUSTERED,  
        ...  
    )  
        WITH (  
            MEMORY_OPTIMIZED = ON,  
            DURABILITY = SCHEMA_AND_DATA);  
    ```

> [!NOTE]  
> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hanno un limite di 8 indici per ogni tipo di tabella o tabella ottimizzata per la memoria. A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] non è più previsto un limite al numero di indici specifici di tabelle ottimizzate per la memoria e tipi di tabella.
  
### <a name="code-sample-for-syntax"></a>Esempio di codice per la sintassi  
  
Questa sottosezione contiene un blocco di codice Transact-SQL che mostra la sintassi per creare vari indici in una tabella ottimizzata per la memoria. Il codice dimostra quanto segue:  
  
1. Creare una tabella ottimizzata per la memoria.  
2. Usare le istruzioni ALTER TABLE per aggiungere due indici.  
3. Usare INSERT per inserire alcune righe di dati.  
   
    ```sql
    DROP TABLE IF EXISTS SupportEvent;  
    go  

    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int               not null   identity(1,1)  
        PRIMARY KEY NONCLUSTERED,  

        StartDateTime        datetime2     not null,  
        CustomerName         nvarchar(16)  not null,  
        SupportEngineerName  nvarchar(16)      null,  
        Priority             int               null,  
        Description          nvarchar(64)      null  
    )  
        WITH (  
        MEMORY_OPTIMIZED = ON,  
        DURABILITY = SCHEMA_AND_DATA);  
    go  
        
        --------------------  
        
    ALTER TABLE SupportEvent  
        ADD CONSTRAINT constraintUnique_SDT_CN  
        UNIQUE NONCLUSTERED (StartDateTime DESC, CustomerName);  
    go  

    ALTER TABLE SupportEvent  
        ADD INDEX idx_hash_SupportEngineerName  
        HASH (SupportEngineerName) WITH (BUCKET_COUNT = 64);  -- Nonunique.  
    go  
        
        --------------------  
        
    INSERT INTO SupportEvent  
        (StartDateTime, CustomerName, SupportEngineerName, Priority, Description)  
        VALUES  
        ('2016-02-23 13:40:41:123', 'Abby', 'Zeke', 2, 'Display problem.'     ),  
        ('2016-02-24 13:40:41:323', 'Ben' , null  , 1, 'Cannot find help.'    ),  
        ('2016-02-25 13:40:41:523', 'Carl', 'Liz' , 2, 'Button is gray.'      ),  
        ('2016-02-26 13:40:41:723', 'Dave', 'Zeke', 2, 'Cannot unhide column.');  
    go 
    ``` 
  
## <a name="duplicate-index-key-values"></a>Valori duplicati delle chiavi di indice

La presenza di valori duplicati per una chiave di indice potrebbe ridurre le prestazioni delle tabelle ottimizzate per la memoria. Il sistema deve attraversare le catene di voci duplicate per la maggior parte delle operazioni di lettura e scrittura dell'indice. Quando una catena di voci duplicate supera le 100 voci, la riduzione delle prestazioni può diventare misurabile.

### <a name="duplicate-hash-values"></a>Valori hash duplicati

Questo problema è più evidente nel caso di indici hash. Gli indici hash ne risentono maggiormente a causa delle considerazioni riportate di seguito:

- Il costo inferiore per ogni operazione per gli indici hash.
- L'interferenza di catene di duplicati di grandi dimensioni con la catena di collisioni hash.

Per ridurre la duplicazione in un indice, provare a eseguire le modifiche seguenti:

- Usare un indice non cluster.
- Aggiungere colonne aggiuntive alla fine della chiave di indice, per ridurre il numero di duplicati.
  - Ad esempio, è possibile aggiungere colonne che sono anche nella chiave primaria.

Per altre informazioni sulle collisioni hash, vedere [Indici hash per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index).

### <a name="example-improvement"></a>Miglioramento di esempio

Ecco un esempio di come evitare problemi di prestazioni insufficienti per l'indice.

Si consideri una tabella `Customers` con una chiave primaria in `CustomerId` e un indice nella colonna `CustomerCategoryID`. In genere esisteranno molti clienti in una determinata categoria e questo significa che saranno presenti molti valori duplicati per CustomerCategoryID all'interno di una determinata chiave dell'indice.

In questo scenario è consigliabile usare un indice non cluster in `(CustomerCategoryID, CustomerId)`. L'indice può essere usato per le query che usano un predicato che coinvolge `CustomerCategoryID`, ma la chiave di indice non contiene duplicati. Pertanto, la presenza di valori CustomerCategoryID duplicati o della colonna aggiuntiva nell'indice non causerà inefficienze per la manutenzione dell'indice.

La query seguente mostra il numero medio di valori di chiave di indice duplicati per l'indice in `CustomerCategoryID` nella tabella `Sales.Customers`, all'interno del database di esempio [WideWorldImporters](../../samples/wide-world-importers-what-is.md).

```sql
SELECT AVG(row_count) FROM
    (SELECT COUNT(*) AS row_count 
        FROM Sales.Customers
        GROUP BY CustomerCategoryID) a
```

Per valutare il numero medio di duplicati di chiave di indice per la tabella e l'indice in uso, sostituire `Sales.Customers` con il nome della tabella e `CustomerCategoryID` con l'elenco delle colonne di chiave di indice.

## <a name="comparing-when-to-use-each-index-type"></a>Confronto tra le situazioni in cui usare ogni tipo di indice  
  
La scelta del tipo di indice ottimale dipende dalla natura query.  

Quando si implementano tabelle ottimizzate per la memoria in un'applicazione esistente, la raccomandazione generale consiste nell'iniziare con gli indici non cluster, poiché le relative funzionalità sono più simili alle funzionalità degli indici non cluster e cluster tradizionali sulle tabelle basate su disco. 
  
### <a name="recommendations-for-nonclustered-index-use"></a>Indicazioni per l'uso di indici non cluster  
  
Un indice non cluster è da preferirsi a un indice hash quando:  
  
- Le query hanno una clausola `ORDER BY` nella colonna indicizzata.  
- Query in cui viene verificata solo la colonna o le colonne iniziali di un indice a più colonne.  
- Le query verificano la colonna indicizzata usando una clausola `WHERE` con:  
  - Una disuguaglianza: `WHERE StatusCode != 'Done'`  
  - Un'analisi dell'intervallo di valori: `WHERE Quantity >= 100`  
  
Un indice non cluster è da preferirsi a un indice hash in tutte le istruzioni SELECT seguenti:  

```sql
SELECT CustomerName, Priority, Description 
FROM SupportEvent  
WHERE StartDateTime > DateAdd(day, -7, GetUtcDate());  

SELECT StartDateTime, CustomerName  
FROM SupportEvent  
ORDER BY StartDateTime DESC; -- ASC would cause a scan.

SELECT CustomerName  
FROM SupportEvent  
WHERE StartDateTime = '2016-02-26';  
```
  
### <a name="recommendations-for-hash-index-use"></a>Indicazioni per l'uso di indici hash   
  
Gli [indici hash](../../relational-databases/sql-server-index-design-guide.md#hash_index) vengono usati principalmente per le ricerche di punti e non per le analisi di intervalli.

Un indice hash è da preferirsi a un indice non cluster quando le query usano predicati di uguaglianza e la clausola `WHERE` esegue il mapping a tutte le colonne chiave dell'indice, come nell'esempio seguente:  
  
```sql
SELECT CustomerName 
FROM SupportEvent  
WHERE SupportEngineerName = 'Liz';
```  

### <a name="multi-column-index"></a>Indice a più colonne  
  
Un indice a più colonne può essere un indice non cluster o un indice hash. Si supponga che le colonne di indice siano col1 e col2. Con l'istruzione `SELECT` seguente, solo l'indice non cluster risulterebbe utile per Query Optimizer:  
  
```sql
SELECT col1, col3  
FROM MyTable_memop  
WHERE col1 = 'dn';  
```

L'indice hash richiede che la clausola `WHERE` specifichi un test di uguaglianza per ognuna delle colonne nella propria chiave. In caso contrario, l'indice hash non è utile per Query Optimizer.  
  
Nessuno dei due tipi di indice è utile se la clausola `WHERE` specifica solo la seconda colonna nella chiave dell'indice.  

## <a name="summary-table-to-compare-index-use-scenarios"></a>Tabella di riepilogo per il confronto degli scenari d'uso degli indici  
  
Nella tabella seguente sono elencate tutte le operazioni supportate dai vari tipi di indice. *Sì* significa che l'indice è in grado di soddisfare la richiesta in modo appropriato e *No* significa che non lo è. 
  
| Operazione | Con ottimizzazione per la memoria, <br/> hash | Con ottimizzazione per la memoria, <br/> non cluster | Basato su disco, <br/> (non) cluster |  
| :-------- | :--------------------------- | :----------------------------------- | :------------------------------------ |  
| Index Scan, recupera tutte le righe della tabella. | Sì | Sì | Sì |  
| Index Seek su predicati di uguaglianza (=). | Sì <br/> (chiave completa necessaria) | Sì  | Sì |  
| Index Seek su predicati di disuguaglianza e di intervallo <br/> (>, <, <=, >=, `BETWEEN`). | No <br/> (risultati in un'analisi di indice) | Sì <sup>1</sup> | Sì |  
| Recupero di righe con un ordinamento corrispondente alla definizione dell'indice. | No | Sì | Sì |  
| Recupero di righe con un ordinamento inverso rispetto alla definizione dell'indice. | No | No | Sì |  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Per un indice non cluster ottimizzato per la memoria, non è necessaria la chiave completa per eseguire una ricerca nell'indice.  

## <a name="automatic-index-and-statistics-management"></a>Gestione automatica dell'indice e delle statistiche

Sfruttare le soluzioni, ad esempio la [deframmentazione dell'indice adattativo](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), per gestire automaticamente la deframmentazione dell'indice e gli aggiornamenti delle statistiche per uno o più database. Questa procedura sceglie automaticamente se ricompilare o riorganizzare un indice in base al relativo livello di frammentazione, tra gli altri parametri, e aggiornare le statistiche con una soglia lineare.

## <a name="see-also"></a><a name="Additional_Reading"></a> Vedere anche   
 [Guida per la progettazione di indici di SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
 [Indici hash per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [Indice non cluster per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)    
 [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) (Deframmentazione dell'indice adattativo)