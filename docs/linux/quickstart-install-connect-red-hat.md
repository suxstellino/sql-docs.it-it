---
title: 'RHEL: Installare SQL Server in Linux'
titleSuffix: SQL Server
description: Questo avvio rapido illustra come installare SQL Server 2017 o SQL Server 2019 in Red Hat Enterprise Linux (RHEL) e come creare ed eseguire query su un database con sqlcmd.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 92503f59-96dc-4f6a-b1b0-d135c43e935e
ms.openlocfilehash: dc3c04334f1816dbfd681d9fda9139b769339231
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352979"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-red-hat"></a>Avvio rapido: Installare SQL Server e creare un database in Red Hat

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In questa guida di avvio rapido si installerà SQL Server 2017 o SQL Server 2019 in Red Hat Enterprise Linux (RHEL). Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In questa Guida introduttiva si installa SQL Server 2019 in Red Hat Enterprise Linux (RHEL) 7/8. Ci si connette quindi con **sqlcmd** per creare il primo database ed eseguire query.

::: moniker-end

> [!TIP]
> Questa esercitazione richiede l'input dell'utente e una connessione Internet. Se si è interessati alle procedure di installazione [automatica](sql-server-linux-setup.md#unattended) o [offline](sql-server-linux-setup.md#offline), vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prerequisiti

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

È necessario un computer RHEL 7.3 - 7.8 o 8.0 - 8.3 con **almeno 2 GB** di memoria.

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

È necessario un computer RHEL 7.3 - 7.8 o 8.0 - 8.3 con **almeno 2 GB** di memoria.

::: moniker-end

Per installare Red Hat Enterprise Linux nel computer in uso, passare a [https://access.redhat.com/products/red-hat-enterprise-linux/evaluation](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation). È anche possibile creare macchine virtuali RHEL in Azure. Vedere [Creare e gestire VM Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/tutorial-manage-vm) e usare `--image RHEL` nella chiamata a `az vm create`.

Se in precedenza è stata installata una versione CTP o RC di SQL Server, occorre rimuovere il repository precedente prima di eseguire questa procedura. Per altre informazioni, vedere [Configurare i repository Linux per SQL Server 2017 e 2019](sql-server-linux-change-repo.md).

Per altri requisiti di sistema, vedere [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Installare SQL Server

> [!NOTE]
> RHEL 8 è supportato per SQL Server 2017 a partire da CU20. I comandi seguenti per SQL Server 2017 puntano al repository RHEL 8. RHEL 8 non viene preinstallato con python2, richiesto per SQL Server. Prima di iniziare la procedura di installazione di SQL Server, eseguire il comando e verificare che python2 sia selezionato come interprete:
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ```
> Per altre informazioni, vedere il blog seguente sull'installazione di python2 e sulla relativa configurazione come interprete predefinito: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>

Per configurare SQL Server in RHEL, eseguire i comandi seguenti in un terminale per installare il pacchetto **mssql-server**:

1. Scaricare il file di configurazione del repository Microsoft SQL Server 2017 per Red Hat:

   Per RHEL7:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```
   
   Per RHEL8:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo
   ```

   > [!TIP]
   > Se si vuole installare SQL Server 2019, occorre invece registrare il repository di SQL Server 2019. Usare il comando seguente per le installazioni di SQL Server 2019:
   >
   > Per RHEL7:
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   > ```
   >
   > Per RHEL8:
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   > ```

2. Eseguire i comandi seguenti per installare SQL Server:

   ```bash
   sudo yum install -y mssql-server
   ```

3. Al termine dell'installazione del pacchetto, eseguire **mssql-conf setup** e seguire le istruzioni per impostare la password dell'amministratore di sistema e scegliere l'edizione.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Le edizioni di SQL Server 2017 seguenti sono concesse in licenza gratuitamente: Evaluation, Developer ed Express.

   > [!NOTE]
   > Assicurarsi di specificare una password complessa per l'account dell'amministratore di sistema (lunghezza minima di 8 caratteri, con lettere sia maiuscole che minuscole, cifre in base 10 e/o simboli non alfanumerici).

4. Al termine della configurazione, verificare che il servizio sia in esecuzione:

   ```bash
   systemctl status mssql-server
   ```

5. Per consentire connessioni remote, aprire la porta di SQL Server nel firewall in RHEL. La porta di SQL Server predefinita è TCP 1433. Se per il firewall si usa **FirewallD**, è possibile usare i comandi seguenti:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

A questo punto, SQL Server è in esecuzione nel computer RHEL ed è pronto per l'uso.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Installare SQL Server

> [!NOTE]
> I comandi seguenti per SQL Server 2019 puntano al repository RHEL 8. RHEL 8 non viene preinstallato con python2, richiesto per SQL Server. Prima di iniziare la procedura di installazione di SQL Server, eseguire il comando e verificare che python2 sia selezionato come interprete: 
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ``` 
> 
> Per altre informazioni su questa procedura, vedere il blog seguente sull'installazione di python2 e sulla relativa configurazione come interprete predefinito: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
> 

Per configurare SQL Server in RHEL, eseguire i comandi seguenti in un terminale per installare il pacchetto **mssql-server**:

1. Scaricare il file di configurazione del repository di Microsoft SQL Server 2019 per Red Hat:
   
   Per RHEL7:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```
   
   Per RHEL8:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   ```

2. Eseguire i comandi seguenti per installare SQL Server:

   ```bash
   sudo yum install -y mssql-server
   ```

3. Al termine dell'installazione del pacchetto, eseguire **mssql-conf setup** e seguire le istruzioni per impostare la password dell'amministratore di sistema e scegliere l'edizione.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Assicurarsi di specificare una password complessa per l'account dell'amministratore di sistema (lunghezza minima di 8 caratteri, con lettere sia maiuscole che minuscole, cifre in base 10 e/o simboli non alfanumerici).

4. Al termine della configurazione, verificare che il servizio sia in esecuzione:

   ```bash
   systemctl status mssql-server
   ```

5. Per consentire connessioni remote, aprire la porta di SQL Server nel firewall in RHEL. La porta di SQL Server predefinita è TCP 1433. Se per il firewall si usa **FirewallD**, è possibile usare i comandi seguenti:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

SQL Server 2019 è ora in esecuzione nel computer RHEL ed è pronto per l'uso.

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installare gli strumenti da riga di comando di SQL Server

Per creare un database, è necessario connettersi con uno strumento in grado di eseguire istruzioni Transact-SQL nel server SQL. La procedura seguente installa gli strumenti da riga di comando di SQL Server [sqlcmd](../tools/sqlcmd-utility.md) e [bcp](../tools/bcp-utility.md).

1. Scaricare il file di configurazione del repository Microsoft per Red Hat.

   Per RHEL7:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
   ```
   
   Per RHEL8:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
   ```

1. Se era già stata installata una versione precedente di **mssql-tools**, rimuovere tutti i pacchetti unixODBC meno recenti.

   ```bash
   sudo yum remove unixODBC-utf16 unixODBC-utf16-devel
   ```

1. Eseguire i comandi seguenti per installare **mssql-tools** con il pacchetto per sviluppatori unixODBC. Per altre informazioni, vedere [Installare Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo yum install -y mssql-tools unixODBC-devel
   ```

1. Per praticità, aggiungere `/opt/mssql-tools/bin/` alla variabile di ambiente **PATH**. In questo modo è possibile eseguire gli strumenti senza specificare il percorso completo. Eseguire i comandi seguenti per modificare la variabile **PATH** sia per le sessioni di accesso che per le sessioni interattive/non di accesso:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
