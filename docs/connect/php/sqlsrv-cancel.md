---
description: sqlsrv_cancel
title: sqlsrv_cancel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_cancel
apitype: NA
helpviewer_keywords:
- sqlsrv_cancel
- API Reference, sqlsrv_cancel
ms.assetid: 75798c9b-f711-445d-9b8f-ba4d405ca50a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0d93f74d4b2df35e83ad4fbd72f0b0dcbd91e216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190270"
---
# <a name="sqlsrv_cancel"></a>sqlsrv_cancel
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Annulla un'istruzione. Ciò significa che qualsiasi risultato in sospeso per l'istruzione viene eliminato. Dopo la chiamata a questa funzione, l'istruzione può essere eseguita nuovamente se è stata preparata con [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). La chiamata a questa funzione non è necessaria se sono stati usati tutti i risultati associati all'istruzione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_cancel( resource $stmt)  
```  
  
#### <a name="parameters"></a>Parametri  
*$stmt*: l'istruzione da annullare.  
  
## <a name="return-value"></a>Valore restituito  
Valore booleano: **true** se l'operazione è riuscita. In caso contrario, **false**.  
  
## <a name="example"></a>Esempio  
L'esempio seguente richiede al database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) di eseguire una query, quindi usa e conta i risultati fino a quando la variabile *$salesTotal* non raggiunge una quantità specificata. I risultati restanti della query vengono ignorati. Nell'esempio si presuppone che SQL Server e il database AdventureWorks siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Prepare and execute the query. */  
$tsql = "SELECT OrderQty, UnitPrice FROM Sales.SalesOrderDetail";  
$stmt = sqlsrv_prepare( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
if( sqlsrv_execute( $stmt ) === false)  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Initialize tracking variables. */  
$salesTotal = 0;  
$count = 0;  
  
/* Count and display the number of sales that produce revenue  
of $100,000. */  
while( ($row = sqlsrv_fetch_array( $stmt)) && $salesTotal <=100000)  
{  
     $qty = $row[0];  
     $price = $row[1];  
     $salesTotal += ( $price * $qty);  
     $count++;  
}  
echo "$count sales accounted for the first $$salesTotal in revenue.\n";  
  
/* Cancel the pending results. The statement can be reused. */  
sqlsrv_cancel( $stmt);  
?>  
```  
  
## <a name="comments"></a>Commenti  
Un'istruzione che viene preparata ed eseguita usando la combinazione di [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) e [sqlsrv_execute](../../connect/php/sqlsrv-execute.md) può essere eseguita nuovamente con **sqlsrv_execute** dopo la chiamata di **sqlsrv_cancel**. Un'istruzione eseguita con [sqlsrv_query](../../connect/php/sqlsrv-query.md) non può essere eseguita nuovamente dopo la chiamata di **sqlsrv_cancel**.  
  
## <a name="see-also"></a>Vedere anche  
[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Connessione al server](../../connect/php/connecting-to-the-server.md)

[Recupero di dati](../../connect/php/retrieving-data.md)

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)

[sqlsrv_free_stmt](../../connect/php/sqlsrv-free-stmt.md)

  
