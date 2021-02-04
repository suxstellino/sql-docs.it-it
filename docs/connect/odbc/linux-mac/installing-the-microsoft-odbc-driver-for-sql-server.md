---
title: Installare Microsoft ODBC Driver for SQL Server (Linux)
description: Informazioni su come installare Microsoft ODBC Driver for SQL Server nei client Linux per abilitare la connettività del database.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f94b959cdbdf1ce3c32b5f832c630428a51246ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186361"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-linux"></a>Installare Microsoft ODBC Driver for SQL Server (Linux)

Questo articolo illustra come installare Microsoft ODBC Driver for SQL Server in Linux. Sono inoltre incluse le istruzioni per gli strumenti della riga di comando facoltativi per SQL Server (`bcp` e `sqlcmd`) e le intestazioni di sviluppo unixODBC.

Questo articolo fornisce i comandi per l'installazione del driver ODBC dalla shell Bash. Se si vogliono scaricare direttamente i pacchetti, vedere [Scaricare ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

## <a name="microsoft-odbc-17"></a><a id="17"></a> Microsoft ODBC 17

Le sezioni seguenti illustrano come installare Microsoft ODBC Driver 17 dalla shell Bash per diverse distribuzioni Linux.

- [Alpine Linux](#alpine17)
- [Debian](#debian17)
- [Red Hat Enterprise Linux e Oracle](#redhat17)
- [SUSE Linux Enterprise Server](#suse17)
- [Ubuntu](#ubuntu17)

> [!IMPORTANT]
> Se è stato installato il pacchetto `msodbcsql` v17 rimasto disponibile per poco tempo, rimuoverlo prima di installare il pacchetto `msodbcsql17`. Si eviteranno così conflitti. È possibile installare il pacchetto `msodbcsql17` side-by-side con il pacchetto `msodbcsql` v13.

### <a name="alpine-linux"></a><a id="alpine17"></a> Alpine Linux

```bash
#Download the desired package(s)
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.apk
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.apk


#(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.sig
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.sig

curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
gpg --verify msodbcsql17_17.7.1.1-1_amd64.sig msodbcsql17_17.7.1.1-1_amd64.apk
gpg --verify mssql-tools_17.7.1.1-1_amd64.sig mssql-tools_17.7.1.1-1_amd64.apk


#Install the package(s)
sudo apk add --allow-untrusted msodbcsql17_17.7.1.1-1_amd64.apk
sudo apk add --allow-untrusted mssql-tools_17.7.1.1-1_amd64.apk
```

> [!NOTE]
> È necessaria la versione del driver 17.5 o versione successiva per il supporto di Alpine.

### <a name="debian"></a><a id="debian17"></a> Debian

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install libgssapi-krb5-2
```

> [!NOTE]
> È possibile sostituire l'impostazione della variabile di ambiente 'ACCEPT_EULA' impostando invece la variabile 'msodbcsql/ACCEPT_EULA': `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

### <a name="red-hat-enterprise-server-and-oracle-linux"></a><a id="redhat17"></a> Red Hat Enterprise Server e Oracle Linux

```bash
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7 and Oracle Linux 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 8 and Oracle Linux 8
curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server"></a><a id="suse17"></a> SUSE Linux Enterprise Server

```bash
sudo su
curl -O https://packages.microsoft.com/keys/microsoft.asc
rpm --import microsoft.asc

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
#Ensure SUSE Linux Enterprise 11 Security Module has been installed 
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

#SUSE Linux Enterprise Server 15
zypper ar https://packages.microsoft.com/config/sles/15/prod.repo
#(Only for driver 17.3 and below)
SUSEConnect -p sle-module-legacy/15/x86_64

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu"></a><a id="ubuntu17"></a> Ubuntu

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 18.04
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.04
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.10
curl https://packages.microsoft.com/config/ubuntu/20.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

> [!NOTE]
> - È necessaria la versione del driver 17.2 o versione successiva per il supporto di Ubuntu 18.04.
> - È necessaria la versione del driver 17.3 o versione successiva per il supporto di Ubuntu 18.10.

> [!NOTE]
> È possibile sostituire l'impostazione della variabile di ambiente 'ACCEPT_EULA' impostando invece la variabile 'msodbcsql/ACCEPT_EULA': `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

## <a name="previous-versions"></a>Versioni precedenti

Nelle sezioni seguenti vengono fornite le istruzioni per l'installazione delle versioni precedenti di Microsoft ODBC Driver in Linux. Le informazioni riguardano le versioni seguenti del driver:

- [Microsoft ODBC Driver 13.1 for SQL Server](#13.1)
- [Microsoft ODBC Driver 13 for SQL Server](#13)
- [Microsoft ODBC Driver 11 for SQL Server](#11)

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

Le sezioni seguenti illustrano come installare Microsoft ODBC Driver 13.1 dalla shell Bash per diverse distribuzioni Linux.

### <a name="debian-8"></a>Debian 8

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

## <a name="odbc-13"></a><a id="13"></a> ODBC 13

Le sezioni seguenti illustrano come installare Microsoft ODBC Driver 13 dalla shell Bash per diverse distribuzioni Linux.

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>Installazione offline

Se si preferisce o si deve installare [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 in un computer senza connessione Internet, sarà necessario risolvere manualmente le dipendenze del pacchetto. [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 presenta le dipendenze dirette seguenti:
- Ubuntu: libc6 (>= 2.21), libstdc++6 (>= 4.9), libkrb5-3, libcurl3, openssl, debconf (>= 0.5), unixodbc (>= 2.3.1-1)
- Red Hat: ```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SUSE: ```glibc, libuuid1, krb5, openssl, unixODBC```

Ognuno di questi pacchetti ha a sua volta le proprie dipendenze, che potrebbero o meno essere presenti nel sistema. Per una soluzione generale di questo problema, fare riferimento alla documentazione della gestione pacchetti per la distribuzione: [Redhat](https://wiki.centos.org/HowTos/CreateLocalRepos), [Ubuntu](https://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian) e [SUSE](https://en.opensuse.org/Portal:Zypper)

È anche una pratica comune scaricare manualmente tutti i pacchetti dipendenti, inserirli nel computer di installazione, quindi installare manualmente ogni pacchetto e terminare con il pacchetto [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13.

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7

- Scaricare la versione più recente di `msodbcsql` `.rpm` da [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/).
- Installare le dipendenze e il driver.
  
```bash
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04

- Scaricare la versione più recente di `msodbcsql` `.deb` da [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/).
- Installare le dipendenze e il driver.

```bash
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

- Scaricare la versione più recente di `msodbcsql` `.rpm` da [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/).
- Installare le dipendenze e il driver.

```bash
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

Dopo aver completato l'installazione del pacchetto, è possibile verificare che [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 trovi tutte le relative dipendenze. Eseguire ldd e verificare l'output per individuare le librerie mancanti:

```bash
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```

## <a name="odbc-11"></a><a id="11"></a> ODBC 11

Le sezioni seguenti illustrano come installare Microsoft ODBC Driver 11 in Linux. Prima di poter usare il driver, installare Gestione driver unixODBC. Per altre informazioni, vedere [Installazione di Gestione driver](../../../connect/odbc/linux-mac/installing-the-driver-manager.md).

### <a name="installation-steps"></a>Procedura di installazione  

> [!IMPORTANT]  
> Queste istruzioni fanno riferimento a `msodbcsql-11.0.2270.0.tar.gz`, il file di installazione per Red Hat Linux. Se si sta installando l'anteprima per SUSE Linux, il nome del file è `msodbcsql-11.0.2260.0.tar.gz`.  
  
Per installare il driver:

1. Assicurarsi di disporre dell'autorizzazione di radice.  

2. Passare alla directory in cui il download ha inserito il file `msodbcsql-11.0.2270.0.tar.gz`. Verificare di avere il file \*.tar.gz corrispondente alla versione di Linux in uso. Per estrarre i file, eseguire il comando seguente, `tar xvzf msodbcsql-11.0.2270.0.tar.gz`.  
  
3. Passare alla directory `msodbcsql-11.0.2270.0` contenente un file denominato **install.sh**.  
  
4. Per visualizzare un elenco delle opzioni di installazione disponibili, eseguire il comando seguente: **./install.sh**  
  
5. Eseguire il backup di **odbcinst.ini**. L'installazione del driver aggiorna **odbcinst.ini**. odbcinst.ini contiene l'elenco dei driver registrati in Gestione driver unixODBC. Per individuare il percorso di odbcinst.ini nel computer in uso, eseguire il comando seguente: ```odbc_config --odbcinstini```.  
  
6. Prima di installare il driver, eseguire il comando seguente: `./install.sh verify`. L'output di `./install.sh verify` segnala se il computer dispone del software necessario per supportare il driver ODBC in Linux.  
  
7. Quando si è pronti a installare il driver ODBC in Linux, eseguire il comando seguente: `./install.sh install`. Se è necessario specificare un comando di installazione (`bin-dir` o `lib-dir`), specificare il comando dopo l'opzione **installa**.  
  
8. Dopo aver esaminato il contratto di licenza, digitare **YES** per continuare l'installazione.  
  
L'installazione inserisce il driver in `/opt/microsoft/msodbcsql/11.0.2270.0`. Il driver e i relativi file di supporto devono trovarsi in `/opt/microsoft/msodbcsql/11.0.2270.0`.  
  
Per verificare che il driver ODBC Microsoft sia stato registrato in Linux, eseguire il comando seguente: ```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```.  
  
### <a name="uninstall"></a>Disinstallare  
  
È possibile disinstallare il driver ODBC 11 in Linux eseguendo i comandi seguenti:  
  
1. `rm -f /usr/bin/sqlcmd`
  
2. `rm -f /usr/bin/bcp`  
  
3. `rm -rf /opt/microsoft/msodbcsql`  
  
4. `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="driver-files"></a>File del driver

Il driver ODBC in Linux include i componenti seguenti:

|Componente|Descrizione|  
|---------------|-----------------|  
|libmsodbcsql-17.X.so.X.X o libmsodbcsql-13.X.so.X.X|File della libreria di collegamento dinamico dell'oggetto condiviso (`so`) che contiene tutte le funzionalità del driver. Questo file viene installato in `/opt/microsoft/msodbcsql17/lib64/` per Driver 17 e in `/opt/microsoft/msodbcsql/lib64/` per Driver 13.|  
|`msodbcsqlr17.rll` o `msodbcsqlr13.rll`|File di risorse associato per la libreria del driver. Questo file viene installato in `[driver .so directory]../share/resources/en_US/`| 
|msodbcsql.h|File di intestazione che contiene tutte le nuove definizioni necessarie per usare il driver.<br /><br /> **Nota:**  non è possibile fare riferimento a msodbcsql.h e a odbcss.h nello stesso programma.<br /><br /> msodbcsql.h viene installato in `/opt/microsoft/msodbcsql17/include/` per Driver 17 e in `/opt/microsoft/msodbcsql/include/` per Driver 13. |
|LICENSE.txt|File di testo che contiene i termini del contratto di licenza con l'utente finale. Questo file viene inserito in `/usr/share/doc/msodbcsql17/` per Driver 17 e in `/usr/share/doc/msodbcsql/` per Driver 13.|
|RELEASE_NOTES|File di testo che contiene le note sulla versione. Questo file viene inserito in `/usr/share/doc/msodbcsql17/` per Driver 17 e in `/usr/share/doc/msodbcsql/` per Driver 13.|

## <a name="resource-file-loading"></a>Caricamento del file di risorse

Perché possa funzionare, il driver deve caricare il file di risorse. Questo file è denominato `msodbcsqlr17.rll` o `msodbcsqlr13.rll` a seconda della versione del driver. La posizione del file `.rll` dipende dalla posizione del driver stesso (`so` o `dylib`), come indicato nella tabella precedente. A partire dalla versione 17.1 il driver tenterà anche di caricare il file `.rll` dalla directory predefinita se il caricamento dal percorso relativo non riesce. Il percorso del file di risorse predefinito in Linux è `/opt/microsoft/msodbcsql17/share/resources/en_US/`.

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se non è possibile effettuare una connessione a SQL Server tramite il driver ODBC, vedere l'articolo sui problemi noti dedicato alla [risoluzione dei problemi di connessione](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver installato il driver, è possibile provare l'[applicazione di esempio ODBC C++](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Per altre informazioni sullo sviluppo di applicazioni ODBC, vedere [Sviluppo di applicazioni](../../../odbc/reference/develop-app/developing-applications.md).

Per altre informazioni, vedere le [note sulla versione](release-notes-odbc-sql-server-linux-mac.md) e i [requisiti di sistema](system-requirements.md) del driver ODBC.
