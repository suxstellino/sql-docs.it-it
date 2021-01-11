---
title: 'Guida introduttiva: Backup e ripristino nel servizio Archiviazione BLOB di Azure'
description: 'Avvio rapido: informazioni su come scrivere backup nel servizio Archiviazione BLOB di Azure e su come eseguire il ripristino. Creare un contenitore BLOB di Azure, scrivere un backup e quindi eseguire il ripristino.'
ms.custom: seo-dt-2019
ms.date: 12/21/2020
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.reviewer: ''
ms.topic: quickstart
author: cawrites
ms.author: chadam
ms.openlocfilehash: d27f53f80c8f987106f90816a4566339d777e6f6
ms.sourcegitcommit: bb54e4c9dd8c97365b7a96dfcd557b8b86d06978
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2020
ms.locfileid: "97736899"
---
# <a name="quickstart-sql-backup-and-restore-to-azure-blob-storage-service"></a>Avvio rapido: Backup e ripristino SQL nel servizio Archiviazione BLOB di Azure

[!INCLUDE [sqlserver2016-asdbmi](../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questa guida di avvio rapido contiene nozioni utili sulla scrittura di backup nel servizio Archiviazione BLOB di Azure e sul ripristino dallo stesso.  L'articolo illustra come creare un contenitore BLOB di Azure, scrivere un backup nel servizio BLOB e quindi eseguire un ripristino.

> [!NOTE]
> SQL Server 2012 SP1 CU2 ha introdotto il supporto per il backup in Archiviazione BLOB di Azure. SQL Server 2014 e versioni precedenti non supportano la firma di accesso condiviso (SAS) descritta in questo articolo della guida di avvio rapido.
>
> Per SQL Server 2014 e versioni precedenti, usare [Esercitazione: Backup e ripristino di SQL Server 2014 nel servizio Archiviazione BLOB di Microsoft Azure](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url).
>
  
## <a name="prerequisites"></a>Prerequisiti

Per completare questa guida di avvio rapido è necessario conoscere i concetti di backup e ripristino di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e la sintassi T-SQL.  Sono necessari un account di archiviazione di Azure, SQL Server Management Studio (SSMS) e l'accesso a un server che esegue SQL Server o Istanza gestita di SQL di Azure. Inoltre, l'account utente usato per eseguire i comandi BACKUP e RESTORE deve essere incluso nel ruolo del database **db_backup operator** con autorizzazioni **Modifica qualsiasi credenziale**. 

- Ottenere un [account Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) gratuito.
- Creare un [account di archiviazione di Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Installare [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads) o distribuire [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance-get-started) con connettività stabilita tramite una [macchina virtuale SQL di Azure](/azure/sql-database/sql-database-managed-instance-configure-vm) o [da punto a sito](/azure/sql-database/sql-database-managed-instance-configure-p2s).
- Assegnare l'account utente al ruolo [db_backupoperator](./security/authentication-access/database-level-roles.md) e concedere le autorizzazioni [Modifica qualsiasi credenziale](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="create-azure-blob-container"></a>Creare un contenitore BLOB di Azure
Un contenitore fornisce un raggruppamento di un set di BLOB. Tutti i BLOB devono essere inclusi in un contenitore. Un account di archiviazione può contenere un numero illimitato di contenitori, tuttavia ne deve contenere almeno uno. In un contenitore è possibile archiviare un numero illimitato di BLOB. 

Per creare un contenitore, seguire questa procedura:

1. Aprire il portale di Azure. 
1. Passare all'account di archiviazione. 
1. Selezionare l'account di archiviazione e scorrere verso il basso fino a **Servizi BLOB**.
1. Selezionare **BLOB** e quindi selezionare **+ Contenitore** per aggiungere un nuovo contenitore. 
1. Immettere il nome del contenitore e prendere nota del nome specificato. Queste informazioni vengono usate nell'URL (percorso del file di backup) nelle istruzioni T-SQL più avanti in questa guida di avvio rapido. 
1. Selezionare **OK**. 
    
    ![Nuovo contenitore](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/new-container.png)

  > [!NOTE]
  > L'autenticazione per l'account di archiviazione è obbligatoria per il backup e il ripristino di SQL Server anche se si sceglie di creare un contenitore pubblico. Inoltre, è possibile creare un contenitore a livello di programmazione tramite le API REST. Per altre informazioni, vedere [Create container](/rest/api/storageservices/Create-Container) (Crea contenitore)

## <a name="create-a-test-database"></a>Creare un database di test 
In questo passaggio viene creato un database di test tramite SQL Server Management Studio (SSMS). 

1. Avviare [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) e connettersi all'istanza di SQL Server.
1. Aprire una finestra **Nuova query**. 
1. Eseguire il codice Transact-SQL (T-SQL) seguente per creare il database di test. Aggiornare il nodo **Database** in **Esplora oggetti** per vedere il nuovo database. Per i nuovi database creati in Istanza gestita di SQL la funzionalità Transparent Data Encryption viene abilitata automaticamente ed è quindi necessario disabilitarla per continuare. 

```sql
USE [master]
GO

-- Create database
CREATE DATABASE [SQLTestDB]
GO

-- Create table in database
USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table 
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

-- Disable TDE for newly-created databases on SQL Managed Instance 
USE [SQLTestDB];
GO
ALTER DATABASE [SQLTestDB] SET ENCRYPTION OFF;
GO
```

## <a name="create-credential"></a>Creare credenziali

Usare l'interfaccia utente grafica di SQL Server Management Studio per creare le credenziali con la procedura descritta di seguito. In alternativa, è possibile creare le credenziali [a livello di programmazione](tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature). 

1. Espandere il nodo **Database** all'interno di **Esplora oggetti** in [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Fare clic con il pulsante destro del mouse sul nuovo database `SQLTestDB`, passare il puntatore del mouse su **Attività** e quindi selezionare **Backup...** per avviare la procedura guidata **Backup database**. 
1. Selezionare l'**URL** dall'elenco a discesa di destinazione **Backup in** e quindi selezionare **Aggiungi** per aprire la finestra di dialogo **Seleziona destinazione di backup**. 

   ![URL di Backup in](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. Selezionare **Nuovo contenitore** nella finestra di dialogo **Seleziona destinazione di backup** per aprire la finestra **Connetti a una sottoscrizione Microsoft**. 

   ![Screenshot della finestra di dialogo Seleziona destinazione di backup con l'opzione Nuovo contenitore evidenziata.](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-backup-destination.png)

1. Accedere al portale di Azure selezionando **Accedi...**  e quindi eseguire la procedura di accesso. 
1. Selezionare la **sottoscrizione** dall'elenco a discesa. 
1. Selezionare l'**account di archiviazione** dall'elenco a discesa. 
1. Selezionare il contenitore creato in precedenza dall'elenco a discesa. 
1. Selezionare **Crea credenziali** per generare la *firma di accesso condiviso*.  **Salvare questo valore perché sarà necessario per il ripristino.**

   ![Creare credenziali](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/create-credential.png)

1. Selezionare **OK** per chiudere la finestra **Connetti a una sottoscrizione Microsoft**. Verrà popolato il valore *Contenitore di Archiviazione di Azure* nella finestra di dialogo **Seleziona destinazione di backup**. Selezionare **OK** per scegliere il contenitore di archiviazione desiderato e chiudere la finestra di dialogo. 
1. A questo punto, è possibile andare al passaggio 4 della sezione successiva per eseguire il backup del database o chiudere la procedura guidata **Backup database** per eseguire il backup del database tramite Transact-SQL. 


## <a name="back-up-database"></a>Eseguire il backup del database
In questo passaggio si eseguirà il backup del database `SQLTestDB` nell'account di Archiviazione BLOB di Azure usando l'interfaccia utente grafica all'interno di SQL Server Management Studio o Transact-SQL (T-SQL). 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Se la procedura guidata **Backup database** non è aperta, espandere il nodo **Database** all'interno di **Esplora oggetti** in [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Fare clic con il pulsante destro del mouse sul nuovo database `SQLTestDB`, passare il puntatore del mouse su **Attività** e quindi selezionare **Backup...** per avviare la procedura guidata **Backup database**. 
1. Selezionare l'**URL** dall'elenco a discesa **Backup in** e quindi selezionare **Aggiungi** per aprire la finestra di dialogo **Seleziona destinazione di backup**. 

   ![URL di Backup in](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. Selezionare il contenitore creato nel passaggio precedente nell'elenco a discesa **Contenitore di Archiviazione di Azure**. 

   ![Contenitore di archiviazione di Azure](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/azure-storage-container.png)

1. Selezionare **OK** nella procedura guidata **Backup database** per eseguire il backup del database. 
1. Al termine del backup selezionare **OK** per chiudere tutte le finestre correlate al backup. 

   > [!TIP]
   > È possibile creare uno script del codice Transact-SQL alla base di questo comando selezionando **Script** nella parte superiore della procedura guidata **Back up database**: ![Comando Script](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/script-backup-command.png)


# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Per eseguire il backup del database tramite Transact-SQL, eseguire il comando seguente: 


```sql
USE [master]

BACKUP DATABASE [SQLTestDB] 
TO  URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak' 
WITH  COPY_ONLY, CHECKSUM
GO
```

---

## <a name="delete-database"></a>Elimina database
In questo passaggio si eliminerà il database prima di eseguire il ripristino. Questo passaggio è necessario solo per gli scopi di questa esercitazione, ma è improbabile che venga usato nelle normali procedure di gestione del database. È possibile ignorare questo passaggio, ma in questo caso è necessario modificare il nome del database durante il ripristino in un'istanza gestita oppure eseguire il comando di ripristino `WITH REPLACE` per ripristinare correttamente il database in locale. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Espandere il nodo **Database** in **Esplora oggetti**, fare clic con il pulsante destro del mouse sul database `SQLTestDB` e selezionare Elimina per avviare la procedura guidata **Elimina oggetto**. 
1. In un'istanza gestita selezionare **OK** per eliminare il database. In locale selezionare la casella di controllo accanto a **Chiudi connessioni esistenti** e quindi selezionare **OK** per eliminare il database. 

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Per eliminare il database, eseguire il comando Transact-SQL seguente:

```sql
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO

-- If connections currently exist on-premises, you'll need to set the database into single user mode first
USE [master]
GO
ALTER DATABASE [SQLTestDB] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE
GO
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO
```

---


## <a name="restore-database"></a>Ripristina database 
In questo passaggio si ripristinerà il database usando l'interfaccia utente grafica in SQL Server Management Studio o tramite Transact-SQL. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Fare clic con il pulsante destro del mouse sul nodo **Database** in **Esplora oggetti** all'interno di SQL Server Management Studio e selezionare **Ripristina database**. 
1. Selezionare **Dispositivo** e quindi i puntini di sospensione (...) per scegliere il dispositivo. 

   ![Selezionare il dispositivo di ripristino](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-device.png)

1. Selezionare l'**URL** nell'elenco a discesa **Tipo di supporti di backup** e selezionare **Aggiungi** per aggiungere il dispositivo. 

   ![Aggiungere il dispositivo di backup](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/add-backup-device.png)

1. Selezionare il contenitore dall'elenco a discesa e quindi incollare la firma di accesso condiviso salvata durante la creazione delle credenziali. 

   ![Screenshot della finestra di dialogo Selezionare un percorso del file di backup con il campo Firma di accesso condiviso compilato.](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/restore-from-container.png)

1. Selezionare **OK** per selezionare il percorso del file di backup. 
1. Espandere **Contenitori** e selezionare il contenitore in cui si trova il file di backup. 
1. Selezionare il file di backup che si vuole ripristinare e quindi selezionare **OK**. Se non è visibile alcun file, è possibile che si stia usando una chiave di firma di accesso condiviso non corretta. È possibile rigenerare la chiave di firma di accesso condiviso seguendo la stessa procedura descritta in precedenza per aggiungere il contenitore. 

   ![Selezionare il file di ripristino](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-file.png)

1. Selezionare **OK** per chiudere la finestra di dialogo **Seleziona dispositivi di backup**.
1. Selezionare **OK** per ripristinare il database.

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Per ripristinare il database locale da Archiviazione BLOB di Azure, modificare il comando Transact-SQL seguente in modo da usare il proprio account di archiviazione e quindi eseguirlo in una nuova finestra di query.

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] FROM 
URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak'
```

---


## <a name="see-also"></a>Vedere anche 
Di seguito sono indicate alcune letture suggerite per comprendere i concetti e le procedure consigliate quando si usa il servizio Archiviazione BLOB di Azure per i backup di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)   
-   [Procedure consigliate e risoluzione dei problemi per il backup di SQL Server nell'URL](../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)  
