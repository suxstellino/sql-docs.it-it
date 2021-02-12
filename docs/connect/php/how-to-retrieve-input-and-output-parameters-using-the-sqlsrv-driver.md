---
title: 'Procedura: Recuperare i parametri di input/output usando il driver SQLSRV'
description: Questo argomento descrive come recuperare i parametri di input e output usando le stored procedure e il driver Microsoft SQLSRV per PHP per SQL Server
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- stored procedure support
ms.assetid: 9a7c5f60-67f9-4968-a3a8-c256ee481da2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33eb988309b6105818e150010dd44627efaced4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046125"
---
# <a name="how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver"></a>Procedura: Recuperare i parametri di input e output usando il driver SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

In questo argomento viene illustrato come usare il driver SQLSRV per chiamare una stored procedure in cui un parametro è definito come parametro di input/output e come recuperare i risultati. Durante il recupero di un parametro di output o di input/output, tutti i risultati restituiti dalla stored procedure devono essere usati prima che il valore del parametro restituito sia accessibile.  
  
> [!NOTE]  
>  Le variabili inizializzate o aggiornate su **null**, **DateTime**, o tipi di flusso non possono essere usate come parametri di output.  
  
## <a name="example-1"></a>Esempio 1
L'esempio seguente chiama una stored procedure che sottrae le ore di ferie usufruite dalle ore di ferie disponibili di un dipendente specifico. La variabile che rappresenta le ore di ferie usufruite, *$vacationHrs*, viene passata alla stored procedure come parametro di input. Dopo aver aggiornato le ore di ferie disponibili, la stored procedure usa lo stesso parametro per restituire il numero di ore di ferie rimanenti.  
  
> [!NOTE]  
> L'inizializzazione di *$vacationHrs* su 4 imposta il valore restituito di PHPTYPE su Integer. Per garantire l'integrità del tipo di dati, i parametri di input/output devono essere inizializzati prima di chiamare la stored procedure. In alternativa, è necessario specificare il valore di PHPTYPE voluto. Per informazioni sull'impostazione di PHPTYPE, vedere [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
Poiché la stored procedure restituisce due risultati, è necessario chiamare [sqlsrv_next_result](../../connect/php/sqlsrv-next-result.md) dopo l'esecuzione della stored procedure per rendere disponibile il valore del parametro di output. Dopo la chiamata a **sqlsrv_next_result**, *$vacationHrs*, contiene il valore del parametro di output restituito dalla stored procedure.  
  
> [!NOTE]  
> È consigliabile pertanto chiamare le stored procedure usando la sintassi canonica. Per altre informazioni sulla sintassi canonica, vedere [Chiamata di una stored procedure](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md).  
  
Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
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
  
/* Drop the stored procedure if it already exists. */  
$tsql_dropSP = "IF OBJECT_ID('SubtractVacationHours', 'P') IS NOT NULL  
                DROP PROCEDURE SubtractVacationHours";  
$stmt1 = sqlsrv_query( $conn, $tsql_dropSP);  
if( $stmt1 === false )  
{  
     echo "Error in executing statement 1.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Create the stored procedure. */  
$tsql_createSP = "CREATE PROCEDURE SubtractVacationHours  
                        @EmployeeID int,  
                        @VacationHrs smallint OUTPUT  
                  AS  
                  UPDATE HumanResources.Employee  
                  SET VacationHours = VacationHours - @VacationHrs  
                  WHERE EmployeeID = @EmployeeID;  
                  SET @VacationHrs = (SELECT VacationHours  
                                      FROM HumanResources.Employee  
                                      WHERE EmployeeID = @EmployeeID)";  
  
$stmt2 = sqlsrv_query( $conn, $tsql_createSP);  
if( $stmt2 === false )  
{  
     echo "Error in executing statement 2.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/*--------- The next few steps call the stored procedure. ---------*/  
  
/* Define the Transact-SQL query. Use question marks (?) in place of  
the parameters to be passed to the stored procedure */  
$tsql_callSP = "{call SubtractVacationHours( ?, ?)}";  
  
/* Define the parameter array. By default, the first parameter is an  
INPUT parameter. The second parameter is specified as an INOUT  
parameter. Initializing $vacationHrs to 8 sets the returned PHPTYPE to  
integer. To ensure data type integrity, output parameters should be  
initialized before calling the stored procedure, or the desired  
PHPTYPE should be specified in the $params array.*/  
  
$employeeId = 4;  
$vacationHrs = 8;  
$params = array(   
                 array($employeeId, SQLSRV_PARAM_IN),  
                 array(&$vacationHrs, SQLSRV_PARAM_INOUT)  
               );  
  
/* Execute the query. */  
$stmt3 = sqlsrv_query( $conn, $tsql_callSP, $params);  
if( $stmt3 === false )  
{  
     echo "Error in executing statement 3.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Display the value of the output parameter $vacationHrs. */  
sqlsrv_next_result($stmt3);  
echo "Remaining vacation hours: ".$vacationHrs;  
  
/*Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_free_stmt( $stmt3);  
sqlsrv_close( $conn);  
?>  
```  

> [!NOTE]
> Quando si associa un parametro di input/output a un tipo bigint, se esiste la possibilità che il valore venga escluso dall'intervallo di un [Integer](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), sarà necessario specificare il tipo di campo SQL come SQLSRV_SQLTYPE_BIGINT. In caso contrario, è possibile che venga generata un'eccezione "Valore esterno all'intervallo consentito".

## <a name="example-2"></a>Esempio 2
Questo esempio di codice mostra come associare un valore begint di grandi dimensioni come parametro di input/output.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"testDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume the stored procedure spTestProcedure exists, which retrieves a bigint value of some large number
// e.g. 9223372036854
$bigintOut = 0;
$outSql = "{CALL spTestProcedure (?)}";
$stmt = sqlsrv_prepare($conn, $outSql, array(array(&$bigintOut, SQLSRV_PARAM_INOUT, null, SQLSRV_SQLTYPE_BIGINT)));
sqlsrv_execute($stmt);
echo "$bigintOut\n";   // Expect 9223372036854

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Vedere anche  
[Procedura: Specificare la direzione del parametro usando il driver SQLSRV](../../connect/php/how-to-specify-parameter-direction-using-the-sqlsrv-driver.md)

[Procedura: Recuperare i parametri di output mediante il driver SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md)

[Recupero di dati](../../connect/php/retrieving-data.md)  
  
