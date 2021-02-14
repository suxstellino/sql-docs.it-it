---
title: Esecuzione di istruzioni diretta e preparata nel driver PDO_SQLSRV
description: Informazioni su come usare l'attributo PDO::SQLSRV_ATTR_DIRECT_QUERY per l'esecuzione di istruzioni diretta quando si usa il driver Microsoft PDO_SQLSRV per PHP per SQL Server
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 05544ca6-1e07-486c-bf03-e8c2c25b3024
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4dc9177ed83e349b4cdd3d38da34285ee90a1f1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045391"
---
# <a name="direct-statement-execution-and-prepared-statement-execution-in-the-pdo_sqlsrv-driver"></a>Esecuzione di istruzioni diretta e preparata nel driver PDO_SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Questo argomento illustra come usare l'attributo PDO::SQLSRV_ATTR_DIRECT_QUERY per specificare l'esecuzione diretta delle istruzioni anziché l'impostazione predefinita, che corrisponde all'esecuzione di istruzioni preparate. L'uso di un'istruzione preparata può determinare prestazioni migliori se l'istruzione viene eseguita più volte tramite l'associazione di parametri.  
  
## <a name="remarks"></a>Osservazioni  
Se si vuole inviare un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] direttamente al server senza la preparazione dell'istruzione da parte del driver, è possibile impostare l'attributo PDO::SQLSRV_ATTR_DIRECT_QUERY con [DOP::SetAttribute](../../connect/php/pdo-setattribute.md) (o come opzione del driver passata a [PDO::__construct](../../connect/php/pdo-construct.md)) o quando si chiama [PDO::prepare](../../connect/php/pdo-prepare.md). Per impostazione predefinita, il valore di DOP::SQLSRV_ATTR_DIRECT_QUERY è False (usare l'esecuzione dell'istruzione preparata).  
  
Se si usa [DOP::query](../../connect/php/pdo-query.md), potrebbe essere necessaria l'esecuzione diretta. Prima di chiamare [DOP::query](../../connect/php/pdo-query.md), chiamare [DOP::SetAttribute](../../connect/php/pdo-setattribute.md) e impostare DOP::SQLSRV_ATTR_DIRECT_QUERY su True.  Ogni chiamata a [PDO::query](../../connect/php/pdo-query.md) può essere eseguita con un'impostazione diversa per PDO::SQLSRV_ATTR_DIRECT_QUERY.  
  
Se si usa [PDO::prepare](../../connect/php/pdo-prepare.md) e [PDOStatement::execute](../../connect/php/pdostatement-execute.md) per eseguire una query più volte tramite parametri associati, l'esecuzione dell'istruzione preparata ottimizza l'esecuzione della query ripetuta.  In questa situazione, chiamare [PDO::prepare](../../connect/php/pdo-prepare.md) con PDO::SQLSRV_ATTR_DIRECT_QUERY impostato su False nel parametro di matrice delle opzioni del driver. Se necessario, è possibile eseguire le istruzioni preparate con PDO::SQLSRV_ATTR_DIRECT_QUERY impostato su False.  
  
Dopo aver chiamato [PDO::prepare](../../connect/php/pdo-prepare.md), il valore di PDO::SQLSRV_ATTR_DIRECT_QUERY non può essere modificato quando si esegue la query preparata.  
  
Se una query richiede il contesto impostato in una query precedente, eseguire le query con PDO::SQLSRV_ATTR_DIRECT_QUERY impostato su True. Se ad esempio si usano tabelle temporanee nelle query, PDO::SQLSRV_ATTR_DIRECT_QUERY deve essere impostato su True.  
  
L'esempio seguente illustra che, quando viene richiesto contesto da un'istruzione precedente, è necessario impostare PDO::SQLSRV_ATTR_DIRECT_QUERY su True. In questo esempio vengono usate le tabelle temporanee, che sono disponibili solo per le istruzioni successive nel programma quando le query vengono eseguite direttamente.  
  
> [!NOTE]
> Se la query consiste nel richiamare una stored procedure in cui vengono usate tabelle temporanee, usare invece [PDO::exec](../../connect/php/pdo-exec.md).

```  
<?php  
   $conn = new PDO('sqlsrv:Server=(local)', '', '');  
   $conn->setAttribute(constant('PDO::SQLSRV_ATTR_DIRECT_QUERY'), true);  
  
   $stmt1 = $conn->query("DROP TABLE #php_test_table");  
  
   $stmt2 = $conn->query("CREATE TABLE #php_test_table ([c1_int] int, [c2_int] int)");  
  
   $v1 = 1;  
   $v2 = 2;  
  
   $stmt3 = $conn->prepare("INSERT INTO #php_test_table (c1_int, c2_int) VALUES (:var1, :var2)");  
  
   if ($stmt3) {  
      $stmt3->bindValue(1, $v1);  
      $stmt3->bindValue(2, $v2);  
  
      if ($stmt3->execute())  
         echo "Execution succeeded\n";       
      else  
         echo "Execution failed\n";  
   }  
   else  
      var_dump($conn->errorInfo());  
  
   $stmt4 = $conn->query("DROP TABLE #php_test_table");  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla programmazione per i driver Microsoft per PHP per SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)
  
