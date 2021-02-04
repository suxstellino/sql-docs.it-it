---
title: sqlsrv_prepare
description: Informazioni di riferimento sulle API per la funzione sqlsrv_prepare nel driver Microsoft SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 04/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_prepare
apitype: NA
helpviewer_keywords:
- executing queries
- API Reference, sqlsrv_prepare
- sqlsrv_prepare
ms.assetid: 8c74c697-3296-4f5d-8fb9-e361f53f19a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 566b5804a042e96bb18c37c5b4405178bd8899f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201090"
---
# <a name="sqlsrv_prepare"></a>sqlsrv_prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crea una risorsa di istruzione associata alla connessione specificata. Questa funzione è utile per l'esecuzione di più query.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_prepare(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Parametri  
*$conn*: risorsa di connessione associata all'istruzione creata.  
  
*$tsql*: espressione Transact-SQL corrispondente all'istruzione creata.  
  
*$params* [FACOLTATIVO]: **matrice** di valori corrispondenti ai parametri di una query con parametri. Ogni elemento della matrice può corrispondere a uno dei seguenti:
  
-   Valore letterale.  
  
-   Riferimento a una variabile PHP.  
  
-   **Matrice** con la struttura seguente:  
  
    ```  
    array(&$value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    > [!NOTE]  
    > Le variabili passate come parametri di query devono essere passate per riferimento e non per valore. Ad esempio, passare `&$myVariable` anziché `$myVariable`. Se viene eseguita una query con parametri per valore, viene generato un avviso PHP.  
  
    Nella tabella riportata di seguito vengono descritti gli elementi di matrice seguenti:  
  
    |Elemento|Descrizione|  
    |-----------|---------------|  
    |*&$value*|Valore letterale o riferimento a una variabile PHP.|  
    |*$direction*[facoltativo]|Una delle seguenti **SQLSRV_PARAM_ \* *_ costanti usate per indicare la direzione del parametro: _* SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. Il valore predefinito è **SQLSRV_PARAM_IN**.<br /><br />Per altre informazioni sulle costanti PHP, vedere [Costanti &#40;driver Microsoft per PHP per SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[facoltativo]|Una costante ** \* SQLSRV_PHPTYPE_* _ che specifica il tipo di dati php del valore restituito.|  
    |_ $ SqlType * [facoltativo]|Una costante ** \* SQLSRV_SQLTYPE_* _ che specifica il tipo di dati SQL Server del valore di input.|  
  
_ $ options * [facoltativo]: matrice associativa che imposta le <a name="properties">proprietà delle query</a>. Nella tabella seguente sono elencate le chiavi supportate e i valori corrispondenti:

|Chiave|Valori supportati|Descrizione|  
|-------|--------------------|---------------|  
|ClientBufferMaxKBSize|Intero positivo|Consente di configurare le dimensioni del buffer che contiene il set di risultati per un cursore sul lato client.<br /><br />Il valore predefinito è 10240 KB. Per altre informazioni, vedere [Specifica di un tipo di cursore e selezione di righe](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|
|DecimalPlaces|Un numero intero compreso tra 0 e 4 inclusi|Specifica i numeri decimali quando si formattano valori money recuperati.<br /><br />Qualsiasi numero intero negativo o valore maggiore di 4 verrà ignorato.<br /><br />Questa opzione funziona solo quando FormatDecimals è **true**.|
|FormatDecimals|**true** o **false**<br /><br />Il valore predefinito è **false**.|Specifica se aggiungere zeri iniziali nelle stringhe decimali quando appropriato e abilita l'opzione `DecimalPlaces` per la formattazione dei tipi money.<br /><br />Per altre informazioni, vedere [Formattazione di stringhe decimali e valori money (driver SQLSRV)](../../connect/php/formatting-decimals-sqlsrv-driver.md).|
|QueryTimeout|Intero positivo|Imposta il timeout in secondi della query. Per impostazione predefinita il driver attende i risultati per un periodo illimitato.|  
|ReturnDatesAsStrings|**true** o **false**<br /><br />Il valore predefinito è **false**.|Configura l'istruzione per recuperare i tipi di data e ora come stringhe (**true**). Per altre informazioni, vedere [Procedura: Recuperare i tipi di data e ora come stringhe usando il driver SQLSRV](../../connect/php/how-to-retrieve-date-and-time-type-as-strings-using-the-sqlsrv-driver.md).
|Scorrimento|SQLSRV_CURSOR_FORWARD<br /><br />SQLSRV_CURSOR_STATIC<br /><br />SQLSRV_CURSOR_DYNAMIC<br /><br />SQLSRV_CURSOR_KEYSET<br /><br />SQLSRV_CURSOR_CLIENT_BUFFERED|Per altre informazioni su questi valori, vedere [Specifica di un tipo di cursore e selezione di righe](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|  
|SendStreamParamsAtExec|**true** o **false**<br /><br />Il valore predefinito è **true**.|Configura il driver per l'invio di tutti i flussi di dati in fase di esecuzione (**true**) o in blocchi (**false**). Per impostazione predefinita, il valore è impostato su **true**. Per altre informazioni, vedere [sqlsrv_send_stream_data](../../connect/php/sqlsrv-send-stream-data.md).|  
  
## <a name="return-value"></a>Valore restituito  
Risorsa di istruzione. Se non è possibile creare la risorsa di istruzione, viene restituito **false** .  
  
## <a name="remarks"></a>Osservazioni  
Quando si prepara un'istruzione che usa variabili come parametri, le variabili vengono associate all'istruzione. Ciò significa che se si aggiornano i valori delle variabili, alla successiva esecuzione l'istruzione verrà eseguita con i valori aggiornati dei parametri.  
  
La combinazione di **sqlsrv_prepare** e **sqlsrv_execute** , che separa la preparazione e l'esecuzione dell'istruzione in due chiamate di funzione, può essere usata per eseguire query con parametri. Questa funzione è ideale per eseguire un'istruzione più volte con valori di parametro diversi per ciascuna esecuzione.  
  
Per strategie alternative per la scrittura e la lettura di grandi quantità di informazioni, vedere la pagina relativa ai [batch di istruzioni SQL](../../odbc/reference/develop-app/batches-of-sql-statements.md) e la pagina [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
Per altre informazioni, vedere [Procedura: Recuperare i parametri di output mediante il driver SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example-1"></a>Esempio 1  
Nell'esempio seguente viene preparata ed eseguita un'istruzione. Quando viene eseguita (vedere [sqlsrv_execute](../../connect/php/sqlsrv-execute.md)), l'istruzione aggiorna un campo nella tabella *Sales.SalesOrderDetail* del database AdventureWorks. Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up Transact-SQL query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = ?   
         WHERE SalesOrderDetailID = ?";  
  
/* Assign parameter values. */  
$param1 = 5;  
$param2 = 10;  
$params = array(&$param1, &$param2);  
  
/* Prepare the statement. */  
if ($stmt = sqlsrv_prepare($conn, $tsql, $params)) {
    echo "Statement prepared.\n";  
} else {  
    echo "Statement could not be prepared.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement. */  
if (sqlsrv_execute($stmt)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Statement could not be executed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example-2"></a>Esempio 2  
Nell'esempio seguente viene illustrato come preparare un'istruzione ed eseguirla nuovamente con valori diversi per i parametri. L'esempio aggiorna la colonna *OrderQty* della tabella *Sales.SalesOrderDetail* nel database AdventureWorks. Dopo gli aggiornamenti, viene eseguita una query nel database per verificare che gli aggiornamenti siano stati completati correttamente. Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
     echo "Could not connect.\n";  
     die(print_r(sqlsrv_errors(), true));  
}  
  
/* Define the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail  
         SET OrderQty = ?  
         WHERE SalesOrderDetailID = ?";  
  
/* Initialize parameters and prepare the statement. Variables $qty  
and $id are bound to the statement, $stmt1. */  
$qty = 0; $id = 0;  
$stmt1 = sqlsrv_prepare($conn, $tsql, array(&$qty, &$id));  
if ($stmt1) {  
    echo "Statement 1 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the SalesOrderDetailID and OrderQty information. This array  
maps the order ID to order quantity in key=>value pairs. */  
$orders = array(1=>10, 2=>20, 3=>30);  
  
/* Execute the statement for each order. */  
foreach ($orders as $id => $qty) {  
    // Because $id and $qty are bound to $stmt1, their updated  
    // values are used with each execution of the statement.   
    if (sqlsrv_execute($stmt1) === false) {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
echo "Orders updated.\n";  
  
/* Free $stmt1 resources.  This allows $id and $qty to be bound to a different statement.*/  
sqlsrv_free_stmt($stmt1);  
  
/* Now verify that the results were successfully written by selecting   
the newly inserted rows. */  
$tsql = "SELECT OrderQty   
         FROM Sales.SalesOrderDetail   
         WHERE SalesOrderDetailID = ?";  
  
/* Prepare the statement. Variable $id is bound to $stmt2. */  
$stmt2 = sqlsrv_prepare($conn, $tsql, array(&$id));  
if ($stmt2) {  
    echo "Statement 2 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement for each order. */  
foreach (array_keys($orders) as $id)  
{  
    /* Because $id is bound to $stmt2, its updated value   
    is used with each execution of the statement. */  
    if (sqlsrv_execute($stmt2)) {  
        sqlsrv_fetch($stmt2);  
        $quantity = sqlsrv_get_field($stmt2, 0);  
        echo "Order $id is for $quantity units.\n";  
    } else {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
  
/* Free $stmt2 and connection resources. */  
sqlsrv_free_stmt($stmt2);  
sqlsrv_close($conn);  
?>  
```  
  
> [!NOTE]
> È consigliabile usare stringhe come input durante l'associazione di valori a una [colonna decimal o numeric](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) per garantire precisione e accuratezza, dato che PHP offre una precisione limitata per i [numeri a virgola mobile](https://php.net/manual/en/language.types.float.php). Lo stesso vale per le colonne di tipo bigint, soprattutto quando i valori non sono compresi nell'intervallo di un [integer](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="example-3"></a>Esempio 3  
Questo esempio di codice mostra come associare un valore decimale come parametro di input.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"YourTestDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume TestTable exists with a decimal field 
$input = "9223372036854.80000";
$params = array($input);
$stmt = sqlsrv_prepare($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);
sqlsrv_execute($stmt);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Vedere anche  
[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Procedura: Eseguire query con parametri](../../connect/php/how-to-perform-parameterized-queries.md)

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)

[Procedura: Inviare dati come flusso](../../connect/php/how-to-send-data-as-a-stream.md)

[Uso dei parametri direzionali](../../connect/php/using-directional-parameters.md)

[Recupero di dati](../../connect/php/retrieving-data.md)

[Aggiornamento dei dati &#40;Driver Microsoft per PHP per SQL Server&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)