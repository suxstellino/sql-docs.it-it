---
description: sqlsrv_send_stream_data
title: sqlsrv_send_stream_data | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_send_stream_data
apitype: NA
helpviewer_keywords:
- sqlsrv_send_stream_data
- API Reference, sqlsrv_send_stream_data
- streaming data
ms.assetid: 826c2d45-694f-42b8-b12b-cd4523a31883
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d70be265001a1113844ba2a4c3f77c47fef0e4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201045"
---
# <a name="sqlsrv_send_stream_data"></a>sqlsrv_send_stream_data
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Invia dati da flussi di parametri al server. Con ogni chiamata a **sqlsrv_send_stream_data** vengono inviati fino a otto kilobyte (8 KB) di dati.  
  
> [!NOTE]  
> Per impostazione predefinita, tutti i dati di flusso vengono inviati al server quando viene eseguita una query. Se questo comportamento predefinito non viene modificato, non è necessario usare **sqlsrv_send_stream_data** per inviare dati di flusso al server. Per informazioni sulla modifica del comportamento predefinito, vedere la sezione Parametri di [sqlsrv_query](../../connect/php/sqlsrv-query.md) o [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_send_stream_data( resource $stmt)  
```  
  
#### <a name="parameters"></a>Parametri  
*$stmt*: risorsa di istruzione corrispondente a un'istruzione eseguita.  
  
## <a name="return-value"></a>Valore restituito  
Valore booleano: **true** se sono presenti altri dati da inviare. In caso contrario, **false**.  
  
## <a name="example"></a>Esempio  
L'esempio seguente apre una revisione del prodotto sotto forma di flusso e lo invia al server. Il comportamento predefinito di invio di tutti i dati di flusso in fase di esecuzione è disabilitato. Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if ($conn === false) {
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Define the query. */  
$tsql = "UPDATE Production.ProductReview   
         SET Comments = (?)   
         WHERE ProductReviewID = 3";  
  
/* Open parameter data as a stream and put it in the $params array. */
$data = 'Insert any lengthy comment here.';
$comment = fopen('data:text/plain,'.urlencode($data), 'r');
$params = array(&$comment);
  
/* Prepare the statement. Use the $options array to turn off the  
default behavior, which is to send all stream data at the time of query  
execution. */  
$options = array("SendStreamParamsAtExec"=>0);  
$stmt = sqlsrv_prepare($conn, $tsql, $params, $options);
  
/* Execute the statement. */  
sqlsrv_execute( $stmt);  
  
/* Send up to 8K of parameter data to the server with each call to  
sqlsrv_send_stream_data. Count the calls. */  
$i = 1;  
while (sqlsrv_send_stream_data($stmt)) {
      echo "$i call(s) made.\n";  
      $i++;  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Aggiornamento dei dati &#40;Driver Microsoft per PHP per SQL Server&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)  

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)  
  
