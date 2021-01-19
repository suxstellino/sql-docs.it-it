---
title: File e filegroup ALTER DATABASE
description: Aggiornare i file e i filegroup di un database usando Transact-SQL.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ADD FILE
- ADD_FILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting files
- removing files
- deleting filegroups
- file modifications [SQL Server]
- databases [SQL Server], size
- relocating files
- databases [SQL Server], modifying
- file additions [SQL Server], ALTER DATABASE
- file moving [SQL Server]
- default filegroups
- ALTER DATABASE statement, files and filegroups
- initializing files [SQL Server]
- database files [SQL Server]
- moving files
- filegroups [SQL Server], deleting
- adding filegroups
- adding files
- database filegroups [SQL Server]
- adding log files
- modifying files
- filegroups [SQL Server], adding
- file initialization [SQL Server]
- files [SQL Server], deleting
- files [SQL Server], adding
- databases [SQL Server], moving
ms.assetid: 1f635762-f7aa-4241-9b7a-b51b22292b07
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a0b3238eb85d1fc916d0543bb9723e9a44a7a884
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171803"
---
# <a name="alter-database-transact-sql-file-and-filegroup-options"></a>Opzioni per file e filegroup ALTER DATABASE (Transact-SQL)

Modifica i file e i filegroup associati al database. Consente inoltre di aggiungere file e filegroup a un database, di rimuoverli da esso e di modificare gli attributi di un database o i relativi file e filegroup. Per altre opzioni di ALTER DATABASE, vedere [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](alter-database-transact-sql-file-and-filegroup-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="syntax"></a>Sintassi

```syntaxsql
ALTER DATABASE database_name
{
    <add_or_modify_files>
  | <add_or_modify_filegroups>
}

<add_or_modify_files>::=
{
    ADD FILE <filespec> [ ,...n ]
        [ TO FILEGROUP { filegroup_name } ]
  | ADD LOG FILE <filespec> [ ,...n ]
  | REMOVE FILE logical_file_name
  | MODIFY FILE <filespec>
}

<filespec>::=
(
    NAME = logical_file_name
    [ , NEWNAME = new_logical_name ]
    [ , FILENAME = {'os_file_name' | 'filestream_path' | 'memory_optimized_data_path' } ]
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB| % ] ]
    [ , OFFLINE ]
)

<add_or_modify_filegroups>::=
{
    | ADD FILEGROUP filegroup_name
        [ CONTAINS FILESTREAM | CONTAINS MEMORY_OPTIMIZED_DATA ]
    | REMOVE FILEGROUP filegroup_name
    | MODIFY FILEGROUP filegroup_name
        { <filegroup_updatability_option>
        | DEFAULT
        | NAME = new_filegroup_name
        | { AUTOGROW_SINGLE_FILE | AUTOGROW_ALL_FILES }
        }
}
<filegroup_updatability_option>::=
{
    { READONLY | READWRITE }
    | { READ_ONLY | READ_WRITE }
}
```

## <a name="arguments"></a>Argomenti

**\<add_or_modify_files>::=**

Specifica i file da aggiungere, rimuovere o modificare.

*database_name* è il nome del database da modificare.

ADD FILE aggiunge un file al database.

TO FILEGROUP { *filegroup_name* } specifica il filegroup in cui aggiungere il file specificato. Per visualizzare i filegroup correnti e determinare il filegroup attualmente predefinito, usare la vista del catalogo [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).

ADD LOG FILE aggiunge un file di log al database specificato.

REMOVE FILE *logical_file_name* rimuove la descrizione del file logico da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed elimina il file fisico. Il file può essere rimosso solo se è vuoto.

*logical_file_name* è il nome logico usato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per fare riferimento al file.

> [!WARNING]
> Sarà possibile rimuovere un file di database con backup `FILE_SNAPSHOT` associati, ma non saranno eliminati tutti gli snapshot associati per evitare di invalidare i backup che fanno riferimento al file di database. Il file verrà troncato, ma non sarà eliminato fisicamente in modo da mantenere inalterati i backup FILE_SNAPSHOT. Per altre informazioni, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive).

MODIFY FILE specifica il file da modificare. È possibile modificare una sola proprietà \<filespec> alla volta. L'opzione NAME deve essere sempre specificata in \<filespec> per identificare il file da modificare. Se si specifica l'opzione SIZE, le nuove dimensioni del file devono essere superiori a quelle correnti.

Per modificare il nome logico di un file di dati o di un file di log, specificare il nome del file logico da rinominare nella clausola `NAME` e specificare il nuovo nome logico per il file nella clausola `NEWNAME`. Ad esempio:

```sql
MODIFY FILE ( NAME = logical_file_name, NEWNAME = new_logical_name )
```

Per spostare un file di dati o un file di log in una nuova posizione, specificare il nome di file logico corrente nella clausola `NAME` e specificare il nuovo percorso e il nome del file nel sistema operativo nella clausola `FILENAME`. Ad esempio:

```sql
MODIFY FILE ( NAME = logical_file_name, FILENAME = ' new_path/os_file_name ' )
```

Per lo spostamento di un catalogo full-text, specificare solo il nuovo percorso nella clausola FILENAME, senza indicare il nome del file nel sistema operativo.

Per altre informazioni, vedere [Spostare file del database](../../relational-databases/databases/move-database-files.md).

Per un filegroup FILESTREAM, NAME può essere modificato online. Sebbene FILENAME possa essere modificato online, la modifica non diventa effettiva fino a quando il contenitore non viene rilocato fisicamente e il server non viene arrestato e successivamente riavviato.

È possibile impostare un file FILESTREAM su OFFLINE. Quando un file FILESTREAM è offline, il filegroup padre sarà contrassegnato internamente come offline. Di conseguenza, ogni accesso a dati FILESTREAM all'interno del filegroup specifico avrà esito negativo.

> [!NOTE]
> Le opzioni \<add_or_modify_files> non sono disponibili in un database indipendente.

**\<filespec>::=**

Controlla le proprietà del file.

NAME *logical_file_name* specifica il nome logico del file.

*logical_file_name* è il nome logico usato in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per fare riferimento al file.

NEWNAME *new_logical_file_name* specifica un nuovo nome logico per il file.

*new_logical_file_name* è il nome da sostituire al nome del file logico esistente. Il nome deve essere univoco all'interno del database e conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome può essere costituito da una costante per valori di carattere o Unicode, da un identificatore regolare o da un identificatore delimitato.

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** | **'** _memory\_optimized\_data\_path_ **'** } specifica il nome file (fisico) del sistema operativo.

' *os_file_name* ' specifica, per un filegroup standard (ROWS), il percorso e il nome file usati dal sistema operativo quando si crea il file. Il file deve trovarsi nel server in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il percorso specificato deve essere esistente prima di eseguire l'istruzione ALTER DATABASE.

> [!NOTE]
> I parametri `SIZE`, `MAXSIZE` e `FILEGROWTH` non possono essere impostati se per il file è specificato un percorso UNC.
>
> I database di sistema non possono trovarsi in directory di condivisione UNC.

I file di dati non dovrebbero essere memorizzati in file system compressi, a meno che tali file non siano file secondari di sola lettura o il database non sia di sola lettura. I file di log non devono mai essere archiviati in file system compressi.

Se il file si trova in una partizione non formattata, nell'argomento *os_file_name* è necessario specificare solo la lettera dell'unità di una partizione non formattata esistente. In ogni partizione non formattata è possibile inserire un solo file.

**'** *filestream_path* **'** : per un filegroup FILESTREAM, FILENAME fa riferimento a un percorso in cui verranno archiviati i dati di FILESTREAM. È necessario che il percorso fino all'ultima cartella esista già, mentre l'ultima cartella non deve essere presente. Se, ad esempio, si specifica il percorso `C:\MyFiles\MyFilestreamData`, `C:\MyFiles` deve esistere già prima di eseguire ALTER DATABASE, mentre la cartella `MyFilestreamData` non deve essere presente.

> [!NOTE]
> Le proprietà SIZE e FILEGROWTH non si applicano a un filegroup FILESTREAM.

**'** *memory_optimized_data_path* **'** : per un filegroup ottimizzato per la memoria, FILENAME fa riferimento a un percorso in cui verranno archiviati i dati ottimizzati per la memoria. È necessario che il percorso fino all'ultima cartella esista già, mentre l'ultima cartella non deve essere presente. Se, ad esempio, si specifica il percorso `C:\MyFiles\MyData`, `C:\MyFiles` deve esistere già prima di eseguire ALTER DATABASE, mentre la cartella `MyData` non deve essere presente.

Il filegroup e il file (`<filespec>`) devono essere creati nella stessa istruzione.

> [!NOTE]
> Le proprietà SIZE e FILEGROWTH non si applicano a un filegroup MEMORY_OPTIMIZED_DATA.

Per altre informazioni sui filegroup ottimizzati per la memoria, vedere [Filegroup ottimizzato per la memoria](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md).

SIZE *size* specifica le dimensioni del file. SIZE non si applica a filegroup FILESTREAM.

*size* corrisponde alle dimensioni del file.

Quando si specifica con ADD FILE, *size* corrisponde alle dimensioni iniziali del file. Se si specifica con MODIFY FILE, *size* corrisponde alle nuove dimensioni del file, che devono essere superiori a quelle correnti.

Se non si specifica *size* per il file primario, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa le dimensioni del file primario del database **model**. Se si specifica un file di dati o un file di log secondario senza specificare *size*, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea un file di 1 MB.

È possibile utilizzare i suffissi KB, MB, GB e TB per indicare kilobyte, megabyte, gigabyte e terabyte. Il valore predefinito è MB. Specificare un numero intero, ovvero non includere decimali. Se si desidera specificare una frazione di megabyte, convertire il valore in kilobyte moltiplicando il numero per 1024. Ad esempio, specificare 1536 KB anziché 1,5 MB (1,5 x 1024 = 1536).

> [!NOTE]
> `SIZE` non può essere impostato:
>
> - Quando per il file viene specificato un percorso UNC
> - Per i filegroup `FILESTREAM` e `MEMORY_OPTIMIZED_DATA`

MAXSIZE { *max_size*| UNLIMITED } specifica le dimensioni massime consentite per il file.

*max_size* corrisponde alle dimensioni massime del file. È possibile utilizzare i suffissi KB, MB, GB e TB per indicare kilobyte, megabyte, gigabyte e terabyte. Il valore predefinito è MB. Specificare un numero intero, ovvero non includere decimali. Se non si specifica *max_size*, le dimensioni del file aumenteranno fino a quando il disco risulta pieno.

UNLIMITED specifica che le dimensioni del file aumentano fino a quando il disco risulta pieno. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un file di log specificato con aumento delle dimensioni illimitato può raggiungere una dimensione massima di 2 TB, mentre un file di dati può raggiungere una dimensione massima di 16 TB. Non vi sono dimensioni massime se questa opzione viene specificata per un contenitore FILESTREAM, il quale continua a crescere finché il disco non è pieno.

> [!NOTE]
> `MAXSIZE` non può essere impostato quando per il file viene specificato un percorso UNC.

FILEGROWTH *growth_increment* specifica l'incremento automatico per l'aumento delle dimensioni del file. Il valore impostato per il parametro FILEGROWTH di un file non può essere superiore al valore del parametro MAXSIZE. FILEGROWTH non si applica a filegroup FILESTREAM.

*growth_increment* è la quantità di spazio aggiunta al file ogni volta che è necessario altro spazio.

È possibile specificare il valore in megabyte (MB), kilobyte (KB), gigabyte (GB) o terabyte (TB) oppure in forma di percentuale (%). Se si specifica un valore senza il suffisso MB, KB o %, il suffisso predefinito è MB. Se si utilizza il suffisso %, l'incremento corrisponde alla percentuale delle dimensioni del file specificata quando si verifica l'incremento. Le dimensioni specificate vengono arrotondate al blocco di 64 KB più prossimo.

Il valore 0 indica che l'aumento automatico delle dimensioni è disattivato e non è consentita l'allocazione di spazio aggiuntivo.

Se FILEGROWTH viene omesso, i valori predefiniti sono i seguenti:

|Versione|Valori predefiniti|
|-------------|--------------------|
|A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]|Dati 64 MB. File di log 64 MB.|
|A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Dati 1 MB. File di log 10%.|
|Prima di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Dati 10%. File di log 10%.|

> [!NOTE]
> `FILEGROWTH` non può essere impostato:
>
> - Quando per il file viene specificato un percorso UNC
> - Per i filegroup `FILESTREAM` e `MEMORY_OPTIMIZED_DATA`

OFFLINE imposta il file offline e rende inaccessibili tutti gli oggetti nel filegroup.

> [!CAUTION]
> Utilizzare questa opzione solo quando il file è danneggiato e non è possibile ripristinarlo. Un file impostato su OFFLINE può essere riportato online solo tramite il ripristino del file dal backup. Per altre informazioni sul ripristino di un singolo file, vedere [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).
>
> Le opzioni \<filespec> non sono disponibili in un database indipendente.

**\<add_or_modify_filegroups>::=**

Aggiunge, modifica o rimuove un filegroup nel database.

ADD FILEGROUP *filegroup_name* aggiunge un filegroup al database.

CONTAINS FILESTREAM specifica che il filegroup archivia BLOB FILESTREAM nel file system.

CONTAINS MEMORY_OPTIMIZED_DATA

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive)

Specifica che il filegroup archivia i dati con ottimizzazione per la memoria nel file system. Per altre informazioni, vedere [OLTP in memoria - Ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). È consentito un solo filegroup `MEMORY_OPTIMIZED_DATA` per ogni database. Per la creazione di tabelle con ottimizzazione per la memoria, il filegroup non può essere vuoto. Deve essere presente almeno un file. *filegroup_name* fa riferimento a un percorso. È necessario che il percorso fino all'ultima cartella esista già, mentre l'ultima cartella non deve essere presente.

REMOVE FILEGROUP *filegroup_name* rimuove un filegroup dal database. Il filegroup può essere rimosso solo se è vuoto. Rimuove tutti i file a partire dal filegroup. Per altre informazioni, vedere "REMOVE FILE *logical_file_name*" più indietro in questo argomento.

> [!NOTE]
> A meno che il Garbage Collector per FILESTREAM non abbia rimosso tutti i file da un contenitore FILESTREAM, l'operazione `ALTER DATABASE REMOVE FILE` per rimuovere un contenitore FILESTREAM avrà esito negativo e verrà restituito un errore. Vedere la sezione [Rimozione di un contenitore FILESTREAM](#removing-a-filestream-container) di seguito in questo argomento.

MODIFY FILEGROUP *filegroup_name* { \<filegroup_updatability_option> | DEFAULT | NAME **=** _new\_filegroup\_name_ } Modifica il filegroup impostando lo stato su READ_ONLY o READ_WRITE, impostando il filegroup come predefinito per il database o cambiando il nome del filegroup.

\<filegroup_updatability_option> Imposta la proprietà di sola lettura o di lettura/scrittura per il filegroup.

DEFAULT cambia il filegroup predefinito del database in *filegroup_name*. In un database può esistere un solo filegroup predefinito. Per altre informazioni, vedere [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

NAME = *new_filegroup_name* cambia il nome del filegroup in *new_filegroup_name*.

AUTOGROW_SINGLE_FILE **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive)

Quando un file del filegroup raggiunge la soglia dell'aumento automatico delle dimensioni, vengono aumentate le dimensioni solo di quel file. Questa è la modalità predefinita.

AUTOGROW_ALL_FILES

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive)

Quando un file del filegroup raggiunge la soglia dell'aumento automatico delle dimensioni, vengono aumentate le dimensioni di tutti i file del filegroup.

> [!NOTE]
> Questo è il valore predefinito per TempDB.

**\<filegroup_updatability_option>::=**

Imposta la proprietà di sola lettura o di lettura/scrittura per il filegroup.

READ_ONLY | READONLY specifica che il filegroup è di sola lettura. Non sono consentiti aggiornamenti degli oggetti nel filegroup. Non è possibile rendere di sola lettura il filegroup primario. Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database. Per altre informazioni, vedere la clausola SINGLE_USER.

I database di sola lettura non consentono modifiche dei dati e pertanto:

- Non viene eseguito il recupero automatico all'avvio del sistema.
- La compattazione del database non è possibile.
- Non vengono attivati blocchi nei database di sola lettura e ciò può portare a migliori prestazioni di esecuzione delle query.

> [!NOTE]
> La parola chiave `READONLY` verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare l'utilizzo di `READONLY` un nuovo progetto di sviluppo e prevedere interventi di modifica per le applicazioni in cui `READONLY` è utilizzato. Usare invece `READ_ONLY`.

READ_WRITE | READWRITE specifica che il filegroup è di lettura/scrittura. Sono consentiti aggiornamenti degli oggetti contenuti nel filegroup. Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database. Per altre informazioni, vedere la clausola SINGLE_USER.

> [!NOTE]
> La parola chiave `READWRITE` verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare l'uso di `READWRITE` in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente usata la parola chiave `READWRITE`. Usare al suo posto la parola chiave `READ_WRITE`.
> [!TIP]
> Per determinare lo stato di queste opzioni, è possibile esaminare la colonna **is_read_only** nella vista del catalogo  **sys.databases** oppure la proprietà **Updateability** della funzione `DATABASEPROPERTYEX`.

## <a name="remarks"></a>Osservazioni

Per ridurre le dimensioni di un database, usare [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Non è possibile aggiungere o rimuovere un file durante l'esecuzione di un'istruzione `BACKUP`.

Per ogni database è possibile specificare un massimo di 32.767 file e 32.767 filegroup.

A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] lo stato di un file di database, ad esempio online o offline, viene mantenuto indipendentemente dallo stato del database. Per altre informazioni, vedere [Stati del file](../../relational-databases/databases/file-states.md).

- Lo stato dei file all'interno di un filegroup determina la disponibilità dell'intero filegroup. Un filegroup è disponibile se tutti i file in esso inclusi sono online.
- Se un filegroup è offline, qualsiasi tentativo di accesso al filegroup tramite un'istruzione SQL avrà esito negativo e verrà generato un errore. Per la compilazione di piani delle query per istruzioni `SELECT`, Query Optimizer evita gli indici non cluster e le viste indicizzate presenti in filegroup offline. Ciò consente la corretta esecuzione di tali istruzioni. Se tuttavia il filegroup offline contiene l'indice cluster o heap della tabella di destinazione, l'istruzione `SELECT` avrà esito negativo, così come tutte le istruzioni `INSERT`, `UPDATE` o `DELETE` che implicano la modifica di una tabella tramite un indice incluso in un filegroup offline.

Non è possibile impostare i parametri SIZE, MAXSIZE e FILEGROWTH se è specificato un percorso UNC per il file.

I parametri SIZE e FILEGROWTH non possono essere impostati per i filegroup con ottimizzazione per la memoria.

La parola chiave `READONLY` verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare l'utilizzo di `READONLY` un nuovo progetto di sviluppo e prevedere interventi di modifica per le applicazioni in cui READONLY è utilizzato. Usare invece `READ_ONLY`.

La parola chiave `READWRITE` verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare l'uso di `READWRITE` in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente usata la parola chiave `READWRITE`. Usare al suo posto la parola chiave `READ_WRITE`.

## <a name="moving-files"></a>Spostamento di file

È possibile spostare file di dati e di log, di sistema o definiti dall'utente, specificando la nuova posizione in FILENAME. Questa procedura può risultare utile nei casi seguenti:

- Recupero da errore. ad esempio quando il database è in modalità sospetta o viene chiuso a causa di un errore hardware.
- Rilocazione pianificata.
- Rilocazione per una manutenzione pianificata del disco.

Per altre informazioni, vedere [Spostare file del database](../../relational-databases/databases/move-database-files.md).

## <a name="initializing-files"></a>Inizializzazione dei file

Per impostazione predefinita, i file di dati e di log vengono inizializzati tramite il riempimento con zeri quando si esegue una delle operazioni seguenti:

- Creare un database.
- Aggiunta di file a un database esistente.
- Aumento delle dimensioni di un file esistente.
- Ripristino di un database o un filegroup.

I file di dati possono essere inizializzati immediatamente. Ciò consente l'esecuzione rapida di queste operazioni sui file. Per altre informazioni, vedere [Inizializzazione di file di database](../../relational-databases/databases/database-instant-file-initialization.md).

## <a name="removing-a-filestream-container"></a><a name="removing-a-filestream-container"></a> Rimozione di un contenitore FILESTREAM

Anche se il contenitore FILESTREAM potrebbe essere stato svuotato mediante l'operazione "DBCC SHRINKFILE", potrebbe essere ancora necessario mantenere nel database i riferimenti ai file eliminati per vari motivi di manutenzione del sistema. [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md) eseguirà il Garbage Collector per FILESTREAM per rimuovere questi file quando è possibile. A meno che il Garbage Collector per FILESTREAM non abbia rimosso tutti i file da un contenitore FILESTREAM, l'operazione ALTER DATABASE REMOVE FILE avrà esito negativo per la rimozione di un contenitore FILESTREAM e verrà restituito un errore. È consigliabile utilizzare il processo seguente per rimuovere un contenitore FILESTREAM.

1. Eseguire [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) con l'opzione EMPTYFILE per spostare il contenuto attivo del contenitore in altri contenitori.
2. Assicurarsi che siano stati eseguiti i backup del log, nel modello di recupero FULL o BULK_LOGGED.
3. Assicurarsi che sia stato eseguito il processo di lettura log repliche, se rilevante.
4. Eseguire [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md) per forzare l'eliminazione tramite Garbage Collector di qualsiasi file non più necessario in questo contenitore.
5. Eseguire ALTER DATABASE con l'opzione REMOVE FILE per rimuovere questo contenitore.
6. Ripetere i passaggi da 2 a 4 ancora una volta per completare la Garbage Collection.
7. Utilizzare ALTER Database...REMOVE FILE per rimuovere questo contenitore.

## <a name="examples"></a>Esempi

### <a name="a-adding-a-file-to-a-database"></a>R. Aggiunta di un file a un database

Nell'esempio seguente viene aggiunto un file di dati da 5 MB al database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = Test1dat2,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat2.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="b-adding-a-filegroup-with-two-files-to-a-database"></a>B. Aggiunta di un filegroup con due file a un database

Nell'esempio seguente viene creato il filegroup `Test1FG1` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] e vengono aggiunti due file da 5 MB al filegroup.

```sql
USE master
GO
ALTER DATABASE AdventureWorks2012
ADD FILEGROUP Test1FG1;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = test1dat3,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat3.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),  
(  
    NAME = test1dat4,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat4.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
)  
TO FILEGROUP Test1FG1;
GO
```

### <a name="c-adding-two-log-files-to-a-database"></a>C. Aggiunta di due file di log a un database

Nell'esempio seguente vengono aggiunti due file di log da 5 MB al database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD LOG FILE
(
    NAME = test1log2,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\test2log.ldf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),
(
    NAME = test1log3,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\test3log.ldf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="d-removing-a-file-from-a-database"></a>D. Rimozione di un file da un database

Nell'esempio seguente viene rimosso uno dei file aggiunti nell'esempio B.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
REMOVE FILE test1dat4;
GO
```

### <a name="e-modifying-a-file"></a>E. Modifica di un file

L'esempio seguente aumenta le dimensioni di uno dei file aggiunti nell'esempio B. L'istruzione ALTER DATABASE con il comando MODIFY FILE può soltanto incrementare le dimensioni di un file, quindi se bisogna ridurle è necessario usare DBCC SHRINKFILE.

```sql
USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

In questo esempio le dimensioni di un file di dati vengono ridotte a 100 MB e ne vengono specificate le dimensioni.

```sql
USE AdventureWorks2012;
GO

DBCC SHRINKFILE (AdventureWorks2012_data, 100);
GO

USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

### <a name="f-moving-a-file-to-a-new-location"></a>F. Spostamento di un file in un diverso percorso

Nell'esempio seguente il file `Test1dat2` creato nell'esempio A viene spostato in una nuova directory.

> [!NOTE]
> È necessario spostare fisicamente il file nella nuova directory prima di eseguire l'esempio. In seguito, arrestare e avviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure impostare il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] su OFFLINE e quindi su ONLINE per implementare la modifica.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILE
(
    NAME = Test1dat2,
    FILENAME = N'c:\t1dat2.ndf'
);
GO
```

### <a name="g-moving-tempdb-to-a-new-location"></a>G. Spostamento del database tempdb in un diverso percorso

Nell'esempio seguente viene spostato il database `tempdb` dal percorso corrente nel disco a un'altro percorso nel disco. Poiché `tempdb` viene ricreato a ogni avvio del servizio MSSQLSERVER, non è necessario spostare fisicamente i dati e i file di log. I file vengono creati quando il servizio viene riavviato nel passaggio 3. Fino a quando il servizio non viene riavviato, `tempdb` continua a funzionare nel percorso esistente.

1. Determinare i nomi di file logici del database `tempdb` e il rispettivo percorso corrente su disco.

    ```sql
    SELECT name, physical_name
    FROM sys.master_files
    WHERE database_id = DB_ID('tempdb');
    GO
    ```

2. Modificare il percorso di ogni file tramite `ALTER DATABASE`.

    ```sql
    USE master;
    GO
    ALTER DATABASE tempdb
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');
    GO
    ALTER DATABASE tempdb
    MODIFY FILE (NAME = templog, FILENAME = 'E:\SQLData\templog.ldf');
    GO
    ```

3. Arrestare e riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

4. Verificare la modifica ai file.

    ```sql
    SELECT name, physical_name
    FROM sys.master_files
    WHERE database_id = DB_ID('tempdb');
    ```

5. Eliminare i file tempdb.mdf e templog.ldf dai percorsi originali.

### <a name="h-making-a-filegroup-the-default"></a>H. Impostazione di un filegroup come predefinito

Nell'esempio seguente il filegroup `Test1FG1` creato nell'esempio B viene impostato come filegroup predefinito. Il filegroup `PRIMARY` viene quindi reimpostato come filegroup predefinito. Si noti che il nome `PRIMARY` deve essere delimitato da parentesi quadre o virgolette.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP Test1FG1 DEFAULT;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP [PRIMARY] DEFAULT;
GO
```

### <a name="i-adding-a-filegroup-using-alter-database"></a>I. Aggiunta di un filegroup utilizzando ALTER DATABASE

Nell'esempio seguente viene aggiunto un `FILEGROUP` che contiene la clausola `FILESTREAM` al database `FileStreamPhotoDB`.

```sql
--Create and add a FILEGROUP that CONTAINS the FILESTREAM clause.
ALTER DATABASE FileStreamPhotoDB
ADD FILEGROUP TodaysPhotoShoot
CONTAINS FILESTREAM;
GO

--Add a file for storing database photos to FILEGROUP
ALTER DATABASE FileStreamPhotoDB
ADD FILE
(
  NAME= 'PhotoShoot1',
  FILENAME = 'C:\Users\Administrator\Pictures\TodaysPhotoShoot.ndf'
)
TO FILEGROUP TodaysPhotoShoot;
GO
```

Nell'esempio seguente viene aggiunto un `FILEGROUP` che contiene la clausola `MEMORY_OPTIMIZED_DATA` al database `xtp_db`. Nel filegroup vengono archiviati i dati ottimizzati per la memoria.

```sql
--Create and add a FILEGROUP that CONTAINS the MEMORY_OPTIMIZED_DATA clause.
ALTER DATABASE xtp_db
ADD FILEGROUP xtp_fg
CONTAINS MEMORY_OPTIMIZED_DATA;
GO

--Add a file for storing memory optimized data to FILEGROUP
ALTER DATABASE xtp_db
ADD FILE
(
  NAME='xtp_mod',
  FILENAME='d:\data\xtp_mod'
)
TO FILEGROUP xtp_fg;
GO
```

### <a name="j-change-filegroup-so-that-when-a-file-in-the-filegroup-meets-the-autogrow-threshold-all-files-in-the-filegroup-grow"></a>J. Modificare il filegroup in modo che, quando un file del filegroup raggiunge la soglia dell'aumento automatico delle dimensioni, vengono aumentate le dimensioni di tutti i file del filegroup

 Nell'esempio seguente vengono generate le istruzioni `ALTER DATABASE` necessarie per modificare il filegroup di lettura/scrittura con l'impostazione di `AUTOGROW_ALL_FILES`.

```sql
--Generate ALTER DATABASE ... MODIFY FILEGROUP statements
--so that all read-write filegroups grow at the same time.
SET NOCOUNT ON;

DROP TABLE IF EXISTS #tmpdbs
CREATE TABLE #tmpdbs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, isdone BIT);

DROP TABLE IF EXISTS #tmpfgs
CREATE TABLE #tmpfgs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, fgname sysname, isdone BIT);

INSERT INTO #tmpdbs ([dbid], [dbname], [isdone])
SELECT database_id, name, 0 FROM master.sys.databases (NOLOCK) WHERE is_read_only = 0 AND state = 0;

DECLARE @dbid INT, @query VARCHAR(1000), @dbname sysname, @fgname sysname

WHILE (SELECT COUNT(id) FROM #tmpdbs WHERE isdone = 0) > 0
BEGIN
  SELECT TOP 1 @dbname = [dbname], @dbid = [dbid] FROM #tmpdbs WHERE isdone = 0

  SET @query = 'SELECT ' + CAST(@dbid AS NVARCHAR) + ', ''' + @dbname + ''', [name], 0 FROM [' + @dbname + '].sys.filegroups WHERE [type] = ''FG'' AND is_read_only = 0;'
  INSERT INTO #tmpfgs
  EXEC (@query)

  UPDATE #tmpdbs
  SET isdone = 1
  WHERE [dbid] = @dbid
END;

IF (SELECT COUNT(ID) FROM #tmpfgs) > 0
BEGIN
  WHILE (SELECT COUNT(id) FROM #tmpfgs WHERE isdone = 0) > 0
  BEGIN
    SELECT TOP 1 @dbname = [dbname], @dbid = [dbid], @fgname = fgname FROM #tmpfgs WHERE isdone = 0

    SET @query = 'ALTER DATABASE [' + @dbname + '] MODIFY FILEGROUP [' + @fgname + '] AUTOGROW_ALL_FILES;'

    PRINT @query

    UPDATE #tmpfgs
    SET isdone = 1
    WHERE [dbid] = @dbid AND fgname = @fgname
  END
END;
GO
```

## <a name="see-also"></a>Vedere anche

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [BLOB](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)
- [DBCC SHRINKFIL](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)
- [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)
- [Inizializzazione di file di database](../../relational-databases/databases/database-instant-file-initialization.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-file-and-filegroup-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita di SQL \*_**<br />&nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Istanza gestita di SQL di Azure

Usare questa istruzione con un database in Istanza gestita di SQL di Azure.

## <a name="syntax-for-azure-sql-managed-instance"></a>Sintassi per Istanza gestita di SQL di Azure

```syntaxsql
ALTER DATABASE database_name
{
    <add_or_modify_files>
  | <add_or_modify_filegroups>
}
[;]

<add_or_modify_files>::=
{
    ADD FILE <filespec> [ ,...n ]
        [ TO FILEGROUP { filegroup_name } ]
  | REMOVE FILE logical_file_name
  | MODIFY FILE <filespec>
}
  
<filespec>::=
(
    NAME = logical_file_name
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB| % ] ]
)

<add_or_modify_filegroups>::=
{
    | ADD FILEGROUP filegroup_name
    | REMOVE FILEGROUP filegroup_name
    | MODIFY FILEGROUP filegroup_name
        { <filegroup_updatability_option>
        | DEFAULT
        | NAME = new_filegroup_name
        | { AUTOGROW_SINGLE_FILE | AUTOGROW_ALL_FILES }
        }
}  
<filegroup_updatability_option>::=
{
    { READONLY | READWRITE }
    | { READ_ONLY | READ_WRITE }
}
```

## <a name="arguments"></a>Argomenti

**\<add_or_modify_files>::=**

Specifica i file da aggiungere, rimuovere o modificare.

*database_name* è il nome del database da modificare.

ADD FILE aggiunge un file al database.

TO FILEGROUP { *filegroup_name* } specifica il filegroup in cui aggiungere il file specificato. Per visualizzare i filegroup correnti e determinare il filegroup attualmente predefinito, usare la vista del catalogo [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).

REMOVE FILE *logical_file_name* rimuove la descrizione del file logico da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed elimina il file fisico. Il file può essere rimosso solo se è vuoto.

*logical_file_name* è il nome logico usato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per fare riferimento al file.

MODIFY FILE specifica il file da modificare. È possibile modificare una sola proprietà \<filespec> alla volta. L'opzione NAME deve essere sempre specificata in \<filespec> per identificare il file da modificare. Se si specifica l'opzione SIZE, le nuove dimensioni del file devono essere superiori a quelle correnti.

**\<filespec>::=**

Controlla le proprietà del file.

NAME *logical_file_name* specifica il nome logico del file.

*logical_file_name* è il nome logico usato in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per fare riferimento al file.

NEWNAME *new_logical_file_name* specifica un nuovo nome logico per il file.

*new_logical_file_name* è il nome da sostituire al nome del file logico esistente. Il nome deve essere univoco all'interno del database e conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Il nome può essere costituito da una costante per valori di carattere o Unicode, da un identificatore regolare o da un identificatore delimitato.

SIZE *size* specifica le dimensioni del file.

*size* corrisponde alle dimensioni del file.

Quando si specifica con ADD FILE, *size* corrisponde alle dimensioni iniziali del file. Se si specifica con MODIFY FILE, *size* corrisponde alle nuove dimensioni del file, che devono essere superiori a quelle correnti.

Se non si specifica *size* per il file primario, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa le dimensioni del file primario del database **model**. Se si specifica un file di dati o un file di log secondario senza specificare *size*, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea un file di 1 MB.

È possibile utilizzare i suffissi KB, MB, GB e TB per indicare kilobyte, megabyte, gigabyte e terabyte. Il valore predefinito è MB. Specificare un numero intero, ovvero non includere decimali. Se si desidera specificare una frazione di megabyte, convertire il valore in kilobyte moltiplicando il numero per 1024. Ad esempio, specificare 1536 KB anziché 1,5 MB (1,5 x 1024 = 1536).

MAXSIZE { *max_size*| UNLIMITED } specifica le dimensioni massime consentite per il file.

*max_size* corrisponde alle dimensioni massime del file. È possibile utilizzare i suffissi KB, MB, GB e TB per indicare kilobyte, megabyte, gigabyte e terabyte. Il valore predefinito è MB. Specificare un numero intero, ovvero non includere decimali. Se non si specifica *max_size*, le dimensioni del file aumenteranno fino a quando il disco risulta pieno.

UNLIMITED specifica che le dimensioni del file aumentano fino a quando il disco risulta pieno. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un file di log specificato con aumento delle dimensioni illimitato può raggiungere una dimensione massima di 2 TB, mentre un file di dati può raggiungere una dimensione massima di 16 TB.

FILEGROWTH *growth_increment* specifica l'incremento automatico per l'aumento delle dimensioni del file. Il valore impostato per il parametro FILEGROWTH di un file non può essere superiore al valore del parametro MAXSIZE.

*growth_increment* è la quantità di spazio aggiunta al file ogni volta che è necessario altro spazio.

È possibile specificare il valore in megabyte (MB), kilobyte (KB), gigabyte (GB) o terabyte (TB) oppure in forma di percentuale (%). Se si specifica un valore senza il suffisso MB, KB o %, il suffisso predefinito è MB. Se si utilizza il suffisso %, l'incremento corrisponde alla percentuale delle dimensioni del file specificata quando si verifica l'incremento. Le dimensioni specificate vengono arrotondate al blocco di 64 KB più prossimo.

Il valore 0 indica che l'aumento automatico delle dimensioni è disattivato e non è consentita l'allocazione di spazio aggiuntivo.

Se FILEGROWTH viene omesso, i valori predefiniti sono i seguenti:

- Dati 64 MB
- File di log 64 MB

**\<add_or_modify_filegroups>::=**

Aggiunge, modifica o rimuove un filegroup nel database.

ADD FILEGROUP *filegroup_name* aggiunge un filegroup al database.

Nell'esempio seguente viene creato un filegroup che viene aggiunto a un database denominato sql_db_mi, quindi viene aggiunto un file al filegroup.

```sql
ALTER DATABASE sql_db_mi ADD FILEGROUP sql_db_mi_fg;
GO
ALTER DATABASE sql_db_mi ADD FILE (NAME='sql_db_mi_mod') TO FILEGROUP sql_db_mi_fg;
```

REMOVE FILEGROUP *filegroup_name* rimuove un filegroup dal database. Il filegroup può essere rimosso solo se è vuoto. Rimuove tutti i file a partire dal filegroup. Per altre informazioni, vedere "REMOVE FILE *logical_file_name*" più indietro in questo argomento.

MODIFY FILEGROUP _filegroup\_name_ { \<filegroup_updatability_option> | DEFAULT | NAME **=** _new\_filegroup\_name_ } Modifica il filegroup impostando lo stato su READ_ONLY o READ_WRITE, impostando il filegroup come predefinito per il database o cambiando il nome del filegroup.

\<filegroup_updatability_option> Imposta la proprietà di sola lettura o di lettura/scrittura per il filegroup.

DEFAULT cambia il filegroup predefinito del database in *filegroup_name*. In un database può esistere un solo filegroup predefinito. Per altre informazioni, vedere [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

NAME = *new_filegroup_name* cambia il nome del filegroup in *new_filegroup_name*.

AUTOGROW_SINGLE_FILE

Quando un file del filegroup raggiunge la soglia dell'aumento automatico delle dimensioni, vengono aumentate le dimensioni solo di quel file. Questa è la modalità predefinita.

AUTOGROW_ALL_FILES

Quando un file del filegroup raggiunge la soglia dell'aumento automatico delle dimensioni, vengono aumentate le dimensioni di tutti i file del filegroup.

**\<filegroup_updatability_option>::=**

Imposta la proprietà di sola lettura o di lettura/scrittura per il filegroup.

READ_ONLY | READONLY specifica che il filegroup è di sola lettura. Non sono consentiti aggiornamenti degli oggetti nel filegroup. Non è possibile rendere di sola lettura il filegroup primario. Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database. Per altre informazioni, vedere la clausola SINGLE_USER.

I database di sola lettura non consentono modifiche dei dati e pertanto:

- Non viene eseguito il recupero automatico all'avvio del sistema.
- La compattazione del database non è possibile.
- Non vengono attivati blocchi nei database di sola lettura e ciò può portare a migliori prestazioni di esecuzione delle query.

> [!NOTE]
> La parola chiave READONLY verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitarne l'utilizzo in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Utilizzare READ_ONLY in alternativa.

READ_WRITE | READWRITE specifica che il filegroup è di lettura/scrittura. Sono consentiti aggiornamenti degli oggetti contenuti nel filegroup. Per modificare questo stato, è necessario disporre dell'accesso esclusivo al database. Per altre informazioni, vedere la clausola SINGLE_USER.

> [!NOTE]
> La parola chiave `READWRITE` verrà rimossa a partire da una delle prossime versioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare l'uso di `READWRITE` in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente usata la parola chiave `READWRITE`. Usare al suo posto la parola chiave `READ_WRITE`.

Per determinare lo stato di queste opzioni, è possibile esaminare la colonna **is_read_only** nella vista del catalogo  **sys.databases** oppure la proprietà **Updateability** della funzione `DATABASEPROPERTYEX`.

## <a name="remarks"></a>Osservazioni

Per ridurre le dimensioni di un database, usare [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Non è possibile aggiungere o rimuovere un file durante l'esecuzione di un'istruzione `BACKUP`.

Per ogni database è possibile specificare un massimo di 32.767 file e 32.767 filegroup.

## <a name="examples"></a>Esempi

### <a name="a-adding-a-file-to-a-database"></a>R. Aggiunta di un file a un database

Nell'esempio seguente viene aggiunto un file di dati da 5 MB al database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
  NAME = Test1dat2,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="b-adding-a-filegroup-with-two-files-to-a-database"></a>B. Aggiunta di un filegroup con due file a un database

Nell'esempio seguente viene creato il filegroup `Test1FG1` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] e vengono aggiunti due file da 5 MB al filegroup.

```sql
USE master
GO
ALTER DATABASE AdventureWorks2012
ADD FILEGROUP Test1FG1;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = test1dat3,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),
(
    NAME = test1dat4,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
)  
TO FILEGROUP Test1FG1;
GO
```

### <a name="c-removing-a-file-from-a-database"></a>C. Rimozione di un file da un database

Nell'esempio seguente viene rimosso uno dei file aggiunti nell'esempio B.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
REMOVE FILE test1dat4;
GO
```

### <a name="d-modifying-a-file"></a>D. Modifica di un file

L'esempio seguente aumenta le dimensioni di uno dei file aggiunti nell'esempio B. L'istruzione ALTER DATABASE con il comando MODIFY FILE può soltanto incrementare le dimensioni di un file, quindi se bisogna ridurle è necessario usare DBCC SHRINKFILE.

```sql
USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

In questo esempio le dimensioni di un file di dati vengono ridotte a 100 MB e ne vengono specificate le dimensioni.

```sql
USE AdventureWorks2012;
GO

DBCC SHRINKFILE (AdventureWorks2012_data, 100);
GO

USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

### <a name="e-making-a-filegroup-the-default"></a>E. Impostazione di un filegroup come predefinito

Nell'esempio seguente il filegroup `Test1FG1` creato nell'esempio B viene impostato come filegroup predefinito. Il filegroup `PRIMARY` viene quindi reimpostato come filegroup predefinito. Si noti che il nome `PRIMARY` deve essere delimitato da parentesi quadre o virgolette.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP Test1FG1 DEFAULT;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP [PRIMARY] DEFAULT;
GO
```

### <a name="f-adding-a-filegroup-using-alter-database"></a>F. Aggiunta di un filegroup utilizzando ALTER DATABASE

Nell'esempio seguente viene aggiunto un `FILEGROUP` al database `MyDB`.

```sql
--Create and add a FILEGROUP
ALTER DATABASE MyDB
ADD FILEGROUP NewFG;
GO

--Add a file to FILEGROUP
ALTER DATABASE MyDB
ADD FILE
(
    NAME= 'MyFile',
)
TO FILEGROUP NewFG;
GO
```

### <a name="g-change-filegroup-so-that-when-a-file-in-the-filegroup-meets-the-autogrow-threshold-all-files-in-the-filegroup-grow"></a>G. Modificare il filegroup in modo che, quando un file del filegroup raggiunge la soglia dell'aumento automatico delle dimensioni, vengono aumentate le dimensioni di tutti i file del filegroup

Nell'esempio seguente vengono generate le istruzioni `ALTER DATABASE` necessarie per modificare il filegroup di lettura/scrittura con l'impostazione di `AUTOGROW_ALL_FILES`.

```sql
--Generate ALTER DATABASE ... MODIFY FILEGROUP statements
--so that all read-write filegroups grow at the same time.
SET NOCOUNT ON;

DROP TABLE IF EXISTS #tmpdbs
CREATE TABLE #tmpdbs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, isdone BIT);

DROP TABLE IF EXISTS #tmpfgs
CREATE TABLE #tmpfgs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, fgname sysname, isdone BIT);

INSERT INTO #tmpdbs ([dbid], [dbname], [isdone])
SELECT database_id, name, 0 FROM master.sys.databases (NOLOCK) WHERE is_read_only = 0 AND state = 0;

DECLARE @dbid INT, @query VARCHAR(1000), @dbname sysname, @fgname sysname

WHILE (SELECT COUNT(id) FROM #tmpdbs WHERE isdone = 0) > 0
BEGIN
    SELECT TOP 1 @dbname = [dbname], @dbid = [dbid] FROM #tmpdbs WHERE isdone = 0

    SET @query = 'SELECT ' + CAST(@dbid AS NVARCHAR) + ', ''' + @dbname + ''', [name], 0 FROM [' + @dbname + '].sys.filegroups WHERE [type] = ''FG'' AND is_read_only = 0;'
    INSERT INTO #tmpfgs
    EXEC (@query)

    UPDATE #tmpdbs
    SET isdone = 1
    WHERE [dbid] = @dbid
END;

IF (SELECT COUNT(ID) FROM #tmpfgs) > 0
BEGIN
    WHILE (SELECT COUNT(id) FROM #tmpfgs WHERE isdone = 0) > 0
    BEGIN
        SELECT TOP 1 @dbname = [dbname], @dbid = [dbid], @fgname = fgname FROM #tmpfgs WHERE isdone = 0

        SET @query = 'ALTER DATABASE [' + @dbname + '] MODIFY FILEGROUP [' + @fgname + '] AUTOGROW_ALL_FILES;'

        PRINT @query

        UPDATE #tmpfgs
        SET isdone = 1
        WHERE [dbid] = @dbid AND fgname = @fgname
    END
END;
GO
```

## <a name="see-also"></a>Vedere anche

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)
- [Filegroup con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)

::: moniker-end
