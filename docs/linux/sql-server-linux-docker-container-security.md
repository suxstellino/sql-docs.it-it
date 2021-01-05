---
title: Proteggere i contenitori Docker di SQL Server
description: Informazioni sulle diverse modalità di protezione dei contenitori Docker di SQL Server e su come è possibile eseguire i contenitori come utente diverso non root nell'host
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 88c179b1a17dd84dc33c2ae77979507e82b7a55a
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878758"
---
# <a name="secure-sql-server-docker-containers"></a>Proteggere i contenitori Docker di SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Per impostazione predefinita, i contenitori di SQL Server 2017 vengono avviati come utente root. Questo può causare problemi di sicurezza. Questo articolo illustra le opzioni di sicurezza disponibili quando si eseguono contenitori Docker di SQL Server e come creare un contenitore di SQL Server come utente non root.

## <a name="build-and-run-non-root-sql-server-2017-containers"></a><a id="buildnonrootcontainer"></a> Compilare ed eseguire contenitori di SQL Server 2017 non radice

Eseguire la procedura seguente per creare un contenitore di SQL Server 2017 che si avvia come utente `mssql` (non root).

> [!NOTE]
> I contenitori di SQL Server 2019 vengono avviati automaticamente come utenti non root. I passaggi seguenti, quindi, si applicano solo ai contenitori di SQL Server 2017, che si avviano come root per impostazione predefinita.

1. Scaricare il [dockerfile di esempio per il contenitore di SQL Server non radice](https://raw.githubusercontent.com/microsoft/mssql-docker/master/linux/preview/examples/mssql-server-linux-non-root/Dockerfile) e salvarlo come `dockerfile`.

2. Eseguire il comando seguente nel contesto della directory dockerfile per compilare il contenitore di SQL Server non radice:

    ```bash
    cd <path to dockerfile>
    docker build -t 2017-latest-non-root .
    ```

3. Avviare il contenitore.

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword@" --cap-add SYS_PTRACE --name sql1 -p 1433:1433 -d 2017-latest-non-root
    ```

    > [!NOTE]
    > Il flag `--cap-add SYS_PTRACE` è necessario per i contenitori di SQL Server non radice per generare dump a scopo di risoluzione dei problemi.

4. Verificare che il contenitore sia in esecuzione come utente non radice:

    ```bash
    docker exec -it sql1 bash
    ```

    Eseguire `whoami` che restituirà l'utente in esecuzione nel contenitore.
    
    ```bash
    whoami
    ```

## <a name="run-container-as-a-different-non-root-user-on-the-host"></a><a id="nonrootuser"></a> Eseguire il contenitore come un altro utente non radice nell'host

Per eseguire il contenitore di SQL Server come un altro utente non radice, aggiungere il flag -u al comando docker run. Il contenitore non root prevede una restrizione, ovvero deve essere eseguito come parte del gruppo root, a meno che non venga montato un volume in `/var/opt/mssql` a cui l'utente non root può accedere. Il gruppo radice non concede alcuna autorizzazione radice aggiuntiva all'utente non radice.

#### <a name="run-as-a-user-with-a-uid-4000"></a>Eseguire come utente con UID 4000

È possibile avviare SQL Server con un UID personalizzato. Ad esempio, il comando seguente avvia SQL Server con l'UID 4000:

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u 4000:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

> [!Warning]
> Verificare che il contenitore di SQL Server disponga di un utente denominato, ad esempio 'mssql' o 'root'. In caso contrario non sarà possibile eseguire SQLCMD all'interno del contenitore. È possibile verificare se il contenitore di SQL Server è in esecuzione come utente denominato eseguendo `whoami` nel contenitore.

#### <a name="run-the-non-root-container-as-the-root-user"></a>Eseguire il contenitore non radice come utente radice

Se necessario, è possibile eseguire il contenitore non root come utente root. In questo modo, tutte le autorizzazioni per i file vengono concesse automaticamente al contenitore poiché ha privilegi più elevati.

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -u 0:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-user-on-your-host-machine"></a>Eseguire come utente nel computer host

È possibile avviare SQL Server con un utente esistente nel computer host con il comando seguente:
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u $(id -u myusername):0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-different-user-and-group"></a>Eseguire come utente e gruppo diversi

È possibile avviare SQL Server con un utente e un gruppo personalizzati. In questo esempio, il volume montato ha le autorizzazioni configurate per l'utente o il gruppo nel computer host.

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u (id -u myusername):(id -g myusername) -v /path/to/mssql:/var/opt/mssql -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

## <a name="configure-persistent-storage-permissions-for-non-root-containers"></a><a id="storagepermissions"></a> Configurare le autorizzazioni di archiviazione permanenti per i contenitori non radice

Per consentire all'utente non root di accedere ai file di database in volumi montati, assicurarsi che l'utente o il gruppo con cui viene eseguito il contenitore possa leggere/scrivere nell'archivio file permanente.  

È possibile ottenere la proprietà corrente dei file di database con questo comando.

```bash
ls -ll <database file dir>
```

Se SQL Server non ha accesso ai file di database persistenti, eseguire uno dei comandi seguenti.

#### <a name="grant-the-root-group-rw-access-to-the-db-files"></a>Concedere al gruppo radice l'accesso in lettura/scrittura ai file di database

Concedere le autorizzazioni per il gruppo radice alle directory seguenti in modo che il contenitore di SQL Server non radice abbia accesso ai file di database.

```bash
chgrp -R 0 <database file dir>
chmod -R g=u <database file dir>
```

#### <a name="set-the-non-root-user-as-the-owner-of-the-files"></a>Impostare l'utente non root come proprietario dei file

Può trattarsi dell'utente non radice predefinito o di qualsiasi altro utente non radice che si vuole specificare. In questo esempio si imposta UID 10001 come utente non radice.

```bash
chown -R 10001:0 <database file dir>
```
## <a name="encrypting-connections-to-sql-server-linux-containers"></a>Crittografia delle connessioni a contenitori Linux di SQL Server

Per crittografare le connessioni ai contenitori Linux di SQL Server, sarà necessario un certificato con i requisiti documentati [qui].

Di seguito è riportato un esempio di come è possibile crittografare la connessione a contenitori Linux di SQL Server. Qui viene usato un certificato autofirmato, che non deve essere usato per gli scenari di produzione per tali ambienti, in cui è necessario usare i certificati di CA.

1. Creare un certificato autofirmato, adatto solo per ambienti di test e non di produzione.
  
      ```bash
      openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=sql1.contoso.com' -keyout /container/sql1/mssql.key -out /container/sql1/mssql.pem -days 365
      ```
     Dove sql1 è il nome host del contenitore SQL, quindi quando ci si connette a questo contenitore il nome usato nella stringa di connessione sarà \'sql1.contoso.com,port\'.

    > [!NOTE]
    > Verificare che il percorso della cartella /container/sql1/ esista già prima di eseguire il comando precedente.

2. Assicurarsi di impostare le autorizzazioni corrette per i file mssql.key e mssql.pem, in modo da evitare errori quando si montano i file nel contenitore SQL:

    ```bash
    chmod 440 /container/sql1/mssql.pem
    chmod 440 /container/sql1/mssql.key
    ```

3. Creare ora un file mssql.conf con il contenuto seguente per abilitare la crittografia avviata dal server. Per la crittografia avviata dal client, modificare l'ultima riga in 'forceencryption = 0\'.

    ```bash
    [network]
    tlscert = /etc/ssl/certs/mssql.pem
    tlskey = /etc/ssl/private/mssql.key
    tlsprotocols = 1.2
    forceencryption = 1
    ```

    > [!NOTE]
    > Per alcune distribuzioni di Linux il percorso per l'archiviazione del certificato e della chiave può anche essere rispettivamente /etc/pki/tls/certs/ e /etc/pki/tls/private/. Verificare il percorso prima di aggiornare mssql.conf per i contenitori SQL. Il percorso impostato in mssql.conf sarà il percorso in cui SQL Server nel contenitore cercherà il certificato e la relativa chiave. In questo caso, il percorso è /etc/ssl/certs/ e /etc/ssl/private/.

    Viene anche creato il file mssql.conf nello stesso percorso della cartella /container/sql1/. Dopo aver eseguito i passaggi precedenti, dovrebbero essere presenti i tre file mssql.conf, mssql.key e mssql.pem nella cartella sql1.

4. Distribuire il contenitore SQL con il comando riportato di seguito:

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5434:1433 --name sql1 -h sql1 -v /container/sql1/mssql.conf:/var/opt/mssql/mssql.conf -v   /container/sql1/mssql.pem:/etc/ssl/certs/mssql.pem -v /container/sql1/mssql.key:/etc/ssl/private/mssql.key -d mcr.microsoft.com/mssql/server:2019-latest
    ```

    Nel comando precedente sono stati montati i file mssql.conf, mssql.pem e mssql.key nel contenitore ed è stato eseguito il mapping della porta 1433 (porta predefinita di SQL Server) nel contenitore alla porta 5434 dell'host. 

    > [!NOTE]
    > Se si usano RHEL 8 e versioni successive, è anche possibile usare il comando \'podman run\' invece di \'docker run\'. 

Vedere le sezioni \"Registrare il certificato nel computer client\" ed \"Esempi di stringhe di connessione\" documentate [qui][1] per iniziare a crittografare le connessioni ai contenitori di SQL Server in Linux.

  [Encrypting connection to SQL Server Linux]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true
  [qui]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#requirements-for-certificates
  [1]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#client-initiated-encryption

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

- [Vedere altre informazioni sulla configurazione e la personalizzazione dei contenitori Docker](sql-server-linux-docker-container-configure.md)

- [Risoluzione dei problemi dei contenitori Docker di SQL Server](sql-server-linux-docker-container-troubleshooting.md)
