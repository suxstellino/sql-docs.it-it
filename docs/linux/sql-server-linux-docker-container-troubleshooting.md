---
title: Risoluzione dei problemi dei contenitori Docker di SQL Server
description: Esplorare le diverse tecniche di risoluzione dei problemi che è possibile usare per risolvere gli errori comuni riscontrati durante l'uso di contenitori Docker Linux con immagini di SQL Server
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: d94ae00cb8f3581be51f53d999068e60c4d5b7f1
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878809"
---
# <a name="troubleshooting-sql-server-docker-containers"></a>Risoluzione dei problemi dei contenitori Docker di SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra gli errori comuni riscontrati durante la distribuzione e l'uso di contenitori Docker di SQL Server e descrive le tecniche di risoluzione dei problemi per risolvere il problema.

## <a name="docker-command-errors"></a>Errori per i comandi Docker

Se vengono visualizzati errori per i comandi `docker`, verificare che il servizio Docker sia in esecuzione e provare a eseguirlo con autorizzazioni elevate.

Ad esempio, in Linux potrebbe essere visualizzato l'errore seguente quando si eseguono i comandi `docker`:

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Se si riceve questo errore in Linux, provare a eseguire gli stessi comandi preceduti da `sudo`. Se il tentativo non riesce, verificare che il servizio Docker sia in esecuzione e, se necessario, avviarlo.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

In Windows assicurarsi di avviare PowerShell o il prompt dei comandi come amministratore.

## <a name="sql-server-container-startup-errors"></a>Errori di avvio del contenitore di SQL Server

Se non è possibile eseguire il contenitore di SQL Server, provare i test seguenti:

- Se si riceve un errore come `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.`, si sta tentando di eseguire il mapping della porta del contenitore 1433 a una porta già in uso. Questo problema può verificarsi se si esegue SQL Server localmente nel computer host. Può verificarsi anche se si avviano due contenitori di SQL Server e si tenta di eseguirne il mapping per entrambi alla stessa porta host. In tal caso, usare il parametro `-p` per eseguire il mapping della porta del contenitore 1433 a una porta host diversa. Ad esempio: 

    <!--SQL Server 2017 on Linux -->
    ::: moniker range="= sql-server-linux-2017 || = sql-server-2017"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end
    
    <!--SQL Server 2019 on Linux-->
    ::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end

- Se si riceve un errore come `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied` quando si tenta di avviare un contenitore, aggiungere l'utente al gruppo docker in Ubuntu. Quindi disconnettersi e accedere di nuovo perché questa modifica influirà sulle nuove sessioni. 

   ```bash
    usermod -aG docker $USER
   ```

- Controllare se sono presenti messaggi di errore dal contenitore.

   ```bash
   docker logs e69e056c702d
   ```

- Assicurarsi di soddisfare i requisiti minimi di memoria e disco specificati nella sezione dei [prerequisiti](quickstart-install-connect-docker.md#requirements) dell'articolo di avvio rapido.

- Se si usa un software di gestione dei contenitori, assicurarsi che supporti l'esecuzione come root dei processi dei contenitori. Il processo sqlservr nel contenitore viene eseguito come root.

- Se il contenitore Docker di SQL Server viene chiuso immediatamente dopo l'avvio, controllare i log di Docker. Se si usa PowerShell in Windows con il comando `docker run`, usare le virgolette doppie anziché le virgolette singole. Con PowerShell Core usare le virgolette singole.

- Esaminare i [log di installazione e degli errori di SQL Server](#errorlogs).

## <a name="enable-dump-captures"></a>Abilitare le acquisizioni di dump

Se il processo di SQL Server non riesce all'interno del contenitore, è necessario creare un nuovo contenitore con **SYS_PTRACE** abilitato. In questo modo viene aggiunta la funzionalità Linux per tracciare un processo, necessaria per la creazione di un file dump per un'eccezione. Il file dump può essere usato dal supporto tecnico per risolvere il problema. Il comando docker run seguente abilita questa funzionalità.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="sql-server-connection-failures"></a>Errori di connessione a SQL Server

Se non è possibile connettersi all'istanza di SQL Server in esecuzione nel contenitore, provare i test seguenti:

- Verificare che il contenitore di SQL Server sia in esecuzione esaminando la colonna **STATUS** dell'output di `docker ps -a`. In caso contrario, usare `docker start <Container ID>` per avviarlo.

- Se è stato eseguito il mapping a una porta host non predefinita (non 1433), assicurarsi di specificare la porta nella stringa di connessione. È possibile visualizzare il mapping delle porte nella colonna **PORTS** dell'output di `docker ps -a`. Il comando seguente, ad esempio, connette sqlcmd a un contenitore in ascolto sulla porta 1401:

    ::: zone pivot="cs1-bash"
    ```bash
    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
    ```
    ::: zone-end

    ::: zone pivot="cs1-powershell"
    ```PowerShell
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

    ::: zone pivot="cs1-cmd"
    ```cmd
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

- Se è stato usato `docker run` con un volume di dati o un contenitore del volume di dati mappato esistente, SQL Server ignora il valore di `SA_PASSWORD`. Viene invece usata la password dell'utente sa preconfigurata dai dati di SQL Server nel volume di dati o nel contenitore del volume di dati. Verificare di usare la password sa associata ai dati da collegare.

- Esaminare i [log di installazione e degli errori di SQL Server](#errorlogs).

## <a name="sql-server-availability-groups"></a>Gruppi di disponibilità di SQL Server

Se si usa Docker con i gruppi di disponibilità di SQL Server, esistono due requisiti aggiuntivi.

- Eseguire il mapping della porta usata per le comunicazioni di replica (valore predefinito 5022). Ad esempio, specificare `-p 5022:5022` come parte del comando `docker run`.

- Impostare in modo esplicito il nome host del contenitore con il parametro `-h YOURHOSTNAME` del comando `docker run`. Questo nome host viene usato quando si configura il gruppo di disponibilità. Se non viene specificato con `-h`, il valore predefinito è l'**ID del contenitore**.

## <a name="sql-server-setup-and-error-logs"></a><a id="errorlogs"></a> Log di installazione e degli errori di SQL Server

È possibile esaminare i log di installazione e degli errori di SQL Server in **/var/opt/mssql/log**. Se il contenitore non è in esecuzione, avviare prima di tutto il contenitore. Usare quindi un prompt dei comandi interattivo per esaminare i log. È possibile ottenere l'ID del contenitore eseguendo il comando `docker ps`.

```bash
docker start <ContainerID>
docker exec -it <ContainerID> "bash"
```

Dalla sessione bash all'interno del contenitore eseguire i comandi seguenti:

```bash
cd /var/opt/mssql/log
cat setup*.log
cat errorlog
```

> [!TIP]
> Se è stata montata una directory host in **/var/opt/mssql** al momento della creazione del contenitore, è invece possibile esaminare la sottodirectory **log** nel percorso mappato nell'host.

## <a name="execute-commands-in-a-container"></a>Eseguire comandi in un contenitore

Se è disponibile un contenitore in esecuzione, è possibile eseguire i comandi all'interno del contenitore da un terminale host.

Per ottenere l'ID del contenitore, eseguire:

```bash
docker ps -a
```

Per avviare un terminale bash nel contenitore, eseguire:

```bash
docker exec -it <Container ID> /bin/bash
```

A questo punto è possibile eseguire i comandi come se fossero in esecuzione nel terminale all'interno del contenitore. Al termine, digitare `exit`. Viene così chiusa la sessione di esecuzione interattiva dei comandi, ma l'esecuzione del contenitore continua.

## <a name="next-steps"></a>Passaggi successivi

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Per informazioni introduttive sull'uso delle immagini del contenitore di SQL Server 2017 in Docker, vedere questo [articolo di avvio rapido](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Per informazioni introduttive sull'uso delle immagini del contenitore di SQL Server 2019 in Docker, vedere questo [articolo di avvio rapido](quickstart-install-connect-docker.md).

::: moniker-end

- [Distribuire e connettersi a contenitori Docker di SQL Server](sql-server-linux-docker-container-deployment.md)

- [Vedere altre informazioni sulla configurazione e la personalizzazione dei contenitori Docker](sql-server-linux-docker-container-configure.md)

- [Proteggere i contenitori Docker di SQL Server](sql-server-linux-docker-container-security.md)
