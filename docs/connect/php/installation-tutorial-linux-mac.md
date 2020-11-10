---
title: Installazione dei driver per PHP in Linux e macOS
description: In queste istruzioni viene illustrato come installare i driver Microsoft per PHP per SQL Server in Linux o macOS.
ms.date: 10/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
manager: v-mabarw
ms.openlocfilehash: 66c505f588d6f250c0e18dc88a79b21ed658f2b5
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243734"
---
# <a name="linux-and-macos-installation-tutorial-for-the-microsoft-drivers-for-php-for-sql-server"></a>Esercitazione sull'installazione in Linux e macOS dei driver Microsoft per PHP per SQL Server
Le istruzioni seguenti presuppongono che venga usato un ambiente pulito e illustrano come installare PHP 7.x, il driver Microsoft ODBC, il server Web Apache e i driver Microsoft per PHP per SQL Server in Ubuntu 16.04, 18.04 e 20.04, Red Hat 7 e 8, Debian 8, 9 e 10, Suse 12 e 15, Alpine 3.11 e macOS 10.13, 10.14 e 10.15. I consigli inclusi in queste istruzioni riguardano l'installazione dei driver con PECL, ma è anche possibile scaricare i file binari precompilati dalla pagina del progetto GitHub relativo ai [driver Microsoft per PHP per SQL Server](https://github.com/Microsoft/msphpsql/releases) e installare tali file seguendo le istruzioni riportate in [Caricamento dei driver Microsoft per PHP per SQL Server](../../connect/php/loading-the-php-sql-driver.md). Per una spiegazione del caricamento delle estensioni e dei motivi per cui non si aggiungeranno le estensioni a php.ini, vedere la sezione relativa al [caricamento dei driver](../../connect/php/loading-the-php-sql-driver.md#loading-the-driver-at-php-startup).

Per impostazione predefinita, con queste istruzioni viene installato PHP 7.4 usando `pecl install`. Potrebbe essere necessario eseguire prima `pecl channel-update pecl.php.net`. Alcune distribuzioni Linux supportate usano per impostazione predefinita PHP 7.1 o versioni precedenti, non supportate per la versione più recente dei driver PHP per SQL Server. Vedere le note all'inizio di ogni sezione per installare PHP 7.2 o 7.3.

Sono incluse anche istruzioni per l'installazione di PHP FastCGI Process Manager, PHP-FPM, in Ubuntu. Questa operazione è necessaria se si usa il server web nginx invece di Apache.

Anche se queste istruzioni contengono comandi per l'installazione di entrambi i driver SQLSRV e PDO_SQLSRV, i driver possono essere installati e funzionano in modo indipendente. Gli utenti che hanno familiarità con la personalizzazione della configurazione possono modificare queste istruzioni in modo che siano specifiche per SQLSRV o PDO_SQLSRV. Entrambi i driver hanno le stesse dipendenze, tranne nei casi indicati di seguito.

## <a name="contents-of-this-page"></a>Contenuto della pagina

- [Installazione dei driver in Ubuntu 16.04, 18.04 e 20.04](#installing-the-drivers-on-ubuntu-1604-1804-and-2004)
- [Installazione dei driver con PHP-FPM in Ubuntu](#installing-the-drivers-with-php-fpm-on-ubuntu)
- [Installazione dei driver in Red Hat 7 e 8](#installing-the-drivers-on-red-hat-7-and-8)
- [Installazione dei driver in Debian 8, 9 e 10](#installing-the-drivers-on-debian-8-9-and-10)
- [Installazione dei driver in Suse 12 e 15](#installing-the-drivers-on-suse-12-and-15)
- [Installazione dei driver in Alpine 3.11](#installing-the-drivers-on-alpine-311)
- [Installazione dei driver in macOS Sierra, High Sierra, Mojave e Catalina](#installing-the-drivers-on-macos-high-sierra-mojave-and-catalina)

## <a name="installing-the-drivers-on-ubuntu-1604-1804-and-2004"></a>Installazione dei driver in Ubuntu 16.04, 18.04 e 20.04

> [!NOTE]
> Per installare PHP 7.2 o 7.3, sostituire 7.4 con 7.2 o 7.3 nei comandi seguenti.

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP
```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php7.4 php7.4-dev php7.4-xml -y --allow-unauthenticated
```
### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per Ubuntu seguendo le istruzioni riportate nell'[articolo relativo all'installazione in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.4/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.4/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 7.4 sqlsrv pdo_sqlsrv
```

Se nel sistema è presente una sola versione di PHP, l'ultimo passaggio può essere semplificato come `phpenmod sqlsrv pdo_sqlsrv`.

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Passaggio 4. Installare Apache e configurare il caricamento dei driver
```bash
sudo su
apt-get install libapache2-mod-php7.4 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php7.4
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Passaggio 5. Riavviare Apache e testare lo script di esempio
```bash
sudo service apache2 restart
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.

## <a name="installing-the-drivers-with-php-fpm-on-ubuntu"></a>Installazione dei driver con PHP-FPM in Ubuntu

> [!NOTE]
> Per installare PHP 7.2 o 7.3, sostituire 7.4 con 7.2 o 7.3 nei comandi seguenti.

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP
```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php7.4 php7.4-dev php7.4-xml php7.4-fpm -y --allow-unauthenticated
```
Verificare lo stato del servizio PHP-FPM eseguendo
```bash
systemctl status php7.4-fpm
```
### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per Ubuntu seguendo le istruzioni riportate nell'[articolo relativo all'installazione in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
```bash
sudo pecl config-set php_ini /etc/php/7.4/fpm/php.ini
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.4/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.4/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 7.4 sqlsrv pdo_sqlsrv
```
Se nel sistema è presente una sola versione di PHP, l'ultimo passaggio può essere semplificato come `phpenmod sqlsrv pdo_sqlsrv`.

Verificare che `sqlsrv.ini` e `pdo_sqlsrv.ini` si trovino in `/etc/php/7.4/fpm/conf.d/`:
```bash
ls /etc/php/7.4/fpm/conf.d/*sqlsrv.ini
```
Riavviare il servizio PHP-FPM:
```bash
sudo systemctl restart php7.4-fpm
```

### <a name="step-4-install-and-configure-nginx"></a>Passaggio 4. Installare e configurare nginx
```bash
sudo apt-get update
sudo apt-get install nginx
sudo systemctl status nginx
```
Per configurare nginx è necessario modificare il file `/etc/nginx/sites-available/default`. Aggiungere `index.php` all'elenco sotto la sezione `# Add index.php to the list if you are using PHP`:
```
# Add index.php to the list if you are using PHP
index index.html index.htm index.nginx-debian.html index.php;
```
Quindi modificare la sezione che segue `# pass PHP scripts to FastCGI server` come indicato di seguito:
```
# pass PHP scripts to FastCGI server
#
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
}
```
### <a name="step-5-restart-nginx-and-test-the-sample-script"></a>Passaggio 5. Riavviare nginx e testare lo script di esempio
```bash
sudo systemctl restart nginx.service
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.

## <a name="installing-the-drivers-on-red-hat-7-and-8"></a>Installazione dei driver in Red Hat 7 e 8

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP

Per installare PHP in Red Hat 7 eseguire il comando seguente:
> [!NOTE]
> Per installare PHP 7.2 o 7.3, sostituire remi-php74 rispettivamente con remi-php72 o remi-php73 nei comandi seguenti.
```bash
sudo su
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
subscription-manager repos --enable=rhel-7-server-optional-rpms
yum install yum-utils
yum-config-manager --enable remi-php74
yum update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
yum install php php-pdo php-xml php-pear php-devel re2c gcc-c++ gcc
```

Per installare PHP in Red Hat 8 eseguire il comando seguente:
> [!NOTE]
> Per installare PHP 7.2 o 7.3, sostituire remi-7.4 rispettivamente con remi-7.2 o remi-7.3 nei comandi seguenti.
```bash
sudo su
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf install yum-utils
dnf module reset php
dnf module install php:remi-7.4
subscription-manager repos --enable codeready-builder-for-rhel-8-x86_64-rpms
dnf update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
dnf install php-pdo php-pear php-devel
```

### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per Red Hat 7 o 8 seguendo le istruzioni riportate nell'[articolo relativo all'installazione in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20-sqlsrv.ini
exit
```

In alternativa è possibile eseguire l'installazione dal repository Remi:
```bash
sudo yum install php-sqlsrv
```
### <a name="step-4-install-apache"></a>Passaggio 4. Installare Apache
```bash
sudo yum install httpd
```
SELinux viene installato per impostazione predefinita ed eseguito in modalità Enforcing. Per consentire la connessione di Apache ai database tramite SELinux, eseguire questo comando:
```bash
sudo setsebool -P httpd_can_network_connect_db 1
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Passaggio 5. Riavviare Apache e testare lo script di esempio
```bash
sudo apachectl restart
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.

## <a name="installing-the-drivers-on-debian-8-9-and-10"></a>Installazione dei driver in Debian 8, 9 e 10

> [!NOTE]
> Per installare PHP 7.2 o 7.3, sostituire 7.4 con 7.2 o 7.3 nei comandi seguenti.

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP
```bash
sudo su
apt-get install curl apt-transport-https
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt-get install -y php7.4 php7.4-dev php7.4-xml php7.4-intl
```
### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per Debian seguendo le istruzioni riportate nell'[articolo relativo all'installazione in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

Per ottenere la visualizzazione corretta dell'output PHP in un browser, potrebbe essere necessario anche generare le impostazioni locali corrette. Per le impostazioni locali en_US UTF-8, ad esempio, eseguire questi comandi:
```bash
sudo su
sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen
```
Potrebbe essere necessario aggiungere `/usr/sbin` a `$PATH`, perché l'eseguibile `locale-gen` si trova in tale percorso.

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.4/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.4/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 7.4 sqlsrv pdo_sqlsrv
```

Se nel sistema è presente una sola versione di PHP, l'ultimo passaggio può essere semplificato come `phpenmod sqlsrv pdo_sqlsrv`. Come per `locale-gen`, `phpenmod` si trova in `/usr/sbin` quindi potrebbe essere necessario aggiungere questa directory a `$PATH`.

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Passaggio 4. Installare Apache e configurare il caricamento dei driver
```bash
sudo su
apt-get install libapache2-mod-php7.4 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php7.4
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Passaggio 5. Riavviare Apache e testare lo script di esempio
```bash
sudo service apache2 restart
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.

## <a name="installing-the-drivers-on-suse-12-and-15"></a>Installazione dei driver in Suse 12 e 15

> [!NOTE]
> Nelle istruzioni seguenti sostituire `<SuseVersion>` con la propria versione di Suse: SLE_15 o SLE_15_SP1 se si usa Suse Enterprise Linux 15. Per SUSE 12 usare SLE_12_SP4 (o versione successiva, se applicabile). Non tutte le versioni di PHP sono disponibili per tutte le versioni di Suse Linux. Fare riferimento a `http://download.opensuse.org/repositories/devel:/languages:/php` per vedere quali versioni di Suse hanno la versione predefinita di PHP disponibile o a `http://download.opensuse.org/repositories/devel:/languages:/php:/` per vedere quali altre versioni di PHP sono disponibili per quali versioni di Suse.

> [!NOTE]
> I pacchetti per PHP 7.4 non sono disponibili per Suse 12. Per installare PHP 7.2, sostituire l'URL del repository riportato di seguito con questo URL: `https://download.opensuse.org/repositories/devel:/languages:/php:/php72/<SuseVersion>/devel:languages:php:php72.repo`.
> Per installare PHP 7.3, sostituire l'URL del repository riportato di seguito con questo URL: `https://download.opensuse.org/repositories/devel:/languages:/php:/php73/<SuseVersion>/devel:languages:php:php73.repo`.

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP
```bash
sudo su
zypper -n ar -f https://download.opensuse.org/repositories/devel:languages:php/<SuseVersion>/devel:languages:php.repo
zypper --gpg-auto-import-keys refresh
zypper -n install php7 php7-devel php7-openssl
```
### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per Suse seguendo le istruzioni riportate nell'[articolo relativo all'installazione in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
> [!NOTE]
> Se viene visualizzato il messaggio di errore `Connection to 'pecl.php.net:443' failed: Unable to find the socket transport "ssl"`, modificare lo script PECL in /usr/bin/pecl e rimuovere l'opzione `-n` nell'ultima riga. Questa opzione impedisce a PECL di caricare file INI quando viene chiamato PHP, impedendo così il caricamento dell'estensione OpenSSL.

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/sqlsrv.ini
exit
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>Passaggio 4. Installare Apache e configurare il caricamento dei driver
```bash
sudo su
zypper install apache2 apache2-mod_php7
a2enmod php7
echo "extension=sqlsrv.so" >> /etc/php7/apache2/php.ini
echo "extension=pdo_sqlsrv.so" >> /etc/php7/apache2/php.ini
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Passaggio 5. Riavviare Apache e testare lo script di esempio
```bash
sudo systemctl restart apache2
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.

## <a name="installing-the-drivers-on-alpine-311"></a>Installazione dei driver in Alpine 3.11

> [!NOTE]
> La versione predefinita di PHP è la 7.3. Versioni alternative di PHP potrebbero essere disponibili in altri repository per Alpine 3.11. È invece possibile compilare PHP dall'origine.

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP
I pacchetti PHP per Alpine sono disponibili nel repository `edge/community`. Vedere [Enable Community Repository](https://wiki.alpinelinux.org/wiki/Enable_Community_Repository) (Abilitare il repository della community) nella pagina WIKI. Aggiungere la riga seguente a `/etc/apt/repositories`, sostituendo `<mirror>` con l'URL di un mirror del repository di Alpine:
```
http://<mirror>/alpine/edge/community
```
Eseguire quindi:
```bash
sudo su
apk update
# Note: The php7-pdo package is required only for the PDO_SQLSRV driver
apk add php7 php7-dev php7-pear php7-pdo php7-openssl autoconf make g++
```
### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per Alpine seguendo le istruzioni riportate nell'[articolo relativo all'installazione in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/10_pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/00_sqlsrv.ini
```

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Passaggio 4. Installare Apache e configurare il caricamento dei driver
```bash
sudo apk add php7-apache2 apache2
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Passaggio 5. Riavviare Apache e testare lo script di esempio
```bash
sudo rc-service apache2 restart
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.


## <a name="installing-the-drivers-on-macos-high-sierra-mojave-and-catalina"></a>Installazione dei driver in macOS Sierra, High Sierra, Mojave e Catalina

Se non è già presente, installare brew come illustrato di seguito:
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> [!NOTE]
> Per installare PHP 7.2 o 7.3, sostituire php@7.4 rispettivamente con php@7.2 o php@7.3 nei comandi seguenti.

### <a name="step-1-install-php"></a>Passaggio 1. Installare PHP

```bash
brew tap
brew tap homebrew/core
brew install php@7.4
```
PHP dovrebbe essere incluso nel percorso. Eseguire `php -v` per verificare che sia in esecuzione la versione corretta di PHP. Se PHP non si trova nel percorso previsto o la versione non è corretta, eseguire questo comando:
```bash
brew link --force --overwrite php@7.4
```

### <a name="step-2-install-prerequisites"></a>Passaggio 2. Prerequisiti di installazione
Installare il driver ODBC per macOS seguendo le istruzioni riportate nell'[articolo relativo all'installazione in macOS](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md). 

Potrebbe essere necessario anche installare gli strumenti GNU make:
```bash
brew install autoconf automake libtool
```

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Passaggio 3. Installare i driver PHP per Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>Passaggio 4. Installare Apache e configurare il caricamento dei driver
```bash
brew install apache2
```
Per trovare il file di configurazione di Apache `httpd.conf` per l'installazione di Apache in uso, eseguire 
```bash
/usr/local/bin/apachectl -V | grep SERVER_CONFIG_FILE
``` 
I comandi seguenti accodano la configurazione richiesta a `httpd.conf`. Assicurarsi di sostituire il percorso restituito dal comando precedente a `/usr/local/etc/httpd/httpd.conf`:
```bash
echo "LoadModule php7_module /usr/local/opt/php@7.4/lib/httpd/modules/libphp7.so" >> /usr/local/etc/httpd/httpd.conf
(echo "<FilesMatch .php$>"; echo "SetHandler application/x-httpd-php"; echo "</FilesMatch>";) >> /usr/local/etc/httpd/httpd.conf
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Passaggio 5. Riavviare Apache e testare lo script di esempio
```bash
sudo apachectl restart
```
Per testare l'installazione, vedere [Test dell'installazione](#testing-your-installation) alla fine di questo documento.

## <a name="testing-your-installation"></a>Test dell'installazione

Per testare questo script di esempio, creare un file denominato testsql.php nella radice dei documenti del sistema, che è `/var/www/html/` in Ubuntu, Debian e RedHat, `/srv/www/htdocs` in SUSE, `/var/www/localhost/htdocs` in Alpine o `/usr/local/var/www` in macOS. Copiarvi lo script seguente, sostituendo il server, il database, il nome utente e la password nel modo appropriato.
```php
<?php
$serverName = "yourServername";
$connectionOptions = array(
    "database" => "yourDatabase",
    "uid" => "yourUsername",
    "pwd" => "yourPassword"
);

function exception_handler($exception) {
    echo "<h1>Failure</h1>";
    echo "Uncaught exception: " , $exception->getMessage();
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

set_exception_handler('exception_handler');

// Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if ($conn === false) {
    die(formatErrors(sqlsrv_errors()));
}

// Select Query
$tsql = "SELECT @@Version AS SQL_VERSION";

// Executes the query
$stmt = sqlsrv_query($conn, $tsql);

// Error handling
if ($stmt === false) {
    die(formatErrors(sqlsrv_errors()));
}
?>

<h1> Success Results : </h1>

<?php
while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
    echo $row['SQL_VERSION'] . PHP_EOL;
}

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

function formatErrors($errors)
{
    // Display errors
    echo "<h1>SQL Error:</h1>";
    echo "Error information: <br/>";
    foreach ($errors as $error) {
        echo "SQLSTATE: ". $error['SQLSTATE'] . "<br/>";
        echo "Code: ". $error['code'] . "<br/>";
        echo "Message: ". $error['message'] . "<br/>";
    }
}
?>
```
Passare nel browser a https://localhost/testsql.php (https://localhost:8080/testsql.php in macOS). Dovrebbe ora essere possibile connettersi al database SQL di Azure/SQL Server. Se non viene visualizzato un messaggio di operazione riuscita che mostra informazioni sulla versione di SQL, vedere [Risorse di supporto](support-resources-for-the-php-sql-driver.md) per indicazioni su dove trovare assistenza.

## <a name="see-also"></a>Vedere anche  
[Introduzione ai driver Microsoft per PHP per SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)

[Caricamento dei Driver Microsoft per PHP per SQL Server](../../connect/php/loading-the-php-sql-driver.md)

[Requisiti di sistema dei driver Microsoft per PHP per SQL Server](../../connect/php/system-requirements-for-the-php-sql-driver.md)
