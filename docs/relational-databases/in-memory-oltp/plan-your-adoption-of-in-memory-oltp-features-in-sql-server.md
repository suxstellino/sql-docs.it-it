---
title: "Pianificare l'adozione delle funzionalità OLTP in memoria "
description: Informazioni sul modo in cui l'adozione delle funzionalità OLTP in memoria in SQL Server influisce su altri aspetti del sistema aziendale.
ms.custom: seo-dt-2019
ms.date: 01/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 041b428f-781d-4628-9f34-4d697894e61e
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fa2aebdfa253167b7cdf900a031affc246cfea3e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492213"
---
# <a name="plan-your-adoption-of-in-memory-oltp-features-in-sql-server"></a>Pianificare l'adozione delle funzionalità OLTP in memoria in SQL Server
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


In questo articolo vengono descritte i modi in cui l'adozione delle funzionalità In-Memory influisce su altri aspetti del sistema aziendale.



## <a name="a-adoption-of-in-memory-oltp-features"></a>R. Adozione delle funzionalità OLTP in memoria


Le sottosezioni seguenti illustrano i fattori da tenere in considerazione quando si intende adottare e implementare le funzionalità In-Memory. Molte altre informazioni esplicative sono disponibili in:

- [Usare OLTP in memoria per migliorare le prestazioni dell'applicazione in un database SQL di Azure](/azure/azure-sql/in-memory-oltp-configure)



### <a name="a1-prerequisites"></a>A.1 Prerequisiti

Uno dei prerequisiti per l'uso di funzionalità In-Memory può implicare l'edizione o il livello di servizio del prodotto SQL. Per questo e altri prerequisiti, vedere:

- [Requisiti per l'utilizzo di tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)
    - [Edizioni e componenti di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)
    - [Indicazioni sui livelli di prezzo del database SQL](/azure/azure-sql/database/service-tiers-vcore)


### <a name="a2-forecast-the-amount-of-active-memory"></a>A.2 Prevedere la quantità di memoria attiva

Il sistema dispone di sufficiente memoria attiva per supportare una nuova tabella ottimizzata per la memoria?

#### <a name="microsoft-sql-server"></a>Microsoft SQL Server

Una tabella ottimizzata per la memoria che contiene 200 GB di dati richiede più di 200 GB di memoria attiva dedicata al suo supporto. Prima di implementare una tabella ottimizzata per la memoria che contiene una grande quantità di dati, si deve prevedere la quantità di memoria aggiuntiva attiva che è necessario aggiungere al computer server. Per linee guida sulla stima, vedere:

- [Stimare i requisiti di memoria delle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)

#### <a name="azure-sql-database"></a>database SQL di Azure

Per un database ospitato nel servizio cloud del database SQL di Azure, il livello di servizio scelto influisce sulla quantità di memoria attiva che il database può usare. È consigliabile monitorare l'utilizzo della memoria del database tramite un avviso. Per informazioni dettagliate, vedere:

- Esaminare i limiti di archiviazione di OLTP in memoria per il [piano tariffario](/azure/sql-database/sql-database-purchase-models)
- [Monitorare l'archiviazione OLTP in memoria](/azure/azure-sql/in-memory-oltp-monitor-space)

#### <a name="memory-optimized-table-variables"></a>Variabili di tabella con ottimizzazione per la memoria

È preferibile usare una variabile di tabella dichiarata con ottimizzazione per la memoria piuttosto che una #TempTable tradizionale che risiede nel database **tempdb** . Tali variabili di tabella possono garantire miglioramenti significativi delle prestazioni senza usare grandi quantità di memoria attiva.

### <a name="a3-table-must-be-offline-to-convert-to-memory-optimized"></a>A.3 La tabella deve essere offline per la conversione in tabella ottimizzata per la memoria

Parte della funzionalità di ALTER TABLE è disponibile per le tabelle ottimizzate per la memoria. Ma non è possibile eseguire un'istruzione ALTER TABLE per convertire una tabella basata su disco in una tabella ottimizzata per la memoria. È invece necessario usare una serie di passaggi più manuali. Di seguito vengono elencati vari modi in cui è possibile convertire la tabella basata su disco in tabella ottimizzata per la memoria.

#### <a name="manual-scripting"></a>Esecuzione manuale di script

Un modo per convertire la tabella basata su disco in una tabella ottimizzata per la memoria consiste nel codificare manualmente i passaggi necessari di Transact-SQL.


1. Sospendere l'attività dell'applicazione.

2. Eseguire un backup completo.

3. Rinominare la tabella basata su disco.

4. Eseguire un'istruzione CREATE TABLE per creare la nuova tabella ottimizzata per la memoria.

5. Aggiungere dati nella tabella ottimizzata per la memoria dalla tabella basata su disco con un clausola sub-SELECT.

6. Eliminare la tabella basata su disco.

7. Eseguire un altro backup completo.

8. Riprendere l'attività dell'applicazione.


#### <a name="memory-optimization-advisor"></a>Ottimizzazione guidata per la memoria

Lo strumento Ottimizzazione guidata per la memoria può generare uno script per supportare l'implementazione della conversione di una tabella basata su disco in una tabella ottimizzata per la memoria. Lo strumento è disponibile nell'installazione di SQL Server Data Tools (SSDT).

- [Ottimizzazione guidata per la memoria](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)
- [Scaricare la versione più recente di SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)


#### <a name="dacpac-file"></a>file DACPAC

È possibile aggiornare il database sul posto tramite un file con estensione DACPAC gestito da SSDT. In SSDT è possibile specificare le modifiche allo schema codificato nel file con estensione DACPAC.

Si usano file con estensione DACPAC nel contesto di un progetto di Visual Studio di tipo *Database*.

- [Applicazioni livello dati](../../relational-databases/data-tier-applications/data-tier-applications.md) e file con estensione DACPAC



### <a name="a4-guidance-for-whether-in-memory-oltp-features-are-right-for-your-application"></a>A.4 Linee guida per verificare se le funzionalità di OLTP in memoria sono adatte all'applicazione

Per le linee guida per verificare se le funzionalità di OLTP in memoria possono migliorare le prestazioni di un'applicazione specifica, vedere:

- [OLTP in memoria (ottimizzazione in memoria)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)



## <a name="b-unsupported-features"></a>B. Funzionalità non supportate

Le funzionalità non supportate in alcuni scenari di OLTP in memoria sono descritte in:

- [Funzionalità di SQL Server non supportate per OLTP in memoria](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)


Le sottosezioni seguenti illustrano alcune delle più importanti funzionalità non supportate.


### <a name="b1-snapshot-of-a-database"></a>B.1 SNAPSHOT di un database

Dopo la prima creazione di un modulo o tabella ottimizzata per la memoria in un database specifico, non è più possibile eseguire [snapshot](../../relational-databases/databases/database-snapshots-sql-server.md) del database. Il motivo specifico è che:

- Il primo elemento ottimizzato per la memoria rende impossibile l'eliminazione dell'ultimo file dal filegroup ottimizzato per la memoria e
- nessun database che ha un file nel filegroup ottimizzato per la memoria può supportare uno snapshot.

In genere uno snapshot può essere utile per le iterazioni di test rapidi.


### <a name="b2-cross-database-queries"></a>B.2 Query tra database

Le tabelle con ottimizzazione per la memoria non supportano transazioni [tra database](../../relational-databases/in-memory-oltp/cross-database-queries.md) . Non è possibile accedere a un altro database dalla stessa transazione o dalla stessa query che accede anche a una tabella ottimizzata per la memoria.

Le variabili di tabella non sono transazionali. Di conseguenza, le [variabili di tabella ottimizzata per la memoria](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md) non possono essere usate nelle query tra database.


### <a name="b3-readpast-table-hint"></a>B.3 Hint di tabella READPAST

Nessuna query può applicare l' [hint di tabella](../../t-sql/queries/hints-transact-sql-table.md) READPAST alle tabelle ottimizzate per la memoria.

L'hint READPAST è utile in scenari con più sessioni che accedono e modificano lo stesso set di piccole dimensioni di righe, ad esempio nell'elaborazione di una coda.


### <a name="b4-rowversion-sequence"></a>B.4 RowVersion, Sequenza

- Nessuna colonna può essere contrassegnata per [RowVersion](../../t-sql/data-types/rowversion-transact-sql.md) in una tabella ottimizzata per la memoria.


- Un oggetto [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md) non può essere usato con un vincolo in una tabella ottimizzata per la memoria. Ad esempio, non è possibile creare un vincolo DEFAULT con una clausola NEXT VALUE FOR. Gli oggetti SEQUENCE possono essere usati con le istruzioni INSERT e UPDATE.


## <a name="c-administrative-maintenance"></a>C. Manutenzione amministrativa


In questa sezione vengono descritte le differenze nell'amministrazione di un database in cui vengono usate tabelle ottimizzate per la memoria.


### <a name="c1-identity-seed-reset-increment--1"></a>C.1 Reimpostazione del valore di inizializzazione Identity, incremento > 1

Per reinizializzare una colonna IDENTITY, non è possibile usare[DBCC CHECKIDENT](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)in una tabella ottimizzata per la memoria.

Il valore di incremento è limitato a 1 per una colonna IDENTITY in una tabella ottimizzata per la memoria.


### <a name="c2-dbcc-checkdb-cannot-validate-memory-optimized-tables"></a>C.2 DBCC CHECKDB non può validare tabelle ottimizzate per la memoria

Il comando [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) non produce alcun risultato quando si esegue in una tabella ottimizzata per la memoria. I passaggi seguenti rappresentano una soluzione alternativa:


1. [Eseguire un backup del log delle transazioni](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).

2. Eseguire il backup dei file del filegroup ottimizzato per la memoria in un dispositivo NULL. Il processo di backup richiama una convalida mediante checksum.

    Se viene rilevato un errore, procedere con i passaggi successivi.

3. Copiare i dati dalle tabelle ottimizzate per la memoria in tabelle basate su disco, per l'archiviazione temporanea.

4. Ripristinare i file del filegroup ottimizzato per la memoria.

5. Inserire nelle tabelle ottimizzate per la memoria i dati temporaneamente archiviati nelle tabelle basate su disco.

6. Eliminare le tabelle basate su disco che includevano temporaneamente i dati.



## <a name="d-performance"></a>D. Prestazioni

In questa sezione vengono descritte situazioni in cui le prestazioni delle tabelle ottimizzate per la memoria non raggiungono la loro completa potenzialità.


### <a name="d1-index-considerations"></a>D.1 Considerazioni sugli indici

Tutti gli indici in una tabella ottimizzata per la memoria vengono creati e gestiti dalle istruzioni CREATE TABLE e ALTER TABLE relative alla tabella. Non è possibile usare come destinazione una tabella ottimizzata per la memoria tramite un'istruzione CREATE INDEX.

L'indice non cluster ad albero B tradizionale è spesso una scelta ragionevole e semplice quando si implementa una tabella ottimizzata per la memoria. In un secondo momento, dopo aver verificato le prestazioni dell'applicazione, è possibile sostituirlo con un altro tipo di indice.

Nel contesto di una tabella ottimizzata per la memoria è necessario considerare due tipi speciali di indici: gli indici hash e gli indici columnstore.

Per una panoramica degli indici in tabelle ottimizzate per la memoria, vedere:

- [Indici per tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)


#### <a name="hash-indexes"></a>Indici hash

Gli indici hash possono rappresentare il formato più veloce per l'accesso a una riga specifica con il valore della chiave primaria preciso tramite l'operatore ' **=** '.

- Gli operatori non precisi, ad esempio ' **! =** ',' **>** ' o '**BETWEEN**' potrebbero influire sulle prestazioni se vengono usati con un indice hash.

- Un indice hash potrebbe non essere la scelta migliore se la velocità di duplicazione del valore chiave diventa troppo elevata.

- Per evitare lunghe catene all'interno di singoli bucket, non sottostimare la quantità di *bucket* necessari all'indice hash, . Per informazioni dettagliate, vedere:
    - [Hash Indexes for Memory-Optimized Tables (Indici hash per tabelle con ottimizzazione per la memoria)](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md)


#### <a name="nonclustered-columnstore-indexes"></a>Indici columnstore non cluster

Le tabelle con ottimizzazione per la memoria offrono un'elevata velocità effettiva con dati transazionali aziendali tipici denominata *elaborazione delle transazioni online* o *OLTP (Online Transaction Processing)* . Gli indici columnstore offrono elevata velocità effettiva con aggregazioni e processi denominata *Analisi*. In passato, l'approccio migliore disponibile per soddisfare le esigenze di analisi e OLTP era quello di separare le tabelle con elevato movimento di dati e con un certo livello di duplicazione dei dati. Oggi è disponibile una **soluzione ibrida** più semplice, ovvero la creazione di un indice columnstore in una tabella ottimizzata per la memoria.


- Un [indice columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) può essere compilato in una tabella basata su disco, anche come indice cluster. Ma in una tabella ottimizzata per la memoria, un indice columnstore non può essere raggruppato in cluster.


- Le colonne LOB (Large Object, Oggetti di grandi dimensioni) o colonne all'esterno di righe in una tabella ottimizzata per la memoria impediscono la creazione di un indice columnstore nella tabella.


- Nessuna istruzione ALTER TABLE può essere eseguita in una tabella ottimizzata per la memoria se è presente un indice columnstore nella tabella.
    - A partire da agosto 2016, Microsoft ha intrapreso un progetto a breve termine per migliorare le prestazioni nel ricreare l'indice columnstore.



### <a name="d2-lob-and-off-row-columns"></a>D.2 Colonne LOB e colonne all'esterno di righe

Le colonne LOB sono di tipi specifici, ad esempio varchar (**max**). Un paio di colonne LOB in una tabella ottimizzata per la memoria probabilmente non influenza le prestazioni in maniera significativa. Evitare comunque la presenza di colonne LOB non necessarie. Lo stesso consiglio si applica alle colonne all'esterno di righe. Non definire una colonna come nvarchar(3072) se varchar(512) è sufficiente.


Altre informazioni sulle colonne all'esterno di righe e colonne LOB sono disponibili in:

- [Dimensioni di tabelle e righe per le tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)
- [Tipi di dati supportati per OLTP In memoria](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)



## <a name="e-limitations-of-native-procs"></a>E. Limiti delle procedure native


Alcuni elementi specifici di Transact-SQL non sono supportati in moduli T-SQL compilati in modo nativo, incluse le stored procedure. Per informazioni dettagliate sulle funzionalità supportate, vedere:

- [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)

Per considerazioni sulla migrazione di un modulo Transact-SQL compilato in modo nativo che usa funzionalità non supportate, vedere:

- [Problemi di migrazione relativi alle stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)

Oltre alle limitazioni su specifici elementi di Transact-SQL, esistono limitazioni anche sugli operatori di query supportati nei moduli T-SQL compilati in modo nativo. A causa di queste limitazioni, le stored procedure compilate in modo nativo non sono adatte per le query analitiche che elaborano grandi set di dati.

#### <a name="no-parallel-processing-in-a-native-proc"></a>Elaborazione parallela in una procedura nativa

L'elaborazione parallela non può far parte di nessun piano di query per una procedura nativa. Le procedure native sono sempre a thread singolo.


#### <a name="join-types"></a>Tipi di join


Hash join e merge join non possono far parte di nessun piano di query per una procedura nativa. Vengono usati join annidati dei cicli.


#### <a name="no-hash-aggregation"></a>Aggregazione di hash

Quando il piano di query per una procedura nativa richiede una fase di aggregazione, è disponibile solo l'aggregazione di flusso. L'aggregazione di hash non è supportata in un piano di query per un procedura nativa.

- L'aggregazione di hash è più efficace quando è necessario aggregare dati da un numero elevato di righe.



## <a name="f-application-design-transactions-and-retry-logic"></a>F. Progettazione di applicazioni: transazioni e logica di ripetizione dei tentativi

Una transazione che coinvolge una tabella ottimizzata per la memoria può diventare dipendente di un'altra transazione che coinvolge la stessa tabella. Se il numero di transazioni dipendenti raggiunge o supera il valore massimo consentito, tutte le transazioni dipendenti avranno esito negativo.

In SQL Server 2016:

- Il valore massimo consentito è di 8 transazioni dipendenti. 8 è anche il limite di transazioni di cui una determinata transazione può essere dipendente.
- Il numero del messaggio di errore è 41839. (In SQL Server 2014 il numero del messaggio di errore è 41301).


È possibile rendere gli script Transact-SQL più affidabili per evitare possibili errori di transazione aggiungendo la *logica ripetizione tentativi* agli script. La logica ripetizione tentativi è più utile quando le chiamate UPDATE e DELETE sono frequenti oppure se viene fatto riferimento alla tabella ottimizzata per la memoria da una chiave esterna in un'altra tabella. Per informazioni dettagliate, vedere:

- [Transactions with Memory-Optimized Tables](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)
- [Transaction dependency limits with memory optimized tables - Error 41839](/archive/blogs/sqlcat/transaction-dependency-limits-with-memory-optimized-tables-error-41839) (Limiti di dipendenza delle transazioni nelle tabelle con ottimizzazione per la memoria: errore 41839)



## <a name="related-links"></a>Collegamenti correlati

- [OLTP in memoria (ottimizzazione in memoria)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)