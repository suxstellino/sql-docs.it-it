---
title: Distribuire e connettersi a contenitori Docker di SQL Server
description: Scoprire in che modo è possibile distribuire SQL Server nei contenitori Docker e informazioni sui vari strumenti disponibili per connettersi a SQL Server dall'interno e dall'esterno del contenitore
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: f789dd2b80515116e7bf0a12e26e6e8fe6c45b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273102"
---
# <a name="deploy-and-connect-to-sql-server-docker-containers"></a>Distribuire e connettersi a contenitori Docker di SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come distribuire e connettersi ai contenitori Docker di SQL Server.

Per altri scenari di distribuzione, vedere:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Kubernetes - Cluster Big Data](../big-data-cluster/deploy-get-started.md)

> [!NOTE]
> Questo articolo è incentrato nello specifico sull'uso dell'immagine mssql-server-linux. L'immagine di Windows non è qui trattata, ma è possibile ottenere informazioni su di essa nella [pagina mssql-server-windows di Docker Hub](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

> [!IMPORTANT]
> Prima di scegliere di eseguire un contenitore SQL Server per i casi d'uso di produzione, vedere i [criteri di supporto per i contenitori SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) per assicurarsi che l'esecuzione avvenga in una configurazione supportata.

Questo video di 6 minuti offre un'introduzione relativa all'esecuzione di SQL Server nei contenitori:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2019-in-Containers/player?WT.mc_id=dataexposed-c9-niner]

## <a name="pull-and-run-the-container-image"></a>Effettuare il pull ed eseguire l'immagine del contenitore

Per il pull e l'esecuzione delle immagini del contenitore Docker per SQL Server 2017 e SQL Server 2019, seguire i prerequisiti e i passaggi illustrati nella guida di avvio rapido seguente:

- [Eseguire l'immagine del contenitore di SQL Server 2017 con Docker](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)
- [Eseguire l'immagine del contenitore di SQL Server 2019 con Docker](quickstart-install-connect-docker.md?view=sql-server-ver15&preserve-view=true)

Questo articolo dedicato alla configurazione descrive altri scenari di utilizzo nelle sezioni seguenti.

## <a name="connect-and-query"></a>Connessione ed esecuzione di query

È possibile connettersi a SQL Server ed eseguire query in un contenitore dall'esterno o dall'interno del contenitore. Nelle sezioni seguenti vengono illustrati entrambi gli scenari.

### <a name="tools-outside-the-container"></a>Strumenti all'esterno del contenitore

È possibile connettersi all'istanza di SQL Server nel computer che esegue Docker da uno strumento esterno Linux, Windows o macOS che supporti le connessioni SQL. Alcuni strumenti comuni includono:

- [Azure Data Studio](../azure-data-studio/quickstart-sql-server.md)
- [sqlcmd](sql-server-linux-setup-tools.md)
- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) per Windows](sql-server-linux-manage-ssms.md)

Nell'esempio seguente viene usato **sqlcmd** per connettersi a SQL Server in esecuzione in un contenitore Docker. L'indirizzo IP nella stringa di connessione è l'indirizzo IP del computer host in cui è in esecuzione il contenitore.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

Se è stato eseguito il mapping di una porta host diversa dalla porta predefinita **1433**, aggiungere tale porta alla stringa di connessione. Ad esempio, se si specifica `-p 1400:1433` nel comando `docker run` connettersi specificando in modo esplicito la porta 1400.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1400 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

### <a name="tools-inside-the-container"></a>Strumenti all'interno del contenitore

A partire da SQL Server 2017, gli [strumenti da riga di comando di SQL Server](sql-server-linux-setup-tools.md) sono inclusi nell'immagine del contenitore. Se ci si connette all'immagine con un prompt dei comandi interattivo, è possibile eseguire gli strumenti localmente.

1. Usare il comando `docker exec -it` per avviare una shell Bash interattiva all'interno del contenitore in esecuzione. Nell'esempio seguente `e69e056c702d` è l'ID del contenitore.

    ```bash
    docker exec -it e69e056c702d "bash"
    ```

    > [!TIP]
    > Non è sempre necessario specificare l'intero ID del contenitore. È sufficiente specificare un numero sufficiente di caratteri per identificarlo in modo univoco. In questo esempio potrebbe essere quindi sufficiente usare `e6` o `e69` anziché l'ID completo. Per individuare l'ID del contenitore, eseguire il comando `docker ps -a`.

2. Una volta all'interno del contenitore, eseguire la connessione in locale con sqlcmd. Sqlcmd non è incluso nel percorso per impostazione predefinita, quindi occorre specificare il percorso completo.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Dopo aver completato le operazioni con sqlcmd, digitare `exit`.

4. Al termine delle operazioni con il prompt dei comandi interattivo, digitare `exit`. Dopo la chiusura della shell Bash interattiva, il contenitore continua l'esecuzione.

## <a name="check-the-container-version"></a><a id="version"></a> Controllare la versione del contenitore

Se si vuole scoprire la versione di SQL Server in un contenitore Docker in esecuzione, eseguire il comando seguente per visualizzarla. Sostituire `<Container ID or name>` con l'ID o il nome del contenitore di destinazione. Sostituire `<YourStrong!Passw0rd>` con la password di SQL Server per l'account di accesso sa.

::: zone pivot="cs1-bash"
```bash
sudo docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P '<YourStrong!Passw0rd>' \
-Q 'SELECT @@VERSION'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd `
-S localhost -U SA -P "<YourStrong!Passw0rd>" `
-Q "SELECT @@VERSION"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd ^
-S localhost -U SA -P "<YourStrong!Passw0rd>" ^
-Q "SELECT @@VERSION"
```
::: zone-end

È anche possibile identificare la versione di SQL Server e il numero di build per un'immagine del contenitore Docker di destinazione. Il comando seguente visualizza la versione di SQL Server e informazioni sulla build per l'immagine **mcr.microsoft.com/mssql/server:2017-latest**. A tale scopo, esegue un nuovo contenitore con una variabile di ambiente **PAL_PROGRAM_INFO = 1**. Il contenitore risultante viene chiuso immediatamente e il comando `docker rm` lo rimuove.

::: zone pivot="cs1-bash"
```bash
sudo docker run -e PAL_PROGRAM_INFO=1 --name sqlver \
-ti mcr.microsoft.com/mssql/server:2019-latest && \
sudo docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e PAL_PROGRAM_INFO=1 --name sqlver `
-ti mcr.microsoft.com/mssql/server:2019-latest; `
docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e PAL_PROGRAM_INFO=1 --name sqlver ^
-ti mcr.microsoft.com/mssql/server:2019-latest && ^
docker rm sqlver
```
::: zone-end

I comandi precedenti visualizzano informazioni sulla versione simili all'output seguente:

```output
sqlservr
  Version 15.0.4063.15
  Build ID 8a3bb4cca325e1d0b3071b3a193f6a1d74b440fbd95d2fb18881651a5b9ec8e8
  Build Type release
  Git Version 0335c462
  Built at Fri Aug 28 04:50:27 GMT 2020

PAL
  Build ID cc5ceea1b3d294f7d0166f99932f98c7eacfaaa81bcd7cf23c6a89f785829b63
  Build Type release
  Git Version ae9d66dff
  Built at Fri Aug 28 04:46:48 GMT 2020

Packages
  system.security                         6.2.9200.10,unset,
  system.certificates                     6.2.9200.10,unset,
  secforwarderxplat                       15.0.4063.15
  sqlservr                                15.0.4063.15
  system.common                           10.0.17134.1246.202005133
  system.netfx                            4.7.2.461814
  system                                  6.2.9200.10,unset,
  sqlagent                                15.0.4063.15
```

## <a name="run-a-specific-sql-server-container-image"></a><a id="tags"></a> Eseguire un'immagine del contenitore di SQL Server specifica

> [!NOTE]
> A partire da SQL Server 2019 CU3 è supportato Ubuntu 18.04. È possibile recuperare un elenco di tutti i tag disponibili per mssql/server in <https://mcr.microsoft.com/v2/mssql/server/tags/list>.

Esistono scenari in cui è possibile che non si voglia usare la versione più recente dell'immagine del contenitore di SQL Server. Per eseguire un'immagine del contenitore di SQL Server specifica, seguire questa procedura:

1. Identificare il **tag** di Docker per la versione che si vuole usare. Per visualizzare i tag disponibili, vedere la [pagina mssql-server-linux dell'hub Docker](https://hub.docker.com/_/microsoft-mssql-server).

2. Eseguire il pull dell'immagine del contenitore di SQL Server con il tag. Ad esempio, per eseguire il pull dell'immagine 2019-CU7-ubuntu-18.04, sostituire `<image_tag>` nel comando seguente con `2019-CU7-ubuntu-18.04`.

   ```bash
   docker pull mcr.microsoft.com/mssql/server:<image_tag>
   ```

3. Per eseguire un nuovo contenitore con tale immagine, specificare il nome del tag nel comando `docker run`. Nel comando seguente sostituire `<image_tag>` con la versione che si vuole eseguire.

   ::: zone pivot="cs1-bash"
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

Questi passaggi possono essere usati anche per effettuare il downgrade di un contenitore esistente. Ad esempio, potrebbe essere necessario effettuare il rollback o il downgrade di un contenitore in esecuzione per la risoluzione dei problemi o per attività di test. Per effettuare il downgrade di un contenitore in esecuzione, è necessario usare una tecnica di persistenza per la cartella dati. Seguire la stessa procedura descritta nella [sezione dedicata all'aggiornamento](#upgrade), ma specificare il nome del tag della versione precedente quando si esegue il nuovo contenitore.

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="run-rhel-based-container-images"></a><a id="rhel"></a> Eseguire immagini del contenitore basate su RHEL

La documentazione dedicata alle immagini del contenitore di SQL Server per Linux si riferisce a contenitori basati su Ubuntu. A partire da SQL Server 2019, è possibile usare contenitori basati su Red Hat Enterprise Linux (RHEL). Un esempio di immagine per RHEL sarà simile a **mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8**.

Il comando seguente, ad esempio, esegue il pull del contenitore di Cumulative Update 1 per SQL Server 2019 che usa RHEL 8:

::: zone pivot="cs1-bash"
```bash
sudo docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: moniker-end

## <a name="run-production-container-images"></a><a id="production"></a> Eseguire immagini del contenitore di produzione

Le istruzioni di [avvio rapido](quickstart-install-connect-docker.md) nella sezione precedente prevedono l'uso dell'edizione Developer gratuita di SQL Server da Docker Hub. La maggior parte delle informazioni è comunque applicabile per l'esecuzione di immagini del contenitore di produzione, ad esempio per le edizioni Enterprise, Standard o Web. Esistono tuttavia alcune differenze descritte di seguito.

- È possibile usare SQL Server in un ambiente di produzione solo se è disponibile una licenza valida. [Qui](https://go.microsoft.com/fwlink/?linkid=857693) è possibile ottenere una licenza di produzione per SQL Server Express gratuita. Le licenze delle edizioni SQL Server Standard ed Enterprise sono disponibili tramite [Microsoft Volume Licensing](https://www.microsoft.com/licensing/default.aspx).

- È possibile configurare l'immagine del contenitore Developer anche per eseguire le edizioni di produzione. Per eseguire le edizioni di produzione, seguire questa procedura:

Esaminare i requisiti e ed eseguire le procedure in questo [avvio rapido](quickstart-install-connect-docker.md). È necessario specificare l'edizione di produzione con la variabile di ambiente **MSSQL_PID**. L'esempio seguente mostra come eseguire la versione più recente dell'immagine del contenitore di SQL Server 2017 per l'edizione Enterprise:

::: zone pivot="cs1-bash"
```bash
docker run --name sqlenterprise \
-e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
-e 'MSSQL_PID=Enterprise' -p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-e "MSSQL_PID=Enterprise" -p 1433:1433 `
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" ^
-e "MSSQL_PID=Enterprise" -p 1433:1433 ^
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

> [!IMPORTANT]
> Passando il valore **Y** alla variabile di ambiente **ACCEPT_EULA** e un valore di edizione a **MSSQL_PID**, si conferma la disponibilità di una licenza valida ed esistente per l'edizione e la versione di SQL Server che si intende usare. Si accetta anche che l'uso del software SQL Server in un'immagine del contenitore Docker sarà disciplinato dalle condizioni della licenza di SQL Server.

> [!NOTE]
> Per un elenco completo dei valori possibili per **MSSQL_PID**, vedere [Configurare le impostazioni di SQL Server con le variabili di ambiente in Linux](sql-server-linux-configure-environment-variables.md).

## <a name="run-multiple-sql-server-containers"></a><a id="multiple"></a>Eseguire più contenitori di SQL Server

Docker prevede un modo per eseguire più contenitori di SQL Server nello stesso computer host. Usare questo approccio per gli scenari che richiedono più istanze di SQL Server nello stesso host. Ogni contenitore deve essere esposto su una porta diversa.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Nell'esempio seguente vengono creati due contenitori di SQL Server 2017 e ne viene eseguito il mapping alle porte **1401** e **1402** nel computer host.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

L'esempio seguente crea due contenitori di SQL Server 2019 ed esegue il mapping di questi alle porte **1401** e **1402** nel computer host.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Sono ora disponibili due istanze di SQL Server in esecuzione in contenitori separati. I client possono connettersi a ogni istanza di SQL Server usando l'indirizzo IP dell'host Docker e il numero di porta per il contenitore.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
sqlcmd -S 10.3.2.4,1402 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

## <a name="upgrade-sql-server-in-containers"></a><a id="upgrade"></a> Aggiornare SQL Server nei contenitori

Per aggiornare l'immagine del contenitore con Docker, identificare prima di tutto il tag per la versione per l'aggiornamento. Eseguire il pull di questa versione dal Registro di sistema con il comando `docker pull`:

```command
docker pull mcr.microsoft.com/mssql/server:<image_tag>
```

L'immagine di SQL Server viene così aggiornata per tutti i nuovi contenitori creati, ma SQL Server non viene aggiornato in alcun contenitore in esecuzione. A tale scopo, è necessario creare un nuovo contenitore con l'immagine del contenitore di SQL Server più recente ed eseguire la migrazione dei dati al nuovo contenitore.

1. Assicurarsi di usare una delle [tecniche di persistenza dei dati](sql-server-linux-docker-container-configure.md#persist) per il contenitore di SQL Server esistente. In questo modo è possibile avviare un nuovo contenitore con gli stessi dati.

1. Arrestare il contenitore di SQL Server con il comando `docker stop`.

1. Creare un nuovo contenitore di SQL Server con `docker run` e specificare una directory host mappata o un contenitore di volumi di dati. Assicurarsi di usare il tag specifico per l'aggiornamento di SQL Server. Il nuovo contenitore usa ora una nuova versione di SQL Server con i dati di SQL Server esistenti.

   > [!IMPORTANT]
   > L'aggiornamento è supportato solo tra le versioni RC1, RC2 e GA al momento.

1. Verificare i database e i dati nel nuovo contenitore.

1. Facoltativamente, rimuovere il contenitore precedente con `docker rm`.

## <a name="next-steps"></a>Passaggi successivi

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Per informazioni introduttive sull'uso delle immagini del contenitore di SQL Server 2017 in Docker, vedere questo [articolo di avvio rapido](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Per informazioni introduttive sull'uso delle immagini del contenitore di SQL Server 2019 in Docker, vedere questo [articolo di avvio rapido](quickstart-install-connect-docker.md)

::: moniker-end

- [Vedere altre informazioni sulla configurazione e la personalizzazione dei contenitori Docker](sql-server-linux-docker-container-configure.md)

- Nel [repository GitHub mssql-docker](https://github.com/Microsoft/mssql-docker) sono disponibili risorse, feedback e documentazione su problemi noti

- [Risoluzione dei problemi dei contenitori Docker di SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Esplorare la disponibilità elevata per i contenitori di SQL Server](sql-server-linux-container-ha-overview.md)

- [Proteggere i contenitori Docker di SQL Server](sql-server-linux-docker-container-security.md)
