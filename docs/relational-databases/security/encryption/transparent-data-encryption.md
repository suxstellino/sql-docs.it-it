---
title: Transparent Data Encryption (TDE) | Microsoft Docs
description: Informazioni su Transparent Data Encryption (TDE) che crittografa i dati di SQL Server, del database SQL di Azure e di Azure Synapse Analytics, ovvero esegue la crittografia dei dati inattivi.
ms.custom: ''
ms.date: 05/09/2019
ms.prod: sql
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption
- database encryption key, about
- TDE
- database encryption key
- TDE, about
- Transparent Data Encryption, about
- encryption [SQL Server], transparent data encryption
ms.assetid: c75d0d4b-4008-4e71-9a9d-cee2a566bd3b
author: shohamMSFT
ms.author: shohamd
ms.reviewer: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 598f57d152308cba8127f4f7351b932e3422c0bd
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298878"
---
# <a name="transparent-data-encryption-tde"></a>Transparent Data Encryption (TDE)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

*Transparent Data Encryption* (TDE) crittografa i file di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDWfull](../../../includes/sssdwfull-md.md)]. Questa crittografia è nota come crittografia dei dati inattivi.

Per proteggere un database, è possibile adottare precauzioni come:

* Progettazione di un sistema protetto.
* Crittografia degli asset riservati.
* Creazione di un firewall per i server di database.

Tuttavia, un utente malintenzionato che riesce a sottrarre supporti fisici come le unità o i nastri di backup può ripristinare o collegare il database ed esplorarne i dati.

Una soluzione consiste nel crittografare i dati sensibili in un database e usare un certificato per proteggere le chiavi di crittografia dei dati. Questa soluzione impedisce l'uso dei dati senza le chiavi. È tuttavia necessario pianificare in anticipo questo tipo di protezione.

TDE esegue la crittografia e la decrittografia delle operazioni di I/O di file di dati e log in tempo reale. Per la crittografia viene usata una chiave di crittografia del database (DEK). Il record di avvio del database archivia la chiave per la disponibilità durante il ripristino. La chiave DEK è una chiave simmetrica. È protetta da un certificato archiviato dal database master del server o da una chiave asimmetrica protetta da un modulo EKM.

TDE consente di proteggere i dati inattivi, ovvero i file di dati e di log, e assicura la conformità a numerose leggi, normative e linee guida stabilite in vari settori. Gli sviluppatori di software possono ora crittografare i dati usando gli algoritmi di crittografia AES e 3DES senza modificare applicazioni esistenti.

> [!IMPORTANT]
> TDE non fornisce funzionalità di crittografia tramite canali di comunicazione. Per altre informazioni su come crittografare i dati su diversi canali di comunicazione, vedere [Abilitare connessioni crittografate al motore di database (Gestione configurazione SQL Server)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).
>
>**Argomenti correlati:**
>
> - [Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)
> - [Introduzione a Transparent Data Encryption (TDE) in Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)
> - [Spostare un database protetto da TDE in un'altra istanza di SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)
> - [Abilitare TDE in SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)
> - [Usare Connettore SQL Server con le funzionalità di crittografia SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)
> - [Blog sulla sicurezza di SQL Server in TDE con domande frequenti](/archive/blogs/sqlsecurity/feature-spotlight-transparent-data-encryption-tde)

## <a name="about-tde"></a>Informazioni su TDE

La crittografia di un file di database viene eseguita a livello di pagina. Le pagine di un database crittografato vengono crittografate prima di essere scritte sul disco e decrittografate quando vengono lette in memoria. L'uso di TDE non comporta un aumento delle dimensioni del database crittografato.

### <a name="information-applicable-to-sssds"></a>Informazioni applicabili a [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]

Quando si usa TDE con [!INCLUDE[sqldbesa](../../../includes/sqldbesa-md.md)] V12 il certificato a livello di server archiviato nel database master viene creato automaticamente da [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. Per spostare un database TDE in [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], non è necessario decrittografare il database per l'operazione di spostamento. Per altre informazioni sull'uso di TDE con [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], vedere [(TDE) Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

### <a name="information-applicable-to-ssnoversion"></a>Informazioni applicabili a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

Dopo aver protetto un database, è possibile ripristinarlo usando il certificato corretto. Per altre informazioni sui certificati, vedere [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

Dopo l'abilitazione di TDE, eseguire immediatamente un backup del certificato e della chiave privata associata. Se il certificato risulta non più disponibile o se il database viene ripristinato o collegato in un altro server, saranno necessari i backup del certificato e della chiave privata. In caso contrario, non sarà possibile aprire il database.

Conservare il certificato di crittografia anche se si disabilita TDE nel database. Anche se il database non è crittografato, è possibile che parti del log delle transazioni rimangano protette. Il certificato potrebbe anche essere necessario per alcune operazioni fino a quando non si esegue un backup completo del database.

È comunque possibile usare un certificato oltre la data di scadenza per crittografare e decrittografare dati con TDE.

### <a name="encryption-hierarchy"></a>Gerarchia di crittografia

La figura seguente illustra l'architettura della crittografia TDE: Solo gli elementi a livello di database, la chiave di crittografia del database e le parti ALTER DATABASE sono configurabili dall'utente quando si usa TDE in [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].

![Architettura TDE (Transparent Database Encryption)](../../../relational-databases/security/encryption/media/tde-architecture.png)

## <a name="enable-tde"></a>Abilitare TDE

Per usare TDE, eseguire le operazioni seguenti:

**Si applica a**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

1. Creare una chiave master.

1. Creare o ottenere un certificato protetto dalla chiave master.

1. Creare una chiave di crittografia del database e proteggerla mediante il certificato.

1. Impostare il database per l'uso della crittografia.

L'esempio seguente illustra come crittografare e decrittografare il database `AdventureWorks2012` usando un certificato denominato `MyServerCert` installato nel server.

```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';
go
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';
go
USE AdventureWorks2012;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_128
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;
GO
ALTER DATABASE AdventureWorks2012
SET ENCRYPTION ON;
GO
```

Le operazioni di crittografia e decrittografia sono pianificate sui thread di background da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per visualizzare lo stato di queste operazioni, usare le viste del catalogo e le viste a gestione dinamica nella tabella riportata più avanti in questo argomento.

> [!CAUTION]
> Anche i file di backup per i database in cui è abilitata la funzionalità TDE vengono crittografati tramite la chiave di crittografia del database. Di conseguenza, quando questi backup vengono ripristinati, è necessario che sia disponibile il certificato che protegge la chiave di crittografia del database. Quindi, oltre a eseguire un backup del database, assicurarsi di conservare i backup dei certificati del server. Se il certificato non è più disponibile, si verificherà la perdita di dati.
>
> Per altre informazioni, vedere [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="commands-and-functions"></a>Comandi e funzioni

Affinché le istruzioni seguenti accettino certificati TDE, usare una chiave master del database per crittografarli. Se vengono crittografati solo tramite password, verranno rifiutati dalle istruzioni come componenti di crittografia.

> [!IMPORTANT]
> Se si proteggono i certificati con password dopo l'uso da parte di TDE, il database diventa inaccessibile dopo un riavvio.

Nella tabella seguente sono inclusi collegamenti e spiegazioni delle funzioni e dei comandi correlati a TDE:

|Comando o funzione|Scopo|
|-------------------------|-------------|
|[CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)|Crea una chiave per la crittografia di un database| 
|[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-database-encryption-key-transact-sql.md)|Modifica la chiave per la crittografia di un database|
|[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md)|Rimuove la chiave per la crittografia di un database|
|[Opzioni ALTER DATABASE SET (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql-set-options.md)|Descrive l'opzione **ALTER DATABASE** usata per abilitare TDE|

## <a name="catalog-views-and-dynamic-management-views"></a>Viste del catalogo e DMV

 Nella tabella seguente vengono illustrate le viste del catalogo e le viste a gestione dinamica di TDE.

|Vista del catalogo e vista a gestione dinamica|Scopo|
|---------------------------------------------|-------------|
|[sys.databases (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Vista del catalogo che consente di visualizzare le informazioni del database|
|[sys.certificates (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|Vista del catalogo che consente di visualizzare i certificati in un database|
|[sys.dm_database_encryption_keys (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)|DMV che fornisce informazioni sulle chiavi di crittografia e lo stato della crittografia di un database|

## <a name="permissions"></a>Autorizzazioni

Ogni funzionalità e comando di TDE ha requisiti specifici relativi alle autorizzazioni, descritti nelle tabelle precedenti.

La visualizzazione dei metadati interessati da TDE richiede l'autorizzazione VIEW DEFINITION per il certificato.

## <a name="considerations"></a>Considerazioni

Durante un'analisi di una nuova crittografia di un database, le operazioni di manutenzione per il database sono disabilitate. È possibile usare l'impostazione della modalità utente singolo per il database per eseguire operazioni di manutenzione. Per altre informazioni, vedere [Impostare un database in modalità utente singolo](../../../relational-databases/databases/set-a-database-to-single-user-mode.md).

Usare la DMV sys.dm_database_encryption_keys per determinare lo stato della crittografia del database. Per altre informazioni, vedere la sezione "Viste del catalogo e DMV" più indietro in questo argomento.

Con TDE vengono crittografati tutti i file e i filegroup in un database. Se un database include un filegroup contrassegnato come READ ONLY, l'operazione di crittografia del database ha esito negativo.

Se si usa un database per il mirroring del database o il log shipping, entrambi i database vengono crittografati. Le transazioni del log vengono crittografate quando vengono inviate tra i database.

> [!IMPORTANT]
> Gli indici full-text vengono crittografati quando un database viene impostato per la crittografia. Gli indici creati in una versione di SQL Server precedente a SQL Server 2008 vengono importati nel database da SQL Server 2008 o versioni successive e crittografati con TDE.

> [!TIP]
> Per monitorare le modifiche dello stato TDE di un database, usare SQL Server Audit o il servizio di controllo del database SQL. Per SQL Server, lo stato TDE è registrato nel gruppo di azioni di controllo DATABASE_CHANGE_GROUP, disponibile in [Azioni e gruppi di azioni di SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).

## <a name="restrictions"></a>Restrizioni

Durante le operazioni di crittografia del database iniziale, modifica della chiave o decrittografia del database, non sono consentite le operazioni seguenti:

- Eliminazione di un file da un filegroup in un database

- Rimozione di un database

- Attivazione della modalità offline per un database

- Scollegamento di un database

- Transizione di un database o filegroup in un stato READ ONLY

Le operazioni seguenti non sono consentite durante l'esecuzione delle istruzioni CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY e ALTER DATABASE...SET ENCRYPTION:

- Eliminazione di un file da un filegroup in un database

- Rimozione di un database

- Attivazione della modalità offline per un database

- Scollegamento di un database

- Transizione di un database o filegroup in un stato READ ONLY

- Uso di un comando ALTER DATABASE

- Avvio del backup di un database o di un file di database

- Avvio del ripristino di un database o di un file di database

- Creazione di uno snapshot

Le operazioni o condizioni seguenti impediscono l'esecuzione delle istruzioni CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY e ALTER DATABASE...SET ENCRYPTION:

- Un database è di sola lettura o contiene filegroup di sola lettura.

- È in corso l'esecuzione di un comando ALTER DATABASE.

- È in corso l'esecuzione di un backup dei dati.

- Un database è offline o è in corso di ripristino.

- Uno snapshot è in corso.

- Le attività di manutenzione del database sono in esecuzione.

Durante la creazione dei file di database, l'inizializzazione immediata dei file non è disponibile se è abilitata la crittografia TDE.

Per crittografare la chiave di crittografia del database con una chiave asimmetrica, è necessario che quest'ultima risieda in un provider EKM (Extensible Key Management).

## <a name="tde-scan"></a>Analisi TDE

Per abilitare TDE per un database, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve eseguire un'analisi della crittografia. L'analisi legge ogni pagina dai file di dati nel pool di buffer e quindi scrive le pagine crittografate di nuovo su disco.

Per avere un maggiore controllo sull'analisi della crittografia, [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] introduce l'analisi TDE, che include una sintassi per sospensione e ripresa. È possibile sospendere l'analisi mentre il carico di lavoro nel sistema è elevato o durante le ore di picco delle attività aziendali e quindi riprendere l'analisi in un secondo momento.

Per sospendere l'analisi della crittografia TDE, usare la sintassi seguente:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION SUSPEND;
```

In modo analogo, per sospendere l'analisi della crittografia TDE, usare la sintassi seguente:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION RESUME;
```

La colonna encryption_scan_state è stata aggiunta alla DMV sys.dm_database_encryption_keys. Indica lo stato corrente dell'analisi della crittografia. È disponibile anche una nuova colonna denominata encryption_scan_modify_date, che contiene la data e l'ora dell'ultima modifica dello stato di analisi della crittografia.

Se l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene riavviata durante la sospensione dell'analisi della crittografia, viene registrato un messaggio nel log degli errori all'avvio. Il messaggio indica che un'analisi esistente è stata sospesa.

## <a name="tde-and-transaction-logs"></a>TDE e log delle transazioni

Se si consente a un database di usare TDE, viene rimossa la parte rimanente del log delle transazioni virtuale corrente. La rimozione forza la creazione del log delle transazioni successivo. Questo comportamento garantisce che non rimanga alcun testo non crittografato nei log dopo l'impostazione del database per la crittografia.

Per determinare lo stato di crittografia dei file di log, vedere la colonna `encryption_state` nella vista `sys.dm_database_encryption_keys`, come in questo esempio:

```sql
USE AdventureWorks2012;
GO
/* The value 3 represents an encrypted state
   on the database and transaction logs. */
SELECT *
FROM sys.dm_database_encryption_keys
WHERE encryption_state = 3;
GO
```

Per altre informazioni sull'architettura del file di log di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Log delle transazioni (SQL Server)](../../../relational-databases/logs/the-transaction-log-sql-server.md).

Prima della modifica della chiave di crittografia del database, la chiave di crittografia del database precedente crittografa tutti i dati scritti nel log delle transazioni.

Se si modifica una chiave di crittografia del database due volte, è necessario eseguire un backup del log prima di poter modificare di nuovo la chiave di crittografia del database.

## <a name="tde-and-the-tempdb-system-database"></a>TDE e database di sistema tempdb

Il database di sistema **tempdb** viene crittografato se altri database nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono crittografati tramite TDE. La crittografia potrebbe influire sulle prestazioni dei database non crittografati presenti nella stessa istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per altre informazioni sul database di sistema **tempdb**, vedere [Database tempdb](../../../relational-databases/databases/tempdb-database.md).

## <a name="tde-and-replication"></a>TDE e replica

La replica non consente di replicare automaticamente dati da un database abilitato per TDE in un formato crittografato. Per proteggere i database di distribuzione e del Sottoscrittore, abilitare separatamente TDE.

La replica snapshot può archiviare dati in file intermedi non crittografati, come i file BCP, così come la distribuzione iniziale dei dati per la replica transazionale e di tipo merge. Durante questo tipo di replica, è possibile abilitare la crittografia per proteggere il canale di comunicazione.

Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database (Gestione configurazione SQL Server)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="tde-and-always-on"></a>TDE e Always On    
È possibile [aggiungere un database crittografato a un gruppo di disponibilità Always On](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md).  

Per crittografare i database che fanno parte di un gruppo di disponibilità, creare i certificati e la chiave master o la chiave asimmetrica (EKM) in tutte le repliche secondarie prima di creare la [chiave di crittografia del database](../../../t-sql/statements/create-database-encryption-key-transact-sql.md) nella replica primaria.  

Se un certificato viene usato per proteggere la chiave di crittografia del database (chiave DEK), [eseguire il backup del certificato](../../../t-sql/statements/backup-certificate-transact-sql.md) creato nella replica primaria e quindi [creare il certificato da un file](../../../t-sql/statements/create-certificate-transact-sql.md) in tutte le repliche secondarie prima di creare la chiave di crittografia del database nella replica primaria. 

## <a name="tde-and-filestream-data"></a>TDE e dati FILESTREAM

I dati **FILESTREAM** non vengono crittografati anche quando si abilita TDE.

<a name="scan-suspend-resume"></a>

## <a name="remove-tde"></a>Rimuovere TDE

Rimuovere la crittografia dal database usando l'istruzione `ALTER DATABASE`.

```sql
ALTER DATABASE <db_name> SET ENCRYPTION OFF;
```

Per visualizzare lo stato della crittografia del database, usare la DMV [sys.dm_database_encryption_keys](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

Attendere il completamento della decrittografia prima di rimuovere la chiave di crittografia del database usando [DROP DATABASE ENCRYPTION KEY](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md).

> [!IMPORTANT]
> Eseguire il backup della chiave master e del certificato usati per TDE in una posizione sicura. La chiave master e il certificato sono necessari per ripristinare i backup eseguiti quando il database era crittografato con TDE. Dopo aver rimosso la chiave di crittografia del database, eseguire un backup del log seguito da un nuovo backup completo del database decrittografato. 

## <a name="tde-and-buffer-pool-extension"></a>TDE ed estensione del pool di buffer

Quando si crittografa un database tramite TDE, i file correlati all'estensione del pool di buffer non vengono crittografati. Per questi file, usare strumenti di crittografia come BitLocker o EFS a livello di file system.

## <a name="tde-and-in-memory-oltp"></a>TDE e OLTP in memoria

È possibile abilitare TDE in un database contenente oggetti OLTP in memoria. In [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] e [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] i dati e i record del log OLTP in memoria vengono crittografati se si abilita TDE. In [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] i record del log OLTP in memoria vengono crittografati se si abilita TDE, ma non vengono crittografati i file nel filegroup MEMORY_OPTIMIZED_DATA.

## <a name="related-tasks"></a>Attività correlate

[Spostare un database protetto da TDE in un'altra istanza di SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
[Abilitare TDE in SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
[Extensible Key Management con Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  

## <a name="related-content"></a>Contenuti correlati

[Transparent Data Encryption con il database SQL di Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
[Introduzione a Transparent Data Encryption (TDE) in Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)  
[Crittografia di SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)  
[Chiavi di crittografia del database e di SQL Server (Motore di database)](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

## <a name="see-also"></a>Vedere anche

[Centro sicurezza per il motore di Database di SQL Server e il Database SQL di Azure](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[FILESTREAM (SQL Server)](../../../relational-databases/blob/filestream-sql-server.md)