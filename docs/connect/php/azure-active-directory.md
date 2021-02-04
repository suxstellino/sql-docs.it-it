---
title: Azure Active Directory
description: Informazioni su come usare l'autenticazione Azure Active Directory con i driver Microsoft per PHP per SQL Server.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- azure active directory, authentication, access token
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae3e734dee5f8ac46de2646cca9c37a7ed7748df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076959"
---
# <a name="connect-using-azure-active-directory-authentication"></a>Connessione con l'autenticazione di Azure Active Directory
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Azure Active Directory](/azure/active-directory/active-directory-whatis) (Azure AD) è una tecnologia di gestione degli ID utente centrale che funge da alternativa all'[autenticazione di SQL Server](how-to-connect-using-sql-server-authentication.md). Azure AD consente le connessioni al database SQL di Microsoft Azure e ad Azure Synapse Analytics con identità federate in Azure AD che usano nome utente e una password, l'autenticazione integrata di Windows o un token di accesso di Azure AD. I driver PHP per SQL Server offrono un supporto parziale per queste funzionalità.

Per usare Azure AD, usare le parole chiave **Authentication** o **AccessToken** (si escludono a vicenda), come illustrato nella tabella seguente. Per informazioni più tecniche, vedere [Uso di Azure Active Directory con il driver ODBC](../odbc/using-azure-active-directory.md).

|Parola chiave|Valori|Descrizione|
|-|-|-|
|**AccessToken**|Non impostata (impostazione predefinita)|Modalità di autenticazione determinata da altre parole chiave. Per altre informazioni, vedere [Connection Options](connection-options.md). |
||Una stringa di byte|Token di accesso di Azure AD estratto da una risposta JSON OAuth. La stringa di connessione non deve contenere l'ID utente, la password o la parola chiave di autenticazione (richiede il driver ODBC versione 17 o successiva in Linux o macOS). |
|**autenticazione**|Non impostata (impostazione predefinita)|Modalità di autenticazione determinata da altre parole chiave. Per altre informazioni, vedere [Connection Options](connection-options.md). |
||`SqlPassword`|Eseguire l'autenticazione direttamente in un'istanza di SQL Server (che può essere un'istanza di Azure) usando un nome utente e una password. Il nome utente e la password devono essere passati nella stringa di connessione usando le parole chiave **UID** e **PWD**. |
||`ActiveDirectoryPassword`|Eseguire l'autenticazione con un'identità di Azure Active Directory usando un nome utente e una password. Il nome utente e la password devono essere passati nella stringa di connessione usando le parole chiave **UID** e **PWD**. |
||`ActiveDirectoryMsi`|Eseguire l'autenticazione usando un'identità gestita assegnata dal sistema o un'identità gestita assegnata dall'utente (richiede il driver ODBC versione 17.3.1.1 o successiva). Per una panoramica e le esercitazioni, vedere [Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).|
||`ActiveDirectoryServicePrincipal`|Eseguire l'autenticazione usando oggetti entità servizio (è necessario il driver ODBC versione 17,7 o successiva). Per altri dettagli ed esempi, vedere [oggetti applicazione e oggetti entità servizio in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals).|

La parola chiave **Authentication** influisce sulle impostazioni di sicurezza della connessione. Se è impostata nella stringa di connessione, per impostazione predefinita la parola chiave **Encrypt** è impostata su true, ovvero il client richiede la crittografia. Inoltre, il certificato del server verrà convalidato indipendentemente dall'impostazione di crittografia, a meno che **TrustServerCertificate** non sia impostato su true (l'impostazione predefinita è **false**). Questa funzionalità si distingue dal metodo di accesso precedente, meno sicuro, in cui il certificato del server viene convalidato solo quando la crittografia è richiesta in modo specifico nella stringa di connessione.

#### <a name="limitations"></a>Limitazioni

In Windows, sebbene il driver ODBC sottostante supporti un ulteriore valore per la parola chiave **Authentication**, ovvero **ActiveDirectoryIntegrated**, i driver PHP non supportano il valore in alcuna piattaforma.

## <a name="example---connect-using-sqlpassword-and-activedirectorypassword"></a>Esempio: connettersi usando SqlPassword e ActiveDirectoryPassword

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = array("UID"=>$myusername, "PWD"=>$mypassword, "Authentication"=>'SqlPassword');

$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=SqlPassword.\n";
    sqlsrv_close($conn);
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = array("Database"=>$azureDatabase,
                        "UID"=>$azureUsername,
                        "PWD"=>$azurePassword,
                        "Authentication"=>'ActiveDirectoryPassword');

$conn = sqlsrv_connect($azureServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    sqlsrv_close($conn);
}

?>
```

## <a name="example---connect-using-the-pdo_sqlsrv-driver"></a>Esempio: connettersi usando il driver PDO_SQLSRV

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = "Database = $databaseName; Authentication = SqlPassword;";

try {
    $conn = new PDO("sqlsrv:server = $serverName ; $connectionInfo", $myusername, $mypassword);
    echo "Connected successfully with Authentication=SqlPassword.\n";
    $conn = null;
} catch (PDOException $e) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryPassword;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer ; $connectionInfo", $azureUsername, $azurePassword);
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-azure-ad-access-token"></a>Esempio: connettersi usando il token di accesso di Azure AD

### <a name="sqlsrv-driver"></a>Driver SQLSRV

```php
<?php
// Using an access token to connect: do not use UID or PWD connection options
// Assume $accToken is the valid byte string extracted from an OAuth JSON response
$connectionInfo = array("Database"=>$azureAdDatabase, "AccessToken"=>$accToken);
$conn = sqlsrv_connect($azureAdServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Azure AD Access Token.\n";
    sqlsrv_close($conn);
}
?>
```

### <a name="pdo_sqlsrv-driver"></a>Driver PDO_SQLSRV

```php
<?php
try {
    // Using an access token to connect: do not pass in $uid or $pwd
    // Assume $accToken is the valid byte string extracted from an OAuth JSON response
    $connectionInfo = "Database = $azureAdDatabase; AccessToken = $accToken;";
    $conn = new PDO("sqlsrv:server = $azureAdServer; $connectionInfo");
    echo "Connected successfully with Azure AD Access Token\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-managed-identities-for-azure-resources"></a>Esempio: connettersi usando le identità gestite per le risorse di Azure

### <a name="using-the-system-assigned-managed-identity-with-sqlsrv-driver"></a>Uso dell'identità gestita assegnata dal sistema con il driver SQLSRV

Quando ci si connette usando l'identità gestita assegnata dal sistema, non usare le opzioni UID o PWD.

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$connectionInfo = array('Database'=>$azureDatabase,
                        'Authentication'=>'ActiveDirectoryMsi');
$conn = sqlsrv_connect($azureServer, $connectionInfo);

if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    $stmt = sqlsrv_query($conn, $tsql);
    if ($stmt === false) {
        echo "Failed to run the simple query (system-assigned).\n";
        print_r(sqlsrv_errors());
    } else {
        while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
            echo $row['SQL_VERSION'] . PHP_EOL;
        }

        sqlsrv_free_stmt($stmt);
    }
    
    sqlsrv_close($conn);
}
?>
```

### <a name="using-the-user-assigned-managed-identity-with-pdo_sqlsrv-driver"></a>Uso dell'identità gestita assegnata dall'utente con il driver SQLSRV

Un'identità gestita assegnata dall'utente viene creata come risorsa di Azure autonoma. Azure crea un'identità nel tenant di Azure AD considerato attendibile dalla sottoscrizione in uso. Dopo la creazione, l'identità può essere assegnata a una o più istanze del servizio di Azure. Copiare `Object ID` di questa identità e impostarlo come nome utente nella stringa di connessione. 

In seguito, quando ci si connette usando l'identità gestita assegnata dall'utente, specificare l'ID oggetto come nome utente, ma omettere la password.

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$azureUser = '2d68f56e-9547-4dae-aee8-f3g28ab9674x';    // Object ID of the identity
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryMsi;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer; $connectionInfo", $azureUser);
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    
    try {
        $stmt = $conn->query($tsql);
        $result = $stmt->fetchall(PDO::FETCH_ASSOC);
        print_r($result);

        unset($stmt);
    } catch (PDOException $e) {
        echo "Failed to run the simple query (user-assigned).\n";
    }
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-service-principal-objects-in-azure-active-directory"></a>Esempio: connettersi usando oggetti entità servizio in Azure Active Directory

Per eseguire l'autenticazione usando un oggetto entità servizio, sono necessari l' [ID client dell'applicazione](/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-and-app-id-values-for-signing-in) corrispondente e il [segreto client](/azure/active-directory/develop/howto-create-service-principal-portal#option-2-create-a-new-application-secret).


### <a name="sqlsrv-driver"></a>Driver SQLSRV

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
$connectionInfo = array("Database"=>$adDatabase, 
                        "Authentication"=>"ActiveDirectoryServicePrincipal",
                        "UID"=>$adSPClientId,
                        "PWD"=>$adSPClientSecret);

$conn = sqlsrv_connect($adServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect using Azure AD Service Principal." . PHP_EOL;
    print_r(sqlsrv_errors());
}

sqlsrv_close($conn);

?>
```

### <a name="pdo_sqlsrv-driver"></a>Driver PDO_SQLSRV

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
try {
    $connectionInfo = "Database = $adDatabase; Authentication = ActiveDirectoryServicePrincipal;";
    $conn = new PDO("sqlsrv:server = $adServer; $connectionInfo", $adSPClientId, $adSPClientSecret);
} catch (PDOException $e) {
    echo "Could not connect using Azure AD Service Principal.\n";
    print_r($e->getMessage());
    echo PHP_EOL;
}

unset($conn);
?>
```


## <a name="see-also"></a>Vedere anche
[Uso di Azure Active Directory con ODBC Driver](../odbc/using-azure-active-directory.md)

[Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview)