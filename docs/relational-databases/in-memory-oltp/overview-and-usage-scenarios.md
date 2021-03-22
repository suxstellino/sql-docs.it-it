---
title: Panoramica e scenari di utilizzo | Microsoft Docs
description: Informazioni su OLTP in memoria, una tecnologia presente in SQL Server e nei database SQL di Azure per l'elaborazione delle transazioni ottimizzata. Rassegna di esempi e risorse aggiuntive.
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 62c964c5-eae4-4cf1-9024-d5a19adbd652
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d84282b089b8695b0fa325e10e7462ce2e70e93a
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793115"
---
# <a name="overview-and-usage-scenarios"></a>Panoramica e scenari di utilizzo

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

OLTP in memoria è la principale tecnologia disponibile in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)] per ottimizzare le prestazioni di elaborazione delle transazioni, l'inserimento di dati, il caricamento di dati e gli scenari di dati temporanei. Questo articolo include una panoramica della tecnologia e descrive gli scenari di utilizzo per OLTP in memoria. Usare queste informazioni per determinare se OLTP in memoria è appropriato all'applicazione usata. L'articolo si conclude con un esempio che illustra gli oggetti OLTP in memoria, un riferimento a una dimostrazione sulle prestazioni e riferimenti a risorse che è possibile usare per i passaggi successivi.

Questo articolo descrive la tecnologia OLTP in memoria in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Il post di blog seguente include un'analisi approfondita dei vantaggi a livello di prestazioni e uso delle risorse in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]: [OLTP in memoria nel database SQL di Azure](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

## <a name="in-memory-oltp-overview"></a>Panoramica di OLTP in memoria

OLTP in memoria può fornire elevati miglioramenti delle prestazioni per i carichi di lavoro appropriati. Anche se i clienti in alcuni casi hanno potuto migliorare di 30 volte le prestazioni, il miglioramento che si ottiene dipende in realtà dal proprio carico di lavoro.

Da cosa dipende questo miglioramento delle prestazioni? In sostanza, OLTP in memoria migliora le prestazioni di elaborazione delle transazioni rendendo più efficiente l'accesso ai dati e l'esecuzione delle transazioni e rimuovendo la contesa latch/blocco tra le transazioni attualmente in esecuzione: non è veloce perché è in memoria; è veloce perché è ottimizzato per i dati in memoria. L'archiviazione dei dati, l'accesso e l'elaborazione degli algoritmi sono stati riprogettati interamente per sfruttare i miglioramenti più recenti di elaborazione in memoria e concorrenza elevata.

Ma solo perché i dati si trovano in memoria non significa che si perdono quando si verifica un errore. Per impostazione predefinita, tutte le transazioni sono completamente durevoli, ovvero si hanno le stesse garanzie di durabilità che si hanno per qualsiasi altra tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: durante il commit della transazione, tutte le modifiche vengono scritte nel log delle transazioni su disco. Se si verifica un errore in qualsiasi momento dopo il commit della transazione, i dati rimangono presenti quando il database torna online. OLTP in memoria funziona inoltre con tutte le funzionalità di disponibilità elevata e ripristino di emergenza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come Always On, backup e ripristino e così via.

Per sfruttare OLTP in memoria nel database, è possibile usare uno o più dei seguenti tipi di oggetti:

- Le *tabelle con ottimizzazione per la memoria* vengono usate per archiviare i dati utente. È possibile dichiarare una tabella ottimizzata per la memoria al momento della creazione.
- Le *tabelle non durevoli* vengono usate per i dati temporanei, per la memorizzazione nella cache o per un set di risultati intermedio (sostituendo le tabelle temporanee tradizionali). Una tabella non durevole è una tabella ottimizzata per la memoria che viene dichiarata con DURABILITY=SCHEMA_ONLY, vale a dire che le modifiche apportate a queste tabelle non comportano operazioni di I/O. Ciò evita l'utilizzo di risorse per le operazioni di I/O sui log per i casi in cui la durabilità non è un problema.
- I *tipi di tabella con ottimizzazione per la memoria* vengono usati per i parametri con valori di tabella, ovvero come set di risultati intermedi nelle stored procedure. Questi tipi possono essere usati al posto dei tipi di tabella tradizionali. Le variabili di tabella e i parametri con valori di tabella che vengono dichiarati usando un tipo di tabella ottimizzata per la memoria ereditano i vantaggi delle tabelle non durevoli ottimizzate per la memoria: accesso efficiente ai dati e nessuna operazione I/O.
- I *moduli T-SQL compilati in modo nativo* vengono usati per ridurre ulteriormente il tempo impiegato per una singola transazione riducendo i cicli di CPU necessari per elaborare le operazioni. È possibile dichiarare un modulo Transact-SQL in modo da essere compilato in modo nativo al momento della creazione. Attualmente, i moduli T-SQL che possono essere compilati in modo nativo sono i seguenti: stored procedure, trigger e funzioni scalari definite dall'utente.

OLTP in memoria è integrato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. E poiché il comportamento di questi oggetti è simile a quello delle relative controparti tradizionali, spesso è possibile ottenere un miglioramento delle prestazioni apportando solo modifiche minime al database e all'applicazione. Inoltre, nello stesso database è possibile avere sia le tabelle ottimizzate per la memoria che le tabelle tradizionali basati su disco ed eseguire le query in entrambi i tipi di tabella. Alla fine di questo articolo è disponibile uno script Transact-SQL che mostra un esempio per ognuno di questi tipi di oggetti.

## <a name="usage-scenarios-for-in-memory-oltp"></a>Scenari di utilizzo per OLTP in memoria

OLTP in memoria non è un pulsante magico per ottenere maggiore velocità e non è appropriato per tutti i carichi di lavoro. Ad esempio, le tabelle ottimizzate per la memoria non riducono l'utilizzo della CPU se la maggior parte delle query comporta aggregazioni per grandi intervalli di dati. Per questo scenario saranno utili gli indici columnstore.

Di seguito è riportato un elenco di scenari e modelli di applicazione in cui i clienti hanno ottenuto risultati positivi con OLTP in memoria.

### <a name="high-throughput-and-low-latency-transaction-processing"></a>Elaborazione di transazioni con velocità effettiva elevata e bassa latenza

È lo scenario principale per cui è stato creato OLTP in memoria: supporto di volumi elevati di transazioni con bassa latenza costante per le singole transazioni.

Scenari di carico di lavoro comuni sono: intermediazione di strumenti finanziari, scommesse sportive, giochi per dispositivi mobili e servizi pubblicitari. Un altro modello comune è quello di un "catalogo" letto e/o aggiornato di frequente. Un esempio è rappresentato dal caso in cui sono presenti file di grandi dimensioni, ognuno distribuito su diversi nodi del cluster, e la posizione di ogni partizione di ogni file viene catalogata in una tabella ottimizzata per la memoria.

#### <a name="implementation-considerations"></a>Considerazioni sull'implementazione

Usare le tabelle ottimizzate per la memoria per le tabelle delle transazioni principali, ad esempio le tabelle con transazioni più critiche per le prestazioni. Usare le stored procedure compilate in modo nativo per ottimizzare l'esecuzione della logica associata alla transazione aziendale. Quanto maggiore è la distribuzione della logica nelle stored procedure del database, tanto maggiore sarà il vantaggio che si ottiene dall'uso di OLTP in memoria.

Per iniziare a usare questo approccio in un'applicazione esistente:

1. Usare il [report di analisi delle prestazioni delle transazioni](determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) per identificare gli oggetti di cui eseguire la migrazione
2. Usare gli advisor [ottimizzazione per la memoria](memory-optimization-advisor.md) e [compilazione nativa](native-compilation-advisor.md) per facilitare la migrazione.

#### <a name="customer-case-studies"></a>Case study dei clienti

- CMC Markets sfrutta OLTP in memoria in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] per ottenere una bassa latenza costante: [Poiché un'attesa di un secondo è troppo lunga, la società di servizi finanziari sta aggiornando il software di trading.](https://customers.microsoft.com/story/because-a-second-is-too-long-to-wait-this-financial-services-firm-is-updating-its-trading-software)
- Derivco sfrutta OLTP in memoria in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] per supportare una maggiore velocità effettiva e gestire i picchi del carico di lavoro: [Quando una società di gioco online non vuole rischiare il proprio futuro, scommette su [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].](https://customers.microsoft.com/story/when-an-online-gaming-company-doesnt-want-to-risk-its-future-it-bets-on-sql-server-2016)

### <a name="data-ingestion-including-iot-internet-of-things"></a>Inserimento di dati, tra cui IoT (Internet delle cose)

OLTP in memoria è consigliato per inserire contemporaneamente grandi volumi di dati da molte origini diverse. Ed è spesso utile inserire dati in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rispetto ad altre destinazioni, perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] velocizza l'esecuzione di query sui dati consentendo di ottenere informazioni in tempo reale.

Modelli di applicazione comuni sono:

- L'inserimento di letture ed eventi dei sensori in modo da consentire le notifiche nonché l'analisi cronologica.
- La gestione degli aggiornamenti batch, anche da più origini, riducendo al minimo l'impatto sul carico di lavoro di lettura simultaneo.

#### <a name="implementation-considerations"></a>Considerazioni sull'implementazione

Usare una tabella ottimizzata per la memoria per l'inserimento dei dati. Se l'operazione di inserimento è costituita principalmente da inserimenti (anziché aggiornamenti) e il footprint di memoria di OLTP in memoria dei dati è un problema, procedere come segue:

- Usare un processo per ripartire regolarmente il carico di lavoro dei dati in batch in una tabella basata su disco con un [indice columnstore cluster](../indexes/columnstore-indexes-overview.md)mediante un processo che esegue l'istruzione `INSERT INTO <disk-based table> SELECT FROM <memory-optimized table>`
- Usare una [tabella temporale ottimizzata per la memoria](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md) per gestire i dati cronologici: in questo modo, i dati cronologici risiedono su disco e lo spostamento dei dati viene gestito dal sistema.

Il repository di esempi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene un'applicazione smart grid che usa una tabella temporale ottimizzata per la memoria, un tipo di tabella ottimizzata per la memoria e una stored procedure compilata in modo nativo per velocizzare l'inserimento dei dati, gestendo il footprint di memoria di OLTP in memoria dei dati del sensore:

- [smart-grid-release](https://github.com/Microsoft/sql-server-samples/releases/tag/iot-smart-grid-v1.0)
- [smart-grid-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/iot-smart-grid)

#### <a name="customer-case-studies"></a>Case study dei clienti

- [Quorum raddoppia il carico di lavoro del database delle chiavi riducendone al contempo l'utilizzo del 70% grazie all'uso di OLTP in memoria nel database SQL di Azure](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)
- Con OLTP in memoria in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], EdgeNet ha migliorato le prestazioni del caricamento batch dei dati e ha rimosso la necessità di mantenere una cache di livello intermedio: [Azienda di servizi dati ottiene l'accesso in tempo reale ai dati dei prodotti con la tecnologia in memoria](https://customers.microsoft.com/story/data-services-firm-gains-real-time-access-to-product-d)
- Il Beth Israel Deaconess Medical Center ha migliorato in modo significativo la velocità di inserimento dei dati dai controller di dominio e la gestione dei picchi del carico di lavoro grazie a OLTP in memoria in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]: https://customers.microsoft.com/story/strengthening-data-security-and-creating-more-time-for

### <a name="caching-and-session-state"></a>Memorizzazione nella cache e stato della sessione

La tecnologia di OLTP in memoria rende SQL molto interessante per mantenere lo stato della sessione (ad esempio, per un'applicazione ASP.NET) e per la memorizzazione nella cache.

Lo stato della sessione ASP.NET è un caso di utilizzo di grande successo per OLTP in memoria. Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un cliente ha quasi raggiunto 1,2 milioni di richieste al secondo. Nel frattempo, questo cliente ha iniziato a usare OLTP in memoria per le esigenze di memorizzazione nella cache di tutte le applicazioni di livello intermedio nell'organizzazione. Dettagli: [In che modo bwin usa OLTP in memoria di [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] per ottenere prestazioni e scalabilità senza precedenti](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/)

#### <a name="implementation-considerations"></a>Considerazioni sull'implementazione

È possibile usare tabelle ottimizzate per la memoria non durevoli come semplice archivio chiave-valore tramite l'archiviazione di un BLOB in una colonna varbinary(max). In alternativa, è possibile implementare una cache semistrutturata con [supporto JSON](https://azure.microsoft.com/blog/json-support-is-generally-available-in-azure-sql-database/) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Infine, è possibile creare una cache relazionale completa tramite tabelle non durevoli con uno schema relazionale completo, compresi vari tipi di dati e vincoli.

Iniziare con l'ottimizzazione per la memoria dello stato della sessione ASP.NET usando gli script pubblicati in GitHub per sostituire gli oggetti creati dal provider di stato della sessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] predefinito: [aspnet-session-state](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/aspnet-session-state)

#### <a name="customer-case-studies"></a>Casi di studio sui clienti

- bwin ha potuto aumentare notevolmente la produttività e ridurre il footprint dell'hardware per lo stato sessione ASP.NET grazie a OLTP in memoria in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]: [Sito di gioco riesce ad arrivare a 250.000 richieste al secondo migliorando l'esperienza dei giocatori](https://customers.microsoft.com/story/gaming-site-can-scale-to-250000-requests-per-second-an)
- bwin ha aumentato ulteriormente la velocità effettiva con lo stato sessione ASP.NET e ha implementato un sistema di memorizzazione nella cache di livello intermedio per l'intera azienda grazie a OLTP in memoria in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]: [In che modo bwin usa OLTP in memoria di [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] per ottenere prestazioni e scalabilità senza precedenti](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/)

### <a name="tempdb-object-replacement"></a>Sostituzione dell'oggetto tempdb

Sfruttare le tabelle non durevoli e i tipi di tabella ottimizzata per la memoria per sostituire le strutture TempDB tradizionali, ad esempio tabelle temporanee, variabili di tabella e parametri con valori di tabella.

Le variabili di tabella con ottimizzazione per la memoria e le tabelle non durevoli riducono in genere l'utilizzo di CPU ed eliminano completamente le operazioni di I/O sui log rispetto alle variabili di tabella tradizionali e alla tabella #temp.

#### <a name="implementation-considerations"></a>Considerazioni sull'implementazione

Per un'introduzione, vedere: [Miglioramento delle prestazioni della tabella temporanea e della variabile di tabella con l'ottimizzazione della memoria.](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)

#### <a name="customer-case-studies"></a>Case study dei clienti

- Un cliente è riuscito a migliorare le prestazioni del 40% semplicemente sostituendo i parametri con valori di tabella tradizionali con i parametri con valori di tabella ottimizzata per la memoria: [Inserimento dati IoT ad alta velocità grazie a OLTP in memoria in Azure](/archive/blogs/sqlserverstorageengine/a-technical-case-study-high-speed-iot-data-ingestion-using-in-memory-oltp-in-azure)
- SentryOne ha migliorato significativamente la capacità di inserimento dati con una latenza quasi nulla nella soluzione di monitoraggio, scambiando le tabelle in tempdb con le tabelle OLTP In-Memory nell'ambito dei miglioramenti di scalabilità aziendale: [Il provider di soluzioni supera il limite massimo delle prestazioni con l'innovazione nel monitoraggio dei dati.](https://customers.microsoft.com/story/sentryone-partner-professional-services-sql-server-azure)

### <a name="etl-extract-transform-load"></a>ETL (Extract, Transform, Load, ovvero estrazione, trasformazione e caricamento)

I flussi di lavoro ETL includono spesso il caricamento dei dati in una tabella di staging, le trasformazioni dei dati e il caricamento nelle tabelle finali.

#### <a name="implementation-considerations"></a>Considerazioni sull'implementazione

Usare tabelle non durevoli ottimizzate per la memoria per lo staging dei dati. Eliminano completamente tutte le operazioni di I/O e rendono più efficiente l'accesso ai dati.

Se si eseguono le trasformazioni nella tabella di staging come parte del flusso di lavoro, è possibile usare le stored procedure compilate in modo nativo per velocizzare tali trasformazioni. Se è possibile eseguire queste trasformazioni in parallelo, si possono ottenere ulteriori vantaggi di scalabilità dall'ottimizzazione per la memoria.

## <a name="sample-script"></a>Script di esempio

Prima di iniziare a usare OLTP in memoria, è necessario creare il filegroup MEMORY_OPTIMIZED_DATA. È inoltre consigliabile usare il livello di compatibilità del database 130 (o superiore) e impostare l'opzione di database MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT su ON.

È possibile usare lo script del collegamento seguente per creare il filegroup nella cartella dati predefinita e per configurare le impostazioni consigliate:

- [enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)

Lo script seguente illustra gli oggetti di OLTP in memoria che è possibile creare nel database:

```sql
-- configure recommended DB option
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
GO
-- memory-optimized table
CREATE TABLE dbo.table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- non-durable table
CREATE TABLE dbo.temp_table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON,
      DURABILITY=SCHEMA_ONLY)
GO
-- memory-optimized table type
CREATE TYPE dbo.tt_table1 AS TABLE
( c1 INT IDENTITY,
  c2 NVARCHAR(MAX),
  is_transient BIT NOT NULL DEFAULT (0),
  INDEX ix_c1 HASH (c1) WITH (BUCKET_COUNT=1024))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- natively compiled stored procedure
CREATE PROCEDURE dbo.usp_ingest_table1
  @table1 dbo.tt_table1 READONLY
WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC
    WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT,
          LANGUAGE=N'us_english')

  DECLARE @i INT = 1

  WHILE @i > 0
  BEGIN
    INSERT dbo.table1
    SELECT c2
    FROM @table1
    WHERE c1 = @i AND is_transient=0

    IF @@ROWCOUNT > 0
      SET @i += 1
    ELSE
    BEGIN
      INSERT dbo.temp_table1
      SELECT c2
      FROM @table1
      WHERE c1 = @i AND is_transient=1

      IF @@ROWCOUNT > 0
        SET @i += 1
      ELSE
        SET @i = 0
    END
  END

END
GO
-- sample execution of the proc
DECLARE @table1 dbo.tt_table1
INSERT @table1 (c2, is_transient) VALUES (N'sample durable', 0)
INSERT @table1 (c2, is_transient) VALUES (N'sample non-durable', 1)
EXECUTE dbo.usp_ingest_table1 @table1=@table1
SELECT c1, c2 from dbo.table1
SELECT c1, c2 from dbo.temp_table1
GO
```

## <a name="resources-to-learn-more"></a>Risorse per altre informazioni

- [Tecnologie OLTP in memoria per ottimizzare le prestazioni di T-SQL](./survey-of-initial-areas-in-in-memory-oltp.md)
- Una dimostrazione sulle prestazioni con OLTP in memoria è disponibile in: [in-memory-oltp-perf-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- [Video di 17 minuti che spiega OLTP in memoria e che illustra la dimostrazione](in-memory-oltp-in-memory-optimization.md#anchorname-17minute-video)
- [Script per abilitare OLTP in memoria e impostare le opzioni consigliate](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)
- [Documentazione principale di OLTP in memoria](in-memory-oltp-in-memory-optimization.md)
- [Vantaggi a livello di prestazioni e uso delle risorse di OLTP in memoria nel database SQL di Azure](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)
- [Miglioramento delle prestazioni della tabella temporanea e della variabile di tabella con l'ottimizzazione della memoria](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)
- [Ottimizzare le prestazioni tramite le tecnologie in memoria nel database SQL](/azure/sql-database/sql-database-in-memory)
- [Tabelle temporali con controllo delle versioni di sistema con tabelle con ottimizzazione per la memoria](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
