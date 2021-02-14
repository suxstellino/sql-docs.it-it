---
title: Ripristinare un database di SQL Server in Docker
description: Questa esercitazione illustra come ripristinare un backup del database di SQL Server in un nuovo contenitore Docker per Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 1bb137243e5a297baae55e72afeaca356e8f4d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347354"
---
# <a name="restore-a-sql-server-database-in-a-linux-docker-container"></a>Ripristinare un database di SQL Server in un contenitore Docker per Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Questa esercitazione illustra come spostare e ripristinare un file di backup di SQL Server in un'immagine del contenitore di SQL Server 2017 per Linux in esecuzione in Docker.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Questa esercitazione illustra come spostare e ripristinare un file di backup di SQL Server in un'immagine del contenitore di SQL Server 2019 su Linux in esecuzione in Docker.

::: moniker-end

> [!div class="checklist"]
> * Effettuare il pull ed eseguire l'immagine del contenitore di SQL Server per Linux più recente.
> * Copiare il file di database Wide World Importers nel contenitore.
> * Ripristinare il database nel contenitore.
> * Eseguire istruzioni Transact-SQL per visualizzare e modificare il database.
> * Eseguire il backup del database modificato.

## <a name="prerequisites"></a>Prerequisites

* Motore Docker 1.8 o versione successiva in qualsiasi distribuzione di Linux supportata oppure Docker per Mac/Windows. Per altre informazioni, vedere [Installare Docker](https://docs.docker.com/engine/installation/).
* Almeno 2 GB di spazio su disco.
* Almeno 2 GB di RAM.
* [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

## <a name="pull-and-run-the-container-image"></a>Effettuare il pull ed eseguire l'immagine del contenitore

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Aprire un terminale Bash in Linux/Mac o una sessione di PowerShell con privilegi elevati in Windows.

1. Eseguire il pull dell'immagine del contenitore di SQL Server 2017 su Linux dall'hub Docker.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   > [!TIP]
   > In questa esercitazione vengono forniti esempi di comandi di Docker sia per la shell Bash (Linux/Mac) che per PowerShell (Windows).

1. Per eseguire l'immagine del contenitore con Docker, è possibile usare il comando seguente:

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   Questo comando crea un contenitore di SQL Server 2017 con l'edizione Developer (impostazione predefinita). La porta **1433** di SQL Server è esposta nell'host come porta **1401**. Il parametro facoltativo `-v sql1data:/var/opt/mssql` crea un contenitore del volume di dati denominato **sql1ddata**, che viene usato per salvare in modo permanente i dati creati da SQL Server.

   > [!IMPORTANT]
   > Questo esempio usa un contenitore di volumi di dati all'interno di Docker. Se invece si sceglie di eseguire il mapping di una directory host, si noti che esistono limitazioni per questo approccio in Docker per Mac e Windows. Per altre informazioni, vedere [Configurare immagini dei contenitori SQL Server in Docker](./sql-server-linux-docker-container-configure.md#persist).

1. Per visualizzare i contenitori di Docker, usare il comando `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Se nella colonna **STATUS** è impostato lo stato **Up**, SQL Server è in esecuzione nel contenitore e in ascolto sulla porta specificata nella colonna **PORTS**. Se la colonna **STATUS** del contenitore di SQL Server è impostata su **Exited**, vedere la [sezione relativa alla risoluzione dei problemi della guida alla configurazione](./sql-server-linux-docker-container-troubleshooting.md).

  ```bash
  $ sudo docker ps -a

  CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
  941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
  ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Aprire un terminale Bash in Linux/Mac o una sessione di PowerShell con privilegi elevati in Windows.

1. Eseguire il pull dell'immagine del contenitore di SQL Server 2019 su Linux da Docker Hub.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   > [!TIP]
   > In questa esercitazione vengono forniti esempi di comandi di Docker sia per la shell Bash (Linux/Mac) che per PowerShell (Windows).

1. Per eseguire l'immagine del contenitore con Docker, è possibile usare il comando seguente:

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   Questo comando crea un contenitore di SQL Server 2019 con l'edizione Developer (impostazione predefinita). La porta **1433** di SQL Server è esposta nell'host come porta **1401**. Il parametro facoltativo `-v sql1data:/var/opt/mssql` crea un contenitore del volume di dati denominato **sql1ddata**, che viene usato per salvare in modo permanente i dati creati da SQL Server.

1. Per visualizzare i contenitori di Docker, usare il comando `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Se nella colonna **STATUS** è impostato lo stato **Up**, SQL Server è in esecuzione nel contenitore e in ascolto sulla porta specificata nella colonna **PORTS**. Se la colonna **STATUS** del contenitore di SQL Server è impostata su **Exited**, vedere la [sezione relativa alla risoluzione dei problemi della guida alla configurazione](./sql-server-linux-docker-container-troubleshooting.md).

   ```bash
   $ sudo docker ps -a

   CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
   941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
   ```

::: moniker-end

## <a name="change-the-sa-password"></a>Cambiare la password dell'amministratore di sistema

[!INCLUDE [Change docker password](../includes/sql-server-linux-change-docker-password.md)]

## <a name="copy-a-backup-file-into-the-container"></a>Copiare un file di backup nel contenitore

Questa esercitazione usa il [database di esempio Wide World Importers](../samples/wide-world-importers-what-is.md). Usare la procedura seguente per scaricare e copiare il file di backup del database Wide World Importers nel contenitore di SQL Server.

1. Usare prima di tutto **docker exec** per creare una cartella di backup. Il comando seguente crea una directory **/var/opt/mssql/backup** nel contenitore di SQL Server.

   ```bash
   sudo docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

   ```PowerShell
   docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

1. Scaricare quindi il file [WideWorldImporters-Full.bak](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) nel computer host. I comandi seguenti consentono di passare alla directory home/user e di scaricare il file di backup come **wwi.bak**.

   ```bash
   cd ~
   curl -L -o wwi.bak 'https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak'
   ```

   ```PowerShell
   curl -OutFile "wwi.bak" "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak"
   ```

1. Usare **docker cp** per copiare il file di backup nel contenitore nella directory **/var/opt/mssql/backup**.

   ```bash
   sudo docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

   ```PowerShell
   docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

## <a name="restore-the-database"></a>Ripristinare il database

Il file di backup si trova ora all'interno del contenitore. Prima di ripristinare il backup, è importante conoscere i nomi dei file logici e i tipi di file all'interno del backup. I comandi Transact-SQL seguenti esaminano il backup ed eseguono il ripristino usando **sqlcmd** nel contenitore.

> [!TIP]
> Questa esercitazione usa **sqlcmd** nel contenitore, perché questo strumento è preinstallato nel contenitore, ma è anche possibile eseguire le istruzioni Transact-SQL con altri strumenti client esterni al contenitore, ad esempio [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) o [SQL Server Management Studio](sql-server-linux-manage-ssms.md). Per connettersi, usare la porta host di cui è stato eseguito il mapping alla porta 1433 nel contenitore. In questo esempio si tratta di **localhost,1401** nel computer host e di **Host_IP_Address,1401** in remoto.

1. Eseguire **sqlcmd** nel contenitore per elencare i nome dei file logici e i percorsi all'interno del backup. Questa operazione viene eseguita con l'istruzione Transact-SQL **RESTORE FILELISTONLY**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost \
      -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/wwi.bak"' \
      | tr -s ' ' | cut -d ' ' -f 1-2
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost `
      -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/wwi.bak'"
   ```

   L'output dovrebbe essere simile al seguente:

   ```
   LogicalName   PhysicalName
   ------------------------------------------
   WWI_Primary   D:\Data\WideWorldImporters.mdf
   WWI_UserData   D:\Data\WideWorldImporters_UserData.ndf
   WWI_Log   E:\Log\WideWorldImporters.ldf
   WWI_InMemory_Data_1   D:\Data\WideWorldImporters_InMemory_Data_1
   ```

1. Chiamare il comando **RESTORE DATABASE** per ripristinare il database nel contenitore. Specificare i nuovi percorsi per ogni file del passaggio precedente.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE DATABASE WideWorldImporters FROM DISK = "/var/opt/mssql/backup/wwi.bak" WITH MOVE "WWI_Primary" TO "/var/opt/mssql/data/WideWorldImporters.mdf", MOVE "WWI_UserData" TO "/var/opt/mssql/data/WideWorldImporters_userdata.ndf", MOVE "WWI_Log" TO "/var/opt/mssql/data/WideWorldImporters.ldf", MOVE "WWI_InMemory_Data_1" TO "/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1"'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE DATABASE WideWorldImporters FROM DISK = '/var/opt/mssql/backup/wwi.bak' WITH MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf', MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_userdata.ndf', MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf', MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1'"
   ```

   L'output dovrebbe essere simile al seguente:

   ```
   Processed 1464 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   Processed 33 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   Processed 3862 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Converting database 'WideWorldImporters' from version 852 to the current version 869.
   Database 'WideWorldImporters' running the upgrade step from version 852 to version 853.
   Database 'WideWorldImporters' running the upgrade step from version 853 to version 854.
   Database 'WideWorldImporters' running the upgrade step from version 854 to version 855.
   Database 'WideWorldImporters' running the upgrade step from version 855 to version 856.
   Database 'WideWorldImporters' running the upgrade step from version 856 to version 857.
   Database 'WideWorldImporters' running the upgrade step from version 857 to version 858.
   Database 'WideWorldImporters' running the upgrade step from version 858 to version 859.
   Database 'WideWorldImporters' running the upgrade step from version 859 to version 860.
   Database 'WideWorldImporters' running the upgrade step from version 860 to version 861.
   Database 'WideWorldImporters' running the upgrade step from version 861 to version 862.
   Database 'WideWorldImporters' running the upgrade step from version 862 to version 863.
   Database 'WideWorldImporters' running the upgrade step from version 863 to version 864.
   Database 'WideWorldImporters' running the upgrade step from version 864 to version 865.
   Database 'WideWorldImporters' running the upgrade step from version 865 to version 866.
   Database 'WideWorldImporters' running the upgrade step from version 866 to version 867.
   Database 'WideWorldImporters' running the upgrade step from version 867 to version 868.
   Database 'WideWorldImporters' running the upgrade step from version 868 to version 869.
   RESTORE DATABASE successfully processed 58455 pages in 18.069 seconds (25.273 MB/sec).
   ```

## <a name="verify-the-restored-database"></a>Verificare il database ripristinato

Eseguire la query seguente per visualizzare un elenco di nomi di database nel contenitore:

```bash
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
   -Q 'SELECT Name FROM sys.Databases'
```

```PowerShell
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
   -Q "SELECT Name FROM sys.Databases"
```

Nell'elenco di database sarà presente **WideWorldImporters**.

## <a name="make-a-change"></a>Apportare una modifica

I passaggi seguenti apportano una modifica nel database.

1. Eseguire una query per visualizzare i primi 10 elementi della tabella **Warehouse.StockItems**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID"
   ```

   Verrà visualizzato un elenco di identificatori e nomi degli elementi:

   ```
   StockItemID StockItemName
   ----------- -----------------
             1 USB missile launcher (Green)
             2 USB rocket launcher (Gray)
             3 Office cube periscope (Black)
             4 USB food flash drive - sushi roll
             5 USB food flash drive - hamburger
             6 USB food flash drive - hot dog
             7 USB food flash drive - pizza slice
             8 USB food flash drive - dim sum 10 drive variety pack
             9 USB food flash drive - banana
            10 USB food flash drive - chocolate bar
   ```

1. Aggiornare la descrizione del primo elemento con l'istruzione **UPDATE** seguente:

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName="USB missile launcher (Dark Green)" WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName='USB missile launcher (Dark Green)' WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   L'output dovrebbe essere simile al testo seguente:

   ```
   (1 rows affected)
   StockItemID StockItemName
   ----------- ------------------------------------
             1 USB missile launcher (Dark Green)
   ```

## <a name="create-a-new-backup"></a>Creare un nuovo backup

Dopo aver ripristinato il database in un contenitore, è anche possibile creare regolarmente backup del database all'interno del contenitore in esecuzione. I passaggi seguono uno schema simile a quello dei passaggi precedenti, ma in ordine inverso.

1. Usare il comando Transact-SQL **BACKUP DATABASE** per creare un backup del database nel contenitore. Questa esercitazione crea un nuovo file di backup, **wwi_2.bak**, nella directory **/var/opt/mssql/backup** creata in precedenza.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   L'output dovrebbe essere simile al seguente:

   ```
   10 percent processed.
   20 percent processed.
   30 percent processed.
   40 percent processed.
   50 percent processed.
   60 percent processed.
   70 percent processed.
   Processed 1200 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   80 percent processed.
   Processed 3865 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Processed 938 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   100 percent processed.
   BACKUP DATABASE successfully processed 59099 pages in 25.056 seconds (18.427 MB/sec).
   ```

1. Copiare quindi il file di backup all'esterno del contenitore e nel computer host.

   ```bash
   cd ~
   sudo docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls -l wwi*
   ```

   ```PowerShell
   cd ~
   docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls wwi*
   ```

## <a name="use-the-persisted-data"></a>Usare i dati salvati in modo permanente

Oltre a eseguire backup del database per proteggere i dati, è anche possibile usare contenitori di volumi di dati. All'inizio di questa esercitazione è stato creato il contenitore **sql1** con il parametro `-v sql1data:/var/opt/mssql`. Il contenitore del volume di dati **sql1data** salva in modo permanente i dati di **/var/opt/mssql** anche dopo la rimozione del contenitore. I passaggi seguenti rimuovono completamente il contenitore **sql1** e quindi creano un nuovo contenitore, **sql2**, con i dati salvati in modo permanente.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Arrestare il contenitore **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Rimuovere il contenitore. Questa operazione non elimina il contenitore del volume di dati **sql1data** creato in precedenza né i dati salvati in modo permanente.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Creare un nuovo contenitore, **sql2**, e riutilizzare il contenitore del volume di dati **sql1data**.

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

1. Il database Wide World Importers è ora nel nuovo contenitore. Eseguire una query per verificare la modifica precedente apportata.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > La password dell'amministratore di sistema non è la password specificata per il contenitore **sql2**, `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`. Tutti i dati di SQL Server sono stati ripristinati da **sql1**, inclusa la password modificata in precedenza nell'esercitazione. Alcune opzioni come questa sono in effetti ignorate a causa del ripristino dei dati in /var/opt/mssql. Per questo motivo, la password è `<YourNewStrong!Passw0rd>`, come mostrato qui.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Arrestare il contenitore **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Rimuovere il contenitore. Questa operazione non elimina il contenitore del volume di dati **sql1data** creato in precedenza né i dati salvati in modo permanente.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Creare un nuovo contenitore, **sql2**, e riutilizzare il contenitore del volume di dati **sql1data**.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
       --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
       --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

1. Il database Wide World Importers è ora nel nuovo contenitore. Eseguire una query per verificare la modifica precedente apportata.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > La password dell'amministratore di sistema non è la password specificata per il contenitore **sql2**, `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`. Tutti i dati di SQL Server sono stati ripristinati da **sql1**, inclusa la password modificata in precedenza nell'esercitazione. Alcune opzioni come questa sono in effetti ignorate a causa del ripristino dei dati in /var/opt/mssql. Per questo motivo, la password è `<YourNewStrong!Passw0rd>`, come mostrato qui.

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In questa esercitazione si è appreso come eseguire il backup di un database in Windows e come spostarlo in un server Linux che esegue SQL Server 2017. Si è appreso come:

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In questa esercitazione si è appreso come eseguire il backup di un database in Windows e come spostarlo in un server Linux che esegue SQL Server 2019. Si è appreso come:

::: moniker-end

> [!div class="checklist"]
> * Creare immagini dei contenitori di SQL Server su Linux.
> * Copiare i backup del database di SQL Server in un contenitore.
> * Eseguire le istruzioni Transact-SQL nel contenitore con **sqlcmd**.
> * Creare ed estrarre i file di backup da un contenitore.
> * Usare i contenitori dei volumi di dati in Docker per salvare in modo permanente i dati di SQL Server.

Esaminare quindi altri scenari di configurazione e risoluzione dei problemi di Docker:

> [!div class="nextstepaction"]
>[Guida alla configurazione per SQL Server 2017 in Docker](./sql-server-linux-docker-container-deployment.md)