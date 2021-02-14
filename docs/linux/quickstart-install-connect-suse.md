---
title: 'SUSE: Installare SQL Server in Linux'
description: Questa guida di avvio rapido illustra come installare SQL Server 2017 o SQL Server 2019 in SUSE Linux Enterprise Server, quindi creare ed eseguire query su un database con sqlcmd.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31ddfb80-f75c-4f51-8540-de6213cb68b8
ms.openlocfilehash: 7aadd757e9e825ace17b69d28f2ab8ecc78123da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340407"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-suse-linux-enterprise-server"></a>Guida introduttiva: Installare SQL Server e creare un database in SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In questa guida di avvio rapido si installerà SQL Server 2017 o SQL Server 2019 in SUSE Linux Enterprise Server (SLES) v12 SP2. Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In questa guida di avvio rapido si installerà SQL Server 2019 in SUSE Linux Enterprise Server (SLES) v12. Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

> [!IMPORTANT]
> SQL Server 2019 è supportato in SUSE Linux Enterprise Server v12 SP2, SP3, SP4 o SP5.

::: moniker-end

> [!TIP]
> Questa esercitazione richiede l'input dell'utente e una connessione Internet. Se si è interessati alle procedure di installazione [automatica](sql-server-linux-setup.md#unattended) o [offline](sql-server-linux-setup.md#offline), vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prerequisiti

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

È necessario un computer SLES v12 SP2 con **almeno 2 GB** di memoria. Il file system deve essere **XFS** o **EXT4**. Altri file system, come **BTRFS**, non sono supportati.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

È necessario un computer SLES v12 SP2, SP3, SP4 o SP5 con **almeno 2 GB** di memoria. Il file system deve essere **XFS** o **EXT4**. Altri file system, come **BTRFS**, non sono supportati.

::: moniker-end

Per installare SUSE Linux Enterprise Server nel computer in uso, accedere a [https://www.suse.com/products/server](https://www.suse.com/products/server). È anche possibile creare macchine virtuali SLES in Azure. Vedere [Creare e gestire VM Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/tutorial-manage-vm) e usare `--image SLES` nella chiamata a `az vm create`.

Se in precedenza è stata installata una versione CTP o RC di SQL Server, occorre rimuovere il repository precedente prima di eseguire questa procedura. Per altre informazioni, vedere [Configurare i repository Linux per SQL Server 2017 e 2019](sql-server-linux-change-repo.md).

> [!NOTE]
> Attualmente il [sottosistema Windows per Linux](/windows/wsl/about) per Windows 10 non è supportato come destinazione dell'installazione.

Per altri requisiti di sistema, vedere [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server-2017"></a><a id="install"></a>Installare SQL Server 2017

Per configurare SQL Server 2017 in SLES, eseguire i comandi seguenti in un terminale per installare il pacchetto **mssql-server**:

1. Scaricare il file di configurazione del repository Microsoft SQL Server 2017 per SLES:

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
   ```

   > [!TIP]
   > Se si vuole installare SQL Server 2019, occorre invece registrare il repository di SQL Server 2019. Usare il comando seguente per le installazioni di SQL Server 2019:
   >
   > ```bash
   > sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   > ```

2. Aggiornare i repository.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```
   
   Per assicurarsi che la chiave di firma del pacchetto Microsoft sia installata sul sistema, importarla usando il comando seguente: 
   
   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Eseguire i comandi seguenti per installare SQL Server:

   ```bash
   sudo zypper install -y mssql-server
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
   systemctl status mssql-server
   ```

6. Se si prevede di connettersi in modalità remota, potrebbe essere necessario aprire anche la porta TCP di SQL Server (per impostazione predefinita, la 1433) sul firewall. Se si usa il firewall SUSE, è necessario modificare il file di configurazione **/etc/sysconfig/SuSEfirewall2**. Modificare la voce **FW_SERVICES_EXT_TCP** in modo che includa il numero di porta di SQL Server.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

A questo punto, SQL Server è in esecuzione nel computer SLES ed è pronto per l'uso.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server-2019"></a><a id="install"></a>Installare SQL Server 2019

Per configurare SQL Server 2019 in SLES, eseguire i comandi seguenti in un terminale per installare il pacchetto **mssql-server**:

1. Scaricare il file di configurazione del repository di Microsoft SQL Server 2019 per SLES:

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   ```

2. Aggiornare i repository.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```

   Per assicurarsi che la chiave di firma del pacchetto Microsoft sia installata nel sistema, usare il comando seguente per importare la chiave: 

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Eseguire i comandi seguenti per installare SQL Server:

   ```bash
   sudo zypper install -y mssql-server
   ```

4. Al termine dell'installazione del pacchetto, eseguire **mssql-conf setup** e seguire le istruzioni per impostare la password dell'amministratore di sistema e scegliere l'edizione.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Assicurarsi di specificare una password complessa per l'account dell'amministratore di sistema (lunghezza minima di 8 caratteri, con lettere sia maiuscole che minuscole, cifre in base 10 e/o simboli non alfanumerici).

5. Al termine della configurazione, verificare che il servizio sia in esecuzione:

   ```bash
   systemctl status mssql-server
   ```

6. Se si prevede di connettersi in modalità remota, potrebbe essere necessario aprire anche la porta TCP di SQL Server (per impostazione predefinita, la 1433) sul firewall. Se si usa il firewall SUSE, è necessario modificare il file di configurazione **/etc/sysconfig/SuSEfirewall2**. Modificare la voce **FW_SERVICES_EXT_TCP** in modo che includa il numero di porta di SQL Server.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

SQL Server 2019 è ora in esecuzione nel computer SLES ed è pronto per l'uso.

::: moniker-end


## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installare gli strumenti da riga di comando di SQL Server

Per creare un database, è necessario connettersi con uno strumento in grado di eseguire istruzioni Transact-SQL nel server SQL. La procedura seguente installa gli strumenti da riga di comando di SQL Server [sqlcmd](../tools/sqlcmd-utility.md) e [bcp](../tools/bcp-utility.md).

1. Aggiungere il repository Microsoft SQL Server a Zypper.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Installare **mssql-tools** con il pacchetto per sviluppatori unixODBC. Per altre informazioni, vedere [Installare Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo zypper install -y mssql-tools unixODBC-devel
   ```

1. Per praticità, aggiungere `/opt/mssql-tools/bin/` alla variabile di ambiente **PATH**. In questo modo è possibile eseguire gli strumenti senza specificare il percorso completo. Eseguire i comandi seguenti per modificare la variabile **PATH** sia per le sessioni di accesso che per le sessioni interattive/non di accesso:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]