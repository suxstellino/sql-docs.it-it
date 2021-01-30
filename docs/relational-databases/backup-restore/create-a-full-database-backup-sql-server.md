---
title: Creare un backup completo del database | Microsoft Docs
description: Questo articolo illustra come creare un backup completo del database in SQL Server usando SQL Server Management Studio, Transact-SQL o PowerShell.
ms.custom: sqlfreshmay19
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up databases [SQL Server], full backups
- backing up databases [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- database backups [SQL Server], SQL Server Management Studio
ms.assetid: 586561fc-dfbb-4842-84f8-204a9100a534
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3eb0b33f2891c8f48d63b4446d31c1918fcb7aab
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076113"
---
# <a name="create-a-full-database-backup"></a>Creazione di un backup completo del database

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento viene descritto come creare un backup completo del database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o PowerShell.

Per informazioni sul backup di SQL Server con il servizio Archiviazione BLOB di Azure, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) e [Backup di SQL Server nell'URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni

- Non è possibile usare l'istruzione `BACKUP` in una transazione esplicita o implicita.
- I backup creati nella versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non possono essere ripristinati nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Per una panoramica approfondita dei concetti e delle attività di backup, vedere [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md) prima di procedere.

## <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni

- Con l'aumento delle dimensioni del database, i backup completi del database richiedono più tempo e più spazio di archiviazione. Per database di grandi dimensioni, valutare la possibilità di integrare un backup completo del database con una serie di [backup di database differenziali](../../relational-databases/backup-restore/differential-backups-sql-server.md).
- Stimare la dimensione di un backup del database completo tramite la stored procedure di sistema [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) .
- Per impostazione predefinita, per ogni operazione di backup eseguita in modo corretto viene aggiunta una voce al log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e al registro eventi di sistema. Se si eseguono frequenti backup, questi messaggi verranno accumulati, creando log di errori enormi. Ciò può rendere difficile l'individuazione di altri messaggi. In questo caso è possibile eliminare le voci di log di backup usando il flag di traccia 3226 se nessuno degli script dipende da esse. Per altre informazioni, vedere [Flag di traccia &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="security"></a><a name="Security"></a> Sicurezza

**TRUSTWORTHY** è impostato su OFF in un backup del database. Per informazioni su come impostare **TRUSTWORTHY** su ON, vedere [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).

A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le opzioni **PASSWORD** e **MEDIAPASSWORD** non sono più disponibili per la creazione di backup. È possibile ripristinare backup creati con password.

## <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

Le autorizzazioni `BACKUP DATABASE` e `BACKUP LOG` vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** e dei ruoli predefiniti del database **db_owner** e **db_backupoperator**.

 Eventuali problemi correlati alla proprietà e alle autorizzazioni sul file fisico del dispositivo di backup possono interferire con l'operazione di backup. Il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere in grado di leggere e scrivere nel dispositivo, il che significa che l'account con cui viene eseguito il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve avere le autorizzazioni di scrittura per il dispositivo di backup. Le autorizzazioni di accesso ai file, tuttavia, non vengono controllate dalla stored procedure [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)che aggiunge una voce per un dispositivo di backup nelle tabelle di sistema. Di conseguenza, i problemi relativi al file fisico del dispositivo di backup potrebbero emergere solo in fase di accesso alla risorsa fisica durante un tentativo di backup o ripristino.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio

> [!NOTE]
> Quando si specifica un'attività di backup usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], è possibile generare lo script [BACKUP](../../t-sql/statements/backup-transact-sql.md) corrispondente di [!INCLUDE[tsql](../../includes/tsql-md.md)] facendo clic sul pulsante **Script** e selezionando una destinazione per lo script.

1. Dopo essersi connessi all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di [!INCLUDE[msCoName](../../includes/msconame-md.md)], in **Esplora oggetti** espandere l'albero del server.

1. Espandere **Database** e selezionare un database utente o espandere **Database di sistema** e selezionare un database di sistema.

1. Fare clic con il pulsante destro del mouse sul database di cui eseguire il backup, scegliere **Attività** e quindi fare clic su **Backup**.

1. Nella finestra di dialogo **Backup database** il database selezionato viene visualizzato nell'elenco a discesa, che può essere sostituito con qualsiasi altro database nel server.

1. Nell'elenco a discesa **Tipo di backup** selezionare il tipo di backup desiderato. Il valore predefinito è **Completo**.

   > [!IMPORTANT]
   > Prima di poter eseguire un backup differenziale o del log delle transazioni, è necessario eseguire almeno un backup completo del database.
   
1. In **Componente di cui eseguire il backup** selezionare **Database**.

1. Nella sezione **Destinazione** esaminare il percorso predefinito per il file di backup (nella cartella ../mssql/data).

   Per eseguire il backup in un dispositivo diverso, modificare la selezione usando l'elenco a discesa **Backup su**. Per eseguire lo striping del set di backup tra più file per aumentare la velocità di backup, fare clic su **Aggiungi** per aggiungere altri oggetti e/o destinazioni di backup.
 
   Per rimuovere una destinazione di backup, selezionarla e fare clic su **Rimuovi**. Per visualizzare il contenuto di una destinazione di backup, selezionarla e fare clic su **Contenuto**.

1. Rivedere le altre impostazioni disponibili nelle pagine **Opzioni supporti** e **Opzioni di backup** (facoltativo).

   Per altre informazioni sulle varie opzioni di backup, vedere le pagine [Generale](back-up-database-general-page.md), [Opzioni supporti](back-up-database-media-options-page.md) e [Opzioni di backup](back-up-database-backup-options-page.md).

1. Fare clic su **OK** per avviare il backup.

1. Quando il backup viene completato correttamente, fare clic su **OK** per chiudere la finestra di dialogo SQL Server Management Studio.

### <a name="additional-information"></a>Informazioni aggiuntive

- Dopo aver creato un backup completo del database, è possibile creare un [backup differenziale del database](create-a-differential-database-backup-sql-server.md) o un [backup del log delle transazioni](back-up-a-transaction-log-sql-server.md).

- È possibile selezionare la casella di controllo **Backup di sola copia** per creare un backup di sola copia (facoltativo). Un *backup di sola copia* è un backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indipendente dalla sequenza di backup convenzionali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Backup di sola copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md). Per il tipo di backup **Differenziale** non è possibile creare un backup di sola copia.

- L'opzione **Sovrascrivi supporti** è disabilitata nella pagina **Opzioni supporti** se si esegue il backup su un URL.

### <a name="examples"></a>Esempi

Per gli esempi seguenti, creare un database di test con il codice Transact-SQL seguente:

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest
   (
      ID INT NOT NULL PRIMARY KEY,
      c1 VARCHAR(100) NOT NULL,
      dt1 DATETIME NOT NULL DEFAULT getdate()
   );
GO

USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```

#### <a name="a-full-back-up-to-disk-to-default-location"></a>R. Eseguire il backup completo su disco nel percorso predefinito

In questo esempio verrà eseguito il backup su disco del database `SQLTestDB` nel percorso di backup predefinito.

1. Dopo essersi connessi all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di [!INCLUDE[msCoName](../../includes/msconame-md.md)], in **Esplora oggetti** espandere l'albero del server.

1. Espandere i **database**, fare clic con il pulsante destro del mouse su `SQLTestDB`, scegliere **Attività**, quindi fare clic su **Backup...** .

1. Fare clic su **OK**.

1. Quando il backup viene completato correttamente, fare clic su **OK** per chiudere la finestra di dialogo SQL Server Management Studio.

![Eseguire il backup SQL](media/quickstart-backup-restore-database/backup-db-ssms.png)

#### <a name="b-full-back-up-to-disk-to-non-default-location"></a>B. Eseguire il backup completo su disco in un percorso non predefinito

In questo esempio verrà eseguito il backup su disco del database `SQLTestDB` nel percorso prescelto.

1. Dopo essersi connessi all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di [!INCLUDE[msCoName](../../includes/msconame-md.md)], in **Esplora oggetti** espandere l'albero del server.

1. Espandere i **database**, fare clic con il pulsante destro del mouse su `SQLTestDB`, scegliere **Attività**, quindi fare clic su **Backup...** .

1. Nella sezione **Destinazione** della pagina **Generale** selezionare **Disco** dall'elenco a discesa **Backup su:** .

1. Selezionare **Rimuovi** finché non vengono rimossi tutti i file di backup esistenti.

1. Selezionare **Aggiungi**. Verrà aperta la finestra di dialogo **Selezionare la destinazione di backup**.

1. Immettere un percorso e un nome file validi nella casella di testo **Nome file** e usare **.bak** come estensione per semplificare la classificazione del file.

1. Fare clic su **OK** e quindi fare di nuovo clic su **OK** per avviare il backup.

1. Quando il backup viene completato correttamente, fare clic su **OK** per chiudere la finestra di dialogo SQL Server Management Studio.

![Modificare il percorso del database](media/create-a-full-database-backup-sql-server/change-db-location.png)

#### <a name="c-create-an-encrypted-backup"></a>C. Creare un backup crittografato

In questo esempio verrà eseguito il backup con crittografia del database `SQLTestDB` nel percorso di backup predefinito.

1. Dopo essersi connessi all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di [!INCLUDE[msCoName](../../includes/msconame-md.md)], in **Esplora oggetti** espandere l'albero del server.

1. Espandere **Database**, espandere **Database di sistema**, fare clic con il pulsante destro del mouse su `master` e scegliere **Nuova query** per aprire una finestra di query con una connessione al database `SQLTestDB`.

1. Eseguire i comandi seguenti per creare una [**chiave master del database**](../../relational-databases/security/encryption/create-a-database-master-key.md) e un [**certificato**](../../t-sql/statements/create-certificate-transact-sql.md) nel database `master`.  

   ```sql
   -- Create the master key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   -- If the master key already exists, open it in the same session that you create the certificate (see next step)
   OPEN MASTER KEY DECRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe'

   -- Create the certificate encrypted by the master key
   CREATE CERTIFICATE MyCertificate
   WITH SUBJECT = 'Backup Cert', EXPIRY_DATE = '20201031';  
   ```

1. In **Esplora oggetti**, nel nodo **Database**, fare clic con il pulsante destro del mouse su `SQLTestDB`, scegliere **Attività** e quindi fare clic su **Backup**.

1. Nella sezione **Sovrascrivi supporti** della pagina **Opzioni supporti** selezionare **Esegui backup di un nuovo set di supporti e cancella tutti i set di backup esistenti**.

1. Nella sezione **Crittografia** della pagina **Opzioni di backup** selezionare la casella di controllo **Crittografa backup** .

1. Nell'elenco a discesa Algoritmo selezionare **AES 256**.

1. Nell'elenco a discesa **Certificato o chiave asimmetrica** selezionare `MyCertificate`.

1. Selezionare **OK**.

![Backup crittografato](media/create-a-full-database-backup-sql-server/encrypted-backup.png)

#### <a name="d-back-up-to-the-azure-blob-storage-service"></a>D. Backup nel servizio Archiviazione BLOB di Azure

L'esempio esegue un backup completo del database di `SQLTestDB` nel servizio Archiviazione BLOB di Azure. Questo esempio presuppone che sia già disponibile un account di archiviazione con un contenitore BLOB. Questo esempio crea automaticamente una firma di accesso condiviso. L'esecuzione dell'esempio non riesce se il contenitore ha una firma di accesso condiviso esistente.

Se non è disponibile un contenitore BLOB di Azure in un account di archiviazione, crearne uno prima di continuare. Per altre informazioni, vedere [Creare un account di archiviazione per utilizzo generico](/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) e [Creare un contenitore](/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container).

1. Dopo essersi connessi all'istanza appropriata del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di [!INCLUDE[msCoName](../../includes/msconame-md.md)], in **Esplora oggetti** espandere l'albero del server.

1. Espandere i **database**, fare clic con il pulsante destro del mouse su `SQLTestDB`, scegliere **Attività**, quindi fare clic su **Backup...** .

1. Nella pagina **Generale** nella sezione **Destinazione** selezionare l' **URL** dall'elenco a discesa **Backup in:** .

1. Fare clic su **Aggiungi** e verrà aperta la finestra di dialogo **Selezionare la destinazione di backup** .

1. Se in precedenza è stato registrato il contenitore di archiviazione di Azure che si vuole usare con SQL Server Management Studio, selezionarlo. In caso contrario, fare clic su **Nuovo contenitore** per registrare un nuovo contenitore.

1. Nella finestra di dialogo **Connetti a una sottoscrizione Microsoft** accedere al proprio account.

1. Nella casella di testo dell'elenco a discesa **Seleziona account di archiviazione** selezionare l'account di archiviazione.

1. Nella casella di testo dell'elenco a discesa **Seleziona contenitore BLOB** selezionare il contenitore BLOB.

1. Nella casella del calendario dell'elenco a discesa **Scadenza criteri di accesso condiviso** selezionare una data di scadenza per i criteri di accesso condiviso creati in questo esempio.

1. Fare clic su **Crea credenziali** per generare una firma di accesso condiviso e le credenziali in SQL Server Management Studio.

1. Fare clic su **OK** per chiudere la finestra di dialogo **Connetti a una sottoscrizione Microsoft**.

1. Nella casella di testo **File di backup** modificare il nome del file di backup (facoltativo).

1. Fare clic su **OK** per chiudere la finestra di dialogo **Seleziona la destinazione di backup**.

1. Fare clic su **OK** per avviare il backup.

1. Quando il backup viene completato correttamente, fare clic su **OK** per chiudere la finestra di dialogo SQL Server Management Studio.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL

Creare un backup completo del database eseguendo l'istruzione `BACKUP DATABASE` per creare il backup completo del database, specificando:

- Il nome del database di cui eseguire il backup.
- Il dispositivo di backup in cui archiviare il backup completo del database.

La sintassi di base dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] per un backup completo del database è la seguente:

 BACKUP DATABASE *database* TO *dispositivo_backup* [ **,** ...*n* ] [ WITH *opzioni_with* [ **,** ...*o* ] ] ;

|Opzione|Descrizione|
|------------|-----------------|
|*database*|Corrisponde al database di cui eseguire il backup.|
|*backup_device* [ **,** ...*n* ]|Specifica un elenco di dispositivi di backup da 1 a 64 da utilizzare per l'operazione di backup. È possibile specificare un dispositivo di backup fisico oppure un dispositivo di backup logico corrispondente se è già stata definito. Per specificare un dispositivo di backup fisico, utilizzare l'opzione DISK o TAPE:<br /><br /> { DISK &#124; TAPE } **=** _nome\_dispositivo\_fisico\_backup_<br /><br /> Per altre informazioni, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|
|WITH *con_opzioni* [ **,** ...*o* ]|Facoltativamente, specifica una o più opzioni aggiuntive, *o*. Per informazioni su alcune opzioni WITH di base, vedere il passaggio 2.|
|||

Facoltativamente, specificare una o più opzioni **WITH**. Alcune opzioni **WITH** di base sono descritte di seguito. Per informazioni su tutte le opzioni **WITH**, vedere [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).

Opzioni **WITH** del set di backup di base:

- **{ COMPRESSION | NO_COMPRESSION }** : Solo in [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] e versioni successive, specifica se la [compressione backup](../../relational-databases/backup-restore/backup-compression-sql-server.md) è eseguita su questo backup, ignorando l'impostazione predefinita a livello di server.
- **ENCRYPTION (ALGORITHM, SERVER CERTIFICATE | ASYMMETRIC KEY)** : Solo in SQL Server 2014 o versioni successive specificare l'algoritmo di crittografia da utilizzare e il certificato o la chiave asimmetrica da utilizzare per proteggere la crittografia.
- **DESCRIPTION** **=** { **'** _text_ **'**  |  **@** _text\_variable_ }: Specifica il testo in formato libero che descrive il set di backup. La stringa può essere composta da un massimo di 255 caratteri.
- **NAME = { *backup_set_name* |  **@** _backup\_set\_name\_var_ }** : Specifica il nome del set di backup. I nomi possono essere composti da un massimo di 128 caratteri. Se si omette NAME, al set di backup non viene assegnato alcun nome specifico.

Per impostazione predefinita, `BACKUP` accoda il backup a un set di supporti esistente, conservando i set di backup esistenti. Per specificarlo in modo esplicito, usare l'opzione `NOINIT`. Per informazioni sull'accodamento a set di backup esistenti, vedere [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

In alternativa, usare l'opzione **FORMAT** per formattare i supporti di backup:

 FORMAT [ **,** MEDIANAME **=** { *nome_supporto* |  **@** _variabile\_nome\_supporto_ } ] [ **,** MEDIADESCRIPTION **=** { *testo* |  **@** _variabile\_testo_ } ]

 Usare la clausola **FORMAT**, se i supporti vengono usati per la prima volta o si vogliono sovrascrivere tutti i dati esistenti. Facoltativamente, assegnare al nuovo supporto un nome e una descrizione.

 > [!IMPORTANT]
 > Usare la clausola **FORMAT** dell'istruzione `BACKUP` con estrema cautela, in quanto entrambe comportano la cancellazione di eventuali backup archiviati in precedenza nei supporti di backup.

### <a name="examples"></a><a name="TsqlExample"></a> Esempi

Per gli esempi seguenti, creare un database di test con il codice Transact-SQL seguente:

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
   ID INT NOT NULL PRIMARY KEY,
   c1 VARCHAR(100) NOT NULL,
   dt1 DATETIME NOT NULL DEFAULT GETDATE()
)
GO

USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```

#### <a name="a-back-up-to-a-disk-device"></a>R. Backup su un dispositivo disco

Nell'esempio riportato di seguito viene eseguito il backup su disco del database `SQLTestDB` completo, utilizzando `FORMAT` per creare un nuovo set di supporti.

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
TO DISK = 'c:\tmp\SQLTestDB.bak'
   WITH FORMAT,
      MEDIANAME = 'SQLServerBackups',
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="b-back-up-to-a-tape-device"></a>B. Backup su un dispositivo nastro

 Nell'esempio seguente viene eseguito il backup completo su nastro del database `SQLTestDB` , accodandolo ai backup precedenti.

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO TAPE = '\\.\Tape0'
   WITH NOINIT,
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="c-back-up-to-a-logical-tape-device"></a>C. Backup su un dispositivo nastro logico

Nell'esempio seguente viene creato in un dispositivo di backup logico per un'unità nastro. Nell'esempio viene quindi eseguito il backup completo del database SQLTestDB su quel dispositivo.

```sql
-- Create a logical backup device,
-- SQLTestDB_Bak_Tape, for tape device \\.\tape0.
USE master;
GO
EXEC sp_addumpdevice 'tape', 'SQLTestDB_Bak_Tape', '\\.\tape0'; USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO SQLTestDB_Bak_Tape
   WITH FORMAT,
      MEDIANAME = 'SQLTestDB_Bak_Tape',
      MEDIADESCRIPTION = '\\.\tape0',
      NAME = 'Full Backup of SQLTestDB';
GO
```

## <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell

Usare il cmdlet **Backup-SqlDatabase** . Per indicare in modo esplicito che si tratta di un backup completo del database, specificare il parametro **-BackupAction** con il relativo valore predefinito **Database**. Questo parametro è facoltativo per i backup completi di database.

> [!NOTE]
> Questi esempi richiedono il modulo SqlServer. Per determinare se è installato, eseguire `Get-Module -Name SqlServer`. Per installare questo modulo, eseguire `Install-Module -Name SqlServer` in una sessione di amministrazione di PowerShell.
>
> Per altre informazioni, vedere [Provider PowerShell per SQL Server](../../powershell/sql-server-powershell-provider.md).

> [!IMPORTANT]
> Se si apre una finestra di PowerShell da SQL Server Management Studio per connettersi a un'installazione di SQL Server, è possibile omettere la parte relativa alle credenziali di questo esempio perché vengono usate automaticamente le credenziali in SSMS per stabilire la connessione tra PowerShell e l'istanza di SQL Server.

### <a name="examples"></a>Esempi

#### <a name="a-full-backup-local"></a>R. Backup completo (locale)

L'esempio seguente consente di creare un backup di database completo del database di `<myDatabase>` nel percorso di backup predefinito dell'istanza del server `Computer\Instance`. Facoltativamente, questo esempio specifica **-BackupAction Database**.

Per la sintassi completa e altri esempi, vedere [Backup-SqlDatabase](/powershell/module/sqlserver/backup-sqldatabase).

```powershell
$credential = Get-Credential

Backup-SqlDatabase -ServerInstance Computer[\Instance] -Database <myDatabase> -BackupAction Database -Credential $credential
```

#### <a name="b-full-backup-to-azure"></a>B. Backup completo in Azure

Nell'esempio seguente viene creato un backup completo del database `<myDatabase>` sull'istanza `<myServer>` per il servizio Archiviazione BLOB di Azure. Sono stati creati i criteri di accesso archiviati con diritti di lettura, scrittura ed elenco. Le credenziali di SQL Server, `https://<myStorageAccount>.blob.core.windows.net/<myContainer>`, sono stati creati usando una firma di accesso condiviso associata a criteri di accesso archiviati. Il comando di PowerShell usa il parametro **BackupFile** per specificare il percorso (URL) e il nome del file di backup.

```powershell
$credential = Get-Credential
$container = 'https://<myStorageAccount>blob.core.windows.net/<myContainer>'
$fileName = '<myDatabase>.bak'
$server = '<myServer>'
$database = '<myDatabase>'
$backupFile = $container + '/' + $fileName

Backup-SqlDatabase -ServerInstance $server -Database $database -BackupFile $backupFile -Credential $credential
```

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks

- [Eseguire il backup di un database (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
- [Creare un backup differenziale del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)
- [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)
- [Ripristinare un backup del database nel modello di recupero con registrazione minima &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)
- [Ripristinare un database fino al punto di errore nel modello di recupero con registrazione completa &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)
- [Ripristinare un database in una nuova posizione &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)
- [Utilizzare la Creazione guidata piano di manutenzione](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)

## <a name="see-also"></a>Vedere anche

- [Risoluzione dei problemi di backup in SQL Server e operazioni di ripristino](https://support.microsoft.com/kb/224071)
- [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)
- [Backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)
- [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)
- [Backup database &#40;pagina Generale&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)
- [Backup database &#40;pagina Opzioni di backup&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)
- [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)
- [Backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)