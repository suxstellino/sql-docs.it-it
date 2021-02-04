---
title: PDOStatement::fetchAll
description: Informazioni di riferimento sulle API per la funzione PDOStatement::fetchAll nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: be74188a-77cd-4d19-b16e-77278373c979
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2e5e848e7496455d17512bf9f70483f7b5ee8a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206324"
---
# <a name="pdostatementfetchall"></a>PDOStatement::fetchAll
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Restituisce le righe in un set di risultati in una matrice.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
array PDOStatement::fetchAll([ $fetch_style[, $column_index ][, ctor_args]] );  
```  
  
#### <a name="parameters"></a>Parametri  
$*fetch_style*: simbolo (intero) che specifica il formato dei dati della riga. Per un elenco di valori, vedere [PDOStatement::fetch](../../connect/php/pdostatement-fetch.md) . PDO::FETCH_COLUMN è consentito. PDO::FETCH_BOTH è il valore predefinito.  
  
$*column_index*: valore intero che rappresenta la colonna da restituire se $*fetch_style* è PDO::FETCH_COLUMN. 0 è il valore predefinito.  
  
$*ctor_args*: matrice di parametri di un costruttore di classe, quando $*fetch_style* è PDO::FETCH_CLASS o PDO::FETCH_OBJ.  
  
## <a name="return-value"></a>Valore restituito  
Una matrice delle righe rimanenti del set di risultati oppure false se la chiamata al metodo ha esito negativo.  
  
## <a name="remarks"></a>Osservazioni  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="example"></a>Esempio  
  
```  
<?php  
   $server = "(local)";  
   $database = "AdventureWorks";  
   $conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
   print "-----------\n";  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetchall(PDO::FETCH_BOTH);  
   print_r( $result );  
   print "\n-----------\n";  
  
   print "-----------\n";  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetchall(PDO::FETCH_NUM);  
   print_r( $result );  
   print "\n-----------\n";  
  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetchall(PDO::FETCH_COLUMN, 1);  
   print_r( $result );  
   print "\n-----------\n";  
  
   class cc {  
      function __construct( $arg ) {  
         echo "$arg\n";  
      }  
  
      function __toString() {  
         echo "To string\n";  
      }  
   };  
  
   $stmt = $conn->query( 'SELECT TOP(2) * FROM Person.ContactType' );  
   $all = $stmt->fetchAll( PDO::FETCH_CLASS, 'cc', array( 'Hi!' ));  
   var_dump( $all );  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Classe PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
