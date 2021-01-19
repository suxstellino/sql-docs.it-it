---
title: Requisiti di memoria - tabelle ottimizzate per la memoria
description: Informazioni sugli scenari di uso e gestione della memoria per le tabelle ottimizzate per la memoria in SQL Server, che richiedono memoria sufficiente per tutte le righe e gli indici.
ms.custom: seo-dt-2019
ms.date: 12/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 5c5cc1fc-1fdf-4562-9443-272ad9ab5ba8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c40cb726e20f015a46cd808200ebbe67d9c1dc10
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168398"
---
# <a name="estimate-memory-requirements-for-memory-optimized-tables"></a>Stimare i requisiti di memoria delle tabelle con ottimizzazione per la memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Le tabelle con ottimizzazione per la memoria richiedono memoria sufficiente per mantenere tutte le righe e tutti gli indici in memoria. Poiché la memoria è una risorsa limitata, è importante conoscere e gestire l'utilizzo di memoria nel sistema. Negli argomenti di questa sezione vengono illustrati gli scenari comuni di utilizzo e gestione della memoria.

Se si crea una nuova tabella ottimizzata per la memoria o si esegue la migrazione di una tabella basata su disco esistente a una tabella ottimizzata per la memoria [!INCLUDE[hek_2](../../includes/hek-2-md.md)], è importante disporre di un numero ragionevole di requisiti di memoria di ogni tabella per poter fornire memoria sufficiente al server. In questa sezione viene descritto come stimare la quantità di memoria necessaria per contenere i dati di una tabella ottimizzata per la memoria.  
  
Se si prende in considerazione la migrazione da tabelle basate su disco a tabelle ottimizzate per la memoria, prima di procedere con questo argomento, vedere [Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) per informazioni aggiuntive sulle tabelle più appropriate per la migrazione. Tutti gli argomenti disponibili in [Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md) offrono informazioni aggiuntive sulla migrazione da tabelle basate su disco a tabelle ottimizzate per la memoria. 
  
## <a name="basic-guidance-for-estimating-memory-requirements"></a>Materiale sussidiario di base per la stima dei requisiti di memoria

A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]non esiste alcun limite alle dimensioni delle tabelle ottimizzate per la memoria, ad eccezione della memoria disponibile.  In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] le dimensioni dei dati supportate sono pari a 256 GB per le tabelle SCHEMA_AND_DATA.

Le dimensioni di una tabella ottimizzata per la memoria corrispondono alle dimensioni dei dati più l'overhead per le intestazioni di riga. Durante la migrazione di una tabella basata su disco a una tabella ottimizzata per la memoria, le dimensioni della tabella ottimizzata per la memoria corrispondono approssimativamente alle dimensioni dell'indice o dell'heap cluster della tabella originale basata su disco.

Gli indici delle tabelle ottimizzate per la memoria tendono a essere più piccoli rispetto agli indici non cluster nelle tabelle basate su disco. Le dimensioni degli indici non cluster corrispondono a `[primary key size] * [row count]`. Le dimensioni degli indici hash corrispondono a `[bucket count] * 8 bytes`. 

Quando è presente un carico di lavoro attivo, è necessaria altra memoria per tenere conto del controllo delle versioni delle righe e di diverse operazioni. La quantità di memoria necessaria dipende dal carico di lavoro, ma per sicurezza è consigliabile iniziare con il doppio della dimensione prevista di indici e tabelle ottimizzate per la memoria e quindi determinare i requisiti di memoria nella pratica. L'overhead per il controllo delle versioni delle righe dipende sempre dalle caratteristiche del carico di lavoro, in particolare le transazioni con esecuzione prolungata aumentano l'overhead. Per la maggior parte dei carichi di lavoro con database di grandi dimensioni (ad esempio >100 GB), l'overhead tende a essere limitato (25% o meno).

  
## <a name="detailed-computation-of-memory-requirements"></a>Calcolo dettagliato dei requisiti di memoria 
  
- [Esempio di tabella ottimizzata per la memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_ExampleTable)  
  
- [Memoria per la tabella](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForTable)  
  
- [Memoria per gli indici](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_IndexMeemory)  
  
- [Memoria per il controllo delle versioni delle righe](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForRowVersions)  
  
- [Memoria per le variabili di tabella](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_TableVariables)  
  
- [Memoria in caso di aumento delle dimensioni](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForGrowth)  
  
###  <a name="example-memory-optimized-table"></a><a name="bkmk_ExampleTable"></a> Esempio di tabella ottimizzata per la memoria  

Si consideri il seguente schema di tabella ottimizzata per la memoria:
  
```sql  
CREATE TABLE t_hk
(  
  col1 int NOT NULL  PRIMARY KEY NONCLUSTERED,  

  col2 int NOT NULL  INDEX t1c2_index   
      HASH WITH (bucket_count = 5000000),  

  col3 int NOT NULL  INDEX t1c3_index   
      HASH WITH (bucket_count = 5000000),  

  col4 int NOT NULL  INDEX t1c4_index   
      HASH WITH (bucket_count = 5000000),  

  col5 int NOT NULL  INDEX t1c5_index NONCLUSTERED,  

  col6 char (50) NOT NULL,  
  col7 char (50) NOT NULL,   
  col8 char (30) NOT NULL,   
  col9 char (50) NOT NULL  

)   WITH (memory_optimized = on)  ;
GO  
```  

Utilizzando questo schema sarà possibile stabilire la memoria minima necessaria per questa tabella ottimizzata per la memoria.  
  
###  <a name="memory-for-the-table"></a><a name="bkmk_MemoryForTable"></a> Memoria per la tabella  

La riga di una tabella ottimizzata per la memoria è costituita da tre parti:
  
- **Timestamp**   
    Intestazione di riga/timestamp = 24 byte.  
  
- **Puntatori dell'indice**   
    Per ogni indice hash nella tabella, a ogni riga è associato un puntatore all'indirizzo di 8 byte alla riga successiva nell'indice.  Poiché sono presenti 4 indici, tramite ogni riga vengono allocati 32 byte per i puntatori dell'indice (un puntatore di 8 byte per ogni indice).  
  
- **Dati**   
    Le dimensioni della parte di dati della riga vengono determinate sommando le dimensioni di tipo per ogni colonna di dati.  Nella tabella di esempio sono contenuti cinque Integer a 4 byte, tre colonne di tipo carattere di 50 byte e una colonna di tipo carattere di 30 byte.  Pertanto la parte di dati di ogni riga è 4 + 4 + 4 + 4 + 4 + 50 + 50 + 30 + 50, vale a dire 200 byte.  
  
Di seguito è riportato un calcolo di dimensioni per 5.000.000 (5 milioni) di righe in una tabella ottimizzata per la memoria. La memoria totale utilizzata dalle righe di dati viene stimata come segue:  
  
#### <a name="memory-for-the-tables-rows"></a>Memoria per le righe della tabella  
  
Dai calcoli sopra riportati, le dimensioni di ogni riga della tabella ottimizzata per la memoria sono pari a 24 + 32 + 200, vale a dire 256 byte.  Dal momento che sono presenti 5 milioni di righe, per la tabella verranno utilizzati 5.000.000 * 256 byte, vale a dire 1.280.000.000 di byte, circa 1,28 GB.  
  
###  <a name="memory-for-indexes"></a><a name="bkmk_IndexMeemory"></a> Memoria per gli indici  

#### <a name="memory-for-each-hash-index"></a>Memoria per ogni indice hash  
  
Ogni indice hash è una matrice di hash di puntatori all'indirizzo di 8 byte.  Le dimensioni della matrice vengono determinata meglio in base al numero di valori di indice univoci per l'indice in questione, ad esempio il numero di valori Col2 univoci è un buon punto di partenza per le dimensioni della matrice per t1c2_index. Una matrice di hash eccessiva comporta uno spreco di memoria.  Una matrice di hash di piccole dimensioni determina un rallentamento delle prestazioni in quanto vi saranno troppe collisioni per i valori di indice con hashing nello stesso indice.  
  
Tramite gli indici hash è possibile ottenere ricerche di uguaglianza estremamente veloci, ad esempio:  
  
```sql  
SELECT * FROM t_hk  
   WHERE Col2 = 3;
```  
  
Gli indici non cluster sono più veloci per le ricerche in intervalli, ad esempio:  
  
```sql  
SELECT * FROM t_hk  
   WHERE Col2 >= 3;
```  
  
Se si esegue la migrazione di una tabella basata su disco, è possibile utilizzare quando riportato di seguito per determinare il numero di valori univoci per l'indice t1c2_index.  
  
```sql
SELECT COUNT(DISTINCT [Col2])  
  FROM t_hk;
```  
  
Se si crea una nuova tabella, sarà necessario stimare le dimensioni della matrice o raccogliere i dati dal test prima di eseguire la distribuzione.  
  
Per informazioni sul funzionamento degli indici hash in tabelle ottimizzate per la memoria [!INCLUDE[hek_2](../../includes/hek-2-md.md)], vedere [Indici hash](/previous-versions/sql/sql-server-2016/dn133190(v=sql.130)).  
  
#### <a name="setting-the-hash-index-array-size"></a>Impostazione delle dimensioni della matrice dell'indice hash  
  
Le dimensioni della matrice di hash vengono impostate tramite `(bucket_count= value)` dove `value` è un intero maggiore di zero. Se `value` non è una potenza di 2, il numero effettivo di bucket_count viene arrotondato per eccesso alla potenza di 2 successiva più vicina.  Nella tabella di esempio, (bucket_count = 5000000), poiché 5.000.000 non è una potenza di 2, il numero effettivo di bucket viene arrotondato per eccesso a 8.388.608 (2^23).  È necessario utilizzare questo numero, non 5.000.000, quando si calcola la memoria necessaria per la matrice di hash.  
  
Pertanto, nell'esempio, la memoria necessaria per ogni matrice di hash è:  
  
8\.388.608 * 8 = 2^23 \* 8 = 2^23 \* 2^3 = 2^26 = 67.108.864 o circa 64 MB.  
  
Poiché vi sono tre indici hash, la memoria necessaria per gli indici hash è 3 * 64 MB = 192 MB.  
  
#### <a name="memory-for-nonclustered-indexes"></a>Memoria per gli indici non cluster  
  
Gli indici non cluster vengono implementati come oggetti BTree con i nodi interni contenenti il valore di indice e i puntatori ai nodi successivi.  Nei nodi foglia sono inclusi il valore di indice e un puntatore alla riga di tabella in memoria.  
  
A differenza degli indici hash, gli indici non cluster non hanno dimensioni fisse per il bucket. Le dimensioni dell'indice aumentano e si riducono dinamicamente in base ai dati.  
  
La memoria necessaria per gli indici non cluster può essere calcolata come indicato di seguito:  
  
- **Memoria allocata ai nodi non foglia**   
    Per una configurazione tipica, la memoria allocata ai nodi non foglia è una percentuale minima della memoria globale utilizzata dall'indice, che per le sue dimensioni contenute può essere sicuramente ignorata.  
  
- **Memoria per i nodi foglia**   
    I nodi foglia hanno una riga per ogni chiave univoca della tabella che punta alle righe di dati con questa chiave univoca.  Se si hanno più righe con la stessa chiave, ovvero si ha un indice non cluster non univoco, c'è una sola riga nel nodo foglia dell'indice che punta a una delle righe, con le altre righe collegate tra loro.  Pertanto, la memoria totale necessaria può essere approssimata nel modo seguente:
  - memoryForNonClusteredIndex = (pointerSize + sum (keyColumnDataTypeSizes) * rowsWithUniqueKeys  
  
 Gli indici non cluster rappresentano la soluzione migliore in caso di ricerche in intervalli, come esemplificato dalla query seguente:  
  
```sql  
SELECT * FROM t_hk  
   WHERE c2 > 5;  
```  
  
###  <a name="memory-for-row-versioning"></a><a name="bkmk_MemoryForRowVersions"></a> Memoria per il controllo delle versioni delle righe

Per evitare blocchi, tramite OLTP in memoria viene utilizzata la concorrenza ottimistica durante l'aggiornamento o l'eliminazione di righe. Pertanto, quando una riga viene aggiornata, viene creata una versione aggiuntiva della riga. Inoltre, le eliminazioni sono logiche: la riga esistente viene contrassegnata come eliminata, ma non viene rimossa immediatamente. Il sistema mantiene disponibili le versioni precedenti delle righe (comprese quelle eliminate) fino al termine dell'esecuzione di tutte le transazioni che potrebbero usare una versione. 
  
Poiché è possibile che vi sia un numero di righe aggiuntive in memoria in qualsiasi momento in attesa del ciclo di Garbage Collection per rilasciare la memoria, è necessario disporre di memoria sufficiente per contenere queste righe aggiuntive.  
  
Il numero di righe aggiuntive può essere stimato calcolando il numero massimo di aggiornamenti ed eliminazioni di righe al secondo, quindi moltiplicando il risultato per il numero di secondi impiegati dalla transazione più lunga (almeno 1).  
  
Il valore viene quindi moltiplicato per le dimensioni della riga per ottenere il numero di byte necessari per il controllo delle versioni delle righe.  
  
`rowVersions = durationOfLongestTransctoinInSeconds * peakNumberOfRowUpdatesOrDeletesPerSecond`  
  
I requisiti di memoria per righe non aggiornate vengono quindi stimati moltiplicando il numero di righe non aggiornate per le dimensioni di una riga di tabella ottimizzata per la memoria. Vedere [Memoria per la tabella](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md#bkmk_MemoryForTable) precedentemente in questo argomento.  
  
`memoryForRowVersions = rowVersions * rowSize`  
  
###  <a name="memory-for-table-variables"></a><a name="bkmk_TableVariables"></a> Memoria per le variabili di tabella
  
La memoria utilizzata per una variabile di tabella viene rilasciata solo quando la variabile di tabella abbandona l'ambito. Le righe eliminate, incluse quelle eliminate come parte di un aggiornamento, da una variabile di tabella non vengono sottoposte a Garbage Collection. Finché la variabile di tabella non abbandona l'ambito, la memoria non viene rilasciata.  
  
Le variabili di tabella definite in un batch SQL di grandi dimensioni, a differenza di un ambito di procedura, utilizzate in molte transazioni, possono richiedere una grande quantità di memoria. Poiché non vengono sottoposte a Garbage Collection, le righe eliminate in una variabile di tabella possono utilizzare una grande quantità di memoria e influire negativamente sulle prestazioni poiché le operazioni di lettura devono eseguire l'analisi delle righe eliminate.  
  
###  <a name="memory-for-growth"></a><a name="bkmk_MemoryForGrowth"></a> Memoria in caso di aumento delle dimensioni

Con i calcoli sopra riportati vengono stimati i requisiti di memoria della tabella attuale. Oltre a questa memoria, è necessario stimare la crescita della tabella e fornire una memoria sufficiente per gestire questa crescita.  Ad esempio, se si prevede una crescita del 10%, sarà necessario moltiplicare i risultati precedenti per 1,1 per ottenere la memoria totale necessaria per la tabella.  
  
## <a name="see-also"></a>Vedere anche

[Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)