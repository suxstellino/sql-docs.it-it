---
title: OLTP in memoria per l'ottimizzazione delle prestazioni di T-SQL
description: Informazioni sulle basi delle caratteristiche di prestazione di OLTP in memoria di SQL Server e database SQL di Azure con spiegazioni rapide ed esempi di codice di base per sviluppatori.
ms.custom: seo-dt-2019
ms.date: 09/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 1c25a164-547d-43c4-8484-6b5ee3cbaf3a
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9e2e2daf03d68efb14dcd90e1b38fecb4973c2c4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492524"
---
# <a name="survey-of-initial-areas-in-in-memory-oltp"></a>Indagine sulle aree iniziali di OLTP in memoria

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  
Questo articolo è per gli sviluppatori che hanno premura di imparare in poco tempo le basi delle caratteristiche di prestazione di OLTP in memoria di Microsoft SQL Server e Database SQL di Azure.  
  
Per OLTP in memoria, questo articolo illustra gli argomenti seguenti:  
  
- Spiegazione concise delle funzionalità.  
- Esempi di codice di base che implementano le funzionalità.  
  
  
SQL Server e Database SQL presentano solo alcune piccole variazioni nel supporto delle tecnologie in memoria.  
  
  
In ambiti informali, alcuni blogger chiamano la funzionalità OLTP in memoria *Hekaton*.  
  
  
<a name="benefits-of-in-memory-features-21a"></a>  
  
## <a name="benefits-of-in-memory-features"></a>Vantaggi delle funzionalità in memoria  
  
SQL Server fornisce funzionalità in memoria che possono migliorare notevolmente le prestazioni di molti sistemi di applicazione. In questa sezione vengono descritte le considerazioni più semplici.  
  
  
### <a name="features-for-oltp-online-transactional-processing"></a>Funzionalità per OLTP (Online Transactional Processing, elaborazione transazionale Online)  
  
  
Le funzionalità OLTP sono più adatte ai sistemi che devono elaborare un numero elevato di SQL INSERT allo stesso tempo.  
  
- Le statistiche mostrano che il miglioramento nella velocità di elaborazione aumenta da 5 a 20 volte se si usano le funzionalità in memoria.  
  
  
Sono più adatti ai sistemi che elaborano complessi calcoli in Transact-SQL.  
  
- Una stored procedure dedicata a calcoli elevati e complessi può essere eseguita fino a 99 volte più veloce.  
  
  
È consigliabile leggere gli articoli seguenti che offrono alcune dimostrazioni di miglioramento delle prestazioni con OLTP in memoria:  
  
- [Dimostrazione: miglioramento delle prestazioni di OLTP in memoria](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md) offre una breve dimostrazione dei potenziali miglioramenti delle prestazioni.  
- [Database di esempio per OLTP in memoria](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md) offre una dimostrazione su larga scala.  
  
  
  
### <a name="features-for-operational-analytics"></a>Funzionalità per l'analisi operativa  
  
L'analisi in memoria si riferisce agli SQL SELECT che aggregano dati transazionali, tipicamente con l'inclusione di una clausola GROUP BY. Il tipo di indice denominato *columnstore* è fondamentale per l'analisi operativa.  
  
Ci sono due scenari principali:  
  
- *Analisi operativa dei batch* si riferisce ai processi di aggregazione che vengono eseguiti dopo gli orari di lavoro oppure su dispositivi secondari che dispongono di copie dei dati transazionali.  
  - [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) riguarda anche l'analisi operativa dei batch.  
- *Analisi operativa in tempo reale* si riferisce a processi di aggregazione che vengono eseguiti durante gli orari di lavoro e nei dispositivi hardware primari usati per i carichi di lavoro transazionali.  
  
  
Questo articolo è incentrato su OLTP e non sull'analisi. Per informazioni sulle funzionalità di analisi introdotte dagli indici columnstore in SQL, vedere:  
  
- [Introduzione a columnstore per l'analisi operativa in tempo reale](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
- [Guida agli indici columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)  
  
  
> [!NOTE]
> Un video di due minuti relativo alle funzionalità in memoria è disponibile alla pagina sulle [tecnologie in memoria per il database SQL di Azure](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-In-Memory-Technologies). Il video è stato pubblicato nel dicembre 2015.  


### <a name="columnstore"></a>columnstore

Una sequenza di accurati post di blog spiega in modo elegante gli indici columnstore da diverse prospettive. La maggior parte dei post descrive ulteriormente il concetto di analisi operativa, supportata da columnstore.  Questi post sono stati redatti da Sunil Agarwal, un Program Manager Microsoft, nel mese di marzo 2016.

#### <a name="real-time-operational-analytics"></a>Analisi operativa in tempo reale

1. [Analisi operativa in tempo reale con tecnologia In-Memory](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)
2. [Real-Time Operational Analytics - Overview nonclustered columnstore index (NCCI)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index) (Analisi operativa in tempo reale: panoramica sugli indici columnstore non cluster (NCCI))
3. [Analisi operativa in tempo reale: un semplice esempio di uso di indici columnstore non cluster e cluster (NCCI) in SQL Server 2016](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)
4. [Analisi operativa in tempo reale: operazioni DML e indici columnstore non cluster (NCCI) in SQL Server 2016](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)
5. [Analisi operativa in tempo reale: indici columnstore non cluster (NCCI) filtrati](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)
6. [Analisi operativa in tempo reale: opzione Ritardo di compressione per indici columnstore non cluster (NCCI)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)
7. [Analisi operativa in tempo reale: opzione Ritardo di compressione con indici columnstore non cluster (NCCI) e relative prestazioni](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)
8. [Analisi operativa in tempo reale: tabelle ottimizzate per la memoria e indici columnstore](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)

#### <a name="defragment-a-columnstore-index"></a>Deframmentare un indice columnstore

1. [Deframmentazione degli indici columnstore tramite il comando REORGANIZE](/archive/blogs/sqlserverstorageengine/columnstore-index-defragmentation-using-reorganize-command)
2. [Criteri di unione degli indici columnstore per REORGANIZE](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)

#### <a name="bulk-importation-of-data"></a>Importazione bulk di dati

1. [Indici columnstore cluster: caricamento bulk](/archive/blogs/sqlserverstorageengine/clustered-column-store-index-bulk-loading-the-data)
2. [Indice columnstore cluster: ottimizzazioni del caricamento dati con registrazione minima](/archive/blogs/sqlserverstorageengine/clustered-columnstore-index-data-load-optimizations-minimal-logging)
3. [Indice columnstore cluster: ottimizzazioni del caricamento dati con importazione bulk parallela](/archive/blogs/sqlserverstorageengine/clustered-columnstore-index-parallel-bulk-import)





<a name="features-on-in-memory-oltp-13b"></a>  
  
## <a name="features-of-in-memory-oltp"></a>Funzionalità di OLTP in memoria  
  
Funzionalità principali di OLTP in memoria.  
  
  
#### <a name="memory-optimized-tables"></a>Tabelle ottimizzate per la memoria  
  
La parola chiave MEMORY_OPTIMIZED di T-SQL nell'istruzione CREATE TABLE indica come viene creata una tabella nella memoria attiva, invece che sul disco.  
  
  
Una [tabella con ottimizzazione per la memoria](./sample-database-for-in-memory-oltp.md) ha una sola rappresentazione nella memoria attiva e una copia secondaria sul disco.  
  
- La copia su disco è destinata al recupero di routine dopo un riavvio del server o del database. Questa dualità memoria-disco è completamente nascosta agli utenti e al codice.  
  
  
#### <a name="natively-compiled-modules"></a>Moduli compilati in modo nativo  
  
La parola chiave NATIVE_COMPILATION di T-SQL nell'istruzione CREATE PROCEDURE indica come viene creata una stored procedure compilata in modo nativo. Le istruzioni di T-SQL vengono compilate con codice macchina quando si usa per la prima volta la procedura nativa ogni volta che il database viene avviato online. Le istruzioni di T-SQL non presentano più una lenta interpretazione di ogni istruzione.  
  
- Come si è visto, la durata dei risultati della compilazione nativa arriva a 1/100 della durata interpretata.  
  
  
Un modulo nativo può fare riferimento solo a tabelle ottimizzate per la memoria e non a tabelle basate su disco.  
  
Esistono tre tipi di moduli compilati in modo nativo:  
  
- [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md).  
- Funzioni definite dall'utente (UDF) compilate in modo nativo, di tipo scalare.  
- Trigger compilati in modo nativo.  
  
  
#### <a name="availability-in-azure-sql-database"></a>Disponibilità nel database SQL di Azure  
  
Le funzionalità OLTP in memoria e columnstore sono disponibili nel database SQL Azure. Per altre informazioni vedere [Introduzione alle tecnologie in memoria (anteprima) in database SQL](/azure/sql-database/sql-database-in-memory).
  
  
<a name="ensure-compatibility-level-gteq-130-99c"></a>  
  
## <a name="1-ensure-compatibility-level--130"></a>1. Assicurare un livello di compatibilità >= 130  
  
  
Questa sezione include una sequenza di sezioni numerate che illustrano insieme la sintassi di Transact-SQL da usare per implementare le funzionalità OLTP in memoria.  
  
  
Innanzitutto, è importante che il database sia impostato su un livello di compatibilità di almeno 130. Di seguito è riportato il codice T-SQL per visualizzare il livello di compatibilità corrente del database su cui è impostato il database.  
  
```sql
SELECT d.compatibility_level
    FROM sys.databases as d
    WHERE d.name = Db_Name();
```
  
  
  
Il codice T-SQL seguente consente di aggiornare il livello, se necessario.  
  
  
  
```sql
ALTER DATABASE CURRENT
    SET COMPATIBILITY_LEVEL = 130;
```
  
  
  
<a name="elevate-to-snapshot-26n"></a>  
  
## <a name="2-elevate-to-snapshot"></a>2. Elevare a SNAPSHOT  
  
  
Quando una transazione include una tabella basata su disco e una tabella ottimizzata per la memoria viene detta *transazione tra contenitori*. In questo tipo di transazione è essenziale che la parte ottimizzata per la memoria sia operativa al livello di isolamento della transazione denominato SNAPSHOT.  
  
Per applicare in modo affidabile questo livello per le tabelle ottimizzate per la memoria in una transazione tra contenitori, [modificare l'impostazione del database](../../t-sql/statements/alter-database-transact-sql-set-options.md) con l'oggetto T-SQL seguente.  
  
  
  
```sql
ALTER DATABASE CURRENT
    SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```
  
  
  
<a name="create-an-optimized-filegroup-24r"></a>  
  
## <a name="3-create-an-optimized-filegroup"></a>3. Creare un FILEGROUP ottimizzato  
  
  
In Microsoft SQL Server prima di creare una tabella ottimizzata per la memoria è necessario creare prima di tutto un FILEGROUP dichiarato come CONTAINS MEMORY_OPTIMIZED_DATA. FILEGROUP è assegnato al database. Per informazioni dettagliate, vedere:  
  
- [FILEGROUP con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)  
  
  
Nel database SQL di Azure non è necessario e non è possibile creare un FILEGROUP.  

Lo script T-SQL di esempio seguente abilita un database per OLTP in memoria e configura tutte le impostazioni consigliate. Funziona con SQL Server e il database SQL di Azure: [enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql).

Si noti che non tutte le funzionalità di SQL Server sono supportate per i database con un filegroup MEMORY_OPTIMIZED_DATA. Per informazioni dettagliate sulle limitazioni, vedere: [Funzionalità di SQL Server non supportate per OLTP in memoria](unsupported-sql-server-features-for-in-memory-oltp.md)
  
<a name="create-a-memory-optimized-table-26y"></a>  
  
## <a name="4-create-a-memory-optimized-table"></a>4. Creare una tabella ottimizzata per la memoria  
  
La parola chiave principale di Transact-SQL è MEMORY_OPTIMIZED.  
  
  
  
```sql
CREATE TABLE dbo.SalesOrder
    (
        SalesOrderId   integer   not null   IDENTITY
            PRIMARY KEY NONCLUSTERED,
        CustomerId   integer    not null,
        OrderDate    datetime   not null
    )
        WITH
            (MEMORY_OPTIMIZED = ON,
            DURABILITY = SCHEMA_AND_DATA);
```
  
  
  
Le istruzioni Transact-SQL INSERT e SELECT eseguite in una tabella ottimizzata per la memoria sono uguali a quelle per una tabella normale.  
  
#### <a name="alter-table-for-memory-optimized-tables"></a>ALTER TABLE per le tabelle con ottimizzazione per la memoria  
  
ALTER TABLE...ADD/DROP può aggiungere o rimuovere una colonna da una tabella ottimizzata per la memoria o un indice.  
  
- CREATE INDEX e DROP INDEX non possono essere eseguiti in una tabella ottimizzata per la memoria. Usare ALTER TABLE ... ADD/DROP INDEX.  
- Per dettagli, vedere [Modifica di tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md).  
  
  
#### <a name="plan-your-memory-optimized-tables-and-indexes"></a>Pianificare indici e tabelle ottimizzate per la memoria  
  
  
- [Indici per tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)  
- [Costrutti Transact-SQL non supportati da OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
  
  
<a name="create-a-natively-compiled-stored-procedure-native-proc-29u"></a>  
  
## <a name="5-create-a-natively-compiled-stored-procedure-native-proc"></a>5. Creare una stored procedure compilata in modo nativo (procedura nativa)  
  
  
La parola chiave principale è NATIVE_COMPILATION.  
  
  
  
```sql
CREATE PROCEDURE ncspRetrieveLatestSalesOrderIdForCustomerId  
        @_CustomerId   INT  
        WITH  
            NATIVE_COMPILATION,  
            SCHEMABINDING  
    AS  
    BEGIN ATOMIC  
        WITH  
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'us_english')  
      
        DECLARE @SalesOrderId int, @OrderDate datetime;
      
        SELECT TOP 1  
                @SalesOrderId = s.SalesOrderId,  
                @OrderDate    = s.OrderDate  
            FROM dbo.SalesOrder AS s  
            WHERE s.CustomerId = @_CustomerId  
            ORDER BY s.OrderDate DESC;  
      
        RETURN @SalesOrderId;  
    END;  
```
  
  
  
La parola chiave SCHEMABINDING indica che le tabelle a cui si fa riferimento nella procedura nativa non possono essere eliminate a meno che non si elimini prima la procedura nativa. Per informazioni dettagliate, vedere [Creazione di stored procedure compilate in modo nativo](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md).  
  
Si noti che non è necessario creare stored procedure compilate in modo nativo per accedere a una tabella con ottimizzazione per la memoria. È anche possibile fare riferimento a tabelle con ottimizzazione per la memoria da stored procedure tradizionali e batch ad hoc.
  
<a name="execute-the-native-proc-31e"></a>  
  
## <a name="6-execute-the-native-proc"></a>6. Eseguire la procedura nativa  
  
  
Popolare la tabella con due righe di dati.  
  
  
  
```sql
INSERT into dbo.SalesOrder  
        ( CustomerId, OrderDate )  
    VALUES  
        ( 42, '2013-01-13 03:35:59' ),
        ( 42, '2015-01-15 15:35:59' );
```
  
  
  
Eseguire quindi una chiamata EXECUTE alla stored procedure compilata in modo nativo.  
  
  
  
```sql
DECLARE @LatestSalesOrderId int, @mesg nvarchar(128);
      
EXECUTE @LatestSalesOrderId =  
    ncspRetrieveLatestSalesOrderIdForCustomerId 42;
      
SET @mesg = CONCAT(@LatestSalesOrderId,  
    ' = Latest SalesOrderId, for CustomerId = ', 42);
PRINT @mesg;  
```
      
Di seguito è riportato l'output effettivo di PRINT:  
```output
-- 2 = Latest SalesOrderId, for CustomerId = 42  
```
  
  
  
  
<a name="guide-to-the-documentation-and-next-steps-32w"></a>  
  
### <a name="guide-to-the-documentation-and-next-steps"></a>Guida alla documentazione e passaggi successivi  
  
  
Gli esempi semplici descritti in precedenza forniscono una base per l'apprendimento delle funzionalità più avanzate di OLTP in memoria. Le sezioni seguenti rappresentano una guida relativa a particolari considerazioni che potrebbe essere necessario conoscere e forniscono dettagli sulle singole funzionalità.  
  
  
  
<a name="how-do-in-memory-oltp-features-work-so-much-faster-33v"></a>  
  
## <a name="how-in-memory-oltp-features-work-so-much-faster"></a>In che modo le funzionalità OLTP in memoria consentono un funzionamento molto più rapido?  
  
  
Le sottosezioni seguenti descrivono brevemente come funzionano internamente le funzionalità OLTP in memoria per fornire prestazioni migliori.  
  
  
<a name="how-do-memory-optimized-tables-perform-faster-34q"></a>  
  
### <a name="how-memory-optimized-tables-perform-faster"></a>Prestazioni più rapide delle tabelle ottimizzate per la memoria  
  
  
**Duplice natura:** una tabella ottimizzata per la memoria ha una duplice natura: una rappresentazione nella memoria attiva e una nel disco rigido. Per ogni transazione viene eseguito il commit per entrambe le rappresentazioni della tabella. Le transazioni usano la rappresentazione nella memoria attiva più veloce. Le tabelle con ottimizzazione per la memoria possono sfruttare la maggiore velocità della memoria attiva rispetto al disco. Inoltre, la maggiore flessibilità della memoria attiva semplifica l'uso di una struttura di tabella più avanzata ottimizzata per la velocità. La struttura avanzata è anche priva di pagine, quindi evita problemi di sovraccarico e contesa associati a latch e spinlock.  
  
  
**Nessun blocco:** la tabella ottimizzata per la memoria si basa su un approccio *ottimistico* volto a raggiungere contemporaneamente gli obiettivi di integrità dei dati e di concorrenza e alta velocità effettiva. Durante la transazione, la tabella non inserisce blocchi in alcune versione delle righe aggiornate dei dati. Questo consente di ridurre notevolmente le contese in alcuni sistemi con volumi elevati.  
  
  
**Versioni di riga:** al posto dei blocchi, la tabella ottimizzata per la memoria aggiunge una nuova versione di una riga aggiornata nella tabella stessa, non in tempdb. La riga originale viene mantenuta fino a dopo il commit della transazione. Durante la transazione, altri processi possono leggere la versione originale della riga.  
  
- Quando vengono create più versioni di una riga per una tabella basata su disco, queste versioni vengono archiviate temporaneamente in tempdb.  
  
  
**Meno attività di registrazione:** la versione precedente e quella successiva all'aggiornamento delle righe vengono mantenute nella tabella ottimizzata per la memoria. La coppia di righe fornisce gran parte delle informazioni solitamente scritte nel file di log. Ciò consente al sistema di scrivere nel log una quantità minore di informazioni e con una frequenza inferiore. L'integrità transazionale viene comunque garantita.  
  
  
<a name="how-do-native-procs-perform-faster-35x"></a>  
  
### <a name="how-native-procs-perform-faster"></a>Prestazioni più rapide delle procedure native  
  
La conversione di una stored procedure interpretata regolarmente in una stored procedure compilata in modo nativo riduce notevolmente il numero di istruzioni da eseguire durante la fase di esecuzione.  
  
  
<a name="trade-offs-of-in-memory-features-36j"></a>  
  
## <a name="trade-offs-of-in-memory-features"></a>Vantaggi e svantaggi delle funzionalità in memoria  
  
  
Come avviene spesso in informatica, il miglioramento delle prestazioni ottenuto con le funzionalità in memoria si basa su un compromesso. I vantaggi offerti dai miglioramenti alle funzionalità valgono più del costo aggiuntivo per l'acquisto. È possibile trovare guide linea complete su vantaggi e svantaggi in:

- [Pianificare l'adozione delle funzionalità OLTP in memoria in SQL Server](../../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)

Nel resto di questa sezione sono elencate alcune delle principali considerazioni di pianificazione, vantaggi e svantaggi.
  
<a name="trade-offs-of-memory-optimized-tables-37d"></a>  
  
### <a name="trade-offs-of-memory-optimized-tables"></a>Vantaggi e svantaggi delle tabelle ottimizzate per la memoria  
  
  
**Stimare la memoria:** è necessario stimare la quantità di memoria attiva che verrà utilizzata dalla tabella ottimizzata per la memoria. Il computer deve avere una capacità di memoria sufficiente per ospitare una tabella ottimizzata per la memoria. Per informazioni dettagliate, vedere:  
  
- [Monitorare e risolvere i problemi relativi all'utilizzo della memoria](../../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)  
- [Stimare i requisiti di memoria delle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)  
- [Dimensioni di tabelle e righe per le tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
  
**Partizionare le tabelle di grandi dimensioni:** un modo per soddisfare la richiesta di grandi quantità di memoria attiva consiste nel partizionare le tabelle di grandi dimensioni in parti in memoria che archiviano righe di dati *attive recenti* e in altre parti su disco che archiviano righe *non attive legacy*, ad esempio ordini di vendita già consegnati e completati. Questo partizionamento è un processo manuale di progettazione e implementazione. Vedere:  
  
- [Partizionamento a livello di applicazione](../../relational-databases/in-memory-oltp/application-level-partitioning.md)  
- [Modello di applicazione per il partizionamento di tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/application-pattern-for-partitioning-memory-optimized-tables.md)  
  
<a name="trade-offs-of-native-procs-38p"></a>  
  
### <a name="trade-offs-of-native-procs"></a>Vantaggi e svantaggi delle procedure native  
  
- Con una stored procedure compilata in modo nativo non è possibile accedere a una tabella basata su disco. Una procedura nativa può accedere solo alle tabelle ottimizzate per la memoria.  
- Quando viene eseguita una procedura nativa per la prima volta dopo aver riportato online il server o il database, è necessario ricompilare la procedura nativa. Ciò causa un ritardo nell'avvio dell'esecuzione della procedura nativa.  
  
<a name="advanced-considerations-for-memory-optimized-tables-39n"></a>  
  
## <a name="advanced-considerations-for-memory-optimized-tables"></a>Considerazioni avanzate sulle tabelle ottimizzate per la memoria  
  
Gli[indici delle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md) sono per alcuni aspetti diversi dagli indici delle tabelle tradizionali su disco. Gli indici hash sono disponibili solo in tabelle ottimizzate per la memoria.
    
- [Hash Indexes for Memory-Optimized Tables (Indici hash per tabelle con ottimizzazione per la memoria)](../../relational-databases/sql-server-index-design-guide.md#hash_index)
- [Indice non cluster per tabelle ottimizzate per la memoria](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index) 
  
È necessario eseguire una pianificazione per garantire una quantità di memoria attiva sufficiente per la tabella ottimizzata per la memoria pianificata e i relativi indici. Vedere:  
  
- [Creazione e gestione dell'archiviazione per gli oggetti ottimizzati per la memoria](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
Una tabella ottimizzata per la memoria può essere dichiarata con DURABILITY = SCHEMA_ONLY:  
  
- Questa sintassi indica al sistema di eliminare tutti i dati dalla tabella ottimizzata per la memoria quando il database viene portato offline. Viene conservata solo la definizione della tabella .  
- Quando il database viene riportato online, la tabella ottimizzata per la memoria viene ricaricata nella memoria attiva senza alcun dato.  
- Le tabelle SCHEMA_ONLY possono essere un'alternativa superiore alle [tabelle #temporary](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md) in tempdb, quando sono coinvolte molte migliaia di righe.  
  
Le variabili di tabella possono anche essere dichiarate come ottimizzate per la memoria. Vedere:  
  
- [Tabella temporanea più rapida e variabile di tabella tramite l'ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)  
  
<a name="advanced-considerations-for-natively-compiled-modules-40k"></a>  
  
## <a name="advanced-considerations-for-natively-compiled-modules"></a>Considerazioni avanzate sui moduli compilati in modo nativo  
  
I tipi di moduli compilati in modo nativo disponibili da Transact-SQL sono:  
  
- Stored procedure compilate in modo nativo (procedure native).  
- [Funzioni definite dall'utente scalari](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)compilate in modo nativo.  
- Trigger compilati in modo nativo (trigger nativi).  
  - Nelle tabelle ottimizzate per la memoria sono consentiti solo i trigger che vengono compilati in modo nativo.  
- [Funzioni con valori di tabella](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)compilate in modo nativo.  
  - [Miglioramento delle prestazioni della tabella temporanea e della variabile di tabella con l'ottimizzazione della memoria](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)  
  
Una funzione definita dall'utente (UDF) e compilata in modo nativo viene eseguita in maniera più rapida di un'UDF interpretata. Alcuni aspetti da considerare con le UDF:  
  
- Quando un'istruzione SELECT di T-SQL usa un'UDF, quest'ultima viene sempre chiamata una volta per ogni riga restituita.  
  - Le funzioni definite dall'utente non vengono mai eseguite inline, ma vengono sempre chiamate.  
  - La distinzione compilata è meno significativa dell'overhead di chiamate ripetute inerente a tutte le UDF.  
  - Comunque, l'overhead delle chiamate delle UDF è spesso accettabile a livello pratico.  
  
Per spiegazioni e dati di prova sulle prestazioni delle UDF native, vedere:  
  
  - [Il blog che spiega come attenuare l'impatto RBAR con funzioni definite dall'utente compilate in modo nativo in SQL Server 2016](/archive/blogs/sqlcat/soften-the-rbar-impact-with-native-compiled-udfs-in-sql-server-2016)  
  - [Il post di Gail Shaw del gennaio 2016 sulle funzioni definite dall'utente compilate in modo nativo](https://sqlinthewild.co.za/index.php/2016/01/12/natively-compiled-user-defined-functions/).  
  
<a name="documentation-guide-for-memory-optimized-tables-41z"></a>  
  
## <a name="documentation-guide-for-memory-optimized-tables"></a>Guida alla documentazione per le tabelle ottimizzate per la memoria  
  
Vedere gli articoli seguenti che presentano alcune considerazioni particolari relative alle tabelle ottimizzate per la memoria:  
  
- [Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  - [Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)  
  - Il report di analisi delle prestazioni delle transazioni in SQL Server Management Studio consente di valutare se OLTP in memoria è in grado di migliorare le prestazioni delle applicazioni del database.  
  - Usare [Ottimizzazione guidata per la memoria](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) per eseguire la migrazione della tabella di database basata su disco a OLTP in memoria.   
- [Eseguire il backup, ripristinare e recuperare tabelle con ottimizzazione per la memoria](/previous-versions/sql/sql-server-2016/dn624160(v=sql.130))  
  - Lo spazio di archiviazione usato dalle tabelle ottimizzate per la memoria può essere di gran lunga superiore rispetto alle relative dimensioni in memoria e influisce sulle dimensioni del backup del database.  
- [Transactions with Memory-Optimized Tables](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)  
  - Include informazioni sulla logica di riesecuzione in T-SQL, per le transazioni in tabelle ottimizzate per la memoria.  
- [Supporto di Transact-SQL per OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  - Tipi di dati e istruzioni T-SQL supportati e non supportati per le tabelle ottimizzate per la memoria e le procedure native.  
- [Associazione di un database con tabelle con ottimizzazione per la memoria a un pool di risorse](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md), che illustra una considerazione avanzata facoltativa.  

<a name="documentation-guide-for-native-procs-42b"></a>  
  
## <a name="documentation-guide-for-native-procs"></a>Guida alla documentazione per le procedure native  

Il seguente articolo e gli articoli correlati indicati nel sommario spiegano in dettaglio le stored procedure compilate in modo nativo.

- [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)
  
<a name="related-links-43f"></a>  
  
## <a name="related-links"></a>Collegamenti correlati  
  
- Articolo iniziale: [OLTP in memoria (ottimizzazione in memoria)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
    
Gli articoli seguenti offrono codice per dimostrare il miglioramento delle prestazioni che è possibile raggiungere tramite OLTP in memoria:  
  
- [Dimostrazione: miglioramento delle prestazioni di OLTP in memoria](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md) offre una breve dimostrazione dei potenziali miglioramenti delle prestazioni.  
- [Database di esempio per OLTP in memoria](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md) offre una dimostrazione su larga scala.