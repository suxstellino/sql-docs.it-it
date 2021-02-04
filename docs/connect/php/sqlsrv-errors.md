---
title: sqlsrv_errors
description: Informazioni di riferimento sulle API per la funzione sqlsrv_errors nel driver Microsoft SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_errors
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_errors
- sqlsrv_errors
- errors and warnings
ms.assetid: d1fcffec-f34f-46de-9a0e-343f3b5dbae2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a74b89b93f924401d18372a04ab65180f2cbde8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194219"
---
# <a name="sqlsrv_errors"></a>sqlsrv_errors
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Restituisce informazioni complete sull'errore e/o sull'avviso relativo all'ultima operazione **sqlsrv** eseguita.  
  
La funzione **sqlsrv_errors** può restituire informazioni su errori e/o avvisi quando viene chiamata con uno dei valori di parametro specificati nella sezione Parametri che segue.  
  
Per impostazione predefinita, gli avvisi generati in una chiamata a qualsiasi funzione **sqlsrv** vengono considerati come errori; se viene generato un avviso in una chiamata a una funzione **sqlsrv** , la funzione restituisce false. Tuttavia, gli avvisi che corrispondono ai valori SQLSTATE 01000, 01001, 01003 e 01S02 non vengono mai considerati come errori.  
  
La riga di codice seguente consente di disattivare il comportamento descritto sopra. Un avviso generato da una chiamata a una funzione **sqlsrv** non fa in modo che la funzione restituisca false:  
  
```  
sqlsrv_configure("WarningsReturnAsErrors", 0);  
```  
  
La riga di codice seguente ripristina il comportamento predefinito; gli avvisi, con le eccezioni descritte sopra, vengono considerati come errori:  
  
```  
sqlsrv_configure("WarningsReturnAsErrors", 1);  
```  
  
Indipendentemente dall'impostazione, gli avvisi possono essere recuperati solo tramite una chiamata a **sqlsrv_errors** con il valore di parametro **SQLSRV_ERR_ALL** o **SQLSRV_ERR_WARNINGS**. Per informazioni dettagliate, vedere la sezione Parametri che segue.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlsrv_errors( [int $errorsAndOrWarnings] )  
```  
  
#### <a name="parameters"></a>Parametri  
*$errorsAndOrWarnings*[FACOLTATIVO]: costante predefinita. Questo parametro può assumere uno dei valori elencati nella tabella seguente:  
  
|Valore|Descrizione|  
|---------|---------------|  
|SQLSRV_ERR_ALL|Vengono restituiti gli errori e gli avvisi generati nell'ultima chiamata di funzione **sqlsrv** .|  
|SQLSRV_ERR_ERRORS|Vengono restituiti gli errori generati nell'ultima chiamata di funzione **sqlsrv** .|  
|SQLSRV_ERR_WARNINGS|Vengono restituiti gli avvisi generati nell'ultima chiamata di funzione **sqlsrv** .|  
  
Se non viene specificato alcun valore di parametro, vengono restituiti gli avvisi e gli errori generati dall'ultima chiamata di funzione **sqlsrv** .  
  
## <a name="return-value"></a>Valore restituito  
Una **matrice** di matrici oppure **null**. Ogni **matrice** della **matrice** restituita contiene tre coppie chiave-valore. Nella tabella seguente sono elencate le chiavi con la relativa descrizione:  
  
|Chiave|Descrizione|  
|-------|---------------|  
|SQLSTATE|Per errori originati dal driver ODBC, il valore SQLSTATE restituito da ODBC. Per informazioni sui valori SQLSTATE per ODBC, vedere [Codici di errore ODBC](../../odbc/reference/appendixes/appendix-a-odbc-error-codes.md).<br /><br />Per gli errori originati dai [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], un valore SQLSTATE di IMSSP.<br /><br />Per gli avvisi originati dai [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], un valore SQLSTATE di 01SSP.|  
|codice|Per errori originati da SQL Server, il codice di errore nativo di SQL Server.<br /><br />Per errori originati dal driver ODBC, il codice di errore restituito da ODBC.<br /><br />Per gli errori originati dai [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], il codice di errore [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] . Per altre informazioni, vedere [Handling Errors and Warnings](../../connect/php/handling-errors-and-warnings.md).|  
|message|Descrizione dell'errore.|  
  
I valori della matrice sono accessibili anche con le chiavi numeriche 0, 1 e 2. Se non si verificano errori o avvisi, viene restituito **null** .  
  
## <a name="example"></a>Esempio  
L'esempio seguente visualizza gli errori che si verificano durante l'esecuzione di un'istruzione non riuscita. (L'istruzione non viene eseguita perché **InvalidColumName** non è un nome di colonna valido nella tabella specificata.) Nell'esempio si presuppone che SQL Server e il database [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) siano installati nel computer locale. Quando si esegue l'esempio dalla riga di comando, tutto l'output viene scritto nel browser.  
  
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
  
/* Set up a query to select an invalid column name. */  
$tsql = "SELECT InvalidColumnName FROM Sales.SalesOrderDetail";  
  
/* Attempt execution. */  
/* Execution will fail because of the invalid column name. */  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
      if( ($errors = sqlsrv_errors() ) != null)  
      {  
         foreach( $errors as $error)  
         {  
            echo "SQLSTATE: ".$error[ 'SQLSTATE']."\n";  
            echo "code: ".$error[ 'code']."\n";  
            echo "message: ".$error[ 'message']."\n";  
         }  
      }  
}  
  
/* Free connection resources */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Riferimento all'API del driver SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Informazioni sugli esempi di codice nella documentazione](../../connect/php/about-code-examples-in-the-documentation.md)  
  
