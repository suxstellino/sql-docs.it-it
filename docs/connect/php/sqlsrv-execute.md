---
description: sqlsrv_execute
title: sqlsrv_execute | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_execute
apitype: NA
helpviewer_keywords:
- sqlsrv_exclude
- executing queries
- API Reference, sqlsrv_execute
ms.assetid: 38331bc2-4391-4f9f-aa83-9873dad605a0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3cad68a3af72a23845d8a584434a4a41614d422a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206501"
---
# <a name="sqlsrv_execute"></a>sqlsrv_execute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Esegue un'istruzione preparata in precedenza. Vedere [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) per informazioni sulla preparazione di un'istruzione per l'esecuzione.  
  
> [!NOTE]  
> Questa funzione è ideale per eseguire più volte un'istruzione preparata applicando valori di parametro diversi.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_execute( resource $stmt)  
```  
  
#### <a name="parameters"></a>Parametri  
*$stmt*: risorsa che specifica l'istruzione da eseguire. Per altre informazioni sulla creazione di una risorsa di istruzione, vedere [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
## <a name="return-value"></a>Valore restituito  
Valore booleano: **true** se l'istruzione è stata eseguita correttamente. In caso contrario, **false**.  
  
## <a name="example"></a>Esempio  
Nell'esempio seguente viene eseguita un'istruzione che aggiorna un campo della tabella *Sales.SalesOrderDetail* del database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works). Nell'esempio si presuppone che SQL Server e il database AdventureWorks siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up the Transact-SQL query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = ( ?)   
         WHERE SalesOrderDetailID = ( ?)";  
  
/* Set up the parameters array. Parameters correspond, in order, to  
question marks in $tsql. */  
$params = array( 5, 10);  
  
/* Create the statement. */  
$stmt = sqlsrv_prepare( $conn, $tsql, $params);  
if( $stmt )  
{  
     echo "Statement prepared.\n";  
}  
else  
{  
     echo "Error in preparing statement.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Execute the statement. Display any errors that occur. */  
if( sqlsrv_execute( $stmt))  
{  
      echo "Statement executed.\n";  
}  
else  
{  
     echo "Error in executing statement.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)  

[sqlsrv_query](../../connect/php/sqlsrv-query.md)  
  
