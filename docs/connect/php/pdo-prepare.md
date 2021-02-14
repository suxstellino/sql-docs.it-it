---
title: PDO::prepare
description: Informazioni di riferimento sulle API per la funzione PDO::prepare nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a8b16fdc-c748-49be-acf2-a6ac7432d16b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5e8a42aa2728ea8136c2889c18f26bafeea36ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039111"
---
# <a name="pdoprepare"></a>PDO::prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prepara un'istruzione per l'esecuzione.

## <a name="syntax"></a>Sintassi

```
PDOStatement PDO::prepare ( $statement [, array(key_pair)] )
```

#### <a name="parameters"></a>Parametri
$*statement*: stringa contenente l'istruzione SQL.

*key_pair*: matrice contenente il nome e il valore di un attributo. Per altre informazioni, vedere la sezione Osservazioni.

## <a name="return-value"></a>Valore restituito
In caso di esito positivo restituisce un oggetto PDOStatement. In caso di esito negativo restituisce un oggetto PDOException o false, a seconda del valore di `PDO::ATTR_ERRMODE`.

## <a name="remarks"></a>Osservazioni
[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] non valuta le istruzioni preparate fino all'esecuzione.

Nella tabella seguente sono elencati i valori *key_pair* possibili.

|Chiave|Descrizione|
|-------|---------------|
|PDO::ATTR_CURSOR|Specifica il comportamento del cursore. Il valore predefinito è `PDO::CURSOR_FWDONLY`, un cursore di tipo forward non scorrevole. `PDO::CURSOR_SCROLL` è un cursore scorrevole.<br /><br />Ad esempio: `array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY )`.<br /><br />Se questo attributo è impostato su `PDO::CURSOR_SCROLL`, è quindi possibile usare `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` per impostare il tipo di cursore scorrevole, come descritto di seguito.<br /><br />Per altre informazioni sui set di risultati e sui cursori nel driver PDO_SQLSRV, vedere [Tipi di cursore &#40;driver PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|
|PDO::ATTR_EMULATE_PREPARES|Per impostazione predefinita, questo attributo è false. È possibile modificare questa impostazione usando `PDO::ATTR_EMULATE_PREPARES => true`. Per informazioni dettagliate e un esempio, vedere la sezione relativa a [EMULATE_PREPARES](#emulate-prepare).|
|PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE|Specifica il tipo di cursore scorrevole. È valido solo quando `PDO::ATTR_CURSOR` è impostato su `PDO::CURSOR_SCROLL`. Vedere di seguito per informazioni sui valori ammessi per questo attributo.|
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|Specifica il numero di posizioni decimali per la formattazione dei valori money recuperati. Questa opzione funziona solo quando `PDO::SQLSRV_ATTR_FORMAT_DECIMALS` è true. Per altre informazioni, vedere [Formattazione di stringhe decimali e valori money (driver PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|Se true, consente di specificare l'esecuzione di una query diretta. False indica l'esecuzione di un'istruzione preparata. Per altre informazioni su `PDO::SQLSRV_ATTR_DIRECT_QUERY`, vedere [Esecuzione di istruzioni diretta e preparata nel driver PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_ENCODING|PDO::SQLSRV_ENCODING_UTF8 (impostazione predefinita)<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|Specifica se recuperare i tipi di data e ora come oggetti [DateTime PHP](http://php.net/manual/en/class.datetime.php). Per altre informazioni, vedere [Procedura: Recuperare i tipi di data e ora come oggetti DateTime PHP usando il driver PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|Gestisce i recuperi numerici dalle colonne con tipi SQL numerici. Per altre informazioni, vedere [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|Specifica se aggiungere zeri iniziali alle stringhe decimali quando appropriato. Se impostata, questa opzione abilita l'opzione `PDO::SQLSRV_ATTR_DECIMAL_PLACES` per la formattazione dei tipi money. Per altre informazioni, vedere [Formattazione di stringhe decimali e valori money (driver PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|Per altre informazioni, vedere [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|

Quando si usa `PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL`, è possibile usare `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` per specificare il tipo di cursore. Per impostare un cursore dinamico, ad esempio, passare la matrice seguente a PDO::prepare:
```
array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_DYNAMIC));
```
La tabella seguente mostra i valori possibili per`PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE`. Per altre informazioni sui cursori scorrevoli, vedere [Tipi di cursore &#40;driver PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

|Valore|Descrizione|
|---------|---------------|
|PDO::SQLSRV_CURSOR_BUFFERED|Crea un cursore statico (con buffer) lato client, che memorizza il set di risultati nel buffer in memoria sul computer client.|
|PDO::SQLSRV_CURSOR_DYNAMIC|Crea un cursore dinamico (non memorizzato nel buffer) sul lato server che consente di accedere alle righe in qualsiasi ordine e riflette le modifiche nel database.|
|PDO::SQLSRV_CURSOR_KEYSET|Crea un cursore keyset sul lato server. Un cursore keyset non aggiorna il conteggio delle righe se dalla tabella viene eliminata una riga (una riga eliminata viene restituita senza alcun valore).|
|PDO::SQLSRV_CURSOR_STATIC|Crea un cursore statico sul lato server che consente di accedere alle righe in qualsiasi ordine ma non riflette le modifiche nel database.<br /><br />`PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` implica `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_STATIC`.|

È possibile chiudere un oggetto PDOStatement chiamando `unset`:
```
unset($stmt);
```

## <a name="forward-only-example"></a>Esempio di cursore forward-only
Questo esempio illustra come usare PDO::prepare con indicatori di parametro e un cursore forward-only.

```
<?php
$database = "Test";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$col1 = 'a';
$col2 = 'b';

$query = "insert into Table1(col1, col2) values(?, ?)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( $col1, $col2 ) );
print $stmt->rowCount();
echo "\n";

$query = "insert into Table1(col1, col2) values(:col1, :col2)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( ':col1' => $col1, ':col2' => $col2 ) );
print $stmt->rowCount();

unset($stmt);
?>
```

## <a name="static-cursor-example"></a>Esempio di cursore statico
Questo esempio illustra come usare PDO::prepare con un cursore statico lato server. Per un esempio con un cursore lato client, vedere [Tipi di cursore &#40;driver PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

```
<?php
$database = "AdventureWorks";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$query = "select * from Person.ContactType";
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));
$stmt->execute();

echo "\n";

while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){
   print "$row[Name]\n";
}
echo "\n..\n";

$row = $stmt->fetch( PDO::FETCH_BOTH, PDO::FETCH_ORI_FIRST );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_ASSOC, PDO::FETCH_ORI_REL, 1 );
print "$row[Name]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_NEXT );
print "$row[1]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_PRIOR );
print "$row[1]..\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_ABS, 0 );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_LAST );
print_r($row);
?>
```

## <a name="targeted-example"></a>Esempio di destinazione
I due frammenti di codice seguenti illustrano come usare PDO::prepare con i dati destinati alle colonne CHAR/VARCHAR. Poiché la codifica predefinita per PDO::prepare è UTF-8, l'utente può usare l'opzione `PDO::SQLSRV_ENCODING_SYSTEM` per evitare le conversioni implicite.

**Opzione 1**
```
$options = array(PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_SYSTEM);
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue',
  $options
);

$statement->bindValue(':myVarcharValue', 'my data', PDO::PARAM_STR);
```

**Opzione 2**
```
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue'
);
$p = 'my data';
$statement->bindParam(':myVarcharValue', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_SYSTEM);
```

<a name="emulate-prepare" />

## <a name="prepare-example"></a>Esempio di preparazione

Questo esempio illustra come usare PDO::prepare con `PDO::ATTR_EMULATE_PREPARES` impostato su true.

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$database = "tempdb";
$conn = new PDO("sqlsrv:server = $serverName; Database = $database", $username, $password);

$pdo_options = array();
$pdo_options[PDO::ATTR_EMULATE_PREPARES] = true;
$pdo_options[PDO::SQLSRV_ATTR_ENCODING] = PDO::SQLSRV_ENCODING_UTF8;

$stmt = $conn->prepare("CREATE TABLE TEST([id] [int] IDENTITY(1,1) NOT NULL,
                                          [name] nvarchar(max))",
                                          $pdo_options);
$stmt->execute();

$prefix = '가각';
$name = '가각ácasa';
$name2 = '가각sample2';

$stmt = $conn->prepare("INSERT INTO TEST(name) VALUES(:p0)", $pdo_options);
$stmt->execute(['p0' => $name]);
unset($stmt);

$stmt = $conn->prepare("SELECT * FROM TEST WHERE NAME LIKE :p0", $pdo_options);
$stmt->execute(['p0' => "$prefix%"]);
foreach ($stmt as $row) {
    echo "\n" . 'FOUND: ' . $row['name'];
}

unset($stmt);
unset($conn);
?>
```

Il driver PDO_SQLSRV sostituisce internamente tutti i segnaposto con i parametri associati da [PDOStatement::bindParam()](../../connect/php/pdostatement-bindparam.md). Al server viene pertanto inviata una stringa di query SQL senza segnaposto. Si consideri questo esempio:

```
$statement = $PDO->prepare("INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)");
$statement->bindParam(:cus_name, "Cardinal");
$statement->bindParam(:con_name, "Tom B. Erichsen");
$statement->execute();
```

Con `PDO::ATTR_EMULATE_PREPARES` impostato su false (caso predefinito), i dati inviati al database sono i seguenti:

```
"INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)"
Information on :cus_name parameter
Information on :con_name parameter
```

Il server eseguirà la query usando la funzionalità delle query con parametri per l'associazione dei parametri. Con `PDO::ATTR_EMULATE_PREPARES` impostato su true, invece, la query inviata al server è essenzialmente la seguente:

```
"INSERT into Customers (CustomerName, ContactName) VALUES ('Cardinal', 'Tom B. Erichsen')"
```

L'impostazione di `PDO::ATTR_EMULATE_PREPARES` su true consente di ignorare alcune restrizioni di SQL Server. Ad esempio, SQL Server non supporta parametri denominati o posizionali in alcune clausole Transact-SQL. SQL Server limita inoltre l'associazione a 2100 parametri.

> [!NOTE]
> Con EMULATE_PREPARES impostato su true, la sicurezza delle query con parametri non viene applicata. Pertanto l'applicazione deve verificare che i dati associati ai parametri non contengano codice Transact-SQL dannoso.

### <a name="encoding"></a>Codifica

Per associare parametri con codifiche diverse, ad esempio UTF-8 o binaria, l'utente dovrà specificare chiaramente la codifica nello script PHP.

Il driver PDO_SQLSRV controlla prima di tutto la codifica specificata in `PDO::bindParam()`, ad esempio `$statement->bindParam(:cus_name, "Cardinal", PDO::PARAM_STR, 10, PDO::SQLSRV_ENCODING_UTF8)`.

Se non è presente, il driver controlla se è impostata una codifica in `PDO::prepare()` o `PDOStatement::setAttribute()`. In caso contrario, il driver userà la codifica specificata in `PDO::__construct()` o `PDO::setAttribute()`.

Inoltre, a partire dalla versione 5.8.0, quando si usa PDO::prepare con `PDO::ATTR_EMULATE_PREPARES` impostato su true, l'utente può usare [i tipi di stringhe estesi introdotti in PHP 7.2](https://wiki.php.net/rfc/extended-string-types-for-pdo) per assicurarsi che venga usato il prefisso `N`. I frammenti di codice seguenti mostrano diverse alternative.

> [!NOTE]
> Per impostazione predefinita, emulate prepares è impostato su false e in questo caso le costanti di tipo stringa PDO estese verranno ignorate.

**Uso dell'opzione del driver PDO::SQLSRV_ENCODING_UTF8 durante l'associazione**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_UTF8);
$stmt->execute();
```

**Uso dell'attributo PDO::SQLSRV_ATTR_ENCODING**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true, PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_UTF8);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

**Uso della costante PDO PDO::PARAM_STR_NATL**
```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR | PDO::PARAM_STR_NATL);
$stmt->execute();
```

**Impostazione del tipo di parametro di stringa predefinito PDO::PARAM_STR_NATL**
```
$conn->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

### <a name="limitations"></a>Limitazioni

Come è possibile osservare, l'associazione viene eseguita internamente dal driver. Al server viene inviata per l'esecuzione una query valida senza parametri. Rispetto al caso normale, quando la funzionalità delle query con parametri non viene usata sussistono alcune limitazioni.

- Non funziona per i parametri associati come `PDO::PARAM_INPUT_OUTPUT`.
    - Quando l'utente specifica `PDO::PARAM_INPUT_OUTPUT` in `PDO::bindParam()`, viene generata un'eccezione PDO.
- Non funziona per i parametri associati come parametri di output.
    - Quando l'utente crea un'istruzione preparata con segnaposto per i parametri di output (ossia con un segno uguale subito dopo il segnaposto, come `SELECT ? = COUNT(*) FROM Table1`), viene generata un'eccezione PDO.
    - Quando un'istruzione preparata richiama una stored procedure con un segnaposto come argomento per un parametro di output, non viene generata alcuna eccezione perché il driver non può rilevare il parametro di output. La variabile specificata dall'utente per il parametro di output rimane tuttavia invariata.
- I segnaposto duplicati per un parametro con codifica binaria non funzioneranno.

## <a name="see-also"></a>Vedere anche
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)

