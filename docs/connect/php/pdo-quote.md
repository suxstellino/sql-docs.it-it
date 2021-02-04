---
title: PDO::quote
description: Informazioni di riferimento sulle API per la funzione PDO::quote nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ab9ddc48-42f8-4edf-aa8b-b0fc66706161
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b9e74b05b48c4e3c225ec37a07408d2ca5c889b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206375"
---
# <a name="pdoquote"></a>PDO::quote
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Elabora una stringa da usare in una query, inserendo le virgolette per racchiudere la stringa di input come richiesto dal database di SQL Server sottostante. PDO::quote ignorerà i caratteri speciali all'interno della stringa di input usando uno stile di delimitazione appropriato per SQL Server.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
string PDO::quote( $string[, $parameter_type ] )  
```  
  
#### <a name="parameters"></a>Parametri  
$*string*: la stringa da delimitare.  
  
$*parameter_type*: simbolo facoltativo (Integer) che indica il tipo di dati.  Il valore predefinito è PDO::PARAM_STR.  

In PHP 7.2 sono state introdotte nuove costanti PDO per aggiungere il supporto per l'[associazione di stringhe Unicode e non Unicode](https://wiki.php.net/rfc/extended-string-types-for-pdo). Le stringhe Unicode possono essere racchiuse tra virgolette con N come prefisso (ad esempio N'string' invece di 'string').

1. PDO::PARAM_STR_NATL: un nuovo tipo per le stringhe Unicode da applicare come OR bit per bit a PDO::PARAM_STR
1. PDO::PARAM_STR_CHAR: un nuovo tipo per le stringhe non Unicode da applicare come operatore OR bit per bit a PDO::PARAM_STR
1. PDO::ATTR_DEFAULT_STR_PARAM: da impostare su PDO::PARAM_STR_NATL o su PDO::PARAM_STR_CHAR per indicare un valore all'operatore OR bit per bit o a PDO::PARAM_STR per impostazione predefinita

A partire dalla versione 5.8.0, è possibile usare queste costanti con PDO::quote.
  
## <a name="return-value"></a>Valore restituito  
Stringa delimitata che può essere passata a un'istruzione SQL oppure FALSE in caso di errore.  
  
## <a name="remarks"></a>Osservazioni  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="string-escape-example"></a>Esempio di string escape  
  
```  
<?php  
$database = "test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$param = 'a \' g';  
$param2 = $conn->quote( $param );  
  
$query = "INSERT INTO Table1 VALUES( ?, '1' )";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param));  
  
$query = "INSERT INTO Table1 VALUES( ?, ? )";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param, $param2));  
?>  
```  
  
## <a name="pdo-quote-example"></a>Esempio di PDO quote  

Lo script seguente mostra alcuni esempi di come i tipi di stringhe estesi influiscono su PDO::quote() con PHP 7.2 e versioni successive.

```
<?php
$database = "test";
$server = "(local)";
$db = new PDO("sqlsrv:server=$server; Database=$database", "", "");

$db->quote('über', PDO::PARAM_STR | PDO::PARAM_STR_NATL); // N'über'
$db->quote('foo'); // 'foo'

$db->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$db->quote('über'); // N'über'
$db->quote('foo', PDO::PARAM_STR | PDO::PARAM_STR_CHAR); // 'foo'
?>
```
  
## <a name="see-also"></a>Vedere anche  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
