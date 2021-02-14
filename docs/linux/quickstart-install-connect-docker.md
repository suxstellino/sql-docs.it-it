---
title: 'Docker: Installare i contenitori per SQL Server in Linux'
description: Questa guida di avvio rapido illustra come usare Docker per eseguire immagini di contenitori di SQL Server 2017 e 2019. Si userà quindi sqlcmd per creare un database ed eseguire query su di esso.
ms.custom: seo-lt-2019, contperf-fy21q1
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.date: 09/07/2020
ms.topic: quickstart
ms.prod: sql
ms.technology: linux
ms.prod_service: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
monikerRange: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 9393064403fbd41255b6be0712813185e745d6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344444"
---
# <a name="quickstart-run-sql-server-container-images-with-docker"></a>Avvio rapido: Eseguire immagini del contenitore di SQL Server con Docker
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

> [!NOTE]
> Gli esempi riportati di seguito usano docker.exe ma la maggior parte di questi comandi funziona anche con Podman. Offre un'interfaccia della riga di comando simile al motore del contenitore Docker. Altre informazioni su Podman sono disponibili [qui](http://docs.podman.io/en/latest).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In questa guida introduttiva si usa Docker per effettuare il pull ed eseguire l'immagine del contenitore di SQL Server 2017, [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server). Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

> [!TIP]
> Se si vogliono eseguire contenitori di SQL Server 2019, vedere la [versione di questo articolo relativa a SQL Server 2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> A partire da SQL Server 2019 CU3 è supportato Ubuntu 18.04.

In questa guida di avvio rapido si usa Docker per il pull e l'esecuzione dell'immagine del contenitore di SQL Server 2019, [mssql-server](https://hub.docker.com/r/microsoft/mssql-server). Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

> [!TIP]
> Questa guida di avvio rapido consente di creare contenitori di SQL Server 2019. Se si preferisce creare contenitori di SQL Server 2017, vedere la [versione di questo articolo relativa a SQL Server 2017](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true).

::: moniker-end

Questa immagine è costituita da SQL Server in esecuzione su Linux basato su Ubuntu 18.04. Può essere usata con il motore Docker 1.8 o versione successiva su Linux o in Docker per Mac/Windows. Questa guida di avvio rapido illustra specificamente l'uso dell'immagine di SQL Server in **Linux**. L'immagine Windows non è argomento di questa guida, ma è possibile ottenere informazioni su di essa nella [pagina mssql-server-windows-developer dell'hub Docker](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

## <a name="prerequisites"></a><a id="requirements"></a> Prerequisiti

- Motore Docker 1.8 o versione successiva in qualsiasi distribuzione di Linux supportata oppure Docker per Mac/Windows. Per altre informazioni, vedere [Installare Docker](https://docs.docker.com/engine/installation/).
- Driver di archiviazione **overlay2** Docker. Si tratta dell'impostazione predefinita per la maggior parte degli utenti. Se non si sta usando questo provider di archiviazione ed è necessario cambiarlo, vedere le istruzioni e gli avvisi nella [documentazione di Docker per la configurazione di overlay2](https://docs.docker.com/storage/storagedriver/overlayfs-driver/#configure-docker-with-the-overlay-or-overlay2-storage-driver).
- Almeno 2 GB di spazio su disco.
- Almeno 2 GB di RAM.
- [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

<!--The following H2 is versioned for 2017 and 2019. Much of the content is duplicated, so
any changes to one section should be duplicated in the other-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="pull-and-run-the-2017-container-image"></a><a id="pullandrun2017"></a> Eseguire il pull dell'immagine del contenitore 2017 ed eseguirla

Prima di iniziare la procedura seguente, assicurarsi di aver selezionato la shell preferita (Bash, PowerShell o cmd) all'inizio di questo articolo.

1. Eseguire il pull dell'immagine del contenitore di SQL Server 2017 in Linux dal Registro Container Microsoft.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!TIP]
   > Se si vogliono eseguire contenitori di SQL Server 2019, vedere la [versione di questo articolo relativa a SQL Server 2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true#pullandrun2019).

   Il comando riportato sopra esegue il pull dell'immagine del contenitore di SQL Server 2017 più recente. Se si vuole eseguire il pull di un'immagine specifica, aggiungere un segno di due punti e il nome del tag (ad esempio `mcr.microsoft.com/mssql/server:2017-GA-ubuntu`). Per visualizzare tutte le immagini disponibili, vedere la [pagina dell'hub Docker su mssql-server](https://hub.docker.com/r/microsoft/mssql-server).

   ::: zone pivot="cs1-bash"
   Per i comandi Bash in questo articolo, si usa `sudo`. In macOS `sudo` potrebbe non essere necessario. In Linux, se non si vuole usare `sudo` per eseguire Docker, è possibile configurare un gruppo **docker** e aggiungere utenti a tale gruppo. Per altre informazioni, vedere [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/) (Passaggi post-installazione per Linux).

   ::: zone-end

2. Per eseguire l'immagine del contenitore con Docker, è possibile usare il comando seguente da una shell Bash (Linux/macOS) o da un prompt dei comandi PowerShell con privilegi elevati.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d \
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Se si usa PowerShell Core, sostituire le virgolette doppie con virgolette singole.
   
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!NOTE]
   > La password deve essere conforme ai criteri password predefiniti di SQL Server, altrimenti il contenitore non potrà configurare SQL Server e smetterà di funzionare. Per impostazione predefinita, la password deve contenere almeno 8 caratteri, appartenenti a tre dei quattro set seguenti: lettere maiuscole, lettere minuscole, cifre in base 10 e simboli. È possibile esaminare il log degli errori eseguendo il comando [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).
   >
   > Per impostazione predefinita, viene creato un contenitore con l'edizione Developer di SQL Server 2017. Il processo di esecuzione delle edizioni di produzione nei contenitori è leggermente diverso. Per altre informazioni, vedere [Run production container images](./sql-server-linux-docker-container-deployment.md#production) (Eseguire immagini del contenitore di produzione).

   La tabella seguente offre una descrizione dei parametri dell'esempio `docker run` precedente:

   | Parametro | Descrizione |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  Impostare la variabile **ACCEPT_EULA** su qualsiasi valore per confermare l'accettazione delle [condizioni di licenza](https://go.microsoft.com/fwlink/?linkid=857698). Impostazione obbligatoria per l'immagine di SQL Server. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Specificare la password complessa composta da almeno 8 caratteri e conforme ai [requisiti per le password di SQL Server](../relational-databases/security/password-policy.md). Impostazione obbligatoria per l'immagine di SQL Server. |
   | **-p 1433:1433** | Eseguire il mapping di una porta TCP nell'ambiente host (primo valore) con una porta TCP nel contenitore (secondo valore). In questo esempio SQL Server è in ascolto sulla porta TCP 1433 nel contenitore e questo è esposto alla porta 1433 nell'host. |
   | **--name sql1** | Specificare un nome personalizzato per il contenitore, invece di un nome generato in modo casuale. Se si eseguono più contenitori, non è possibile riutilizzare questo stesso nome. |
   | **-h sql1** | Usato per impostare in modo esplicito il nome host del contenitore; se non viene specificato, il valore predefinito è l'ID contenitore, ovvero un GUID di sistema generato in modo casuale. |
   | **-d** | Eseguire il contenitore in background (daemon) |
   | **mcr.microsoft.com/mssql/server:2017-latest** | Immagine del contenitore di SQL Server 2017 su Linux. |

3. Per visualizzare i contenitori di Docker, usare il comando `docker ps`.


   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   L'output risultante dovrebbe essere simile allo screenshot seguente:

   ![Output del comando ps di Docker](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Se nella colonna **STATUS** è impostato lo stato **Up**, SQL Server è in esecuzione nel contenitore e in ascolto sulla porta specificata nella colonna **PORTS**. Se la colonna **STATUS** del contenitore di SQL Server è impostata su **Exited**, vedere la [sezione relativa alla risoluzione dei problemi della guida alla configurazione](./sql-server-linux-docker-container-troubleshooting.md).

Il parametro `-h` (nome host) illustrato in precedenza modifica il nome interno del contenitore in un valore personalizzato. È il nome che viene restituito nella query Transact-SQL seguente:

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

L'impostazione di `-h` e `--name` sullo stesso valore è un buon modo per identificare facilmente il contenitore di destinazione.

5. Come passaggio finale, cambiare la password dell'amministratore di sistema, perché il valore di `SA_PASSWORD` è visibile nell'output di `ps -eax` e viene archiviato nella variabile di ambiente con lo stesso nome. Vedere i passaggi seguenti.

::: moniker-end
<!--End of 2017 "Pull and run" section-->

<!--This is the 2019 version of the "Pull and run" section-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="pull-and-run-the-2019-container-image"></a><a id="pullandrun2019"></a> Eseguire il pull dell'immagine del contenitore 2019 ed eseguirla

Prima di iniziare la procedura seguente, assicurarsi di aver selezionato la shell preferita (Bash, PowerShell o cmd) all'inizio di questo articolo.

1. Eseguire il pull dell'immagine del contenitore di SQL Server 2019 in Linux dal Registro Container Microsoft.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Se si usa PowerShell Core, sostituire le virgolette doppie con virgolette singole.
   
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!TIP]
   > Questa guida di avvio rapido usa l'immagine Docker di SQL Server 2019. Se si vuole eseguire l'immagine di SQL Server 2017, vedere la [versione di questo articolo relativa a SQL Server 2017](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true#pullandrun2017).

   Il comando precedente esegue il pull dell'immagine del contenitore di SQL Server 2019 basata su Ubuntu. Per usare invece immagini del contenitore basate su RedHat, vedere [Eseguire immagini del contenitore basate su RHEL](./sql-server-linux-docker-container-deployment.md#rhel). Per vedere tutte le immagini disponibili, vedere la [pagina mssql-server-linux dell'hub Docker](https://hub.docker.com/_/microsoft-mssql-server).

   ::: zone pivot="cs1-bash"
   Per i comandi Bash in questo articolo, si usa `sudo`. In macOS `sudo` potrebbe non essere necessario. In Linux, se non si vuole usare `sudo` per eseguire Docker, è possibile configurare un gruppo **docker** e aggiungere utenti a tale gruppo. Per altre informazioni, vedere [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/) (Passaggi post-installazione per Linux).
   ::: zone-end

2. Per eseguire l'immagine del contenitore con Docker, è possibile usare il comando seguente da una shell Bash (Linux/macOS) o da un prompt dei comandi PowerShell con privilegi elevati.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!NOTE]
   > La password deve essere conforme ai criteri password predefiniti di SQL Server, altrimenti il contenitore non potrà configurare SQL Server e smetterà di funzionare. Per impostazione predefinita, la password deve contenere almeno 8 caratteri, appartenenti a tre dei quattro set seguenti: lettere maiuscole, lettere minuscole, cifre in base 10 e simboli. È possibile esaminare il log degli errori eseguendo il comando [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).
   >
   > Per impostazione predefinita, viene creato un contenitore con l'edizione Developer di SQL Server 2019.

   La tabella seguente offre una descrizione dei parametri dell'esempio `docker run` precedente:

   | Parametro | Descrizione |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  Impostare la variabile **ACCEPT_EULA** su qualsiasi valore per confermare l'accettazione delle [condizioni di licenza](https://go.microsoft.com/fwlink/?LinkId=746388). Impostazione obbligatoria per l'immagine di SQL Server. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Specificare la password complessa composta da almeno 8 caratteri e conforme ai [requisiti per le password di SQL Server](../relational-databases/security/password-policy.md). Impostazione obbligatoria per l'immagine di SQL Server. |
   | **-p 1433:1433** | Eseguire il mapping di una porta TCP nell'ambiente host (primo valore) con una porta TCP nel contenitore (secondo valore). In questo esempio SQL Server è in ascolto sulla porta TCP 1433 nel contenitore e questo è esposto alla porta 1433 nell'host. |
   | **--name sql1** | Specificare un nome personalizzato per il contenitore, invece di un nome generato in modo casuale. Se si eseguono più contenitori, non è possibile riutilizzare questo stesso nome. |
   | **-h sql1** | Usato per impostare in modo esplicito il nome host del contenitore; se non viene specificato, il valore predefinito è l'ID contenitore, ovvero un GUID di sistema generato in modo casuale. |
   | **mcr.microsoft.com/mssql/server:2019-latest** | Immagine del contenitore di SQL Server 2019 su Ubuntu Linux. |

3. Per visualizzare i contenitori di Docker, usare il comando `docker ps`.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   L'output risultante dovrebbe essere simile allo screenshot seguente:

   ![Output del comando ps di Docker](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Se nella colonna **STATUS** è impostato lo stato **Up**, SQL Server è in esecuzione nel contenitore e in ascolto sulla porta specificata nella colonna **PORTS**. Se la colonna **STATUS** (Stato) del contenitore di SQL Server è impostata su **Exited** (Chiuso), vedere la [Risoluzione dei problemi dei contenitori Docker di SQL Server](sql-server-linux-docker-container-troubleshooting.md).

Il parametro `-h` (nome host) illustrato in precedenza modifica il nome interno del contenitore in un valore personalizzato. Cambia il nome interno del contenitore sostituendolo con un valore personalizzato. È il nome che viene restituito nella query Transact-SQL seguente:

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

L'impostazione di `-h` e `--name` sullo stesso valore è un buon modo per identificare facilmente il contenitore di destinazione.


5. Come passaggio finale, cambiare la password dell'amministratore di sistema, perché il valore di `SA_PASSWORD` è visibile nell'output di `ps -eax` e viene archiviato nella variabile di ambiente con lo stesso nome. Vedere i passaggi seguenti.


::: moniker-end
<!--End of 2019 "Pull and run" section-->



## <a name="change-the-sa-password"></a><a id="sapassword"></a> Cambiare la password dell'amministratore di sistema

<!-- This section was pasted in from includes/sql-server-linux-change-docker-password.md, to better support zone pivots. 2019/02/11 -->

L'account **SA** è un amministratore di sistema dell'istanza di SQL Server creato durante l'installazione. Dopo aver creato il contenitore SQL Server, la variabile di ambiente `SA_PASSWORD` specificata diventa individuabile eseguendo `echo $SA_PASSWORD` nel contenitore. Per motivi di sicurezza, modificare la password dell'amministratore di sistema.

1. Scegliere una password complessa da usare per l'utente SA.

1. Usare `docker exec` per eseguire **sqlcmd** per modificare la password usando Transact-SQL. Nell'esempio seguente sostituire la vecchia password, `<YourStrong!Passw0rd>`, e la nuova password, `<YourNewStrong!Passw0rd>`, con le password personali.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P "<YourStrong@Passw0rd>" \
      -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong@Passw0rd>"'
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong@Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong!Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

## <a name="connect-to-sql-server"></a>Connessione a SQL Server

La procedura seguente usa lo strumento da riga di comando di SQL Server, [**sqlcmd**](../tools/sqlcmd-utility.md), all'interno del contenitore per stabilire la connessione a SQL Server.

1. Usare il comando `docker exec -it` per avviare una shell Bash interattiva all'interno del contenitore in esecuzione. Nell'esempio seguente `sql1` è il nome specificato dal parametro `--name`quando è stato creato il contenitore.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

2. Una volta all'interno del contenitore, eseguire la connessione in locale con sqlcmd. Sqlcmd non è incluso nel percorso per impostazione predefinita, quindi occorre specificare il percorso completo.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
   ```

   > [!TIP]
   > È possibile omettere la password nella riga di comanda perché sia richiesto di essere immessa.

3. Se la connessione viene eseguita correttamente, il prompt dei comandi **sqlcmd** sarà: `1>`.

## <a name="create-and-query-data"></a>Creare i dati e recuperarli tramite query

Nelle sezioni seguenti viene descritto l'uso di **sqlcmd** e Transact-SQL per creare un nuovo database, aggiungere dati ed eseguire una query.

### <a name="create-a-new-database"></a>Creare un nuovo database

La seguente procedura consente di creare un nuovo database denominato `TestDB`.

1. Dal prompt dei comandi **sqlcmd** incollare il comando seguente di Transact-SQL per creare un database di test:

   ```sql
   CREATE DATABASE TestDB
   ```

2. Nella riga successiva scrivere una query perché vengano restituiti i nomi di tutti database nel server:

   ```sql
   SELECT Name from sys.Databases
   ```

3. I due comandi precedenti non sono stati eseguiti immediatamente. Digitare `GO` in una nuova riga per eseguire i comandi precedenti:

   ```sql
   GO
   ```

### <a name="insert-data"></a>Inserire i dati

Creare poi una nuova tabella `Inventory` e inserire due nuove righe.

1. Dal prompt dei comandi **sqlcmd** spostare il contesto nel nuovo database `TestDB`:

   ```sql
   USE TestDB
   ```

2. Creare una nuova tabella denominata `Inventory`:

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

3. Inserire i dati nella nuova tabella:

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

4. Digitare `GO` per eseguire i comandi precedenti:

   ```sql
   GO
   ```

### <a name="select-data"></a>Selezionare i dati

A questo punto, eseguire una query per restituire i dati dalla tabella `Inventory`.

1. Dal prompt dei comandi **sqlcmd** immettere una query che restituisca le righe dalla tabella `Inventory` che ne contiene oltre 152:

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

2. Eseguire il comando seguente:

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Uscire dal prompt dei comandi sqlcmd

1. Per terminare la sessione **sqlcmd**, digitare `QUIT`:

   ```sql
   QUIT
   ```

2. Per uscire dal prompt dei comandi interattivo nel contenitore, digitare `exit`. Dopo la chiusura della shell Bash interattiva, il contenitore continua l'esecuzione.

## <a name="connect-from-outside-the-container"></a><a id="connectexternal"></a> Eseguire la connessione dall'esterno del contenitore

È anche possibile connettersi all'istanza di SQL Server nel computer che esegue Docker da uno strumento esterno Linux, Windows o macOS che supporti le connessioni SQL.

La procedura seguente usa **sqlcmd** all'esterno del contenitore per stabilire la connessione a SQL Server in esecuzione nel contenitore. Questa procedura presuppone che gli strumenti da riga di comando di SQL Server siano già installati all'esterno del contenitore. Gli stessi principi valgono quando si usano altri strumenti, ma il processo di connessione è specifico di ogni strumento.

1. Trovare l'indirizzo IP del computer che ospita il contenitore. Su Linux usare **ifconfig** o **ip addr**. Su Windows usare **ipconfig**.

1. Per questo esempio, installare lo strumento **sqlcmd** nel computer client. Per altre informazioni, vedere [Installare sqlcmd in Windows](../tools/sqlcmd-utility.md) o [Installare sqlcmd in Linux](sql-server-linux-setup-tools.md).

1. Eseguire sqlcmd specificando l'indirizzo IP e la porta mappata alla porta 1433 nel contenitore. In questo esempio si tratta della stessa porta, la 1433, nel computer host. Se si è specificata una porta con mapping diverso nel computer host, è possibile usarla qui. Sarà inoltre necessario aprire la porta in ingresso appropriata nel firewall per consentire la connessione.

   ::: zone pivot="cs1-bash"
   ```bash
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

1. Eseguire i comandi Transact-SQL. Al termine, digitare `QUIT`.

Altri strumenti usati comunemente per connettersi a SQL Server sono:

- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) per Windows](sql-server-linux-manage-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [mssql-cli (Preview)](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)
- [PowerShell Core](sql-server-linux-manage-powershell-core.md)

## <a name="remove-your-container"></a>Rimuovere il contenitore

Se si vuole rimuovere il contenitore di SQL Server usato in questa esercitazione, eseguire i comandi seguenti:

::: zone pivot="cs1-bash"
```bash
sudo docker stop sql1
sudo docker rm sql1
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker stop sql1
docker rm sql1
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker stop sql1
docker rm sql1
```
::: zone-end

> [!WARNING]
> Se si arresta e si rimuove un contenitore, i dati di SQL Server eventualmente presenti nel contenitore vengono eliminati in modo definitivo. Se occorre conservare i dati, [creare un file di backup e copiarlo all'esterno del contenitore](tutorial-restore-backup-in-sql-server-container.md) oppure usare una [tecnica di persistenza dei dati del contenitore](sql-server-linux-docker-container-configure.md#persist).

## <a name="docker-demo"></a>Demo di Docker

Se, dopo aver provato a usare l'immagine del contenitore di SQL Server per Docker, si vuole sapere come si può usare questo strumento per migliorare le attività di sviluppo e test, il video seguente illustra l'uso di Docker in uno scenario di integrazione continua e distribuzione.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T152/player]

## <a name="next-steps"></a>Passaggi successivi

Per un'esercitazione sul ripristino dei file di backup di database in un contenitore, vedere [Restore a SQL Server database in a Linux Docker container](tutorial-restore-backup-in-sql-server-container.md) (Ripristinare un database di SQL Server in un contenitore Docker Linux). Esplorare altri scenari, ad esempio l'esecuzione di [più contenitori](sql-server-linux-docker-container-deployment.md#multiple), la [persistenza dei dati](sql-server-linux-docker-container-configure.md#persist) e la [risoluzione dei problemi](sql-server-linux-docker-container-troubleshooting.md).

Nel [repository GitHub mssql-docker](https://github.com/Microsoft/mssql-docker) sono inoltre disponibili risorse, feedback e documentazione su problemi noti.
