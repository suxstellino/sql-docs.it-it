---
title: Installazione automatica per SQL Server in SUSE Linux Enterprise Server
titleSuffix: SQL Server
description: Usare uno script Bash di esempio per installare SQL Server 2017 in SUSE Linux Enterprise Server (SLES) v12 SP2 senza input interattivo.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 410a4c1fcb378f4d93730721ecfc0416564794bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339141"
---
# <a name="sample-unattended-sql-server-installation-script-for-suse-linux-enterprise-server"></a>Esempio: Script di installazione automatica di SQL Server per SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo script Bash di esempio installa SQL Server 2017 in SUSE Linux Enterprise Server (SLES) v12 SP2 senza input interattivo. Fornisce esempi di installazione del motore di database, gli strumenti da riga di comando SQL Server e SQL Server Agent ed esegue le operazioni successive all'installazione. Facoltativamente, è possibile installare la ricerca full-text e creare un utente amministratore.

> [!TIP]
> Se non è necessario uno script di installazione automatica, il modo più rapido per installare SQL Server consiste nel seguire la [guida di avvio rapido per SLES](quickstart-install-connect-suse.md). Per altre informazioni sull'installazione, vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prerequisiti

- Per eseguire SQL Server in Linux sono necessari almeno 2 GB di memoria.
- Il file system deve essere **XFS** o **EXT4**. Altri file system, come **BTRFS**, non sono supportati.
- Per altri requisiti di sistema, vedere [Requisiti di sistema per SQL Server su Linux](sql-server-linux-setup.md#system).

> [!IMPORTANT]
> SQL Server 2017 richiede libsss_nss_idmap0, che non viene fornito dai repository SLES predefiniti. È possibile installarlo da SLES v12 SP2 SDK.

## <a name="sample-script"></a>Script di esempio

```bash
#!/bin/bash -e

# Use the following variables to control your install:

# Password for the SA user (required)
MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'

# Product ID of the version of SQL server you're installing
# Must be evaluation, developer, express, web, standard, enterprise, or your 25 digit product key
# Defaults to developer
MSSQL_PID='evaluation'

# Install SQL Server Agent (recommended)
SQL_INSTALL_AGENT='y'

# Install SQL Server Full Text Search (optional)
# SQL_INSTALL_FULLTEXT='y'

# Create an additional user with sysadmin privileges (optional)
# SQL_INSTALL_USER='<Username>'
# SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'

if [ -z $MSSQL_SA_PASSWORD ]
then
  echo Environment variable MSSQL_SA_PASSWORD must be set for unattended install
  exit 1
fi

echo Adding Microsoft repositories...
sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
sudo zypper --gpg-auto-import-keys refresh

#Add the SLES v12 SP2 SDK to obtain libsss_nss_idmap0
sudo SUSEConnect -p sle-sdk/12.2/x86_64

echo Installing SQL Server...
sudo zypper install -y mssql-server

echo Running mssql-conf setup...
sudo MSSQL_SA_PASSWORD=$MSSQL_SA_PASSWORD \
     MSSQL_PID=$MSSQL_PID \
     /opt/mssql/bin/mssql-conf -n setup accept-eula

echo Installing mssql-tools and unixODBC developer...
sudo ACCEPT_EULA=Y zypper install -y mssql-tools unixODBC-devel

# Add SQL Server tools to the path by default:
echo Adding SQL Server tools to your path...
echo PATH="$PATH:/opt/mssql-tools/bin" >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc

# Optional SQL Server Agent installation:
if [ ! -z $SQL_INSTALL_AGENT ]
then
  echo Installing SQL Server Agent...
  sudo zypper install -y mssql-server-agent
fi

# Optional SQL Server Full Text Search installation:
if [ ! -z $SQL_INSTALL_FULLTEXT ]
then
    echo Installing SQL Server Full-Text Search...
    sudo zypper install -y mssql-server-fts
fi

# Configure firewall to allow TCP port 1433:
echo Configuring SuSEfirewall2 to allow traffic on port 1433...
sudo SuSEfirewall2 open INT TCP 1433
sudo SuSEfirewall2 stop
sudo SuSEfirewall2 start

# Example of setting post-installation configuration options
# Set trace flags 1204 and 1222 for deadlock tracing:
# echo Setting trace flags...
# sudo /opt/mssql/bin/mssql-conf traceflag 1204 1222 on

# Restart SQL Server after making configuration changes:
echo Restarting SQL Server...
sudo systemctl restart mssql-server

# Connect to server and get the version:
counter=1
errstatus=1
while [ $counter -le 5 ] && [ $errstatus = 1 ]
do
  echo Waiting for SQL Server to start...
  sleep 5s
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "SELECT @@VERSION" 2>/dev/null
  errstatus=$?
  ((counter++))
done

# Display error if connection failed:
if [ $errstatus = 1 ]
then
  echo Cannot connect to SQL Server, installation aborted
  exit $errstatus
fi

# Optional new user creation:
if [ ! -z $SQL_INSTALL_USER ] && [ ! -z $SQL_INSTALL_USER_PASSWORD ]
then
  echo Creating user $SQL_INSTALL_USER
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "CREATE LOGIN [$SQL_INSTALL_USER] WITH PASSWORD=N'$SQL_INSTALL_USER_PASSWORD', DEFAULT_DATABASE=[master], CHECK_EXPIRATION=ON, CHECK_POLICY=ON; ALTER SERVER ROLE [sysadmin] ADD MEMBER [$SQL_INSTALL_USER]"
fi

echo Done!
```

### <a name="running-the-script"></a>Esecuzione dello script

Per eseguire lo script

1. Incollare l'esempio nell'editor di testo preferito e salvarlo con un nome facile da ricordare, ad esempio `install_sql.sh`.

1. Personalizzare `MSSQL_SA_PASSWORD`, `MSSQL_PID` e tutte le altre variabili che si vuole modificare.

1. Contrassegnare lo script come eseguibile

   ```bash
   chmod +x install_sql.sh
   ```

1. Eseguire lo script

   ```bash
   ./install_sql.sh
   ```

### <a name="understanding-the-script"></a>Informazioni sullo script
La prima operazione eseguita dallo script Bash è l'impostazione di alcune variabili. Possono essere variabili di scripting, come nell'esempio, o variabili di ambiente. La variabile `MSSQL_SA_PASSWORD` è **obbligatoria** per l'installazione di SQL Server, mentre le altre sono variabili personalizzate create per lo script. Lo script di esempio esegue le operazioni seguenti:

1. Importare le chiavi di Microsoft GPG pubbliche.

1. Registrare i repository Microsoft per SQL Server e gli strumenti da riga di comando.

1. Aggiornare i repository locali.

1. Installare SQL Server

1. Configurare SQL Server con ```MSSQL_SA_PASSWORD``` e accettare automaticamente il contratto di licenza con l'utente finale.

1. Accettare automaticamente il contratto di licenza con l'utente finale per gli strumenti da riga di comando di SQL Server, installarli e installare il pacchetto unixodbc-dev.

1. Aggiungere gli strumenti da riga di comando di SQL Server al percorso per facilitarne l'uso.

1. Installare SQL Server Agent se la variabile di scripting ```SQL_INSTALL_AGENT``` è abilitata per impostazione predefinita.

1. Facoltativamente, installare la ricerca full-text di SQL Server, se la variabile ```SQL_INSTALL_FULLTEXT``` è impostata.

1. Sbloccare la porta 1433 per TCP sul firewall di sistema, necessaria per connettersi a SQL Server da un altro sistema.

1. Facoltativamente, impostare i flag di traccia per la traccia dei deadlock (è necessario rimuovere i commenti dalle righe).

1. SQL Server ora è installato. Per renderlo operativo, riavviare il processo.

1. Verificare che SQL Server sia installato correttamente, nascondendo eventuali messaggi di errore.

1. Creare un nuovo utente amministratore del server se ```SQL_INSTALL_USER``` e ```SQL_INSTALL_USER_PASSWORD``` sono entrambi impostati.

## <a name="next-steps"></a>Passaggi successivi

Semplificare più installazioni automatiche e creare uno script Bash autonomo che imposta le variabili di ambiente appropriate. È possibile rimuovere le variabili usate dallo script di esempio e inserirle nel proprio script Bash.

```bash
#!/bin/bash
export MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'
export MSSQL_PID='evaluation'
export SQL_INSTALL_AGENT='y'
export SQL_INSTALL_USER='<Username>'
export SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'
export SQL_INSTALL_AGENT='y'
```

Eseguire quindi lo script Bash come indicato di seguito:
```bash
. ./my_script_name.sh
```

Per altre informazioni su SQL Server in Linux, vedere la [panoramica di SQL Server in Linux](sql-server-linux-overview.md).
