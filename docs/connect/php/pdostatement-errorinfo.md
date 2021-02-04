---
title: PDOStatement::errorInfo
description: Informazioni di riferimento sulle API per la funzione PDOStatement::errorInfo nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e45bebe8-ea4c-49b6-93db-cf1ae65f530c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0474b0c1225a248b47f0892ac940f2e58ab0efa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206361"
---
# <a name="pdostatementerrorinfo"></a>PDOStatement::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera informazioni dettagliate sugli errori dell'ultima operazione dell'handle di istruzione.  
  
## <a name="syntax"></a>Sintassi  

```
array PDOStatement::errorInfo();
```
  
## <a name="return-value"></a>Valore restituito  
Una matrice di informazioni sugli errori dell'ultima operazione dell'handle di istruzione. La matrice include i campi seguenti:  
  
-   Codice di errore SQLSTATE  
  
-   Codice di errore specifico del driver.  
  
-   Messaggio di errore specifico del driver.  
  
Se non si verificano errori o se SQLSTATE non è impostata, i campi specifici del driver saranno NULL.  
  
## <a name="remarks"></a>Osservazioni  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="example"></a>Esempio  
In questo esempio, l'istruzione SQL presenta un errore che verrà quindi restituito.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks", "", "");  
$stmt = $conn->prepare('SELECT * FROM Person.Addressx');  
  
$stmt->execute();  
print_r ($stmt->errorInfo());  
?>  
```

## <a name="additional-odbc-messages"></a>Messaggi ODBC aggiuntivi

Quando si verifica un'eccezione, il driver ODBC può restituire più di un errore per facilitare la diagnosi dei problemi. Tuttavia, PDOStatement:: errorInfo Visualizza sempre solo il primo errore. In risposta a questo [report sui bug](https://bugs.php.net/bug.php?id=78196), [DOP:: ErrorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) e [PDOStatement:: ErrorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) sono stati aggiornati per indicare *che i driver dovrebbero visualizzare almeno* i tre campi seguenti:
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

A partire da 5.9.0, il comportamento predefinito di PDOStatement:: errorInfo prevede la visualizzazione di eventuali errori ODBC aggiuntivi, se disponibili. Per ulteriori informazioni, vedere la pagina relativa a [DOP:: ErrorInfo](../../connect/php/pdo-errorinfo.md) .
  
## <a name="see-also"></a>Vedere anche  
[Classe PDOStatement](../../connect/php/pdostatement-class.md)

[PDO::errorInfo](../../connect/php/pdo-errorinfo.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
