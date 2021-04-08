---
title: 'Ubuntu: Installare SQL Server in Linux'
description: Questa guida di avvio rapido illustra come installare SQL Server 2017 o SQL Server 2019 in Ubuntu e quindi come creare ed eseguire query su un database con sqlcmd.
author: VanMSFT
ms.author: vanto
ms.date: 04/07/2021
ms.topic: conceptual
ms.prod: sql
ms.custom: seo-lt-2019
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: 4d96b75d35af95f646ba41864ee9df846642338f
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003917"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-ubuntu"></a>Guida introduttiva: Installare SQL Server e creare un database in Ubuntu
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]


<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In questa Guida introduttiva si installa SQL Server 2017 in Ubuntu 16.04/18.04. Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

> [!TIP]
> Questa esercitazione richiede l'input dell'utente e una connessione Internet. Se si è interessati alle procedure di installazione automatica o offline, vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md). Per un elenco delle piattaforme supportate, vedere le [Note sulla versione](sql-server-linux-release-notes.md).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In questa Guida introduttiva si installa SQL Server 2019 in Ubuntu 16,04, 18,04 o 20,04. Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

> [!NOTE]
> Ubuntu 20,04 è supportato a partire da SQL Server 2019 CU10 dalla.

> [!TIP]
> Questa esercitazione richiede l'input dell'utente e una connessione Internet. Se si è interessati alle procedure di installazione automatica o offline, vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md). Per un elenco delle piattaforme supportate, vedere le [Note sulla versione](sql-server-linux-release-notes-2019.md).

::: moniker-end

## <a name="prerequisites"></a>Prerequisiti

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

È necessario un computer Ubuntu 16.04 o 18.04 con **almeno 2 GB** di memoria.

Per installare Ubuntu 18.04 nel computer in uso, passare a <http://releases.ubuntu.com/bionic/>. È anche possibile creare macchine virtuali Ubuntu in Azure. Vedere [Creare e gestire VM Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> Attualmente il [sottosistema Windows per Linux](/windows/wsl/about) per Windows 10 non è supportato come destinazione dell'installazione.

Per altri requisiti di sistema, vedere [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

> [!NOTE]
> Ubuntu 18.04 è supportato a partire da SQL Server 2017 CU20. Se si vogliono usare le istruzioni in questo articolo con Ubuntu 18,04, assicurarsi di usare il [percorso corretto](sql-server-linux-change-repo.md), `18.04` anziché `16.04`.
>
> Se si esegue SQL Server in una versione precedente, la configurazione è possibile adottando [modifiche](/archive/blogs/sql_server_team/installing-sql-server-2017-for-linux-on-ubuntu-18-04-lts).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

È necessario disporre di un computer Ubuntu 16,04, 18,04 o 20,04 con almeno **2 GB** di memoria.

Per installare Ubuntu 20,04 nel computer in uso, passare a <https://releases.ubuntu.com/20.04/> . È anche possibile creare macchine virtuali Ubuntu in Azure. Vedere [Creare e gestire VM Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> Attualmente il [sottosistema Windows per Linux](/windows/wsl/about) per Windows 10 non è supportato come destinazione dell'installazione.

Per altri requisiti di sistema, vedere [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Installare SQL Server

> [!NOTE]
> I comandi seguenti per SQL Server 2017 puntano al repository Ubuntu 18.04. Se si usa Ubuntu 16.04, modificare il percorso riportato di seguito sostituendo `/ubuntu/18.04/` con `/ubuntu/16.04/`.

Per configurare SQL Server in Ubuntu e installare il pacchetto **mssql-server**, eseguire i comandi seguenti in un terminale.

1. Importare le chiavi GPG del repository pubblico:

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Registrare il repository Microsoft SQL Server per Ubuntu:
   
   Per Ubuntu 16,04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```
   
   Per Ubuntu 18,04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"
   ```

   > [!TIP]
   > Se si vuole installare SQL Server 2019, occorre invece registrare il repository di SQL Server 2019. Usare il comando seguente per le installazioni di SQL Server 2019:
   >
   > Per Ubuntu 16,04:
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   > ```
   >
   > Per Ubuntu 18,04:
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   > ```

3. Eseguire i comandi seguenti per installare SQL Server:

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Al termine dell'installazione del pacchetto, eseguire **mssql-conf setup** e seguire le istruzioni per impostare la password dell'amministratore di sistema e scegliere l'edizione.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Le edizioni di SQL Server 2017 seguenti sono concesse in licenza gratuitamente: Evaluation, Developer ed Express.

   > [!NOTE]
   > Assicurarsi di specificare una password complessa per l'account dell'amministratore di sistema (lunghezza minima di 8 caratteri, con lettere sia maiuscole che minuscole, cifre in base 10 e/o simboli non alfanumerici).

5. Al termine della configurazione, verificare che il servizio sia in esecuzione:

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Se si prevede di connettersi in modalità remota, potrebbe essere necessario aprire anche la porta TCP di SQL Server (per impostazione predefinita, la 1433) sul firewall.

A questo punto, SQL Server è in esecuzione nel computer Ubuntu ed è pronto per l'uso.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Installare SQL Server

> [!NOTE]
> I comandi seguenti per SQL Server 2019 puntano al repository Ubuntu 20,04. Se si usa Ubuntu 18,04 o 16,04, modificare il percorso seguente in `/ubuntu/18.04/` o `/ubuntu/16.04/` anziché `/ubuntu/20.04/` .

Per configurare SQL Server in Ubuntu e installare il pacchetto **mssql-server**, eseguire i comandi seguenti in un terminale.

1. Importare le chiavi GPG del repository pubblico:

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Registrare il repository di Microsoft SQL Server per Ubuntu per SQL Server 2019:
   
   Per Ubuntu 16,04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```
   
   Per Ubuntu 18,04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   ```

   Per Ubuntu 20,04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/mssql-server-2019.list)"
   ```

3. Eseguire i comandi seguenti per installare SQL Server:

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Al termine dell'installazione del pacchetto, eseguire **mssql-conf setup** e seguire le istruzioni per impostare la password dell'amministratore di sistema e scegliere l'edizione.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Assicurarsi di specificare una password complessa per l'account dell'amministratore di sistema (lunghezza minima di 8 caratteri, con lettere sia maiuscole che minuscole, cifre in base 10 e/o simboli non alfanumerici).

5. Al termine della configurazione, verificare che il servizio sia in esecuzione:

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Se si prevede di connettersi in modalità remota, potrebbe essere necessario aprire anche la porta TCP di SQL Server (per impostazione predefinita, la 1433) sul firewall.

SQL Server 2019 è ora in esecuzione nel computer Ubuntu ed è pronto per l'uso.

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installare gli strumenti da riga di comando di SQL Server

Per creare un database, è necessario connettersi con uno strumento in grado di eseguire istruzioni Transact-SQL nel server SQL. La procedura seguente installa gli strumenti da riga di comando di SQL Server [sqlcmd](../tools/sqlcmd-utility.md) e [bcp](../tools/bcp-utility.md).

Seguire questa procedura per installare **mssql-tools** in Ubuntu. 

1. Importare le chiavi GPG del repository pubblico.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrare il repository Microsoft per Ubuntu.
   
   Per Ubuntu 16,04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

   Per Ubuntu 18,04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

   Per Ubuntu 20,04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Aggiornare l'elenco di origini ed eseguire il comando di installazione con il pacchetto per sviluppatori unixODBC. Per altre informazioni, vedere [Installare Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Per effettuare l'aggiornamento alla versione più recente di **mssql-tools**, eseguire i comandi seguenti:
   >
   > ```bash
   > sudo apt-get update 
   > sudo apt-get install mssql-tools 
   > ```

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

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
