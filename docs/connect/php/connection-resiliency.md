---
title: Resilienza delle connessioni inattive
description: Informazioni sulla resilienza delle connessioni inattive e sul suo comportamento all'interno dei driver Microsoft per PHP per SQL Server.
ms.date: 03/04/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07223e87c82a64fe17cd1b2969b5c93227c70af2
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247482"
---
# <a name="idle-connection-resiliency"></a>Resilienza delle connessioni inattive
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[La resilienza della connessione](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) è il principio in base al quale è possibile ristabilire una connessione interrotta inattiva con determinati vincoli. Se una connessione a Microsoft SQL Server non riesce, la resilienza della connessione consente al client di provare automaticamente a ristabilire la connessione. La resilienza della connessione è una proprietà dell'origine dati supportata solo da SQL Server 2014 e versioni successive e dal database SQL di Azure.

Viene implementata con due parole chiave di connessione che possono essere aggiunte alle stringhe di connessione: **ConnectRetryCount** e **ConnectRetryInterval**.

|Parola chiave|Valori|Predefinito|Descrizione|
|-|-|-|-|
|**ConnectRetryCount**| Numero intero compreso tra 0 e 255 inclusi|1|Numero massimo di tentativi di ristabilire una connessione interrotta prima di rinunciare. Per impostazione predefinita, viene fatto un solo tentativo di ristabilire una connessione interrotta. Il valore 0 indica che non verrà tentata alcuna riconnessione.|
|**ConnectRetryInterval**| Numero intero compreso tra 1 e 60 inclusi|10| Tempo, in secondi, tra i tentativi di ristabilire una connessione. L'applicazione tenterà di riconnettersi immediatamente dopo aver rilevato una connessione interrotta e attenderà **ConnectRetryInterval** secondi prima di riprovare. Questa parola chiave viene ignorata se **ConnectRetryCount** è uguale a 0.

Se il prodotto di **ConnectRetryCount** moltiplicato per **ConnectRetryInterval** è maggiore di **LoginTimeout**, il client smette di tentare di connettersi quando viene raggiunto **LoginTimeout**, in caso contrario, continuerà a tentare di riconnettersi finché non viene raggiunto **ConnectRetryCount**.

#### <a name="remarks"></a>Osservazioni

La resilienza della connessione si applica quando la connessione è inattiva. Gli errori che si verificano durante l'esecuzione di una transazione, ad esempio, non attivano i tentativi di riconnessione e determinano un esito negativo come previsto. Nelle situazioni seguenti, note come stati irreversibili della sessione, i tentativi di riconnessione non vengono attivati:

* Tabelle temporanee
* Cursori globali e locali
* Blocchi delle transazioni a livello di sessione e di contesto di transazione
* Blocchi a livello di applicazione
* Contesto di protezione EXECUTE AS/REVERT
* Handle dell'automazione OLE
* Handle XML preparati
* Flag di traccia

## <a name="example"></a>Esempio

Il codice seguente consente di connettersi a un database e di eseguire una query. La connessione viene interrotta terminando la sessione e viene tentata una nuova query utilizzando la connessione interrotta. Questo esempio usa il database di esempio [AdventureWorks](/previous-versions/sql/sql-server-2008/ms124501(v=sql.100)).

In questo esempio viene specificato un cursore con buffer prima di interrompere la connessione. Se non si specifica un cursore con buffer, la connessione non viene ristabilita perché è presente un cursore attivo sul lato server e quindi la connessione non sarebbe inattiva se venisse interrotta. In questo caso è tuttavia possibile chiamare sqlsrv_free_stmt() prima di interrompere la connessione per lasciare libero il cursore e la connessione verrebbe ristabilita correttamente.

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
Output previsto:
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>Vedere anche
[Resilienza di connessione nel driver ODBC di Windows](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
