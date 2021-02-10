---
title: Opzioni di ALTER DATABASE SET (Transact-SQL) | Microsoft Docs
description: Informazioni su come impostare le opzioni di database, ad esempio l'ottimizzazione automatica, la crittografia, Query Store in SQL Server e il database SQL di Azure.
ms.custom: ''
ms.date: 09/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- online database state [SQL Server]
- database options [SQL Server]
- emergency database state [SQL Server]
- databases [SQL Server], options
- read-only databases
- recovery models [SQL Server], switching
- ALTER DATABASE statement, SET options
- offline database state [SQL Server]
- snapshot isolation framework option
- checksums [SQL Server]
- Automatic tuning
- " Data Retention Policy"
- query plan regression correction
- auto_create_statistics
- auto_update_statistics
- Query Store options
ms.assetid: f76fbd84-df59-4404-806b-8ecb4497c9cc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current
ms.openlocfilehash: 37ea2f4896d68f4ae5a47b14025d54a344febdc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016317"
---
# <a name="alter-database-set-options-transact-sql"></a>Opzioni di ALTER DATABASE SET (Transact-SQL)

Imposta le opzioni del database in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Per altre opzioni di ALTER DATABASE, vedere [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Selezionare una delle schede seguenti per la sintassi, gli argomenti, i commenti, le autorizzazioni e gli esempi per la specifica versione di SQL in uso.

Per altre informazioni sulle convenzioni della sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!NOTE]
> L'impostazione di alcune opzioni con ALTER DATABASE potrebbe richiedere l'accesso esclusivo al database. Se l'istruzione ALTER DATABASE non viene completata in modo tempestivo, verificare se sono presenti altre sessioni nel database che bloccano la sessione ALTER DATABASE.

## <a name="select-a-product"></a>Selezionare un prodotto

Nella riga seguente selezionare il nome del prodotto a cui si è interessati. Verrà così visualizzato un contenuto diverso in questa pagina Web, appropriato per il prodotto selezionato.

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Database SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Il mirroring del database, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] e i livelli di compatibilità sono opzioni `SET` ma, poiché richiedono lunghe descrizioni, vengono illustrati in articoli distinti. Per altre informazioni, vedere [Mirroring del database ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md), [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md) e [Livello di compatibilità ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

Le configurazioni con ambito database vengono usate per impostare diverse configurazioni di database a livello di singolo database. Per altre informazioni, vedere [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

> [!NOTE]
> Molte opzioni SET di database possono essere configurate per la sessione corrente tramite [Istruzioni SET](../../t-sql/statements/set-statements-transact-sql.md) e spesso vengono configurate dalle applicazioni durante la connessione. Le opzioni SET a livello di sessione sostituiscono i valori **ALTER DATABASE SET**. Le opzioni di database descritte nelle sezioni seguenti sono valori che è possibile impostare per le sessioni che non prevedono esplicitamente altri valori di opzioni SET.

## <a name="syntax"></a>Sintassi

```syntaxsql
ALTER DATABASE { database_name | CURRENT }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}

<option_spec> ::=
{
    <accelerated_database_recovery>
  | <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <containment_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <external_access_option>
  | FILESTREAM ( <FILESTREAM_option> )
  | <HADR_options>
  | <mixed_page_allocation_option>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <remote_data_archive_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
}
;

<accelerated_database_recovery> ::=
{
    ACCELERATED_DATABASE_RECOVERY = { ON | OFF }
     [ ( PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name } ) ];
}

<auto_option> ::=
{
    AUTO_CLOSE { ON | OFF }
  | AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
   {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
   }
}

<change_tracking_option_list> ::=
{
   AUTO_CLEANUP = { ON | OFF }
 | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
}

<containment_option> ::=
   CONTAINMENT = { NONE | PARTIAL }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
  | CURSOR_DEFAULT { LOCAL | GLOBAL }
}

<database_mirroring_option>
  ALTER DATABASE Database Mirroring

<date_correlation_optimization_option> ::=
    DATE_CORRELATION_OPTIMIZATION { ON | OFF }
  
<db_encryption_option> ::=
    ENCRYPTION { ON | OFF | SUSPEND | RESUME }

<db_state_option> ::=
    { ONLINE | OFFLINE | EMERGENCY }

<db_update_option> ::=
    { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
    { SINGLE_USER | RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::=
    DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<external_access_option> ::=
{
    DB_CHAINING { ON | OFF }
  | TRUSTWORTHY { ON | OFF }
  | DEFAULT_FULLTEXT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | DEFAULT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | NESTED_TRIGGERS = { OFF | ON }
  | TRANSFORM_NOISE_WORDS = { OFF | ON }
  | TWO_DIGIT_YEAR_CUTOFF = { 1753, ..., 2049, ..., 9999 }
}

<FILESTREAM_option> ::=
{
    NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL
  | DIRECTORY_NAME = <directory_name>
}

<HADR_options> ::=
    ALTER DATABASE SET HADR

<mixed_page_allocation_option> ::=
    MIXED_PAGE_ALLOCATION { OFF | ON }

<parameterization_option> ::=
    PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
    QUERY_STORE
    {
          = OFF [ ( FORCED ) ] 
        | = ON [ ( <query_store_option_list> [,...n] ) ]
        | ( < query_store_option_list> [,...n] )
        | CLEAR [ ALL ]
    }
}

<query_store_option_list> ::=
{
      OPERATION_MODE = { READ_WRITE | READ_ONLY }
    | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
    | DATA_FLUSH_INTERVAL_SECONDS = number
    | MAX_STORAGE_SIZE_MB = number
    | INTERVAL_LENGTH_MINUTES = number
    | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
    | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
    | MAX_PLANS_PER_QUERY = number
    | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
    | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
      STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<recovery_option> ::=
{
    RECOVERY { FULL | BULK_LOGGED | SIMPLE }
  | TORN_PAGE_DETECTION { ON | OFF }
  | PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }
}

<remote_data_archive_option> ::=
{
    REMOTE_DATA_ARCHIVE =
    {
        ON ( SERVER = <server_name> ,
             { 
                  CREDENTIAL = <db_scoped_credential_name>
                  | FEDERATED_SERVICE_ACCOUNT = ON | OFF
             }
        )
        | OFF
    }
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | DISABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
  | HONOR_BROKER_PRIORITY { ON | OFF}
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT { ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = { ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<target_recovery_time_option> ::=
    TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }

<termination>::=
{
    ROLLBACK AFTER number [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention> ::=
    TEMPORAL_HISTORY_RETENTION { ON | OFF }

<data_retention_policy> ::=
    DATA_RETENTION { ON | OFF }

```

## <a name="arguments"></a>Argomenti

*database_name*     
Nome del database da modificare.

CURRENT     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Esegue l'azione nel database corrente. `CURRENT` non è supportato per tutte le opzioni in tutti i contesti. In caso di errore di `CURRENT`, specificare il nome del database.

**\<accelerated_database_recovery> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Abilita il [ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-management.md) (ADR, Accelerated Database Recovery) per ogni database. Per impostazione predefinita, ADR è impostato su OFF in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. Usando questa sintassi è possibile designare un filegroup specifico per i dati dell'archivio versioni permanente (PVS, Persistent Version Store). Se non è specificato alcun filegroup, per l'archivio versioni permanente viene usato il filegroup PRIMARY. Per altre informazioni ed esempi, vedere [Ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-management.md).

**\<auto_option> ::=**      
Consente di controllare le opzioni automatiche.

<a name="auto_close"></a> AUTO_CLOSE { ON | **OFF** }     
ON     
Il database viene chiuso normalmente e le relative risorse vengono rilasciate dopo la disconnessione dell'ultimo utente.

Il database viene riaperto automaticamente quando un utente tenta di usarlo nuovamente, Ad esempio, questo comportamento di riapertura si verifica quando un utente rilascia un'istruzione `USE database_name`. Se l'opzione AUTO_CLOSE è impostata su ON, è possibile che il database venga arrestato correttamente. In questo caso, non viene riaperto finché un utente non prova a usarlo al successivo riavvio del [!INCLUDE[ssDE](../../includes/ssde-md.md)].

OFF     
Il database rimane aperto dopo la disconnessione dell'ultimo utente.

L'opzione AUTO_CLOSE è utile per i database desktop perché consente di gestire i file di database come normali file. I file possono essere spostati, copiati per creare backup o anche inviati tramite posta elettronica ad altri utenti. Il processo AUTO_CLOSE è asincrono. Operazioni ripetute di apertura e chiusura del database non comportano una riduzione delle prestazioni.

> [!NOTE]
> L'opzione AUTO_CLOSE non è disponibile in un database indipendente o nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_close_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsAutoClose` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).
>
> Se l'opzione AUTO_CLOSE è impostata su ON, alcune colonne nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) e la funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) restituiranno NULL perché il database non è disponibile per il recupero dei dati. Per risolvere questo problema, eseguire un'istruzione USE per aprire il database.
>
> Per il mirroring del database è necessario che AUTO_CLOSE sia OFF.

Quando il database è impostato su `AUTOCLOSE = ON`, un'operazione che ne avvia un arresto automatico cancella la cache dei piani per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La cancellazione della cache dei piani comporta la ricompilazione di tutti i piani di esecuzione successivi e può causare un peggioramento improvviso e temporaneo delle prestazioni di esecuzione delle query. A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2, per ogni archivio cache cancellato nella cache dei piani, il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene il messaggio informativo seguente: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Questo messaggio viene registrato ogni cinque minuti per tutta la durata dello scaricamento della cache.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
ON     
Query Optimizer crea statistiche per colonne singole nei predicati di query, in base alle esigenze, per migliorare i piani di query e le prestazioni di esecuzione delle query. Queste statistiche per colonne singole vengono create quando Query Optimizer compila le query. Vengono create solo sulle colonne che non sono già le prime di un oggetto statistiche esistente.

L'impostazione predefinita è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

OFF     
Query Optimizer non crea statistiche sulle singole colonne nei predicati di query durante la compilazione delle query. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_create_stats_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoCreateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione "Opzioni relative alle statistiche nel database" in [Statistiche](../../relational-databases/statistics/statistics.md).

INCREMENTAL = ON | **OFF**     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Imposta AUTO_CREATE_STATISTICS su ON e INCREMENTAL su ON. Questa impostazione crea automaticamente statistiche incrementali ogni volta che sono supportate. Il valore predefinito è OFF. Per altre informazioni, vedere [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ON     
I file di database vengono compattati periodicamente, se necessario.

È possibile compattare automaticamente sia i file di dati sia i file di log. AUTO_SHRINK riduce le dimensioni del log delle transazioni solo se per il database è impostato il modello di recupero con registrazione minima oppure se viene eseguito il backup del log. Quando si imposta AUTO_SHRINK su OFF, i file di database non vengono compattati automaticamente durante i controlli periodici per verificare la presenza di spazio inutilizzato.

L'opzione AUTO_SHRINK compatta i file quando più del 25% dello spazio del file risulta inutilizzato. Compatta il file in una delle due dimensioni (a seconda del valore maggiore):

- La dimensione in cui il 25% del file è costituito da spazio inutilizzato
- La dimensione del file quando è stato creato

Non è possibile compattare un database di sola lettura.

OFF     
I file di database non vengono compattati automaticamente durante i controlli periodici per la presenza di spazio inutilizzato.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_shrink_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoShrink` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> L'opzione AUTO_SHRINK non è disponibile in un database indipendente.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ON     
Specifica che Query Optimizer aggiorna le statistiche quando queste vengono usate da una query e quando potrebbero non essere aggiornate. Le statistiche diventano obsolete in seguito a operazioni di inserimento, aggiornamento, eliminazione o unione che modificano la distribuzione dei dati nella tabella o nella vista indicizzata. Query Optimizer determina che le statistiche potrebbero non essere aggiornate contando il numero di modifiche apportate ai dati dopo l'ultimo aggiornamento delle statistiche e confrontando il numero di modifiche con una soglia. basata sul numero di righe nella tabella o nella vista indicizzata.

Query Optimizer controlla la presenza di statistiche non aggiornate prima di compilare una query e di eseguire un piano di query memorizzato nella cache. Usa le colonne, le tabelle e le viste indicizzate nel predicato di query per determinare quali statistiche potrebbero non essere aggiornate. Query Optimizer verifica questa possibilità prima di compilare una query. Prima di eseguire un piano di query memorizzato nella cache, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica che tale piano faccia riferimento alle statistiche aggiornate.

L'opzione AUTO_UPDATE_STATISTICS_ASYNC si applica alle statistiche create per indici, colonne singole nei predicati di query e statistiche create con l'istruzione CREATE STATISTICS. Questa opzione si applica anche alle statistiche filtrate.

Il valore predefinito è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

Usare l'opzione AUTO_UPDATE_STATISTICS_ASYNC per specificare se le statistiche vengono aggiornate in modo sincrono o asincrono.

OFF     
Specifica che Query Optimizer non aggiorna le statistiche quando queste vengono usate da una query né quando potrebbero essere non aggiornate. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_update_stats_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoUpdateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione "Opzioni relative alle statistiche nel database" in [Statistiche](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ON     
Specifica che gli aggiornamenti delle statistiche per l'opzione AUTO_UPDATE_STATISTICS sono asincroni. Query Optimizer non attende il completamento degli aggiornamenti delle statistiche per compilare le query.

L'impostazione di questa opzione su ON non produce alcun effetto, a meno che AUTO_UPDATE_STATISTICS non sia impostata su ON.

L'impostazione predefinita dell'opzione AUTO_UPDATE_STATISTICS_ASYNC è OFF e Query Optimizer aggiorna le statistiche in modo sincrono.

OFF     
Specifica che gli aggiornamenti delle statistiche per l'opzione AUTO_UPDATE_STATISTICS sono sincroni. Query Optimizer attende il completamento degli aggiornamenti delle statistiche per compilare le query.

> [!NOTE]
> L'impostazione di questa opzione su OFF non produce alcun effetto, a meno che AUTO_UPDATE_STATISTICS non sia impostata su ON.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_update_stats_async_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Per altre informazioni su quando usare gli aggiornamenti delle statistiche sincroni o asincroni, vedere la sezione sulle opzioni relative alle statistiche in [Statistiche](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)])

Abilita o disabilita l'opzione di `FORCE_LAST_GOOD_PLAN` [Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
ON     
Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] forza automaticamente l'ultimo piano valido noto nelle query [!INCLUDE[tsql-md](../../includes/tsql-md.md)] nel caso in cui il nuovo piano di query causi un peggioramento delle prestazioni. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continua a monitorare le prestazioni delle query [!INCLUDE[tsql-md](../../includes/tsql-md.md)] usando il piano forzato.

Se si rilevano miglioramenti delle prestazioni, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continuerà a usare l'ultimo piano adeguato noto. Se non si rilevano miglioramenti delle prestazioni, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] creerà un nuovo piano di query. L'istruzione avrà esito negativo se Query Store non è abilitato o se non è in modalità di *lettura/scrittura*.

OFF     
Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] segnala potenziali peggioramenti delle prestazioni di query causati da modifiche apportate al piano di query nella vista [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). I consigli qui segnalati non vengono tuttavia applicati automaticamente. È possibile visualizzare i consigli attivi e risolvere i problemi identificati applicando gli script [!INCLUDE[tsql-md](../../includes/tsql-md.md)] disponibili nella vista. Il valore predefinito è OFF.

**\<change_tracking_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSFull](../../includes/sssds-md.md)]

Controlla le opzioni di rilevamento delle modifiche. È possibile abilitare il rilevamento delle modifiche, impostare le opzioni, modificare le opzioni e disabilitare il rilevamento delle modifiche. Per alcuni esempi, vedere la sezione "Esempi" più avanti in questo articolo.

ON     
Abilita il rilevamento delle modifiche per il database. Quando si abilita il rilevamento delle modifiche, è possibile impostare anche le opzioni AUTO CLEANUP e CHANGE RETENTION.

AUTO_CLEANUP = { ON | **OFF** }     
ON     
Le informazioni sul rilevamento delle modifiche vengono rimosse automaticamente una volta trascorso il periodo di memorizzazione specificato.

OFF     
I dati relativi al rilevamento delle modifiche non vengono rimossi automaticamente dal database.

CHANGE_RETENTION = *retention_period* { **DAYS** | HOURS | MINUTES }     
Specifica il periodo minimo di conservazione delle informazioni sul rilevamento delle modifiche nel database. I dati vengono rimossi solo quando il valore di AUTO_CLEANUP è ON.

*retention_period* è un numero intero che specifica il componente numerico del periodo di memorizzazione.

Il periodo di conservazione predefinito è **2 giorni**. Il periodo di memorizzazione minimo è 1 minuto. Il tipo di conservazione predefinito è **DAYS**.

OFF     
Disabilita il rilevamento delle modifiche per il database. Prima di disabilitare il rilevamento delle modifiche per il database, disabilitarlo per tutte le tabelle.

**\<containment_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Consente di controllare le opzioni di indipendenza del database.

CONTAINMENT = { **NONE** | PARTIAL}     
NONE     
Il database non è un database indipendente.

PARTIAL     
Il database è un database indipendente. L'impostazione dell'indipendenza del database su PARTIAL restituisce un errore se per il database è abilitato Change Data Capture, la replica o il rilevamento delle modifiche. Il controllo degli errori viene arrestato dopo un errore. Per altre informazioni sui database indipendenti, vedere [Contained Databases](../../relational-databases/databases/contained-databases.md).

**\<cursor_option> ::=**     

Consente di controllare le opzioni del cursore.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ON     
Quando si esegue il commit o il rollback di una transazione, eventuali cursori aperti vengono chiusi.

OFF     
I cursori rimangono aperti quando viene eseguito il commit di una transazione. Quando si esegue il rollback di una transazione vengono chiusi tutti i cursori, ad eccezione di quelli definiti come INSENSITIVE o STATIC.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per CURSOR_CLOSE_ON_COMMIT. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta CURSOR_CLOSE_ON_COMMIT su OFF per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_cursor_close_on_commit_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsCloseCursorsOnCommitEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

CURSOR_DEFAULT { LOCAL | GLOBAL }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Determina se l'ambito del cursore è LOCAL o GLOBAL.

LOCAL     
Se si specifica LOCAL e non si definisce un cursore come GLOBAL al momento della creazione, l'ambito del cursore è locale. In particolare, l'ambito è locale rispetto al batch, alla stored procedure o al trigger in cui il cursore è stato creato. Il nome del cursore è valido solo in questo ambito.

È possibile fare riferimento al cursore tramite variabili di cursore locali nel batch, nella stored procedure o nel trigger oppure tramite un parametro OUTPUT di stored procedure. Il cursore viene deallocato in modo implicito al termine dell'esecuzione del batch, della stored procedure o del trigger, a meno che non sia stato passato a un parametro OUTPUT. Il cursore potrebbe essere passato di nuovo a un parametro OUTPUT. In questo caso, viene deallocato quando l'ultima variabile che vi fa riferimento viene deallocata o esce dall'ambito.

GLOBAL     
Se si specifica GLOBAL e se un cursore non viene definito come LOCAL al momento della creazione, l'ambito del cursore è globale rispetto alla connessione. È possibile fare riferimento al nome del cursore in qualsiasi stored procedure o batch eseguiti tramite la connessione.

Il cursore viene deallocato in modo implicito soltanto al momento della disconnessione. Per altre informazioni, vedere [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_local_cursor_default` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsLocalCursorsDefault` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<data_retention_policy> ::=**

**Si applica a**: solo *SQL Edge di Azure*

DATA_RETENTION { ON | OFF }   
ON    
Abilita la pulizia basata sui criteri di conservazione dei dati in un database.

OFF   
Disabilita la pulizia basata sui criteri di conservazione dei dati in un database.

**\<database_mirroring>**     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Per le descrizioni dell'argomento, vedere [Mirroring del database ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md).

**\<date_correlation_optimization_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controlla l'opzione date_correlation_optimization.

DATE_CORRELATION_OPTIMIZATION { ON | **OFF** }     
ON     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene statistiche di correlazione in cui un vincolo FOREIGN KEY collega due tabelle qualsiasi nel database e le tabelle hanno colonne **datetime**.

OFF     
Non vengono mantenute statistiche di correlazione.

Per impostare DATE_CORRELATION_OPTIMIZATION su ON, è necessario che non siano presenti connessioni attive al database, ad eccezione della connessione che esegue l'istruzione ALTER DATABASE. Successivamente, sono supportate più connessioni.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_date_correlation_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<db_encryption_option> ::=**      
Controlla lo stato della crittografia del database.

ENCRYPTION { ON \| **OFF** \| SUSPEND \| RESUME }     
ON     
Imposta il database in modo che venga crittografato.

OFF     
Imposta il database in modo che non venga crittografato.

SUSPEND     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])     

Consente di sospendere l'analisi della crittografia dopo l'abilitazione o la disabilitazione di Transparent Data Encryption oppure dopo la modifica della chiave di crittografia.

RESUME     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Consente di riprendere l'analisi della crittografia precedentemente sospesa.

Per altre informazioni sulla crittografia del database, vedere [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) e [Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Quando la crittografia è abilitata a livello di database, tutti i filegroup vengono crittografati. I nuovi filegroup erediteranno la proprietà di crittografia. Se in un database sono presenti filegroup impostati su READ ONLY, l'operazione di crittografia del database avrà esito negativo.

È possibile vedere lo stato di crittografia del database, così come lo stato dell'analisi di crittografia, usando la DMV [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_state_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controlla lo stato del database.

OFFLINE     
Il database viene chiuso normalmente e contrassegnato come offline. Mentre è offline, il database non può essere modificato.

ONLINE     
Il database è aperto e disponibile per l'utilizzo.

EMERGENCY     
Il database è contrassegnato come READ_ONLY, la registrazione è disabilitata e l'accesso è limitato ai soli membri del ruolo predefinito del server sysadmin. L'opzione EMERGENCY viene usata principalmente per attività di risoluzione dei problemi. Ad esempio, è possibile impostare lo stato EMERGENCY per un database contrassegnato come sospetto a causa di un file di log danneggiato. Con questa impostazione, l'amministratore di sistema potrà accedere in sola lettura al database. Solo i membri del ruolo predefinito del server sysadmin possono impostare lo stato EMERGENCY per un database.

È richiesta l'autorizzazione `ALTER DATABASE` per il database dell'area di interesse, per impostare il database sullo stato offline o emergency, nonché l'autorizzazione `ALTER ANY DATABASE` a livello di server per portare un database da offline a online.

È possibile determinare lo stato di questa opzione esaminando le colonne `state` e `state_desc` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `Status` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Per altre informazioni, vedere [Stati del database](../../relational-databases/databases/database-states.md).

Un database contrassegnato come RESTORING non può essere impostato su OFFLINE, ONLINE o EMERGENCY. Lo stato RESTORING può essere impostato durante un'operazione di ripristino attiva o quando un'operazione di ripristino di un database o di un file di log ha esito negativo a causa di un file di backup danneggiato.

**\<db_update_option> ::=**

Indica se sono consentiti aggiornamenti nel database.

READ_ONLY     
Gli utenti possono leggere i dati dal database, ma non modificarli.

> [!NOTE]
> Per migliorare le prestazioni di esecuzione delle query, aggiornare le statistiche prima di impostare un database su READ_ONLY. Se dopo che un database viene impostato su READ_ONLY sono necessarie statistiche aggiuntive, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] verranno create statistiche in tempdb. Per altre informazioni sulle statistiche per un database di sola lettura, vedere [Statistiche](../../relational-databases/statistics/statistics.md).

READ_WRITE     
Il database è disponibile per operazioni di lettura e scrittura.

Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database. Per altre informazioni, vedere la clausola SINGLE_USER.

> [!NOTE]
> Nei database federati di [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], `SET { READ_ONLY | READ_WRITE }` è disabilitato.

**\<db_user_access_option> ::=**      
Controlla l'accesso degli utenti al database.

SINGLE_USER     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Specifica che l'accesso al database è consentito a un solo utente alla volta. Se si specifica SINGLE_USER e al database sono connessi altri utenti, l'istruzione ALTER DATABASE viene bloccata finché tutti gli utenti non si disconnettono dal database specificato. Per eseguire l'override di questo comportamento, vedere la clausola WITH \<termination>.

Il database rimane in modalità SINGLE_USER anche se l'utente che ha impostato l'opzione si disconnette. A questo punto, un altro utente (ma solo uno) potrà connettersi al database.

Prima di impostare il database in modalità SINGLE_USER, verificare che l'opzione AUTO_UPDATE_STATISTICS_ASYNC sia impostata su OFF. Se l'opzione è impostata su ON, il thread in background usato per aggiornare le statistiche stabilisce una connessione con il database che non sarà quindi accessibile in modalità utente singolo. Per visualizzare lo stato di questa opzione, eseguire una query sulla colonna `is_auto_update_stats_async_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Se l'opzione è impostata su ON, effettuare le operazioni seguenti:

1. Impostare AUTO_UPDATE_STATISTICS_ASYNC su OFF.

2. Verificare la presenza di processi asincroni attivi relativi alle statistiche eseguendo una query sulla DMV [sys.dm_exec_background_job_queue](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md).

Se sono presenti processi attivi, consentire il completamento di tali processi o terminarli manualmente usando [KILL STATS JOB](../../t-sql/language-elements/kill-stats-job-transact-sql.md).

RESTRICTED_USER     
Consente la connessione al database solo ai membri del ruolo predefinito del database `db_owner` e ai membri dei ruoli predefiniti del server `dbcreator` e `sysadmin`. senza tuttavia imporre un limite al numero di connessioni. Tutte le connessioni al database vengono interrotte entro l'intervallo di tempo specificato nella clausola di terminazione dell'istruzione ALTER DATABASE. Dopo l'impostazione dello stato RESTRICTED_USER per il database, qualsiasi tentativo di connessione da parte di utenti non qualificati viene rifiutato.

MULTI_USER     
Consente la connessione al database a tutti gli utenti che dispongono di autorizzazioni appropriate.

È possibile determinare lo stato di questa opzione esaminando la colonna `user_access` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `UserAccess` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

Determina se le transazioni sottoposte a commit sono completamente durevoli o durevoli posticipate.

DISABLED     
Tutte le transazioni in cui viene usato `SET DISABLED` sono completamente durevoli. Tutte le opzioni di durabilità impostate in un blocco atomico o in un'istruzione COMMIT vengono ignorate.

ALLOWED     
Tutte le transazioni in cui viene usato `SET ALLOWED` sono completamente durevoli o durevoli posticipate, a seconda del set di opzioni di durabilità nel blocco atomico o nell'istruzione COMMIT.

FORCED     
Tutte le transazioni in cui viene usato `SET FORCED` sono durevoli posticipate. Tutte le opzioni di durabilità impostate in un blocco atomico o in un'istruzione COMMIT vengono ignorate.

**\<external_access_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Determina se è consentito l'accesso al database da parte di risorse esterne, ad esempio oggetti di un altro database.

DB_CHAINING { ON | **OFF** }     
ON     
Il database può essere l'origine o la destinazione di una catena di proprietà tra database.

OFF     
Il database non può partecipare al concatenamento della proprietà tra database.

> [!IMPORTANT]
> Questa impostazione viene riconosciuta dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quando l'opzione del server cross db ownership chaining è impostata su 0 (OFF). Quando cross db ownership chaining è 1 (ON), tutti i database utente possono partecipare ai concatenamenti della proprietà tra database, a prescindere dal valore di questa opzione. Questa opzione viene impostata tramite [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

Per impostare questa opzione, è necessaria l'autorizzazione `CONTROL SERVER` nel database.

Non è possibile impostare l'opzione DB_CHAINING per i database di sistema master, model e tempdb.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_db_chaining_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

TRUSTWORTHY { ON | **OFF** }     
ON     
I moduli di database, ad esempio stored procedure o funzioni definite dall'utente, che usano un contesto di rappresentazione, possono accedere a risorse esterne al database.

OFF     
I moduli di database in un contesto di rappresentazione non possono accedere a risorse esterne al database.

L'opzione TRUSTWORTHY viene impostata su OFF ogni volta che il database viene collegato.

Per impostazione predefinita, in tutti i database di sistema, a eccezione del database msdb, TRUSTWORTHY è impostata su OFF. Il valore non può essere cambiato per i database model e tempdb. È consigliabile evitare di impostare l'opzione TRUSTWORTHY su ON per il database master.

Per impostare questa opzione, è necessaria l'autorizzazione `CONTROL SERVER` nel database.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_trustworthy_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

DEFAULT_FULLTEXT_LANGUAGE     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Consente di specificare il valore della lingua predefinita per le colonne con indicizzazione full-text.

> [!IMPORTANT]
> Questa opzione è consentita solo quando l'opzione CONTAINMENT è stata impostata su PARTIAL. Se l'opzione CONTAINMENT è impostata su NONE, si verificheranno errori.

DEFAULT_LANGUAGE     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Specifica la lingua predefinita per tutti i nuovi account di accesso creati. È possibile specificare la lingua indicando l'ID locale (lcid), il nome della lingua o l'alias di lingua. Per un elenco dei nomi e degli alias di lingua accettabili, vedere [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). Questa opzione è consentita solo quando l'opzione CONTAINMENT è stata impostata su PARTIAL. Se l'opzione CONTAINMENT è impostata su NONE, si verificheranno errori.

NESTED_TRIGGERS     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Specifica se un trigger AFTER supporta la propagazione, ovvero un'azione che avvia un altro trigger, che a sua volta ne avvia un altro e così via. Questa opzione è consentita solo quando l'opzione CONTAINMENT è stata impostata su PARTIAL. Se l'opzione CONTAINMENT è impostata su NONE, si verificheranno errori.

TRANSFORM_NOISE_WORDS     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Consente di eliminare un messaggio di errore visualizzato nel caso in cui parole non significative impediscono l'esecuzione di un'operazione booleana in una query full-text. Questa opzione è consentita solo quando l'opzione CONTAINMENT è stata impostata su PARTIAL. Se l'opzione CONTAINMENT è impostata su NONE, si verificheranno errori.

TWO_DIGIT_YEAR_CUTOFF     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Specifica un numero intero compreso tra 1753 e 9999 che rappresenta l'anno di cambio data per l'interpretazione degli anni a due cifre come anni a quattro cifre. Questa opzione è consentita solo quando l'opzione CONTAINMENT è stata impostata su PARTIAL. Se l'opzione CONTAINMENT è impostata su NONE, si verificheranno errori.

**\<FILESTREAM_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Consente di controllare le impostazioni per le tabelle FileTable.

NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }     
OFF     
L'accesso non transazionale ai dati delle tabelle FileTable è disabilitato.

READ_ONLY     
I dati FILESTREAM nelle tabelle FileTable in questo database possono essere letti da processi non transazionali.

FULL     
Abilita l'accesso non transazionale completo a dati di FILESTREAM nelle tabelle FileTable.

DIRECTORY_NAME = *\<directory_name>*      
Nome di directory compatibile con Windows. Il nome deve essere univoco in tutti i nomi di directory a livello di database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il confronto di univocità non supporta la distinzione tra maiuscole e minuscole, indipendentemente dalle impostazioni delle regole di confronto. È necessario impostare questa opzione prima di creare una tabella FileTable nel database.

**\<HADR_options> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Vedere [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md).

**\<mixed_page_allocation_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Controlla se il database può creare pagine iniziali usando un extent misto per le prime otto pagine di un indice o di una tabella.

MIXED_PAGE_ALLOCATION { OFF | **ON** }     
OFF     
Il database crea sempre le pagine iniziali usando extent uniformi. OFF è il valore predefinito.

ON     
Il database crea sempre le pagine iniziali usando extent misti.

Questa opzione è impostata su ON per tutti i database di sistema. **tempdb** è l'unico database di sistema che supporta l'opzione OFF.

**\<PARAMETERIZATION_option> ::=**      
Consente di controllare l'opzione di parametrizzazione. Per altre informazioni sulla parametrizzazione, vedere [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).

PARAMETERIZATION { **SIMPLE** | FORCED }     
SEMPLICE     
Le query vengono parametrizzate in base al comportamento predefinito del database.

FORCED     
Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguita la parametrizzazione di tutte le query nel database.

L'impostazione corrente di questa opzione può essere determinata esaminando `is_parameterization_forced column` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

ON | **OFF** [ ( FORCED )  ] | CLEAR [ ALL ]     
Verifica se Query Store è abilitato nel database e controlla anche la rimozione dei contenuti di Query Store. Per altre informazioni, vedere [Scenari di utilizzo di Query Store](../../relational-databases/performance/query-store-usage-scenarios.md).

ON     
Abilita Query Store.

OFF [ ( FORCED ) ]      
Disabilita Query Store. OFF è il valore predefinito. FORCED è facoltativo. FORCED interrompe tutte le attività in background di Query Store e ignora lo scaricamento sincrono quando Query Store è disattivato. Determina la chiusura di Query Store nel minor tempo possibile. FORCED si applica a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU14, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU21, [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU6 e build successive.

> [!NOTE]  
> Non è possibile disabilitare Query Store in database singolo [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e in un pool elastico. L'esecuzione di `ALTER DATABASE [database] SET QUERY_STORE = OFF` restituirà l'avviso `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

CLEAR [ ALL ]     
Rimuove i dati correlati alla query da Query Store. ALL è facoltativo. Rimuove i dati e i metadati correlati alla query da Query Store.

OPERATION_MODE { READ_ONLY | READ_WRITE }     
Descrive la modalità operativa di Query Store.

READ_WRITE     
Query Store raccoglie e salva in modo permanente le informazioni sulle statistiche di esecuzione di runtime e i piani di query.

READ_ONLY     
Le informazioni possono essere lette da Query Store, ma non vengono aggiunte nuove informazioni. Se lo spazio massimo allocato di Query Store viene esaurito, la modalità operativa di Query Store passa a READ_ONLY.

CLEANUP_POLICY     
Descrive i criteri di conservazione dei dati di Query Store. STALE_QUERY_THRESHOLD_DAYS determina il numero di giorni per cui le informazioni per una query vengono conservate in Query Store. STALE_QUERY_THRESHOLD_DAYS è di tipo **bigint**. Il valore predefinito è 30.

DATA_FLUSH_INTERVAL_SECONDS     
Determina la frequenza con cui i dati scritti in Query Store vengono salvati in modo permanente sul disco. Per ottimizzare le prestazioni, i dati raccolti da Query Store vengono scritti in modo asincrono sul disco. La frequenza con cui si verifica questo trasferimento asincrono viene configurata tramite l'argomento DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS è di tipo **bigint**. Il valore predefinito **900** (15 min).

MAX_STORAGE_SIZE_MB     
Determina lo spazio allocato a Query Store. MAX_STORAGE_SIZE_MB è di tipo **bigint**. Il valore predefinito è **100 MB** per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il valore predefinito è **1 GB**.

> [!NOTE]
> Il limite `MAX_STORAGE_SIZE_MB` non è necessariamente applicato. Le dimensioni di archiviazione vengono controllate solo quando Query Store scrive i dati su disco. Questo intervallo viene impostato dall'opzione `DATA_FLUSH_INTERVAL_SECONDS` o dall'opzione **Intervallo di scaricamento dati** della finestra di dialogo Query Store di [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. Il valore predefinito dell'intervallo è 900 secondi (o 15 minuti).
> Se Query Store ha violato il limite `MAX_STORAGE_SIZE_MB` tra i controlli delle dimensioni di archiviazione, passa alla modalità di sola lettura. Se è abilitata la `SIZE_BASED_CLEANUP_MODE`, viene attivato anche il meccanismo di pulizia per applicare il limite `MAX_STORAGE_SIZE_MB`.
> Dopo la cancellazione di spazio sufficiente, la modalità di Query Store cambierà automaticamente in lettura/scrittura.

> [!IMPORTANT]
> Se si ritiene che per l'acquisizione del carico di lavoro siano necessari più di 10 GB di spazio su disco, è probabile che sia opportuno ripensare e ottimizzare il carico di lavoro in modo da riusare i piani di query (ad esempio, usando la [parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) oppure modificare le configurazioni di Query Store.    
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], è possibile impostare `QUERY_CAPTURE_MODE` su CUSTOM per un controllo aggiuntivo sui criteri di acquisizione delle query.

INTERVAL_LENGTH_MINUTES     
Determina l'intervallo di tempo in base a cui vengono aggregati i dati delle statistiche di esecuzione di runtime in Query Store. Per ottimizzare l'utilizzo dello spazio, le statistiche di esecuzione di runtime nell'archivio di statistiche di runtime vengono aggregate in un intervallo di tempo fisso. L'intervallo di tempo predefinito viene configurato tramite l'argomento INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES è di tipo **bigint**. Il valore predefinito è **60**.

SIZE_BASED_CLEANUP_MODE { **AUTO** | OFF }     
Determina se la pulizia deve essere attivata automaticamente quando la quantità totale dei dati ha quasi raggiunto le dimensioni massime.

AUTO     
La pulizia basata sulle dimensioni viene attivata automaticamente quando le dimensioni sul disco raggiungono il 90% di **MAX_STORAGE_SIZE_MB**. La pulizia basata sulle dimensioni rimuove per prime le query meno recenti e meno dispendiose. Si arresta quando raggiunge all'incirca l'80% di **MAX_STORAGE_SIZE_MB**. Si tratta del valore di configurazione predefinito.

OFF     
La pulizia basata sulle dimensioni non viene attivata automaticamente.

SIZE_BASED_CLEANUP_MODE è di tipo **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Determina la modalità di acquisizione query attiva. Ogni modalità definisce criteri di acquisizione delle query specifici.

> [!NOTE]
> I cursori, le query all'interno delle stored procedure e le query compilate in modo nativo vengono sempre acquisiti quando la modalità di acquisizione query è impostata su ALL, AUTO o CUSTOM.

ALL     
Consente di acquisire tutte le query. **ALL** è il valore di configurazione predefinito per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]).

AUTO     
Consente di acquisire le query pertinenti in base al conteggio delle esecuzioni e al consumo delle risorse. Si tratta del valore di configurazione predefinito per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

NONE     
Consente di arrestare l'acquisizione di nuove query. Query Store continuerà a raccogliere le statistiche di compilazione e runtime per le query che sono già state acquisite. Usare con cautela questa configurazione perché si rischia di perdere query importanti.

CUSTOM     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Consente di controllare le opzioni QUERY_CAPTURE_POLICY.

QUERY_CAPTURE_MODE è di tipo **nvarchar**. 

max_plans_per_query     
Definisce il numero massimo di piani mantenuti per ogni query. MAX_PLANS_PER_QUERY è di tipo **int**. Il valore predefinito è **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

Controlla se verranno acquisite le statistiche di attesa per ogni query.

ON    
Verranno acquisite informazioni sulle statistiche di attesa per ogni query. Si tratta del valore di configurazione predefinito.

OFF    
Non verranno acquisite informazioni sulle statistiche di attesa per ogni query.

**\<query_capture_policy_option_list> :: =**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Controlla le opzioni dei criteri di acquisizione di Query Store. Ad eccezione di STALE_CAPTURE_POLICY_THRESHOLD, queste opzioni definiscono le condizioni OR che devono verificarsi perché le query vengano acquisite nel valore della soglia dei criteri di acquisizione non aggiornati definito.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Definisce l'intervallo di valutazione per determinare se una query deve essere acquisita. Il valore predefinito è 1 giorno e l'intervallo può essere impostato da 1 ora a sette giorni. *number* è di tipo **int**.

EXECUTION_COUNT     
Definisce il numero di esecuzioni di una query nel periodo di valutazione. Il valore predefinito è 30. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati è necessario eseguire una query almeno 30 volte al giorno perché venga salvata in modo permanente in Query Store. EXECUTION_COUNT è di tipo **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Definisce il tempo CPU di compilazione trascorso totale usato da una query nel periodo di valutazione. Il valore predefinito è 1000. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati una query deve aver usato in totale almeno un secondo di tempo CPU al giorno per la compilazione perché venga salvata in modo permanente in Query Store. TOTAL_COMPILE_CPU_TIME_MS è di tipo **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Definisce il tempo CPU di esecuzione trascorso totale usato da una query nel periodo di valutazione. Il valore predefinito è 100. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati una query deve aver usato almeno 100 ms di tempo CPU al giorno per l'esecuzione perché venga salvata in modo permanente in Query Store. TOTAL_EXECUTION_CPU_TIME_MS è di tipo **int**.

**\<recovery_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controlla le opzioni di recupero del database e il controllo degli errori di I/O su disco.

FULL     
Consente il recupero completo in caso di errori dei supporti tramite i backup del log delle transazioni. Se un file di dati risulta danneggiato, il recupero dei supporti consente di ripristinare tutte le transazioni di cui è stato eseguito il commit. Per altre informazioni, vedere [Modelli di recupero](../../relational-databases/backup-restore/recovery-models-sql-server.md).

BULK_LOGGED     
Consente il ripristino in caso di errori dei supporti. Combina le prestazioni ottimali e la quantità minima di spazio per i log per determinate operazioni su larga scala o bulk. Per informazioni sulle operazioni a cui può essere applicata la registrazione minima, vedere [Log delle transazioni](../../relational-databases/logs/the-transaction-log-sql-server.md). Con il modello di recupero BULK_LOGGED vengono registrate informazioni minime per queste operazioni. Per altre informazioni, vedere [Modelli di recupero](../../relational-databases/backup-restore/recovery-models-sql-server.md).

SEMPLICE     
Viene implementata una strategia di backup semplice che usano una quantità minima di spazio del log. Lo spazio dei log può essere riutilizzato automaticamente quando non è più necessario per il ripristino in seguito a errori del server. Per altre informazioni, vedere [Modelli di recupero](../../relational-databases/backup-restore/recovery-models-sql-server.md).

> [!IMPORTANT]
> La gestione del modello di recupero con registrazione minima risulta più semplice rispetto agli altri due modelli, ma comporta rischi maggiori di perdita dei dati in caso di danni a un file di dati. Tutte le modifiche apportate dopo l'ultimo backup completo o differenziale del database vanno perdute ed è necessario immetterle nuovamente in modo manuale.

Il modello di recupero predefinito dipende dal modello di recupero impostato per il database **model**. Per altre informazioni sulla scelta del modello di recupero appropriato, vedere [Modelli di recupero](../../relational-databases/backup-restore/recovery-models-sql-server.md).

È possibile determinare lo stato di questa opzione esaminando le colonne **recovery_model** e **recovery_model_desc** nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `Recovery` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

TORN_PAGE_DETECTION { ON | **OFF** }     
ON     
Le pagine incomplete possono essere rilevate da [!INCLUDE[ssDE](../../includes/ssde-md.md)].

OFF     
Le pagine incomplete non possono essere rilevate da [!INCLUDE[ssDE](../../includes/ssde-md.md)].

> [!IMPORTANT]
> La struttura della sintassi TORN_PAGE_DETECTION ON | OFF verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare pertanto di usarla in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente usano questa struttura. In alternativa, usare l'opzione PAGE_VERIFY.

<a name="page_verify"></a> PAGE_VERIFY { **CHECKSUM** | TORN_PAGE_DETECTION | NONE }     
Individua le pagine del database danneggiate in seguito a errori di percorso di I/O su disco. Gli errori di percorso di I/O su disco possono essere la causa di problemi di danneggiamento del database. Questi errori sono il più delle volte dovuti a interruzioni dell'alimentazione o a problemi hardware che si verificano nel momento in cui la pagina viene scritta su disco.

CHECKSUM     
Calcola un checksum sul contenuto dell'intera pagina e archivia il valore nell'intestazione della pagina quando questa viene scritta su disco. In fase di lettura della pagina dal disco, il checksum viene ricalcolato e confrontato con il valore di checksum archiviato nell'intestazione della pagina. Se i valori non corrispondono, viene segnalato il messaggio di errore 824 (che indica un errore di checksum) sia nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sia nel registro eventi di Windows. Un errore di checksum indica un problema di percorso di I/O. Per determinare la causa principale del problema, è necessaria un'analisi accurata di hardware, driver del firmware, BIOS, driver dei filtri, ad esempio software antivirus, e altri componenti del percorso di I/O.

TORN_PAGE_DETECTION     
Salva un modello a 2 bit specifico per ogni settore da 512 byte della pagina di database da 8 kilobyte (KB) e archivia tali bit nell'intestazione della pagina di database quando questa viene scritta su disco. In fase di lettura della pagina dal disco, i bit per il rilevamento di pagine incomplete archiviati nell'intestazione della pagina vengono confrontati con le informazioni effettive sui settori della pagina.

La presenza di valori non corrispondenti indica che la pagina è stata scritta su disco solo in parte. In questa situazione viene segnalato il messaggio di errore 824 (che indica un errore di pagina incompleta) sia nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sia nel registro eventi di Windows. Le pagine incomplete vengono generalmente rilevate durante il recupero del database, se si tratta effettivamente di un problema di scrittura incompleta di una pagina. Altri errori di percorso di I/O possono tuttavia causare in qualsiasi momento pagine incomplete.

NONE     
Le operazioni di scrittura di pagine di database non genereranno un valore CHECKSUM o TORN_PAGE_DETECTION. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene eseguito alcun controllo del checksum o della presenza di pagine incomplete durante una lettura, anche se nell'intestazione della pagina è presente un valore CHECKSUM o TORN_PAGE_DETECTION.

Per l'utilizzo dell'opzione PAGE_VERIFY, è importante tenere presente quanto segue:

- L'impostazione predefinita è **CHECKSUM**.
- Quando un database utente o di sistema viene aggiornato a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versione successiva, il valore di PAGE_VERIFY (NONE o TORN_PAGE_DETECTION) rimane invariato. È consigliabile usare CHECKSUM.

    > [!NOTE]
    > Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'opzione di database PAGE_VERIFY è impostata su NONE per il database TempDB e non può essere modificata. A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], il valore predefinito per il database TempDB è CHECKSUM per le nuove installazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando si aggiorna un'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], viene mantenuto il valore predefinito NONE. L'opzione può essere modificata. È consigliabile utilizzare CHECKSUM per il database tempdb.

- TORN_PAGE_DETECTION può consentire l'utilizzo di un numero più limitato di risorse, ma offre una protezione minore rispetto all'opzione CHECKSUM.
- È possibile impostare PAGE_VERIFY senza attivare la modalità offline per il database, senza bloccarlo o senza impedire in altro modo la concorrenza nel database.
- Le opzioni CHECKSUM e TORN_PAGE_DETECTION si escludono a vicenda. Non è possibile abilitare contemporaneamente entrambe le opzioni.

Se viene rilevato un errore di pagina incompleta o di checksum, è possibile eseguire il recupero tramite il ripristino dei dati o potenzialmente tramite la ricompilazione dell'indice se l'errore è limitato alle pagine di indice. Se si verifica un errore di checksum, eseguire DBCC CHECKDB per determinare il tipo della pagina o delle pagine del database interessate dal problema. Per altre informazioni sulle opzioni di ripristino, vedere [Argomenti RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Sebbene il ripristino dei dati consenta di risolvere il problema di danneggiamento dei dati, è necessario individuare il prima possibile la causa principale, ad esempio un errore hardware del disco, per eseguire i necessari interventi di correzione ed evitare che gli errori si ripresentino.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue quattro tentativi per qualsiasi operazione di lettura non riuscita a causa di un errore di checksum, di pagina incompleta o di I/O. Se la lettura riesce con uno dei tentativi, viene scritto un messaggio nel log degli errori. Il comando che ha attivato la lettura continuerà a essere eseguito. Se tutti i tentativi hanno esito negativo, il comando viene interrotto con il messaggio di errore 824.

Per altre informazioni sui messaggi di errore 823, 824 e 825, vedere:

- [Come risolvere un errore Msg 823 in SQL Server](https://support.microsoft.com/help/2015755)
- [Come risolvere un errore Msg 824 in SQL Server](https://support.microsoft.com/help/2015756)
- [How to troubleshoot Msg 825 (read retry) in SQL Server](https://support.microsoft.com/help/2015757) (Come risolvere un errore Msg 825 (tentativo di lettura) in SQL Server).

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `page_verify_option` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsTornPageDetectionEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<remote_data_archive_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Abilita o disabilita Stretch Database per il database. Per ulteriori informazioni, vedere [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

REMOTE_DATA_ARCHIVE = { ON ( SERVER = \<server_name> , { CREDENTIAL = \<db_scoped_credential_name> \| FEDERATED_SERVICE_ACCOUNT = ON \| OFF } )\| **OFF**     
ON     
Abilita o disabilita Stretch Database per il database. Per altre informazioni, inclusi i prerequisiti aggiuntivi, vedere [Abilitare Stretch Database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

Richiede l'autorizzazione `db_owner` per abilitare Stretch Database per una tabella. Richiede le autorizzazioni `db_owner` e `CONTROL DATABASE` per abilitare Stretch Database per un database.

SERVER = \<server_name>     
Specifica l'indirizzo del server di Azure. Includere la parte `.database.windows.net` del nome. Ad esempio: `MyStretchDatabaseServer.database.windows.net`.

CREDENTIAL = \<db_scoped_credential_name>     
Specifica la credenziale con ambito database che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa per connettersi al server di Azure. Assicurarsi dell'esistenza della credenziale prima di eseguire questo comando. Per altre informazioni, vedere [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

FEDERATED_SERVICE_ACCOUNT = { ON | OFF }     
È possibile usare un account del servizio federato per SQL Server locale per comunicare con il server di Azure remoto quando vengono soddisfatte tutte le condizioni seguenti.

- L'account del servizio usato per l'esecuzione dell'istanza di SQL Server è un account di dominio.
- L'account di dominio appartiene a un dominio il cui Active Directory è federato con Azure Active Directory.
- Il server Azure remoto è configurato per supportare l'autenticazione di Azure Active Directory.
- L'account del servizio in cui è in esecuzione l'istanza di SQL Server deve essere configurato come account `dbmanager` o `sysadmin` nel server di Azure remoto.

Se si specifica che l'account del servizio federato è impostato su ON, è anche possibile specificare l'argomento CREDENTIAL. Se si specifica OFF, è necessario fornire l'argomento CREDENTIAL.

OFF     
Disabilita Stretch Database per il database. Per altre informazioni, vedere [Disabilitare Stretch Database e ripristinare i dati remoti](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

È possibile disabilitare Stretch Database per un database solo quando il database non contiene più tutte le tabelle abilitate per Stretch Database. Dopo aver disabilitato Stretch Database, la migrazione dei dati si interrompe. Inoltre, i risultati delle query non includono più i risultati delle tabelle remote.

La disabilitazione di Stretch non comporta la rimozione del database remoto. Per eliminare il database remoto, usare il portale di Azure.

**\<service_broker_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controlla le opzioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)] seguenti, ovvero abilitazione o disabilitazione del recapito dei messaggi, impostazione di un nuovo identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] o impostazione delle priorità di conversazione su ON oppure OFF.

ENABLE_BROKER     
Indica che [!INCLUDE[ssSB](../../includes/sssb-md.md)] è abilitato per il database specificato. Il recapito dei messaggi viene avviato e il flag is_broker_enabled viene impostato su true nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Il database mantiene l'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] esistente. Service Broker non può essere abilitato se il database è il database principale in una configurazione di mirroring.

> [!NOTE]
> ENABLE_BROKER richiede un blocco esclusivo a livello di database. Se altre sessioni hanno bloccato risorse nel database, ENABLE_BROKER attende il rilascio dei blocchi da parte delle altre sessioni. Per abilitare [!INCLUDE[ssSB](../../includes/sssb-md.md)] in un database utente, accertarsi che nessun'altra sessione stia usando il database prima di eseguire l'istruzione ALTER DATABASE SET ENABLE_BROKER, ad esempio impostando il database in modalità utente singolo. Per abilitare [!INCLUDE[ssSB](../../includes/sssb-md.md)] nel database msdb, arrestare innanzitutto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per consentire a [!INCLUDE[ssSB](../../includes/sssb-md.md)] di ottenere il blocco necessario.

DISABLE_BROKER     
Indica che [!INCLUDE[ssSB](../../includes/sssb-md.md)] è disabilitato per il database specificato. Il recapito dei messaggi viene arrestato e il flag is_broker_enabled viene impostato su false nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Il database mantiene l'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] esistente.

NEW_BROKER     
Specifica che al database deve essere assegnato un nuovo identificatore di Service Broker. Il database funge da nuovo Service Broker. Di conseguenza, tutte le conversazioni esistenti nel database vengono rimosse immediatamente senza generare messaggi di fine dialogo. Tutte le route che fanno riferimento all'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] precedente devono essere ricreate con il nuovo identificatore.

ERROR_BROKER_CONVERSATIONS     
Specifica che il recapito dei messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è abilitato. Questa impostazione mantiene l'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] esistente per il database. [!INCLUDE[ssSB](../../includes/sssb-md.md)] termina tutte le conversazioni nel database con un errore. Questa impostazione consente alle applicazioni di eseguire operazioni regolari di pulizia per le conversazioni esistenti.

HONOR_BROKER_PRIORITY {ON | OFF}     
ON     
Per le operazioni di invio vengono presi in considerazione i livelli di priorità assegnati alle conversazioni. I messaggi provenienti da conversazioni con livelli di priorità alti vengono inviati prima dei messaggi provenienti da conversazioni con livelli di priorità bassi.

OFF     
Le operazioni di invio vengono eseguite come se a tutte le conversazioni fosse assegnato il livello di priorità predefinito.

Le modifiche all'opzione HONOR_BROKER_PRIORITY vengono applicate immediatamente ai nuovi dialoghi o ai dialoghi per cui non vi sono messaggi in attesa di essere inviati. Per i dialoghi con messaggi da inviare al momento dell'esecuzione di ALTER DATABASE, la nuova impostazione verrà applicata solo quando verranno inviati alcuni messaggi del dialogo. La quantità di tempo che deve trascorrere prima che la nuova impostazione venga usata per tutti i dialoghi può variare notevolmente.

L'impostazione corrente di questa proprietà è indicata nella colonna `is_broker_priority_honored` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<snapshot_option> ::=**      
Calcola il livello di isolamento delle transazioni.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
ON     
Abilita l'opzione relativa allo snapshot a livello di database. Se questa opzione è abilitata, le istruzioni DML iniziano a generare versioni di riga anche quando nessuna transazione usa l'isolamento dello snapshot. Una volta abilitata l'opzione, le transazioni possono specificare il livello di isolamento della transazione SNAPSHOT. Nelle transazioni eseguite con il livello di isolamento SNAPSHOT, tutte le istruzioni possono accedere a uno snapshot dei dati corrispondente allo stato dei dati al momento dell'avvio della transazione. Se una transazione eseguita con il livello di isolamento SNAPSHOT deve accedere ai dati in più database, è necessario impostare ALLOW_SNAPSHOT_ISOLATION su ON in tutti i database oppure ogni istruzione della transazione deve usare hint di blocco per qualsiasi riferimento in una clausola FROM a una tabella di un database per cui l'opzione ALLOW_SNAPSHOT_ISOLATION è impostata su OFF.

OFF     
Consente di disabilitare l'opzione Snapshot a livello di database. Per le transazioni non può essere impostato il livello di isolamento SNAPSHOT.

Quando si imposta un nuovo stato per l'opzione ALLOW_SNAPSHOT_ISOLATION (da ON a OFF oppure da OFF a ON), ALTER DATABASE restituisce il controllo al chiamante solo dopo il completamento del commit di tutte le transazioni esistenti nel database. Se per il database è già attivo lo stato specificato nell'istruzione ALTER DATABASE, il controllo viene restituito immediatamente al chiamante. Se l'istruzione ALTER DATABASE non restituisce il controllo rapidamente, usare [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) per verificare se sono presenti transazioni con esecuzione prolungata. Se l'istruzione ALTER DATABASE viene annullata, il database rimane nello stato attivo al momento dell'avvio dell'istruzione ALTER DATABASE. La vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indica lo stato delle transazioni di isolamento dello snapshot nel database. Se **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF attenderà sei secondi prima di ritentare l'operazione.

Non è possibile cambiare lo stato di ALLOW_SNAPSHOT_ISOLATION se il database è OFFLINE.

Se si imposta ALLOW_SNAPSHOT_ISOLATION in un database READ_ONLY, tale impostazione viene mantenuta anche se il database viene in seguito impostato su READ_WRITE.

È possibile modificare le impostazioni di ALLOW_SNAPSHOT_ISOLATION per i database master, model, msdb e tempdb. Se si cambia l'impostazione per il database tempdb, tale impostazione viene mantenuta ogni volta che l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene arrestata e riavviata. Se si modifica l'impostazione per il database model, questa diventa l'impostazione predefinita per i nuovi database creati, con l'eccezione di tempdb.

L'impostazione predefinita dell'opzione è ON per i database master e msdb.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `snapshot_isolation_state` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ON     
Abilita l'opzione relativa allo snapshot Read Committed a livello di database. Se questa opzione è abilitata, le istruzioni DML iniziano a generare versioni di riga anche quando nessuna transazione usa l'isolamento dello snapshot. Una volta abilitata questa opzione, le transazioni che specificano il livello di isolamento Read committed usano il controllo delle versioni delle righe anziché il blocco. Quando una transazione viene eseguita con il livello di isolamento READ COMMITTED, tutte le istruzioni vedono uno snapshot dei dati nello stato in cui si trovano all'avvio dell'istruzione.

OFF     
Disabilita l'opzione relativa allo snapshot Read Committed a livello di database. Le transazioni per cui è impostato il livello di isolamento READ COMMITTED usano il blocco.

Per impostare READ_COMMITTED_SNAPSHOT su ON o su OFF, è necessario che non siano presenti connessioni attive al database, ad eccezione della connessione usata per eseguire il comando ALTER DATABASE. Non è tuttavia necessario che il database sia in modalità utente singolo. Non è possibile cambiare lo stato di questa opzione quando il database è OFFLINE.

Se si imposta READ_COMMITTED_SNAPSHOT in un database READ_ONLY, tale impostazione viene mantenuta anche se il database viene in seguito impostato su READ_WRITE.

Non è possibile impostare READ_COMMITTED_SNAPSHOT su ON per i database di sistema master, tempdb o msdb. Se si modifica l'impostazione per il database model, questa diventa l'impostazione predefinita per i nuovi database creati, con l'eccezione di tempdb.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_read_committed_snapshot_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Quando si crea una tabella con **DURABILITY = SCHEMA_ONLY**, e successivamente si modifica **READ_COMMITTED_SNAPSHOT** usando **ALTER DATABASE**, i dati della tabella andranno perduti.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

ON     
Quando l'isolamento della transazione è impostato su un livello inferiore a SNAPSHOT, tutte le operazioni interpretate di [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle tabelle ottimizzate per la memoria vengono eseguite con l'isolamento SNAPSHOT. I livelli di isolamento inferiori a SNAPSHOT sono, ad esempio, READ COMMITTED e READ UNCOMMITTED. Queste operazioni vengono eseguite indipendentemente dal fatto che venga impostato in modo esplicito il livello di isolamento della transazione a livello di sessione o che venga usata in modo implicito l'impostazione predefinita.

OFF     
Non eleva il livello di isolamento della transazione per le operazioni interpretate di [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle tabelle ottimizzate per la memoria.

Non è possibile cambiare lo stato di MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT se il database è OFFLINE.

L'impostazione predefinita è OFF.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_memory_optimized_elevate_to_snapshot_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Controlla le opzioni di conformità ANSI a livello di database.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Determina il valore predefinito, NULL o NOT NULL, per una colonna o un [tipo CLR definito dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) per cui il supporto dei valori Null non è definito in modo esplicito nell'istruzione CREATE TABLE o ALTER TABLE. Le colonne definite con vincoli seguono le regole dei vincoli indipendentemente da questa impostazione.

ON     
Il valore predefinito di una colonna non definita è NULL.

OFF     
Il valore predefinito di una colonna non definita è NOT NULL.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_NULL_DEFAULT. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_NULL_DEFAULT su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Per motivi di compatibilità con ANSI, l'impostazione dell'opzione di database ANSI_NULL_DEFAULT su ON modifica l'impostazione predefinita del database su NULL.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_null_default_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiNullDefault` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | **OFF** }     
ON     
Tutti i confronti con un valore Null restituiscono UNKNOWN.

OFF     
I confronti di valori non UNICODE con un valore Null restituiscono TRUE se entrambi i valori sono NULL.

> [!IMPORTANT]
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS sarà sempre impostata su ON e qualsiasi applicazione che imposta tale opzione in modo esplicito su OFF genererà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_NULLS. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_NULLS su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> È inoltre necessario che l'opzione SET ANSI_NULLS sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_nulls_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiNullsEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ON     
Le stringhe vengono riempite in modo che abbiano tutte la stessa lunghezza prima della conversione. o dell'inserimento in un tipo di dati **varchar** o **nvarchar**.

OFF     
Vengono inseriti spazi vuoti finali nei valori di tipo carattere in colonne di tipo **varchar** o **nvarchar**. Vengono inoltre lasciati gli zeri finali nei valori binari inseriti nelle colonne di tipo **varbinary**. I valori non vengono riempiti per l'intera lunghezza della colonna.

Se si specifica OFF, questa impostazione ha effetto solo sulla definizione di nuove colonne.

> [!IMPORTANT]
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING sarà sempre impostata su ON e qualsiasi applicazione che imposta tale opzione in modo esplicito su OFF genererà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. È consigliabile impostare l'opzione ANSI_PADDING sempre su ON. È necessario che l'opzione ANSI_PADDING sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

Le colonne **char(_n_)** e **binary(_n_)** che consentono valori Null vengono riempite fino alla loro lunghezza quando ANSI_PADDING è impostata su ON. I valori vuoti e gli zeri finali vengono eliminati quando ANSI_PADDING è impostata su OFF. Le colonne **char(_n_)** e **binary(_n_)** che non consentono valori Null vengono sempre riempite fino alla loro lunghezza.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_PADDING. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_PADDING su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_padding_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiPaddingEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ON     
Vengono restituiti messaggi di errore o di avviso quando si verificano condizioni come la divisione per zero e inoltre quando nelle funzioni di aggregazione sono presenti valori Null.

OFF     
Non vengono generati avvisi e vengono restituiti valori Null quando si verificano condizioni come la divisione per zero.

> [!IMPORTANT]
> È necessario che l'opzione ANSI_WARNINGS sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_WARNINGS. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_WARNINGS su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_warnings_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiWarningsEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ON     
Interrompe una query quando si verifica un overflow o un errore di divisione per zero durante l'esecuzione della query stessa.

OFF     
Quando si verifica uno di questi errori, viene visualizzato un messaggio di avviso. L'esecuzione della query, del batch o della transazione prosegue come se non si fosse verificato alcun errore, anche se viene visualizzato un messaggio di avviso.

> [!IMPORTANT]
> È necessario che l'opzione ARITHABORT sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_arithabort_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsArithmeticAbortEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }     

Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ON     
Il risultato di un'operazione di concatenazione è NULL quando uno degli operandi è NULL. La concatenazione della stringa di caratteri "Questo è" con NULL restituisce, ad esempio, il valore NULL anziché il valore "Questo è".

OFF     
Il valore Null viene considerato come una stringa di caratteri vuota.

> [IMPORTANTE ] È necessario che l'opzione CONCAT_NULL_YIELDS_NULL sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.
>
> Nelle versioni future di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'opzione CONCAT_NULL_YIELDS_NULL sarà sempre impostata su ON e qualsiasi applicazione che imposta l'opzione in modo esplicito su OFF restituirà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per CONCAT_NULL_YIELDS_NULL. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta CONCAT_NULL_YIELDS_NULL su ON per la sessione quando viene stabilita una connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_concat_null_yields_null_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsNullConcat` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ON     
È possibile usare le virgolette doppie per delimitare gli identificatori delimitati.

Tutte le stringhe delimitate da virgolette doppie vengono interpretate come identificatori di oggetto. Gli identificatori delimitati non devono necessariamente essere conformi alle regole di [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori. Possono essere parole chiave e includere caratteri normalmente non consentiti negli identificatori di [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se una virgoletta singola (') fa parte della stringa letterale, può essere rappresentata tramite virgolette doppie (").

OFF     
Gli identificatori non possono essere delimitati da virgolette e devono rispettare tutte le regole di [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori. È possibile delimitare i valori letterali con virgolette singole o doppie.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente inoltre di racchiudere gli identificatori tra parentesi quadre ([ ]). Gli identificatori tra parentesi quadre possono essere sempre usati, indipendentemente dall'impostazione di QUOTED_IDENTIFIER. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).

Quando viene creata una tabella, l'opzione QUOTED IDENTIFIER viene sempre archiviata con l'impostazione ON nei relativi metadati, anche se l'opzione è impostata su OFF.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per QUOTED_IDENTIFIER. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta QUOTED_IDENTIFIER. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_quoted_identifier_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsQuotedIdentifiersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ON     
Viene generato un errore quando si verifica una perdita di precisione in un'espressione.

OFF     
La perdita di precisione non genera messaggi di errore e il risultato viene arrotondato alla precisione della colonna o della variabile in cui viene archiviato.

> [!IMPORTANT]
> È necessario che l'opzione NUMERIC_ROUNDABORT sia impostata su OFF quando vengono creati o modificati indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_numeric_roundabort_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsNumericRoundAbortEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ON     
È consentita l'attivazione ricorsiva di trigger AFTER.

OFF     
È possibile determinare lo stato di questa opzione esaminando la colonna `is_recursive_triggers_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsRecursiveTriggersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> Se l'opzione RECURSIVE_TRIGGERS è impostata su OFF, viene impedita esclusivamente la ricorsione diretta. Per disabilitare la ricorsione indiretta, è necessario impostare anche l'opzione del server nested triggers su 0.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_recursive_triggers_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsRecursiveTriggersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Specifica la frequenza di checkpoint indiretti per database singolo. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], il valore predefinito per i nuovi database è **1 minuto**, a indicare che il database userà checkpoint indiretti. Per le versioni precedenti, il valore predefinito è 0, a indicare che il database userà checkpoint automatici la cui frequenza dipende dall'impostazione dell'intervallo di recupero dell'istanza del server. [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di usare 1 minuto per la maggior parte dei sistemi.

TARGET_RECOVERY_TIME **=** *target_recovery_time* { SECONDS | MINUTES }     
*target_recovery_time*     
Specifica il limite massimo di tempo per recuperare il database specificato in caso di un arresto anomalo del sistema. *target_recovery_time* è di tipo **int**.

SECONDS     
Indica che *target_recovery_time* viene espresso come numero di secondi.

MINUTES     
Indica che *target_recovery_time* viene espresso come numero di minuti.

Per altre informazioni sui checkpoint indiretti, vedere [Checkpoint di database](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Specifica quando eseguire il rollback di transazioni incomplete in caso di transizione dello stato del database. Se questa clausola viene omessa, l'attesa da parte dell'istruzione ALTER DATABASE è illimitata in presenza di qualsiasi blocco attivo sul database. È possibile specificare una sola clausola di terminazione, che deve seguire le clausole SET.

> [!NOTE]
> Non tutte le opzioni di database usano la clausola WITH \<termination>. Per altre informazioni, vedere la tabella in "[Impostazione delle opzioni](#SettingOptions)" nella sezione "Osservazioni" di questo articolo.

ROLLBACK AFTER *number* [SECONDS] | ROLLBACK IMMEDIATE     
Specifica se eseguire il rollback dopo il numero di secondi specificato o immediatamente. *number* è di tipo **int**.

NO_WAIT     
Specifica che la richiesta ha esito negativo se non è possibile completare immediatamente la modifica richiesta dello stato del database o dell'opzione, senza aspettare il commit o il rollback automatico delle transazioni.

## <a name="setting-options"></a><a name="SettingOptions"></a> Impostazione delle opzioni
Per recuperare le impostazioni correnti delle opzioni di database, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)

Dopo l'impostazione di un'opzione di database, la modifica diventa effettiva immediatamente.

Per cambiare i valori predefiniti di qualsiasi opzione di database per tutti i nuovi database, cambiare l'opzione appropriata nel database modello.

Non tutte le opzioni di database usano la clausola WITH \<termination> o possono essere specificate in combinazione con altre opzioni. Nella tabella seguente sono elencate tali opzioni con indicazione del supporto della clausola di terminazione o dell'impostazione in combinazione con altre opzioni.

|Categoria di opzioni|Impostazione in combinazione con altre opzioni|Possono usare la clausola WITH \<termination>|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<db_state_option>|Sì|Sì|
|\<db_user_access_option>|Sì|Sì|
|\<db_update_option>|Sì|Sì|
|\<delayed_durability_option>|Sì|Sì|
|\<external_access_option>|Sì|No|
|\<cursor_option>|Sì|No|
|\<auto_option>|Sì|No|
|\<sql_option>|Sì|No|
|\<recovery_option>|Sì|No|
|\<target_recovery_time_option>|No|Sì|
|\<database_mirroring_option>|No|No|
|ALLOW_SNAPSHOT_ISOLATION|No|No|
|READ_COMMITTED_SNAPSHOT|No|Sì|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Sì|Sì|
|\<service_broker_option>|Sì|No|
|DATE_CORRELATION_OPTIMIZATION|Sì|Sì|
|\<parameterization_option>|Sì|Sì|
|\<change_tracking_option>|Sì|Sì|
|\<db_encryption_option>|Sì|No|

La cache dei piani per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene cancellata quando si imposta una delle opzioni seguenti:

:::row:::
    :::column:::
        OFFLINE
    :::column-end:::
    :::column:::
        ONLINE
    :::column-end:::
    :::column:::
        MODIFY_NAME
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        COLLATE
    :::column-end:::
    :::column:::
        READ_ONLY
    :::column-end:::
    :::column:::
        READ_WRITE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        MODIFY FILEGROUP DEFAULT
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_WRITE
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_ONLY
    :::column-end:::
:::row-end:::

La cache dei piani viene inoltre scaricata negli scenari seguenti.

- L'opzione AUTO_CLOSE di un database è impostata su ON. Se il database non viene utilizzato da alcuna connessione utente, neanche come riferimento, tramite l'attività in background viene effettuato il tentativo di chiusura e di arresto automatici del database.
- Vengono eseguite diverse query su un database contenente opzioni predefinite. Successivamente, il database viene eliminato.
- Viene eliminato uno snapshot del database per un database di origine.
- Viene ricompilato correttamente il log delle transazioni per un database.
- Viene ripristinato un backup del database.
- Viene scollegato un database.

La cancellazione della cache dei piani comporta la ricompilazione di tutti i piani di esecuzione successivi e può causare un peggioramento improvviso e temporaneo delle prestazioni di esecuzione delle query. Per ogni archivio cache cancellato nella cache dei piani, il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene il messaggio informativo seguente: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Questo messaggio viene registrato ogni cinque minuti per tutta la durata dello scaricamento della cache.

## <a name="examples"></a>Esempi

### <a name="a-setting-options-on-a-database"></a>R. Impostazione di opzioni in un database

Nell'esempio seguente vengono impostate le opzioni relative al modello di recupero e alla verifica delle pagine di dati per il database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RECOVERY FULL PAGE_VERIFY CHECKSUM;
GO

```

### <a name="b-setting-the-database-to-read_only"></a>B. Impostazione del database su READ_ONLY

Per modificare lo stato di un database o di un filegroup impostandolo su READ_ONLY o READ_WRITE, è necessario l'accesso esclusivo al database. Nell'esempio seguente viene impostata la modalità `SINGLE_USER` per il database in modo da ottenere l'accesso esclusivo. Nell'esempio lo stato del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene quindi impostato su `READ_ONLY` e viene ripristinato l'accesso al database per tutti gli utenti.

> [!NOTE]
> In questo esempio viene usata l'opzione di terminazione `WITH ROLLBACK IMMEDIATE` nella prima istruzione `ALTER DATABASE`. Verrà eseguito il rollback di tutte le transazioni incomplete e tutte le altre connessioni al database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] verranno interrotte immediatamente.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="c-enabling-snapshot-isolation-on-a-database"></a>C. Abilitazione dell'isolamento dello snapshot in un database

Nell'esempio seguente viene abilitata l'opzione relativa al framework di isolamento dello snapshot per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO

```

Il set di risultati indica che il framework di isolamento dello snapshot è abilitato.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ON |

### <a name="d-enabling-modifying-and-disabling-change-tracking"></a>D. Abilitazione, modifica e disabilitazione del rilevamento delle modifiche

Nell'esempio seguente viene abilitato il rilevamento delle modifiche per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e il periodo di memorizzazione viene impostato su `2` giorni.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

Nell'esempio seguente viene illustrato come modificare il periodo di memorizzazione impostandolo su `3` giorni.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

Nell'esempio seguente viene illustrato come disabilitare il rilevamento delle modifiche per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="e-enabling-the-query-store"></a>E. Abilitazione di Archivio query

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="f-enabling-the-query-store-with-wait-statistics"></a>F. Abilitazione di Query Store con statistiche di attesa

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="g-enabling-the-query-store-with-custom-capture-policy-options"></a>G. Abilitazione di Query Store con opzioni dei criteri di acquisizione personalizzate

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Vedere anche

- [Livello di compatibilità di ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Mirroring del database ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)
- [Statistiche](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Abilitare e disabilitare il rilevamento delle modifiche](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Procedure consigliate per l'archivio query](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Database SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>Database SQL

I livelli di compatibilità sono opzioni `SET`, ma sono descritti in [Livello di compatibilità ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> Molte opzioni SET di database possono essere configurate per la sessione corrente tramite [Istruzioni SET](../../t-sql/statements/set-statements-transact-sql.md) e spesso vengono configurate dalle applicazioni durante la connessione. Le opzioni SET a livello di sessione sostituiscono i valori **ALTER DATABASE SET**. Le opzioni di database descritte nelle sezioni seguenti sono valori che è possibile impostare per le sessioni che non prevedono esplicitamente altri valori di opzioni SET.

## <a name="syntax"></a>Sintassi

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}
;

<option_spec> ::=
{
    <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }
  | AUTOMATIC_TUNING ( CREATE_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( DROP_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
        = OFF
      | = ON [ ( <change_tracking_option_list > [,...n] ) ]
      | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<db_update_option> ::=
  { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
  { RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::= DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
      = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<termination>::=
{
    ROLLBACK AFTER integer [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention>::=TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Argomenti

*database_name*     
Nome del database da modificare.

CURRENT     
`CURRENT` esegue l'azione nel database corrente. `CURRENT` non è supportato per tutte le opzioni in tutti i contesti. In caso di errore di `CURRENT`, specificare il nome del database.

**\<auto_option> ::=**      
Consente di controllare le opzioni automatiche.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
ON     
Query Optimizer crea statistiche per colonne singole nei predicati di query, se necessario, per migliorare i piani di query e le prestazioni di esecuzione delle query. Queste statistiche per colonne singole vengono create quando Query Optimizer compila le query. Tali statistiche vengono create solo sulle colonne che ancora non sono le prime colonne di un oggetto statistiche esistente.

Il valore predefinito è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

OFF     
Query Optimizer non crea statistiche sulle singole colonne nei predicati di query durante la compilazione delle query. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_create_stats_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoCreateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione sulle opzioni relative alle statistiche in [Statistiche](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Imposta AUTO_CREATE_STATISTICS su ON e INCREMENTAL su ON. Questa impostazione crea automaticamente statistiche incrementali ogni volta che sono supportate. Il valore predefinito è OFF. Per altre informazioni, vedere [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ON     
I file di database vengono compattati periodicamente, se necessario.

È possibile compattare automaticamente sia i file di dati sia i file di log. AUTO_SHRINK riduce le dimensioni del log delle transazioni solo se per il database è impostato il modello di recupero con registrazione minima oppure se viene eseguito il backup del log. Se questa opzione è impostata su OFF, i file di database non vengono compattati automaticamente durante i controlli periodici per verificare la presenza di spazio inutilizzato.

Con l'opzione AUTO_SHRINK i file vengono compattati quando più del 25% dello spazio del file risulta inutilizzato. L'opzione causa il compattamento del file in una di due dimensioni, ossia la più grande tra:

- La dimensione in cui il 25% del file è costituito da spazio inutilizzato
- La dimensione del file quando è stato creato

Non è possibile compattare un database di sola lettura.

OFF     
I file di database non vengono compattati automaticamente durante i controlli periodici per la presenza di spazio inutilizzato.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_shrink_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoShrink` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> L'opzione AUTO_SHRINK non è disponibile in un database indipendente.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ON     
Specifica che Query Optimizer aggiorna le statistiche quando queste vengono usate da una query e quando potrebbero non essere aggiornate. Le statistiche diventano obsolete in seguito a operazioni di inserimento, aggiornamento, eliminazione o unione che modificano la distribuzione dei dati nella tabella o nella vista indicizzata. Query Optimizer determina che le statistiche potrebbero non essere aggiornate contando il numero di modifiche apportate ai dati dopo l'ultimo aggiornamento delle statistiche e confrontando il numero di modifiche con una soglia. basata sul numero di righe nella tabella o nella vista indicizzata.

Query Optimizer controlla la presenza di statistiche non aggiornate prima di compilare una query e di eseguire un piano di query memorizzato nella cache. Usa le colonne, le tabelle e le viste indicizzate nel predicato di query per determinare quali statistiche potrebbero non essere aggiornate. Query Optimizer verifica questa possibilità prima di compilare una query. Prima di eseguire un piano di query memorizzato nella cache, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica che tale piano faccia riferimento alle statistiche aggiornate.

L'opzione AUTO_UPDATE_STATISTICS_ASYNC si applica alle statistiche create per indici, colonne singole nei predicati di query e statistiche create con l'istruzione CREATE STATISTICS. Questa opzione si applica anche alle statistiche filtrate.

Il valore predefinito è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

Usare l'opzione AUTO_UPDATE_STATISTICS_ASYNC per specificare se le statistiche vengono aggiornate in modo sincrono o asincrono.

OFF     
Specifica che Query Optimizer non aggiorna le statistiche quando queste vengono usate da una query né quando potrebbero essere non aggiornate. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_update_stats_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoUpdateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione sulle opzioni relative alle statistiche in [Statistiche](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ON     
Specifica che gli aggiornamenti delle statistiche per l'opzione AUTO_UPDATE_STATISTICS sono asincroni. Query Optimizer non attende il completamento degli aggiornamenti delle statistiche per compilare le query.

L'impostazione di questa opzione su ON non produce alcun effetto, a meno che AUTO_UPDATE_STATISTICS non sia impostata su ON.

Per impostazione predefinita, l'opzione AUTO_UPDATE_STATISTICS_ASYNC è impostata su OFF. Query Optimizer aggiorna pertanto le statistiche in modo sincrono.

OFF     
Specifica che gli aggiornamenti delle statistiche per l'opzione AUTO_UPDATE_STATISTICS sono sincroni. Query Optimizer attende il completamento degli aggiornamenti delle statistiche per compilare le query.

L'impostazione di questa opzione su OFF non produce alcun effetto, a meno che AUTO_UPDATE_STATISTICS non sia impostata su ON.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_update_stats_async_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Per altre informazioni su quando usare gli aggiornamenti delle statistiche sincroni o asincroni, vedere la sezione sulle opzioni relative alle statistiche in [Statistiche](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Si applica a**: [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Controlla le opzioni automatiche per l'[ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md).

AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }     
AUTO     
Se si imposta il valore di ottimizzazione automatica su AUTO, vengono applicate le impostazioni predefinite di configurazione di Azure per l'ottimizzazione automatica.

INHERIT     
L'uso del valore INHERIT fa ereditare la configurazione predefinita dal server padre. Ciò risulta particolarmente utile se si vuole personalizzare la configurazione di ottimizzazione automatica in un server padre e fare in modo che tutti i database del server ereditino queste impostazioni personalizzate. Si noti che affinché l'ereditarietà funzioni, le tre opzioni di ottimizzazione FORCE_LAST_GOOD_PLAN, CREATE_INDEX e DROP_INDEX devono essere impostate sul valore predefinito per i database.

CUSTOM     
Se si usa il valore CUSTOM, è necessario personalizzare ciascuna delle opzioni di ottimizzazione automatica disponibili nei database.

Abilita o disabilita l'opzione di gestione automatica degli indici `CREATE_INDEX` di [ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md).

CREATE_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Fa ereditare le impostazioni predefinite dal server. In questo caso, le opzioni per l'attivazione o la disattivazione delle funzionalità di ottimizzazione automatica sono definite a livello del server.

ON     
Quando questa opzione è abilitata, gli indici mancanti vengono generati automaticamente per un database. Dopo la creazione dell'indice, vengono verificati i miglioramenti delle prestazioni del carico di lavoro. Quando non offre più vantaggi in termini di prestazioni del carico di lavoro, tale indice creato viene annullato automaticamente. Gli indici creati automaticamente vengono contrassegnati come indici generati dal sistema.

OFF     
Gli indici mancanti del database non vengono generati automaticamente.

Abilita o disabilita l'opzione di gestione automatica degli indici `DROP_INDEX` di [ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md).

DROP_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Fa ereditare le impostazioni predefinite dal server. In questo caso, le opzioni per l'attivazione o la disattivazione delle funzionalità di ottimizzazione automatica sono definite a livello del server.

ON     
Elimina automaticamente gli indici duplicati o superflui per il carico di lavoro delle prestazioni.

OFF     
Non rimuove automaticamente gli indici mancanti del database.

Abilita o disabilita l'opzione di correzione automatica dei piani `FORCE_LAST_GOOD_PLAN` di [ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF }     
DEFAULT     
Fa ereditare le impostazioni predefinite dal server. In questo caso, le opzioni per l'attivazione o la disattivazione delle funzionalità di ottimizzazione automatica sono definite a livello del server.

ON     
Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] forza automaticamente l'ultimo piano valido noto nelle query [!INCLUDE[tsql-md](../../includes/tsql-md.md)] nel caso in cui il nuovo piano di query causi un peggioramento delle prestazioni. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continua a monitorare le prestazioni delle query [!INCLUDE[tsql-md](../../includes/tsql-md.md)] usando il piano forzato. Se si rilevano miglioramenti delle prestazioni, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continuerà a usare l'ultimo piano adeguato noto. Se non si rilevano miglioramenti delle prestazioni, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] creerà un nuovo piano di query. L'istruzione avrà esito negativo se Query Store non è abilitato o se non è in modalità di *lettura/scrittura*.

OFF     
Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] segnala potenziali peggioramenti delle prestazioni di query causati da modifiche apportate al piano di query nella vista [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). I consigli qui segnalati non vengono tuttavia applicati automaticamente. È possibile visualizzare i consigli attivi e risolvere i problemi identificati applicando gli script [!INCLUDE[tsql-md](../../includes/tsql-md.md)] disponibili nella vista. Si tratta del valore predefinito.

**\<change_tracking_option> ::=**      
Controlla le opzioni di rilevamento delle modifiche. È possibile abilitare il rilevamento delle modifiche, impostare le opzioni, modificare le opzioni e disabilitare il rilevamento delle modifiche. Per alcuni esempi, vedere la sezione "Esempi" più avanti in questo articolo.

ON     
Abilita il rilevamento delle modifiche per il database. Quando si abilita il rilevamento delle modifiche, è possibile impostare anche le opzioni AUTO CLEANUP e CHANGE RETENTION.

AUTO_CLEANUP = { ON | OFF }     
ON     
Le informazioni sul rilevamento delle modifiche vengono rimosse automaticamente una volta trascorso il periodo di memorizzazione specificato.

OFF     
I dati relativi al rilevamento delle modifiche non vengono rimossi dal database.

CHANGE_RETENTION = *retention_period* { **DAYS** | HOURS | MINUTES }     
Specifica il periodo minimo di conservazione delle informazioni sul rilevamento delle modifiche nel database. I dati vengono rimossi solo quando il valore di AUTO_CLEANUP è ON.

*retention_period* è un numero intero che specifica il componente numerico del periodo di memorizzazione.

Il periodo di conservazione predefinito è **2 giorni**. Il periodo di memorizzazione minimo è 1 minuto. Il tipo di conservazione predefinito è **DAYS**.

OFF     
Disabilita il rilevamento delle modifiche per il database. Prima di disabilitare il rilevamento delle modifiche per il database, disabilitarlo per tutte le tabelle.

**\<cursor_option> ::=**      
Consente di controllare le opzioni del cursore.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ON     
Quando si esegue il commit o il rollback di una transazione, eventuali cursori aperti vengono chiusi.

OFF     
I cursori rimangono aperti quando viene eseguito il commit di una transazione. Quando si esegue il rollback di una transazione vengono chiusi tutti i cursori, ad eccezione di quelli definiti come INSENSITIVE o STATIC.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per CURSOR_CLOSE_ON_COMMIT. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta CURSOR_CLOSE_ON_COMMIT su OFF per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_cursor_close_on_commit_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsCloseCursorsOnCommitEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Il cursore viene deallocato in modo implicito soltanto al momento della disconnessione. Per altre informazioni, vedere [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Controlla lo stato della crittografia del database.

ENCRYPTION { ON | OFF }     
Imposta il database per l'utilizzo della crittografia (ON) o no (OFF). Per altre informazioni sulla crittografia del database, vedere [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) e [Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Quando la crittografia è abilitata a livello di database, tutti i filegroup vengono crittografati. I nuovi filegroup erediteranno la proprietà di crittografia. Se in un database sono presenti filegroup impostati su READ ONLY, l'operazione di crittografia del database avrà esito negativo.

È possibile visualizzare lo stato della crittografia del database usando la DMV [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_update_option> ::=**      
Indica se sono consentiti aggiornamenti nel database.

READ_ONLY     
Gli utenti possono leggere i dati dal database, ma non modificarli.

> [!NOTE]
> Per migliorare le prestazioni di esecuzione delle query, aggiornare le statistiche prima di impostare un database su READ_ONLY. Se dopo che un database viene impostato su READ_ONLY sono necessarie statistiche aggiuntive, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] verranno create statistiche in tempdb. Per altre informazioni sulle statistiche per un database di sola lettura, vedere [Statistiche](../../relational-databases/statistics/statistics.md).

READ_WRITE     
Il database è disponibile per operazioni di lettura e scrittura.

Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database. Per altre informazioni, vedere la clausola SINGLE_USER.

> [!NOTE]
> Nei database federati di [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], `SET { READ_ONLY | READ_WRITE }` è disabilitato.

**\<db_user_access_option> ::=**      
Controlla l'accesso degli utenti al database.

RESTRICTED_USER     
Consente la connessione al database solo ai membri del ruolo predefinito del database `db_owner` e ai membri dei ruoli predefiniti del server `dbcreator` e `sysadmin`, senza tuttavia imporre un limite al numero di connessioni. Tutte le connessioni al database vengono interrotte entro l'intervallo di tempo specificato nella clausola di interruzione dell'istruzione ALTER DATABASE. Dopo l'impostazione dello stato RESTRICTED_USER per il database, qualsiasi tentativo di connessione da parte di utenti non qualificati viene rifiutato. **RESTRICTED_USER** non può essere modificato con Istanza gestita di SQL.

MULTI_USER     
Consente la connessione al database a tutti gli utenti che dispongono di autorizzazioni appropriate.

È possibile determinare lo stato di questa opzione esaminando la colonna `user_access` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `UserAccess` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
Determina se le transazioni sottoposte a commit sono completamente durevoli o durevoli posticipate.

DISABLED     
Tutte le transazioni in cui viene usato `SET DISABLED` sono completamente durevoli. Tutte le opzioni di durabilità impostate in un blocco atomico o in un'istruzione COMMIT vengono ignorate.

ALLOWED     
Tutte le transazioni in cui viene usato `SET ALLOWED` sono completamente durevoli o durevoli posticipate, a seconda del set di opzioni di durabilità nel blocco atomico o nell'istruzione COMMIT.

FORCED     
Tutte le transazioni in cui viene usato `SET FORCED` sono durevoli posticipate. Tutte le opzioni di durabilità impostate in un blocco atomico o in un'istruzione COMMIT vengono ignorate.

**\<PARAMETERIZATION_option> ::=**      
Consente di controllare l'opzione di parametrizzazione.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SEMPLICE     
Le query vengono parametrizzate in base al comportamento predefinito del database.

FORCED     
Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguita la parametrizzazione di tutte le query nel database.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_parameterization_forced` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Verifica se Query Store è abilitato nel database e controlla anche la rimozione dei contenuti di Query Store.

ON     
Abilita Query Store. ON è il valore predefinito.

OFF     
Disabilita Query Store. 

> [!NOTE]  
> Non è possibile disabilitare Query Store in database singolo [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e in un pool elastico. L'esecuzione di `ALTER DATABASE [database] SET QUERY_STORE = OFF` restituirà l'avviso `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

CLEAR     
Rimuove i contenuti di Query Store.

OPERATION_MODE     
Descrive la modalità operativa di Query Store. I valori validi sono READ_ONLY e READ_WRITE. In modalità READ_WRITE Query Store raccoglie e salva in modo permanente le informazioni sulle statistiche di esecuzione di runtime e i piani di query. In modalità READ_ONLY le informazioni possono essere lette da Query Store, ma non vengono aggiunte nuove informazioni. Se lo spazio massimo allocato di Query Store viene esaurito, la sua modalità operativa passa a READ_ONLY.

CLEANUP_POLICY     
Descrive i criteri di conservazione dei dati di Query Store. STALE_QUERY_THRESHOLD_DAYS determina il numero di giorni per cui le informazioni per una query vengono conservate in Query Store. STALE_QUERY_THRESHOLD_DAYS è di tipo **bigint**. Il valore predefinito è 30. Per l'edizione [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic, l'impostazione predefinita è **7** giorni.

DATA_FLUSH_INTERVAL_SECONDS     
Determina la frequenza con cui i dati scritti in Query Store vengono salvati in modo permanente sul disco. Per ottimizzare le prestazioni, i dati raccolti da Query Store vengono scritti in modo asincrono sul disco. La frequenza con cui si verifica questo trasferimento asincrono viene configurata tramite l'argomento DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS è di tipo **bigint**. Il valore predefinito **900** (15 min).

MAX_STORAGE_SIZE_MB     
Determina lo spazio allocato a Query Store. MAX_STORAGE_SIZE_MB è di tipo **bigint**. Per l'edizione [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Premium, il valore predefinito è **1 GB**, mentre per l'edizione [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic, il valore predefinito è **10 MB**.

> [!NOTE]
> Il limite dell'impostazione `MAX_STORAGE_SIZE_MB` è 10.240 MB in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

> [!NOTE]
> Il limite `MAX_STORAGE_SIZE_MB` non è necessariamente applicato. Le dimensioni di archiviazione vengono controllate solo quando Query Store scrive i dati su disco. Questo intervallo viene impostato dall'opzione `DATA_FLUSH_INTERVAL_SECONDS` o dall'opzione **Intervallo di scaricamento dati** della finestra di dialogo Query Store di [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. Il valore predefinito dell'intervallo è 900 secondi (o 15 minuti).
> Se Query Store ha violato il limite `MAX_STORAGE_SIZE_MB` tra i controlli delle dimensioni di archiviazione, passa alla modalità di sola lettura. Se è abilitata la `SIZE_BASED_CLEANUP_MODE`, viene attivato anche il meccanismo di pulizia per applicare il limite `MAX_STORAGE_SIZE_MB`.
> Dopo la cancellazione di spazio sufficiente, la modalità di Query Store cambierà automaticamente in lettura/scrittura.

> [!IMPORTANT]
> Se si ritiene che per l'acquisizione del carico di lavoro siano necessari più di 10 GB di spazio su disco, è probabile che sia opportuno ripensare e ottimizzare il carico di lavoro in modo da riusare i piani di query (ad esempio, usando la [parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) oppure modificare le configurazioni di Query Store.    
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], è possibile impostare `QUERY_CAPTURE_MODE` su CUSTOM per un controllo aggiuntivo sui criteri di acquisizione delle query.

INTERVAL_LENGTH_MINUTES     
Determina l'intervallo di tempo in base a cui vengono aggregati i dati delle statistiche di esecuzione di runtime in Query Store. Per ottimizzare l'utilizzo dello spazio, le statistiche di esecuzione di runtime nell'archivio di statistiche di runtime vengono aggregate in un intervallo di tempo fisso. L'intervallo di tempo predefinito viene configurato tramite l'argomento INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES è di tipo **bigint**. Il valore predefinito è **60**.

SIZE_BASED_CLEANUP_MODE     
Determina se la pulizia viene attivata automaticamente quando la quantità totale dei dati ha quasi raggiunto le dimensioni massime.

OFF     
La pulizia basata sulle dimensioni non viene attivata automaticamente.

AUTO     
La pulizia basata sulle dimensioni viene attivata automaticamente quando le dimensioni sul disco raggiungono il 90% di **max_storage_size_mb**. La pulizia basata sulle dimensioni rimuove per prime le query meno recenti e meno dispendiose. Si arresta quando raggiunge all'incirca l'80% di **max_storage_size_mb**. Si tratta del valore di configurazione predefinito.

SIZE_BASED_CLEANUP_MODE è di tipo **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Determina la modalità di acquisizione query attiva. Ogni modalità definisce criteri di acquisizione delle query specifici.   

> [!NOTE]
> I cursori, le query all'interno delle stored procedure e le query compilate in modo nativo vengono sempre acquisiti quando la modalità di acquisizione query è impostata su ALL, AUTO o CUSTOM.

ALL     
Consente di acquisire tutte le query.

AUTO     
Consente di acquisire le query pertinenti in base al conteggio delle esecuzioni e al consumo delle risorse. Si tratta del valore di configurazione predefinito di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

NONE     
Consente di arrestare l'acquisizione di nuove query. Query Store continuerà a raccogliere le statistiche di compilazione e runtime per le query che sono già state acquisite. Usare con cautela questa configurazione perché si rischia di perdere query importanti.

CUSTOM     
Consente di controllare le opzioni QUERY_CAPTURE_POLICY.

QUERY_CAPTURE_MODE è di tipo **nvarchar**.

max_plans_per_query     
Definisce il numero massimo di piani mantenuti per ogni query. MAX_PLANS_PER_QUERY è di tipo **int**. Il valore predefinito è **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Controlla se verranno acquisite le statistiche di attesa per ogni query.

ON    
Verranno acquisite informazioni sulle statistiche di attesa per ogni query. Si tratta del valore di configurazione predefinito.

OFF    
Non verranno acquisite informazioni sulle statistiche di attesa per ogni query.

**\<query_capture_policy_option_list> :: =**      
Controlla le opzioni dei criteri di acquisizione di Query Store. Ad eccezione di STALE_CAPTURE_POLICY_THRESHOLD, queste opzioni definiscono le condizioni OR che devono verificarsi perché le query vengano acquisite nel valore della soglia dei criteri di acquisizione non aggiornati definito.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Definisce l'intervallo di valutazione per determinare se una query deve essere acquisita. Il valore predefinito è 1 giorno e l'intervallo può essere impostato da 1 ora a sette giorni. *number* è di tipo **int**.

EXECUTION_COUNT     
Definisce il numero di esecuzioni di una query nel periodo di valutazione. Il valore predefinito è 30. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati è necessario eseguire una query almeno 30 volte al giorno perché venga salvata in modo permanente in Query Store. EXECUTION_COUNT è di tipo **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Definisce il tempo CPU di compilazione trascorso totale usato da una query nel periodo di valutazione. Il valore predefinito è 1000. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati una query deve aver usato in totale almeno un secondo di tempo CPU al giorno per la compilazione perché venga salvata in modo permanente in Query Store. TOTAL_COMPILE_CPU_TIME_MS è di tipo **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Definisce il tempo CPU di esecuzione trascorso totale usato da una query nel periodo di valutazione. Il valore predefinito è 100. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati una query deve aver usato almeno 100 ms di tempo CPU al giorno per l'esecuzione perché venga salvata in modo permanente in Query Store. TOTAL_EXECUTION_CPU_TIME_MS è di tipo **int**.

**\<snapshot_option> ::=**      
Determina il livello di isolamento delle transazioni.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
ON     
Abilita l'opzione relativa allo snapshot a livello di database. Se questa opzione è abilitata, le istruzioni DML iniziano a generare versioni di riga anche quando nessuna transazione usa l'isolamento dello snapshot. Una volta abilitata l'opzione, le transazioni possono specificare il livello di isolamento della transazione SNAPSHOT. Nelle transazioni eseguite con il livello di isolamento SNAPSHOT, tutte le istruzioni possono accedere a uno snapshot dei dati corrispondente allo stato dei dati al momento dell'avvio della transazione. Se una transazione eseguita con il livello di isolamento SNAPSHOT deve accedere ai dati in più database, è necessario impostare ALLOW_SNAPSHOT_ISOLATION su ON in tutti i database oppure ogni istruzione della transazione deve usare hint di blocco per qualsiasi riferimento in una clausola FROM a una tabella di un database per cui l'opzione ALLOW_SNAPSHOT_ISOLATION è impostata su OFF.

OFF     
Consente di disabilitare l'opzione Snapshot a livello di database. Per le transazioni non può essere impostato il livello di isolamento SNAPSHOT.

Quando si imposta un nuovo stato per l'opzione ALLOW_SNAPSHOT_ISOLATION (da ON a OFF oppure da OFF a ON), ALTER DATABASE restituisce il controllo al chiamante solo dopo il completamento del commit di tutte le transazioni esistenti nel database. Se per il database è già attivo lo stato specificato nell'istruzione ALTER DATABASE, il controllo viene restituito immediatamente al chiamante. Se l'istruzione ALTER DATABASE non restituisce il controllo rapidamente, usare [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) per verificare se sono presenti transazioni con esecuzione prolungata. Se l'istruzione ALTER DATABASE viene annullata, il database rimane nello stato attivo al momento dell'avvio dell'istruzione ALTER DATABASE. La vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indica lo stato delle transazioni di isolamento dello snapshot nel database. Se **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF attenderà sei secondi prima di ritentare l'operazione.

Non è possibile cambiare lo stato di ALLOW_SNAPSHOT_ISOLATION se il database è OFFLINE.

Se si imposta ALLOW_SNAPSHOT_ISOLATION in un database READ_ONLY, tale impostazione viene mantenuta anche se il database viene in seguito impostato su READ_WRITE.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `snapshot_isolation_state` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ON     
Abilita l'opzione relativa allo snapshot Read Committed a livello di database. Se questa opzione è abilitata, le istruzioni DML iniziano a generare versioni di riga anche quando nessuna transazione usa l'isolamento dello snapshot. Una volta abilitata questa opzione, le transazioni che specificano il livello di isolamento READ COMMITTED usano il controllo delle versioni delle righe anziché il blocco. Quando una transazione viene eseguita con il livello di isolamento READ COMMITTED, tutte le istruzioni vedono uno snapshot dei dati nello stato in cui si trovano all'avvio dell'istruzione.

OFF     
Disabilita l'opzione relativa allo snapshot Read Committed a livello di database. Le transazioni per cui è impostato il livello di isolamento READ COMMITTED usano il blocco.

Per impostare READ_COMMITTED_SNAPSHOT su ON o su OFF, è necessario che non siano presenti connessioni attive al database, ad eccezione della connessione usata per eseguire il comando ALTER DATABASE. Non è tuttavia necessario che il database sia in modalità utente singolo. Non è possibile cambiare lo stato di questa opzione quando il database è OFFLINE.

Se si imposta READ_COMMITTED_SNAPSHOT in un database READ_ONLY, tale impostazione viene mantenuta anche se il database viene in seguito impostato su READ_WRITE.

Non è possibile impostare READ_COMMITTED_SNAPSHOT su ON per i database di sistema master, tempdb o msdb. Se si modifica l'impostazione per il database model, questa diventa l'impostazione predefinita per i nuovi database creati, con l'eccezione di tempdb.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_read_committed_snapshot_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Quando viene creata una tabella con `DURABILITY = SCHEMA_ONLY` e l'opzione **READ_COMMITTED_SNAPSHOT** viene successivamente cambiata tramite `ALTER DATABASE`, i dati della tabella vengono persi.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
ON     
Quando l'isolamento della transazione è impostato su un livello inferiore a SNAPSHOT, tutte le operazioni interpretate di [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle tabelle ottimizzate per la memoria vengono eseguite con l'isolamento SNAPSHOT. I livelli di isolamento inferiori a SNAPSHOT sono, ad esempio, READ COMMITTED e READ UNCOMMITTED. Queste operazioni vengono eseguite indipendentemente dal fatto che venga impostato in modo esplicito il livello di isolamento della transazione a livello di sessione o che venga usata in modo implicito l'impostazione predefinita.

OFF     
Non eleva il livello di isolamento della transazione per le operazioni interpretate di [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle tabelle ottimizzate per la memoria.

Non è possibile cambiare lo stato di MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT se il database è OFFLINE.

Il valore predefinito è OFF.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_memory_optimized_elevate_to_snapshot_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Controlla le opzioni di conformità ANSI a livello di database.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Determina il valore predefinito, NULL o NOT NULL, per una colonna o un [tipo CLR definito dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) per cui il supporto dei valori Null non è definito in modo esplicito nell'istruzione CREATE TABLE o ALTER TABLE. Le colonne definite con vincoli seguono le regole dei vincoli indipendentemente da questa impostazione.

ON     
Il valore predefinito è NULL.

OFF     
Il valore predefinito è NOT NULL.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_NULL_DEFAULT. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_NULL_DEFAULT su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Per motivi di compatibilità con ANSI, l'impostazione dell'opzione di database ANSI_NULL_DEFAULT su ON modifica l'impostazione predefinita del database su NULL.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_null_default_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiNullDefault` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | **OFF** }     
ON     
Tutti i confronti con un valore Null restituiscono UNKNOWN.

OFF     
I confronti di valori non UNICODE con un valore Null restituiscono TRUE se entrambi i valori sono NULL.

> [!IMPORTANT]
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS sarà sempre impostata su ON e qualsiasi applicazione che imposta tale opzione in modo esplicito su OFF genererà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_NULLS. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_NULLS su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!NOTE]
> È inoltre necessario che l'opzione SET ANSI_NULLS sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_nulls_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiNullsEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ON     
Le stringhe vengono riempite in modo che abbiano tutte la stessa lunghezza prima della conversione. o dell'inserimento in un tipo di dati **varchar** o **nvarchar**.

OFF     
Vengono inseriti spazi vuoti finali nei valori di tipo carattere in colonne di tipo **varchar** o **nvarchar**. Vengono inoltre lasciati gli zeri finali nei valori binari inseriti nelle colonne di tipo **varbinary**. I valori non vengono riempiti per l'intera lunghezza della colonna.

Se si specifica OFF, questa impostazione ha effetto solo sulla definizione di nuove colonne.

> [!IMPORTANT]
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING sarà sempre impostata su ON e qualsiasi applicazione che imposta tale opzione in modo esplicito su OFF genererà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. È consigliabile impostare l'opzione ANSI_PADDING sempre su ON. È necessario che l'opzione ANSI_PADDING sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

Le colonne **char(_n_)** e **binary(_n_)** che consentono valori Null vengono riempite fino alla loro lunghezza quando ANSI_PADDING è impostata su ON. I valori vuoti e gli zeri finali vengono eliminati quando ANSI_PADDING è impostata su OFF. Le colonne **char(_n_)** e **binary(_n_)** che non consentono valori Null vengono sempre riempite fino alla loro lunghezza.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_PADDING. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_PADDING su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_padding_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiPaddingEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ON     
Vengono restituiti messaggi di errore o di avviso quando si verificano condizioni come la divisione per zero e inoltre quando nelle funzioni di aggregazione sono presenti valori Null.

OFF     
Non vengono generati avvisi e vengono restituiti valori Null quando si verificano condizioni come la divisione per zero.

> [!NOTE]
> È necessario che l'opzione ANSI_WARNINGS sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_WARNINGS. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_WARNINGS su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_warnings_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiWarningsEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ON     
Interrompe una query quando si verifica un overflow o un errore di divisione per zero durante l'esecuzione della query stessa.

OFF     
Quando si verifica uno di questi errori, viene visualizzato un messaggio di avviso. L'esecuzione della query, del batch o della transazione prosegue come se non si fosse verificato alcun errore, anche se viene visualizzato un messaggio di avviso.

> [!NOTE]
> È necessario che l'opzione ARITHABORT sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_arithabort_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsArithmeticAbortEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ON     
Il risultato di un'operazione di concatenazione è NULL quando uno degli operandi è NULL. La concatenazione della stringa di caratteri "Questo è" con NULL restituisce, ad esempio, il valore NULL anziché il valore "Questo è".

OFF     
Il valore Null viene considerato come una stringa di caratteri vuota.

> [!NOTE]
> È necessario che l'opzione CONCAT_NULL_YIELDS_NULL sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.
>
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'opzione CONCAT_NULL_YIELDS_NULL sarà sempre impostata su ON e qualsiasi applicazione che la imposta in modo esplicito su OFF restituirà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per CONCAT_NULL_YIELDS_NULL. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta CONCAT_NULL_YIELDS_NULL su ON per la sessione quando viene stabilita una connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_concat_null_yields_null_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsNullConcat` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ON     
È possibile usare le virgolette doppie per delimitare gli identificatori delimitati.

Tutte le stringhe delimitate da virgolette doppie vengono interpretate come identificatori di oggetto. Gli identificatori delimitati non devono necessariamente essere conformi alle regole di [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori. Possono essere parole chiave e includere caratteri normalmente non consentiti negli identificatori di [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se una virgoletta singola (') fa parte della stringa letterale, può essere rappresentata tramite virgolette doppie (").

OFF     
Gli identificatori non possono essere delimitati da virgolette e devono rispettare tutte le regole di [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori. È possibile delimitare i valori letterali con virgolette singole o doppie.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente inoltre di racchiudere gli identificatori tra parentesi quadre ([ ]). Gli identificatori tra parentesi quadre possono essere sempre usati, indipendentemente dall'impostazione di QUOTED_IDENTIFIER. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).

Quando viene creata una tabella, l'opzione QUOTED IDENTIFIER viene sempre archiviata con l'impostazione ON nei relativi metadati, anche se l'opzione è impostata su OFF.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per QUOTED_IDENTIFIER. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta QUOTED_IDENTIFIER. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_quoted_identifier_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsQuotedIdentifiersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ON     
Viene generato un errore quando si verifica una perdita di precisione in un'espressione.

OFF     
La perdita di precisione non genera messaggi di errore e il risultato viene arrotondato alla precisione della colonna o della variabile in cui viene archiviato.

> [!IMPORTANT]
> È necessario che l'opzione NUMERIC_ROUNDABORT sia impostata su OFF quando vengono creati o modificati indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_numeric_roundabort_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsNumericRoundAbortEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ON     
È consentita l'attivazione ricorsiva di trigger AFTER.

OFF     
È possibile determinare lo stato di questa opzione esaminando la colonna `is_recursive_triggers_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsRecursiveTriggersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> Se l'opzione RECURSIVE_TRIGGERS è impostata su OFF, viene impedita esclusivamente la ricorsione diretta. Per disabilitare la ricorsione indiretta, è necessario impostare anche l'opzione del server nested triggers su 0.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_recursive_triggers_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsRecursiveTriggersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
Specifica la frequenza di checkpoint indiretti per database singolo. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], il valore predefinito per i nuovi database è 1 minuto, a indicare che il database userà checkpoint indiretti. Per le versioni precedenti, il valore predefinito è 0, a indicare che il database userà checkpoint automatici la cui frequenza dipende dall'impostazione dell'intervallo di recupero dell'istanza del server. [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di usare 1 minuto per la maggior parte dei sistemi.

TARGET_RECOVERY_TIME **=** target_recovery_time { SECONDS | MINUTES }     
*target_recovery_time*     
Specifica il limite massimo di tempo per recuperare il database specificato in caso di un arresto anomalo del sistema.

SECONDS     
Indica che *target_recovery_time* viene espresso come numero di secondi.

MINUTES     
Indica che *target_recovery_time* viene espresso come numero di minuti.

Per altre informazioni sui checkpoint indiretti, vedere [Checkpoint di database](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Specifica quando eseguire il rollback di transazioni incomplete in caso di transizione dello stato del database. Se questa clausola viene omessa, l'attesa da parte dell'istruzione ALTER DATABASE è illimitata in presenza di qualsiasi blocco attivo sul database. È possibile specificare una sola clausola di terminazione, che deve seguire le clausole SET.

> [!NOTE]
> Non tutte le opzioni di database usano la clausola WITH \<termination>. Per altre informazioni, vedere la tabella in "[Impostazione delle opzioni](#SettingOptions)" nella sezione "Osservazioni" di questo articolo.

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Specifica se eseguire il rollback dopo il numero di secondi specificato o immediatamente.

NO_WAIT     
Specifica che la richiesta ha esito negativo se non è possibile completare immediatamente la modifica richiesta dello stato del database o dell'opzione, senza aspettare il commit o il rollback automatico delle transazioni.

## <a name="setting-options"></a><a name="SettingOptions"></a> Impostazione delle opzioni

Per recuperare le impostazioni correnti delle opzioni di database, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)

Dopo l'impostazione di un'opzione di database, la modifica diventa effettiva immediatamente.

Per cambiare i valori predefiniti di qualsiasi opzione di database per tutti i nuovi database, cambiare l'opzione appropriata nel database modello.

Non tutte le opzioni di database usano la clausola WITH \<termination> o possono essere specificate in combinazione con altre opzioni. Nella tabella seguente sono elencate tali opzioni con indicazione del supporto della clausola di terminazione o dell'impostazione in combinazione con altre opzioni.

|Categoria di opzioni|Impostazione in combinazione con altre opzioni|Possono usare la clausola WITH \<termination>|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<auto_option>|Sì|No|
|\<change_tracking_option>|Sì|Sì|
|\<cursor_option>|Sì|No|
|\<db_encryption_option>|Sì|No|
|\<db_update_option>|Sì|Sì|
|\<db_user_access_option>|Sì|Sì|
|\<delayed_durability_option>|Sì|Sì|
|\<parameterization_option>|Sì|Sì|
|ALLOW_SNAPSHOT_ISOLATION|No|No|
|READ_COMMITTED_SNAPSHOT|No|Sì|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Sì|Sì|
|DATE_CORRELATION_OPTIMIZATION|Sì|Sì|
|\<sql_option>|Sì|No|
|\<target_recovery_time_option>|No|Sì|

## <a name="examples"></a>Esempi

### <a name="a-setting-the-database-to-read_only"></a>R. Impostazione del database su READ_ONLY
Per modificare lo stato di un database o di un filegroup impostandolo su READ_ONLY o READ_WRITE, è necessario l'accesso esclusivo al database. Nell'esempio seguente viene impostata la modalità `RESTRICTED_USER` per il database in modo da limitare l'accesso. Nell'esempio lo stato del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene quindi impostato su `READ_ONLY` e viene ripristinato l'accesso al database per tutti gli utenti.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO

```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Abilitazione dell'isolamento dello snapshot in un database
Nell'esempio seguente viene abilitata l'opzione relativa al framework di isolamento dello snapshot per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

Il set di risultati indica che il framework di isolamento dello snapshot è abilitato.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ON |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Abilitazione, modifica e disabilitazione del rilevamento delle modifiche
Nell'esempio seguente viene abilitato il rilevamento delle modifiche per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e il periodo di memorizzazione viene impostato su `2` giorni.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

L'esempio seguente illustra come modificare il periodo di conservazione impostandolo su 3 giorni.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

Nell'esempio seguente viene illustrato come disabilitare il rilevamento delle modifiche per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. Abilitazione di Archivio query
L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Abilitazione di Query Store con statistiche di attesa
L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Abilitazione di Query Store con opzioni dei criteri di acquisizione personalizzate
L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Vedere anche

- [Livello di compatibilità di ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Mirroring del database ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [Statistiche](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Abilitare e disabilitare il rilevamento delle modifiche](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Procedure consigliate per l'archivio query](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita di SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Istanza gestita di SQL di Azure

I livelli di compatibilità sono opzioni `SET`, ma sono descritti in [Livello di compatibilità ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> Molte opzioni SET di database possono essere configurate per la sessione corrente tramite [Istruzioni SET](../../t-sql/statements/set-statements-transact-sql.md) e spesso vengono configurate dalle applicazioni durante la connessione. Le opzioni SET a livello di sessione sostituiscono i valori **ALTER DATABASE SET**. Le opzioni di database descritte nelle sezioni seguenti sono valori che è possibile impostare per le sessioni che non prevedono esplicitamente altri valori di opzioni SET.

## <a name="syntax"></a>Sintassi

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <optionspec> [ ,...n ]
}
;

<optionspec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;
<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<delayed_durability_option> ::=DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
    = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<temporal_history_retention>::= TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Argomenti

*database_name*     
Nome del database da modificare.

CURRENT     
`CURRENT` esegue l'azione nel database corrente. `CURRENT` non è supportato per tutte le opzioni in tutti i contesti. In caso di errore di `CURRENT`, specificare il nome del database.

**\<auto_option> ::=**      
Consente di controllare le opzioni automatiche.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
ON     
Query Optimizer crea statistiche per colonne singole nei predicati di query, se necessario, per migliorare i piani di query e le prestazioni di esecuzione delle query. Queste statistiche per colonne singole vengono create quando Query Optimizer compila le query. Tali statistiche vengono create solo sulle colonne che ancora non sono le prime colonne di un oggetto statistiche esistente.

Il valore predefinito è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

OFF     
Query Optimizer non crea statistiche sulle singole colonne nei predicati di query durante la compilazione delle query. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_create_stats_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoCreateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione sulle opzioni relative alle statistiche in [Statistiche](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Imposta AUTO_CREATE_STATISTICS su ON e INCREMENTAL su ON. Questa impostazione crea automaticamente statistiche incrementali ogni volta che sono supportate. Il valore predefinito è OFF. Per altre informazioni, vedere [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ON     
I file di database vengono compattati periodicamente, se necessario.

È possibile compattare automaticamente sia i file di dati sia i file di log. AUTO_SHRINK riduce le dimensioni del log delle transazioni solo se per il database è impostato il modello di recupero con registrazione minima oppure se viene eseguito il backup del log. Se questa opzione è impostata su OFF, i file di database non vengono compattati automaticamente durante i controlli periodici per verificare la presenza di spazio inutilizzato.

Con l'opzione AUTO_SHRINK i file vengono compattati quando più del 25% dello spazio del file risulta inutilizzato. L'opzione causa il compattamento del file in una di due dimensioni, ossia la più grande tra:

- La dimensione in cui il 25% del file è costituito da spazio inutilizzato
- La dimensione del file quando è stato creato

Non è possibile compattare un database di sola lettura.

OFF     
I file di database non vengono compattati automaticamente durante i controlli periodici per la presenza di spazio inutilizzato.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_auto_shrink_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoShrink` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> L'opzione AUTO_SHRINK non è disponibile in un database indipendente.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ON     
Specifica che Query Optimizer aggiorna le statistiche quando queste vengono usate da una query e quando potrebbero non essere aggiornate. Le statistiche diventano obsolete in seguito a operazioni di inserimento, aggiornamento, eliminazione o unione che modificano la distribuzione dei dati nella tabella o nella vista indicizzata. Query Optimizer determina che le statistiche potrebbero non essere aggiornate contando il numero di modifiche apportate ai dati dopo l'ultimo aggiornamento delle statistiche e confrontando il numero di modifiche con una soglia. basata sul numero di righe nella tabella o nella vista indicizzata.

Query Optimizer controlla la presenza di statistiche non aggiornate prima di compilare una query e di eseguire un piano di query memorizzato nella cache. Usa le colonne, le tabelle e le viste indicizzate nel predicato di query per determinare quali statistiche potrebbero non essere aggiornate. Query Optimizer verifica questa possibilità prima di compilare una query. Prima di eseguire un piano di query memorizzato nella cache, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica che tale piano faccia riferimento alle statistiche aggiornate.

L'opzione AUTO_UPDATE_STATISTICS_ASYNC si applica alle statistiche create per indici, colonne singole nei predicati di query e statistiche create con l'istruzione CREATE STATISTICS. Questa opzione si applica anche alle statistiche filtrate.

Il valore predefinito è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

Usare l'opzione AUTO_UPDATE_STATISTICS_ASYNC per specificare se le statistiche vengono aggiornate in modo sincrono o asincrono.

OFF     
Specifica che Query Optimizer non aggiorna le statistiche quando queste vengono usate da una query né quando potrebbero essere non aggiornate. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

È possibile determinare lo stato di questa opzione esaminando la colonna is_auto_update_stats_on nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoUpdateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione "Opzioni relative alle statistiche nel database" in [Statistiche](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ON     
Specifica che gli aggiornamenti delle statistiche per l'opzione AUTO_UPDATE_STATISTICS sono asincroni. Query Optimizer non attende il completamento degli aggiornamenti delle statistiche per compilare le query.

L'impostazione di questa opzione su ON non produce alcun effetto, a meno che AUTO_UPDATE_STATISTICS non sia impostata su ON.

Per impostazione predefinita, l'opzione AUTO_UPDATE_STATISTICS_ASYNC è impostata su OFF. Query Optimizer aggiorna pertanto le statistiche in modo sincrono.

OFF     
Specifica che gli aggiornamenti delle statistiche per l'opzione AUTO_UPDATE_STATISTICS sono sincroni. Query Optimizer attende il completamento degli aggiornamenti delle statistiche per compilare le query.

L'impostazione di questa opzione su OFF non produce alcun effetto, a meno che AUTO_UPDATE_STATISTICS non sia impostata su ON.

È possibile determinare lo stato di questa opzione esaminando la colonna is_auto_update_stats_async_on nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Per altre informazioni su quando usare gli aggiornamenti delle statistiche sincroni o asincroni, vedere la sezione "Opzioni relative alle statistiche nel database" in [Statistiche](../../relational-databases/statistics/statistics.md).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Si applica a**: [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Abilita o disabilita l'opzione di `FORCE_LAST_GOOD_PLAN` [Ottimizzazione automatica](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
ON     
Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] forza automaticamente l'ultimo piano valido noto nelle query [!INCLUDE[tsql-md](../../includes/tsql-md.md)] nel caso in cui il nuovo piano di query causi un peggioramento delle prestazioni. Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continua a monitorare le prestazioni delle query [!INCLUDE[tsql-md](../../includes/tsql-md.md)] usando il piano forzato. Se si rilevano miglioramenti delle prestazioni, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continuerà a usare l'ultimo piano adeguato noto. Se non si rilevano miglioramenti delle prestazioni, il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] creerà un nuovo piano di query. L'istruzione avrà esito negativo se Query Store non è abilitato o se non è in modalità di *lettura/scrittura*.

OFF     
Il [!INCLUDE[ssde_md](../../includes/ssde_md.md)] segnala potenziali peggioramenti delle prestazioni di query causati da modifiche apportate al piano di query nella vista [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). I consigli qui segnalati non vengono tuttavia applicati automaticamente. È possibile visualizzare i consigli attivi e risolvere i problemi identificati applicando gli script [!INCLUDE[tsql-md](../../includes/tsql-md.md)] disponibili nella vista. Si tratta del valore predefinito.

**\<change_tracking_option> ::=**      
Controlla le opzioni di rilevamento delle modifiche. È possibile abilitare il rilevamento delle modifiche, impostare le opzioni, modificare le opzioni e disabilitare il rilevamento delle modifiche. Per alcuni esempi, vedere la sezione "Esempi" più avanti in questo articolo.

ON abilita il rilevamento delle modifiche per il database. Quando si abilita il rilevamento delle modifiche, è possibile impostare anche le opzioni AUTO CLEANUP e CHANGE RETENTION.

AUTO_CLEANUP = { ON | OFF }     
ON     
Le informazioni sul rilevamento delle modifiche vengono rimosse automaticamente una volta trascorso il periodo di memorizzazione specificato.

OFF     
I dati relativi al rilevamento delle modifiche non vengono rimossi dal database.

CHANGE_RETENTION = *retention_period* { **DAYS** | HOURS | MINUTES }     
Specifica il periodo minimo di conservazione delle informazioni sul rilevamento delle modifiche nel database. I dati vengono rimossi solo quando il valore di AUTO_CLEANUP è ON.

*retention_period* è un numero intero che specifica il componente numerico del periodo di memorizzazione.

Il periodo di conservazione predefinito è **2 giorni**. Il periodo di memorizzazione minimo è 1 minuto. Il tipo di conservazione predefinito è **DAYS**.

OFF     
Disabilita il rilevamento delle modifiche per il database. Prima di disabilitare il rilevamento delle modifiche per il database, disabilitarlo per tutte le tabelle.

**\<cursor_option> ::=**      
Consente di controllare le opzioni del cursore.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ON     
Quando si esegue il commit o il rollback di una transazione, eventuali cursori aperti vengono chiusi.

OFF     
I cursori rimangono aperti quando viene eseguito il commit di una transazione. Quando si esegue il rollback di una transazione vengono chiusi tutti i cursori, ad eccezione di quelli definiti come INSENSITIVE o STATIC.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per CURSOR_CLOSE_ON_COMMIT. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta CURSOR_CLOSE_ON_COMMIT su OFF per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna is_cursor_close_on_commit_on nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà IsCloseCursorsOnCommitEnabled della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Il cursore viene deallocato in modo implicito soltanto al momento della disconnessione. Per altre informazioni, vedere [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Controlla lo stato della crittografia del database.

ENCRYPTION { ON | **OFF** }     
Imposta il database per l'utilizzo della crittografia (ON) o no (OFF). Per altre informazioni sulla crittografia del database, vedere [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) e [Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Quando la crittografia è abilitata a livello di database, tutti i filegroup vengono crittografati. I nuovi filegroup erediteranno la proprietà di crittografia. Se in un database sono presenti filegroup impostati su READ ONLY, l'operazione di crittografia del database avrà esito negativo.

È possibile visualizzare lo stato della crittografia del database usando la DMV [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_update_option> ::=**      
Indica se sono consentiti aggiornamenti nel database.

READ_ONLY     
Gli utenti possono leggere i dati dal database, ma non modificarli.

> [!NOTE]
> Per migliorare le prestazioni di esecuzione delle query, aggiornare le statistiche prima di impostare un database su READ_ONLY. Se dopo che un database viene impostato su READ_ONLY sono necessarie statistiche aggiuntive, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] verranno create statistiche in tempdb. Per altre informazioni sulle statistiche per un database di sola lettura, vedere [Statistiche](../../relational-databases/statistics/statistics.md).

READ_WRITE     
Il database è disponibile per operazioni di lettura e scrittura.

Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database.

**\<db_user_access_option> ::=**      
Controlla l'accesso degli utenti al database.

RESTRICTED_USER     
Consente la connessione al database solo ai membri del ruolo predefinito del database `db_owner` e ai membri dei ruoli predefiniti del server `dbcreator` e `sysadmin`, senza tuttavia imporre un limite al numero di connessioni. Tutte le connessioni al database vengono interrotte entro l'intervallo di tempo specificato nella clausola di interruzione dell'istruzione ALTER DATABASE. Dopo l'impostazione dello stato RESTRICTED_USER per il database, qualsiasi tentativo di connessione da parte di utenti non qualificati viene rifiutato. **RESTRICTED_USER** non può essere modificato con Istanza gestita di SQL.

MULTI_USER     
Consente la connessione al database a tutti gli utenti che dispongono di autorizzazioni appropriate.

È possibile determinare lo stato di questa opzione esaminando la colonna user_access nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `UserAccess` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
Determina se le transazioni sottoposte a commit sono completamente durevoli o durevoli posticipate.

DISABLED     
Tutte le transazioni in cui viene usato `SET DISABLED` sono completamente durevoli. Tutte le opzioni di durabilità impostate in un blocco atomico o in un'istruzione COMMIT vengono ignorate.

ALLOWED     
Tutte le transazioni in cui viene usato `SET ALLOWED` sono completamente durevoli o durevoli posticipate, a seconda del set di opzioni di durabilità nel blocco atomico o nell'istruzione COMMIT.

FORCED     
Tutte le transazioni in cui viene usato `SET FORCED` sono durevoli posticipate. Tutte le opzioni di durabilità impostate in un blocco atomico o in un'istruzione COMMIT vengono ignorate.

**\<PARAMETERIZATION_option> ::=**      
Consente di controllare l'opzione di parametrizzazione.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SEMPLICE     
Le query vengono parametrizzate in base al comportamento predefinito del database.

FORCED     
Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguita la parametrizzazione di tutte le query nel database.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_parameterization_forced` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Verifica se Query Store è abilitato nel database e controlla anche la rimozione dei contenuti di Query Store.

ON     
Abilita Query Store.

OFF     
Disabilita Query Store. Si tratta del valore predefinito.

CLEAR     
Rimuove i contenuti di Query Store.

OPERATION_MODE     
Descrive la modalità operativa di Query Store. I valori validi sono READ_ONLY e READ_WRITE. In modalità READ_WRITE Query Store raccoglie e salva in modo permanente le informazioni sulle statistiche di esecuzione di runtime e i piani di query. In modalità READ_ONLY le informazioni possono essere lette da Query Store, ma non vengono aggiunte nuove informazioni. Se lo spazio massimo allocato di Query Store viene esaurito, la sua modalità operativa passa a READ_ONLY.

CLEANUP_POLICY     
Descrive i criteri di conservazione dei dati di Query Store. STALE_QUERY_THRESHOLD_DAYS determina il numero di giorni per cui le informazioni per una query vengono conservate in Query Store. STALE_QUERY_THRESHOLD_DAYS è di tipo **bigint**. Il valore predefinito è 30. Per l'edizione [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic, l'impostazione predefinita è **7** giorni.

DATA_FLUSH_INTERVAL_SECONDS     
Determina la frequenza con cui i dati scritti in Query Store vengono salvati in modo permanente sul disco. Per ottimizzare le prestazioni, i dati raccolti da Query Store vengono scritti in modo asincrono sul disco. La frequenza con cui si verifica questo trasferimento asincrono viene configurata tramite l'argomento DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS è di tipo **bigint**. Il valore predefinito **900** (15 min).

MAX_STORAGE_SIZE_MB     
Determina lo spazio allocato a Query Store. MAX_STORAGE_SIZE_MB è di tipo **bigint**. Il valore predefinito è **100 MB**.

> [!NOTE]
> Il limite `MAX_STORAGE_SIZE_MB` non è necessariamente applicato. Le dimensioni di archiviazione vengono controllate solo quando Query Store scrive i dati su disco. Questo intervallo viene impostato dall'opzione `DATA_FLUSH_INTERVAL_SECONDS` o dall'opzione **Intervallo di scaricamento dati** della finestra di dialogo Query Store di [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. Il valore predefinito dell'intervallo è 900 secondi (o 15 minuti).
> Se Query Store ha violato il limite `MAX_STORAGE_SIZE_MB` tra i controlli delle dimensioni di archiviazione, passa alla modalità di sola lettura. Se è abilitata la `SIZE_BASED_CLEANUP_MODE`, viene attivato anche il meccanismo di pulizia per applicare il limite `MAX_STORAGE_SIZE_MB`.
> Dopo la cancellazione di spazio sufficiente, la modalità di Query Store cambierà automaticamente in lettura/scrittura.

> [!IMPORTANT]
> Se si ritiene che per l'acquisizione del carico di lavoro siano necessari più di 10 GB di spazio su disco, è probabile che sia opportuno ripensare e ottimizzare il carico di lavoro in modo da riusare i piani di query (ad esempio, usando la [parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) oppure modificare le configurazioni di Query Store.    
> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], è possibile impostare `QUERY_CAPTURE_MODE` su CUSTOM per un controllo aggiuntivo sui criteri di acquisizione delle query.

INTERVAL_LENGTH_MINUTES     
Determina l'intervallo di tempo in base a cui vengono aggregati i dati delle statistiche di esecuzione di runtime in Query Store. Per ottimizzare l'utilizzo dello spazio, le statistiche di esecuzione di runtime nell'archivio di statistiche di runtime vengono aggregate in un intervallo di tempo fisso. L'intervallo di tempo predefinito viene configurato tramite l'argomento INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES è di tipo **bigint**. Il valore predefinito è **60**.

SIZE_BASED_CLEANUP_MODE     
Determina se la pulizia viene attivata automaticamente quando la quantità totale dei dati ha quasi raggiunto le dimensioni massime.

OFF     
La pulizia basata sulle dimensioni non viene attivata automaticamente.

AUTO     
La pulizia basata sulle dimensioni viene attivata automaticamente quando le dimensioni sul disco raggiungono il 90% di **max_storage_size_mb**. La pulizia basata sulle dimensioni rimuove per prime le query meno recenti e meno dispendiose. Si arresta quando raggiunge all'incirca l'80% di **max_storage_size_mb**. Si tratta del valore di configurazione predefinito.

SIZE_BASED_CLEANUP_MODE è di tipo **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Determina la modalità di acquisizione query attiva.

ALL     
Vengono acquisite tutte le query.

AUTO     
Consente di acquisire le query pertinenti in base al conteggio delle esecuzioni e al consumo delle risorse. Si tratta del valore di configurazione predefinito di [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

NONE     
Consente di arrestare l'acquisizione di nuove query. Query Store continuerà a raccogliere le statistiche di compilazione e runtime per le query che sono già state acquisite. Usare con cautela questa configurazione perché si rischia di perdere query importanti.

QUERY_CAPTURE_MODE è di tipo **nvarchar**.

max_plans_per_query     
Intero che rappresenta il numero massimo di piani mantenuti per ogni query. MAX_PLANS_PER_QUERY è di tipo **int**. Il valore predefinito è **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Controlla se verranno acquisite le statistiche di attesa per ogni query.

ON    
Verranno acquisite informazioni sulle statistiche di attesa per ogni query. Si tratta del valore di configurazione predefinito.

OFF    
Non verranno acquisite informazioni sulle statistiche di attesa per ogni query.

**\<query_capture_policy_option_list> :: =**      
Controlla le opzioni dei criteri di acquisizione di Query Store. Ad eccezione di STALE_CAPTURE_POLICY_THRESHOLD, queste opzioni definiscono le condizioni OR che devono verificarsi perché le query vengano acquisite nel valore della soglia dei criteri di acquisizione non aggiornati definito.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Definisce l'intervallo di valutazione per determinare se una query deve essere acquisita. Il valore predefinito è 1 giorno e l'intervallo può essere impostato da 1 ora a sette giorni. *number* è di tipo **int**.

EXECUTION_COUNT     
Definisce il numero di esecuzioni di una query nel periodo di valutazione. Il valore predefinito è 30. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati è necessario eseguire una query almeno 30 volte al giorno perché venga salvata in modo permanente in Query Store. EXECUTION_COUNT è di tipo **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Definisce il tempo CPU di compilazione trascorso totale usato da una query nel periodo di valutazione. Il valore predefinito è 1000. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati una query deve aver usato in totale almeno un secondo di tempo CPU al giorno per la compilazione perché venga salvata in modo permanente in Query Store. TOTAL_COMPILE_CPU_TIME_MS è di tipo **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Definisce il tempo CPU di esecuzione trascorso totale usato da una query nel periodo di valutazione. Il valore predefinito è 100. Questo significa che per il valore predefinito della soglia dei criteri di acquisizione non aggiornati una query deve aver usato almeno 100 ms di tempo CPU al giorno per l'esecuzione perché venga salvata in modo permanente in Query Store. TOTAL_EXECUTION_CPU_TIME_MS è di tipo **int**.

**\<snapshot_option> ::=**

Determina il livello di isolamento delle transazioni.

ALLOW_SNAPSHOT_ISOLATION { ON | OFF }     
ON     
Abilita l'opzione relativa allo snapshot a livello di database. Se questa opzione è abilitata, le istruzioni DML iniziano a generare versioni di riga anche quando nessuna transazione usa l'isolamento dello snapshot. Una volta abilitata l'opzione, le transazioni possono specificare il livello di isolamento della transazione SNAPSHOT. Nelle transazioni eseguite con il livello di isolamento SNAPSHOT, tutte le istruzioni possono accedere a uno snapshot dei dati corrispondente allo stato dei dati al momento dell'avvio della transazione. Se una transazione eseguita con il livello di isolamento SNAPSHOT deve accedere ai dati in più database, è necessario impostare ALLOW_SNAPSHOT_ISOLATION su ON in tutti i database oppure ogni istruzione della transazione deve usare hint di blocco per qualsiasi riferimento in una clausola FROM a una tabella di un database per cui l'opzione ALLOW_SNAPSHOT_ISOLATION è impostata su OFF.

OFF     
Consente di disabilitare l'opzione Snapshot a livello di database. Per le transazioni non può essere impostato il livello di isolamento SNAPSHOT.

Quando si imposta un nuovo stato per l'opzione ALLOW_SNAPSHOT_ISOLATION (da ON a OFF oppure da OFF a ON), ALTER DATABASE restituisce il controllo al chiamante solo dopo il completamento del commit di tutte le transazioni esistenti nel database. Se per il database è già attivo lo stato specificato nell'istruzione ALTER DATABASE, il controllo viene restituito immediatamente al chiamante. Se l'istruzione ALTER DATABASE non restituisce il controllo rapidamente, usare [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) per verificare se sono presenti transazioni con esecuzione prolungata. Se l'istruzione ALTER DATABASE viene annullata, il database rimane nello stato attivo al momento dell'avvio dell'istruzione ALTER DATABASE. La vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indica lo stato delle transazioni di isolamento dello snapshot nel database. Se **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF attenderà sei secondi prima di ritentare l'operazione.

Non è possibile cambiare lo stato di ALLOW_SNAPSHOT_ISOLATION se il database è OFFLINE.

Se si imposta ALLOW_SNAPSHOT_ISOLATION in un database READ_ONLY, tale impostazione viene mantenuta anche se il database viene in seguito impostato su READ_WRITE.

È possibile modificare le impostazioni di ALLOW_SNAPSHOT_ISOLATION per i database master, model, msdb e tempdb. Se si cambia l'impostazione per il database tempdb, tale impostazione viene mantenuta ogni volta che l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene arrestata e riavviata. Se si modifica l'impostazione per il database model, questa diventa l'impostazione predefinita per i nuovi database creati, con l'eccezione di tempdb.

L'impostazione predefinita dell'opzione è ON per i database master e msdb.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `snapshot_isolation_state` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | **OFF** }     
ON     
Abilita l'opzione relativa allo snapshot Read Committed a livello di database. Se questa opzione è abilitata, le istruzioni DML iniziano a generare versioni di riga anche quando nessuna transazione usa l'isolamento dello snapshot. Quando questa opzione è abilitata, le transazioni che specificano il livello di isolamento READ COMMITTED usano il controllo delle versioni delle righe anziché il blocco. Quando una transazione viene eseguita con il livello di isolamento READ COMMITTED, tutte le istruzioni vedono uno snapshot dei dati nello stato in cui si trovano all'avvio dell'istruzione.

OFF     
Disabilita l'opzione relativa allo snapshot Read Committed a livello di database. Le transazioni per cui è impostato il livello di isolamento READ COMMITTED usano il blocco.

Per impostare READ_COMMITTED_SNAPSHOT su ON o su OFF, è necessario che non siano presenti connessioni attive al database, ad eccezione della connessione usata per eseguire il comando ALTER DATABASE. Non è tuttavia necessario che il database sia in modalità utente singolo. Non è possibile cambiare lo stato di questa opzione quando il database è OFFLINE.

Se si imposta READ_COMMITTED_SNAPSHOT in un database READ_ONLY, tale impostazione viene mantenuta anche se il database viene in seguito impostato su READ_WRITE.

Non è possibile impostare READ_COMMITTED_SNAPSHOT su ON per i database di sistema master, tempdb o msdb. Se si modifica l'impostazione per il database model, questa diventa l'impostazione predefinita per i nuovi database creati, con l'eccezione di tempdb.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_read_committed_snapshot_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Quando si crea una tabella con **DURABILITY = SCHEMA_ONLY**, e successivamente si modifica **READ_COMMITTED_SNAPSHOT** usando **ALTER DATABASE**, i dati della tabella andranno perduti.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
ON     
Quando l'isolamento della transazione è impostato su un livello inferiore a SNAPSHOT, tutte le operazioni interpretate di [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle tabelle ottimizzate per la memoria vengono eseguite con l'isolamento SNAPSHOT. I livelli di isolamento inferiori a SNAPSHOT sono, ad esempio, READ COMMITTED e READ UNCOMMITTED. Queste operazioni vengono eseguite indipendentemente dal fatto che venga impostato in modo esplicito il livello di isolamento della transazione a livello di sessione o che venga usata in modo implicito l'impostazione predefinita.

OFF     
Non eleva il livello di isolamento della transazione per le operazioni interpretate di [!INCLUDE[tsql](../../includes/tsql-md.md)] nelle tabelle ottimizzate per la memoria.

Non è possibile cambiare lo stato di MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT se il database è OFFLINE.

Il valore predefinito è OFF.

L'impostazione corrente di questa opzione può essere determinata esaminando la colonna `is_memory_optimized_elevate_to_snapshot_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Controlla le opzioni di conformità ANSI a livello di database.

ANSI_NULL_DEFAULT { ON | OFF }     
Determina il valore predefinito, NULL o NOT NULL, per una colonna o un [tipo CLR definito dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) per cui il supporto dei valori Null non è definito in modo esplicito nell'istruzione CREATE TABLE o ALTER TABLE. Le colonne definite con vincoli seguono le regole dei vincoli indipendentemente da questa impostazione.

ON     
Il valore predefinito è NULL.

OFF     
Il valore predefinito è NOT NULL.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_NULL_DEFAULT. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_NULL_DEFAULT su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Per motivi di compatibilità con ANSI, l'impostazione dell'opzione di database ANSI_NULL_DEFAULT su ON modifica l'impostazione predefinita del database su NULL.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_null_default_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiNullDefault` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | OFF }     
ON     
Tutti i confronti con un valore Null restituiscono UNKNOWN.

OFF     
I confronti di valori non UNICODE con un valore Null restituiscono TRUE se entrambi i valori sono NULL.

> [!IMPORTANT]
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS sarà sempre impostata su ON e qualsiasi applicazione che imposta tale opzione in modo esplicito su OFF genererà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.

  Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_NULLS. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_NULLS su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> È inoltre necessario che l'opzione SET ANSI_NULLS sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_nulls_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiNullsEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ON     
Le stringhe vengono riempite in modo che abbiano tutte la stessa lunghezza prima della conversione. o dell'inserimento in un tipo di dati **varchar** o **nvarchar**.

OFF     
Vengono inseriti spazi vuoti finali nei valori di tipo carattere in colonne di tipo **varchar** o **nvarchar**. Vengono inoltre lasciati gli zeri finali nei valori binari inseriti nelle colonne di tipo **varbinary**. I valori non vengono riempiti per l'intera lunghezza della colonna.

Se si specifica OFF, questa impostazione ha effetto solo sulla definizione di nuove colonne.

> [!IMPORTANT]
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING sarà sempre impostata su ON e qualsiasi applicazione che imposta tale opzione in modo esplicito su OFF genererà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. È consigliabile impostare l'opzione ANSI_PADDING sempre su ON. È necessario che l'opzione ANSI_PADDING sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

Le colonne **char(_n_)** e **binary(_n_)** che consentono valori Null vengono riempite fino alla loro lunghezza quando ANSI_PADDING è impostata su ON. I valori vuoti e gli zeri finali vengono eliminati quando ANSI_PADDING è impostata su OFF. Le colonne **char(_n_)** e **binary(_n_)** che non consentono valori Null vengono sempre riempite fino alla loro lunghezza.

  Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_PADDING. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_PADDING su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_padding_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiPaddingEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ON     
Vengono restituiti messaggi di errore o di avviso quando si verificano condizioni come la divisione per zero e inoltre quando nelle funzioni di aggregazione sono presenti valori Null.

OFF     
Non vengono generati avvisi e vengono restituiti valori Null quando si verificano condizioni come la divisione per zero.

> [!IMPORTANT]
> È necessario che l'opzione ANSI_WARNINGS sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

  Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per ANSI_WARNINGS. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta ANSI_WARNINGS su ON per la sessione. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_ansi_warnings_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAnsiWarningsEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ON     
Interrompe una query quando si verifica un overflow o un errore di divisione per zero durante l'esecuzione della query stessa.

OFF     
Quando si verifica uno di questi errori, viene visualizzato un messaggio di avviso. L'esecuzione della query, del batch o della transazione prosegue come se non si fosse verificato alcun errore, anche se viene visualizzato un messaggio di avviso.

> [!IMPORTANT]
> È necessario che l'opzione ARITHABORT sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.

  È possibile determinare lo stato di questa opzione esaminando la colonna `is_arithabort_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsArithmeticAbortEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ON     
Il risultato di un'operazione di concatenazione è NULL quando uno degli operandi è NULL. La concatenazione della stringa di caratteri "Questo è" con NULL restituisce, ad esempio, il valore NULL anziché il valore "Questo è".

OFF     
Il valore Null viene considerato come una stringa di caratteri vuota.

> [!IMPORTANT]
> È necessario che l'opzione CONCAT_NULL_YIELDS_NULL sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate.
>
> In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'opzione CONCAT_NULL_YIELDS_NULL sarà sempre impostata su ON e qualsiasi applicazione che la imposta in modo esplicito su OFF restituirà un errore. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per CONCAT_NULL_YIELDS_NULL. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta CONCAT_NULL_YIELDS_NULL su ON per la sessione quando viene stabilita una connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

È possibile determinare lo stato di questa opzione esaminando la colonna `is_concat_null_yields_null_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsNullConcat` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ON     
È possibile usare le virgolette doppie per delimitare gli identificatori delimitati.

Tutte le stringhe delimitate da virgolette doppie vengono interpretate come identificatori di oggetto. Gli identificatori delimitati non devono necessariamente essere conformi alle regole di [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori. Possono essere parole chiave e includere caratteri normalmente non consentiti negli identificatori di [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se una virgoletta singola (') fa parte della stringa letterale, può essere rappresentata tramite virgolette doppie (").

OFF     
Gli identificatori non possono essere delimitati da virgolette e devono rispettare tutte le regole di [!INCLUDE[tsql](../../includes/tsql-md.md)] per gli identificatori. È possibile delimitare i valori letterali con virgolette singole o doppie.

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente inoltre di racchiudere gli identificatori tra parentesi quadre ([ ]). Gli identificatori tra parentesi quadre possono essere sempre usati, indipendentemente dall'impostazione di QUOTED_IDENTIFIER. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).

  Quando viene creata una tabella, l'opzione QUOTED IDENTIFIER viene sempre archiviata con l'impostazione ON nei relativi metadati, anche se l'opzione è impostata su OFF.

Le impostazioni a livello di connessione definite usando l'istruzione SET sono prioritarie rispetto all'impostazione predefinita del database per QUOTED_IDENTIFIER. Per impostazione predefinita, i client ODBC e OLE DB eseguono un'istruzione SET a livello di connessione che imposta QUOTED_IDENTIFIER. I client eseguono l'istruzione quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

  È possibile determinare lo stato di questa opzione esaminando la colonna `is_quoted_identifier_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsQuotedIdentifiersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ON     
Viene generato un errore quando si verifica una perdita di precisione in un'espressione.

OFF     
La perdita di precisione non genera messaggi di errore e il risultato viene arrotondato alla precisione della colonna o della variabile in cui viene archiviato.

> [!IMPORTANT]
> È necessario che l'opzione NUMERIC_ROUNDABORT sia impostata su OFF quando vengono creati o modificati indici in colonne calcolate o viste indicizzate.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_numeric_roundabort_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsNumericRoundAbortEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ON     
È consentita l'attivazione ricorsiva di trigger AFTER.

OFF     
È possibile determinare lo stato di questa opzione esaminando la colonna `is_recursive_triggers_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsRecursiveTriggersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> Se l'opzione RECURSIVE_TRIGGERS è impostata su OFF, viene impedita esclusivamente la ricorsione diretta. Per disabilitare la ricorsione indiretta, è necessario impostare anche l'opzione del server nested triggers su 0.

È possibile determinare lo stato di questa opzione esaminando la colonna `is_recursive_triggers_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la proprietà `IsRecursiveTriggersEnabled` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
Specifica la frequenza di checkpoint indiretti per database singolo. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], il valore predefinito per i nuovi database è **1 minuto**, a indicare che il database userà checkpoint indiretti. Per le versioni precedenti, il valore predefinito è 0, a indicare che il database userà checkpoint automatici la cui frequenza dipende dall'impostazione dell'intervallo di recupero dell'istanza del server. [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di usare 1 minuto per la maggior parte dei sistemi.

TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }     
*target_recovery_time*     
Specifica il limite massimo di tempo per recuperare il database specificato in caso di un arresto anomalo del sistema.

SECONDS     
Indica che *target_recovery_time* viene espresso come numero di secondi.

MINUTES     
Indica che *target_recovery_time* viene espresso come numero di minuti.

Per altre informazioni sui checkpoint indiretti, vedere [Checkpoint di database](../../relational-databases/logs/database-checkpoints-sql-server.md).

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Specifica se eseguire il rollback dopo il numero di secondi specificato o immediatamente.

NO_WAIT     
Specifica che la richiesta ha esito negativo se non è possibile completare immediatamente la modifica richiesta dello stato del database o dell'opzione, senza aspettare il commit o il rollback automatico delle transazioni.

## <a name="setting-options"></a><a name="SettingOptions"></a> Impostazione delle opzioni
Per recuperare le impostazioni correnti delle opzioni di database, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)

Dopo l'impostazione di un'opzione di database, la modifica diventa effettiva immediatamente.

Per cambiare i valori predefiniti di qualsiasi opzione di database per tutti i nuovi database, cambiare l'opzione appropriata nel database modello.

## <a name="examples"></a>Esempi

### <a name="a-setting-the-database-to-read_only"></a>R. Impostazione del database su READ_ONLY
Per modificare lo stato di un database o di un filegroup impostandolo su READ_ONLY o READ_WRITE, è necessario l'accesso esclusivo al database. Nell'esempio seguente viene impostata la modalità `RESTRICTED_USER` per il database in modo da limitare l'accesso. Nell'esempio lo stato del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene quindi impostato su `READ_ONLY` e viene ripristinato l'accesso al database per tutti gli utenti.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Abilitazione dell'isolamento dello snapshot in un database
Nell'esempio seguente viene abilitata l'opzione relativa al framework di isolamento dello snapshot per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

Il set di risultati indica che il framework di isolamento dello snapshot è abilitato.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ON |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Abilitazione, modifica e disabilitazione del rilevamento delle modifiche
Nell'esempio seguente viene abilitato il rilevamento delle modifiche per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e il periodo di memorizzazione viene impostato su `2` giorni.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

Nell'esempio seguente viene illustrato come modificare il periodo di memorizzazione impostandolo su `3` giorni.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

Nell'esempio seguente viene illustrato come disabilitare il rilevamento delle modifiche per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. Abilitazione di Archivio query
L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Abilitazione di Query Store con statistiche di attesa
L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Abilitazione di Query Store con opzioni dei criteri di acquisizione personalizzate
L'esempio seguente abilita Query Store e configura i relativi parametri.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Vedere anche

- [Livello di compatibilità di ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Mirroring del database ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [Statistiche](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [Abilitare e disabilitare il rilevamento delle modifiche](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Procedure consigliate per l'archivio query](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Sintassi

```syntaxsql
ALTER DATABASE { database_name }
SET
{
    <optionspec> [ ,...n ]
}
;

<option_spec>::=
{
    <auto_option>
  | <db_encryption_option>
  | <query_store_options>
  | <result_set_caching>
  | <snapshot_option>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON }
}

<db_encryption_option> ::=
{
    ENCRYPTION { ON | OFF }
}

<query_store_option> ::=
{
    QUERY_STORE { OFF |  ON }
}

<result_set_caching_option> ::=
{
    RESULT_SET_CACHING {ON | OFF}
}

<snapshot_option> ::=
{
    READ_COMMITTED_SNAPSHOT {ON | OFF }
}

```

## <a name="arguments"></a>Argomenti

*database_name*     
Nome del database da modificare.

**<auto_option> ::=**      
Consente di controllare le opzioni automatiche.

AUTO_CREATE_STATISTICS { **ON** | OFF }     

ON     
Query Optimizer crea statistiche per colonne singole nei predicati di query, se necessario, per migliorare i piani di query e le prestazioni di esecuzione delle query. Queste statistiche per colonne singole vengono create quando Query Optimizer compila le query. Tali statistiche vengono create solo sulle colonne che ancora non sono le prime colonne di un oggetto statistiche esistente.

Il valore predefinito è ON. È consigliabile usare l'impostazione predefinita per la maggior parte dei database.

OFF     
Query Optimizer non crea statistiche sulle singole colonne nei predicati di query durante la compilazione delle query. L'impostazione di questa opzione su OFF può determinare piani di query e prestazioni di esecuzione delle query non ottimali.

### <a name="remarks"></a>Osservazioni
Questo comando deve essere eseguito mentre si è connessi al database utente.

È possibile determinare lo stato di questa opzione esaminando la colonna i`s_auto_create_stats_on` nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). È anche possibile determinare lo stato esaminando la proprietà `IsAutoCreateStatistics` della funzione [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Per altre informazioni, vedere la sezione "Opzioni relative alle statistiche nel database" in Statistiche.

**<db_encryption_option> ::=**      
Controlla lo stato della crittografia del database.

ENCRYPTION { ON | OFF }     

ON     
Imposta il database in modo che venga crittografato.

OFF     
Imposta il database in modo che non venga crittografato.

Per altre informazioni sulla crittografia del database, vedere Transparent Data Encryption e Transparent Data Encryption con il database SQL di Azure.

Quando la crittografia è abilitata a livello di database, tutti i filegroup vengono crittografati. I nuovi filegroup erediteranno la proprietà di crittografia. Se in un database sono presenti filegroup impostati su READ ONLY, l'operazione di crittografia del database avrà esito negativo.

È possibile vedere lo stato di crittografia del database, così come lo stato dell'analisi di crittografia, usando la DMV sys.dm_database_encryption_keys.

**<query_store_option> ::=**      
Controlla se Query Store è abilitato in questo data warehouse.

QUERY_STORE { ON |  **OFF**  }     

ON     
Abilita Query Store.

OFF     
Disabilita Query Store. OFF è il valore predefinito.

> [!NOTE]
> Per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], è necessario eseguire il comando `ALTER DATABASE SET QUERY_STORE` dal database utente. L'esecuzione dell'istruzione da un'altra istanza del data warehouse non è supportata.

**<result_set_caching_option> ::=**      
**Si applica a**: Azure Synapse Analytics  

Controlla se il risultato della query viene memorizzato nella cache del database.

RESULT_SET_CACHING { ON | OFF}     

ON     
Specifica che i set di risultati delle query restituiti da questo database verranno memorizzati nella cache nel database.

OFF     
Specifica che i set di risultati delle query restituiti da questo database non verranno memorizzati nella cache nel database.

### <a name="remarks"></a>Osservazioni

Questo comando deve essere eseguito mentre si è connessi al database `master`.  Le modifiche apportate a questa impostazione del database hanno effetto immediato.  La memorizzazione nella cache dei set di risultati delle query prevede l'addebito dei costi di archiviazione. Dopo aver disabilitato la memorizzazione nella cache dei risultati per un database, la cache dei risultati precedentemente salvati in modo permanente verrà immediatamente eliminata dalla risorsa di archiviazione di Azure Synapse.

Eseguire questo comando per controllare la configurazione della memorizzazione nella cache dei set di risultati di un database.  Se la memorizzazione nella cache dei set di risultati è attivata, is_result_set_caching_on restituisce 1.

```sql
SELECT name, is_result_set_caching_on FROM sys.databases
WHERE name = <'Your_Database_Name'>
```

Eseguire questo comando per verificare se è stata eseguita una query usando il risultato memorizzato nella cache.  La colonna result_cache_hit restituisce 1 per il riscontro nella cache, 0 per un mancato riscontro nella cache e valori negativi per i motivi per cui non è stata usata la memorizzazione nella cache del set di risultati.  Per informazioni dettagliate, vedere [sys.dm_pdw_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true).  

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

> [!IMPORTANT]
> Le operazioni per creare la cache dei set di risultati e recuperare i dati dalla cache vengono eseguite nel nodo di controllo di un'istanza del data warehouse. Quando la memorizzazione nella cache del set di risultati è attivata, l'esecuzione di query che restituiscono set di risultati di grandi dimensioni, ad esempio più di un milione di righe, può causare un utilizzo elevato della CPU nel nodo di controllo e rallentare nel complesso la risposta alle query nell'istanza. Queste query vengono in genere usate durante l'esplorazione dei dati o le operazioni ETL (estrazione, trasformazione e caricamento). Per evitare il sovraccarico del nodo di controllo e la comparsa di problemi di prestazioni, gli utenti devono DISATTIVARE la memorizzazione nella cache del set di risultati nel database prima di eseguire query di questo tipo.  

Per informazioni dettagliate sull'ottimizzazione delle prestazioni con la memorizzazione nella cache dei set di risultati, vedere [Linee guida sull'ottimizzazione delle prestazioni](/azure/sql-data-warehouse/performance-tuning-result-set-caching).

### <a name="permissions"></a>Autorizzazioni

Per impostare l'opzione RESULT_SET_CACHING, un utente deve avere un account di accesso di tipo entità di livello server, ovvero quello creato dal processo di provisioning, oppure essere un membro del ruolo del database `dbmanager`.  

**<snapshot_option> ::=**      
**Si applica a**: Azure Synapse Analytics

Controlla il livello di isolamento delle transazioni di un database.

READ_COMMITTED_SNAPSHOT  { ON | **OFF** }     

ON     
Abilita l'opzione READ_COMMITTED_SNAPSHOT a livello di database.

OFF     
Disabilita l'opzione READ_COMMITTED_SNAPSHOT a livello di database.

### <a name="remarks"></a>Osservazioni

Questo comando deve essere eseguito mentre si è connessi al database `master`. L'impostazione di READ_COMMITTED_SNAPSHOT su ON o su OFF per un database utente comporterà la terminazione di tutte le connessioni aperte al database. È possibile apportare questa modifica durante l'intervallo di manutenzione del database o attendere fino a quando non esiste alcuna connessione attiva al database, ad eccezione della connessione che esegue il comando ALTER DATABASE.  Il database non deve essere in modalità utente singolo. La modifica dell'impostazione READ_COMMITTED_SNAPSHOT a livello di sessione non è supportata.  Per verificare questa impostazione per un database, controllare la colonna is_read_committed_snapshot_on in sys.databases.

In un database con l'impostazione READ_COMMITTED_SNAPSHOT abilitata, è possibile che le query riscontrino prestazioni più lente a causa dell'analisi delle versioni se sono presenti più versioni dei dati. Anche le transazioni aperte da tempo possono causare un aumento delle dimensioni del database. Questo problema si verifica in caso di modifiche ai dati apportate da tali transazioni che bloccano la pulizia delle versioni.  

### <a name="permissions"></a>Autorizzazioni

Per impostare l'opzione READ_COMMITTED_SNAPSHOT, un utente deve avere l'autorizzazione ALTER per il database.

## <a name="examples"></a>Esempi

### <a name="check-statistics-setting-for-a-database"></a>Controllare l'impostazione delle statistiche per un database

```sql
SELECT name, is_auto_create_stats_on FROM sys.databases
```

### <a name="enable-query-store-for-a-database"></a>Abilitare Query Store per un database

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON;
```

### <a name="enable-result-set-caching-for-a-database"></a>Abilitare la memorizzazione nella cache dei set di risultati per un database

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE [database_name]
SET RESULT_SET_CACHING ON;
```

### <a name="check-result-set-caching-setting-for-a-database"></a>Verificare l'impostazione di memorizzazione nella cache dei set di risultati per un database

```sql
SELECT name, is_result_set_caching_on
FROM sys.databases;
```

### <a name="enable-the-read_committed_snapshot-option-for-a-database"></a>Abilitare l'opzione Read_Committed_Snapshot per un database

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE MyDatabase
SET READ_COMMITTED_SNAPSHOT ON
```

## <a name="see-also"></a>Vedere anche

- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [Procedure consigliate per Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-best-practices#maintain-statistics)
- [Progettazione di tabelle in Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview#statistics)
- [Elementi del linguaggio di Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end
