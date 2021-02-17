---
description: Spostare database utente
title: Spostare database utente | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- editions [SQL Server], moving databases between
- moving full-text catalogs
- scheduled disk maintenace [SQL Server]
- moving databases
- full-text catalogs [SQL Server], moving
- moving database files
- moving user databases
- relocating database files
- planned database relocations [SQL Server]
- databases [SQL Server], moving
ms.assetid: ad9a4e92-13fb-457d-996a-66ffc2d55b79
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64b8416375cdc61c43e8799b33891096858a79f7
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636718"
---
# <a name="move-user-databases"></a>Spostare database utente
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile spostare i file di dati, di log e del catalogo full-text di un database utente specificando il nuovo percorso file nella clausola FILENAME dell'istruzione [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). Questo metodo è valido per lo spostamento dei file del database all'interno della stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per spostare un database in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in un altro server, usare le operazioni di [backup e ripristino](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) o di [collegamento e scollegamento](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md).  
  
## <a name="considerations"></a>Considerazioni  
 Quando si sposta un database in un'altra istanza del server, per garantire un sistema consistente a utenti e applicazioni, potrebbe essere necessario ricreare tutti i metadati del database o parte di essi. Per altre informazioni, vedere [Gestione dei metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
 Alcune funzionalità del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] comportano una modifica della modalità di archiviazione delle informazioni nei file di database da parte del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Queste funzionalità sono disponibili solo in edizioni specifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un database che contiene queste funzionalità non può essere spostato a un'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non le supporta. Utilizzare la vista a gestione dinamica sys.dm_db_persisted_sku_features per ottenere un elenco di tutte le caratteristiche specifiche dell'edizione abilitate nel database corrente.  
  
 Le procedure descritte in questo argomento richiedono il nome logico dei file di database. Per ottenere il nome, eseguire una query sulla colonna name della vista del catalogo [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) .  
  
 A partire da [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], i cataloghi full-text sono integrati nel database anziché essere archiviati nel file system. I cataloghi full-text vengono ora spostati automaticamente quando si sposta un database.  
  
## <a name="planned-relocation-procedure"></a>Procedura di rilocazione pianificata  
 Per spostare un file di dati o di log nell'ambito di una rilocazione pianificata, eseguire la procedura seguente:  
  
1.  Per ogni file che si desidera spostare, eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name, FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name SET OFFLINE;  
    ```  
  
3.  Spostare il file o i file nella nuova posizione.  
  
4.  Eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name SET ONLINE;  
    ```  
  
5.  Verificare la modifica ai file eseguendo la query riportata di seguito.  

    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="relocation-for-scheduled-disk-maintenance"></a>Rilocazione per una manutenzione pianificata del disco  
 Per rilocare un file nell'ambito di un processo di manutenzione pianificata del disco, eseguire la procedura seguente:  
  
1.  Per ogni file che si desidera spostare, eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o arrestare il sistema per eseguire la manutenzione. Per altre informazioni, vedere [Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Spostare il file o i file nella nuova posizione.  
  
4.  Riavviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il server. Per altre informazioni, vedere [Avviare, arrestare, sospendere, riprendere, riavviare il motore di database, SQL Server Agent o SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
5.  Verificare la modifica ai file eseguendo la query riportata di seguito.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="failure-recovery-procedure"></a>Procedura di recupero da errore  
 Se è necessario spostare un file a causa di un errore hardware, eseguire la procedura seguente per rilocare il file in una nuova posizione.  
  
> [!IMPORTANT]  
>  Se non è possibile avviare il database, ovvero se il database è in modalità sospetta o in stato non recuperato, il file può essere spostato solo dai membri del ruolo predefinito sysadmin.  
  
1.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se avviata.  
  
2.  Avviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità di recupero del solo database master digitando uno dei comandi seguenti al prompt dei comandi.  
  
    -   Per l'istanza predefinita (MSSQLSERVER), eseguire il comando seguente.  
  
        ```  
        NET START MSSQLSERVER /f /T3608  
        ```  
  
    -   Per un'istanza denominata, eseguire il comando riportato di seguito.  
  
        ```  
        NET START MSSQL$instancename /f /T3608  
        ```  
  
     Per altre informazioni, vedere [Avviare, arrestare, sospendere, riprendere, riavviare i servizi SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Per ogni file da spostare, usare i comandi **sqlcmd** oppure [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per eseguire l'istruzione seguente.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
     Per altre informazioni su come usare l'utilità **sqlcmd** , vedere [Usare l'utilità sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Uscire dall'utilità **sqlcmd** o [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Arrestare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
6.  Spostare il file o i file nella nuova posizione.  
  
7.  Avviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ad esempio, eseguire `NET START MSSQLSERVER`.  
  
8.  Verificare la modifica ai file eseguendo la query riportata di seguito.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente il file di log del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene spostato in una nuova posizione nell'ambito di una rilocazione pianificata.  
  
```  
USE master;  
GO  
-- Return the logical file name.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
GO  
ALTER DATABASE AdventureWorks2012 SET OFFLINE;  
GO  
-- Physically move the file to a new location.  
-- In the following statement, modify the path specified in FILENAME to  
-- the new location of the file on your server.  
ALTER DATABASE AdventureWorks2012   
    MODIFY FILE ( NAME = AdventureWorks2012_Log,   
                  FILENAME = 'C:\NewLoc\AdventureWorks2012_Log.ldf');  
GO  
ALTER DATABASE AdventureWorks2012 SET ONLINE;  
GO  
--Verify the new location.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Spostare i database di sistema](../../relational-databases/databases/move-system-databases.md)   
 [Spostare file del database](../../relational-databases/databases/move-database-files.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Avviare, arrestare, sospendere, riprendere, riavviare il motore di database, SQL Server Agent o SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
