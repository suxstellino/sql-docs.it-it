---
title: Risoluzione dei problemi per gli indici hash per tabelle ottimizzate per la memoria
description: Usare queste informazioni per risolvere i problemi relativi agli indici hash per le tabelle ottimizzate per la memoria in SQL Server e nel database SQL di Azure.
ms.custom: seo-dt-2019
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e922cc3a-3d6e-453b-8d32-f4b176e98488
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f91392232e871ad9ac9228edf31c116053feaa42
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492317"
---
# <a name="troubleshooting-hash-indexes-for-memory-optimized-tables"></a>Risoluzione dei problemi per gli indici hash per tabelle ottimizzate per la memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

## <a name="prerequisite"></a>Prerequisito  
  
Informazioni sul contesto importanti per la comprensione di questo articolo sono disponibili nell'articolo:  
  
- [Indici per tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)
- [Hash Indexes for Memory-Optimized Tables (Indici hash per tabelle con ottimizzazione per la memoria)](../../relational-databases/sql-server-index-design-guide.md#hash_index) 
 
## <a name="practical-numbers"></a>Numeri pratici  
  
Quando si crea un indice hash per una tabella ottimizzata per la memoria, il numero di bucket deve essere specificato al momento della creazione. Nella maggior parte dei casi, il numero di bucket dovrebbe essere in teoria impostato su un valore compreso tra una e due volte il numero di valori distinct della chiave di indice. 

Tuttavia, anche se il valore **BUCKET_COUNT** è moderatamente inferiore o superiore all'intervallo preferito, è probabile che le prestazioni dell'indice hash siano tollerabili o accettabili. Come minimo, valutare la possibilità di assegnare all'indice hash un valore **BUCKET_COUNT** quasi uguale al numero di righe che si prevede di raggiungere nella tabella ottimizzata per la memoria.  
Ad esempio, se la tabella espandibile contiene 2.000.000 righe, ma si prevede che crescerà di 10 volte, fino a 20.000.000 righe, iniziare con un numero di bucket 10 volte superiore al numero di righe nella tabella. In questo modo si avrà spazio sufficiente per un numero maggiore di righe.  
  
- In teoria, è consigliabile aumentare il numero di bucket quando la quantità di righe raggiunge il numero di bucket iniziale.  
- Anche se il numero di righe diventasse 5 volte superiore rispetto al numero di bucket, le prestazioni sarebbero ancora soddisfacenti in molte situazioni.  
  
Si supponga che un indice hash includa 10.000.000 valori di chiave distinct.  
  
- Un numero di bucket pari a 2.000.000 sarebbe il valore minimo accettabile. La riduzione del livello delle prestazioni potrebbe essere tollerabile.  
  
## <a name="too-many-duplicate-values-in-the-index"></a>L'indice contiene troppi valori duplicati?  
  
Se i valori indicizzati di hash presentano un tasso elevato di duplicati, i bucket di hash avranno catene più lunghe.  
  
Si supponga di avere la stessa tabella SupportEvent del blocco di codice con sintassi T-SQL riportato sopra. Il codice T-SQL seguente dimostra come trovare e visualizzare il rapporto tra *tutti* i valori e i valori *univoci* :  
  
```sql
-- Calculate ratio of:  Rows / Unique_Values.  
DECLARE @allValues float(8) = 0.0, @uniqueVals float(8) = 0.0;  
  
SELECT @allValues = Count(*) FROM SupportEvent;  
  
SELECT @uniqueVals = Count(*) FROM  
  (SELECT DISTINCT SupportEngineerName  
      FROM SupportEvent) as d;  
  
    -- If (All / Unique) >= 10.0, use a nonclustered index, not a hash.   
SELECT Cast((@allValues / @uniqueVals) as float) as [All_divby_Unique];  
go  
```
  
- Un rapporto di 10:0 o superiore indica che un hash sarebbe un tipo di indice inadeguato. Considerare la possibilità di utilizzare in alternativa un indice non cluster.   
  
## <a name="troubleshooting-hash-index-bucket-count"></a>Risoluzione dei problemi relativi al numero di bucket dell'indice hash  
  
Questa sezione illustra come risolvere i problemi relativi al numero di bucket per l'indice hash.  
  
### <a name="monitor-statistics-for-chains-and-empty-buckets"></a>Monitorare le statistiche per le catene e i bucket vuoti  
  
È possibile monitorare l'integrità statistica degli indici hash eseguendo l'istruzione T-SQL SELECT seguente. L'istruzione SELECT usa la vista di gestione dati (DMV) denominata **sys.dm_db_xtp_hash_index_stats**.  
  
```sql
SELECT  
  QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [table],   
  i.name                   as [index],   
  h.total_bucket_count,  
  h.empty_bucket_count,  
    
  FLOOR((  
    CAST(h.empty_bucket_count as float) /  
      h.total_bucket_count) * 100)  
                            as [empty_bucket_percent],  
  h.avg_chain_length,   
  h.max_chain_length  
FROM  
        sys.dm_db_xtp_hash_index_stats  as h   
  JOIN sys.indexes                     as i  
          ON h.object_id = i.object_id  
          AND h.index_id  = i.index_id  
JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
JOIN sys.tables t on h.object_id=t.object_id
WHERE ia.type=1
ORDER BY [table], [index];  
```
  
Confrontare i risultati dell'istruzione SELECT con le linee guida statistiche seguenti:  
  
- Bucket vuoti:  
  - 33% è un valore di destinazione valido, ma una percentuale più grande (anche 90%) è in genere accettabile.  
  - Quando il numero di bucket è pari al numero di valori di chiave distinct, circa il 33% dei bucket è vuoto.  
  - Un valore inferiore al 10% è troppo basso.  
- Catene all'interno dei bucket:  
  - Una lunghezza media della catena pari a 1 è ideale nel caso in cui non sono presenti valori duplicati delle chiavi di indice. Le lunghezze della catena fino a 10 sono in genere accettabili.  
  - Se la lunghezza media delle catene è maggiore di 10 e la percentuale di bucket vuoti è superiore al 10%, il numero di duplicati dei dati è tale per cui un indice hash potrebbe non essere il tipo più appropriato.  
  
### <a name="demonstration-of-chains-and-empty-buckets"></a>Dimostrazione delle catene e dei bucket vuoti  
  
Il blocco di codice T-SQL seguente offre un modo semplice per testare un `SELECT * FROM sys.dm_db_xtp_hash_index_stats;`. Il blocco di codice viene completato in 1 minuto. Di seguito sono riportate le fasi del blocco di codice seguente:  
  
1. Crea una tabella ottimizzata per la memoria con alcuni indici hash.  
2. Popola la tabella con migliaia di righe.  
    a. Viene usato un operatore modulo per configurare il tasso di valori duplicati nella colonna StatusCode.  
    b. Il ciclo inserisce 262.144 righe in circa 1 minuto.  
3. Con PRINT viene stampato un messaggio che chiede di eseguire l'istruzione SELECT precedente da **sys.dm_db_xtp_hash_index_stats**.  

```sql
DROP TABLE IF EXISTS SalesOrder_Mem;  
go  
  
  
CREATE TABLE SalesOrder_Mem  
(  
  SalesOrderId   uniqueidentifier  NOT NULL  DEFAULT newid(),  
  OrderSequence  int               NOT NULL,  
  OrderDate      datetime2(3)      NOT NULL,  
  StatusCode     tinyint           NOT NULL,  
  
  PRIMARY KEY NONCLUSTERED  
      HASH (SalesOrderId)  WITH (BUCKET_COUNT = 262144),  
  
  INDEX ix_OrderSequence  
      HASH (OrderSequence) WITH (BUCKET_COUNT = 20000),  
  
  INDEX ix_StatusCode  
      HASH (StatusCode)    WITH (BUCKET_COUNT = 8),  
  
  INDEX ix_OrderDate       NONCLUSTERED (OrderDate DESC)  
)  
  WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
--------------------  
  
SET NOCOUNT ON;  
  
-- Same as PK bucket_count.  68 seconds to complete.  
DECLARE @i int = 262144;  
  
BEGIN TRANSACTION;  
  
WHILE @i > 0  
BEGIN  
  
  INSERT SalesOrder_Mem  
      (OrderSequence, OrderDate, StatusCode)  
    Values  
      (@i, GetUtcDate(), @i % 8);  -- Modulo technique.  
  
  SET @i -= 1;  
END  
COMMIT TRANSACTION;  
  
PRINT 'Next, you should query:  sys.dm_db_xtp_hash_index_stats .';  
go  
```  
  
Il ciclo `INSERT` precedente esegue queste operazioni:  
  
- Inserisce valori univoci per l'indice di chiave primaria e per *ix_OrderSequence*.  
- Inserisce un paio di centinaia di migliaia di righe che rappresentano solo 8 valori distinct per `StatusCode`. È quindi presente un tasso elevato di duplicazione di valori nell'indice *ix_StatusCode*.  
  
Per la risoluzione dei problemi quando il numero di bucket non è ottimale, esaminare il seguente output di SELECT da **sys.dm_db_xtp_hash_index_stats**. Per questi risultati abbiamo aggiunto `WHERE Object_Name(h.object_id) = 'SalesOrder_Mem'` all'istruzione SELECT copiata dalla sezione D.1.  
  
I risultati `SELECT` vengono visualizzati dopo il codice, artificialmente suddivisi in due tabelle di risultati più ristretti per una migliore visualizzazione.  
  
- Di seguito sono riportati i risultati per *bucket_count*.  
  
| IndexName | total_bucket_count | empty_bucket_count | EmptyBucketPercent |  
| :-------- | -----------------: | -----------------: | -----------------: |  
| ix_OrderSequence | 32768 | 13 | 0 |  
| ix_StatusCode | 8 | 4 | 50 |  
| PK_SalesOrd_B14003... | 262144 | 96525 | 36 |  
  
- Successivamente vengono riportati i risultati per *chain_length*.  
  
| IndexName | avg_chain_length | max_chain_length |  
| :-------- | ---------------: | ---------------: |  
| ix_OrderSequence | 8 | 26 |  
| ix_StatusCode | 65536 | 65536 |  
| PK_SalesOrd_B14003... | 1 | 8 |  
  
Interpretiamo la tabella dei risultati precedenti per i tre indici hash:  
  
*ix_StatusCode:*  
  
- Il 50% dei bucket è vuoto, una condizione positiva.  
- La lunghezza media della catena, però, è molto elevata (65536).  
  - Questo è indicativo di un tasso elevato di valori duplicati.  
  - Quindi, l'uso di un indice hash non è appropriato in questo caso. È più opportuno utilizzare in alternativa un indice non cluster.  
  
*ix_OrderSequence:*  
  
- Lo 0% dei bucket è vuoto, un valore troppo basso.  
- La lunghezza media della catena è 8, anche se tutti i valori dell'indice sono univoci.  
  - È quindi consigliabile aumentare il numero di bucket per avvicinare la lunghezza media della catena a 2 o 3.  
- Dato che la chiave di indice ha 262144 valori univoci, il numero di bucket deve essere pari ad almeno 262144.  
  - Se si prevede una crescita futura, il numero di bucket deve essere maggiore.  
  
*Indice di chiave primaria (PK_SalesOrd_...):*  
  
- Il 36% dei bucket è vuoto, una condizione positiva.  
- La lunghezza media della catena è pari a 1, un'altra condizione positiva. Non è necessario apportare alcuna modifica.  
  
### <a name="balancing-the-trade-off"></a>Soluzione di compromesso  
  
I carichi di lavoro OLTP si focalizzano su singole righe. Le scansioni complete delle tabelle non fanno in genere parte del percorso critico per le prestazioni per i carichi di lavoro OLTP. È pertanto necessario trovare un compromesso tra **quantità dell'utilizzo della memoria** e **prestazioni dei test di uguaglianza e delle operazioni di inserimento**.  
  
**Se l'utilizzo della memoria è l'aspetto più rilevante:**  
  
- Scegliere un numero di bucket simile al numero di record di chiave di indice.  
- Il numero di bucket non deve essere notevolmente inferiore al numero di valori di chiave di indice, in quanto ciò influisce sulla maggior parte delle operazioni DML nonché sul tempo necessario per il recupero del database dopo il riavvio del server.  
  
**Se le prestazioni dei test di uguaglianza sono l'aspetto più rilevante:**  
  
- Un numero di bucket superiore, due o tre volte maggiore rispetto al numero di valori di indice univoci, è appropriato. Un numero maggiore comporta:  
  - Maggiore rapidità di recupero quando si cerca uno specifico valore.  
  - Un utilizzo maggiore della memoria.  
  - Prolungamento del tempo necessario per l'analisi completa dell'indice hash.  
  

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Ulteriori informazioni  
 [Indici hash per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [Indice non cluster per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)  
