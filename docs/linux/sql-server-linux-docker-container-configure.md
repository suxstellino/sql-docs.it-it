---
title: Configurare e personalizzare contenitori Docker di SQL Server
description: Informazioni sui diversi modi disponibili per personalizzare contenitori Docker di SQL Server e su come configurarli in base alle specifiche esigenze
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 3fa589fdb522014994923edfa1a7d7858cfa7b72
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878818"
---
# <a name="configure-and-customize-sql-server-docker-containers"></a>Configurare e personalizzare contenitori Docker di SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come è possibile configurare e personalizzare contenitori Docker di SQL Server, ad esempio per salvare in modo permanente i dati, spostare file da e verso i contenitori e modificare le impostazioni predefinite.

## <a name="create-a-customized-container"></a><a id="customcontainer"></a> Creare un contenitore personalizzato

È possibile creare un [Dockerfile](https://docs.docker.com/engine/reference/builder/#usage) personalizzato per creare un contenitore di SQL Server personalizzato. Per altre informazioni, vedere [una demo che combina SQL Server e un'applicazione Node](https://github.com/twright-msft/mssql-node-docker-demo-app). Se si crea un Dockerfile personalizzato, tenere conto del processo in primo piano, perché questo processo controlla la durata del contenitore. Se viene chiuso, il contenitore verrà arrestato. Ad esempio, se si vuole eseguire uno script e avviare SQL Server, assicurarsi che il processo di SQL Server sia il comando più a destra. Tutti gli altri comandi vengono eseguiti in background. Il comando seguente illustra questa operazione all'interno di un Dockerfile:

```bash
/usr/src/app/do-my-sql-commands.sh & /opt/mssql/bin/sqlservr
```

Se si invertissero i comandi nell'esempio precedente, il contenitore verrebbe arrestato al termine dello script do-my-sql-commands.sh.

## <a name="persist-your-data"></a><a id="persist"></a> Rendere persistenti i dati

Le modifiche apportate alla configurazione del SQL Server e i file di database vengono salvati in modo permanente nel contenitore anche se il contenitore viene riavviato con `docker stop` e `docker start`. Tuttavia, se si rimuove il contenitore con `docker rm` viene eliminato tutto il contenuto al suo interno, inclusi SQL Server e i database. La sezione seguente illustra come usare i **volumi di dati** per rendere persistenti i file di database anche se vengono eliminati i contenitori associati.

> [!IMPORTANT]
> Per SQL Server è fondamentale comprendere la persistenza dei dati in Docker. Oltre alle informazioni in questa sezione, vedere la documentazione di Docker su [come gestire i dati nei contenitori Docker](https://docs.docker.com/engine/tutorials/dockervolumes/).

### <a name="mount-a-host-directory-as-data-volume"></a>Montare una directory host come volume di dati

La prima opzione consiste nel montare una directory nell'host come volume di dati nel contenitore. A tale scopo, usare il comando `docker run` con il flag `-v <host directory>:/var/opt/mssql`. In questo modo è possibile ripristinare i dati tra le esecuzioni dei contenitori.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Questa tecnica consente anche di condividere e visualizzare i file nell'host all'esterno di Docker.

> [!IMPORTANT]
> Il mapping del volume host per **Docker in Windows** non supporta attualmente il mapping della directory `/var/opt/mssql` completa. È tuttavia possibile eseguire il mapping di una sottodirectory, ad esempio `/var/opt/mssql/data` al computer host.
>
> Il mapping del volume host per **Docker su Mac** con l'immagine di SQL Server in Linux non è attualmente supportato. Usare invece i contenitori di volumi di dati. Questa restrizione è specifica per la directory `/var/opt/mssql`. La lettura da una directory montata funziona correttamente. Ad esempio, è possibile montare una directory host usando -v in Mac e ripristinare un backup da un file con estensione bak che risiede nell'host.

### <a name="use-data-volume-containers"></a>Usare i contenitori di volumi di dati

La seconda opzione consiste nell'usare un contenitore di volumi di dati. È possibile creare un contenitore di volumi di dati specificando un nome di volume anziché una directory host con il parametro `-v`. Nell'esempio seguente viene creato un volume di dati condiviso denominato **sqlvolume**.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

> [!NOTE]
> Questa tecnica per la creazione implicita di un volume di dati nel comando run non funziona con le versioni precedenti di Docker. In tal caso, usare i passaggi espliciti descritti nella documentazione di Docker, in [Creating and mounting a data volume container](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container) (Creazione e montaggio di un contenitore di volumi di dati).

Anche se si arresta e rimuove il contenitore, il volume di dati è persistente. È possibile visualizzarlo con il comando `docker volume ls`.

```command
docker volume ls
```

Se successivamente si crea un altro contenitore con lo stesso nome di volume, il nuovo contenitore userà gli stessi dati di SQL Server contenuti nel volume.

Per rimuovere un contenitore di volumi di dati, usare il comando `docker volume rm`.

> [!WARNING]
> Se si elimina il contenitore del volume di dati, tutti i dati di SQL Server nel contenitore verranno eliminati *definitivamente*.

### <a name="backup-and-restore"></a>Backup e ripristino

Oltre a queste tecniche con i contenitori, è anche possibile usare le tecniche standard di backup e ripristino di SQL Server. È possibile usare i file di backup per proteggere i dati o per spostare i dati in un'altra istanza di SQL Server. Per altre informazioni, vedere [Backup e ripristino di database di SQL Server in Linux](sql-server-linux-backup-and-restore-database.md).

> [!WARNING]
> Se si creano backup, assicurarsi di creare o copiare i file di backup all'esterno del contenitore. In caso contrario, se il contenitore viene rimosso, vengono eliminati anche i file di backup.

## <a name="copy-files-from-a-container"></a>Copiare file da un contenitore

Per copiare un file dal contenitore, usare il comando seguente:

```command
docker cp <Container ID>:<Container path> <host path>
```

È possibile ottenere l'ID del contenitore eseguendo il comando `docker ps -a`.

**Esempio:**

::: zone pivot="cs1-bash"
```bash
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog /tmp/errorlog
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

## <a name="copy-files-into-a-container"></a>Copiare file in un contenitore

Per copiare un file nel contenitore, usare il comando seguente:

```command
docker cp <Host path> <Container ID>:<Container path>
```

**Esempio:**

::: zone pivot="cs1-bash"
```bash
docker cp /tmp/mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

## <a name="configure-the-timezone"></a><a id="tz"></a> Configurare il fuso orario

Per eseguire SQL Server in un contenitore Linux con un fuso orario specifico, configurare la variabile di ambiente `TZ`. Per trovare il valore del fuso orario corretto, eseguire il comando `tzselect` da un prompt di bash per Linux:

```command
tzselect
```

Dopo aver selezionato il fuso orario, `tzselect` visualizza un output simile al seguente:

```output
The following information has been given:

        United States
        Pacific

Therefore TZ='America/Los_Angeles' will be used.
```

È possibile usare queste informazioni per impostare la stessa variabile di ambiente nel contenitore Linux. L'esempio seguente mostra come eseguire SQL Server in un contenitore nel fuso orario `Americas/Los_Angeles`:

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'A_PASSWORD=<YourStrong!Passw0rd>' \
-p 1433:1433 --name sql1 \
-e 'TZ=America/Los_Angeles'\
-d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 `
-e "TZ=America/Los_Angeles" `
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 ^
-e "TZ=America/Los_Angeles" ^
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -p 1433:1433 --name sql1 \
   -e 'TZ=America/Los_Angeles'\
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="change-the-default-file-location"></a><a id="changefilelocation"></a> Modificare il percorso predefinito del file

Aggiungere la variabile `MSSQL_DATA_DIR` per modificare la directory dei dati nel comando `docker run`, quindi montare un volume in quel percorso a cui ha accesso l'utente del contenitore.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Per informazioni introduttive sull'uso delle immagini del contenitore di SQL Server 2017 in Docker, vedere questo [articolo di avvio rapido](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Per informazioni introduttive sull'uso delle immagini del contenitore di SQL Server 2019 in Docker, vedere questo [articolo di avvio rapido](quickstart-install-connect-docker.md)

::: moniker-end

- [Distribuire e connettersi a contenitori Docker di SQL Server](sql-server-linux-docker-container-deployment.md)

- [Risoluzione dei problemi dei contenitori Docker di SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Proteggere i contenitori Docker di SQL Server](sql-server-linux-docker-container-security.md)
