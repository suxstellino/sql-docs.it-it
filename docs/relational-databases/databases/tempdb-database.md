---
title: Database tempdb | Microsoft Docs
description: Questo argomento illustra i dettagli relativi alla configurazione e all'uso del database tempdb in SQL Server e nel database SQL di Azure.
ms.custom: P360
ms.date: 09/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- temporary tables [SQL Server], tempdb database
- tempdb database [SQL Server], about tempdb
- temporary stored procedures [SQL Server]
- tempdb database [SQL Server]
ms.assetid: ce4053fb-e37a-4851-b711-8e504059a780
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5dc33d8bd3abf6e204c0ab602bc25105d78dced9
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421963"
---
# <a name="tempdb-database"></a>database tempdb

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il database di sistema `tempdb` è una risorsa globale disponibile per tutti gli utenti connessi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Microsoft Azure. `tempdb` include:  
  
- *Oggetti utente* temporanei creati in modo esplicito. Tra questi vi sono tabelle e indici temporanei globali o locali, stored procedure temporanee, variabili di tabella, tabelle restituite in funzioni con valori di tabella e cursori.  
- *Oggetti interni* creati dal motore di database. e comprendono:
  - Tabelle di lavoro in cui archiviare i risultati intermedi di operazioni di spooling e di ordinamento e cursori, nonché in cui archiviare LOB (Large Object) temporanei.
  - File di lavoro per operazioni hash join o hash aggregate.
  - Risultati intermedi dell'ordinamento per operazioni quali la creazione o la ricompilazione di indici (se è specificato `SORT_IN_TEMPDB`) o per alcune query `GROUP BY`, `ORDER BY` o `UNION`.

  Ogni oggetto interno usa un minimo di nove pagine: una pagina IAM e un extent di otto pagine. Per altre informazioni sulle pagine e sugli extent, vedere [Pagine ed extent](../../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

  > [!IMPORTANT]
  > I database singoli e i pool elastici di database SQL di Azure supportano tabelle temporanee globali e stored procedure temporanee globali archiviate in `tempdb` e con ambito a livello di database. 
  >
  > Le tabelle temporanee globali e le stored procedure temporanee globali vengono condivise per le sessioni di tutti gli utenti all'interno dello stesso database SQL. Le sessioni utente da altri database SQL non possono accedere alle tabelle temporanee globali. Per altre informazioni, vedere [Tabelle temporanee globali con ambito database (database SQL di Azure)](../../t-sql/statements/create-table-transact-sql.md#database-scoped-global-temporary-tables-azure-sql-database). [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) supporta gli stessi oggetti temporanei di SQL Server.
  >
  > Per i database singoli e i pool elastici di database SQL di Azure si applicano solo il database master e il database `tempdb`. Per altre informazioni, vedere [Informazioni sul server di database SQL di Azure](/azure/sql-database/sql-database-servers-databases#what-is-an-azure-sql-database-server). Per una descrizione di `tempdb` nel contesto di database singoli e pool elastici di database SQL di Azure, vedere [Database tempdb nei database singoli e pool elastici di database SQL di Azure](#tempdb-database-in-sql-database). 
  >
  > Per Istanza gestita di SQL di Azure si applicano tutti i database di sistema.

- *Archivi delle versioni*, raccolte di pagine di dati che contengono le righe di dati che supportano le caratteristiche per il controllo delle versioni delle righe. Esistono due tipi di archivi delle versioni: uno comune e uno per la compilazione di indici online. Gli archivi delle versioni contengono:
  - versioni di riga generate dalle transazioni di modifica dei dati in un database in cui viene usato `READ COMMITTED` tramite isolamento del controllo delle versioni delle righe o transazioni di isolamento dello snapshot.  
  - Versioni di riga generate dalle transazioni di modifica dei dati per le caratteristiche, ad esempio le operazioni sugli indici online, la caratteristica MARS (Multiple Active Result Set) e i trigger `AFTER`.  
  
Le operazioni all'interno di `tempdb` sono a registrazione minima in modo che sia possibile eseguire il rollback delle transazioni. `tempdb` viene ricreato ogni volta che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviato in modo che il sistema inizi sempre con una copia pulita del database. Poiché le tabelle e le stored procedure temporanee vengono eliminate automaticamente al momento della disconnessione e poiché al momento della chiusura del sistema non vi sono connessioni attive, 

`tempdb` non ha mai nulla da salvare da una sessione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un'altra. Le operazioni di backup e ripristino non sono consentite in `tempdb`.  

## <a name="physical-properties-of-tempdb-in-sql-server"></a>Proprietà fisiche di tempdb in SQL Server

Nella tabella seguente sono elencati i valori iniziali di configurazione dei dati `tempdb` e dei file di log in SQL Server. I valori sono basati sulle impostazioni predefinite per il database `model`. Le dimensioni di questi file potrebbero variare leggermente a seconda dell'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|File|Nome logico|Nome fisico|Dimensioni iniziali|Aumento di dimensioni del file|  
|----------|------------------|-------------------|------------------|-----------------|  
|Dati primari|tempdev|tempdb.mdf|8 megabyte|Aumento automatico di 64 MB fino a quando il disco risulta pieno|  
|File di dati secondari|temp#|tempdb_mssql_#.ndf|8 megabyte|Aumento automatico di 64 MB fino a quando il disco risulta pieno|  
|File di log|templog|templog.ldf|8 megabyte|Aumento automatico di 64 megabyte fino a un massimo di 2 terabyte|  
  
Il numero di file di dati secondari dipende dal numero di processori (logici) del computer. In generale, se il numero di processori logici è minore o uguale a otto, usare un numero di file di dati pari al numero dei processori logici. Se il numero di processori logici è maggiore di otto, usare otto file di dati. Se la contesa persiste, aumentare il numero di file di dati per multipli di quattro fino a quando la contesa si riduce a livelli accettabili o modificare il carico di lavoro o il codice.

> [!NOTE]
> Il valore predefinito per il numero di file di dati si basa sulle linee guida generali in [KB 2154845](https://support.microsoft.com/kb/2154845/).  

> [!NOTE]
> Per controllare la dimensione e i parametri di crescita correnti per `tempdb` , visualizzazione query `tempdb.sys.database_files` .
  
### <a name="moving-the-tempdb-data-and-log-files-in-sql-server"></a>Spostamento dei file di dati e di log di tempdb in SQL Server

Per spostare i file di dati e di log di `tempdb`, vedere [Spostare i database di sistema](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options-for-tempdb-in-sql-server"></a>Opzioni di database per tempdb in SQL Server

Nella tabella seguente vengono elencati i valori predefiniti per ogni opzione di database del database `tempdb` ed è indicato se è possibile modificare le varie opzioni. Per visualizzare le impostazioni correnti di queste opzioni, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Opzione di database|Valore predefinito|Modificabile|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|Sì|  
|ANSI_NULL_DEFAULT|OFF|Sì|  
|ANSI_NULLS|OFF|Sì|  
|ANSI_PADDING|OFF|Sì|  
|ANSI_WARNINGS|OFF|Sì|  
|ARITHABORT|OFF|Sì|  
|AUTO_CLOSE|OFF|No|  
|AUTO_CREATE_STATISTICS|ON|Sì|  
|AUTO_SHRINK|OFF|No|  
|AUTO_UPDATE_STATISTICS|ON|Sì|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Sì|  
|CHANGE_TRACKING|OFF|No|  
|CONCAT_NULL_YIELDS_NULL|OFF|Sì|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Sì|  
|CURSOR_DEFAULT|GLOBAL|Sì|  
|Opzioni relative alla disponibilità del database|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|No<br /><br /> No<br /><br /> No|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Sì|  
|DB_CHAINING|ON|No|  
|ENCRYPTION|OFF|No|  
|MIXED_PAGE_ALLOCATION|OFF|No|  
|NUMERIC_ROUNDABORT|OFF|Sì|  
|PAGE_VERIFY|CHECKSUM per nuove installazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NONE per aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Sì|  
|PARAMETERIZATION|SEMPLICE|Sì|  
|QUOTED_IDENTIFIER|OFF|Sì|  
|READ_COMMITTED_SNAPSHOT|OFF|No|  
|RECOVERY|SEMPLICE|No|  
|RECURSIVE_TRIGGERS|OFF|Sì|  
|Opzioni relative a Service Broker|ENABLE_BROKER|Sì|  
|TRUSTWORTHY|OFF|No|  
  
Per una descrizione di queste opzioni di database, vedere [Opzioni ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="tempdb-database-in-sql-database"></a>Database tempdb nel database SQL

### <a name="tempdb-sizes-for-dtu-based-service-tiers"></a>Dimensioni di tempdb per i livelli di servizio basati su DTU

<!-- tempdb being larger for Basic and 50 eDTU pools than for 100-400 eDTU pools reflects actual config (historical reasons) --> 

|Obiettivo del livello di servizio|Dimensione massima del file di dati (GB) `tempdb`|Numero di file di dati `tempdb`|Dimensioni massime dati (GB) `tempdb`|
|---|---:|---:|---:|
|Basic|13,9|1|13,9|
|S0|13,9|1|13,9|
|S1|13,9|1|13,9|
|S2|13,9|1|13,9|
|S3|32|1|32
|S4|32|2|64|
|S6|32|3|96|
|S7|32|6|192|
|S9|32|12|384|
|S12|32|12|384|
|P1|13,9|12|166,7|
|P2|13,9|12|166,7|
|P4|13,9|12|166,7|
|P6|13,9|12|166,7|
|P11|13,9|12|166,7|
|P15|13,9|12|166,7|
|Pool elastici Basic (tutte le configurazioni DTU)|13,9|12|166,7|
|Pool elastici standard (50 eDTU)|13,9|12|166,7|
|Pool elastici standard (100 eDTU)|32|1|32|
|Pool elastici standard (200 eDTU)|32|2|64|
|Pool elastici standard (300 eDTU)|32|3|96|
|Pool elastici standard (400 eDTU)|32|3|96|
|Pool elastici standard (800 eDTU)|32|6|192|
|Pool elastici standard (1200 eDTU)|32|10|320|
|Pool elastici standard (1600-3000 eDTU)|32|12|384|
|Pool elastici Premium (tutte le configurazioni DTU)|13,9|12|166,7|
||||

### <a name="tempdb-sizes-for-vcore-based-service-tiers"></a>Dimensioni di tempdb per i livelli di servizio basati su vCore

Vedere [Limiti delle risorse basati su vCore](/azure/sql-database/sql-database-vcore-resource-limits).

## <a name="restrictions"></a>Restrizioni

Nel database `tempdb` non è possibile eseguire le operazioni seguenti:  
  
- Aggiunta di filegroup.
- Backup o ripristino del database.
- Modifica delle regole di confronto. Le regole di confronto predefinite corrispondono a quelle del server.
- Modifica del proprietario del database. `tempdb` è di proprietà di *sa*.
- Creazione di uno snapshot del database.
- Eliminazione del database.
- Eliminazione dell'utente *guest* dal database.
- Abilitazione dell'acquisizione dei dati delle modifiche.
- Partecipazione al mirroring del database.
- Rimozione del filegroup primario, del file di dati primario o del file di log.
- Ridenominazione del filegroup primario o del database.
- Esecuzione di `DBCC CHECKALLOC`.
- Esecuzione di `DBCC CHECKCATALOG`.
- Impostazione del database su `OFFLINE`.
- Impostazione del filegroup primario o del database su `READ_ONLY`.
  
## <a name="permissions"></a>Autorizzazioni

Qualsiasi utente può creare oggetti temporanei in `tempdb`. Gli utenti possono accedere solo ai propri oggetti, a meno che non ottengano ulteriori autorizzazioni. È possibile revocare l'autorizzazione di connessione a `tempdb` per impedire a un utente di usare `tempdb`. Non è consigliabile perché per alcune operazioni di routine è necessario usare `tempdb`.  

## <a name="optimizing-tempdb-performance-in-sql-server"></a>Ottimizzazione delle prestazioni di tempdb in SQL Server
Le dimensioni e la posizione fisica del database `tempdb` possono influire sulle prestazioni di un sistema. Se, ad esempio, le dimensioni definite per `tempdb` sono eccessivamente ridotte, il carico di elaborazione del sistema potrebbe essere in parte dovuto alla necessità di aumentare automaticamente le dimensioni di `tempdb` fino a raggiungere quelle necessarie per supportare il carico di lavoro a ogni riavvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Se possibile, usare l'[inizializzazione immediata dei file](../../relational-databases/databases/database-instant-file-initialization.md) per migliorare le prestazioni delle operazioni di aumento delle dimensioni dei file di dati.

Preallocare lo spazio per tutti i file di `tempdb` impostando le relative dimensioni su un valore adeguato per il carico di lavoro tipico nell'ambiente. La preallocazione evita che `tempdb` si espanda con una frequenza eccessiva, con effetti negativi sulle prestazioni. È opportuno impostare il database `tempdb` per l'aumento automatico delle dimensioni, per aumentare lo spazio su disco per le eccezioni non pianificate.

All'interno di ogni [filegroup](../../relational-databases/databases/database-files-and-filegroups.md#filegroups) i file di dati devono avere le stesse dimensioni, perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa un algoritmo di riempimento proporzionale che favorisce le allocazioni all'interno di file con maggiore spazio disponibile. La suddivisione di `tempdb` in più file di dati di dimensioni uguali garantisce un livello elevato di efficienza parallela nelle operazioni che usano `tempdb`.

Impostare un valore di aumento delle dimensioni del file tale da evitare aumenti troppo ridotti delle dimensioni dei file del database `tempdb`. Se l'aumento delle dimensioni dei file è troppo ridotto in confronto alla quantità di dati scritti nel database `tempdb`, `tempdb` potrebbe espandersi costantemente. Questo comportamento ha un impatto negativo sulle prestazioni.

Per controllare i parametri di dimensione e crescita correnti di `tempdb`, usare la query seguente:

```sql
 SELECT name AS FileName,
    size*1.0/128 AS FileSizeInMB,
    CASE max_size
        WHEN 0 THEN 'Autogrowth is off.'
        WHEN -1 THEN 'Autogrowth is on.'
        ELSE 'Log file grows to a maximum size of 2 TB.'
    END,
    growth AS 'GrowthValue',
    'GrowthIncrement' =
        CASE
            WHEN growth = 0 THEN 'Size is fixed.'
            WHEN growth > 0 AND is_percent_growth = 0
                THEN 'Growth value is in 8-KB pages.'
            ELSE 'Growth value is a percentage.'
        END
FROM tempdb.sys.database_files;
GO
```

Posizionare il database `tempdb` in un sottosistema di I/O veloce. In presenza di molti dischi collegati direttamente, utilizzare lo striping del disco. File singoli o gruppi di file di dati di `tempdb` non devono necessariamente trovarsi in dischi o spindle diversi, a meno che non si verifichino anche colli di bottiglia di I/O.

Posizionare il database `tempdb` in dischi diversi da quelli usati dai database utente.

## <a name="performance-improvements-in-tempdb-for-sql-server"></a>Miglioramenti delle prestazioni in tempdb per SQL Server
A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], le prestazioni di `tempdb` vengono ulteriormente ottimizzate nei modi seguenti:  
  
- Le tabelle temporanee e le variabili di tabella vengono memorizzate nella cache. La memorizzazione nella cache consente di eseguire molto rapidamente le operazioni di eliminazione e creazione degli oggetti temporanei. La memorizzazione nella cache riduce anche l'allocazione delle pagine e la contesa dei metadati.  
- Il protocollo di latch delle pagine di allocazione è stato migliorato per ridurre il numero di latch di `UP` (aggiornamento) usati.  
- L'overhead di registrazione per `tempdb` è stato ridotto per diminuire l'uso della larghezza di banda per le operazioni di I/O del disco nel file di log di `tempdb`.  
- Durante l'installazione di una nuova istanza vengono aggiunti più file di dati di `tempdb`. Questa attività può essere eseguita usando il nuovo controllo input dell'interfaccia utente nella sezione **Configurazione del motore di database** e il parametro della riga di comando `/SQLTEMPDBFILECOUNT`. Per impostazione predefinita, il programma di installazione aggiunge un numero di file di dati `tempdb` pari al numero di processori logici oppure a otto, a seconda di quale sia il valore inferiore.  
- Se ci sono più file di dati `tempdb`, le dimensioni di tutti i file aumentano contemporaneamente e della stessa quantità in base alle impostazioni di espansione specificate. Il [flag di traccia 1117](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) non è più necessario.  
- Tutte le allocazioni in `tempdb` usano extent uniformi. Il [flag di traccia 1118](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) non è più necessario.  
- Per il filegroup primario, la proprietà `AUTOGROW_ALL_FILES` è attivata e non può essere modificata.

Per ulteriori informazioni sui miglioramenti apportati alle prestazioni in `tempdb`, vedere l'articolo del blog [TEMPDB - Files and Trace Flags and Updates, Oh My!](/archive/blogs/sql_server_team/tempdb-files-and-trace-flags-and-updates-oh-my).

## <a name="memory-optimized-tempdb-metadata"></a>Metadati tempdb ottimizzati per la memoria
La contesa tra metadati in `tempdb` è tipicamente un collo di bottiglia per la scalabilità per molti carichi di lavoro in esecuzione su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce una nuova funzionalità che fa parte della famiglia di funzionalità [database in memoria](../in-memory-database.md): metadati tempdb ottimizzati per la memoria. 

Questa funzionalità rimuove in modo efficace questo collo di bottiglia e sblocca un nuovo livello di scalabilità per carichi di lavoro con utilizzo intensivo di tempdb. In [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] le tabelle di sistema coinvolte nella gestione dei metadati delle tabelle temporanee possono essere spostate in tabelle ottimizzate per la memoria non durevoli senza latch.

Guardare questo video di sette minuti per una panoramica su come e quando usare i metadati tempdb ottimizzati per la memoria:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-and-When-To-Memory-Optimized-TempDB-Metadata/player?WT.mc_id=dataexposed-c9-niner]


### <a name="configuring-and-using-memory-optimized-tempdb-metadata"></a>Configurazione e utilizzo dei metadati tempdb ottimizzati per la memoria

Per accettare questa nuova funzionalità, usare lo script seguente:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
```

Per rendere effettiva questa modifica della configurazione è necessario riavviare il servizio.

È possibile verificare se `tempdb` è ottimizzato per la memoria usando il comando T-SQL seguente:

```sql
SELECT SERVERPROPERTY('IsTempdbMetadataMemoryOptimized');
```

Se il server non viene avviato per qualsiasi motivo dopo aver abilitato i metadati `tempdb` ottimizzati per la memoria, è possibile ignorare la funzionalità avviando l'istanza di SQL Server con la [configurazione minima](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md) usando l'opzione di avvio **-f**. In questo modo sarà possibile disabilitare la funzionalità e riavviare SQL Server in modalità normale.

Per proteggere il server da potenziali condizioni di memoria insufficiente, è possibile associare `tempdb` a un [pool di risorse](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md). Questa operazione viene eseguita con il comando [`ALTER SERVER`](../../t-sql/statements/alter-server-configuration-transact-sql.md) anziché i passaggi che si seguono normalmente per associare un pool di risorse a un database.

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
```

Questa modifica richiede inoltre un riavvio per avere effetto, anche se i metadati tempdb ottimizzati per la memoria sono già abilitati.

### <a name="memory-optimized-tempdb-limitations"></a>Limitazioni degli elementi tempdb ottimizzati per la memoria

- L'attivazione o la disattivazione della funzionalità non avviene in modo dinamico. A causa delle modifiche intrinseche che devono essere apportate alla struttura del database `tempdb`, è necessario un riavvio per abilitare o disabilitare la funzionalità.

- È possibile che una singola transazione non possa accedere alle tabelle ottimizzate per la memoria in più di un database. Tutte le transazioni che interessano una tabella ottimizzata per la memoria in un database utente non saranno in grado di accedere alle viste di sistema `tempdb` nella stessa transazione. Se si tenta di accedere alle viste di sistema `tempdb` nella stessa transazione della tabella ottimizzata per la memoria in un database utente, viene visualizzato l'errore seguente:
    
  ```
  A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.
  ```
    
  Esempio:
    
  ```sql
  BEGIN TRAN;
  
  SELECT *
  FROM tempdb.sys.tables;  -----> Creates a user in-memory OLTP transaction in tempdb
  
  INSERT INTO <user database>.<schema>.<mem-optimized table>
  VALUES (1); ----> Tries to create a user in-memory OLTP transaction in the user database but will fail
  
  COMMIT TRAN;
  ```
    
- Poiché le query eseguite nelle tabelle ottimizzate per la memoria non supportano gli hint di blocco e isolamento, le query eseguite nelle viste di catalogo `tempdb` ottimizzate per la memoria non rispetteranno gli hint di blocco e isolamento. Come nelle altre viste del catalogo di sistema in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tutte le transazioni eseguite nelle viste di sistema saranno nell'isolamento `READ COMMITTED` (o in questo caso `READ COMMITTED SNAPSHOT`).

- Non è possibile creare [indici columnstore](../indexes/columnstore-indexes-overview.md) nelle tabelle temporanee quando sono abilitati i metadati `tempdb` ottimizzati per la memoria.

- A causa della limitazione per gli indici columnstore, l'uso della stored procedure di sistema `sp_estimate_data_compression_savings` con il parametro di compressione dei dati `COLUMNSTORE` o `COLUMNSTORE_ARCHIVE` non è supportato quando sono abilitati i metadati `tempdb` ottimizzati per la memoria.

> [!NOTE] 
> Queste limitazioni si applicano solo quando si fa riferimento alle visualizzazioni di sistema `tempdb`. È possibile creare una tabella temporanea nella stessa transazione quando si accede a una tabella ottimizzata per la memoria in un database utente, se necessario.

## <a name="capacity-planning-for-tempdb-in-sql-server"></a>Pianificazione delle capacità per tempdb in SQL Server
La determinazione delle dimensioni appropriate per `tempdb` in un ambiente di produzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dipende da molti fattori. Come precedentemente descritto, questi fattori includono il carico di lavoro esistente e le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usate. È consigliabile analizzare il carico di lavoro esistente eseguendo le attività seguenti in un ambiente di test di SQL Server:

- Attivare l'aumento automatico delle dimensioni di `tempdb`.
- Eseguire singole query o file di traccia del carico di lavoro e monitorare l'uso dello spazio da parte di `tempdb`.
- Eseguire operazioni di manutenzione degli indici, ad esempio la ricompilazione degli indici stessi e monitorare lo spazio occupato da `tempdb`.
- Usare i valori di utilizzo dello spazio dei passaggi precedenti per stimare l'utilizzo totale del carico di lavoro. Modificare questo valore per l'attività simultanea proiettata, quindi impostare le dimensioni di `tempdb` di conseguenza.

## <a name="monitoring-tempdb-use"></a>Monitoraggio dell'utilizzo di tempdb
L'esaurimento dello spazio su disco in `tempdb` può causare gravi rotture nell'ambiente di produzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Può anche impedire alle applicazioni in esecuzione di completare le operazioni. Per eseguire il monitoraggio dello spazio su disco usato dai file di `tempdb`, è possibile usare la vista a gestione dinamica (DMV, Dynamic Management View) [sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md):

```sql
 -- Determining the amount of free space in tempdb
SELECT SUM(unallocated_extent_page_count) AS [free pages],
  (SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by the version store
SELECT SUM(version_store_reserved_page_count) AS [version store pages used],
  (SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by internal objects
SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],
  (SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by user objects
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],
  (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]
FROM sys.dm_db_file_space_usage;
 ```

Per eseguire il monitoraggio dell'allocazione delle pagine o dell'attività di deallocazione in `tempdb` a livello di sessione o di attività, è possibile usare le DMV [sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) e [sys.dm_db_task_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md). Queste viste consentono di identificare le query, le tabelle temporanee o le variabili di tabella che usano una grande quantità di spazio su disco in `tempdb`. È possibile usare inoltre diversi contatori che consentono di monitorare lo spazio libero disponibile in `tempdb` e le risorse che stanno usando `tempdb`.

```sql
-- Obtaining the space consumed by internal objects in all currently running tasks in each session
SELECT session_id,
  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,
  SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count
FROM sys.dm_db_task_space_usage
GROUP BY session_id;

-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks
SELECT R2.session_id,
  R1.internal_objects_alloc_page_count
  + SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count
  + SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count
FROM sys.dm_db_session_space_usage AS R1
INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id
GROUP BY R2.session_id, R1.internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count;;
```

## <a name="related-content"></a>Contenuti correlati
- [Opzione SORT_IN_TEMPDB per gli indici](../../relational-databases/indexes/sort-in-TempDB-option-for-indexes.md)    
- [Database di sistema](../../relational-databases/databases/system-databases.md)    
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)    
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)    
- [Spostare file del database](../../relational-databases/databases/move-database-files.md)    
