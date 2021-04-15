---
title: Elaborazione delle query per le tabelle con ottimizzazione per la memoria
description: Informazioni sull'elaborazione delle query per le tabelle ottimizzate per la memoria e le stored procedure compilate in modo nativo in OLTP in memoria in SQL Server.
ms.custom: seo-dt-2019
ms.date: 05/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 065296fe-6711-4837-965e-252ef6c13a0f
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c7b672ca80ffcd9a749e1d3144239cf7f4e344e4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492373"
---
# <a name="a-guide-to-query-processing-for-memory-optimized-tables"></a>Guida all'elaborazione delle query per le tabelle con ottimizzazione per la memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Con OLTP in memoria sono state introdotte le tabelle ottimizzate per la memoria e le stored procedure compilate in modo nativo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questo articolo viene fornita una panoramica sull'elaborazione delle query per le tabelle ottimizzate per la memoria e le stored procedure compilate in modo nativo.  
  
 Nel documento viene illustrato come compilare ed eseguire query sulle tabelle ottimizzate per la memoria, tra cui:  
  
-   La pipeline di elaborazione delle query in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le tabelle basate su disco.  
  
-   Ottimizzazione query; il ruolo delle statistiche sulle tabelle ottimizzate per la memoria e linee guida per la risoluzione dei problemi relativi a piani di query errati.  
  
-   L'uso del codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato per accedere alle tabelle ottimizzate per la memoria.  
  
-   Considerazioni sull'ottimizzazione query per l'accesso alle tabelle ottimizzate per la memoria.  
  
-   Compilazione ed elaborazione di stored procedure compilate in modo nativo.  
  
-   Statistiche utilizzate per la stima dei costi in Query Optimizer.  
  
-   Modi per correggere piani di query errati.  
  
## <a name="example-query"></a>Query di esempio  
 L'esempio seguente verrà utilizzato per illustrare i concetti di elaborazione delle query descritti in questo articolo.  
  
 Vengono considerate due tabelle, Customer e Order. Il seguente script di [!INCLUDE[tsql](../../includes/tsql-md.md)] contiene le definizioni di queste due tabelle e gli indici associati, nel formato basato su disco (tradizionale):  
  
```sql  
CREATE TABLE dbo.[Customer] (  
  CustomerID nchar (5) NOT NULL PRIMARY KEY,  
  ContactName nvarchar (30) NOT NULL   
)  
GO  
  
CREATE TABLE dbo.[Order] (  
  OrderID int NOT NULL PRIMARY KEY,  
  CustomerID nchar (5) NOT NULL,  
  OrderDate date NOT NULL  
)  
GO  
CREATE INDEX IX_CustomerID ON dbo.[Order](CustomerID)  
GO  
CREATE INDEX IX_OrderDate ON dbo.[Order](OrderDate)  
GO  
```  
  
 Per la creazione dei piani di query illustrati in questo articolo, le due tabelle sono state popolate con dati del database di esempio Northwind, che è possibile scaricare da [Northwind and pubs Sample Databases for SQL Server 2000](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)(Database di esempio Northwind e pubs per SQL Server 2000).  
  
 Si consideri la query seguente, che crea un join tra le tabelle Customer e Order e restituisce l'ID dell'ordine e le informazioni sul cliente associato:  
  
```sql  
SELECT o.OrderID, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
 Il piano di esecuzione stimato visualizzato in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è illustrato di seguito:  
  
 ![Piano di query per il join di tabelle basate su disco.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-1.png "Piano di query per il join di tabelle basate su disco.")  
Piano di query per il join di tabelle basate su disco.  
  
 Informazioni su questo piano di query:  
  
-   Le righe della tabella Customer vengono recuperate dall'indice cluster, che è la struttura dei dati primaria e contiene i dati completi della tabella.  
  
-   I dati della tabella Order vengono recuperati usando l'indice non cluster della colonna CustomerID. L'indice contiene sia la colonna CustomerID, utilizzata per il join, sia la colonna chiave primaria OrderID, che viene restituita all'utente. La restituzione di colonne aggiuntive dalla tabella Order richiederebbe ricerche nell'indice cluster della tabella stessa.  
  
-   L'operatore logico **Inner Join** viene implementato dall'operatore fisico **Merge join**. Gli altri tipi di join fisico sono **Cicli annidati** e **Hash join**. L'operatore **Merge Join** consente di sfruttare il fatto che entrambi gli indici sono ordinati in base alla colonna di join CustomerID.  
  
 Si consideri una leggera variazione in questa query, con la restituzione di tutte le righe della tabella Order e non solo di OrderID, come illustrato di seguito:  
  
```sql  
SELECT o.*, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
 Il piano stimato per la query è il seguente:  
  
 ![Piano di query per un hash join di tabelle basate su disco.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-2.png "Piano di query per un hash join di tabelle basate su disco.")  
Piano di query per un hash join di tabelle basate su disco.  
  
 In questa query le righe della tabella Order vengono recuperate utilizzando l'indice cluster. L'operatore fisico **Hash Match** viene ora usato per l'operatore **Inner Join**. L'indice cluster di Order non è ordinato in base a CustomerID, quindi per un operatore **Merge Join** sarebbe necessario un operatore di ordinamento, che influirebbe sulle prestazioni. Si noti il costo relativo dell'operatore **Hash Match** (75%) rispetto al costo dell'operatore **Merge Join** nell'esempio precedente (46%). In Query Optimizer l'operatore **Hash Match** è stato preso in considerazione anche nell'esempio precedente, con la conclusione, tuttavia, che l'operatore **Merge Join** avrebbe offerto prestazioni migliori.  
  
## <a name="ssnoversion-query-processing-for-disk-based-tables"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Elaborazione delle query per tabelle basate su disco  
 Nel diagramma seguente viene illustrato il flusso di elaborazione delle query ad hoc in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
 ![Pipeline di elaborazione delle query di SQL Server.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-3.png "Pipeline di elaborazione delle query di SQL Server.")  
Pipeline di elaborazione delle query di SQL Server.  
  
 In questo scenario:  
  
1.  L'utente esegue una query.  
  
2.  Il parser e il normalizzatore creano un albero della query con operatori logici basati sul testo [!INCLUDE[tsql](../../includes/tsql-md.md)] inviato dall'utente.  
  
3.  Query Optimizer crea un piano di query ottimizzato che contiene operatori fisici, ad esempio join a cicli annidati. Dopo l'ottimizzazione, il piano può essere archiviato nella cache dei piani. Questo passaggio viene ignorato se la cache dei piani già contiene un piano per la query.  
  
4.  Un'interpretazione del piano di query viene elaborato dal motore di esecuzione delle query.  
  
5.  Per ogni operatore Index Seek, Index Scan e Table Scan, il motore di esecuzione richiede righe delle rispettive strutture di indice e di tabella ad Access Methods.  
  
6.  Access Methods recupera le righe dalle pagine dell'indice e dei dati nel pool di buffer e carica le pagine dal disco nel pool di buffer in base alle esigenze.  

 Per la prima query di esempio, il motore di esecuzione richiede ad Access Methods le righe dell'indice cluster di Customer e dell'indice non cluster di Order. Access Methods attraversa le strutture di indice ad albero B per recuperare le righe richieste. In questo caso vengono recuperate tutte le righe poiché il piano richiede le analisi complete degli indici.  
  
## <a name="interpreted-tsql-access-to-memory-optimized-tables"></a>Accesso del codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato alle tabelle con ottimizzazione per la memoria  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] ai batch ad hoc e alle stored procedure si fa riferimento anche con l'espressione " [!INCLUDE[tsql](../../includes/tsql-md.md)]interpretato". L'interpretazione si riferisce al fatto che ogni operatore nel piano di query viene interpretato dal motore di esecuzione delle query. Il motore di esecuzione legge l'operatore e i relativi parametri ed esegue l'operazione.  
  
 Il codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato può essere utilizzato per accedere sia a tabelle ottimizzate per la memoria che a tabelle basate su disco. Nella figura seguente viene illustrata l'elaborazione delle query per l'accesso del codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato alle tabelle ottimizzate per la memoria:  
  
 ![Pipeline di elaborazione query per tsql interpretato.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-4.png "Pipeline di elaborazione delle query per tsql interpretato.")  
Pipeline di elaborazione delle query per l'accesso del codice Transact-SQL interpretato alle tabelle ottimizzate per la memoria.  
  
 Come illustrato nella figura, la pipeline di elaborazione delle query rimane per lo più invariata:  
  
-   Il parser e il normalizzatore creano l'albero della query.  
  
-   Query Optimizer crea il piano di esecuzione.  
  
-   Il piano di esecuzione viene interpretato dal motore di esecuzione delle query.  
  
 La differenza principale rispetto alla pipeline tradizionale di elaborazione delle query (figura 2) sta nel fatto che le righe delle tabelle ottimizzate per la memoria non vengono recuperate dal pool di buffer tramite Access Methods. Al contrario, le righe vengono recuperate dalle strutture dei dati in memoria tramite il motore di OLTP in memoria. Le differenze nelle strutture dei dati provocano in alcuni casi la selezione di piani diversi da parte di Query Optimizer, come illustrato nell'esempio seguente.  
  
 Lo script [!INCLUDE[tsql](../../includes/tsql-md.md)] riportato di seguito contiene versioni ottimizzate per la memoria delle tabelle Order e Customer, con indici hash:  
  
```sql  
CREATE TABLE dbo.[Customer] (  
  CustomerID nchar (5) NOT NULL PRIMARY KEY NONCLUSTERED,  
  ContactName nvarchar (30) NOT NULL   
) WITH (MEMORY_OPTIMIZED=ON)  
GO  
  
CREATE TABLE dbo.[Order] (  
  OrderID int NOT NULL PRIMARY KEY NONCLUSTERED,  
  CustomerID nchar (5) NOT NULL INDEX IX_CustomerID HASH(CustomerID) WITH (BUCKET_COUNT=100000),  
  OrderDate date NOT NULL INDEX IX_OrderDate HASH(OrderDate) WITH (BUCKET_COUNT=100000)  
) WITH (MEMORY_OPTIMIZED=ON)  
GO  
```  
  
 Si consideri la stessa query eseguita su tabelle ottimizzate per la memoria:  
  
```sql  
SELECT o.OrderID, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
 Il piano stimato è il seguente:  
  
 ![Piano di query per il join di tabelle con ottimizzazione per la memoria.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-5.png "Piano di query per il join di tabelle ottimizzate per la memoria.")  
Piano di query per il join di tabelle ottimizzate per la memoria.  
  
 Osservare le seguenti differenze del piano per la stessa query su tabelle basate su disco (figura 1):  
  
-   Questo piano contiene un'analisi di tabella anziché un'analisi di indice cluster per la tabella Customer:  
  
    -   La definizione della tabella non include un indice cluster.  
  
    -   Gli indici cluster non sono supportati con le tabelle ottimizzate per la memoria. Ogni tabella ottimizzata per la memoria deve invece disporre di almeno un indice non cluster e tutti gli indici delle tabelle ottimizzate per la memoria possono accedere in modo efficace a tutte le colonne della tabella senza che sia necessario archiviare tali colonne nell'indice o fare riferimento a un indice cluster.  
  
-   Questo piano contiene un **Hash Match** anziché un **Merge Join**. Sia gli indici della tabella Order che quelli della tabella Customer sono indici hash, quindi non ordinati. Un **Merge Join** richiederebbe un operatore di ordinamento, che ridurrebbe le prestazioni.  
  
## <a name="natively-compiled-stored-procedures"></a>stored procedure compilate in modo nativo  
 Le stored procedure compilate in modo nativo sono stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] compilate nel codice macchina, piuttosto che interpretate dal motore di esecuzione delle query. Lo script seguente crea una stored procedure compilata in modo nativo che esegue la query di esempio (della sezione Query di esempio).  
  
```sql  
CREATE PROCEDURE usp_SampleJoin  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH   
(  TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
  LANGUAGE = 'english')  
  
  SELECT o.OrderID, c.CustomerID, c.ContactName   
FROM dbo.[Order] o INNER JOIN dbo.[Customer] c   
  ON c.CustomerID = o.CustomerID  
  
END  
```  
  
 Le stored procedure compilate in modo nativo vengono compilate al momento della creazione, mentre le stored procedure interpretate vengono compilate alla prima esecuzione (una parte della compilazione, specificamente l'analisi e la normalizzazione, avviene al momento della creazione; tuttavia, per le stored procedure interpretate, l'ottimizzazione dei piani di query ha luogo alla prima esecuzione). La logica di ricompilazione è simile. Le stored procedure compilate in modo nativo vengono ricompilate alla prima esecuzione della procedura se il server viene riavviato. Le stored procedure interpretate vengono ricompilate se il piano non si trova più nella cache dei piani. Nella tabella seguente vengono riepilogati i casi di compilazione e di ricompilazione per le stored procedure compilate in modo nativo e interpretate:  
  
|Tipo di compilazione|Compilate in modo nativo|Accesso del codice|  
|-|-----------------------|-----------------|  
|Compilazione iniziale|Al momento della creazione.|Alla prima esecuzione.|  
|Ricompilazione automatica|Alla prima esecuzione della procedura dopo il riavvio del database o del server.|Al riavvio del server. In alternativa, eliminazione dalla cache dei piani, in genere in base alle modifiche di schema o di statistiche o a utilizzo elevato di memoria.|  
|Ricompilazione manuale|Usare **sp_recompile**.|Usare **sp_recompile**. È possibile eliminare manualmente il piano dalla cache, ad esempio tramite DBCC FREEPROCCACHE. È inoltre possibile creare la stored procedure specificando WITH RECOMPILE affinché venga ricompilata a ogni esecuzione.|  
  
### <a name="compilation-and-query-processing"></a>Compilazione ed elaborazione delle query  
 Nel diagramma seguente viene illustrato il processo di compilazione per le stored procedure compilate in modo nativo:  
  
 ![Compilazione nativa delle stored procedure.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-6.png "Compilazione nativa delle stored procedure.")  
Compilazione nativa delle stored procedure.  
  
 Il processo è il seguente:  
  
1.  L'utente esegue un'istruzione **CREATE PROCEDURE** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Il parser e il normalizzatore creano il flusso di elaborazione per la stored procedure, oltre ad alberi per le query [!INCLUDE[tsql](../../includes/tsql-md.md)] nella stored procedure.  
  
3.  Query Optimizer crea piani di esecuzione ottimizzati per tutte le query nella stored procedure.  
  
4.  Il compilatore di OLTP in memoria acquisisce il flusso di elaborazione con i piani di query ottimizzati incorporati e genera una DLL contenente il codice macchina per l'esecuzione della stored procedure.  
  
5.  La DLL generata viene caricata in memoria.  
  
 La chiamata di una stored procedure compilata in modo nativo viene convertita in chiamata a una funzione nella DLL.  
  
 ![Esecuzione di stored procedure compilate in modo nativo.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-7.png "Esecuzione di stored procedure compilate in modo nativo.")  
Esecuzione di stored procedure compilate in modo nativo.  
  
 La chiamata di una stored procedure compilata in modo nativo viene descritta nel modo riportato di seguito.  
  
1.  L'utente esegue un'istruzione **EXEC** _usp_myproc_.  
  
2.  Il parser estrae il nome e i parametri della stored procedure.  
  
     Se l'istruzione è stata preparata, ad esempio usando **sp_prep_exec**, non è necessario che il parser estragga il nome e i parametri della procedura in fase di esecuzione.  
  
3.  Il runtime di OLTP in memoria individua il punto di ingresso della DLL per la stored procedure.  
  
4.  Viene eseguito il codice della macchina nella DLL e i risultati vengono restituiti al client.  
  
 **Sniffing dei parametri**  
  
 Le stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretate vengono compilate alla prima esecuzione, contrariamente alle stored procedure compilate in modo nativo che vengono compilate al momento della creazione. Quando le stored procedure interpretate vengono compilate al momento della chiamata, i valori dei parametri forniti per la chiamata vengono utilizzati da Query Optimizer durante la generazione del piano di esecuzione. Questo utilizzo dei parametri durante la compilazione viene chiamato sniffing dei parametri.  
  
 Lo sniffing dei parametri non viene utilizzato per la compilazione delle stored procedure compilate in modo nativo. Tutti i parametri della stored procedure vengono considerati con valore UNKNOWN. Analogamente alle stored procedure interpretate, anche le stored procedure compilate in modo nativo supportano l'hint **OPTIMIZE FOR** . Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
### <a name="retrieving-a-query-execution-plan-for-natively-compiled-stored-procedures"></a>Recupero di un piano di esecuzione di query per le stored procedure compilate in modo nativo  
 Il piano di esecuzione di query per una stored procedure compilata in modo nativo può essere recuperato usando **Piano di esecuzione stimato** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]o tramite l'opzione SHOWPLAN_XML in [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ad esempio:  
  
```sql  
SET SHOWPLAN_XML ON  
GO  
EXEC dbo.usp_myproc  
GO  
SET SHOWPLAN_XML OFF  
GO  
```  
  
 Il piano di esecuzione generato da Query Optimizer consiste in un albero con gli operatori di query sui nodi e sulle foglie. La struttura dell'albero determina l'interazione (il flusso di righe da un operatore a un altro) tra operatori. Nella visualizzazione grafica di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], la direzione del flusso è da destra a sinistra. Ad esempio, il piano di query nella figura 1 contiene due operatori Index Scan che forniscono le righe a un operatore Merge Join. L'operatore Merge Join fornisce le righe a un operatore Select. L'operatore Select, infine, restituisce le righe al client.  
  
### <a name="query-operators-in-natively-compiled-stored-procedures"></a>Operatori di query nelle stored procedure compilate in modo nativo  
 Nella tabella seguente vengono riepilogati gli operatori di query supportati nelle stored procedure compilate in modo nativo:  
  
|Operatore|Query di esempio|Note|  
|--------------|------------------|-----------|  
|SELECT|`SELECT OrderID FROM dbo.[Order]`||  
|INSERT|`INSERT dbo.Customer VALUES ('abc', 'def')`||  
|UPDATE|`UPDATE dbo.Customer SET ContactName='ghi' WHERE CustomerID='abc'`||  
|DELETE|`DELETE dbo.Customer WHERE CustomerID='abc'`||  
|Compute Scalar|`SELECT OrderID+1 FROM dbo.[Order]`|Questo operatore viene utilizzato sia per le funzioni intrinseche che per le conversioni dei tipi. Non tutte le funzioni e conversioni dei tipi sono supportate nelle stored procedure compilate in modo nativo.|  
|Join a cicli annidati|`SELECT o.OrderID, c.CustomerID FROM dbo.[Order] o INNER JOIN dbo.[Customer] c`|Nested Loops è l'unico operatore di join supportato nelle stored procedure compilate in modo nativo. In tutti i piani che contengono join viene utilizzato l'operatore Nested Loops, anche se il piano per la stessa query eseguita come codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato contiene un hash join o un merge join.|  
|Ordina|`SELECT ContactName FROM dbo.Customer ORDER BY ContactName`||  
|TOP|`SELECT TOP 10 ContactName FROM dbo.Customer`||  
|Top-sort|`SELECT TOP 10 ContactName FROM dbo.Customer  ORDER BY ContactName`|L'espressione **TOP** (il numero di righe da restituire) non può superare le 8.000 righe. Meno se nella query sono presenti anche operatori di aggregazione e di join. I join e le aggregazioni in genere riducono il numero di righe da ordinare, rispetto al numero di righe delle tabelle di base.|  
|Stream Aggregate|`SELECT count(CustomerID) FROM dbo.Customer`|Si noti che l'operatore Hash Match non è supportato per l'aggregazione. Pertanto, in tutte le aggregazioni nelle stored procedure compilate in modo nativo viene utilizzato l'operatore Stream Aggregate, anche se il piano per la stessa query nel codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato utilizza l'operatore Hash Match.|  
  
## <a name="column-statistics-and-joins"></a>Statistiche di colonna e join  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene statistiche sui valori nelle colonne chiave di indice per facilitare la stima del costo di determinate operazioni, quali l'analisi e le ricerche sugli indici. Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è anche possibile creare statistiche sulle colonne chiave non di indice, se create in modo esplicito dall'utente o create da Query Optimizer in risposta a una query con un predicato. La principale metrica nella stima dei costi è il numero di righe elaborate da un singolo operatore. Si noti che, per le tabelle basate su disco, il numero di pagine a cui accede un operatore specifico è significativo nella stima dei costi. Tuttavia, poiché il conteggio delle pagine non è importante per le tabelle ottimizzate per la memoria (è sempre zero), questa descrizione è incentrata sul conteggio delle righe. La stima inizia con gli operatori Index Seek e Index Scan nel piano e viene quindi estesa agli altri operatori, quale l'operatore di join. Il numero stimato di righe da elaborare da parte di un operatore di join è basato sulla stima per gli operatori Index Seek e Index Scan sottostanti. Per l'accesso del codice [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretato alle tabelle ottimizzate per la memoria, è possibile osservare il piano di esecuzione effettivo per vedere la differenza tra il numero di righe stimato ed effettivo per gli operatori nel piano.  
  
Per l'esempio nella figura 1:  
  
- L'operatore Clustered Index Scan su Customer presenta un valore stimato di 91 ed effettivo di 91.  
- L'operatore Nonclustered Index Scan su CustomerID presenta un valore stimato di 830 ed effettivo di 830.  
- L'operatore Merge Join presenta un valore stimato di 815 ed effettivo di 830.  
  
Le stime per le analisi di indice sono accurate. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene il conteggio delle righe per le tabelle basate su disco. Le stime per le analisi complete di tabelle e indici sono sempre accurate. Anche la stima per il join è ragionevolmente accurata.  
  
Se le stime cambiano, cambiano anche le considerazioni sui costi per le diverse alternative di piano. Ad esempio, se uno dei lati del join presenta un conteggio stimato di una o solo poche righe, l'uso di join a cicli annidati è meno costoso. Si consideri la query seguente:  
  
```sql
SELECT o.OrderID, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
Dopo aver eliminato tutte le righe tranne una nella tabella `Customer`, viene generato il piano di query seguente:  
  
![Statistiche di colonna e join.](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-9.png "Statistiche di colonna e join.")  
  
Rispetto a questo piano di query:  
  
- L'operatore Hash Match è stato sostituito con un operatore fisico di join a cicli annidati.  
- L'analisi completa dell'indice su IX_CustomerID è stata sostituita con una ricerca nell'indice. In questo modo sono state analizzate 5 righe, anziché le 830 righe richieste per l'analisi completa dell'indice.  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle ottimizzate per la memoria](./sample-database-for-in-memory-oltp.md)  
  
