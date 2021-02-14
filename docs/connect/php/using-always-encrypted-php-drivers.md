---
title: Uso di Always Encrypted con i driver PHP per SQL Server
description: Informazioni su come usare Always Encrypted con i driver PHP per SQL Server per proteggere i dati sensibili nell'applicazione.
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-kaywon
ms.author: v-daenge
author: David-Engel
manager: v-mabarw
ms.openlocfilehash: faa8974dd8ab7201158592c50288c31e7656e5f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058166"
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>Uso di Always Encrypted con i driver PHP per SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>Applicabile a
 -   Microsoft Driver 5.2 per PHP per SQL Server
 
## <a name="introduction"></a>Introduzione

Questo articolo fornisce informazioni su come sviluppare applicazioni PHP usando [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) e [Driver PHP per SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md).

Always Encrypted consente alle applicazioni client di eseguire la crittografia dei dati sensibili senza mai rivelare i dati o le chiavi di crittografia a SQL Server o al database SQL di Azure. Un driver abilitato per Always Encrypted, come ODBC Driver for SQL Server, esegue in modo trasparente la crittografia e la decrittografia dei dati sensibili nell'applicazione client. Il driver determina automaticamente i parametri di query corrispondenti alle colonne di database con dati sensibili (protette mediante Always Encrypted) e crittografa i valori di tali parametri prima di passare i dati a SQL Server o al database SQL di Azure. Analogamente, il driver esegue in modo trasparente la decrittografia dei dati, recuperati dalle colonne di database crittografate nei risultati delle query. Per altre informazioni, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). I driver PHP per SQL Server usano il driver ODBC per SQL Server per crittografare i dati sensibili.

## <a name="prerequisites"></a>Prerequisiti

 -   Configurare Always Encrypted nel database. Questa configurazione implica il provisioning di chiavi Always Encrypted e l'impostazione della crittografia per le colonne di database selezionate. Se non è presente un database in cui Always Encrypted è configurato, seguire le istruzioni fornite nel blog di [introduzione a Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). In particolare, il database deve contenere le definizioni dei metadati per una chiave master della colonna, una chiave di crittografia della colonna e una tabella contenente una o più colonne crittografate con tale chiave di crittografia.
 -   Assicurarsi che nel computer di sviluppo sia installato ODBC Driver 17 (o versione successiva) per SQL Server. Per informazioni dettagliate, vedere [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="enabling-always-encrypted-in-a-php-application"></a>Abilitazione di Always Encrypted in un'applicazione PHP

Il modo più semplice per abilitare la crittografia dei parametri destinati alle colonne crittografate e la decrittografia dei risultati delle query consiste nell'impostare il valore della parola chiave della stringa di connessione `ColumnEncryption` su `Enabled`. Di seguito sono riportati alcuni esempi di abilitazione di Always Encrypted nei driver SQLSRV e PDO_SQLSRV:

SQLSRV:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

L'abilitazione di Always Encrypted non è sufficiente per l'esito positivo della crittografia o della decrittografia. È anche necessario assicurarsi che:
 -   L'applicazione abbia le autorizzazioni di database VIEW ANY COLUMN MASTER KEY DEFINITION e VIEW ANY COLUMN ENCRYPTION KEY DEFINITION, necessarie per accedere ai metadati sulle chiavi Always Encrypted nel database. Per informazioni dettagliate, vedere [Autorizzazioni per il database](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
 -   L'applicazione possa accedere alla chiave master della colonna che protegge le chiavi di crittografia della colonna per le colonne crittografate sottoposte a query. Questo requisito dipende dal provider dell'archivio chiavi in cui è archiviata la chiave CMK. Per altre informazioni, vedere [Uso degli archivi delle chiavi master delle colonne](#working-with-column-master-key-stores).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recupero e modifica di dati nelle colonne crittografate

Dopo aver abilitato Always Encrypted per una connessione, è possibile usare le API SQLSRV standard (vedere [Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)) o le API PDO_SQLSRV (vedere [Riferimento all'API del driver PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)) per recuperare o modificare i dati nelle colonne di database crittografate. Supponendo che l'applicazione abbia le autorizzazioni necessarie per il database e che possa accedere alla chiave master della colonna, il driver crittografa tutti i parametri di query che puntano a colonne crittografate e decrittografa i dati recuperati dalle colonne crittografate, comportandosi in modo trasparente per l'applicazione come se le colonne non fossero crittografate.

Se la funzionalità Always Encrypted non è abilitata, le query con parametri destinati alle colonne crittografate hanno esito negativo. I dati possono essere comunque recuperati dalle colonne crittografate, a condizione che non presentino parametri destinati alle colonne crittografate. Il driver tuttavia non tenta alcuna decrittografia e l'applicazione riceve i dati binari crittografati, sotto forma di matrici di byte.

La tabella seguente riepiloga il comportamento delle query, a seconda che la funzionalità Always Encrypted sia abilitata o meno:

|Caratteristica della query|Always Encrypted è abilitato e l'applicazione può accedere alle chiavi e ai metadati delle chiavi|Always Encrypted è abilitato e l'applicazione non può accedere alle chiavi o ai metadati delle chiavi|Always Encrypted è disabilitato|
|---|---|---|---|
|Parametri destinati alle colonne crittografate.|I valori dei parametri vengono crittografati in modo trasparente.|Errore|Errore|
|Recupero di dati dalle colonne crittografate, senza parametri destinati alle colonne crittografate.|I risultati delle colonne vengono decrittografati in modo trasparente. L'applicazione riceve i valori di colonna in testo non crittografato. |Errore|I risultati delle colonne crittografate non vengono decrittografate. L'applicazione riceve i valori crittografati come matrici di byte.|
 
Gli esempi seguenti illustrano il recupero e la modifica dei dati nelle colonne crittografate. Negli esempi si presuppone una tabella con lo schema seguente. Le colonne SSN e BirthDate sono crittografate.
```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>Esempio di inserimento dati

Gli esempi seguenti illustrano come usare i driver SQLSRV e PDO_SQLSRV per inserire una riga nella tabella Patient. Tenere presente quanto segue:
 -   Il codice di esempio non contiene alcun elemento specifico per la crittografia. Il driver rileva automaticamente e crittografa i valori dei parametri SSN e BirthDate, che puntano a colonne crittografate. In questo modo la crittografia diventa trasparente per l'applicazione.
 -   I valori inseriti nelle colonne di database, incluse quelle crittografate, vengono passati come parametri associati. Quando si inviano valori a colonne non crittografate, l'uso dei parametri è facoltativo, nonostante sia consigliabile per prevenire attacchi SQL injection. È invece necessario usare i parametri in presenza di valori destinati a colonne crittografate. Se i valori inseriti nelle colonne SSN o BirthDate sono stati passati come valori letterali incorporati nell'istruzione della query, la query avrà esito negativo perché il driver non prova a crittografare o a elaborare i valori letterali nelle query. Di conseguenza, il server li rifiuterà come incompatibili con le colonne crittografate.
 -   Quando si inseriscono valori tramite parametri di binding è necessario passare al database un tipo SQL identico al tipo di dati della colonna di destinazione o per il quale è supportata la conversione al tipo di dati della colonna di destinazione. Questo requisito è dovuto al fatto che Always Encrypted supporta alcune conversioni di tipi. Per informazioni dettagliate, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). I due driver PHP, SQLSRV e PDO_SQLSRV, hanno un meccanismo che consente all'utente di determinare il tipo SQL del valore. Pertanto l'utente non è obbligato a specificare il tipo SQL in modo esplicito.
  -   Per il driver SQLSRV l'utente ha due opzioni:
   -   Basarsi sul driver PHP per determinare e impostare il tipo SQL appropriato. In questo caso l'utente deve usare `sqlsrv_prepare` e `sqlsrv_execute` per eseguire una query con parametri.
   -   Impostare il tipo SQL in modo esplicito.
  -   Per il driver PDO_SQLSRV l'utente non ha la possibilità di impostare in modo esplicito il tipo SQL di un parametro. Il driver PDO_SQLSRV consente all'utente di determinare in modo automatico il tipo SQL quando associa un parametro.
 -   Il rilevamento del tipo SQL da parte dei driver presenta alcune limitazioni:
  -   Driver SQLSRV:
   -   Se l'utente vuole che il driver determini i tipi SQL per le colonne crittografate, deve usare `sqlsrv_prepare` e `sqlsrv_execute`.
   -   Se è preferibile `sqlsrv_query`, l'utente deve specificare i tipi SQL per tutti i parametri. Il tipo SQL specificato deve includere la lunghezza della stringa per i tipi di stringa e la scala e la precisione per i tipi decimali.
  -   Driver PDO_SQLSRV:
   -   L'attributo di istruzione `PDO::SQLSRV_ATTR_DIRECT_QUERY` non è supportato in una query con parametri.
   -   L'attributo di istruzione `PDO::ATTR_EMULATE_PREPARES` non è supportato in una query con parametri.
   
Driver SQLSRV e [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

Driver SQLSRV e [sqlsrv_query](../../connect/php/sqlsrv-query.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

Driver PDO_SQLSRV e [PDO::prepare](../../connect/php/pdo-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>Esempio di recupero di dati di testo non crittografato

L'esempio seguente illustra come filtrare i dati in base ai valori crittografati e come recuperare i dati in testo non crittografato da colonne crittografate usando i driver SQLSRV e PDO_SQLSRV. Tenere presente quanto segue:
 -   Il valore usato nella clausola WHERE per filtrare la colonna SSN deve essere passato usando il parametro bind, in modo che il driver possa crittografarlo in modo trasparente prima dell'invio al server.
 -   Quando si esegue una query con parametri associati, i driver PHP determinano automaticamente il tipo SQL, a meno che l'utente non specifichi in modo esplicito il tipo SQL quando usa il driver SQLSRV.
 -   Tutti i valori stampati dal programma sono in testo non crittografato, perché il driver decrittografa in modo trasparente i dati recuperati dalle colonne SSN e BirthDate.
 
Nota: le query possono eseguire confronti di uguaglianza nelle colonne crittografate solo se la crittografia è deterministica. Per altre informazioni, vedere [Selezione della crittografia deterministica o casuale](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>Esempio di recupero di dati di testo crittografato

Se Always Encrypted non è abilitato, una query può comunque recuperare dati dalle colonne crittografate, a condizione che non presenti parametri destinati alle colonne crittografate.

Gli esempi seguenti illustrano come recuperare i dati crittografati binari dalle colonne crittografate, usando i driver SQLSRV e PDO_SQLSRV. Tenere presente quanto segue:
 -   Poiché la funzionalità Always Encrypted non è abilitata nella stringa di connessione, la query restituisce i valori crittografati di SSN e BirthDate come matrici di byte. Il programma converte i valori in stringhe.
 -   Una query che recupera dati dalle colonne crittografate con Always Encrypted disabilitato può avere parametri, a condizione che nessuno dei parametri sia destinato a una colonna crittografata. La query seguente filtra in base alla colonna LastName, che non è crittografata nel database. Se la query filtrasse in base a SSN o BirthDate, l'esito sarebbe negativo.
 
SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Come evitare i problemi comuni quando si eseguono query su colonne crittografate

Questa sezione descrive categorie di errori comuni che si verificano quando si eseguono query su colonne crittografate da applicazioni PHP e contiene alcune linee guida su come correggere tali errori.

#### <a name="unsupported-data-type-conversion-errors"></a>Errori di conversione dei tipi di dati non supportati

Always Encrypted supporta alcune conversioni per i tipi di dati crittografati. Per l'elenco dettagliato delle conversioni dei tipi supportati, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Per evitare errori di conversione dei tipi di dati, eseguire le operazioni seguenti:
 -   Quando si usa il driver SQLSRV con `sqlsrv_prepare` e `sqlsrv_execute` viene determinato automaticamente il tipo SQL, insieme alle dimensioni della colonna e al numero di cifre decimali del parametro.
 -   Anche quando si usa il driver SQLSRV per eseguire una query, il tipo SQL viene determinato automaticamente, insieme alle dimensioni della colonna e al numero di cifre decimali del parametro.
 -   Quando si usa il driver SQLSRV con `sqlsrv_query` per eseguire una query:
  -   Il tipo SQL del parametro è esattamente lo stesso del tipo della colonna di destinazione oppure è supportata la conversione dal tipo SQL al tipo della colonna.
  -   La precisione e la scala dei parametri destinati alle colonne dei tipi di dati `decimal` e `numeric` di SQL Server sono uguali a quelle configurate per la colonna di destinazione.
  -   La precisione dei parametri destinati alle colonne dei tipi di dati `datetime2`, `datetimeoffset` e `time` di SQL Server non è maggiore della precisione per la colonna di destinazione nelle query che modificano la colonna di destinazione.
 -   Non usare gli attributi istruzione PDO_SQLSRV `PDO::SQLSRV_ATTR_DIRECT_QUERY` o `PDO::ATTR_EMULATE_PREPARES` in una query con parametri
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errori causati dal passaggio di testo non crittografato anziché di valori crittografati

Qualsiasi valore destinato a una colonna crittografata deve essere crittografato prima di essere inviato al server. Il tentativo di inserire, modificare o filtrare in base a un valore di testo non crittografato in una colonna crittografata genera un errore. Per evitare tali errori, verificare che:
 -   Always Encrypted è abilitato (nella stringa di connessione impostare la parola chiave `ColumnEncryption` su `Enabled`).
 -   Per inviare i dati destinati alle colonne crittografate venga usata l'associazione dei parametri. L'esempio seguente illustra una query che filtra in modo errato in base a un valore letterale o costante in una colonna crittografata (SSN):
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>Controllo dell'impatto di Always Encrypted sulle prestazioni

Considerato che Always Encrypted è una tecnologia di crittografia lato client, la maggior parte del sovraccarico delle prestazioni si verifica sul lato client, non nel database. A parte il costo delle operazioni di crittografia e decrittografia, le altre origini di sovraccarico delle prestazioni sul lato client sono le seguenti:
 -   Round trip aggiuntivi al database per recuperare i metadati per i parametri di query.
 -   Chiamate a un archivio delle chiavi master delle colonne per accedere alla chiave master di una colonna.
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>Round trip per recuperare i metadati per i parametri di query

Se Always Encrypted è abilitato per una connessione, per impostazione predefinita il driver ODBC chiamerà [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) per ogni query con parametri, passando l'istruzione di query (senza i valori dei parametri) a SQL Server. Questa stored procedure analizza l'istruzione di query per verificare se sono presenti parametri da crittografare e, in tal caso, restituisce le informazioni relative alla crittografia per ogni parametro in modo da consentire al driver di crittografarli.

Poiché i driver PHP consentono all'utente di associare un parametro in un'istruzione preparata senza specificare il tipo SQL, quando si associa un parametro in una connessione che supporta Always Encrypted i driver PHP chiamano [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md) sul parametro per ottenere il tipo SQL, le dimensioni della colonna e le cifre decimali. I metadati vengono quindi usati per chiamare [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md). Queste chiamate `SQLDescribeParam` aggiuntive non richiedono round trip aggiuntivi al database, perché il driver ODBC ha già archiviato le informazioni sul lato client al momento della chiamata di `sys.sp_describe_parameter_encryption`.

Il comportamento precedente garantisce all'applicazione client un elevato livello di trasparenza: non è necessario che l'applicazione (e lo sviluppatore dell'applicazione) sappiano quali query accedono alle colonne crittografate, a condizione che i valori destinati alle colonne crittografate vengano passati al driver nei parametri.

A differenza del driver ODBC per SQL Server, l'abilitazione di Always Encrypted a livello di istruzione/query non è ancora supportata nei driver PHP. 

### <a name="column-encryption-key-caching"></a>Memorizzazione nella cache delle chiavi di crittografia della colonna

Per ridurre il numero di chiamate a un archivio chiavi master della colonna per decrittografare le chiavi di crittografia della colonna (CEK), il driver memorizza nella cache le chiavi di crittografia della colonna di testo non crittografato. Dopo aver ricevuto la chiave CEK crittografata (ECEK) dai metadati del database, il driver ODBC prova prima di tutto a trovare la chiave di crittografia della colonna di testo non crittografato corrispondente al valore della chiave crittografata nella cache. Il driver chiama l'archivio chiavi contenente la chiave master della colonna solo se non riesce a trovare la chiave di crittografia della colonna di testo non crittografato corrispondente nella cache.

Nota: nel driver ODBC per SQL Server le voci nella cache vengono rimosse dopo un timeout di due ore. Questo comportamento significa che per una determinata chiave ECEK, il driver contatta l'archivio chiavi una sola volta nel corso della durata dell'applicazione o ogni due ore, a seconda di quale sia il valore più basso.

## <a name="working-with-column-master-key-stores"></a>Uso degli archivi delle chiavi master delle colonne

Per crittografare o decrittografare i dati, il driver deve ottenere una chiave di crittografia della colonna configurata per la colonna di destinazione. Le chiavi di crittografia di colonna vengono archiviate in formato crittografato (ECEK) nei metadati del database. Ogni chiave di crittografia della colonna ha una chiave master della colonna corrispondente che è stata usata per crittografarla. I [metadati del database](../../t-sql/statements/create-column-master-key-transact-sql.md) non archiviano la chiave master della colonna stessa. Contengono solo il nome dell'archivio chiavi e le informazioni che l'archivio chiavi può usare per trovare la chiave master della colonna.

Per ottenere il valore di testo non crittografato di una chiave ECEK, il driver ottiene prima di tutto i metadati relativi sia alla chiave di crittografia della colonna che alla chiave master della colonna corrispondente, quindi usa queste informazioni per contattare l'archivio chiavi contenente la chiave master della colonna e richiede di decrittografare la chiave ECEK. Il driver comunica con un archivio chiavi usando un provider dell'archivio chiavi.

Il driver Microsoft 5.3.0 per PHP per SQL Server supporta solo i provider per l'archivio certificati di Windows e Azure Key Vault. Non supporta ancora l'altro provider dell'archivio chiavi supportato dal driver ODBC (provider dell'archivio chiavi personalizzato).

### <a name="using-the-windows-certificate-store-provider"></a>Uso del provider per l'archivio certificati Windows

ODBC Driver for SQL Server in Windows include un provider di archivio chiavi master della colonna predefinito per l'archivio certificati Windows denominato `MSSQL_CERTIFICATE_STORE`. (Questo provider non è disponibile in macOS o Linux). Con questo provider, la chiave master della colonna viene archiviata in locale nel computer client e non è necessaria alcuna configurazione aggiuntiva da parte dell'applicazione per usarla con il driver. Tuttavia, l'applicazione deve avere accesso al certificato e alla relativa chiave privata nell'archivio. Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-azure-key-vault"></a>Uso di Azure Key Vault

Azure Key Vault offre un metodo per archiviare chiavi di crittografia, password e altri segreti tramite Azure e può essere usato per archiviare le chiavi di Always Encrypted. ODBC Driver for SQL Server 17 (e versioni successive) include un provider dell'archivio chiavi master predefinito per Azure Key Vault. Le seguenti opzioni di connessione gestiscono la configurazione di Azure Key Vault: `KeyStoreAuthentication`, `KeyStorePrincipalId` e `KeyStoreSecret`. 
 -   `KeyStoreAuthentication` può assumere uno di due valori stringa possibili: `KeyVaultPassword` e `KeyVaultClientSecret`. Questi valori controllano il tipo di credenziali di autenticazione usate con le altre due parole chiave.
 -   `KeyStorePrincipalId` accetta una stringa che rappresenta un identificatore per l'account che prova ad accedere ad Azure Key Vault. 
     -   Se `KeyStoreAuthentication` è impostata su `KeyVaultPassword`, `KeyStorePrincipalId` deve essere il nome di un utente di Azure Active Directory.
     -   Se `KeyStoreAuthentication` è impostata su `KeyVaultClientSecret`, `KeyStorePrincipalId` deve essere un ID client dell'applicazione.
 -   `KeyStoreSecret` accetta una stringa che rappresenta un segreto delle credenziali. 
     -   Se `KeyStoreAuthentication` è impostata su `KeyVaultPassword`, `KeyStoreSecret` deve essere la password dell'utente. 
     -   Se `KeyStoreAuthentication` è impostata su `KeyVaultClientSecret`, `KeyStoreSecret` deve essere il segreto dell'applicazione associato all'ID client dell'applicazione.

Tutte e tre le opzioni devono essere presenti nella stringa di connessione per usare Azure Key Vault. Inoltre `ColumnEncryption` deve essere impostato su `Enabled`. Se `ColumnEncryption` è impostato su `Disabled` ma sono presenti le opzioni di Azure Key Vault, lo script procede senza errori, ma non viene eseguita nessuna crittografia.

Gli esempi seguenti illustrano come stabilire la connessione a SQL Server usando Azure Key Vault.

SQLSRV:

Con un account Azure Active Directory:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultPassword", "KeyStorePrincipalId"=>$AADUsername, "KeyStoreSecret"=>$AADPassword);
$conn = sqlsrv_connect($server, $connectionInfo);
```
Con un ID client e un segreto dell'applicazione Azure:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultClientSecret", "KeyStorePrincipalId"=>$applicationClientID, "KeyStoreSecret"=>$applicationClientSecret);
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV: Con un account Azure Active Directory:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultPassword; KeyStorePrincipalId = $AADUsername; KeyStoreSecret = $AADPassword;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```
Con un ID client e un segreto dell'applicazione Azure:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultClientSecret; KeyStorePrincipalId = $applicationClientID; KeyStoreSecret = $applicationClientSecret;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>Limitazioni del driver PHP quando si usa Always Encrypted

SQLSRV e PDO_SQLSRV:
 -   Linux/macOS non supportano il provider dell'Archivio certificati Windows
 -   Forzare la crittografia dei parametri
 -   Abilitazione di Always Encrypted a livello di istruzione 
 -   Quando si usa la funzionalità Always Encrypted e le impostazioni locali non UTF8 in Linux e macOS (ad esempio "en_US.ISO-8859-1"), l'inserimento di dati null o di una stringa vuota in una colonna char(n) crittografata potrebbe non funzionare se la Tabella codici 1252 non è stata installata nel sistema
 
Solo SQLSRV:
 -   Uso di `sqlsrv_query` per il parametro di associazione senza specificare il tipo SQL
 -   Uso di `sqlsrv_prepare` per i parametri di associazione in un batch di istruzioni SQL  
 
Solo PDO_SQLSRV:
 -   Attributo di istruzione `PDO::SQLSRV_ATTR_DIRECT_QUERY` specificato in una query con parametri
 -   Attributo di istruzione `PDO::ATTR_EMULATE_PREPARE` specificato in una query con parametri
 -   parametri di associazione in un batch di istruzioni SQL
 
I driver PHP ereditano anche le limitazioni imposte da ODBC Driver for SQL Server e dal database. Vedere [Limitazioni del driver ODBC quando si usa Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) e [Informazioni sulle funzionalità di Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details).  
  
## <a name="see-also"></a>Vedere anche  
[Guida di programmazione per il driver SQL PHP](../../connect/php/programming-guide-for-php-sql-driver.md)

[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Riferimento all'API del driver PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
