---
title: Recuperare i tipi di data e ora come stringhe usando il driver SQLSRV
description: Informazioni su come recuperare i tipi data e ora come stringhe usando il driver SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 02/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- date and time types, retrieving as strings
ms.assetid: 58a974ea-4daf-4e3b-98ed-9731b9c9250f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 31e4daeab0019aaf2ea9eb4a26af3f47c564c680
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495639"
---
# <a name="how-to-retrieve-date-and-time-types-as-strings-using-the-sqlsrv-driver"></a>Procedura: Recuperare i tipi di data e ora come stringhe usando il driver SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Quando si usa il driver SQLSRV per [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], è possibile recuperare i tipi di data e ora (**smalldatetime**, **datetime**, **date**, **time**, **datetime2** e **datetimeoffset**) come stringhe specificando l'opzione seguente nella stringa di connessione o a livello di istruzione:

```
'ReturnDatesAsStrings'=>true
```

Il valore predefinito è **false**, vale a dire che i tipi **smalldatetime**, **datetime**, **date**, **time**, **datetime2** e **datetimeoffset** verranno restituiti come oggetti [DateTime PHP](http://php.net/manual/en/class.datetime.php). Se questa opzione è impostata a livello di istruzione, esegue l'override dell'impostazione a livello di connessione.

Per impostazione predefinita, il driver PDO_SQLSRV restituisce i tipi di data e ora come stringhe. Per recuperarli come oggetti DateTime PHP, vedere [Procedura: Recuperare i tipi di data e ora come oggetti Datetime PHP usando PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)

## <a name="example-1"></a>Esempio 1
L'esempio seguente illustra la sintassi per specificare il recupero dei tipi di data e ora come stringhe.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks", 'ReturnDatesAsStrings'=> true);
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_close($conn);
?>
```

## <a name="example-2"></a>Esempio 2
L'esempio seguente illustra che è possibile recuperare le date come stringhe specificando UTF-8 in fase di recupero della stringa, anche quando la connessione è stata stabilita con `"ReturnDatesAsStrings" => false`.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks", "ReturnDatesAsStrings" => false);
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tsql = "SELECT VersionDate FROM AWBuildVersion";

$stmt = sqlsrv_query($conn, $tsql);

if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_fetch($stmt);

// retrieve date as string
$date = sqlsrv_get_field($stmt, 0, SQLSRV_PHPTYPE_STRING("UTF-8"));

if ($date === false) {
   die(print_r(sqlsrv_errors(), true));
}

echo $date;

sqlsrv_close($conn);
?>
```

## <a name="example-3"></a>Esempio 3
L'esempio seguente illustra come recuperare le date come stringhe specificando UTF-8 e `"ReturnDatesAsStrings" => true` nella stringa di connessione.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks", 'ReturnDatesAsStrings'=> true, "CharacterSet" => 'utf-8');
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tsql = "SELECT VersionDate FROM AWBuildVersion";

$stmt = sqlsrv_query($conn, $tsql);

if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_fetch($stmt);

// retrieve date as string
$date = sqlsrv_get_field($stmt, 0);

if ($date === false) {
   die(print_r(sqlsrv_errors(), true));
}

echo $date;
sqlsrv_close($conn);
?>
```

## <a name="example-4"></a>Esempio 4
L'esempio seguente illustra come recuperare la data come tipo PHP. `'ReturnDatesAsStrings'=> false` è attivato per impostazione predefinita.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tsql = "SELECT VersionDate FROM AWBuildVersion";

$stmt = sqlsrv_query($conn, $tsql);

if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_fetch($stmt);

// retrieve date as a DateTime object, then convert to string using PHP's date_format function
$date = sqlsrv_get_field($stmt, 0);

if ($date === false) {
   die(print_r(sqlsrv_errors(), true));
}

$date_string = date_format($date, 'jS, F Y');
echo "Date = $date_string\n";

sqlsrv_close($conn);
?>
```

## <a name="example-5"></a>Esempio 5
L'opzione ReturnDatesAsStrings a livello di istruzione esegue l'override dell'opzione di connessione corrispondente.

```php
<?php
$serverName = 'MyServer';
$connectionInfo = array('Database' => 'MyDatabase', 'ReturnDatesAsStrings' => false);
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tableName = 'MyTable';
$options = array('ReturnDatesAsStrings' => true);
$query = "SELECT DateTimeCol FROM $tableName";
$stmt = sqlsrv_prepare($conn, $query, array(), $options);
if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}
sqlsrv_execute($stmt);

// Expect the fetched value to be a string
$field = sqlsrv_get_field($stmt, 0);
echo $field . PHP_EOL;

sqlsrv_close($conn);
?>
```

## <a name="see-also"></a>Vedere anche
[Recupero di dati](../../connect/php/retrieving-data.md)

[Procedura: Recuperare i tipi di data e ora come oggetti Datetime PHP usando PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)
