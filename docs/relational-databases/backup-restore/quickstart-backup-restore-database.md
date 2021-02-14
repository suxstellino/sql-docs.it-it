---
title: 'Avvio rapido: Eseguire il backup e il ripristino del database'
titleSuffix: SQL Server
description: In questo articolo viene illustrato come creare un nuovo database, eseguire un backup del database e ripristinare il backup in SQL Server.
author: cawrites
ms.author: chadam
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.assetid: ''
ms.openlocfilehash: 66b4277674c1144a716e5153a9fa062f988c75c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340211"
---
# <a name="quickstart-backup-and-restore-a-sql-server-database-on-premises"></a>Avvio rapido: Eseguire il backup e il ripristino di un database di SQL Server in locale
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questa guida di avvio rapido si procederà alla creazione di un nuovo database, all'esecuzione di un semplice backup e quindi al ripristino. 

Per una procedura più dettagliata, vedere [Creare un backup completo del database](create-a-full-database-backup-sql-server.md) e [Ripristinare un backup tramite SSMS](restore-a-database-backup-using-ssms.md).

## <a name="prerequisites"></a>Prerequisiti
Per completare questa guida di avvio rapido sono necessari: 

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
- [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

## <a name="create-a-test-database"></a>Creare un database di test 

1. Avviare [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) e connettersi all'istanza di SQL Server.
1. Aprire una finestra **Nuova query**. 
1. Eseguire il codice Transact-SQL (T-SQL) seguente per creare il database di test. Aggiornare il nodo **Database** in **Esplora oggetti** per vedere il nuovo database. 

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
    dt1 DATETIME NOT NULL DEFAULT getdate()
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
 
## <a name="take-a-backup"></a>Eseguire un backup
Per eseguire il backup del database, seguire questa procedura: 

1. Avviare [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) e connettersi all'istanza di SQL Server.
1. Espandere il nodo **Database** in **Esplora oggetti**.  
1. Fare clic con il pulsante destro del mouse sul database, passare il puntatore su **Attività** e selezionare **Backup...** . 
1. In **Destinazione** verificare che il percorso per il backup sia corretto. Per modificarlo, selezionare **Rimuovi** per rimuovere il percorso esistente e quindi **Aggiungi** per digitare un nuovo percorso. È possibile usare i puntini di sospensione per passare a un file specifico. 
1. Selezionare **OK** eseguire il backup del database. 

![Eseguire il backup SQL](media/quickstart-backup-restore-database/backup-db-ssms.png)

In alternativa è possibile eseguire il comando Transact-SQL seguente per eseguire il backup del database: 

```sql
BACKUP DATABASE [SQLTestDB] 
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' 
WITH NOFORMAT, NOINIT,  
NAME = N'SQLTestDB-Full Database Backup', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
GO
```


## <a name="restore-a-backup"></a>Ripristinare un backup
Per ripristinare il database, seguire questa procedura: 

1. Avviare [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) e connettersi all'istanza di SQL Server.
1. Fare clic con il pulsante destro del mouse sul nodo **Database** in **Esplora oggetti** e selezionare **Ripristina database...** .

    ![Ripristinare un database](media/quickstart-backup-restore-database/restore-db-ssms1.png)

1. Selezionare **Dispositivo:** e quindi i puntini di sospensione (...) per individuare il file di backup. 
1. Selezionare **Aggiungi** e passare alla posizione in cui si trova il file `.bak`. Selezionare il file `.bak` e quindi **OK**. 
1. Selezionare **OK** per chiudere la finestra di dialogo **Seleziona dispositivi di backup**. 
1. Selezionare **OK** per ripristinare il backup del database. 

    ![Ripristinare il database](media/quickstart-backup-restore-database/restore-db-ssms2.png)

In alternativa è possibile eseguire lo script Transact-SQL seguente per ripristinare il database:

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] 
FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

### <a name="clean-up-resources"></a>Pulire le risorse
Eseguire il comando Transact-SQL seguente per rimuovere il database creato, insieme alla relativa cronologia di backup nel database MSDB:

```sql
EXEC msdb.dbo.sp_delete_database_backuphistory @database_name = N'SQLTestDB'
GO

USE [master]
DROP DATABASE [SQLTestDB]
GO
```

## <a name="see-more"></a>Altre informazioni
[Panoramica di backup e ripristino](back-up-and-restore-of-sql-server-databases.md)
[Eseguire il backup in un URL](sql-server-backup-to-url.md)
[Creare un backup completo](create-a-full-database-backup-sql-server.md)
[Ripristinare un backup del database](restore-a-database-backup-using-ssms.md)
