---
title: Creare un backup crittografato | Microsoft Docs
description: Questo articolo illustra come creare un backup crittografato in SQL Server usando Transact-SQL. È possibile eseguire il backup su disco o in Archiviazione di Azure.
ms.custom: ''
ms.date: 08/04/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: e29061d3-c2ab-4d98-b9be-8e90a11d17fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: b02f55100110952bce5785130ba330b5c96bce81
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130444"
---
# <a name="create-an-encrypted-backup"></a>Creare un backup crittografato
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento vengono descritti i passaggi necessari per creare un backup crittografato tramite Transact-SQL.  Per un esempio dell'uso di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vedere [Creazione di un backup completo del database (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md). 
  
## <a name="backup-to-disk-with-encryption"></a>Backup su disco con crittografia  
 **Prerequisiti:**  
  
-   Accesso a un disco locale o a uno spazio di archiviazione con una quantità di spazio adeguata per creare un backup del database.  
  
-   Chiave master di un database master e certificato o chiave asimmetrica disponibile nell'istanza di SQL Server. Per le autorizzazioni e i requisiti di crittografia, vedere [Crittografia dei backup](../../relational-databases/backup-restore/backup-encryption.md).  
  
 Utilizzare i passaggi seguenti per creare un backup crittografato di un database in un disco locale. In questo esempio viene utilizzato un database utente denominato MyTestDB.  
  
1.  **Creare una chiave master del database del database master:** Scegliere una password per la crittografia della copia della chiave master che verrà archiviata nel database. Connettersi al motore di database, avviare una nuova finestra Query e copiare e incollare l'esempio seguente, quindi fare clic su **Esegui**.  
  
    ```  
    -- Creates a database master key.   
    -- The key is encrypted using the password "<master key password>"  
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
  
    ```  
  
2.  **Creare un certificato di backup:** creare un certificato di backup nel database master. Copiare e incollare l'esempio seguente nella finestra Query e quindi fare clic su **Esegui**.  
  
    ```  
    Use Master  
    GO  
    CREATE CERTIFICATE MyTestDBBackupEncryptCert  
       WITH SUBJECT = 'MyTestDB Backup Encryption Certificate';  
    GO  
  
    ```  
  
3.  **Eseguire il backup del database:** specificare l'algoritmo di crittografia e il certificato da usare. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  

    ```
    BACKUP DATABASE [MyTestDB]  
    TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
    WITH  
      COMPRESSION,  
      ENCRYPTION   
       (  
       ALGORITHM = AES_256,  
       SERVER CERTIFICATE = MyTestDBBackupEncryptCert  
       ),  
      STATS = 10  
    GO
    ```  
  
 Per un esempio di crittografia di un backup protetto da Extensible Key Management, vedere [Extensible Key Management tramite l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
### <a name="backup-to-azure-storage-with-encryption"></a>Backup nel servizio Archiviazione di Azure con crittografia  
 Se si crea un backup nell'archiviazione di Azure usando l'opzione **SQL Server Backup to URL** ( Backup di SQL Server nell'URL), i passaggi di crittografia sono gli stessi, ma è necessario usare un URL come destinazione e le credenziali di SQL per autenticare l'archiviazione di Azure. Se si desidera configurare [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] con le opzioni di crittografia, vedere [Abilitare il backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md).  
  
 **Prerequisiti:**  
  
-   Account di archiviazione di Windows con contenitore. Per altre informazioni, vedere [Lezione 1: Creare oggetti di Archiviazione di Azure](/previous-versions/sql/sql-server-2016/jj720557(v=sql.130)).  
  
-   Chiave master di un database master e certificato o chiave asimmetrica nell'istanza di SQL Server. Per le autorizzazioni e i requisiti di crittografia, vedere [Crittografia dei backup](../../relational-databases/backup-restore/backup-encryption.md).  
  
1.  **Creare credenziali di SQL Server:** per creare le credenziali di SQL Server, connettersi al motore di database, aprire una nuova finestra Query, copiare e incollare l'esempio seguente e quindi fare clic su **Esegui**.  
  
    ```  
    CREATE CREDENTIAL mycredential   
    WITH IDENTITY= 'mystorageaccount' - this is the name of the storage account you specified when creating a storage account    
    , SECRET = '<storage account access key>' - this should be either the Primary or Secondary Access Key for the storage account  
    ```  
  
2.  **Creare una chiave master del database:** Scegliere una password per la crittografia della copia della chiave master che verrà archiviata nel database. Connettersi al motore di database, avviare una nuova finestra Query e copiare e incollare l'esempio seguente, quindi fare clic su **Esegui**.  
  
    ```  
    -- Creates a database master key.  
    -- The key is encrypted using the password "<master key password>"  
    USE Master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
  
    ```  
  
3.  **Creare un certificato di backup:** creare un certificato di backup nel database master. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**  
  
    ```  
    USE Master;  
    GO  
    CREATE CERTIFICATE MyTestDBBackupEncryptCert  
       WITH SUBJECT = 'MyTestDBBackupEncryptCert ';  
    GO  
  
    ```  
  
4.  **Eseguire il backup del database:** specificare l'algoritmo di crittografia e il certificato da usare. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    BACKUP DATABASE [MyTestDB]  
    TO URL = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
    WITH  
      CREDENTIAL 'mycredential' - this is the name of the credential created in the first step.  
      ,COMPRESSION  
      ,ENCRYPTION   
       (  
       ALGORITHM = AES_256,  
       SERVER CERTIFICATE = MyTestDBBackupEncryptCert  
       ),  
      STATS = 10  
    GO  
  
    ```  
  
