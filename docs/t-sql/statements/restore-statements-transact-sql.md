---
description: Istruzioni RESTORE (Transact-SQL)
title: RESTORE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RESTORE DATABASE
- RESTORE_TSQL
- RESTORE_DATABASE_TSQL
- RESTORE
- RESTORE_LOG_TSQL
- RESTORE LOG
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE DATABASE, see RESTORE statement
- full backups [SQL Server]
- RECOVERY option
- database snapshots [SQL Server], reverting to
- STOPAT syntax
- differential backups, RESTORE statement
- point in time recovery [SQL Server]
- restoring [SQL Server]
- NORECOVERY option
- online restores [SQL Server], RESTORE statement
- moving databases
- RESTORE statement, syntax
- cross-platform restores
- restoring databases [SQL Server], options
- RESTORE statement
- snapshots [SQL Server database snapshots], reverting to
- reverting database snapshots
- transaction log backups [SQL Server], RESTORE statement
- RESTORE LOG, see RESTORE statement
ms.assetid: 877ecd57-3f2e-4237-890a-08f16e944ef1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: 70fe2283752411bb7c9ffc6818b5a5165b66be79
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076093"
---
# <a name="restore-statements-transact-sql"></a>Istruzioni RESTORE (Transact-SQL)

Consente di ripristinare i backup del database SQL eseguiti tramite il comando BACKUP.

Fare clic su una delle schede seguenti per la sintassi, gli argomenti, i commenti, le autorizzazioni e gli esempi per la specifica versione di SQL in uso.

Per altre informazioni sulle convenzioni di sintassi, vedere [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Questo comando consente di effettuare gli scenari di ripristino seguenti:

- Ripristinare un intero database da un backup completo del database (ripristino completo).
- Ripristinare parte di un database (ripristino parziale).
- Ripristinare file o filegroup in un database (ripristino di file).
- Ripristinare pagine specifiche in un database (ripristino di pagina).
- Ripristinare un log delle transazioni in un database (ripristino del log delle transazioni).
- Eseguire un ripristino temporizzato di un database fino al punto nel tempo acquisito in uno snapshot del database.

Per altre informazioni sugli scenari di ripristino di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Panoramica del ripristino e del recupero](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md). Per altre informazioni sulle descrizioni degli argomenti, vedere [Argomenti dell'istruzione RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Quando si ripristina un database da un'altra istanza, vedere le informazioni in [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).

> [!NOTE]
> Per altre informazioni sul ripristino dal servizio di archiviazione BLOB di Microsoft Azure, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="syntax"></a>Sintassi

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
    [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
   | ,  <general_WITH_options> [ ,...n ]
   | , <replication_WITH_option>
   | , <change_data_capture_WITH_option>
   | , <FILESTREAM_WITH_option>
   | , <service_broker_WITH options>
   | , \<point_in_time_WITH_options-RESTORE_DATABASE>
   } [ ,...n ]
 ]
[;]

--To perform the first step of the initial restore sequence of a piecemeal restore:
RESTORE DATABASE { database_name | @database_name_var }
   <files_or_filegroups> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
      PARTIAL, NORECOVERY
      [  , <general_WITH_options> [ ,...n ]
       | , \<point_in_time_WITH_options-RESTORE_DATABASE>
      ] [ ,...n ]
[;]  
  
--To Restore Specific Files or Filegroups:
RESTORE DATABASE { database_name | @database_name_var }
   <file_or_filegroup> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
   {
      [ RECOVERY | NORECOVERY ]
      [ , <general_WITH_options> [ ,...n ] ]
   } [ ,...n ]
[;]  
  
--To Restore Specific Pages:
RESTORE DATABASE { database_name | @database_name_var }
   PAGE = 'file:page [ ,...n ]'
 [ , <file_or_filegroups> ] [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
       NORECOVERY
      [ , <general_WITH_options> [ ,...n ] ]
[;]

--To Restore a Transaction Log:
RESTORE LOG { database_name | @database_name_var }
 [ <file_or_filegroup_or_pages> [ ,...n ] ]
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
     [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
    | , <general_WITH_options> [ ,...n ]
    | , <replication_WITH_option>
    | , \<point_in_time_WITH_options-RESTORE_LOG>
   } [ ,...n ]
 ]
[;]

--To Revert a Database to a Database Snapshot:
RESTORE DATABASE { database_name | @database_name_var }
FROM DATABASE_SNAPSHOT = database_snapshot_name

<backup_device>::=
{
   { logical_backup_device_name |
      @logical_backup_device_name_var }
 | { DISK
     | TAPE
     | URL
   } = { 'physical_backup_device_name' |
      @physical_backup_device_name_var }
}
Note: URL is the format used to specify the location and the file name for the Microsoft Azure Blob. Although Microsoft Azure storage is a service, the implementation is similar to disk and tape to allow for a consistent and seamless restore experience for all the three devices.
<files_or_filegroups>::=
{
   FILE = { logical_file_name_in_backup | @logical_file_name_in_backup_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 | READ_WRITE_FILEGROUPS
}

<general_WITH_options> [ ,...n ]::=
--Restore Operation Options
   MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'
          [ ,...n ]
 | REPLACE
 | RESTART
 | RESTRICTED_USER | CREDENTIAL

--Backup Set Options
 | FILE = { backup_set_file_number | @backup_set_file_number }
 | PASSWORD = { password | @password_variable }

--Media Set Options
 | MEDIANAME = { media_name | @media_name_variable }
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
 | BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
 | { CHECKSUM | NO_CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Monitoring Options
 | STATS [ = percentage ]

--Tape Options.
 | { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

<replication_WITH_option>::=
 | KEEP_REPLICATION

<change_data_capture_WITH_option>::=
 | KEEP_CDC

<FILESTREAM_WITH_option>::=
 | FILESTREAM ( DIRECTORY_NAME = directory_name )

<service_broker_WITH_options>::=
 | ENABLE_BROKER
 | ERROR_BROKER_CONVERSATIONS
 | NEW_BROKER

\<point_in_time_WITH_options-RESTORE_DATABASE>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
   }

\<point_in_time_WITH_options-RESTORE_LOG>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
   }
```

## <a name="arguments"></a>Argomenti

Per le descrizioni degli argomenti, vedere [Argomenti dell'istruzione RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md).

## <a name="about-restore-scenarios"></a>Informazioni sugli scenari di ripristino

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta un'ampia gamma di scenari di ripristino:

- ripristino di database completo

  Viene ripristinato l'intero database, partendo da un backup completo del database, che può essere seguito dal ripristino di un backup differenziale del database e dei backup del log. Per altre informazioni, vedere [Ripristini di database completi - Modello di recupero con registrazione minima](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) o [Ripristini di database completi - Modello di recupero con registrazione completa](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).

- Ripristino di file

  Viene ripristinato un file o un filegroup in un database con più filegroup. Si noti che con il modello di recupero con registrazione minima, il file deve appartenere a un filegroup di sola lettura. Dopo un ripristino di file completo è possibile ripristinare un backup di file differenziale. Per altre informazioni, vedere [Ripristini di file - Modello di recupero con registrazione completa](../../relational-databases/backup-restore/file-restores-full-recovery-model.md) e [Ripristini di file - Modello di recupero con registrazione minima](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md).

- Ripristino di pagine

  Vengono ripristinate singole pagine. Il ripristino di pagine è disponibile solo con i modelli di recupero con registrazione completa e con registrazione minima delle operazioni bulk. Per altre informazioni, vedere [Ripristinare pagine - SQL Server](../../relational-databases/backup-restore/restore-pages-sql-server.md).

- Ripristino a fasi

  Il database viene ripristinato in più fasi, partendo dal filegroup primario e da uno o più filegroup secondari. Il ripristino a fasi inizia con un comando RESTORE DATABASE con l'opzione PARTIAL e l'impostazione di uno o più filegroup secondari da ripristinare. Per altre informazioni, vedere [Ripristini a fasi - SQL Server](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).

- Solo recupero

  Vengono recuperati i dati che sono già coerenti con il database e che devono solo essere resi disponibili. Per altre informazioni, vedere [Recupero di un database senza ripristino dei dati](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).

- Ripristino del log delle transazioni

  Con il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, il ripristino dei backup del log è necessario per raggiungere il punto di recupero desiderato. Per altre informazioni sul ripristino di backup del log, vedere [Applicazione dei backup di log delle transazioni - SQL Server](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).

- Preparare un database di disponibilità per un gruppo di disponibilità Always On

  Per altre informazioni, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità - SQL Server](../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).

- Preparare un database mirror per il mirroring del database

  Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring - SQL Server](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).

- Ripristino online

  > [!NOTE]
  > Il ripristino in linea è consentito solo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition.

Se il ripristino online è supportato e il database è online, i ripristini di file e pagine vengono eseguiti automaticamente online, così come i ripristini di filegroup secondari dopo la fase iniziale di un ripristino a fasi.

  > [!NOTE]
  > Nei ripristini online possono essere incluse [transazioni posticipate](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).

Per altre informazioni, vedere [Ripristino in linea](../../relational-databases/backup-restore/online-restore-sql-server.md).

## <a name="additional-considerations-about-restore-options"></a>Considerazioni aggiuntive sulle opzioni RESTORE

### <a name="discontinued-restore-keywords"></a>Parole chiave RESTORE obsolete

In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] non sono più disponibili le parole chiave seguenti:

|Parola chiave obsoleta|Parola chiave di sostituzione|Esempio di parola chiave di sostituzione|
|--------------------------|------------------|------------------------------------|
|LOAD|RESTORE|`RESTORE DATABASE`|
|TRANSACTION|LOG|`RESTORE LOG`|
|DBO_ONLY|RESTRICTED_USER|`RESTORE DATABASE ... WITH RESTRICTED_USER`|

### <a name="restore-log"></a>RESTORE LOG

È possibile includere un elenco di file in RESTORE LOG per consentire la creazione di file durante il rollforward. Questa procedura viene utilizzata quando il backup del log contiene record di log scritti al momento dell'aggiunta di un file al database.

> [!NOTE]
> Per un database impostato per l'utilizzo del modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, nella maggior parte dei casi è necessario eseguire il backup della parte finale del log prima di ripristinare il database. Se si esegue il ripristino di un database senza aver prima eseguito il backup della parte finale del log si verificherà un errore, a meno che nell'istruzione RESTORE DATABASE non sia inclusa la clausola WITH REPLACE o WITH STOPAT, che deve consentire di specificare l'ora o la transazione verificatasi al termine del backup dei dati. Per altre informazioni sui backup della parte finale del log, vedere [Backup della parte finale del log](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

### <a name="comparison-of-recovery-and-norecovery"></a>Differenze tra RECOVERY e NORECOVERY
Il rollback viene controllato dall'istruzione RESTORE tramite le opzioni [ RECOVERY | NORECOVERY ]:

- NORECOVERY specifica che il rollback non viene eseguito. Questo consente la continuazione del rollforward con l'istruzione successiva nella sequenza.

  In questo caso, tramite la sequenza di ripristino è possibile ripristinare altri backup ed eseguirne il roll forward.

- RECOVERY è l'opzione predefinita e indica che il rollback verrà eseguito dopo il completamento del rollforward per il backup corrente.

  Per il recupero del database è necessario che l'intero set di dati da ripristinare (*set di rollforward*) sia coerente con il database. Se il rollforward del set di rollforward non è stato eseguito a un livello sufficiente per assicurarne la coerenza con il database e si specifica l'opzione RECOVERY, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore. Per altre informazioni sul processo di recupero, vedere [Panoramica del ripristino e del recupero (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

## <a name="compatibility-support"></a>Informazioni sulla compatibilità
I backup dei database **master**, **model** e **msdb** creati tramite una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non possono essere ripristinati da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Nessun backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere ripristinato in una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedente a quella tramite cui è stato creato il backup.

Ogni versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza un percorso predefinito diverso rispetto alle versioni precedenti. Per ripristinare un database creato nel percorso predefinito per i backup di versioni precedenti, è necessario utilizzare l'opzione MOVE. Per informazioni sul nuovo percorso predefinito, vedere [Percorsi dei file per le istanze predefinite e denominate di SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).

Dopo aver ripristinato un database di una versione precedente a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], il database viene aggiornato automaticamente. In genere, il database diventa subito disponibile. Se tuttavia un database di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] include indici full-text, questi vengono importati, reimpostati o ricompilati dal processo di aggiornamento, a seconda dell'impostazione della proprietà del server **upgrade_option**. Se l'opzione di aggiornamento è impostata per l'importazione (**upgrade_option** = 2) o la ricompilazione (**upgrade_option** = 0), gli indici full-text non saranno disponibili durante l'aggiornamento. A seconda della quantità di dati indicizzati, l'importazione può richiedere diverse ore, mentre la ricompilazione può risultare dieci volte più lunga. Si noti inoltre che quando l'opzione di aggiornamento è impostata sull'importazione, gli indici full-text associati vengono ricompilati se non è disponibile un catalogo full-text. Per modificare l'impostazione della proprietà del server **upgrade_option** , usare [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).

Quando un database viene collegato per la prima volta a una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o ripristinato, nel server non è ancora archiviata una copia della chiave master del database, crittografata dalla chiave master del servizio. È necessario usare l'istruzione **OPEN MASTER KEY** per decrittografare la chiave master del database. Dopo aver decrittografato la DMK, è possibile usare l'istruzione **ALTER MASTER KEY REGENERATE** per abilitare la decrittografia automatica per le operazioni successive, in modo da fornire al server una copia della DMK crittografata con la chiave master del servizio (SMK). Quando un database è stato aggiornato da una versione precedente, la DMK deve essere rigenerata per usare l'algoritmo AES più recente. Per altre informazioni sulla rigenerazione della DMK, vedere [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Il tempo richiesto per rigenerare la chiave DMK e aggiornarla ad AES dipende dal numero di oggetti protetti dalla DMK. È necessario rigenerare la chiave DMK per l'aggiornamento ad AES una sola volta e l'operazione non influenza le rigenerazioni future che fanno parte di una strategia di rotazione della chiave.

## <a name="general-remarks"></a>Osservazioni generali
Durante un ripristino offline, se il database specificato è in uso, l'istruzione RESTORE comporta la disconnessione degli utenti dopo un breve intervallo. Per il ripristino online di un filegroup non primario, il database può rimanere in uso a meno che il filegroup da ripristinare non venga portato offline. Tutti i dati del database specificato vengono sostituiti dai dati ripristinati.

È possibile eseguire operazioni di ripristino tra piattaforme diverse, e anche tra tipi di processore diversi, a condizione che il sistema operativo supporti le regole di confronto del database.

È possibile riavviare un'operazione RESTORE dopo un errore. È anche possibile impostare l'istruzione RESTORE per continuare le operazioni anche in caso di errori e in questo caso viene eseguito il ripristino di tutti i dati possibili (vedere l'opzione `CONTINUE_AFTER_ERROR`).

Non è possibile utilizzare RESTORE in una transazione esplicita o implicita.

Il ripristino di un database **master** danneggiato viene effettuato tramite una procedura speciale. Per altre informazioni, vedere [Backup e ripristino di database di sistema](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).

Il ripristino di un database comporta la cancellazione della cache dei piani relativa al database ripristinato. La cancellazione della cache dei piani comporta la ricompilazione di tutti i piani di esecuzione successivi e può causare un peggioramento improvviso e temporaneo delle prestazioni di esecuzione delle query. 

Per ripristinare un database di disponibilità, ripristinare innanzitutto il database in base all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quindi aggiungerlo al gruppo di disponibilità.

## <a name="interoperability"></a>Interoperabilità

### <a name="database-settings-and-restoring"></a>Impostazioni e ripristino del database

Durante il ripristino, la maggior parte delle opzioni del database che possono essere impostate con ALTER DATABASE viene reimpostata sui valori attivi alla fine del backup.

Se si utilizza l'opzione WITH RESTRICTED_USER, tuttavia, questo comportamento viene ignorato per l'impostazione dell'opzione di accesso dell'utente. Questa opzione viene sempre impostata dopo l'esecuzione di un'istruzione RESTORE in cui è inclusa l'opzione WITH RESTRICTED_USER.

### <a name="restoring-an-encrypted-database"></a>Ripristino di un database crittografato

Per ripristinare un database crittografato, è necessario poter accedere alla chiave asimmetrica o al certificato utilizzato per crittografare il database. Non è possibile effettuare l'operazione di ripristino del database senza almeno uno di questi due elementi. Di conseguenza, il certificato utilizzato per crittografare la chiave di crittografia del database deve essere conservato fino a quando il backup è necessario. Per altre informazioni, vedere [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

### <a name="restoring-a-database-enabled-for-vardecimal-storage"></a>Ripristino di un database abilitato per l'archiviazione vardecimal

Il backup e il ripristino funzionano correttamente con il formato di archiviazione **vardecimal**. Per altre informazioni sul formato di archiviazione **vardecimal**, vedere [sp_db_vardecimal_storage_forma](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).

### <a name="restore-full-text-data"></a>Ripristino di dati full-text

I dati full-text vengono ripristinati insieme agli altri dati del database durante un ripristino completo. Con la normale sintassi `RESTORE DATABASE database_name FROM backup_device`, i file full-text vengono ripristinati nell'ambito del ripristino dei file del database.

L'istruzione RESTORE consente inoltre di eseguire ripristini in posizioni alternative, ripristini differenziali, ripristini di file e filegroup, nonché ripristini differenziali di file e filegroup di dati full-text. È inoltre possibile utilizzare l'istruzione RESTORE per ripristinare solo i file full-text, come nel caso dei dati di database.

> [!NOTE]
> I cataloghi full-text importati da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] vengono ancora gestiti come file di database. Per tali file, la procedura di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] per l'esecuzione del backup dei cataloghi full-text rimane applicabile, a meno che la sospensione e la ripresa durante l'operazione di backup non siano più necessarie. Per altre informazioni, vedere [Backup e ripristino di cataloghi full-text](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md).

### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="metadata"></a>Metadati

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono incluse tabelle cronologiche per backup e ripristini in cui viene tenuta traccia dell'attività di backup e ripristino per ogni istanza del server. Quando si effettua un'operazione di ripristino, vengono modificate anche le tabelle di cronologia di backup. Per informazioni su queste tabelle, vedere [Informazioni sulla cronologia e sull'intestazione del backup](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md).

## <a name="replace-option-impact"></a><a name="REPLACEoption"></a> Impatto dell'opzione REPLACE
L'opzione REPLACE deve essere utilizzata raramente e solo dopo un'attenta valutazione. In genere, il ripristino impedisce la sovrascrittura accidentale di un database con un altro. Se il database specificato in un'istruzione RESTORE esiste già nel server corrente e il GUID del gruppo di database specificato è diverso da quello registrato nel set di backup, il database non verrà ripristinato. Questa misura di sicurezza è importante.

L'opzione REPLACE ignora diversi controlli di sicurezza importanti, eseguiti generalmente durante il ripristino. I controlli ignorati riguardano le operazioni seguenti:

- Ripristino di un database esistente con un backup di un altro database

  Con l'opzione REPLACE, il ripristino consente di sovrascrivere un database esistente con qualsiasi database incluso nel set di backup, anche se il nome del database specificato è diverso da quello registrato nel set di backup. Questa operazione può causare la sovrascrittura accidentale di un database con un altro.
  
- Ripristino di un database mediante il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk in cui manca il backup della parte finale del log e non viene utilizzata l'opzione STOPAT

  Con l'opzione REPLACE, è possibile perdere le transazioni di cui è stato eseguito il commit, in quanto non viene eseguito il backup del log in cui sono stati registrati i dati più recenti.

- Sovrascrittura di file esistenti

  In seguito a un errore, ad esempio, potrebbe essere consentita la sovrascrittura di file esistenti di tipo non corretto, ad esempio con estensione xls, o utilizzati da un altro database che non è online. Se i file vengono sovrascritti, può verificarsi la perdita di dati arbitrari, anche se il database ripristinato è completo.

## <a name="redoing-a-restore"></a>Ripetizione di un ripristino
Non è possibile annullare gli effetti di un ripristino. È tuttavia possibile annullare gli effetti della copia e del rollforward dei dati rieseguendo l'operazione file per file. Per ricominciare, ripristinare il file desiderato ed eseguire nuovamente il rollforward. Se, ad esempio, vengono ripristinati per errore troppi backup del log superando il punto previsto per l'interruzione del ripristino, è necessario riavviare la sequenza.

È possibile interrompere e riavviare una sequenza di ripristino tramite il ripristino dell'intero contenuto dei file interessati.

## <a name="reverting-a-database-to-a-database-snapshot"></a>Ripristino di un database a uno snapshot del database
L'*operazione di ripristino del database* (specificata con l'opzione DATABASE_SNAPSHOT) consente di portare un database di origine completo in un momento anteriore nel tempo tramite il ripristino del momento di uno snapshot del database, ovvero sovrascrivendo il database di origine con i dati di quel momento presenti nello snapshot specificato del database. Al momento del ripristino, può esistere solo lo snapshot da ripristinare. L'operazione di ripristino prevede la ricompilazione del log. Non è pertanto possibile eseguire successivamente il rollforward di un database ripristinato al punto dell'errore utente.

La perdita di dati è limitata agli aggiornamenti del database eseguiti dopo la creazione dello snapshot. I metadati di un database così ripristinato corrispondono a quelli esistenti al momento della creazione dello snapshot. Il ripristino a uno snapshot causa tuttavia l'eliminazione di tutti i cataloghi full-text.

Il ripristino da uno snapshot del database non è adatto per il recupero di supporti. A differenza di un normale set di backup, lo snapshot del database è una copia incompleta dei file del database. Se il database o lo snapshot del database è danneggiato, è improbabile che sia possibile eseguire il ripristino da uno snapshot. Anche quando possibile, è inoltre improbabile che il ripristino in caso di danneggiamento consenta di risolvere il problema.

### <a name="restrictions-on-reverting"></a>Restrizioni per il ripristino da snapshot
Il ripristino da snapshot non è supportato nei casi seguenti:

- Il database di origine contiene filegroup di sola lettura o compressi.
- Alcuni file online al momento della creazione dello snapshot sono offline.
- Sono disponibili più snapshot del database.

Per altre informazioni, vedere [Ripristinare un database a uno snapshot del database](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

## <a name="security"></a>Sicurezza
Per un'operazione di backup è possibile specificare password per un set di supporti o un set di backup oppure per entrambi. Se è stata impostata una password per un set di supporti o un set di backup, la password o le password corrette devono essere specificate nell'istruzione RESTORE. Queste password impediscono operazioni di ripristino non autorizzate e l'aggiunta non autorizzata di set di backup ai supporti tramite gli strumenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I supporti protetti con password possono tuttavia essere sovrascritti con l'opzione FORMAT dell'istruzione BACKUP.

> [!IMPORTANT]
> Il livello di protezione garantito da questa password è vulnerabile. Lo scopo è impedire un ripristino non corretto da parte di utenti autorizzati o non autorizzati mediante gli strumenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non impedisce la lettura dei dati di backup eseguita con altri mezzi o la sostituzione della password. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Per ottenere un livello di protezione adeguato dei backup è consigliabile archiviare i nastri di backup in un luogo sicuro oppure eseguire il backup in file su disco protetti da elenchi di controllo di accesso (ACL) appropriati. Gli elenchi di controllo di accesso devono essere impostati a livello della directory radice in cui vengono creati i backup.

> [!NOTE]
> Per informazioni specifiche sul backup e sul ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

### <a name="permissions"></a>Autorizzazioni
Se il database da ripristinare non esiste, l'utente deve avere le autorizzazioni `CREATE DATABASE` per eseguire un'istruzione RESTORE. Se il database esiste, le autorizzazioni per l'istruzione RESTORE vengono assegnate per impostazione predefinita ai membri dei ruoli predefiniti del server `sysadmin` e `dbcreator` e al proprietario (`dbo`) del database. Per l'opzione `FROM DATABASE_SNAPSHOT`, il database esiste sempre.

Le autorizzazioni per l'istruzione RESTORE vengono assegnate ai ruoli in cui le informazioni sull'appartenenza sono sempre disponibili per il server. Poiché è possibile controllare l'appartenenza ai ruoli predefiniti del database solo quando il database è accessibile e non è danneggiato, condizioni che non risultano sempre vere quando si esegue un'operazione RESTORE, i membri del ruolo predefinito del database `db_owner` non dispongono delle autorizzazioni per l'istruzione RESTORE.

## <a name="examples"></a><a name="examples"></a> Esempi

In tutti gli esempi si presuppone che sia stato eseguito un backup completo del database.

Sono disponibili gli esempi seguenti per l'istruzione RESTORE:

- R. [Ripristino di un database completo](#restoring_full_db)
- B. [Ripristino di backup completi e differenziali del database](#restoring_full_n_differential_db_backups)
- C. [Ripristino di un database con la sintassi RESTART](#restoring_db_using_RESTART)
- D. [Ripristino di un database e spostamento dei file](#restoring_db_n_move_files)
- E. [Copia di un database tramite BACKUP e RESTORE](#copying_db_using_bnr)
- F. [Ripristino temporizzato tramite STOPAT](#restoring_to_pit_using_STOPAT)
- G. [Ripristino del log delle transazioni fino a un contrassegno](#restoring_transaction_log_to_mark)
- H. [Ripristino con la sintassi TAPE](#restoring_using_TAPE)
- I. [Ripristino con la sintassi FILE e FILEGROUP](#restoring_using_FILE_n_FG)
- J. [Ripristino da uno snapshot del database](#reverting_from_db_snapshot)
- K. [Ripristino dal servizio di archiviazione BLOB di Microsoft Azure](#Azure_Blob)

> [!NOTE]
> Per altri esempi, vedere gli argomenti relativi alle procedure di ripristino elencati in [Panoramica del ripristino e del recupero](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

### <a name="a-restoring-a-full-database"></a><a name="restoring_full_db"></a> A. Ripristino di un database completo

Nell'esempio seguente viene ripristinato un backup completo del database dal dispositivo di backup logico `AdventureWorksBackups`. Per un esempio della creazione di questo dispositivo, vedere [Dispositivi di backup](../../relational-databases/backup-restore/backup-devices-sql-server.md).

```sql
RESTORE DATABASE AdventureWorks2012
  FROM AdventureWorks2012Backups;
```

> [!NOTE]
> Per un database impostato per l'utilizzo del modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nella maggior parte dei casi è necessario eseguire il backup della parte finale del log prima di ripristinare il database. Per altre informazioni, vedere [Backup della parte finale del log](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="b-restoring-full-and-differential-database-backups"></a><a name="restoring_full_n_differential_db_backups"></a> B. Ripristino di backup completi e differenziali del database

Nell'esempio seguente viene ripristinato un backup completo del database seguito da un backup differenziale dal dispositivo di backup `Z:\SQLServerBackups\AdventureWorks2012.bak`, in cui sono entrambi contenuti. Il backup completo del database da ripristinare rappresenta il sesto set di backup nel dispositivo (`FILE = 6`), mentre il backup differenziale del database rappresenta il nono set di backup nel dispositivo (`FILE = 9`). Il database viene recuperato al termine del recupero del backup differenziale.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 6
      NORECOVERY;
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 9
      RECOVERY;
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="c-restoring-a-database-using-restart-syntax"></a><a name="restoring_db_using_RESTART"></a> C. Ripristino di un database con la sintassi RESTART

Nell'esempio seguente viene utilizzata l'opzione `RESTART` per riavviare un'operazione `RESTORE` interrotta a causa di un'interruzione dell'alimentazione del server.

```sql
-- This database RESTORE halted prematurely due to power failure.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups;
-- Here is the RESTORE RESTART operation.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups WITH RESTART;
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="d-restoring-a-database-and-move-files"></a><a name="restoring_db_n_move_files"></a> D. Ripristino di un database e spostamento dei file

Nell'esempio seguente vengono ripristinati un database completo e il log delle transazioni. Il database ripristinato viene quindi spostato nella directory `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH NORECOVERY,
      MOVE 'AdventureWorks2012_Data' TO
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.mdf',
      MOVE 'AdventureWorks2012_Log'
TO 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.ldf';
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH RECOVERY;
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="e-copying-a-database-using-backup-and-restore"></a><a name="copying_db_using_bnr"></a> E. Copia di un database tramite BACKUP e RESTORE

Nell'esempio seguente vengono utilizzate le istruzioni `BACKUP` e `RESTORE` per creare una copia del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'istruzione `MOVE` consente di ripristinare i file di dati e di log nelle posizioni specificate. L'istruzione `RESTORE FILELISTONLY` viene utilizzata per determinare il numero e i nomi dei file del database da ripristinare. Alla nuova copia del database viene assegnato il nome `TestDB`. Per altre informazioni, vedere [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).

```sql
BACKUP DATABASE AdventureWorks2012
    TO AdventureWorksBackups ;

RESTORE FILELISTONLY
    FROM AdventureWorksBackups ;

RESTORE DATABASE TestDB
    FROM AdventureWorksBackups
    WITH MOVE 'AdventureWorks2012_Data' TO 'C:\MySQLServer\testdb.mdf',
    MOVE 'AdventureWorks2012_Log' TO 'C:\MySQLServer\testdb.ldf';
GO
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="f-restoring-to-a-point-in-time-using-stopat"></a><a name="restoring_to_pit_using_STOPAT"></a> F. Ripristino temporizzato tramite STOPAT

Nell'esempio seguente viene ripristinato lo stato del database corrispondente alle ore `12:00 AM` del giorno `April 15, 2020` e viene illustrata un'operazione di ripristino di più backup del log. Nel dispositivo di backup, `AdventureWorksBackups`, il backup completo del database da ripristinare è il terzo set di backup (`FILE = 3`), il primo backup del log è il quarto set di backup (`FILE = 4`) e il secondo backup del log è il quinto set di backup (`FILE = 5`).

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=3, NORECOVERY;
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
RESTORE DATABASE AdventureWorks2012 WITH RECOVERY;

```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="g-restoring-the-transaction-log-to-a-mark"></a><a name="restoring_transaction_log_to_mark"></a> G. Ripristino del log delle transazioni fino a un contrassegno

Nell'esempio seguente viene ripristinato il log delle transazioni fino al contrassegno nella transazione contrassegnata denominata `ListPriceUpdate`.

```sql
USE AdventureWorks2012
GO
BEGIN TRANSACTION ListPriceUpdate
    WITH MARK 'UPDATE Product list prices';
GO

UPDATE Production.Product
    SET ListPrice = ListPrice * 1.10
    WHERE ProductNumber LIKE 'BK-%';
GO

COMMIT TRANSACTION ListPriceUpdate;
GO

-- Time passes. Regular database
-- and log backups are taken.
-- An error occurs in the database.
USE master;
GO

RESTORE DATABASE AdventureWorks2012
FROM AdventureWorksBackups
WITH FILE = 3, NORECOVERY;
GO

RESTORE LOG AdventureWorks2012
  FROM AdventureWorksBackups
    WITH FILE = 4,
    RECOVERY,
    STOPATMARK = ListPriceUpdate;
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="h-restoring-using-tape-syntax"></a><a name="restoring_using_TAPE"></a> H. Ripristino con la sintassi TAPE

Nell'esempio seguente viene ripristinato un backup completo del database da un dispositivo di backup di tipo `TAPE`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM TAPE = '\\.\tape0';
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="i-restoring-using-file-and-filegroup-syntax"></a><a name="restoring_using_FILE_n_FG"></a> I. Ripristino con la sintassi FILE e FILEGROUP

Nell'esempio seguente viene ripristinato un database denominato `MyDatabase` che include due file, un filegroup secondario e un log delle transazioni. Per il database viene utilizzato il modello di recupero con registrazione completa.

Il backup del database è il nono set di backup nel set di supporti in un dispositivo di backup logico denominato `MyDatabaseBackups`. Vengono quindi ripristinati tre backup del log, disponibili nei tre set di backup successivi (`10`, `11` e `12`) nel dispositivo `MyDatabaseBackups`, utilizzando `WITH NORECOVERY`. Dopo il ripristino dell'ultimo backup del log, il database viene recuperato.

> [!NOTE]
> Il recupero viene eseguito come passaggio distinto per ridurre la possibilità che questa operazione venga eseguita troppo presto, ovvero prima del ripristino di tutti i backup del log. Per altre informazioni sul processo di recupero, vedere [Panoramica del ripristino e del recupero (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

Si noti che in `RESTORE DATABASE` sono disponibili due tipi di opzioni `FILE`. Le opzioni `FILE` che precedono il nome del dispositivo di backup specificano i nomi di file logici dei file di database da ripristinare dal set di backup, ad esempio `FILE = 'MyDatabase_data_1'`. Questo set di backup non è il primo backup del database nel set di supporti. La relativa posizione nel set di supporti viene pertanto indicata tramite l'opzione `FILE` nella clausola `WITH`, `FILE=9`.

```sql
RESTORE DATABASE MyDatabase
    FILE = 'MyDatabase_data_1',
    FILE = 'MyDatabase_data_2',
    FILEGROUP = 'new_customers'
    FROM MyDatabaseBackups
    WITH
      FILE = 9,
      NORECOVERY;
GO  
-- Restore the log backups
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 10,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 11,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 12,
      NORECOVERY;
GO
--Recover the database
RESTORE DATABASE MyDatabase WITH RECOVERY;
GO
```

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="j-reverting-from-a-database-snapshot"></a><a name="reverting_from_db_snapshot"></a> J. Ripristino da uno snapshot del database

Nell'esempio seguente viene eseguito il ripristino di un database con uno snapshot del database. Nell'esempio si presuppone che per il database esista un solo snapshot. Per un esempio di come creare lo snapshot di questo database, vedere [Creare uno snapshot del database](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).

> [!NOTE]
> Il ripristino da uno snapshot causa l'eliminazione di tutti i cataloghi full-text.

```sql
USE master;
RESTORE DATABASE AdventureWorks2012 FROM DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';
GO
```

Per altre informazioni, vedere [Ripristinare un database a uno snapshot del database](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

[&#91;Inizio degli esempi&#93;](#examples)

### <a name="k-restoring-from-the-microsoft-azure-blob-storage-service"></a><a name="Azure_Blob"></a> K. Ripristino dal servizio di archiviazione BLOB di Microsoft Azure

I tre esempi seguenti prevedono l'uso del servizio di archiviazione di Microsoft Azure. Il nome dell'account di archiviazione è `mystorageaccount`, mentre Il contenitore per i file di dati è denominato `myfirstcontainer`. Il contenitore per i file di backup è denominato `mysecondcontainer`. Sono stati creati criteri di accesso archiviati con diritti di lettura, scrittura, eliminazione ed elenco per ogni contenitore. Le credenziali di SQL Server sono state create usando una firma di accesso condiviso associata ai criteri di accesso archiviati. Per informazioni specifiche sul backup e sul ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

**K1. Ripristinare un backup completo del database dal servizio di archiviazione di Microsoft Azure**    
Un backup completo, disponibile in `mysecondcontainer`, del database `Sales` verrà ripristinato in `myfirstcontainer`. `Sales` attualmente non esiste nel server.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

**K2. Ripristinare un backup completo del database dal servizio di archiviazione di Microsoft Azure in una risorsa di archiviazione locale** Un backup completo del database `Sales`, situato in `mysecondcontainer`, verrà ripristinato in una risorsa di archiviazione locale. `Sales` attualmente non esiste nel server.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'H:\DATA\Sales_Data1.mdf',
  MOVE 'Sales_log' to 'O:\LOG\Sales_log.ldf',
  STATS = 10;
```

**K3. Ripristinare un backup completo del database da una risorsa di archiviazione locale al servizio di archiviazione di Microsoft Azure**

```sql
RESTORE DATABASE Sales
  FROM DISK = 'E:\BAK\Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

[&#91;Inizio degli esempi&#93;](#examples)

## <a name="more-information"></a>Ulteriori informazioni

[Panoramica del ripristino e del recupero (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)     
[Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)    
[Backup e ripristino di database di sistema (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)      
[Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)     
[Backup e ripristino di indici e cataloghi full-text](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)      
[Backup e ripristino di database replicati](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)      
[BACKUP](../../t-sql/statements/restore-statements-transact-sql.md)      
[Set di supporti, gruppi di supporti e set di backup](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)      
[RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)     
[RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)     
[RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)     
[RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)     
[Informazioni sulla cronologia e sull'intestazione del backup](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)       

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-2017&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Istanza gestita di SQL \*_**
    :::column-end:::
    :::column:::
        [Piattaforma di strumenti<br />analitici (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Istanza gestita di SQL di Azure

Questo comando consente di ripristinare un intero database da un backup completo del database (ripristino completo) dall'account di archiviazione BLOB di Azure.

Per informazioni sugli altri comandi RESTORE supportati, vedere:

- [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY ONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)

> [!IMPORTANT]
> Per eseguire il ripristino dai backup automatici di Istanza gestita di SQL di Azure, vedere [Ripristino del database SQL](/azure/sql-database/sql-database-recovery-using-backups).

## <a name="syntax"></a>Sintassi

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 FROM URL = { 'physical_device_name' | @physical_device_name_var } [ ,...n ]
[;]

```

## <a name="arguments"></a>Argomenti

DATABASE

Specifica il database di destinazione.

FROM URL

Specifica uno o più dispositivi di backup inseriti sugli URL che verranno usati per l'operazione di ripristino. Il formato URL viene usato per il ripristino dei backup dal servizio di archiviazione di Microsoft Azure.

> [!IMPORTANT]
> Per eseguire il ripristino da più dispositivi durante il ripristino da URL, è necessario usare token di firma di accesso condiviso (SAS). Per esempi di creazione di una firma di accesso condiviso, vedere [Backup di SQL Server nell'URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md) e [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Semplificazione della creazione di credenziali SQL con token di firma di accesso condiviso (SAS) in Archiviazione di Azure con Powershell).

*n* è un segnaposto che indica che è possibile specificare fino a 64 dispositivi di backup in un elenco delimitato da virgole.

## <a name="general-remarks"></a>Osservazioni generali

Come prerequisito, è necessario creare una credenziale con il nome corrispondente all'URL dell'account di archiviazione BLOB e la firma di accesso condiviso inserita come segreto. Il comando RESTORE cercherà le credenziali usando l'URL di archiviazione BLOB per trovare le informazioni necessarie per leggere il dispositivo di backup.

L'operazione di ripristino è asincrona, vale a dire che il ripristino continua anche se la connessione client si interrompe. Se la connessione è interrotta, è possibile controllare nella visualizzazione [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) lo stato di un'operazione di ripristino (e lo stato di CREATE e DROP DATABASE).

Le seguenti opzioni di database sono impostate/sottoposte a override e non possono essere modificate in un secondo momento:

- NEW_BROKER (se il broker non è abilitato nel file con estensione bak)
- ENABLE_BROKER (se il broker non è abilitato nel file con estensione bak)
- AUTO_CLOSE=OFF (se un database nel file con estensione bak ha l'impostazione AUTO_CLOSE=ON)
- RECOVERY FULL (se un database nel file con estensione bak ha la modalità di ripristino SIMPLE o BULK_LOGGED)
- Il filegroup con ottimizzazione per la memoria viene aggiunto e denominato XTP se non era presente nel file con estensione bak di origine. Qualsiasi filegroup con ottimizzazione per la memoria esistente viene rinominato come XTP
- Le opzioni SINGLE_USER e RESTRICTED_USER vengono convertite in MULTI_USER

## <a name="limitations---sql-managed-instance"></a>Limitazioni - Istanza gestita di SQL

Si applicano le seguenti limitazioni:

- I file con estensione bak contenenti più set di backup non possono essere ripristinati.
- I file con estensione bak contenenti più file di log non possono essere ripristinati.
- Il ripristino non riesce se il file con estensione bak contiene dati FILESTREAM.
- Non è possibile ripristinare i backup contenenti database con oggetti in memoria attivi in un livello di prestazioni Utilizzo generico.
- Attualmente non è possibile ripristinare i backup contenenti database in modalità di sola lettura. Questa limitazione verrà rimossa a breve.

Per altre informazioni, vedere [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance).

## <a name="restoring-an-encrypted-database"></a>Ripristino di un database crittografato
Per ripristinare un database crittografato, è necessario poter accedere alla chiave asimmetrica o al certificato utilizzato per crittografare il database. Non è possibile effettuare l'operazione di ripristino del database senza almeno uno di questi due elementi. Di conseguenza, il certificato utilizzato per crittografare la chiave di crittografia del database deve essere conservato fino a quando il backup è necessario. Per altre informazioni, vedere [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="permissions"></a>Autorizzazioni
Per eseguire l'istruzione RESTORE, l'utente deve avere le autorizzazioni `CREATE DATABASE`.

```sql
CREATE LOGIN mylogin WITH PASSWORD = 'Very Strong Pwd123!';
GRANT CREATE ANY DATABASE TO [mylogin];
```

Le autorizzazioni per l'istruzione RESTORE vengono assegnate ai ruoli in cui le informazioni sull'appartenenza sono sempre disponibili per il server. Poiché è possibile controllare l'appartenenza ai ruoli predefiniti del database solo quando il database è accessibile e non è danneggiato, condizioni che non risultano sempre vere quando si esegue un'operazione RESTORE, i membri del ruolo predefinito del database `db_owner` non dispongono delle autorizzazioni per l'istruzione RESTORE.

## <a name="examples"></a><a name="examples"></a> Esempi

L'esempio seguente ripristina un backup di database di sola copia da URL, inclusa la creazione di una credenziale.

### <a name="a-restore-database-from-four-backup-devices"></a><a name="restore-mi-database"></a> A. Ripristinare il database da quattro dispositivi di backup

```sql

-- Create credential
CREATE CREDENTIAL [https://mybackups.blob.core.windows.net/wide-world-importers]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
      SECRET = 'sv=2017-11-09&ss=bq&srt=sco&sp=rl&se=2022-06-19T22:41:07Z&st=2018-06-01T14:41:07Z&spr=https&sig=s7wddcf0w%3D';
GO
-- Restore database
RESTORE DATABASE WideWorldImportersStandard
FROM URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/00-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/01-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/02-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/03-WideWorldImporters-Standard.bak'
```

Se il database esiste già viene visualizzato l'errore seguente:

```
Msg 1801, Level 16, State 1, Line 9
Database 'WideWorldImportersStandard' already exists. Choose a different database name.
```

### <a name="b-restore-database-specified-via-variable"></a><a name="restore-mi-database-variables"></a> B. Ripristinare il database specificato tramite la variabile

```sql
DECLARE @db_name sysname = 'WideWorldImportersStandard';
DECLARE @url nvarchar(400) = N'https://mybackups.blob.core.windows.net/wide-world-importers/WideWorldImporters-Standard.bak';

RESTORE DATABASE @db_name
FROM URL = @url
```

### <a name="c-track-progress-of-restore-statement"></a><a name="restore-mi-database-progress"></a> C. Tenere traccia dello stato dell'istruzione RESTORE

```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command = 'RESTORE DATABASE'
```

> [!Note]
> In questa visualizzazione verranno probabilmente mostrate due richieste di ripristino. Una è l'istruzione RESTORE originale inviata dal client e l'altra è l'istruzione RESTORE in background eseguita anche se la connessione client non riesce.

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Istanza gestita di SQL](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Piattaforma di strumenti<br />analitici (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema della piattaforma di analisi

Ripristina un database utente [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] da un backup di database a un'appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Il database viene ripristinato da un backup creato in precedenza dal comando [BACKUP DATABASE - Piattaforma di strumenti analitici](../../t-sql/statements/backup-transact-sql.md) di [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Usare le operazioni di backup e ripristino per creare un piano di ripristino di emergenza o per spostare i database da un'appliance a un'altra.

> [!NOTE]
> Il ripristino del database master include il ripristino delle informazioni di accesso all'appliance. Per ripristinare il database master, usare la pagina [Ripristinare il database master](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md) nello strumento **Configuration Manager**. Questa operazione può essere eseguita da un amministratore con accesso al nodo di controllo. Per altre informazioni sui backup di database [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vedere la sezione relativa a backup e ripristino nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Sintassi

```syntaxsql

-- Restore the master database
-- Use the Configuration Manager tool.

Restore a full user database backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\full_backup_directory'
[;]

--Restore a full user database backup and then a differential backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\differential_backup_directory'
    WITH [ ( ] BASE = '\\UNC_path\full_backup_directory' [ ) ]
[;]

--Restore header information for a full or differential user database backup.
RESTORE HEADERONLY
    FROM DISK = '\\UNC_path\backup_directory'
[;]
```

## <a name="arguments"></a>Argomenti

RESTORE DATABASE *database_name* specifica il ripristino di un database utente in un database denominato *database_name*. Il database ripristinato può avere un nome diverso da quello del database di origine di cui è stato eseguito il backup. *database_name* non può essere un database già esistente nell'appliance di destinazione. Per altri dettagli sui nomi consentiti per i database, vedere la sezione relativa alle regole di denominazione degli oggetti nella [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Il ripristino di un database utente implica il ripristino di un backup completo del database e, facoltativamente, di un backup differenziale nell'appliance. Il ripristino di un database utente include il ripristino degli utenti e dei ruoli del database.

FROM DISK = '\\\\*UNC_path*\\*backup_directory*' indica il percorso di rete e la directory da cui [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ripristina i file di backup. Ad esempio, FROM DISK = '\\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup'.

*backup_directory* specifica il nome della directory che contiene il backup completo o differenziale. È ad esempio possibile eseguire un'operazione RESTORE HEADERONLY per un backup completo o differenziale.

*full_backup_directory* specifica il nome della directory che contiene il backup completo.

*differential_backup_directory* specifica il nome della directory che contiene il backup differenziale.

- Il percorso e la directory di backup devono essere già esistenti e devono essere specificati sotto forma di percorso UNC completo.
- Il percorso della directory di backup non può essere un percorso locale e non può essere il percorso di uno dei nodi di appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
- La lunghezza massima del percorso UNC e del nome della directory di backup è di 200 caratteri.
- Il server o l'host deve essere specificato come indirizzo IP.

RESTORE HEADERONLY specifica che vengono restituite solo le informazioni di intestazione per un backup del database utente. Tra gli altri campi, l'intestazione include la descrizione in formato testo del backup e il nome del backup stesso. Il nome del backup non deve necessariamente corrispondere al nome della directory in cui sono archiviati i file di backup.

I risultati di RESTORE HEADERONLY seguono il modello dei risultati di RESTORE HEADERONLY di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il risultato ha più di 50 colonne, che non vengono usate tutte da [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Per una descrizione delle colonne nei risultati di RESTORE HEADERONLY di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).

## <a name="permissions"></a>Autorizzazioni
È necessaria l'autorizzazione `CREATE ANY DATABASE`.

È necessario un account di Windows con l'autorizzazione di accesso e lettura per la directory di backup. È anche necessario archiviare il nome e la password dell'account di Windows in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

- Per verificare le credenziali già presenti, usare [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).
- Per aggiungere o aggiornare le credenziali, usare [sp_pdw_add_network_credentials - [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).
- Per rimuovere credenziali da [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], usare [sp_pdw_remove_network_credentials - [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).

## <a name="error-handling"></a>Gestione degli errori

Il comando RESTORE DATABASE genera errori nelle condizioni seguenti:

- Il nome del database da ripristinare esiste già nell'appliance di destinazione. Per evitare questo problema, scegliere un nome di database univoco oppure rilasciare il database esistente prima di eseguire il ripristino.
- Nella directory di backup è presente un set di file di backup non valido.
- Le autorizzazioni di accesso non sono sufficienti per il ripristino di un database.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] non ha le autorizzazioni corrette per il percorso di rete in cui si trovano i file di backup.
- Il percorso di rete della directory di backup non esiste o non è disponibile.
- Lo spazio su disco è insufficiente nei nodi di calcolo o nel nodo di controllo. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] non conferma la disponibilità di spazio su disco sufficiente nell'appliance prima dell'avvio del ripristino. È quindi possibile che venga generato un errore di spazio su disco insufficiente durante l'esecuzione dell'istruzione RESTORE DATABASE. Quando si verifica l'errore di spazio su disco insufficiente, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] esegue il rollback del ripristino.
- L'appliance di destinazione in cui il database è in corso di ripristino ha un numero di nodi di calcolo inferiore a quello dell'appliance di origine da cui è stato eseguito il backup del database.
- Viene tentato il ripristino del database dall'interno di una transazione.

## <a name="general-remarks"></a>Osservazioni generali

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] tiene traccia dell'esito positivo del ripristino dei database. Prima di ripristinare un backup differenziale del database, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verifica che il ripristino completo del database sia stato completato.

Dopo un ripristino, il database utente ha un livello di compatibilità pari a 120. Questo vale per tutti i database, indipendentemente dal livello di compatibilità originale.

## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>Ripristino in un'appliance con un numero maggiore di nodi di calcolo

Eseguire [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) dopo il ripristino di un database da un'appliance più piccola a una più grande, poiché la ridistribuzione aumenta le dimensioni del log delle transazioni.

Il ripristino di un backup in un'appliance con un numero maggiore di nodi di calcolo aumenta la dimensione allocata al database proporzionalmente al numero di nodi di calcolo.

Se ad esempio si ripristina un database da 60 GB da un'appliance da 2 nodi (30 GB per nodo) in un'appliance da 6 nodi, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crea un database di 180 GB (6 nodi con 30 GB per nodo) nell'appliance da 6 nodi. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] inizialmente ripristina il database in 2 nodi, in modo che corrisponda alla configurazione dell'origine, e quindi ridistribuisce i dati in tutti i 6 nodi.

Dopo la ridistribuzione, ogni nodo di calcolo contiene meno dati effettivi e più spazio disponibile rispetto a ogni nodo di calcolo nell'appliance di origine di dimensioni inferiori. Usare lo spazio aggiuntivo per l'aggiunta di altri dati al database. Se le dimensioni del database ripristinato sono maggiori del necessario, è possibile usare [ALTER DATABASE - PDW](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) per compattare le dimensioni dei file del database stesso.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

In questa sezione, per appliance di origine si intende l'appliance da cui è stato creato il backup del database e per appliance di destinazione si intende l'appliance in cui viene ripristinato il database.

- Il ripristino di un database non implica la rigenerazione automatica delle statistiche.
- In qualsiasi momento, nell'appliance può essere in esecuzione una sola istruzione RESTORE DATABASE o BACKUP DATABASE. Se vengono inviate contemporaneamente più istruzioni di backup e ripristino, l'appliance le inserisce in una coda e le elabora una alla volta.
- È possibile ripristinare un backup del database solo in un'appliance di destinazione [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] con un numero di nodi di calcolo uguale o maggiore a quello dell'appliance di origine. L'appliance di destinazione non può avere un numero di nodi di calcolo minore dell'appliance di origine.
- Non è possibile ripristinare un backup creato in un'appliance con hardware SQL Server 2012 PDW in un'appliance con hardware SQL Server 2008 R2. Ciò vale anche se l'appliance è stata originariamente acquistata con hardware SQL Server 2008 R2 PDW e ora esegue software SQL Server 2012 PDW.

## <a name="locking"></a>Blocco
Acquisisce un blocco esclusivo per l'oggetto DATABASE.

## <a name="examples"></a>Esempi

### <a name="a-simple-restore-examples"></a>R. Esempi di RESTORE semplici

L'esempio seguente ripristina un backup completo nel database `SalesInvoices2013`. I file di backup vengono archiviati nella directory `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Nell'appliance di destinazione non può essere già presente un database SalesInvoices2013. In caso contrario, questo comando avrà esito negativo e genererà un errore.

```sql
RESTORE DATABASE SalesInvoices2013
FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="b-restore-a-full-and-differential-backup"></a>B. Ripristinare un backup completo e un backup differenziale

L'esempio seguente ripristina un backup completo e quindi un backup differenziale nel database SalesInvoices2013

Il backup completo del database viene ripristinato usando il backup completo archiviato nella directory `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Se il ripristino viene completato correttamente, viene ripristinato il backup differenziale nel database SalesInvoices2013. Il backup differenziale viene archiviato nella directory `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff`.

```syntaxsql
RESTORE DATABASE SalesInvoices2013
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH BASE = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]

```

### <a name="c-restoring-the-backup-header"></a>C. Ripristino dell'intestazione del backup

Questo esempio ripristina le informazioni di intestazione per il backup del database `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Il comando ha come risultato una riga di informazioni per il backup Invoices2013Full.

```syntaxsql
RESTORE HEADERONLY
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]
```

È possibile usare le informazioni di intestazione per controllare il contenuto di un backup o per assicurarsi che l'appliance di ripristino di destinazione sia compatibile con l'appliance di backup di origine prima di tentare il ripristino del backup.

## <a name="see-also"></a>Vedere anche
[BACKUP DATABASE - Piattaforma di strumenti analitici](../../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)     

::: moniker-end