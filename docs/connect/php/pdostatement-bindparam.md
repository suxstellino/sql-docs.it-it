---
title: PDOStatement::bindParam
description: Informazioni di riferimento sulle API per la funzione PDOStatement::bindParam nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 65212058-2632-47a4-ba7d-2206883abf09
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0d0ad16359ccf5cd2fe5da71469e8e51d2fac7ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195213"
---
# <a name="pdostatementbindparam"></a>PDOStatement::bindParam
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Associa un parametro a un segnaposto denominato o punto interrogativo nell'istruzione SQL.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
bool PDOStatement::bindParam($parameter, &$variable[, $data_type[, $length[, $driver_options]]]);  
```  
  
#### <a name="parameters"></a>Parametri  
$*parameter*: identificatore del parametro (misto). Per un'istruzione che usa segnaposti denominati, usare un nome parametro (:name). Per un'istruzione preparata che usa la sintassi punto interrogativo, è l'indice in base 1 del parametro.  
  
&$*variable*: nome (misto) della variabile PHP da associare al parametro dell'istruzione SQL.  
  
$*data_type*: costante (intero) PDO::PARAM_* facoltativa. Il valore predefinito è PDO::PARAM_STR.  
  
$*length*: lunghezza (intero) facoltativa del tipo di dati. È possibile specificare PDO::SQLSRV_PARAM_OUT_DEFAULT_SIZE per indicare le dimensioni predefinite quando si usa PDO::PARAM_INT o PDO::PARAM_BOOL in $*data_type*.  
  
$*driver_options*: opzioni specifiche del driver (miste) facoltative. Ad esempio, è possibile specificare PDO::SQLSRV_ENCODING_UTF8 per associare la colonna a una variabile come stringa codificata in UTF-8.  
  
## <a name="return-value"></a>Valore restituito  
TRUE in caso di esito positivo; in caso contrario, FALSE.  
  
## <a name="remarks"></a>Osservazioni  
Quando si associano dati Null alle colonne del server di tipo varbinary, binary o varbinary(max), è consigliabile specificare la codifica binaria (PDO::SQLSRV_ENCODING_BINARY) usando $*driver_options*. Per altre informazioni sulle costanti di codifica, vedere [Costanti](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  

## <a name="parameter-example"></a>Esempio di parametro  
In questo esempio viene illustrato che dopo l'associazione di $contact al parametro, la modifica del valore non modifica il valore passato nella query.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = ?");  
$stmt->bindParam(1, $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
  
$stmt = null;  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = :contact");  
$stmt->bindParam(':contact', $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
?>  
```  
  
## <a name="output-parameter-example"></a>Esempio di parametro di output  
In questo esempio di codice viene illustrato come accedere a un parametro di output.  
  
```  
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$input1 = 'bb';  
  
$stmt = $conn->prepare("select ? = count(*) from Sys.tables");  
$stmt->bindParam(1, $input1, PDO::PARAM_STR, 10);  
$stmt->execute();  
echo $input1;  
?>  
```  
  
> [!NOTE]
> Quando si associa un parametro di output a un tipo bigint, se esiste la possibilità che il valore venga escluso dall'intervallo di un [Integer](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), l'uso di PDO::PARAM_INT con PDO::SQLSRV_PARAM_OUT_DEFAULT_SIZE può generare un'eccezione "Valore esterno all'intervallo consentito". Usare invece il valore PDO::PARAM_STR predefinito e specificare le dimensioni della stringa risultante, che sono al massimo pari a 21, ovvero il numero massimo di cifre, incluso il segno meno, di qualsiasi valore bigint. 

## <a name="inputoutput-example"></a>Esempio di input/output  
In questo esempio di codice viene illustrato come usare un parametro di input/output.  
  
```  
<?php  
   $database = "AdventureWorks";  
   $server = "(local)";  
   $dbh = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
   $dbh->query("IF OBJECT_ID('dbo.sp_ReverseString', 'P') IS NOT NULL DROP PROCEDURE dbo.sp_ReverseString");  
   $dbh->query("CREATE PROCEDURE dbo.sp_ReverseString @String as VARCHAR(2048) OUTPUT as SELECT @String = REVERSE(@String)");  
   $stmt = $dbh->prepare("EXEC dbo.sp_ReverseString ?");  
   $string = "123456789";  
   $stmt->bindParam(1, $string, PDO::PARAM_STR | PDO::PARAM_INPUT_OUTPUT, 2048);  
   $stmt->execute();  
   print $string;   // Expect 987654321  
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
$input = "9223372036854.80000";
$stmt = $conn->prepare("INSERT INTO TestTable (DecimalCol) VALUES (?)");
// by default it is PDO::PARAM_STR, rounding of a large input value may
// occur if PDO::PARAM_INT is specified
$stmt->bindParam(1, $input, PDO::PARAM_STR);
$stmt->execute();
```


## <a name="see-also"></a>Vedere anche  
[Classe PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
