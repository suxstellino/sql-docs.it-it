---
title: PDOStatement::bindValue
description: Informazioni di riferimento sulle API per la funzione PDOStatement::bindValue nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 13bc4ece-420e-4887-8809-bf0705ddf126
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40b696cf3cd8cc0c475a052d78966ee45b5f7b72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195210"
---
# <a name="pdostatementbindvalue"></a>PDOStatement::bindValue
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Associa un valore a un segnaposto denominato o punto interrogativo nell'istruzione SQL.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
bool PDOStatement::bindValue($parameter, $value[, $data_type]);  
```  
  
#### <a name="parameters"></a>Parametri  
$*parameter*: identificatore del parametro (misto). Per un'istruzione che usa segnaposti denominati, usare un nome parametro (:name). Per un'istruzione preparata che usa la sintassi punto interrogativo, è l'indice in base 1 del parametro.
  
$*value*: valore (misto) da associare al parametro.  
  
$*data_type*: tipo di dati (intero) facoltativo rappresentato da una costante PDO::PARAM_*. Il valore predefinito è PDO::PARAM_STR.  
  
## <a name="return-value"></a>Valore restituito  
TRUE in caso di esito positivo; in caso contrario, FALSE.  
  
## <a name="remarks"></a>Osservazioni  
  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="parameter-example"></a>Esempio di parametro  
Questo esempio mostra che dopo l'associazione del valore di $contact, la modifica del valore non modifica il valore passato nella query.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = ?");  
$stmt->bindValue(1, $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
  
$stmt = null;  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = :contact");  
$stmt->bindValue(':contact', $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
?>  
```

> [!NOTE]
> È consigliabile usare stringhe come input durante l'associazione di valori a una [colonna decimal o numeric](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) per garantire precisione e accuratezza, dato che PHP offre una precisione limitata per i [numeri a virgola mobile](https://php.net/manual/en/language.types.float.php). Lo stesso vale per le colonne di tipo bigint, soprattutto quando i valori non sono compresi nell'intervallo di un [integer](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="decimal-input-example"></a>Esempio di input decimale  
Questo esempio di codice mostra come associare un valore decimale come parametro di input.  

```
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  

// Assume TestTable exists with a decimal field 
$input = 9223372036854.80000;
$stmt = $conn->prepare("INSERT INTO TestTable (DecimalCol) VALUES (?)");
// by default it is PDO::PARAM_STR, rounding of a large input value may
// occur if PDO::PARAM_INT is specified
$stmt->bindValue(1, $input, PDO::PARAM_STR);
$stmt->execute();
```
  
## <a name="see-also"></a>Vedere anche  
[Classe PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
