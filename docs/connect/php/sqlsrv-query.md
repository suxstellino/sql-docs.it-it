---
title: sqlsrv_query
description: La funzione sqlsrv_query offre un metodo per eseguire una query con una quantità minima di codice e può essere usata per eseguire query con parametri.
ms.custom: ''
ms.date: 04/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_query
apitype: NA
helpviewer_keywords:
- sqlsrv_query
- executing queries
- API Reference, sqlsrv_query
ms.assetid: 9fa7c4c8-4da8-4299-9893-f61815055aa3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 751fd08e63cccfe4293785191a223728690ed13e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201077"
---
# <a name="sqlsrv_query"></a>sqlsrv_query
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prepara ed esegue un'istruzione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_query(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Parametri  
*$conn*: risorsa di connessione associata all'istruzione preparata.  
  
*$tsql*: espressione Transact-SQL corrispondente all'istruzione preparata.  
  
*$params* [FACOLTATIVO]: **matrice** di valori corrispondenti ai parametri di una query con parametri. Ogni elemento della matrice può corrispondere a uno dei seguenti:
  
-   Valore letterale.  
  
-   Variabile PHP.  
  
-   **Matrice** con la struttura seguente:  
  
    ```  
    array($value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    La descrizione di ogni elemento della matrice è visualizzata nella tabella seguente:  
  
    |Elemento|Descrizione|  
    |-----------|---------------|  
    |*$value*|Un valore letterale, una variabile PHP o una variabile PHP per riferimento.|  
    |*$direction*[facoltativo]|Una delle seguenti **SQLSRV_PARAM_ \* *_ costanti usate per indicare la direzione del parametro: _* SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. Il valore predefinito è **SQLSRV_PARAM_IN**.<br /><br />Per altre informazioni sulle costanti PHP, vedere [Costanti &#40;driver Microsoft per PHP per SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[facoltativo]|Una costante ** \* SQLSRV_PHPTYPE_* _ che specifica il tipo di dati php del valore restituito.<br /><br />Per altre informazioni sulle costanti PHP, vedere [Costanti &#40;driver Microsoft per PHP per SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |_ $ SqlType * [facoltativo]|Una costante ** \* SQLSRV_SQLTYPE_* _ che specifica il tipo di dati SQL Server del valore di input.<br /><br />Per altre informazioni sulle costanti PHP, vedere [Costanti &#40;driver Microsoft per PHP per SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
  
_ $ options * [facoltativo]: matrice associativa che imposta le proprietà delle query. È lo stesso elenco di chiavi supportate anche da [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md#properties).
  
## <a name="return-value"></a>Valore restituito  
Risorsa di istruzione. Se non è possibile creare e/o eseguire l'istruzione, viene restituito **false**.  
  
## <a name="remarks"></a>Osservazioni  
La funzione **sqlsrv_query** è particolarmente adatta alle query eseguite una sola volta e deve essere la scelta predefinita per l'esecuzione di query, a meno che non si verifichino circostanze speciali. Questa funzione fornisce un metodo semplificato per eseguire una query con una quantità minima di codice. La funzione **sqlsrv_query** esegue sia la preparazione che l'esecuzione dell'istruzione e può essere usata per eseguire query con parametri.  
  
Per altre informazioni, vedere [Procedura: Recuperare i parametri di output mediante il driver SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example-1"></a>Esempio 1  
Nell'esempio seguente viene inserita un'unica riga nella tabella *Sales.SalesOrderDetail* del database AdventureWorks. Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
> [!NOTE]  
> Nell'esempio seguente viene usata un'istruzione INSERT per illustrare l'uso di **sqlsrv_query** per l'esecuzione di un'istruzione una sola volta, ma il concetto si applica a qualsiasi istruzione Transact-SQL.  
  
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
  
/* Set up the parameterized query. */  
$tsql = "INSERT INTO Sales.SalesOrderDetail   
        (SalesOrderID,   
         OrderQty,   
         ProductID,   
         SpecialOfferID,   
         UnitPrice,   
         UnitPriceDiscount)  
        VALUES   
        (?, ?, ?, ?, ?, ?)";  
  
/* Set parameter values. */  
$params = array(75123, 5, 741, 1, 818.70, 0.00);  
  
/* Prepare and execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if ($stmt) {  
    echo "Row successfully inserted.\n";  
} else {  
    echo "Row insertion failed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example-2"></a>Esempio 2  
L'esempio seguente aggiorna un campo nella tabella *Sales.SalesOrderDetail* del database AdventureWorks. Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
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
  
/* Set up the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = (?)   
         WHERE SalesOrderDetailID = (?)";  
  
/* Assign literal parameter values. */  
$params = array(5, 10);  
  
/* Execute the query. */  
if (sqlsrv_query($conn, $tsql, $params)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Error in statement execution.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free connection resources. */  
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
$stmt = sqlsrv_query($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="example-4"></a>Esempio 4
Questo esempio illustra come creare una tabella di tipi [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) e recuperare i dati inseriti.

```
<?php
$server = 'serverName';
$dbName = 'databaseName';
$uid = 'yourUserName';
$pwd = 'yourPassword';

$options = array("Database"=>$dbName, "UID"=>$uid, "PWD"=>$pwd);
$conn = sqlsrv_connect($server, $options);
if($conn === false) {
    die(print_r(sqlsrv_errors(), true));
}

$tableName = 'testTable';
$query = "CREATE TABLE $tableName ([c1_int] sql_variant, [c2_varchar] sql_variant)";

$stmt = sqlsrv_query($conn, $query);
if($stmt === false) {
    die(print_r(sqlsrv_errors(), true));
}
sqlsrv_free_stmt($stmt);

$query = "INSERT INTO [$tableName] (c1_int, c2_varchar) VALUES (1, 'test_data')";
$stmt = sqlsrv_query($conn, $query);
if($stmt === false) {
    die(print_r(sqlsrv_errors(), true));
}
sqlsrv_free_stmt($stmt);

$query = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $query);

if(sqlsrv_fetch($stmt) === false) {
    die(print_r(sqlsrv_errors(), true));
}

$col1 = sqlsrv_get_field($stmt, 0);
echo "First field:  $col1 \n";

$col2 = sqlsrv_get_field($stmt, 1);
echo "Second field:  $col2 \n";

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

?>
```

L'output previsto è il seguente:

```
First field:  1
Second field:  test_data
```

## <a name="see-also"></a>Vedere anche  
[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Procedura: Eseguire query con parametri](../../connect/php/how-to-perform-parameterized-queries.md)  

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)  

[Procedura: Inviare dati come flusso](../../connect/php/how-to-send-data-as-a-stream.md)  

[Uso dei parametri direzionali](../../connect/php/using-directional-parameters.md)  

