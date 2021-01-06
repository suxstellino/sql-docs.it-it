---
description: 'Esercitazione: Uso del servizio di archiviazione BLOB di Azure con SQL Server 2016'
title: 'Esercitazione: Uso del servizio di archiviazione BLOB di Azure con SQL Server 2016'
ms.custom: seo-dt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 31407e6d056ce8ec847f52346b8aa3ddf8e621f3
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644187"
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>Esercitazione: Uso del servizio di archiviazione BLOB di Azure con SQL Server 2016

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Benvenuti nell'esercitazione sull'uso di SQL Server 2016 nel servizio di archiviazione BLOB di Microsoft Azure. Questa esercitazione descrive come usare il servizio di archiviazione BLOB di Microsoft Azure per i file di dati e i backup di SQL Server.  
  
Il supporto di integrazione di SQL Server per il servizio di archiviazione BLOB di Microsoft Azure è stato per la prima volta incluso come funzionalità avanzata di SQL Server 2012 Service Pack 1 CU2, ulteriormente potenziato in SQL Server 2014 e SQL Server 2016. Per una panoramica delle funzionalità e dei vantaggi offerti dall'uso di questa funzione, vedere [File di dati di SQL Server in Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). Per una dimostrazione dal vivo, vedere la [demo del ripristino temporizzato](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo).  

Nell'esercitazione viene illustrato come usare i file di dati di SQL Server nel servizio di archiviazione BLOB di Microsoft Azure in più sezioni. Ogni sezione è incentrata su un'attività specifica e le sezioni devono essere completate in sequenza. In primo luogo, si apprenderà come creare un nuovo contenitore nell'archiviazione BLOB con criteri di accesso archiviati e una firma di accesso condiviso. Successivamente, verrà illustrato come creare le credenziali di SQL Server per poter integrare SQL Server con l'archivio BLOB di Azure. Verrà quindi eseguito il backup di un database nel servizio di archiviazione BLOB e ne verrà eseguito il ripristino in una macchina virtuale di Azure. Verrà quindi usato il backup del log delle transazioni di snapshot di file di SQL Server 2016 per il ripristino temporizzato e in un nuovo database. Infine, l'esercitazione descriverà l'uso di stored procedure e funzioni di sistema dei metadati per consentire di comprendere e usare i backup di snapshot di file.
  
## <a name="prerequisites"></a>Prerequisiti

Per completare l'esercitazione è necessario conoscere i concetti di backup e ripristino di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e la sintassi T-SQL. Per usare questa esercitazione, sono necessari un account di archiviazione di Azure, SQL Server Management Studio (SSMS), l'accesso a un'istanza di SQL Server locale, l'accesso a una macchina virtuale di Azure che esegue SQL Server 2016 e un database AdventureWorks2016. Inoltre, l'account utente usato per eseguire i comandi BACKUP e RESTORE deve essere incluso nel ruolo del database **db_backup operator** con autorizzazioni **Modifica qualsiasi credenziale**. 

- Ottenere un [account Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) gratuito.
- Creare un [account di archiviazione di Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Effettuare il provisioning di una [macchina virtuale di Azure che esegue SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)
- Installare [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Scaricare i [database di esempio AdventureWorks2016](../samples/adventureworks-install-configure.md).
- Assegnare l'account utente al ruolo [db_backupoperator](./security/authentication-access/database-level-roles.md) e concedere le autorizzazioni [Modifica qualsiasi credenziale](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="1---create-stored-access-policy-and-shared-access-storage"></a>1: Creare criteri di accesso archiviati e l'archivio di accesso condiviso

In questa lezione si userà uno script di [Azure PowerShell](/powershell/azure/) per creare una firma di accesso condiviso in un contenitore BLOB di Azure tramite criteri di accesso archiviati.  
  
> [!NOTE]  
> Questo script è stato scritto con Azure PowerShell 5.0.10586.  
  
Una firma di accesso condivisa è un URI che concede diritti di accesso limitati a contenitori, BLOB, code o tabelle. I criteri di accesso archiviati garantiscono un livello di controllo aggiuntivo sulle firme di accesso condivise sul lato server, ad esempio revoca, scadenza ed estensione dei diritti di accesso. Per usare questi nuovi miglioramenti, è necessario creare in un contenitore criteri con almeno diritti di lettura, scrittura ed elenco.  
  
È possibile creare criteri di accesso archiviati e una firma di accesso condiviso usando Azure PowerShell, Azure Storage SDK, l'API REST di Azure o un'utilità di terze parti. Questa esercitazione illustra come usare uno script Azure PowerShell per completare questa attività. Lo script usa il modello di distribuzione Resource Manager e crea le nuove risorse seguenti  
  
-   Resource group   
-   Account di archiviazione  
-   Contenitore BLOB di Azure   
-   Criteri di firma di accesso condiviso    

Questo script inizia con la dichiarazione di variabili con le quali specificare i nomi per le risorse indicate sopra e i nomi dei valori di input necessari seguenti:  
  
-   Nome di prefisso usato nei nomi di altri oggetti risorsa    
-   Nome sottoscrizione    
-   Ubicazione data center  

  
Lo script viene completato generando l'istruzione CREATE CREDENTIAL appropriata che sarà usata in [2: Creare credenziali di SQL Server usando una firma di accesso condiviso](#2---create-a-sql-server-credential-using-a-shared-access-signature). Questa istruzione viene copiata negli Appunti e viene visualizzata nella console.  
  
Per creare i criteri per il contenitore e generare una chiave di firma di accesso condiviso, seguire questa procedura:  
  
1.  Aprire Windows PowerShell o Windows PowerShell ISE (vedere i requisiti della versione precedenti).  
  
2.  Modificare e quindi eseguire lo script sottostante:  
  
    ```powershell
    # Define global variables for the script  
    $prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
    $subscriptionID = '<your subscription ID>'   # the ID  of subscription name you will use  
    $locationName = '<a data center location>'  # the data center region you will use  
    $storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
    $containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = $prefixName + 'policy' # the name of the SAS policy 

    # Set a variable for the name of the resource group you will create or use  
    $resourceGroupName=$prefixName + 'rg'   
  
    # Add an authenticated Azure account for use in the session   
    Connect-AzAccount    
  
    # Set the tenant, subscription and environment for use in the rest of   
    Set-AzContext -SubscriptionId $subscriptionID   
  
    # Create a new resource group - comment out this line to use an existing resource group  
    New-AzResourceGroup -Name $resourceGroupName -Location $locationName   
  
    # Create a new Azure Resource Manager storage account - comment out this line to use an existing Azure Resource Manager storage account  
    New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   
  
    # Get the access keys for the Azure Resource Manager storage account  
    $accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  
  
    # Create a new storage account context using an Azure Resource Manager storage account  
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].Value

    # Creates a new container in blob storage  
    $container = New-AzStorageContainer -Context $storageContext -Name $containerName  
  
    # Sets up a Stored Access Policy and a Shared Access Signature for the new container  
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -StartTime $(Get-Date).ToUniversalTime().AddMinutes(-5) -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission rwld

    # Gets the Shared Access Signature for the policy  
    $sas = New-AzStorageContainerSASToken -name $containerName -Policy $policyName -Context $storageContext
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

    # Sets the variables for the new container you just created
    $container = Get-AzStorageContainer -Context $storageContext -Name $containerName
    $cbc = $container.CloudBlobContainer 
  
    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
    $tSql | clip  
    Write-Host $tSql 

    # Once you're done with the tutorial, remove the resource group to clean up the resources. 
    # Remove-AzResourceGroup -Name $resourceGroupName  
    ```  

3.  Dopo aver completato lo script, l'istruzione CREATE CREDENTIAL sarà aggiunta agli Appunti perché sia possibile usarla nella sezione successiva.  


## <a name="2---create-a-sql-server-credential-using-a-shared-access-signature"></a>2: Creare credenziali di SQL Server usando una firma di accesso condiviso

In questa sezione verranno create credenziali per archiviare le informazioni sulla sicurezza che saranno poi usate da SQL Server per scrivere e leggere dal contenitore di Azure creato nel passaggio precedente.  
  
Una credenziale di SQL Server è un oggetto utilizzato per archiviare le informazioni di autenticazione necessarie per connettersi a una risorsa all'esterno di SQL Server. Nelle credenziali vengono archiviati il percorso URI del contenitore di archiviazione e i valori della firma di accesso condivisa per questo contenitore.  

Per creare credenziali di SQL Server, seguire questa procedura:  
  
1.  Connettersi a SQL Server Management Studio.  
2.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server del motore di database nell'ambiente locale.  
  
3.  Nella nuova finestra query incollare l'istruzione CREATE CREDENTIAL con la firma di accesso condiviso illustrata nella sezione 1 ed eseguire lo script.  
  
    Lo script sarà simile al codice seguente.  
  
    ```sql   
    /* Example:
    USE master  
    CREATE CREDENTIAL [https://msfttutorial.blob.core.windows.net/containername] 
    WITH IDENTITY='SHARED ACCESS SIGNATURE'   
    , SECRET = 'sharedaccesssignature' 
    GO */
    
    USE master  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      -- this name must match the container path, start with https and must not contain a forward slash at the end
    WITH IDENTITY='SHARED ACCESS SIGNATURE' 
      -- this is a mandatory string and should not be changed   
     , SECRET = 'sharedaccesssignature' 
       -- this is the shared access signature key that you obtained in section 1.   
    GO    
    ```  
  
4.  Per visualizzare tutte le credenziali disponibili, è possibile eseguire l'istruzione seguente nella finestra di query collegata all'istanza:  
  
    ```sql  
    SELECT * from sys.credentials  
    ```  
  
5.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server del motore di database nella macchina virtuale di Azure.  
  
6.  Nella nuova finestra query incollare l'istruzione CREATE CREDENTIAL con la firma di accesso condiviso illustrata nella sezione 1 ed eseguire lo script.  
  
7.  Ripetere i passaggi 5 e 6 per tutte le istanze di SQL Server aggiuntive che devono avere accesso al contenitore di Azure.  

## <a name="3---database-backup-to-url"></a>3: Backup del database su URL

In questa sezione si esegue un backup del database AdventureWorks2016 presente nell'istanza di SQL Server 2016 nel contenitore di Azure creato nella [sezione 1](#1---create-stored-access-policy-and-shared-access-storage).
  
> [!NOTE]  
> Se si vuole eseguire il backup di un database di SQL Server 2012 SP1 CU2 o versione successiva o di un database di SQL Server 2014 nel contenitore di Azure, è possibile usare la sintassi deprecata illustrata [qui](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2014&preserve-view=true) per eseguire il backup nell'URL usando la sintassi con credenziali.  
  
Per eseguire il backup di un database nell'archiviazione BLOB, seguire questi passaggi:  
  
1.  Connettersi a SQL Server Management Studio.  
2.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server 2016 del motore di database nella macchina virtuale di Azure.  
3.  Copiare e incollare lo script Transact-SQL seguente nella finestra della query. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1 e quindi eseguire questo script.  
  
    ```sql  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2016  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2016 database to the container that you created in section 1  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'  
  
    ```  
  
4.  Aprire Esplora oggetti e connettersi all'archiviazione di Azure usando l'account di archiviazione e la chiave dell'account. 
    1.  Espandere i contenitori, espandere il contenitore creato nella sezione 1 e verificare che il file di backup usato nel passaggio 3 sia visualizzato in questo contenitore.  
  
   ![Connettersi all'account di archiviazione di Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/connect-to-azure-storage.png)


## <a name="4----restore-database-to-virtual-machine-from-url"></a>4: Ripristinare il database in una macchina virtuale da un URL

In questa sezione il database AdventureWorks2016 viene ripristinato nell'istanza di SQL Server 2016 nella macchina virtuale di Azure.
  
> [!NOTE]  
> Ai fini della semplicità dell'esercitazione, viene usato lo stesso contenitore per i file di dati e di log usato per il backup del database. In un ambiente di produzione probabilmente si usano più contenitori e spesso anche più file di dati. Con SQL Server 2016 si può anche considerare lo striping del backup in più BLOB per migliorare le prestazioni del backup quando si esegue il backup di un database di grandi dimensioni.  
  
Per ripristinare il database AdventureWorks2016 dall'archivio BLOB di Azure nell'istanza di SQL Server 2016 nella macchina virtuale di Azure, seguire questa procedura:  
  
1.  Connettersi a SQL Server Management Studio.   
2.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server 2016 del motore di database nella macchina virtuale di Azure.   
3.  Copiare e incollare lo script Transact-SQL seguente nella finestra della query. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1 e quindi eseguire questo script.  
  
    ```sql  
    -- Restore AdventureWorks2016 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Data.mdf'  
         ,MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  Aprire Esplora oggetti e connettersi all'istanza di Azure SQL Server 2016.   
5.  In Esplora oggetti espandere il nodo Database e verificare che il database AdventureWorks2016 sia stato ripristinato, aggiornando il nodo se necessario.    
    1. Fare clic con il pulsante destro del mouse su AdventureWorks2016 e scegliere Proprietà.
    1. Selezionare File e verificare che i percorsi dei due file di database siano URL che puntano ai BLOB nel contenitore BLOB di Azure. Al termine, selezionare Annulla.
    
     ![Database AdventureWorks nella macchina virtuale di Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/adventureworks-on-azure-vm.png)

    
8.  In Esplora oggetti connettersi all'archiviazione di Azure.
    1. Espandere il contenitore creato nella sezione 1 e verificare che i file AdventureWorks2016_Data.mdf e AdventureWorks2016_Log.ldf del passaggio 3 appaiano nel contenitore, insieme ai file di backup dalla sezione 3. Aggiornare il nodo in base alle esigenze.  
  
   ![File di dati all'interno del contenitore in Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/data-files-in-container.png)

## <a name="5---backup-database-using-file-snapshot-backup"></a>5: Eseguire il backup del database con il backup di snapshot di file

In questa sezione si eseguirà il backup del database AdventureWorks2016 nella macchina virtuale di Azure usando il backup di snapshot di file per eseguire un backup quasi istantaneo con gli snapshot di Azure. Per altre informazioni sui backup di snapshot, vedere [Backup di snapshot di file per i file di database in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
  
Per eseguire il backup del database AdventureWorks2016 usando un backup di snapshot, eseguire queste operazioni:  
  
1.  Connettersi a SQL Server Management Studio.   
2.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server 2016 del motore di database nella macchina virtuale di Azure.  
3.  Copiare, incollare ed eseguire lo script Transact-SQL seguente nella finestra di query. Non chiudere questa finestra di query, lo script verrà eseguito di nuovo nel passaggio 5. Questa store procedure di sistema consente di visualizzare i backup di snapshot esistenti per ogni file che include un database specificato. Si noterà che non esistono backup di snapshot per questo database.  
  
    ```sql  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
    ```  
  
4.  Copiare e incollare lo script Transact-SQL seguente nella finestra della query. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1 e quindi eseguire questo script. Si noti la rapidità con cui viene eseguito il backup.  
  
    ```sql  
    -- Backup the AdventureWorks2016 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Azure.bak'   
       WITH FILE_SNAPSHOT;    
    ```  
  
5.  Dopo aver verificato che lo script del passaggio 4 è stato eseguito correttamente, eseguire di nuovo lo script seguente. Si noti che l'operazione di backup di snapshot di file del passaggio 4 ha generato snapshot sia per il file di dati che per il file di log.  
  
    ```sql  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
  
    ```  
  
    ![Risultati di fn_db_backup_file_snapshots con gli snapshot visualizzati](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-showing-snapshot.png) 
  
6.  In Object Explorer, nell'istanza di SQL Server 2016 nella macchina virtuale di Azure, espandere il nodo Databases e verificare che il database AdventureWorks2016 sia stato ripristinato in questa istanza. Aggiornare il nodo in base alle esigenze.  
7.  In Esplora oggetti connettersi all'archiviazione di Azure.   
8.  Espandere il contenitore creato nella sezione 1 e verificare che AdventureWorks2016_Azure.bak del passaggio 4 appaia nel contenitore, insieme ai file di backup dalla sezione 3 e i file di database della sezione 4. Aggiornare il nodo in base alle esigenze.  
  
    ![Backup di snapshot in Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-backup-on-azure.PNG)

## <a name="6----generate-activity-and-backup-log-using-file-snapshot-backup"></a>6: Generare attività e backup del log usando il backup di snapshot di file

In questa sezione viene generata un'attività nel database AdventureWorks2016 e periodicamente vengono eseguiti i backup del log delle transazioni tramite backup di snapshot di file. Per altre informazioni sui backup di snapshot di file, vedere [Backup di snapshot di file per i file di database in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Per generare un'attività nel database AdventureWorks2016 e creare periodicamente backup del log delle transazioni tramite backup di snapshot di file, seguire questa procedura:  
  
1.  Connettersi a SQL Server Management Studio.   
2.  Aprire due nuove finestra di query e connettere ogni finestra all'istanza di SQL Server 2016 del motore di database nella macchina virtuale di Azure.   
3.  Copiare, incollare ed eseguire lo script Transact-SQL seguente in una delle finestre di query. Si noti che, prima di aggiungere nuove righe nel passaggio 4, la tabella Production.Location contiene 14 righe.  
  
    ```sql 
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location;    
    ```  
  
4.  Copiare e incollare gli script Transact-SQL seguenti nelle due diverse finestre di query. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1 ed eseguire questi script contemporaneamente in due finestre di query distinte. Dopo 7 minuti circa questi script saranno completi.  
  
    ```sql  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2016.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;    
    ```  
  
    ```sql  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2016.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2016 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  Esaminare l'output del primo script. Si noti che il numero di righe finale è ora 29.939.  
  
    ![Numero di righe pari a 29.939 visualizzato](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png)
  
6.  Esaminare l'output del secondo script. Si noti che ogni volta che si esegue l'istruzione BACKUP LOG vengono creati due nuovi snapshot di file, vale a dire uno snapshot del file di log e uno del file di dati, per un totale di due snapshot per ogni file di database. Dopo aver completato il secondo script, si noti che gli snapshot di file sono in totale 16, 8 per ogni file di database, uno proveniente dall'istruzione BACKUP DATABASE e uno per ogni esecuzione dell'istruzione BACKUP LOG.  
  
   ![Risultati dello snapshot di backup](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-back-results.png)
  
7.  In Esplora oggetti connettersi all'archiviazione di Azure.  
  
8.  Espandere Contenitori, espandere il contenitore creato nella sezione 1, verificare che i sette nuovi file di backup siano visualizzati nel contenitore con i file di dati delle sezioni precedenti. Se necessario aggiornare il nodo.  
  
    ![Snapshot multipli nel contenitore di Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/tutorial-snapshots-in-container.png)

## <a name="7---restore-a-database-to-a-point-in-time"></a>7: Ripristinare un database fino a un punto nel tempo

In questa sezione il database AdventureWorks2016 verrà ripristinato in base a un dato momento tra due backup del log delle transazioni.  
  
Con i backup tradizionali, per eseguire il ripristino temporizzato, è necessario usare il backup completo del database, ad esempio un backup differenziale, e tutti i file di log delle transazioni fino a e subito dopo il momento in base al quale si esegue il ripristino. Per i backup di snapshot di file sono necessari solo i due file di backup del log che rappresentano i due limiti del periodo di tempo a cui si riferisce il ripristino. Sono necessari due soli set di backup di snapshot di file di log poiché ogni backup del log crea uno snapshot di ogni file del database (file di dati e file di log).  
  
Per ripristinare un database in base a un momento specifico dal set di backup di snapshot di file, seguire questa procedura:  
  
1.  Connettersi a SQL Server Management Studio.  
2.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server 2016 del motore di database nella macchina virtuale di Azure.   
3.  Copiare, incollare ed eseguire lo script Transact-SQL seguente nella finestra della query. Verificare che la tabella Production.Location contenga 29.939 righe prima di eseguire il ripristino temporizzato quando sono presenti meno righe nel passaggio 5.  
  
    ```sql
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location   
    ```  
  
    ![Numero di righe pari a 29.939 visualizzato](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png) 
  
4.  Copiare e incollare lo script Transact-SQL seguente nella finestra della query. Selezionare due file di backup del log adiacenti e convertire il nome del file nella data e nell'ora necessarie per questo script. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1, specificare i nomi del primo e del secondo file di backup, indicare il tempo STOPAT nel formato "June 26, 2018 01:48 PM" e quindi eseguire lo script. Il completamento di questo script richiederà alcuni minuti  
  
    ```sql  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2016 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2018 01:48 PM';  
    ALTER DATABASE AdventureWorks2016 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2016.Production.Location ;
    ```  
  
5.  Esaminare l'output. Si noti che dopo il ripristino il conteggio delle righe è 18.389, il numero di righe tra i backup del log 5 e 6 (il numero di righe conteggiato può variare).  
  
    ![18-thousand-rows.JPG](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/18-thousand-rows.png)

## <a name="8----restore-as-new-database-from-log-backup"></a>8: Ripristinare come nuovo database dal backup del log

In questa sezione il database AdventureWorks2016 verrà ripristinato come nuovo database dal backup del log delle transazioni dello snapshot di file.  
  
In questo scenario viene eseguito un ripristino in un'istanza di SQL Server in una macchina virtuale diversa per l'analisi business e la creazione di report. Il ripristino in un'altra istanza in una macchina virtuale diversa ripartisce il carico di lavoro su una macchina virtuale dedicata con dimensioni adatte allo scopo eliminando le richieste di risorse dal sistema transazionale.  
  
Il ripristino da un backup del log delle transazioni con backup di snapshot di file è un'operazione rapida, molto più veloce dei backup di flusso tradizionali. Con i backup di flusso tradizionali, è necessario usare il backup di database completo, probabilmente un backup differenziale e alcuni o tutti i backup del log delle transazioni oppure un nuovo backup di database completo. Con i backup del log dello snapshot di file, invece, è necessario solo il backup del log più recente oppure qualsiasi altro backup del log o due backup del log adiacenti per il ripristino temporizzato in un momento tra due orari di backup del log. Per maggiore chiarezza, è necessario un solo set di backup di snapshot di file di log poiché ogni backup del log di snapshot di file crea uno snapshot di ogni file del database (file di dati e file di log).  
  
Per ripristinare un database in un nuovo database da un backup del log delle transazioni usando il backup di snapshot di file, eseguire i passaggi seguenti:  
  
1.  Connettersi a SQL Server Management Studio.   
2.  Aprire una nuova finestra di query e connettersi all'istanza di SQL Server 2016 del motore di database in una macchina virtuale di Azure.  
  
    > [!NOTE]  
    > Se si tratta di una macchina virtuale di Azure diversa da quella usata per le sezioni precedenti, assicurarsi di aver eseguito i passaggi descritti in [2: Creare una credenziale di SQL Server usando una firma di accesso condiviso](#2---create-a-sql-server-credential-using-a-shared-access-signature). Per eseguire il ripristino in un contenitore diverso, seguire i passaggi descritti in [1: Creare criteri di accesso archiviati e l'archivio di accesso condiviso](#1---create-stored-access-policy-and-shared-access-storage) per il nuovo contenitore.  
  
3.  Copiare e incollare lo script Transact-SQL seguente nella finestra della query. Selezionare il file di backup del log che si vuole usare. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1, specificare il nome del file di backup del log e quindi eseguire questo script.  
  
    ```sql  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2016_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak>'    
        WITH MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE   
    ```  
  
4.  Rivedere l'output per verificare il completamento del ripristino.   
5.  In Esplora oggetti connettersi all'archiviazione di Azure.    
6.  Espandere Contenitori, espandere il contenitore creato nella sezione 1, aggiornare se necessario e verificare che i nuovi file di dati e di log siano visualizzati nel contenitore con i BLOB delle sezioni precedenti.  
  
    ![Contenitore di Azure con i file di log e di dati per il nuovo database](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/new-db-in-azure-container.png)

## <a name="9---manage-backup-sets-and-file-snapshot-backups"></a>9: Gestire set di backup e backup di snapshot di file

In questa sezione verrà eliminato un set di backup tramite la stored procedure di sistema [sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md). Questa stored procedure di sistema elimina il file di backup e lo snapshot del file in ogni file di database associato a questo set di backup.  
  
> [!NOTE]  
> Se si prova a eliminare un set di backup semplicemente eliminando il file di backup dal contenitore BLOB di Azure, verrà rimosso solo file di backup, mentre gli snapshot dei file rimarranno. Se ci si ritrova in questo scenario, usare la funzione di sistema [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) per identificare l'URL degli snapshot di file orfani e usare la stored procedure di sistema [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) per eliminare ogni snapshot di file orfano. Per altre informazioni, vedere  [Backup di snapshot di file per i file di database in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Per eliminare un set di backup di snapshot di file, seguire questi passaggi:  
  
1.  Connettersi a SQL Server Management Studio.  
2.  Aprire una nuova finestra Query e connettersi all'istanza di SQL Server 2016 del motore di database nella macchina virtuale di Azure (o a qualsiasi istanza di SQL Server 2016 con autorizzazioni di lettura e scrittura sul contenitore).   
3.  Copiare e incollare lo script Transact-SQL seguente nella finestra della query. Selezionare il backup del log che si vuole eliminare insieme ai relativi snapshot di file associati. Modificare l'URL in modo appropriato per il nome di account di archiviazione e il contenitore specificato nella sezione 1, specificare il nome del file di backup del log e quindi eseguire questo script.  
  
  ```sql
  sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-21764-20181003205236.bak';  
  ```  
  
4.  In Esplora oggetti connettersi all'archiviazione di Azure.  
5.  Espandere i contenitori, espandere il contenitore creato nella sezione 1 e verificare che il file di backup usato nel passaggio 3 non sia più visualizzato in questo contenitore. Aggiornare il nodo se necessario.  
  
    ![Eliminazione del BLOB di backup del log dal contenitore di Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/deleted-backup-snapshot.png)
  
6.  Copiare, incollare ed eseguire lo script Transact-SQL seguente nella finestra della query per verificare che i due snapshot di file siano stati eliminati.  
  
    ```sql  
    -- verify that two file snapshots have been removed  
    SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');   
    ```  
    ![Riquadro dei risultati con i 2 snapshot di file eliminati](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-of-two-deleted-snapshot-files.png)

## <a name="10---remove-resources"></a>10: Rimuovere le risorse

Dopo aver completato questa esercitazione, per risparmiare risorse, assicurarsi di eliminare il gruppo di risorse creato in questa esercitazione. 

Per eliminare il gruppo di risorse, eseguire il codice PowerShell seguente:

  ```powershell
  # Define global variables for the script  
  $prefixName = '<prefix name>'  # should be the same as the beginning of the tutorial
  
  # Set a variable for the name of the resource group you will create or use  
  $resourceGroupName=$prefixName + 'rg'   
  
  # Adds an authenticated Azure account for use in the session   
  Connect-AzAccount    
  
  # Set the tenant, subscription and environment for use in the rest of   
  Set-AzContext -SubscriptionId $subscriptionID    
    
  # Remove the resource group
  Remove-AzResourceGroup -Name $resourceGroupName   
  ```


  
## <a name="see-also"></a>Vedere anche

[File di dati di SQL Server in Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Backup di snapshot di file per i file di database in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[Backup di SQL Server nell'URL](../relational-databases/backup-restore/sql-server-backup-to-url.md) 
[Firme di accesso condiviso, Parte 1: Informazioni sul modello di firma di accesso condiviso](/azure/storage/common/storage-sas-overview)  
[Create Container](/rest/api/storageservices/Create-Container)  
[Set Container ACL](/rest/api/storageservices/Set-Container-ACL)  
[Get Container ACL](/rest/api/storageservices/Get-Container-ACL)
[Credenziali &#40;motore di database&#41;](../relational-databases/security/authentication-access/credentials-database-engine.md)  
[CREATE CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-credential-transact-sql.md)  
[sys.credentials &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
[sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
[sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
[sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) [Backup di snapshot di file per i file di database in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)