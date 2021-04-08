---
title: Installare gli strumenti da riga di comando di SQL Server in Linux
titleSuffix: SQL Server
description: Informazioni su come installare gli strumenti da riga di comando di SQL Server, i driver Microsoft ODBC e le rispettive dipendenze in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 04/07/2021
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: eff8e226-185f-46d4-a3e3-e18b7a439e63
ms.openlocfilehash: 436bf5fc0f069a9712d4eb45e51d994ec91cda22
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003901"
---
# <a name="install-sqlcmd-and-bcp-the-sql-server-command-line-tools-on-linux"></a>Installare gli strumenti da riga di comando di SQL Server sqlcmd e bcp in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

I passaggi seguenti installano gli strumenti da riga di comando, i driver Microsoft ODBC e le relative dipendenze. Il pacchetto **mssql-tools** contiene:

- **sqlcmd**: utilità di query della riga di comando.
- **bcp**: utilità di importazione/esportazione in blocco.

Installare gli strumenti per la piattaforma usata:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)
- [macOS](#macos)
- [Docker](#docker)

Questo articolo descrive come installare gli strumenti da riga di comando. Per esempi di come usare **sqlcmd** o **bcp**, vedere i [collegamenti](#next-steps) alla fine di questo argomento.

## <a name="a-idrhelinstall-tools-on-rhel-8"></a><a id="RHEL"><a/>Installare gli strumenti in RHEL 8

Seguire questa procedura per installare **mssql-tools** in Red Hat Enterprise Linux. 

1. Entrare in modalità utente con privilegi avanzati.

   ```bash
   sudo su
   ```

1. Scaricare il file di configurazione del repository Microsoft per Red Hat.

   ```bash
   curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/msprod.repo
   ```

1. Uscire dalla modalità utente con privilegi avanzati.

   ```bash
   exit
   ```

1. Se era già stata installata una versione precedente di **mssql-tools**, rimuovere tutti i pacchetti unixODBC meno recenti.

   ```bash
   sudo yum remove mssql-tools unixODBC-utf16-devel
   ```

1. Eseguire i comandi seguenti per installare **mssql-tools** con il pacchetto per sviluppatori unixODBC.

   ```bash
   sudo yum install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Per effettuare l'aggiornamento alla versione più recente di **mssql-tools**, eseguire i comandi seguenti:
   >    ```bash
   >   sudo yum check-update
   >   sudo yum update mssql-tools
   >   ```

1. **Facoltativo**: aggiungere `/opt/mssql-tools/bin/` alla variabile di ambiente **PATH** in una shell Bash.

   Per rendere **sqlcmd/bcp** accessibile dalla shell Bash per le sessioni di accesso, modificare **PATH** nel file **~/.bash_profile** con il comando seguente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Per rendere **sqlcmd/bcp** accessibile dalla shell Bash per le sessioni interattive/non di accesso, modificare **PATH** nel file **~/.bashrc** con il comando seguente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-ubuntu-1604"></a><a id="ubuntu"></a>Installare gli strumenti in 16.04

Seguire questa procedura per installare **mssql-tools** in Ubuntu.

> [!NOTE]
>
> - Ubuntu 18.04 è supportato a partire da SQL Server 2019 CU3.
> - Ubuntu 20,04 è supportato a partire da SQL Server 2019 CU10 dalla.
> - Se si usa Ubuntu 20,04, modificare il percorso del repository da `/ubuntu/16.04` a `/ubuntu/20.04` .

1. Importare le chiavi GPG del repository pubblico.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrare il repository Microsoft per Ubuntu.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Aggiornare l'elenco di origini ed eseguire il comando di installazione con il pacchetto per sviluppatori unixODBC.

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Per effettuare l'aggiornamento alla versione più recente di **mssql-tools**, eseguire i comandi seguenti:
   >    ```bash
   >   sudo apt-get update 
   >   sudo apt-get install mssql-tools 
   >   ```

1. **Facoltativo**: aggiungere `/opt/mssql-tools/bin/` alla variabile di ambiente **PATH** in una shell Bash.

   Per rendere **sqlcmd/bcp** accessibile dalla shell Bash per le sessioni di accesso, modificare **PATH** nel file **~/.bash_profile** con il comando seguente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Per rendere **sqlcmd/bcp** accessibile dalla shell Bash per le sessioni interattive/non di accesso, modificare **PATH** nel file **~/.bashrc** con il comando seguente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-sles-12"></a><a id="SLES"></a>Installare gli strumenti in SLES 12

Seguire questa procedura per installare **mssql-tools** in SUSE Linux Enterprise Server. 

1. Aggiungere il repository Microsoft SQL Server a Zypper.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Installare **mssql-tools** con il pacchetto per sviluppatori unixODBC.

   ```bash
   sudo zypper install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Per effettuare l'aggiornamento alla versione più recente di **mssql-tools**, eseguire i comandi seguenti:
   >    ```bash
   >   sudo zypper refresh
   >   sudo zypper update mssql-tools
   >   ```

1. **Facoltativo**: aggiungere `/opt/mssql-tools/bin/` alla variabile di ambiente **PATH** in una shell Bash.

   Per rendere **sqlcmd/bcp** accessibile dalla shell Bash per le sessioni di accesso, modificare **PATH** nel file **~/.bash_profile** con il comando seguente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Per rendere **sqlcmd/bcp** accessibile dalla shell Bash per le sessioni interattive/non di accesso, modificare **PATH** nel file **~/.bashrc** con il comando seguente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-macos"></a><a id="macos"></a> Installare gli strumenti in macOS

In macOS è ora disponibile un'anteprima di **sqlcmd** e **bcp**. Per altre informazioni, vedere l'[annuncio](https://blogs.technet.microsoft.com/dataplatforminsider/2017/05/16/sql-server-command-line-tools-for-macos-released/).

*Installare [Homebrew](https://brew.sh) se non è già stato fatto:*

- `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Per installare gli strumenti per Mac El Capitan e Sierra, usare i comandi seguenti:

```bash
# brew untap microsoft/mssql-preview if you installed the preview version 
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install mssql-tools
#for silent install: 
#HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=y brew install mssql-tools
```

## <a name="docker"></a><a id="docker"></a> Docker

Se si [esegue SQL Server in un contenitore Docker](quickstart-install-connect-docker.md), gli strumenti da riga di comando di SQL Server sono già inclusi nell'immagine del contenitore Linux di SQL Server. Se ci si connette a un contenitore in esecuzione con una shell bash interattiva, è possibile eseguire gli strumenti in locale.

## <a name="offline-installation"></a>Installazione offline

[!INCLUDE[SQL Server Linux offline package installation](../includes/sql-server-linux-offline-package-install-intro.md)]

1. Prima di tutto, individuare e copiare il pacchetto **mssql-tools** per la distribuzione di Linux:

   | Distribuzione di Linux | Posizione del pacchetto **mssql-tools** |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/7.3/prod](https://packages.microsoft.com/rhel/7.3/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools) |

1. Individuare e copiare anche il pacchetto **msodbcsql** che è una dipendenza. Anche il pacchetto **msodbcsql** ha una dipendenza da **unixODBC-devel** (Red Hat e SLES) o da **unixodbc-dev** (Ubuntu). Le posizioni dei pacchetti **msodbcsql** sono elencate nella tabella seguente:

   | Distribuzione di Linux | Posizione dei pacchetti ODBC |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/8/prod](https://packages.microsoft.com/rhel/8/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [**msodbcsql**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql)<br/>[**unixodbc-dev**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/u/unixodbc/) |

1. **Spostare i pacchetti scaricati nel computer Linux**. Se è stato usato un computer diverso per scaricare i pacchetti, per spostarli nel computer Linux, è possibile usare il comando **scp**.

1. **Installare i pacchetti**: installare i pacchetti **mssql-tools** e **msodbc**. Se vengono visualizzati errori di dipendenza, ignorarli fino al passaggio successivo.

    | Piattaforma | Comandi di installazione dei pacchetti |
    |-----|-----|
    | Red Hat | `sudo yum localinstall msodbcsql-<version>.rpm`<br/>`sudo yum localinstall mssql-tools-<version>.rpm` |
    | SLES | `sudo zypper install msodbcsql-<version>.rpm`<br/>`sudo zypper install mssql-tools-<version>.rpm` |
    | Ubuntu | `sudo dpkg -i msodbcsql_<version>.deb`<br/>`sudo dpkg -i mssql-tools_<version>.deb` |

1. **Risolvere le dipendenze mancanti**: a questo punto potrebbero mancare alcune dipendenze. In caso contrario, è possibile ignorare questo passaggio. In alcuni casi, è necessario individuare e installare le dipendenze manualmente.

    Per i pacchetti RPM è possibile esaminare le dipendenze necessarie con i comandi seguenti:

    ```bash
    rpm -qpR msodbcsql-<version>.rpm
    rpm -qpR mssql-tools-<version>.rpm
    ```

    Per i pacchetti Debian, se si ha accesso a repository approvati contenenti tali dipendenze, la soluzione più semplice è usare il comando **apt-get**:

    ```bash
    sudo apt-get -f install
    ```

    > [!NOTE]
    > Questo comando completa anche l'installazione dei pacchetti di SQL Server.

    Se questa soluzione non funziona per il pacchetto Debian, è possibile esaminare le dipendenze necessarie con i comandi seguenti:

    ```bash
    dpkg -I msodbcsql_<version>_amd64.deb | grep "Depends:"
    dpkg -I mssql-tools_<version>_amd64.deb | grep "Depends:"
    ```

## <a name="next-steps"></a>Passaggi successivi

Per un esempio di come usare **sqlcmd** per connettersi a SQL Server e creare un database, vedere uno degli argomenti di avvio rapido seguenti:

- [Eseguire l'installazione in Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Eseguire l'installazione in SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Eseguire l'installazione in Ubuntu](quickstart-install-connect-ubuntu.md)
- [Esecuzione in Docker](quickstart-install-connect-ubuntu.md)

Per un esempio di come usare **bcp** per importare ed esportare i dati in blocco, vedere [Eseguire la copia bulk dei dati in SQL Server in Linux](sql-server-linux-migrate-bcp.md).
