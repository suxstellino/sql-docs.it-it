---
title: PDO::errorInfo
description: Informazioni di riferimento sulle API per la funzione PDO::errorInfo nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46b57275d92f4eb6acb64c276e3b34ea67efb429
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202031"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera informazioni dettagliate sugli errori dell'ultima operazione dell'handle di database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Valore restituito  
Una matrice di informazioni sugli errori dell'ultima operazione dell'handle di database. La matrice include i campi seguenti:  
  
-   Codice di errore SQLSTATE.  
  
-   Codice di errore specifico del driver.  
  
-   Messaggio di errore specifico del driver.  
  
Se non si verificano errori o se il valore di SQLSTATE non è impostato, i campi specifici del driver sono NULL.  
  
## <a name="remarks"></a>Osservazioni  
PDO::errorInfo recupera solo le informazioni sugli errori per le operazioni eseguite direttamente sul database. Usare PDOStatement::errorInfo quando viene creata un'istanza di PDOStatement mediante PDO::prepare o PDO::query.  
  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="example"></a>Esempio  
In questo esempio il nome della colonna errato (`Cityx` anziché `City`) causa un errore, che viene quindi segnalato.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  

## <a name="additional-odbc-messages"></a>Messaggi ODBC aggiuntivi

Quando si verifica un'eccezione, il driver ODBC può restituire più di un errore per facilitare la diagnosi dei problemi. Tuttavia, DOP:: errorInfo Visualizza sempre solo il primo errore. In risposta a questo [report sui bug](https://bugs.php.net/bug.php?id=78196), [DOP:: ErrorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) e [PDOStatement:: ErrorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) sono stati aggiornati per indicare *che i driver dovrebbero visualizzare almeno* i tre campi seguenti:
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

A partire da 5.9.0, il comportamento predefinito di DOP:: errorInfo è quello di visualizzare altri errori ODBC, se disponibili. Ad esempio:

```php
<?php  
try {
    $conn = new PDO("sqlsrv:server=$server;", $uid, $pwd);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SET NOCOUNT ON; USE $database; SELECT 1/0 AS col1");
    $stmt->execute();
} catch (PDOException $e) {
    var_dump($e->errorInfo);
}
?>  
```  

L'esecuzione dello script precedente dovrebbe avere generato un'eccezione e l'output è simile al seguente:

```
array(6) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
  [3]=>
  string(5) "22012"
  [4]=>
  int(8134)
  [5]=>
  string(87) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Divide by zero error encountered."
}
```

Se l'utente preferisce il modo precedente, è possibile usare una nuova opzione di configurazione per disattivarla `pdo_sqlsrv.report_additional_errors` . È sufficiente aggiungere la riga seguente all'inizio di uno script php:

```
ini_set('pdo_sqlsrv.report_additional_errors', 0);
```

In questo caso, le informazioni sull'errore visualizzate saranno simili alle seguenti, quando si esegue lo stesso script di esempio:

```
array(3) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
}
```

Se necessario, l'utente può scegliere di aggiungere la riga seguente al file di php.ini per disattivare questa funzionalità in tutti gli script php:

```
pdo_sqlsrv.report_additional_errors = 0
```

## <a name="warnings-and-errors"></a>Avvisi ed errori

A partire da 5.9.0, gli avvisi ODBC non verranno più registrati come errori. Ovvero, i [codici di errore](https://docs.microsoft.com/sql/odbc/reference/appendixes/appendix-a-odbc-error-codes) con prefisso "01" vengono registrati come avvisi. In altre parole, se l'utente vuole registrare solo gli errori, aggiornare il php.ini come segue:

```
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = 1
```

In questo caso, il file di log non conterrà alcun messaggio di avviso. Verificare il funzionamento della [registrazione](https://docs.microsoft.com/sql/connect/php/logging-activity#logging-activity-using-the-pdo_sqlsrv-driver) per PDO_SQLSRV gli utenti.

## <a name="see-also"></a>Vedere anche  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  

[PDOStatement::errorInfo](../../connect/php/pdostatement-errorinfo.md)
