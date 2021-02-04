---
description: sqlsrv_fetch
title: sqlsrv_fetch | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_fetch
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch
- API Reference, sqlsrv_fetch
- retrieving data, as a single field
ms.assetid: a5a640a1-6e7d-452e-8b66-850a4dc2ce89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1ab164eb61bcde26862d47c544e366a296117e5a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154385"
---
# <a name="sqlsrv_fetch"></a>sqlsrv_fetch
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Rende la riga successiva di un set di risultati disponibile per la lettura. Usare [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) per leggere i campi della riga.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_fetch( resource $stmt[, row[, ]offset])  
```  
  
#### <a name="parameters"></a>Parametri  
*$stmt*: risorsa di istruzione corrispondente a un'istruzione eseguita.  
  
> [!NOTE]  
> Un'istruzione deve essere eseguita prima che sia possibile recuperare i risultati. Per informazioni sull'esecuzione di un'istruzione, vedere [sqlsrv_query](../../connect/php/sqlsrv-query.md) e [sqlsrv_execute](../../connect/php/sqlsrv-execute.md).  
  
*Row* [facoltativo]: uno dei valori seguenti, che specifica la riga per accedere a un set di risultati che usa un cursore scorrevole:  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
Per altre informazioni su questi valori, vedere [Specifica di un tipo di cursore e selezione di righe](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).  
  
*offset* [FACOLTATIVO]: usato con SQLSRV_SCROLL_ABSOLUTE e SQLSRV_SCROLL_RELATIVE per specificare la riga da recuperare. Il primo record nel set di risultati è 0.  
  
## <a name="return-value"></a>Valore restituito  
Se la riga successiva del set di risultati è stata recuperata correttamente, viene restituito **true** . Se nel set di risultati non sono presenti altri risultati, viene restituito **null** . Se si è verificato un errore, viene restituito **false** .  
  
## <a name="example"></a>Esempio  
L'esempio seguente usa **sqlsrv_fetch** per recuperare una riga di dati contenente una revisione di prodotto e il nome del revisore. Per recuperare i dati dal set di risultati, è possibile usare [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md). Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up and execute the query. Note that both ReviewerName and  
Comments are of SQL Server type nvarchar. */  
$tsql = "SELECT ReviewerName, Comments   
         FROM Production.ProductReview  
         WHERE ProductReviewID=1";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Make the first row of the result set available for reading. */  
if( sqlsrv_fetch( $stmt ) === false)  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Note: Fields must be accessed in order.  
Get the first field of the row. Note that no return type is  
specified. Data will be returned as a string, the default for  
a field of type nvarchar.*/  
$name = sqlsrv_get_field( $stmt, 0);  
echo "$name: ";  
  
/*Get the second field of the row as a stream.  
Because the default return type for a nvarchar field is a  
string, the return type must be specified as a stream. */  
$stream = sqlsrv_get_field( $stmt, 1,   
                            SQLSRV_PHPTYPE_STREAM( SQLSRV_ENC_CHAR));  
while( !feof( $stream ))  
{   
    $str = fread( $stream, 10000);  
    echo $str;  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Recupero di dati](../../connect/php/retrieving-data.md)  

[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)  
  
