---
title: Stima della cardinalità (SQL Server) | Microsoft Docs
description: SQL Server Query Optimizer seleziona i piani di query con il minor costo di elaborazione stimato, che determina in base alle righe elaborate e a un modello di costo.
ms.custom: ''
ms.date: 02/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- cardinality estimator
- CE (cardinality estimator)
- estimating cardinality
ms.assetid: baa8a304-5713-4cfe-a699-345e819ce6df
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fcbc86a73fb6ac70fce78c381a5aac1f1accc108
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171783"
---
# <a name="cardinality-estimation-sql-server"></a>Stima della cardinalità (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è un'utilità di ottimizzazione basata sui costi. Questo significa che vengono selezionati i piani di query con il minor costo di elaborazione stimato per l'esecuzione. Query Optimizer determina il costo di esecuzione di un piano di query in base a due fattori principali:

- Numero totale di righe elaborate a ogni livello del piano di query, detto cardinalità del piano.
- Modello di costo dell'algoritmo determinato dagli operatori utilizzati nella query.

Il primo fattore, ovvero la cardinalità, viene utilizzato come parametro di input del secondo fattore, ovvero il modello di costo. Una migliore cardinalità comporta pertanto costi stimati migliori e, di conseguenza, piani di esecuzione più rapidi.

In SQL Server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la stima di cardinalità deriva principalmente da istogrammi creati al momento della creazione di indici o statistiche, in modo manuale o automatico. In alcuni casi, per determinare la cardinalità in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono inoltre utilizzate le informazioni sui vincoli e le riscritture logiche delle query.

Nei casi seguenti, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è possibile eseguire un controllo accurato delle cardinalità. Questo comporta calcoli dei costi non accurati che potrebbero portare a piani di query non ottimali. Evitando questi costrutti nelle query, è possibile migliorare le prestazioni. In alcuni casi, è possibile ricorrere a formulazioni di query alternative o ad altre misure, indicate di seguito:

- Query con predicati che utilizzano operatori di confronto tra colonne della stessa tabella.
- Query con predicati che utilizzano operatori e una delle seguenti condizioni è vera:
  - Non vi sono statistiche nelle colonne coinvolte a destra o a sinistra degli operatori.
  - La distribuzione dei valori nelle statistiche non è uniforme, ma la query cerca un set di valori estremamente selettivo. Questa situazione si verifica in particolare se l'operatore è diverso dall'operatore di uguaglianza (=).
  - Il predicato usa l'operatore di confronto diverso da (!=) o l'operatore logico `NOT`.
- Query che usano qualsiasi funzione predefinita di SQL Server o una funzione definita dall'utente a valori scalari il cui argomento non è un valore costante.
- Query che implicano il join di colonne attraverso operatori aritmetici o di concatenazione delle stringhe.
- Query che confrontano variabili i cui valori non sono noti al momento della compilazione e dell'ottimizzazione della query.

Questo articolo illustra come valutare e scegliere la migliore configurazione di stima della cardinalità per il sistema. Per la maggior parte dei sistemi è disponibile la stima della cardinalità più recente perché è la più accurata. La stima della cardinalità prevede il numero di righe che verranno probabilmente restituite dalla query. La stima della cardinalità è usata da Query Optimizer per generare il piano di query ottimale. Con stime più accurate, Query Optimizer è in genere in grado di produrre un piano di query migliore.  
  
Il sistema di applicazioni può includere una query importante il cui piano viene sostituito con un piano più lento a causa di modifiche della stima della cardinalità tra versioni diverse. Sono disponibili tecniche e strumenti per l'identificazione di una query che risulta più lenta a causa di problemi di stima della cardinalità. Inoltre, sono disponibili opzioni per la risoluzione del problema di prestazioni conseguente.
  
## <a name="versions-of-the-ce"></a>Versioni della stima della cardinalità

Nel 1998 è stato incluso un aggiornamento importante della stima di cardinalità in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, per cui il livello di compatibilità era 70. Questa versione del modello di stima di cardinalità si basa su quattro presupposti:

-  **Indipendenza:** si presuppone che le distribuzioni dei dati in colonne diverse siano indipendenti una dall'altra, a meno che siano disponibili e utilizzabili informazioni di correlazione.
-  **Uniformità:** i valori Distinct sono divisi uniformemente e hanno tutti la stessa frequenza. Più precisamente, all'interno di ogni intervallo dell'[istogramma](../../relational-databases/statistics/statistics.md#histogram), i valori distinct sono distribuiti uniformemente e hanno tutti la stessa frequenza. 
-  **Indipendenza (semplice):** gli utenti eseguono una query per i dati esistenti. Ad esempio, per un join di uguaglianza tra due tabelle, considerare la selettività di join <sup>1</sup> in ogni istogramma di input prima di creare un join di istogrammi e stimarne la selettività. 
-  **Inclusione:** per i predicati di filtro dove `Column = Constant`, si presuppone che la costante sia effettivamente esistente nella colonna associata. Se un intervallo dell'istogramma corrispondente non è vuoto, si presuppone che uno dei valori distinct dell'intervallo corrisponda al valore del predicato.

  <sup>1</sup> Numero di righe che soddisfa il predicato.

Gli aggiornamenti successivi sono iniziati con [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], con livelli di compatibilità 120 e superiori. Gli aggiornamenti della stima di cardinalità per i livelli 120 e superiori integrano presupposti e algoritmi aggiornati che funzionano bene nel data warehousing moderno e nei carichi di lavoro OLTP. Dopo i presupporti della stima di cardinalità per i livelli 70, con la stima di cardinalità per i livelli 120 sono stati modificati i presupposti del modello seguenti:

-  **Indipendenza** diventa **correlazione:** la combinazione dei diversi valori di colonna che non sono necessariamente indipendenti. Può assomigliare all'esecuzione di query sui dati più reali.
-  **Indipendenza semplice** diventa **indipendenza di base:** gli utenti possono eseguire query per dati che non esistono. Ad esempio, per un join di uguaglianza tra due tabelle, si usano gli istogrammi delle tabelle di base per stimare la selettività di join e considerare la selettività dei predicati.
  
**Livello di compatibilità:** è possibile assicurarsi che il database sia impostato su un determinato livello usando il codice [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente per [COMPATIBILITY_LEVEL](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

```sql  
SELECT ServerProperty('ProductVersion');  
GO  
  
ALTER DATABASE <yourDatabase>  
SET COMPATIBILITY_LEVEL = 130;  
GO  
  
SELECT d.name, d.compatibility_level  
FROM sys.databases AS d  
WHERE d.name = 'yourDatabase';  
GO  
```  
  
Per un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostato sul livello di compatibilità 120 o su un livello superiore, l'attivazione del [flag di traccia 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) impone al sistema di usare la stima di cardinalità per i livelli 70.  
  
**Stima di cardinalità legacy:** per un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostato sul livello di compatibilità 120 o superiore, è possibile attivare la stima di cardinalità per i livelli 70 a livello di database usando l'istruzione [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION 
SET LEGACY_CARDINALITY_ESTIMATION = ON;  
GO  
  
SELECT name, value  
FROM sys.database_scoped_configurations  
WHERE name = 'LEGACY_CARDINALITY_ESTIMATION';  
GO
```  
 
In alternativa, a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1, l'[hint per la query](../../t-sql/queries/hints-transact-sql-query.md#use_hint) `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')`.
 
 ```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01'
OPTION (USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION'));  
```
 
**Query Store:** a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], Query Store è uno strumento utile per esaminare le prestazioni delle query. In [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] in **Esplora oggetti** nel nodo del database viene visualizzato un nodo **Query Store** quando Query Store è abilitato.  
  
```sql  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE = ON;  
GO  
  
SELECT q.actual_state_desc AS [actual_state_desc_of_QueryStore],  
        q.desired_state_desc,  
        q.query_capture_mode_desc  
FROM sys.database_query_store_options AS q;  
GO  
  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE CLEAR;  
```  
  
> [!TIP] 
> Si consiglia di installare la versione più recente di [Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) e di aggiornarlo spesso.  

> [!IMPORTANT] 
> Verificare che Query Store sia configurato correttamente per il database e il carico di lavoro. Per altre informazioni, vedere [Best practices with Query Store](../../relational-databases/performance/best-practice-with-the-query-store.md) (Procedure consigliate per Query Store). 
  
Un'altra opzione per tenere traccia del processo relativo alle stime di cardinalità consiste nell'usare l'evento esteso denominato **query_optimizer_estimate_cardinality**. Il codice di esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seguente viene eseguito su [!INCLUDE[tsql](../../includes/tsql-md.md)]. Scrive un file con estensione xel in `C:\Temp\`. Il percorso è comunque modificabile. Quando si apre il file con estensione xel in [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], le informazioni dettagliate sono visualizzate in modo intuitivo.  
  
```sql  
DROP EVENT SESSION Test_the_CE_qoec_1 ON SERVER;  
go  
  
CREATE EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
ADD EVENT sqlserver.query_optimizer_estimate_cardinality  
    (  
        ACTION (sqlserver.sql_text)  
            WHERE (  
                sql_text LIKE '%yourTable%'  
                and sql_text LIKE '%SUM(%'  
            )  
    )  
ADD TARGET package0.asynchronous_file_target   
        (SET  
            filename = 'c:\temp\xe_qoec_1.xel',  
            metadatafile = 'c:\temp\xe_qoec_1.xem'  
        );  
GO  
  
ALTER EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
STATE = START;  --STOP;  
GO  
```  
  
Per informazioni sugli eventi estesi specifici per [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vedere [Eventi estesi nel database SQL](/azure/azure-sql/database/xevent-db-diff-from-svr).  
  
## <a name="steps-to-assess-the-ce-version"></a>Procedura per valutare la versione della stima di cardinalità  
  
Di seguito sono indicate alcune operazioni da eseguire per stabilire se una delle query più importanti presenta prestazioni inferiori con la stima di cardinalità più recente. Alcuni dei passaggi vengono completati eseguendo un esempio di codice presentato in una sezione precedente.  
  
1.  Aprire [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. Assicurarsi che il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia impostato sul massimo livello di compatibilità disponibile.  
  
2.  Seguire questa procedura preliminare:  
  
    1.  Aprire [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].  
  
    2.  Eseguire T-SQL per assicurarsi che il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia impostato sul massimo livello di compatibilità disponibile.  
  
    3.  Assicurarsi che la configurazione `LEGACY_CARDINALITY_ESTIMATION` del database sia impostata su OFF.  
  
    4.  Cancellare il contenuto di Query Store. Verificare che Query Store sia attivo.  
  
    5.  Eseguire l'istruzione: `SET NOCOUNT OFF;`  
  
3.  Eseguire l'istruzione: `SET STATISTICS XML ON;`  
  
4.  Eseguire la query importante.  
  
5.  Nel riquadro dei risultati, nella scheda **Messaggi** , notare il numero effettivo di righe interessate.  
  
6.  Nel riquadro dei risultati, nella scheda **Risultati** , fare doppio clic sulla cella che contiene le statistiche in formato XML. Viene visualizzato un piano di query grafico.  
  
7.  Fare clic con il pulsante destro del mouse nella prima casella del piano di query grafico e scegliere **Proprietà**.  
  
8.  Per un successivo confronto con una configurazione diversa, notare i valori per le proprietà seguenti:  
  
    -   **CardinalityEstimationModelVersion**.  
  
    -   **Numero stimato di righe**.  
  
    -   **Costo I/O stimato***Stimato* simili che riguardano le prestazioni effettive anziché le previsioni del numero di righe.  
  
    -   **Operazione logica** e **Operazione fisica**. *Parallelismo* è un valore adeguato.  
  
    -   **Modalità di esecuzione effettiva**. *Batch* è un valore adeguato, migliore di *Riga*.  
  
9. Confrontare il numero stimato di righe con il numero effettivo di righe. La stima di cardinalità è imprecisa del 1% (per eccesso o per difetto) o del 10%?  
  
10. Eseguire: `SET STATISTICS XML OFF;`  
  
11. Eseguire l'istruzione T-SQL per abbassare il livello di compatibilità del database di un livello (ad esempio da 130 a 120).  
  
12. Eseguire di nuovo tutti i passaggi non preliminari.  
  
13. Confrontare i valori delle proprietà della stima di cardinalità delle due esecuzioni.  
  
    - La percentuale di imprecisione con la stima di cardinalità più recente è inferiore rispetto a quella meno recente?  
  
14. Infine, confrontare i vari valori delle proprietà delle prestazioni delle due esecuzioni.  
  
    -   La query ha usato un piano diverso nelle due stime di cardinalità?  
  
    -   La query è risultata più lenta con la stima di cardinalità più recente?  
  
    -   A meno che la query non venga eseguita con prestazioni migliori e con un piano diverso con la stima di cardinalità precedente, quasi certamente è preferibile usare la stima di cardinalità più recente.  
  
    -   Se però la query viene eseguita con un piano più veloce con la stima di cardinalità precedente, è possibile forzare il sistema affinché usi il piano più veloce e ignori la stima di cardinalità. In questo modo si può usare la stima di cardinalità più recente per tutto, mantenendo il piano più veloce nel caso particolare.  
  
## <a name="how-to-activate-the-best-query-plan"></a>Come attivare il piano di query ottimale  
  
Si supponga che con la stima di cardinalità per i livelli 120 o superiori, venga generato un piano di query meno efficiente per la query. Si seguito sono riportate alcune opzioni per attivare il piano migliore:  
  
1. È possibile impostare il livello di compatibilità su un valore inferiore rispetto a quello più recente, per l'intero database.  
  
   - Ad esempio, impostando il livello di compatibilità per i livelli 110 o inferiori, si attiva la stima di cardinalità per i livelli 70, ma si rendono tutte le query soggette al modello di stima di cardinalità precedente.  
  
   - Impostando un livello di compatibilità inferiore viene mancare anche una serie di miglioramenti in Query Optimizer per le versioni più recenti.  
  
2. Per fare in modo che l'intero database usi la stima di cardinalità precedente, mantenendo gli altri miglioramenti in Query Optimizer, è possibile usare l'opzione di database `LEGACY_CARDINALITY_ESTIMATION`.   

3. Per fare in modo che una sola query usi la stima di cardinalità precedente, mantenendo gli altri miglioramenti in Query Optimizer, è possibile usare l'hint per la query `LEGACY_CARDINALITY_ESTIMATION`.  
  
Per il massimo controllo, è possibile *imporre* al sistema di usare il piano generato con la stima di cardinalità per i livelli 70 durante i test. Dopo avere *aggiunto* il piano preferito, è possibile impostare l'intero database per l'uso della compatibilità e della stima di cardinalità più recenti. L'opzione viene elaborata successivamente.  
  
### <a name="how-to-force-a-particular-query-plan"></a>Come forzare un piano di query specifico  
  
L'archivio query offre diversi modi per forzare l'uso di un determinato piano di query:  
  
- Eseguire **sp_query_store_force_plan**.  
  
- In [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] espandere il nodo **Query Store**, fare clic con il pulsante destro del mouse su **Top Resource Consuming Nodes** (Primi nodi per consumo di risorse) e quindi su **View Top Resource Consuming Nodes** (Visualizza primi nodi per consumo di risorse). Verranno visualizzati i pulsanti **Forza piano** e **Annulla forzatura piano**.  
  
Per altre informazioni sull'archivio query, vedere [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="examples-of-ce-improvements"></a>Esempi di miglioramenti della stima di cardinalità  
  
Questa sezione descrive query di esempio che usufruiscono dai miglioramenti implementati nella stima di cardinalità nelle versioni recenti. Si tratta di informazioni in background che non richiedono un'azione specifica da parte dell'utente.  
  
### <a name="example-a-ce-understands-maximum-value-might-be-higher-than-when-statistics-were-last-gathered"></a>Esempio A. La stima di cardinalità riconosce che il valore massimo potrebbe essere superiore rispetto al momento in cui sono state raccolte le statistiche  
  
Si supponga che le statistiche siano state raccolte per `OrderTable` in data `2016-04-30`, quando il valore massimo `OrderAddedDate` era `2016-04-30`. La stima di cardinalità per i livelli 120 (e successivi) comprende che le colonne in `OrderTable` con dati in ordine *crescente* potrebbero contenere valori maggiori rispetto al valore massimo registrato dalle statistiche. Questa consapevolezza migliora il piano di query per le istruzioni SELECT di [!INCLUDE[tsql](../../includes/tsql-md.md)] come la seguente.  
  
```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01';  
```  
  
### <a name="example-b-ce-understands-that-filtered-predicates-on-the-same-table-are-often-correlated"></a>Esempio B. La stima di cardinalità riconosce che i predicati filtrati nella stessa tabella sono spesso correlati  
  
Nell'istruzione SELECT seguente sono presenti predicati filtrati su `Model` e `ModelVariant`. Si comprende intuitivamente che quando `Model` è "Xbox" esiste la possibilità che `ModelVariant` sia "One" poiché Xbox ha una variante denominata One.  
  
A partire dalla stima di cardinalità per i livelli 120, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riconosce che potrebbe esserci una correlazione tra le due colonne nella stessa tabella, ovvero `Model` e `ModelVariant`. La stima di cardinalità prevede in modo più preciso quante righe verranno restituite dalla query e [Query Optimizer](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements) genera un piano più ottimale.  
  
```sql  
SELECT Model, Purchase_Price  
FROM dbo.Hardware  
WHERE Model = 'Xbox' AND  
      ModelVariant = 'One';  
```  
  
### <a name="example-c-ce-no-longer-assumes-any-correlation-between-filtered-predicates-from-different-tables"></a>Esempio C. La stima di cardinalità non presume più alcuna correlazione tra i predicati filtrati da tabelle diverse 
Una nuova ricerca estesa su carichi di lavoro moderni e dati di business effettivi rivelano che i filtri del predicato da tabelle diverse in genere non sono correlati tra loro. Nella query seguente la stima di cardinalità presuppone che non esista correlazione tra `s.type` e `r.date`. La stima di cardinalità riduce quindi la stima del numero di righe restituite.  
  
```sql  
SELECT s.ticket, s.customer, r.store  
FROM dbo.Sales AS s  
CROSS JOIN dbo.Returns AS r  
WHERE s.ticket = r.ticket AND  
      s.type = 'toy' AND  
      r.date = '2016-05-11';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [Ottimizzazione dei piani di query con la funzionalità di stima di cardinalità di SQL Server 2014](/previous-versions/dn673537(v=msdn.10))  
 [Query Hints](../../t-sql/queries/hints-transact-sql-query.md)    (Hint per la query)  
 [Hint per le query USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)       
 [Aggiornamento di database mediante l'Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
 [Monitoraggio delle prestazioni tramite Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)    
 [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md)