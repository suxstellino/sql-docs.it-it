---
title: CREATE DATABASE (Transact-SQL) | Microsoft Docs
description: Sintassi di creazione database per SQL Server, database SQL di Azure, Azure Synapse Analytics e piattaforma di strumenti analitici
ms.custom: references_regions
ms.date: 12/11/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_TSQL
- DATABASE
- CONTAINMENT_TSQL
- CREATE DATABASE
- CREATE_DATABASE_TSQL
- CONTAINS_FILESTREAM_TSQL
- CONTAINS FILESTREAM
- CONTAINMENT
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], creating
- databases [SQL Server], creating
- model database [SQL Server], database creation
- mounted drives [SQL Server]
- CREATE DATABASE
- CREATE DATABASE statement
- file creation [SQL Server]
- creating databases
- containment
- filegroups [SQL Server], database creation
- database creation [SQL Server], CREATE DATABASE statement
- moving databases
- attaching databases [SQL Server], CREATE DATABASE...FOR ATTACH
ms.assetid: 29ddac46-7a0f-4151-bd94-75c1908c89f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: aa78f13b844a742374be212a06a5687a3a7f46fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186888"
---
# <a name="create-database"></a>CREATE DATABASE

Crea un nuovo database.

Fare clic su una delle schede seguenti per la sintassi, gli argomenti, i commenti, le autorizzazioni e gli esempi per la specifica versione di SQL in uso.

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Database SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="overview"></a>Panoramica

In SQL Server, questa istruzione crea un nuovo database, i file usati e i filegroup. Può anche essere usata per creare uno snapshot del database oppure collegare file di database per creare un database dai file scollegati di un altro database.

## <a name="syntax"></a>Sintassi

Creazione di un database

```syntaxsql
CREATE DATABASE database_name
[ CONTAINMENT = { NONE | PARTIAL } ]
[ ON
      [ PRIMARY ] <filespec> [ ,...n ]
      [ , <filegroup> [ ,...n ] ]
      [ LOG ON <filespec> [ ,...n ] ]
]
[ COLLATE collation_name ]
[ WITH <option> [,...n ] ]
[;]

<option> ::=
{
      FILESTREAM ( <filestream_option> [,...n ] )
    | DEFAULT_FULLTEXT_LANGUAGE = { lcid | language_name | language_alias }
    | DEFAULT_LANGUAGE = { lcid | language_name | language_alias }
    | NESTED_TRIGGERS = { OFF | ON }
    | TRANSFORM_NOISE_WORDS = { OFF | ON}
    | TWO_DIGIT_YEAR_CUTOFF = <two_digit_year_cutoff>
    | DB_CHAINING { OFF | ON }
    | TRUSTWORTHY { OFF | ON }
    | PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='<Filepath to folder on DAX formatted volume>' )
}

<filestream_option> ::=
{
      NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }
    | DIRECTORY_NAME = 'directory_name'
}

<filespec> ::=
{
(
    NAME = logical_file_name ,
    FILENAME = { 'os_file_name' | 'filestream_path' }
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB | % ] ]
)
}

<filegroup> ::=
{
FILEGROUP filegroup name [ [ CONTAINS FILESTREAM ] [ DEFAULT ] | CONTAINS MEMORY_OPTIMIZED_DATA ]
    <filespec> [ ,...n ]
}
```

Collegare un database

```syntaxsql
CREATE DATABASE database_name
    ON <filespec> [ ,...n ]
    FOR { { ATTACH [ WITH <attach_database_option> [ , ...n ] ] }
        | ATTACH_REBUILD_LOG }
[;]

<attach_database_option> ::=
{
      <service_broker_option>
    | RESTRICTED_USER
    | FILESTREAM ( DIRECTORY_NAME = { 'directory_name' | NULL } )
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
}
```

Creare uno snapshot del database

```syntaxsql
CREATE DATABASE database_snapshot_name
    ON
    (
        NAME = logical_file_name,
        FILENAME = 'os_file_name'
    ) [ ,...n ]
    AS SNAPSHOT OF
[;]
```

## <a name="arguments"></a>Argomenti

*database_name* è il nome del nuovo database. I nomi dei database devono essere univoci all'interno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).

*database_name* può essere composto da un massimo di 128 caratteri, eccetto i casi in cui non è stato specificato un nome logico per il file di log. Se non è stato specificato un nome logico per il file di log, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera *logical_file_name* e *os_file_name* per il log accodando un suffisso a *database_name*. Questo limita il numero di caratteri di *database_name* a 123 per fare in modo che il nome di file logico generato includa un massimo di 128 caratteri.

Se non è stato specificato alcun nome file di dati, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa *database_name* sia come *logical_file_name* che come *os_file_name*. Il percorso predefinito viene ottenuto dal Registro di sistema. Il percorso predefinito può essere modificato tramite **Proprietà server (pagina Impostazioni database)** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La modifica del percorso predefinito richiede il riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

CONTAINMENT = { NONE | PARTIAL }

**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive

Viene specificato lo stato di indipendenza del database. NONE = Database non indipendente. PARTIAL = database parzialmente indipendente.

ON specifica che i file su disco usati per archiviare le sezioni di dati del database (file di dati) vengono definiti in modo esplicito. ON è obbligatorio quando è seguito da un elenco di valori delimitato da virgole di elementi \<filespec> che definiscono i file di dati per il filegroup primario. L'elenco di file del filegroup primario può essere seguito da un elenco di valori delimitati da virgole facoltativo di elementi \<filegroup> che definiscono i filegroup utente e i relativi file.

PRIMARY specifica che l'elenco \<filespec> associato definisce il file primario. Il primo file specificato nella voce \<filespec> nel filegroup primario diventa il file primario. Un database può includere un solo file primario. Per altre informazioni, vedere [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

Se la parola chiave PRIMARY viene omessa, il primo file elencato nell'istruzione CREATE DATABASE diventa il file primario.

LOG ON specifica che i file su disco usati per archiviare il log del database (file di log) vengono definiti in modo esplicito. LOG ON è seguito da un elenco di valori delimitati da virgole di elementi \<filespec> che definiscono i file di log. Se la parola chiave LOG ON viene omessa, viene creato automaticamente un singolo file di log con dimensioni pari al 25% della somma delle dimensioni di tutti i file di dati del database o pari a 512 KB, a seconda del valore maggiore. Questo file viene posizionato nel percorso predefinito del file di log. Per informazioni su questa posizione, vedere [Visualizzare o modificare i percorsi predefiniti per i file di dati e di log - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

Non è possibile specificare LOG ON in uno snapshot del database.

COLLATE *collation_name* specifica le regole di confronto predefinite per il database. È possibile usare nomi di regole di confronto di Windows o SQL. Se collation_name viene omesso, al database vengono assegnate le regole di confronto predefinite dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è possibile specificare un nome di regole di confronto in uno snapshot del database.

Non è possibile specificare un nome di regole di confronto con le clausole FOR ATTACH o FOR ATTACH_REBUILD_LOG. Per informazioni sulla modifica delle regole di confronto di un database collegato, visitare il [sito Web Microsoft](https://go.microsoft.com/fwlink/?linkid=16419&kbid=325335).

Per altre informazioni sui nomi di regole di confronto Windows e SQL, vedere [COLLATE](~/t-sql/statements/collations.md).

> [!NOTE]
> Le regole di confronto per i database indipendenti sono diverse rispetto a quelle dei database non indipendenti. Per altre informazioni, vedere [Regole di confronto dei database indipendenti](../../relational-databases/databases/contained-database-collations.md).

WITH \<option>
 **\<filestream_option>**

NON_TRANSACTED_ACCESS = { **OFF** | READ_ONLY | FULL } **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.

Specifica il livello di accesso FILESTREAM non transazionale al database.

|valore|Descrizione|
|-----------|-----------------|
|OFF|L'accesso non transazionale è disabilitato.|
|READONLY|I dati FILESTREAM di questo database possono essere letti da processi non transazionali.|
|FULL|L'accesso non transazionale completo a tabelle FileTable FILESTREAM è abilitato.|

DIRECTORY_NAME = \<directory_name>
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive

Nome di directory compatibile con Windows. Il nome deve essere univoco in tutti i nomi di Database_Directory nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il confronto di univocità non supporta la distinzione tra maiuscole e minuscole, indipendentemente dalle impostazioni delle regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È necessario impostare questa opzione prima di creare una tabella FileTable in questo database.

Le opzioni seguenti sono consentite solo quando CONTAINMENT è stato impostato su PARTIAL. Se l'opzione CONTAINMENT è impostata su NONE, si verificheranno errori.

- **DEFAULT_FULLTEXT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive

  Per una descrizione completa di questa opzione, vedere [Configurare l'opzione di configurazione del server Lingua predefinita full-text](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md).

- **DEFAULT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive

  Per una descrizione completa di questa opzione, vedere [Configurare l'opzione di configurazione del server Lingua predefinita](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).

- **NESTED_TRIGGERS = { OFF | ON}**

  **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive

  Per una descrizione completa di questa opzione, vedere [Configurare l'opzione di configurazione del server Trigger annidati](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md).

- **TRANSFORM_NOISE_WORDS = { OFF | ON}**

  **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive

  Per una descrizione completa di questa opzione, vedere [Configurare l'opzione di configurazione del server Transform Noise Words](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).

- **TWO_DIGIT_YEAR_CUTOFF = { 2049 | \<any year between 1753 and 9999> }**

  Quattro cifre che rappresentano un anno. Il valore predefinito è 2049. Per una descrizione completa di questa opzione, vedere [Configurare l'opzione di configurazione del server two-digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).

- **DB_CHAINING { OFF | ON }**

  Se l'opzione è impostata su ON, il database può essere l'origine o la destinazione di una catena di proprietà tra database.

  Se l'opzione è impostata su OFF, il database non può partecipare alla catena di proprietà tra database. Il valore predefinito è OFF.

  > [!IMPORTANT]
  > Questa impostazione viene riconosciuta dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quando l'opzione del server cross db ownership chaining è impostata su 0 (OFF). Quando cross db ownership chaining è 1 (ON), tutti i database utente possono partecipare ai concatenamenti della proprietà tra database, a prescindere dal valore di questa opzione. Questa opzione viene impostata tramite [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

  Per impostare questa opzione è richiesta l'appartenenza al ruolo predefinito del server sysadmin. L'opzione DB_CHAINING non può essere impostata in questi database di sistema: master, model, tempdb.

- **TRUSTWORTHY { OFF | ON }**

  Se l'opzione è impostata su ON, i moduli del database (ad esempio le viste, le funzioni definite dall'utente o le stored procedure) che utilizzano un contesto di rappresentazione possono accedere alle risorse esterne al database.

  Se l'opzione è impostata su OFF, i moduli del database in un contesto di rappresentazione non possono accedere alle risorse esterne al database. Il valore predefinito è OFF.

  L'opzione TRUSTWORTHY viene impostata su OFF ogni volta che il database viene collegato.

  Per impostazione predefinita, in tutti i database di sistema, a eccezione del database msdb, TRUSTWORTHY è impostata su OFF. Per i database model e tempdb questo valore non può essere modificato. È consigliabile evitare di impostare l'opzione TRUSTWORTHY su ON per il database master.

- **PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='' )**

  Quando si specifica questa opzione, il buffer del log delle transazioni viene creato in un volume che si trova in un dispositivo disco con Storage Class Memory (memoria non volatile NVDIMM-N), noto anche come buffer del log persistente. Per altre informazioni, vedere [Transaction Commit latency acceleration using Storage Class Memory](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1) (Accelerazione della latenza di commit delle transazioni con Storage Class Memory). **Si applica a** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] e versioni successive.

FOR ATTACH [ WITH \< attach_database_option > ] specifica che il database viene creato [collegando](../../relational-databases/databases/database-detach-and-attach-sql-server.md) un set di file del sistema operativo già esistente. È necessario che una voce \<filespec> specifichi il file primario. Le altre voci \<filespec> obbligatorie sono quelle relative ai file con percorso diverso rispetto al percorso usato in fase di creazione del database o al momento dell'ultimo collegamento del database stesso. Per questi file è necessario specificare una voce \<filespec>.

FOR ATTACH richiede le seguenti condizioni:

- Tutti i file di dati (MDF e NDF) devono essere disponibili.
- Tutti i file di log esistenti devono essere disponibili.

Se un database in lettura/scrittura contiene un singolo file di log attualmente non disponibile, e il database è stato chiuso senza utenti o transazioni aperte prima dell'operazione di collegamento, FOR ATTACH ricompila automaticamente il file di log e aggiorna il file primario. Per un database in sola lettura, invece, non è possibile ricostruire il log perché il file primario non può essere aggiornato. Pertanto, quando si collega un database in sola lettura a un log non disponibile, è necessario specificare i file o i file di log nella clausola FOR ATTACH.

> [!NOTE]
> Un database creato con una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può essere collegato con versioni precedenti.

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tutti i file full-text inclusi nel database che viene collegato verranno collegati insieme al database. Per specificare un nuovo percorso per il catalogo full-text, specificare la nuova posizione senza il nome del file del sistema operativo full-text. Per altre informazioni, vedere la sezione Esempi.

Il collegamento di un database contenente un'opzione FILESTREAM "directory name" a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di verificare che il nome Database_Directory sia univoco. In caso contrario, l'operazione di collegamento ha esito negativo e viene restituito l'errore "Il nome FILESTREAM Database_Directory \<name> non è univoco in questa istanza di SQL Server". Per evitare questo errore, è necessario passare il parametro facoltativo *directory_name* a questa operazione.

Non è possibile specificare FOR ATTACH in uno snapshot del database.

FOR ATTACH può specificare l'opzione RESTRICTED_USER. RESTRICTED_USER consente la connessione al database solo ai membri del ruolo predefinito del database db_owner e ai membri dei ruoli predefiniti del server dbcreator e sysadmin, senza tuttavia imporre un limite al numero di connessioni. Tentativi dagli utenti non qualificati vengono rifiutati.

Se il database usa [!INCLUDE[ssSB](../../includes/sssb-md.md)], usare WITH \<service_broker_option> nella clausola FOR ATTACH:

\<service_broker_option> Controlla il recapito dei messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] e l'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] per il database. È possibile specificare le opzioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)] solo quando viene utilizzata la clausola FOR ATTACH.

ENABLE_BROKER specifica che [!INCLUDE[ssSB](../../includes/sssb-md.md)] è abilitato per il database specificato. In altre parole, viene avviato il recapito dei messaggi e is_broker_enabled viene impostato su true nella vista del catalogo sys.databases. Il database mantiene l'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] esistente.

NEW_BROKER crea un nuovo valore di service_broker_guid sia in sys.databases che nel database ripristinato e termina tutti gli endpoint di conversazione con la pulizia. Service Broker è abilitato, ma agli endpoint di conversazione remoti non viene inviato alcun messaggio. Tutte le route che fanno riferimento all'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] precedente devono essere ricreate con il nuovo identificatore.

ERROR_BROKER_CONVERSATIONS termina tutte le conversazioni e restituisce un errore che indica che il database è collegato o ripristinato. Service Broker viene disabilitato fino al termine dell'operazione e quindi viene riabilitato. Il database mantiene l'identificatore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] esistente.

Quando si collega un database replicato copiato anziché scollegato, è necessario considerare quanto segue:

- Se si collega il database alla stessa istanza del server e alla stessa versione del database originale, non sono necessari passaggi aggiuntivi.
- Se si collega il database alla stessa istanza del server ma si usa una versione aggiornata, dopo il completamento dell'operazione di collegamento è necessario eseguire [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) per aggiornare la replica.
- Se si collega il database a un'istanza del server diversa, indipendentemente dalla versione, dopo il completamento dell'operazione di collegamento è necessario eseguire [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) per rimuovere la replica.

> [!NOTE]
> Il collegamento supporta il formato di archiviazione **vardecimal**, ma è necessario aggiornare il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] almeno a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2. Non è possibile collegare un database con un formato di archiviazione vardecimal a una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni sul formato di archiviazione **vardecimal**, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).

Quando un database viene collegato per la prima volta a una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o ripristinato, nel server non è ancora archiviata una copia della chiave master del database, crittografata dalla chiave master del servizio. È necessario usare l'istruzione **OPEN MASTER KEY** per decrittografare la chiave master del database. Dopo aver decrittografato la DMK, è possibile usare l'istruzione **ALTER MASTER KEY REGENERATE** per abilitare la decrittografia automatica per le operazioni successive, in modo da fornire al server una copia della DMK crittografata con la chiave master del servizio (SMK). Quando un database è stato aggiornato da una versione precedente, la DMK deve essere rigenerata per usare l'algoritmo AES più recente. Per altre informazioni sulla rigenerazione della DMK, vedere [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Il tempo richiesto per rigenerare la chiave DMK e aggiornarla ad AES dipende dal numero di oggetti protetti dalla DMK. È necessario rigenerare la chiave DMK per l'aggiornamento ad AES una sola volta e l'operazione non influenza le rigenerazioni future che fanno parte di una strategia di rotazione della chiave. Per informazioni su come aggiornare un database tramite collegamento, vedere [Aggiornamento di un database usando le operazioni di scollegamento e collegamento](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md).

> [!IMPORTANT]
> È consigliabile non collegare database da origini sconosciute o non attendibili. Tali database possono contenere codice dannoso che potrebbe eseguire codice [!INCLUDE[tsql](../../includes/tsql-md.md)] indesiderato o causare errori modificando lo schema o la struttura fisica di database. Prima di usare un database da un'origine sconosciuta o non attendibile, eseguire [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database in un server non di produzione ed esaminare il codice contenuto nel database, ad esempio le stored procedure o altro codice definito dall'utente.
> [!NOTE]
> Le opzioni **TRUSTWORTHY** e **DB_CHAINING** non hanno effetto quando si collega un database.

FOR ATTACH_REBUILD_LOG specifica che il database viene creato collegando un set di file del sistema operativo già esistente. Questa opzione è limitata ai database in lettura/scrittura. È necessario che una voce *\<filespec>* specifichi il file primario. Se uno o più file di log delle transazioni sono mancanti, il file di log viene ricostruito. Il log ATTACH_REBUILD_LOG crea automaticamente un nuovo file di log da 1 MB. Questo file viene posizionato nel percorso predefinito del file di log. Per informazioni su questa posizione, vedere [Visualizzare o modificare i percorsi predefiniti per i file di dati e di log - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

> [!NOTE]
> Se i file di log sono disponibili, [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizza questi file invece di ricompilare i file di log.

FOR ATTACH_REBUILD_LOG richiede le seguenti condizioni:

- Una chiusura normale del database.
- Tutti i file di dati (MDF e NDF) devono essere disponibili.

> [!IMPORTANT]
> Questa operazione interrompe la catena di backup del log. È consigliabile eseguire un backup completo del database al termine dell'operazione. Per altre informazioni, vedere [BACKUP](../../t-sql/statements/backup-transact-sql.md).

In genere, l'opzione FOR ATTACH_REBUILD_LOG viene utilizzata quando si copia un database in lettura/scrittura con un log di grandi dimensioni in un altro server dove la copia verrà utilizzata principalmente, o esclusivamente, per operazioni di lettura e richiederà quindi una quantità minore di spazio di log rispetto al database originale.

Non è possibile specificare FOR ATTACH_REBUILD_LOG in uno snapshot del database.

Per altre informazioni sul collegamento e lo scollegamento di database, vedere [Collegamento e scollegamento di un database](../../relational-databases/databases/database-detach-and-attach-sql-server.md).

\<filespec> Controlla le proprietà del file.

NAME *logical_file_name* specifica il nome logico del file. Il parametro NAME è necessario quando FILENAME è specificato, eccetto quando viene specificata una delle clausole FOR ATTACH. Non è possibile assegnare il nome PRIMARY a un filegroup FILESTREAM.

*logical_file_name* è il nome logico usato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per fare riferimento al file. *Logical_file_name* deve essere univoco all'interno del database e conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome può essere un carattere o una costante Unicode oppure un identificatore normale o delimitato.

FILENAME{ **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** } specifica il nome file (fisico) del sistema operativo.

**'** *os_file_name* **'** indica il percorso e il nome file usati dal sistema operativo quando si crea il file. Il file deve risiedere in uno dei dispositivi seguenti: il server locale in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], una rete di archiviazione (SAN) o una rete basata su iSCSI. Il percorso specificato deve essere esistente prima dell'esecuzione dell'istruzione CREATE DATABASE. Per altre informazioni, vedere "Filegroup e file di database" nella sezione Osservazioni.

È possibile impostare i parametri SIZE, MAXSIZE e FILEGROWTH se è specificato un percorso UNC per il file.

Se il file si trova in una partizione non formattata, nell'argomento *os_file_name* è necessario specificare solo la lettera dell'unità di una partizione non formattata esistente. È possibile creare soltanto un file di dati su ogni partizione non formattata dal sistema operativo.

I file di dati non devono essere archiviati in file system compressi a meno che non si tratti di file secondari in sola lettura o il database non sia in sola lettura. I file di log non devono mai essere archiviati in file system compressi.

**'** *filestream_path* **'** : per un filegroup FILESTREAM, FILENAME fa riferimento a un percorso in cui verranno archiviati i dati di FILESTREAM. È necessario che il percorso fino all'ultima cartella esista già, mentre l'ultima cartella non deve essere presente. Se, ad esempio, si specifica il percorso C:\MyFiles\MyFilestreamData, C:\MyFiles deve esistere già prima di eseguire ALTER DATABASE, mentre la cartella MyFilestreamData non deve essere presente.

Il filegroup e il file (`<filespec>`) devono essere creati nella stessa istruzione.

Le proprietà SIZE e FILEGROWTH non si applicano a un filegroup FILESTREAM.

SIZE *size* specifica le dimensioni del file.

Non è possibile specificare SIZE quando *os_file_name* è specificato come percorso UNC. SIZE non si applica a un filegroup FILESTREAM.

*size* corrisponde alle dimensioni iniziali del file.

Se non si specifica *size* per il file primario, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa le dimensioni del file primario del database modello. Le dimensioni predefinite del modello corrispondono a 8 MB (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) o a 1 MB (per le versioni precedenti). Se si specifica un file di dati o un file di log secondario senza specificare *size* per il file, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea un file di 8 MB (a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) o di 1 MB (per le versioni precedenti). Le dimensioni specificate per il file di dati primario devono essere uguali almeno alle dimensioni del file primario del database model.

È possibile usare i suffissi per kilobyte (KB), megabyte (MB), gigabyte (GB) e terabyte (TB). Il valore predefinito è MB. Specificare un numero intero, ovvero non includere decimali. *Size* è un valore intero. Per i valori superiori a 2.147.483.647, usare le unità maggiori.

MAXSIZE *max_size* specifica le dimensioni massime consentite per il file. Non è possibile specificare MAXSIZE quando *os_file_name* è specificato come percorso UNC.

*max_size* corrisponde alle dimensioni massime del file. È possibile usare i suffissi KB, MB, GB e TB. Il valore predefinito è MB. Specificare un numero intero, ovvero non includere decimali. Se *max_size* viene omesso, le dimensioni del file aumentano fino all'esaurimento dello spazio su disco. *Max_size* è un valore intero. Per i valori superiori a 2.147.483.647, usare le unità maggiori.

UNLIMITED specifica che le dimensioni del file aumentano fino a quando il disco risulta pieno. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un file di log specificato con aumento delle dimensioni illimitato può raggiungere una dimensione massima di 2 TB, mentre un file di dati può raggiungere una dimensione massima di 16 TB.

> [!NOTE]
> Non vi sono dimensioni massime se questa opzione viene specificata per un contenitore FILESTREAM, il quale continua a crescere finché il disco non è pieno.

FILEGROWTH *growth_increment* specifica l'incremento automatico per l'aumento delle dimensioni del file. Il valore impostato per il parametro FILEGROWTH di un file non può essere superiore al valore del parametro MAXSIZE. Non è possibile specificare FILEGROWTH quando *os_file_name* è specificato come percorso UNC. FILEGROWTH non si applica a un filegroup FILESTREAM.

*growth_increment* è la quantità di spazio aggiunta al file ogni volta che è necessario altro spazio.

È possibile specificare il valore in megabyte (MB), kilobyte (KB), gigabyte (GB) o terabyte (TB) oppure in forma di percentuale (%). Se si specifica un valore senza il suffisso MB, KB o %, il suffisso predefinito è MB. Se si utilizza il suffisso %, l'incremento corrisponde alla percentuale delle dimensioni del file specificata quando si verifica l'incremento. Le dimensioni specificate vengono arrotondate al blocco di 64 KB più prossimo e il valore minimo è 64 KB.

Un valore 0 indica che l'opzione per l'aumento automatico è disattivata e non è consentito spazio aggiuntivo.

Se FILEGROWTH viene omesso, i valori predefiniti sono i seguenti:

|Versione|Valori predefiniti|
|-------------|--------------------|
|Inizio [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]|Dati 64 MB. File di log 64 MB.|
|Inizio [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Dati 1 MB. File di log 10%.|
|Prima di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Dati 10%. File di log 10%.|

\<filegroup> Controlla le proprietà del filegroup. Non è possibile specificare il filegroup in uno snapshot del database.

FILEGROUP *filegroup_name* è il nome logico del filegroup.

*filegroup_name*
*filegroup_name* deve essere univoco nel database e deve essere diverso dai nomi PRIMARY e PRIMARY_LOG forniti dal sistema. Il nome può essere un carattere o una costante Unicode oppure un identificatore normale o delimitato. Il nome deve essere conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).

CONTAINS FILESTREAM specifica che il filegroup archivia BLOB FILESTREAM nel file system.

CONTAINS MEMORY_OPTIMIZED_DATA

**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive

Specifica che il filegroup archivia i dati memory_optimized nel file system. Per altre informazioni, vedere [OLTP in memoria - Ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). È ammesso un solo filegroup MEMORY_OPTIMIZED_DATA per database. Per esempi di codice che creano un filegroup per l'archiviazione di dati ottimizzati per la memoria, vedere [Creazione di una tabella ottimizzata per la memoria e di una stored procedure compilata in modo nativo](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).

DEFAULT indica che il filegroup specificato è il filegroup predefinito nel database.

*database_snapshot_name* è il nome del nuovo snapshot del database. I nomi dei database devono essere univoci all'interno di un'istanza d [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e devono essere conformi alle regole per gli identificatori. *database_snapshot_name* può essere composto da un massimo di 128 caratteri.

ON **(** NAME **=** _logical\_file\_name_ **,** FILENAME **='** _os\_file\_name_ **')** [ **,** ... *n* ] per la creazione di uno snapshot del database, specifica un elenco di file nel database di origine. Per il funzionamento dello snapshot, è necessario specificare tutti i file di dati singolarmente. I file di log non sono tuttavia consentiti per gli snapshot del database. I filegroup FILESTREAM non sono supportati dagli snapshot del database. Se un file di dati FILESTREAM è incluso in una clausola CREATE DATABASE ON, l'istruzione non verrà eseguita e sarà generato un errore.

Per le descrizioni di NAME e FILENAME e i rispettivi valori, vedere le descrizioni dei valori \<filespec> equivalenti.

> [!NOTE]
> Quando si crea uno snapshot del database, le altre opzioni \<filespec> e la parola chiave PRIMARY non sono consentite.

AS SNAPSHOT OF *source_database_name* specifica che il database in fase di creazione è uno snapshot del database di origine specificato da *source_database_name*. Lo snapshot e il database di origine devono essere archiviati nella stessa istanza.

Per altre informazioni, vedere [Snapshot del database](#database-snapshots) nella sezione Remarks.

## <a name="remarks"></a>Osservazioni

Il backup del [database master](../../relational-databases/databases/master-database.md) deve essere eseguito ogni volta che si crea, si modifica o si rilascia un database utente.

L'istruzione `CREATE DATABASE` deve essere eseguita in modalità autocommit, che è la modalità predefinita di gestione delle transazioni, e non è consentita in una transazione esplicita o implicita.

È possibile usare un'istruzione `CREATE DATABASE` per creare un database e i file che lo archiviano. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa l'istruzione CREATE DATABASE tramite i passaggi seguenti:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa una copia del [database modello](../../relational-databases/databases/model-database.md) per inizializzare il database e i relativi metadati.
2. Un GUID di Service Broker viene assegnato al database.
3. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] compila quindi la parte rimanente del database con pagine vuote, ad eccezione delle pagine con dati interni che registrano la modalità di utilizzo dello spazio nel database.

In un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]è possibile specificare al massimo 32.767 database.

Ogni database ha un proprietario che può eseguire attività particolari nel database. Il proprietario è l'utente che crea il database. Il proprietario del database può essere modificato tramite [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).

Alcune funzionalità del database dipendono dalle caratteristiche o dalle funzionalità presenti nel file system per la disponibilità completa delle funzionalità del database. Ecco alcuni esempi di funzionalità che dipendono dal set di funzionalità del file system:

- DBCC CHECKDB
- FileStream
- Backup online tramite il Servizio Copia Shadow del volume e gli snapshot dei file
- Creazione di snapshot del database
- Filegroup di dati ottimizzati per la memoria

## <a name="database-files-and-filegroups"></a>Filegroup e file di database

Ogni database ha almeno due file, un *file primario* e un *file registro transazioni*, e almeno un filegroup. Per ogni database è possibile specificare un massimo di 32.767 file e 32.767 filegroup.

Durante la creazione di un database, creare file di dati di dimensioni corrispondenti alla quantità massima di dati che si prevede di includere nel database.

Per l'archiviazione dei file di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è consigliabile usare una rete di archiviazione (SAN), una rete basata su iSCSI o un disco collegato localmente, poiché questa configurazione ottimizza le prestazioni e l'affidabilità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="database-snapshots"></a>Snapshot del database

È possibile usare l'istruzione `CREATE DATABASE` per creare una visualizzazione statica, di sola lettura, uno *snapshot* del *database di origine*. Uno snapshot del database è consistente dal punto di vista transazionale con il database di origine al momento della creazione dello snapshot. Un database di origine può avere più snapshot.

> [!NOTE]
> Quando si crea uno snapshot del database, l'istruzione `CREATE DATABASE` non può far riferimento a file di log, file offline, file di ripristino e file inattivi.

Se la creazione di uno snapshot del database ha esito negativo, lo snapshot diventa sospetto e deve essere eliminato. Per altre informazioni, vedere [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

Ogni snapshot viene mantenuto fino a quando non viene eliminato tramite `DROP DATABASE`.

Per altre informazioni, vedere [Snapshot del database](../../relational-databases/databases/database-snapshots-sql-server.md).

## <a name="database-options"></a>Opzioni di database

Quando si crea un database, vengono impostate automaticamente diverse opzioni. Per un elenco di queste opzioni, vedere [Opzioni ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md).

## <a name="the-model-database-and-creating-new-databases"></a>Database modello e creazione di nuovi database

Tutti gli oggetti definiti dall'utente inclusi nel [database modello](../../relational-databases/databases/model-database.md) vengono copiati in tutti i nuovi database. È possibile aggiungere al database model qualsiasi oggetto che si desidera includere in tutti i database appena creati, ad esempio tabelle, viste, stored procedure, tipi di dati e così via.

Quando si specifica un'istruzione `CREATE DATABASE <database_name>` senza parametri delle dimensioni aggiuntivi, per il file di dati primario vengono usate le dimensioni del file primario nel database modello.

A meno che non si specifichi `FOR ATTACH`, ogni nuovo database eredita le impostazioni delle opzioni di database dal database modello. Ad esempio, l'opzione di database auto shrink viene impostata su **true** nel database modello e in tutti i nuovi database creati. Se si modificano le opzioni nel database model, queste nuove impostazioni vengono utilizzate in tutti i nuovi database creati. La modifica delle operazioni nel database model non ha effetto sui database esistenti. Se viene specificata l'opzione FOR ATTACH nell'istruzione CREATE DATABASE, i nuovi database ereditano le impostazioni delle opzioni di database dal database originale.

## <a name="viewing-database-information"></a>Visualizzazione delle informazioni sui database

Per restituire informazioni su database, file e filegroup, è possibile usare viste del catalogo, funzioni di sistema e stored procedure di sistema. Per altre informazioni, vedere [Viste di sistema](../language-reference.md).

## <a name="permissions"></a>Autorizzazioni

È necessaria l'autorizzazione `CREATE DATABASE`, `CREATE ANY DATABASE` o `ALTER ANY DATABASE`.

Per mantenere il controllo sull'utilizzo del disco per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'autorizzazione per la creazione dei database è in genere limitata a pochi account di accesso.

Nell'esempio seguente viene fornita l'autorizzazione per creare un database per l'utente del database Fay.

```sql
USE master;
GO
GRANT CREATE DATABASE TO [Fay];
GO
```

### <a name="permissions-on-data-and-log-files"></a>Autorizzazioni per i file di dati e di log

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono impostate autorizzazioni specifiche per i file di dati e di log in ogni database. Le autorizzazioni seguenti vengono impostate quando le operazioni elencate di seguito vengono eseguite in un database.

- Collegamento
- Esecuzione del backup
- Data di creazione
- Scollegamento
- Modifica per l'aggiunta di un nuovo file
- Ripristino

Con le autorizzazioni è possibile evitare che vengano accidentalmente alterati i file che si trovano in una directory con autorizzazioni aperte.

> [!NOTE]
> In [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] non vengono impostate autorizzazioni per i file di dati e di log.

## <a name="examples"></a>Esempi

### <a name="a-creating-a-database-without-specifying-files"></a>R. Creazione di un database senza specificare i file

Nell'esempio seguente viene creato il database `mytest` insieme al file di log delle transazioni e al file primario corrispondenti. Poiché l'istruzione non specifica elementi \<filespec>, le dimensioni del file di database primario corrispondono a quelle del file primario del database modello. Il file di log delle transazioni viene impostato sul valore più maggiore tra 512 kB e il 25% delle dimensioni del file di dati primario. Poiché MAXSIZE non è specificato, le dimensioni dei file possono aumentare fino a riempire lo spazio disponibile su disco. In questo esempio viene inoltre illustrato come eliminare l'eventuale database denominato `mytest` prima di creare il database `mytest`.

```sql
USE master;
GO
IF DB_ID (N'mytest') IS NOT NULL
DROP DATABASE mytest;
GO
CREATE DATABASE mytest;
GO
-- Verify the database files and sizes
SELECT name, size, size*1.0/128 AS [Size in MBs]
FROM sys.master_files
WHERE name = N'mytest';
GO
```

### <a name="b-creating-a-database-that-specifies-the-data-and-transaction-log-files"></a>B. Creazione di un database che specifica i file di dati e i file di log delle transazioni

Nell'esempio seguente viene creato il database `Sales`. Dal momento che la parola chiave PRIMARY non è specificata, il primo file, cioè `Sales_dat`, corrisponde al file primario. Poiché nel parametro SIZE non viene specificato il suffisso MB o KB per le dimensioni del file `Sales_dat` , viene utilizzato MB e le dimensioni del file vengono allocate in megabyte. Il backup del database `Sales_log` vengono allocate in megabyte perché nel parametro `MB` è stato specificato in modo esplicito il suffisso `SIZE` .

```sql
USE master;
GO
CREATE DATABASE Sales
ON
( NAME = Sales_dat,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\saledat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="c-creating-a-database-by-specifying-multiple-data-and-transaction-log-files"></a>C. Creazione di un database specificando più file di dati e più file di log delle transazioni

Nell'esempio seguente viene creato il database `Archive` che include tre file di dati da `100-MB` e due file del log delle transazioni da `100-MB`. Il file primario è il primo file dell'elenco e viene specificato in modo esplicito con la parola chiave `PRIMARY`. I file di log delle transazioni vengono specificati dopo le parole chiave `LOG ON`. Si notino le estensioni utilizzate per i file nell'opzione `FILENAME`: `.mdf` per i file di dati primari, `.ndf` per i file di dati secondari e `.ldf` per i file di log delle transazioni. In questo esempio il database viene collocato nell'unità `D:` anziché con il database `master`.

```sql
USE master;
GO
CREATE DATABASE Archive
ON
PRIMARY
    (NAME = Arch1,
    FILENAME = 'D:\SalesData\archdat1.mdf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch2,
    FILENAME = 'D:\SalesData\archdat2.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch3,
    FILENAME = 'D:\SalesData\archdat3.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20)
LOG ON
  (NAME = Archlog1,
    FILENAME = 'D:\SalesData\archlog1.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
  (NAME = Archlog2,
    FILENAME = 'D:\SalesData\archlog2.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20) ;
GO
```

### <a name="d-creating-a-database-that-has-filegroups"></a>D. Creazione di un database con filegroup

Nell'esempio seguente viene creato il database `Sales` che include i filegroup seguenti:

- Il filegroup primario con i file `Spri1_dat` e `Spri2_dat`. Gli incrementi specificati nel parametro FILEGROWTH per tali file corrispondono al `15%`.
- Un filegroup `SalesGroup1` con i file `SGrp1Fi1` e `SGrp1Fi2`.
- Un filegroup `SalesGroup2` con i file `SGrp2Fi1` e `SGrp2Fi2`.

In questo esempio i file di dati e di log vengono collocati in dischi diversi al fine di migliorare le prestazioni.

```sql
USE master;
GO
CREATE DATABASE Sales
ON PRIMARY
( NAME = SPri1_dat,
    FILENAME = 'D:\SalesData\SPri1dat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
( NAME = SPri2_dat,
    FILENAME = 'D:\SalesData\SPri2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
FILEGROUP SalesGroup1
( NAME = SGrp1Fi1_dat,
    FILENAME = 'D:\SalesData\SG1Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp1Fi2_dat,
    FILENAME = 'D:\SalesData\SG1Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
FILEGROUP SalesGroup2
( NAME = SGrp2Fi1_dat,
    FILENAME = 'D:\SalesData\SG2Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp2Fi2_dat,
    FILENAME = 'D:\SalesData\SG2Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'E:\SalesLog\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="e-attaching-a-database"></a>E. Collegamento di un database

Nell'esempio seguente viene scollegato il database `Archive` creato nell'esempio D, quindi viene collegato tramite la clausola `FOR ATTACH`. `Archive` è stato definito in modo da avere più dati e file di log. Tuttavia, poiché il percorso dei file non è stato modificato dopo la creazione, deve essere specificato solo il file primario nella clausola `FOR ATTACH`. A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], tutti i file full-text inclusi nel database in fase di collegamento verranno collegati assieme al database.

```sql
USE master;
GO
sp_detach_db Archive;
GO
CREATE DATABASE Archive
  ON (FILENAME = 'D:\SalesData\archdat1.mdf')
  FOR ATTACH ;
GO
```

### <a name="f-creating-a-database-snapshot"></a>F. Creazione di uno snapshot del database

L'esempio seguente crea lo snapshot del database `sales_snapshot0600`. Poiché uno snapshot del database è in sola lettura, non è possibile specificare un file di log. In conformità con la sintassi, viene specificato ogni file nel database di origine, mentre i filegroup non vengono specificati.

Il database di origine per questo esempio è il database `Sales` creato nell'esempio D.

```sql
USE master;
GO
CREATE DATABASE sales_snapshot0600 ON
    ( NAME = SPri1_dat, FILENAME = 'D:\SalesData\SPri1dat_0600.ss'),
    ( NAME = SPri2_dat, FILENAME = 'D:\SalesData\SPri2dt_0600.ss'),
    ( NAME = SGrp1Fi1_dat, FILENAME = 'D:\SalesData\SG1Fi1dt_0600.ss'),
    ( NAME = SGrp1Fi2_dat, FILENAME = 'D:\SalesData\SG1Fi2dt_0600.ss'),
    ( NAME = SGrp2Fi1_dat, FILENAME = 'D:\SalesData\SG2Fi1dt_0600.ss'),
    ( NAME = SGrp2Fi2_dat, FILENAME = 'D:\SalesData\SG2Fi2dt_0600.ss')
AS SNAPSHOT OF Sales ;
GO
```

### <a name="g-creating-a-database-and-specifying-a-collation-name-and-options"></a>G. Creazione di un database e specifica di un nome delle regole di confronto e delle opzioni

Nell'esempio seguente viene creato il database `MyOptionsTest`. Viene specificato un nome delle regole di confronto e le opzioni `TRUSTYWORTHY` e `DB_CHAINING` vengono impostate su `ON`.

```sql
USE master;
GO
IF DB_ID (N'MyOptionsTest') IS NOT NULL
DROP DATABASE MyOptionsTest;
GO
CREATE DATABASE MyOptionsTest
COLLATE French_CI_AI
WITH TRUSTWORTHY ON, DB_CHAINING ON;
GO
--Verifying collation and option settings.
SELECT name, collation_name, is_trustworthy_on, is_db_chaining_on
FROM sys.databases
WHERE name = N'MyOptionsTest';
GO
```

### <a name="h-attaching-a-full-text-catalog-that-has-been-moved"></a>H. Collegamento di un catalogo full-text che è stato spostato

Nell'esempio seguente viene collegato il catalogo full-text `AdvWksFtCat` insieme ai file di log e di dati di `AdventureWorks2012`. In questo esempio, il catalogo full-text viene spostato dalla posizione predefinita in una nuova posizione `c:\myFTCatalogs`. I file di dati e di log rimangono nelle posizioni predefinite.

```sql
USE master;
GO
--Detach the AdventureWorks2012 database
sp_detach_db AdventureWorks2012;
GO
-- Physically move the full text catalog to the new location.
--Attach the AdventureWorks2012 database and specify the new location of the full-text catalog.
CREATE DATABASE AdventureWorks2012 ON
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_data.mdf'),
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf'),
    (FILENAME = 'c:\myFTCatalogs\AdvWksFtCat')
FOR ATTACH;
GO
```

### <a name="i-creating-a-database-that-specifies-a-row-filegroup-and-two-filestream-filegroups"></a>I. Creazione di un database che specifica un filegroup di righe e due filegroup FILESTREAM

Nell'esempio seguente viene creato il database `FileStreamDB`. Il database viene creato con un filegroup di righe e due filegroup FILESTREAM. Ogni filegroup contiene un file:

- `FileStreamDB_data` contiene dati delle righe. Contiene un file, `FileStreamDB_data.mdf`, con il percorso predefinito.
- `FileStreamPhotos` contiene dati FILESTREAM. Contiene due contenitori di dati FILESTREAM, `FSPhotos` nel percorso `C:\MyFSfolder\Photos` e `FSPhotos2` nel percorso `D:\MyFSfolder\Photos`. È contrassegnato come filegroup FILESTREAM predefinito.
- `FileStreamResumes` contiene dati FILESTREAM. Contiene un contenitore di dati FILESTREAM, ossia `FSResumes` nel percorso `C:\MyFSfolder\Resumes`.

```sql
USE master;
GO
-- Get the SQL Server data path.
DECLARE @data_path nvarchar(256);
SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

 -- Execute the CREATE DATABASE statement.
EXECUTE ('CREATE DATABASE FileStreamDB
ON PRIMARY
    (
    NAME = FileStreamDB_data
    ,FILENAME = ''' + @data_path + 'FileStreamDB_data.mdf''
    ,SIZE = 10MB
    ,MAXSIZE = 50MB
    ,FILEGROWTH = 15%
    ),
FILEGROUP FileStreamPhotos CONTAINS FILESTREAM DEFAULT
    (
    NAME = FSPhotos
    ,FILENAME = ''C:\MyFSfolder\Photos''
-- SIZE and FILEGROWTH should not be specified here.
-- If they are specified an error will be raised.
, MAXSIZE = 5000 MB
    ),
    (
      NAME = FSPhotos2
      , FILENAME = ''D:\MyFSfolder\Photos''
      , MAXSIZE = 10000 MB
     ),
FILEGROUP FileStreamResumes CONTAINS FILESTREAM
    (
    NAME = FileStreamResumes
    ,FILENAME = ''C:\MyFSfolder\Resumes''
    )
LOG ON
    (
    NAME = FileStream_log
    ,FILENAME = ''' + @data_path + 'FileStreamDB_log.ldf''
    ,SIZE = 5MB
    ,MAXSIZE = 25MB
    ,FILEGROWTH = 5MB
    )'
);
GO
```

### <a name="j-creating-a-database-that-has-a-filestream-filegroup-with-multiple-files"></a>J. Creazione di un database contenente un filegroup FILESTREAM con più file

Nell'esempio seguente viene creato il database `BlobStore1`. Il database viene creato con un filegroup di righe e un filegroup FILESTREAM, `FS`. Nel filegroup FILESTREAM sono contenuti due file, vale a dire `FS1` e `FS2`. Successivamente il database viene modificato con l'aggiunta di un terzo file, `FS3`, al filegroup FILESTREAM.

```sql
USE master;
GO

CREATE DATABASE [BlobStore1]
CONTAINMENT = NONE
ON PRIMARY
(
    NAME = N'BlobStore1',
    FILENAME = N'C:\BlobStore\BlobStore1.mdf',
    SIZE = 100MB,
    MAXSIZE = UNLIMITED,
    FILEGROWTH = 1MB
),
FILEGROUP [FS] CONTAINS FILESTREAM DEFAULT
(  
    NAME = N'FS1',
    FILENAME = N'C:\BlobStore\FS1',
    MAXSIZE = UNLIMITED
),
(
    NAME = N'FS2',
    FILENAME = N'C:\BlobStore\FS2',
    MAXSIZE = 100MB
)
LOG ON
(
    NAME = N'BlobStore1_log',
    FILENAME = N'C:\BlobStore\BlobStore1_log.ldf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 1MB
);
GO

ALTER DATABASE [BlobStore1]
ADD FILE
(
    NAME = N'FS3',
    FILENAME = N'C:\BlobStore\FS3',
    MAXSIZE = 100MB
)
TO FILEGROUP [FS];
GO
```

## <a name="see-also"></a>Vedere anche

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [Collegamento e scollegamento di database](../../relational-databases/databases/database-detach-and-attach-sql-server.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)
- [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)
- [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)
- [snapshot del database](../../relational-databases/databases/database-snapshots-sql-server.md)
- [Spostare file del database](../../relational-databases/databases/move-database-files.md)
- [Database](../../relational-databases/databases/databases.md)
- [BLOB - Dati BLOB](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Database SQL \*_**
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>Database SQL

## <a name="overview"></a>Panoramica

In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] questa istruzione può essere usata con un server SQL di Azure per creare un database singolo o un database in un pool elastico. Con questa istruzione si specificano il nome del database, le regole di confronto, le dimensioni massime, l'edizione, obiettivo di servizio e, se applicabile, il pool elastico per il nuovo database. Può anche essere usata per creare il database in un pool elastico. Inoltre, può essere usata per creare una copia del database in un altro server di database SQL.

## <a name="syntax"></a>Sintassi

### <a name="create-a-database"></a>Creazione di un database

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
{
  (<edition_options> [, ...n])
}
[ WITH <with_options> [,..n]]
[;]

<with_options> ::=
{
  CATALOG_COLLATION = { DATABASE_DEFAULT | SQL_Latin1_General_CP1_CI_AS }
  | BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
}

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | ( EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale' }
  | SERVICE_OBJECTIVE =
    { 'Basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
}
```

### <a name="copy-a-database"></a>Copiare un database

```syntaxsql
CREATE DATABASE database_name
    AS COPY OF [source_server_name.] source_database_name
    [ ( SERVICE_OBJECTIVE =
      { 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
   ]
[;]
```

## <a name="arguments"></a>Argomenti

*database_name* è il nome del nuovo database. Il nome deve essere univoco nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed essere conforme alle regole di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per gli identificatori. Per altre informazioni, vedere [Identificatori](../../relational-databases/databases/database-identifiers.md).

*collation_name* specifica le regole di confronto predefinite per il database. È possibile usare nomi di regole di confronto di Windows o SQL. Se non vengono specificate, al database vengono assegnate le regole di confronto predefinite, ovvero SQL_Latin1_General_CP1_CI_AS.

Per altre informazioni sui nomi delle regole di confronto Windows e SQL, vedere [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

CATALOG_COLLATION specifica le regole di confronto predefinite per il catalogo di metadati. *DATABASE_DEFAULT* specifica che il catalogo di metadati usato per le visualizzazioni e le tabelle di sistema viene sottoposto a confronto per la corrispondenza con le regole di confronto predefinite per il database. Questo è il comportamento disponibile in SQL Server.

*SQL_Latin1_General_CP1_CI_AS* specifica che il catalogo di metadati usato per le visualizzazioni e le tabelle di sistema viene sottoposto a confronto con regole di confronto fisse SQL_Latin1_General_CP1_CI_AS. Questa è l'impostazione predefinita del database SQL di Azure se non è specificata un'altra opzione.

BACKUP_STORAGE_REDUNDANCY specifica la modalità di replica dei backup per il ripristino temporizzato e dei backup con conservazione a lungo termine di un database. Il ripristino geografico o la possibilità di eseguire il ripristino da un'interruzione a livello di area è disponibile solo quando il database viene creato con ridondanza dell'archivio di backup 'GEO'. Se non specificato in modo esplicito, i database creati con T-SQL usano l'archivio di backup con ridondanza geografica. 

> [!IMPORTANT]
> L'opzione BACKUP_STORAGE_REDUNDANCY per il database SQL di Azure è attualmente disponibile in anteprima pubblica nell'area Brasile meridionale ed è disponibile a livello generale solo nell'area Azure Asia sud-orientale.  

EDITION specifica il livello di servizio del database.

Database singoli e in pool. I valori disponibili sono: 'Basic', 'Standard', 'Premium', 'GeneralPurpose', 'BusinessCritical' e 'Hyperscale'.

MAXSIZE specifica le dimensioni massime del database. MAXSIZE deve essere valido per il livello del servizio specificato in EDITION. Nella tabella seguente sono elencati i valori MAXSIZE supportati e i valori predefiniti (P) per i livelli del servizio.

> [!NOTE]
> L'argomento **MAXSIZE** non è applicabile ai singoli database nel livello di servizio Hyperscale. I database di livello Hyperscale possono crescere in base alle necessità, fino a 100 TB. Il servizio database SQL aggiunge automaticamente l'archiviazione: non è necessaria impostare le dimensioni massime.

**Modello DTU per database singoli e in pool in un server di database SQL**

|**MAXSIZE**|**Base**|**S0-S2**|**S3-S12**|**P1-P6**| **P11-P15** |
|:---|:---|:---|:---|:---|:---|
|100 MB|√|√|√|√|√|
|500 MB|√|√|√|√|√|
|1 GB|√|√|√|√|√|
|2 GB|√ (P)|√|√|√|√|
|5 GB|N/D|√|√|√|√|
|10 GB|N/D|√|√|√|√|
|20 GB|N/D|√|√|√|√|
|30 GB|N/D|√|√|√|√|
|40 GB|N/D|√|√|√|√|
|50 GB|N/D|√|√|√|√|
|100 GB|N/D|√|√|√|√|
|150 GB|N/D|√|√|√|√|
|200 GB|N/D|√|√|√|√|
|250 GB|N/D|√ (P)|√ (P)|√|√|
|300 GB|N/D|N/D|√|√|√|
|400 GB|N/D|N/D|√|√|√|
|500 GB|N/D|N/D|√|√ (P)|√|
|750 GB|N/D|N/D|√|√|√|
|1024 GB|N/D|N/D|√|√|√ (P)|
|Da 1024 GB fino a 4096 GB con incrementi di 256 GB* |N/D|N/D|N/D|N/D|√|√|

\* P11 e P15 consentono un valore massimo di MAXSIZE pari a 4 TB. Le dimensioni predefinite sono 1024 GB. P11 e P15 possono usare fino a 4 TB di spazio di archiviazione incluso senza addebiti aggiuntivi. Nel livello Premium, MAXSIZE maggiore di 1 TB è attualmente disponibile nelle aree seguenti: Stati Uniti orientali 2, Stati Uniti occidentali, US Gov Virginia, Europa occidentale, Germania centrale, Asia sud-orientale, Giappone orientale, Australia orientale, Canada centrale e Canada orientale. Per altri dettagli relativi ai limiti delle risorse per il modello DTU, vedere [DTU resource limits](/azure/sql-database/sql-database-dtu-resource-limits) (Limiti delle risorse DTU).

Il valore MAXSIZE per il modello DTU, se specificato, deve essere un valore valido presente nella tabella precedente per il livello di servizio specificato.

**Modello vCore**

**Utilizzo generico - Calcolo con provisioning - Gen4 (parte 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Dimensioni massime dei dati (GB)|1024|1024|1024|1536|1536|1536|

**Utilizzo generico - Calcolo con provisioning - Gen4 (parte 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Dimensioni massime dei dati (GB)|1536|3072|3072|3072|4096|4096|

**Utilizzo generico - Calcolo con provisioning - Gen5 (parte 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Dimensioni massime dei dati (GB)|1024|1024|1024|1536|1536|1536|1536|

**Utilizzo generico - Calcolo con provisioning - Gen5 (parte 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Dimensioni massime dei dati (GB)|3072|3072|3072|4096|4096|4096|4096|

**Utilizzo generico - Calcolo con provisioning - Serie Fsv2 (parte 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Dimensioni massime dei dati (GB)|1024|1024|1024|1024|1536|1536|

**Utilizzo generico - Calcolo con provisioning - Serie Fsv2 (parte 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Dimensioni massime dei dati (GB)|1536|1536|3072|3072|4096|

**Utilizzo generico - Calcolo serverless - Gen5 (parte 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Numero massimo vCore|1|2|4|6|8|

**Utilizzo generico - Calcolo serverless - Gen5 (parte 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|Numero massimo vCore|10|12|14|16|

**Utilizzo generico - Calcolo serverless - Gen5 (parte 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Numero massimo vCore|18|20|24|32|40|

**Business critical - Calcolo con provisioning - Gen4 (parte 1)**

|Dimensioni di calcolo (obiettivo di servizio)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Dimensioni massime dei dati (GB)|1024|1024|1024|1024|1024|1024|

**Business critical - Calcolo con provisioning - Gen4 (parte 2)**

|Dimensioni di calcolo (obiettivo di servizio)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Dimensioni massime dei dati (GB)|1024|1024|1024|1024|1024|1024|

**Business critical - Calcolo con provisioning - Gen5 (parte 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Dimensioni massime dei dati (GB)|1024|1024|1024|1536|1536|1536|1536|

**Business critical - Calcolo con provisioning - Gen5 (parte 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Dimensioni massime dei dati (GB)|3072|3072|3072|4096|4096|4096|4096|

**Business critical - Calcolo con provisioning - Serie M (parte 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Dimensioni massime dei dati (GB)|512|640|768|896|1024|1152|

**Business critical - Calcolo con provisioning - Serie M (parte 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Dimensioni massime dei dati (GB)|1280|1536|2048|4096|4096|

Se non viene impostato alcun valore `MAXSIZE` quando si usa il modello vCore, il valore predefinito è 32 GB. Per altri dettagli relativi ai limiti delle risorse per il modello vCore, vedere [Limiti delle risorse vCore](/azure/sql-database/sql-database-dtu-resource-limits).

Le seguenti regole vengono applicate agli argomenti MAXSIZE ed EDITION:

- Se il valore di EDITION è specificato e il valore di MAXSIZE viene omesso, viene utilizzato il valore predefinito dell'edizione. Se ad esempio EDITION è impostato su Standard e MAXSIZE non è specificato, il valore di MAXSIZE viene automaticamente impostato su 250 MB.
- Se né MAXSIZE né EDITION sono specificati, EDITION viene impostato su `GeneralPurpose` e MAXSIZE viene impostato su 32 GB.

SERVICE_OBJECTIVE

- **Per database singoli e in pool**

  - Specifica le dimensioni di calcolo (obiettivo di servizio). I valori disponibili per l'obiettivo di servizio sono: `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Per database serverless**
- 
  - Specifica le dimensioni di calcolo (obiettivo di servizio). I valori disponibili per l'obiettivo di servizio sono: `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Per database singoli nel livello di servizio Hyperscale**

  - Specifica le dimensioni di calcolo (obiettivo di servizio). I valori disponibili per l'obiettivo di servizio sono: `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Per le descrizioni degli obiettivi di servizio e altre informazioni su dimensioni, edizioni e combinazioni di obiettivi di servizio, vedere [Azure SQL Database Service Tiers](/azure/sql-database/sql-database-service-tiers) (Livelli di servizio del database SQL di Azure). Se SERVICE_OBJECTIVE non è supportato da EDITION, viene visualizzato un errore. Per cambiare il valore di SERVICE_OBJECTIVE da un livello a un altro (ad esempio da S1 a P1), è necessario modificare anche il valore EDITION. Per le descrizioni degli obiettivi di servizio e altre informazioni su dimensioni, edizioni e combinazioni di obiettivi di servizio, vedere [Azure SQL Database Service Tiers and Performance Levels](/azure/azure-sql/database/purchasing-models) (Livelli di servizio e livelli di prestazioni del database SQL di Azure), [DTU resource limits](/azure/sql-database/sql-database-dtu-resource-limits) (Limiti delle risorse DTU) e [vCore resource limits](/azure/sql-database/sql-database-dtu-resource-limits) (Limiti delle risorse vCore). Il supporto per gli obiettivi di servizio PRS è stato rimosso. In caso di domande, usare l'alias di posta elettronica seguente: premium-rs@microsoft.com.

ELASTIC_POOL (name = \<elastic_pool_name>) **Si applica a:** solo database singoli e in pool. Non è applicabile ai database nel livello di servizio Hyperscale.
Per creare un nuovo database in un pool di database elastico, impostare SERVICE_OBJECTIVE del database su ELASTIC_POOL e specificare il nome del pool. Per altre informazioni, vedere [Creare e gestire un pool elastico in un database SQL](/azure/azure-sql/database/elastic-pool-overview).

AS COPY OF [source_server_name.]source_database_name **Si applica a:** solo database singoli e in pool.
Per la copia di un database nello stesso server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o in un server diverso.

*source_server_name* Nome del server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in cui si trova il database di origine. Questo parametro è facoltativo se il database di origine e il database di destinazione devono trovarsi nello stesso server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

> [!NOTE]
> l'argomento `AS COPY OF` non supporta nomi di dominio univoci completi. In altre parole, se il nome di dominio completo del server è `serverName.database.windows.net`, usare solo `serverName` durante la copia del database.

*nome_database_di_origine*

Nome del database di cui eseguire la copia.

## <a name="remarks"></a>Osservazioni

I database nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] presentano varie impostazioni predefinite impostate alla creazione del database. Per altre informazioni su queste impostazioni predefinite, vedere l'elenco di valori in [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

`MAXSIZE` consente di limitare le dimensioni del database. Se le dimensioni del database raggiungono il valore di `MAXSIZE`, viene visualizzato il codice di errore 40544. In questo caso, non è possibile inserire o aggiornare dati, né creare nuovi oggetti quali tabelle, stored procedure, viste e funzioni. È tuttavia ancora possibile leggere ed eliminare dati, troncare tabelle, eliminare tabelle e indici e ricompilare indici. È quindi possibile aggiornare `MAXSIZE` a un valore maggiore delle dimensioni correnti del database o eliminare alcuni dati per liberare spazio di archiviazione. Potrebbe verificarsi un ritardo di quindici minuti prima di poter inserire nuovi dati.

Per cambiare le dimensioni o i valori degli obiettivi di servizio in un secondo momento, usare [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true).

L'argomento `CATALOG_COLLATION` è disponibile solo durante la creazione del database.

## <a name="database-copies"></a>Copie di database

**Si applica a:** solo database singoli e in pool.

La copia di un database tramite l'istruzione `CREATE DATABASE` è un'operazione asincrona. Pertanto, una connessione al server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] non è necessaria per la durata totale del processo di copia. L'istruzione `CREATE DATABASE` restituisce il controllo all'utente dopo la creazione della voce in sys.databases e prima che l'operazione di copia del database venga completata. In altre parole, l'istruzione `CREATE DATABASE` ha esito positivo quando la copia del database è ancora in corso.

- Monitoraggio del processo di copia in un server [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]: eseguire query sulle colonne `percentage_complete` o `replication_state_desc` in [dm_database_copies](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md) o sulla colonna `state` nella visualizzazione **sys.databases**. È possibile usare anche la visualizzazione [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) perché restituisce lo stato delle operazioni del database, inclusa la copia del database.

Al termine del processo di copia, il database di destinazione è transazionalmente coerente con il database di origine.

La sintassi e le regole semantiche seguenti si applicano all'utilizzo dell'argomento `AS COPY OF`:

- Il nome del server di origine e il nome del server per la destinazione della copia potrebbe essere uguale o diverso. Se corrispondono, questo parametro è facoltativo e il contesto del server della sessione corrente viene usato per impostazione predefinita.
- I nomi dei database di origine e di destinazione devono essere specificati, univoci e conformi alle regole di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per gli identificatori. Per altre informazioni, vedere [Identificatori](../../relational-databases/databases/database-identifiers.md).
- L'istruzione `CREATE DATABASE` deve essere eseguita nel contesto del database master del server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in cui il nuovo database verrà creato.
- Al termine della copia, il database di destinazione deve essere gestito come database indipendente. È possibile eseguire le istruzioni `ALTER DATABASE` e `DROP DATABASE` per il nuovo database indipendentemente dal database di origine. È inoltre possibile copiare il nuovo database in un altro nuovo database.
- Il database di origine continuerà a essere accessibile durante la copia del database.

Per altre informazioni, vedere [Create a copy of an Azure SQL database using Transact-SQL](/azure/azure-sql/database/database-copy) (Creare una copia di un database SQL di Azure mediante Transact-SQL).

> [!IMPORTANT]
> Per impostazione predefinita, la copia del database viene creata con la stessa ridondanza dell'archivio di backup del database di origine. La modifica della ridondanza dell'archivio di backup durante la creazione di una copia del database non è supportata tramite T-SQL. 


## <a name="permissions"></a>Autorizzazioni

Per creare un database, l'account di accesso deve essere uno dei seguenti:

- Account di accesso principale di livello server
- Account amministratore di Azure AD per il server SQL di Azure
- Un account di accesso membro del ruolo del database `dbmanager`

**Requisiti aggiuntivi per l'uso della sintassi `CREATE DATABASE ... AS COPY OF`** : l'account di accesso che esegue l'istruzione nel server locale deve anche essere almeno un account `db_owner` nel server di origine. Se l'account di accesso è basato sull'autenticazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'account di accesso che esegue l'istruzione nel server locale deve avere un account di accesso corrispondente nel server [!INCLUDE[ssSDS](../../includes/sssds-md.md)] di origine, con nome e password identici.

## <a name="examples"></a>Esempi

### <a name="simple-example"></a>Esempio semplice

Esempio semplice per la creazione di un database.

```sql
CREATE DATABASE TestDB1;
```

### <a name="simple-example-with-edition"></a>Esempio semplice con Edition

Esempio semplice per la creazione di un database per utilizzo generico.

```sql
CREATE DATABASE TestDB2
( EDITION = 'GeneralPurpose' );
```

### <a name="example-with-additional-options"></a>Esempio con opzioni aggiuntive

Esempio che usa varie opzioni.

```sql
CREATE DATABASE hito
COLLATE Japanese_Bushu_Kakusu_100_CS_AS_KS_WS
( MAXSIZE = 500 MB, EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_GEN4_8' ) ;
```

### <a name="creating-a-copy"></a>Creazione di una copia

Esempio di creazione di una copia di un database.

**Si applica a:** solo database singoli e in pool.

```sql
CREATE DATABASE escuela
AS COPY OF school;
```

### <a name="creating-a-database-in-an-elastic-pool"></a>Creazione di un database in un pool elastico

Crea un nuovo database nel pool denominato S3M100:

**Si applica a:** solo database singoli e in pool.

```sql
CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = S3M100 ) ) ;
```

### <a name="creating-a-copy-of-a-database-on-another-server"></a>Creazione di una copia di un database in un altro server

L'esempio seguente crea una copia del database db_original denominata db_copy nelle dimensioni di calcolo P2 (obiettivo di servizio) per un singolo database. Questo vale indipendentemente dal fatto che db_original si trovi in un pool elastico o nelle dimensioni di calcolo (obiettivo di servizio) per un singolo database.

**Si applica a:** solo database singoli e in pool.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original ( SERVICE_OBJECTIVE = 'P2' );
```

L'esempio seguente crea una copia del database db_original denominata db_copy in un pool elastico con nome ep1. Questo vale indipendentemente dal fatto che db_original si trovi in un pool elastico o nelle dimensioni di calcolo (obiettivo di servizio) per un singolo database. Se db_original si trova in un pool elastico con un nome diverso, db_copy viene comunque creato in ep1.

**Si applica a:** solo database singoli e in pool.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original
  (SERVICE_OBJECTIVE = ELASTIC_POOL( name = ep1 ) ) ;
```

### <a name="create-database-with-specified-catalog-collation-value"></a>Creare un database con il valore di regole di confronto del catalogo specificato

L'esempio seguente imposta le regole di confronto del catalogo su DATABASE_DEFAULT durante la creazione del database. In questo modo le regole di confronto del catalogo vengono impostate in modo da corrispondere alle regole di confronto del database.

```sql
CREATE DATABASE TestDB3 COLLATE Japanese_XJIS_140 (MAXSIZE = 100 MB, EDITION = 'Basic')
  WITH CATALOG_COLLATION = DATABASE_DEFAULT
```

### <a name="create-database-using-zone-redundancy-for-backups"></a>Creare database usando la ridondanza della zona per i backup

Nell'esempio seguente viene impostata la ridondanza della zona per i backup del database. Sia i backup per il ripristino temporizzato che i backup con conservazione a lungo termine (se configurati) useranno la stessa ridondanza dell'archivio di backup.

```sql
CREATE DATABASE test_zone_redundancy 
  WITH BACKUP_STORAGE_REDUNDANCY = 'ZONE';
```

## <a name="see-also"></a>Vedere anche

- [sys.dm_database_copies - Database SQL di Azure](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)
- [ALTER DATABASE - Database SQL di Azure](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita di SQL \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Istanza gestita di SQL di Azure

## <a name="overview"></a>Panoramica

In Istanza gestita di SQL di Azure questa istruzione viene usata per creare un database. Quando si crea un database in un'istanza gestita, si specificano il nome del database e le regole di confronto.

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;]
```

> [!IMPORTANT]
> Per aggiungere file o configurare l'indipendenza per un database in un'istanza gestita, usare l'istruzione [ALTER DATABASE](alter-database-transact-sql.md?tabs=sqldbmi).

## <a name="arguments"></a>Argomenti

*database_name* è il nome del nuovo database. Il nome deve essere univoco in SQL Server ed essere conforme alle regole di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per gli identificatori. Per altre informazioni, vedere [Identificatori](../../relational-databases/databases/database-identifiers.md).

*collation_name* specifica le regole di confronto predefinite per il database. È possibile usare nomi di regole di confronto di Windows o SQL. Se non vengono specificate, al database vengono assegnate le regole di confronto predefinite, ovvero SQL_Latin1_General_CP1_CI_AS.

Per altre informazioni sui nomi delle regole di confronto Windows e SQL, vedere [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

## <a name="remarks"></a>Osservazioni

I database nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] presentano varie impostazioni predefinite impostate alla creazione del database. Per altre informazioni su queste impostazioni predefinite, vedere l'elenco di valori in [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!IMPORTANT]
> L'istruzione `CREATE DATABASE` deve essere l'unica istruzione in un batch [!INCLUDE[tsql](../../includes/tsql-md.md)].

`CREATE DATABASE` prevede le limitazioni seguenti:

- Non è possibile definire file e filegroup.
- Le opzioni `WITH` non sono supportate.

  > [!TIP]
  > Come soluzione alternativa, usare [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) dopo `CREATE DATABASE` per impostare le opzioni di database e aggiungere i file.

## <a name="permissions"></a>Autorizzazioni

Per creare un database, l'account di accesso deve essere uno dei seguenti:

- Account di accesso principale di livello server
- Account amministratore di Azure AD per il server SQL di Azure
- Un account di accesso membro del ruolo del database `dbcreator`

## <a name="examples"></a>Esempi

### <a name="simple-example"></a>Esempio semplice

Esempio semplice per la creazione di un database.

```sql
CREATE DATABASE TestDB1;
```

## <a name="see-also"></a>Vedere anche

Vedere [ALTER DATABASE](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="overview"></a>Panoramica

In Azure Synapse questa istruzione può essere usata con un server di database SQL di Azure per creare un database di Analisi SQL. Con questa istruzione è possibile specificare il nome del database, le regole di confronto, le dimensioni massime, l'edizione e l'obiettivo di servizio.

## <a name="syntax"></a>Sintassi

### <a name="dedicated-sql-pool"></a>[Pool SQL dedicato](#tab/sqlpool)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
(
    [ MAXSIZE = {
          250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 | 30720
        | 40960 | 51200 | 61440 | 71680 | 81920 | 92160 | 102400
        | 153600 | 204800 | 245760
      } GB ,
    ]
    EDITION = 'datawarehouse',
    SERVICE_OBJECTIVE = {
         'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' | 'DW600'
        | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' | 'DW3000' | 'DW6000'
        |'DW100c' | 'DW200c' | 'DW300c' | 'DW400c' | 'DW500c'
        | 'DW1000c' | 'DW1500c' | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c'
        | 'DW6000c' | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
    }
)
[;]
```
### <a name="serverless-sql-pool"></a>[Pool SQL serverless](#tab/sqlod)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;] 
```
---

## <a name="arguments"></a>Argomenti

*database_name* è il nome del nuovo database. Questo nome deve essere univoco in SQL Server, che può ospitare sia database [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sia database [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Deve anche rispettare le regole [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per gli identificatori. Per altre informazioni, vedere [Identificatori](../../relational-databases/databases/database-identifiers.md).

*collation_name* specifica le regole di confronto predefinite per il database. È possibile usare nomi di regole di confronto di Windows o SQL. Se non vengono specificate, al database vengono assegnate le regole di confronto predefinite, ovvero SQL_Latin1_General_CP1_CI_AS.

Per altre informazioni sui nomi delle regole di confronto Windows e SQL, vedere [COLLATE (Transact-SQL)](./collations.md).

*EDITION* specifica il livello del servizio del database. Per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] usare 'datawarehouse'.

*MAXSIZE*: il valore predefinito è 245.760 GB (240 TB).

**Si applica a:** ottimizzato per il calcolo di prima generazione

Dimensioni massime consentite per il database. Le dimensioni del database non possono superare il valore di MAXSIZE.

**Si applica a:** ottimizzato per il calcolo di seconda generazione

Dimensioni massime consentite per i dati rowstore nel database. Le dimensioni dei dati archiviati nelle tabelle rowstore, nel deltastore di un indice columnstore o in un indice non cluster in un indice columnstore cluster non possono superare MAXSIZE. I dati compressi in formato columnstore non hanno un limite di dimensioni e non sono vincolati dal valore MAXSIZE.

SERVICE_OBJECTIVE specifica le dimensioni di calcolo (obiettivo di servizio). Per altre informazioni sugli obiettivi di servizio per Azure Synapse, vedere [Unità Data Warehouse (DWU)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="general-remarks"></a>Osservazioni generali

Usare [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) per visualizzare le proprietà del database.

Usare [ALTER DATABASE - Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) per cambiare le dimensioni massime o i valori degli obiettivi di servizio in un secondo momento.

Azure Synapse è impostato su COMPATIBILITY_LEVEL 130 e non può essere modificato. Per altri dettagli, vedere [Improved Query Performance with Compatibility Level 130 in Azure SQL Database](./alter-database-transact-sql-compatibility-level.md) (Prestazioni di query migliorate con Compatibility Level 130 nel database SQL di Azure).

## <a name="permissions"></a>Autorizzazioni

Autorizzazioni necessarie:

- Accesso principale di livello server (creato dal processo di provisioning) oppure
- Membro del ruolo del database `dbmanager`.

## <a name="error-handling"></a>Gestione degli errori

Se le dimensioni del database raggiungono il valore MAXSIZE, viene visualizzato il codice di errore 40544. In questo caso non è possibile inserire e aggiornare dati, né creare nuovi oggetti quali tabelle, stored procedure, viste e funzioni. È ancora possibile leggere ed eliminare dati, troncare tabelle, eliminare tabelle e indici e ricompilare indici. È quindi possibile aggiornare MAXSIZE a un valore maggiore delle dimensioni correnti del database o eliminare alcuni dati per liberare spazio di archiviazione. Potrebbe verificarsi un ritardo di quindici minuti prima di poter inserire nuovi dati.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

È necessario essere connessi al database master per creare un nuovo database.

L'istruzione `CREATE DATABASE` deve essere l'unica istruzione in un batch [!INCLUDE[tsql](../../includes/tsql-md.md)].

Non è possibile modificare le regole di confronto del database dopo la creazione del database stesso.

## <a name="examples-sssdwfull"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

### <a name="a-simple-example"></a>R. Esempio semplice
Esempio semplice per la creazione di un database del data warehouse. Crea il database con le dimensioni massime più ridotte, ovvero 10240 GB, le regole di confronto predefinite, ovvero SQL_Latin1_General_CP1_CI_AS e la potenza di elaborazione più ridotta, pari a DW100.

```sql
CREATE DATABASE TestDW
(EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
```

### <a name="b-create-a-data-warehouse-database-with-all-the-options"></a>B. Creare un database del data warehouse con tutte le opzioni
Esempio di creazione di un data warehouse di 10 TB usando tutte le opzioni.

```sql
CREATE DATABASE TestDW COLLATE Latin1_General_100_CI_AS_KS_WS
(MAXSIZE = 10240 GB, EDITION = 'datawarehouse', SERVICE_OBJECTIVE = 'DW1000');
```

## <a name="see-also"></a>Vedere anche

- [ALTER DATABASE- Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [CREATE TABLE- Azure Synapse Analytics](../../t-sql/statements/create-table-azure-sql-data-warehouse.md)
- [DROP DATABASE - Transact-SQL](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Database SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\*Piattaforma di strumenti<br />analitici (PDW)\*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema della piattaforma di analisi

## <a name="overview"></a>Panoramica

Nella piattaforma di strumenti analitici questa istruzione si usa per creare un nuovo database in un'appliance della piattaforma stessa. Usare questa istruzione per creare tutti i file associati a un database di appliance e per impostare le opzioni relative alle dimensioni massime e all'aumento automatico per le tabelle e i log delle transazioni del database stesso.

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE DATABASE database_name
WITH (
    [ AUTOGROW = ON | OFF , ]
    REPLICATED_SIZE = replicated_size [ GB ] ,
    DISTRIBUTED_SIZE = distributed_size [ GB ] ,
    LOG_SIZE = log_size [ GB ] )
[;]
```

## <a name="arguments"></a>Argomenti

*database_name* è il nome del nuovo database. Per altre informazioni sui nomi di database consentiti, vedere le sezioni relative alle regole di denominazione degli oggetti e ai nomi di database riservati nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

AUTOGROW = ON | **OFF** specifica se i parametri *replicated_size*, *distributed_size* e *log_size* per il database aumenteranno automaticamente in base alle esigenze oltre le dimensioni specificate. Il valore predefinito è **OFF**.

Se AUTOGROW corrisponde a ON, *replicated_size*, *distributed_size* e *log_size* aumenteranno secondo necessità (non in blocchi delle dimensioni specificate inizialmente) a ogni inserimento o aggiornamento di dati o quando vengono eseguite altre azioni che richiedono più spazio di archiviazione di quanto ne sia già stato allocato.

Se AUTOGROW corrisponde a OFF, le dimensioni non aumenteranno automaticamente. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] restituisce un errore se si tenta di eseguire un'azione che richiede l'aumento di *replicated_size*, *distributed_size* o *log_size* oltre i rispettivi valori specificati.

L'impostazione di AUTOGROW (ON o OFF) si applica a tutte le dimensioni. Non è ad esempio possibile impostare AUTOGROW su ON per *log_size* ma non per *replicated_size*.

*replicated_size* [ GB ] è un numero positivo. Imposta le dimensioni (in GB, con un valore intero o decimale) per lo spazio totale allocato per le tabelle replicate e per i dati corrispondenti *in ogni nodo di calcolo*. Per i requisiti minimo e massimo di *replicated_size*, vedere la sezione corrispondente nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Se AUTOGROW corrisponde a ON, è consentito l'aumento delle tabelle replicate oltre il limite impostato.

Se AUTOGROW corrisponde a OFF, verrà restituito un errore se un utente tenta di creare una nuova tabella replicata, di inserire dati in una tabella replicata esistente o di aggiornare quest'ultima in modo tale da aumentarne le dimensioni oltre il valore di *replicated_size*.

*distributed_size* [ GB ] è un numero positivo. Dimensioni (in GB, con un valore intero o decimale) per lo spazio totale allocato per le tabelle distribuite e per i dati corrispondenti *nell'intera appliance*. Per i requisiti minimo e massimo di *distributed_size*, vedere la sezione corrispondente nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Se AUTOGROW corrisponde a ON, è consentito l'aumento delle tabelle distribuite oltre il limite impostato.

Se AUTOGROW corrisponde a OFF, verrà restituito un errore se un utente tenta di creare una nuova tabella distribuita, di inserire dati in una tabella distribuita esistente o di aggiornare quest'ultima in modo tale da aumentarne le dimensioni oltre il valore di *distributed_size*.

*log_size* [ GB ] è un numero positivo. Dimensioni (in GB, con un valore intero o decimale) per il log delle transazioni *nell'intera appliance*.

Per i requisiti minimo e massimo di *log_size*, vedere la sezione corrispondente nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Se AUTOGROW corrisponde a ON, è consentito l'aumento del file di log oltre il limite impostato. Usare l'istruzione [DBCC SHRINKLOG (Azure Synapse Analytics)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) per ridurre le dimensioni dei file di log fino alle dimensioni originali.

Se AUTOGROW corrisponde a OFF, verrà restituito un errore per qualsiasi azione che aumenti le dimensioni del log in un singolo nodo di calcolo oltre il valore di *log_size*.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione `CREATE ANY DATABASE` nel database master o l'appartenenza al ruolo predefinito del server **sysadmin**.

Nell'esempio seguente viene fornita l'autorizzazione per creare un database per l'utente del database Fay.

```sql
USE master;
GO
GRANT CREATE ANY DATABASE TO [Fay];
GO
```

## <a name="general-remarks"></a>Osservazioni generali

I database vengono creati con livello di compatibilità 120, corrispondente al livello di compatibilità per [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. In questo modo il database sarà in grado di usare tutte le funzionalità [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] che usano PDW.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

L'istruzione CREATE DATABASE non è consentita in una transazione esplicita. Per altre informazioni, vedere [Istruzioni](../../t-sql/statements/statements.md).

Per informazioni sui vincoli minimi e massimi nei database, vedere la sezione relativa ai valori minimi e massimi nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Al momento della creazione di un database, deve essere disponibile spazio sufficiente *in ogni nodo di calcolo* per allocare il totale combinato delle dimensioni seguenti:

- Database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con tabelle di dimensioni corrispondenti a *replicated_table_size*.
- Database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con tabelle di dimensioni corrispondenti a (*distributed_table_size*/numero di nodi di calcolo).
- Log di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] delle dimensioni corrispondenti a (*log_size*/numero di nodi di calcolo).

## <a name="locking"></a>Blocco

Consente di acquisire un blocco condiviso per l'oggetto DATABASE.

## <a name="metadata"></a>Metadati

Al termine di questa operazione, nelle viste di metadati [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) e [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) viene visualizzata una voce per questo database.

## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-basic-database-creation-examples"></a>R. Esempi di creazione di database di base

L'esempio seguente crea il database `mytest` con un'allocazione dello spazio di archiviazione pari a 100 GB per ogni nodo di calcolo per le tabelle replicate, 500 GB per appliance per le tabelle distribuite e 100 GB per appliance per il log delle transazioni. In questo esempio, AUTOGROW è OFF per impostazione predefinita.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB );
```

L'esempio seguente crea il database `mytest` con gli stessi parametri dell'esempio precedente, ad eccezione di AUTOGROW, che è ON. Ciò consente al database di aumentare oltre i parametri di dimensione specificati.

```sql
CREATE DATABASE mytest
  WITH
    (AUTOGROW = ON,
    REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB);
```

### <a name="b-creating-a-database-with-partial-gigabyte-sizes"></a>B. Creazione di un database con dimensioni in GB parziali

L'esempio seguente crea il database `mytest` con AUTOGROW OFF, un'allocazione dello spazio di archiviazione pari a 1,5 GB per ogni nodo di calcolo per le tabelle replicate, 5,25 GB per appliance per le tabelle distribuite e 10 GB per appliance per il log delle transazioni.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 1.5 GB,
    DISTRIBUTED_SIZE = 5.25 GB,
    LOG_SIZE = 10 GB);
```

## <a name="see-also"></a>Vedere anche

- [ALTER DATABASE - Piattaforma di strumenti analitici](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end