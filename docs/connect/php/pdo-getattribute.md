---
title: PDO::getAttribute
description: Informazioni di riferimento sulle API per la funzione PDO::getAttribute nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c81833ea-8b8a-459d-8f24-920098da994d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ba4e6b81216933ea4367bb4b37feeb61dd646c88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187613"
---
# <a name="pdogetattribute"></a>PDO::getAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera il valore di un attributo predefinito del PDO o del driver.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
mixed PDO::getAttribute ( $attribute )  
```  
  
#### <a name="parameters"></a>Parametri  
*$attribute*: uno degli attributi supportati. Vedere la sezione Osservazioni per l'elenco degli attributi supportati.  
  
## <a name="return-value"></a>Valore restituito  
In caso di esito positivo, restituisce il valore di un'opzione di connessione, dell'attributo del PDO predefinito o dell'attributo del driver personalizzato. In caso di errore, restituisce null.  
  
## <a name="remarks"></a>Osservazioni  
Nella tabella seguente sono elencati gli attributi supportati.  
  
|Attributo|Elaborato da|Valori supportati|Descrizione|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|Specifica se i nomi di colonna devono essere in lettere maiuscole o minuscole. PDO::CASE_LOWER impone nomi di colonna in lettere minuscole, PDO::CASE_NATURAL lascia i nomi di colonna come restituiti dal database e PDO::CASE_UPPER impone nomi di colonne in lettere maiuscole.<br /><br />Il valore predefinito è PDO::CASE_NATURAL.<br /><br />Questo attributo può essere impostato anche usando PDO::setAttribute.|  
|PDO::ATTR_CLIENT_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Matrice di stringhe|Descrive le versioni del driver e delle librerie correlate. Restituisce una matrice con gli elementi seguenti: versione ODBC (*MajorVer*.*MinorVer*), nome e versione della DLL di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] versione (*MajorVer*.*MinorVer*.*BuildNumber*.*Revision*)|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|Se non è impostato su PDO::PARAM_STR_CHAR, viene restituito PDO::PARAM_STR_NATL.|
|PDO::ATTR_DRIVER_NAME|PDO|string|Restituisce sempre "sqlsrv".|  
|PDO::ATTR_DRIVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|string|Indica la versione [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (*MajorVer*.*MinorVer*.*BuildNumber*.*Revision*)|  
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|Specifica la modalità di gestione degli errori da parte del driver.<br /><br />PDO::ERRMODE_SILENT (predefinito) imposta i codici di errore e le informazioni.<br /><br />PDO::ERRMODE_WARNING genera E_WARNING.<br /><br />PDO::ERRMODE_EXCEPTION genera un'eccezione.<br /><br />Questo attributo può essere impostato anche usando PDO::setAttribute.|  
|PDO::ATTR_ORACLE_NULLS|PDO|Vedere la documentazione del PDO.|Vedere la documentazione del PDO.|  
|PDO::ATTR_SERVER_INFO|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Matrice di 3 elementi|Restituisce il database corrente, la versione di SQL Server e l'istanza di SQL Server.|  
|PDO::ATTR_SERVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|string|Indica la versione di SQL Server (*Major*.*Minor*.*BuildNumber*)|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|Vedere la documentazione del PDO|Vedere la documentazione del PDO.|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|1 per il limite di memoria PHP.|Consente di configurare le dimensioni del buffer che contiene il set di risultati per un cursore sul lato client.<br /><br />Il valore predefinito è 10.240 KB (10 MB).<br /><br />Per altre informazioni sui cursori sul lato client, vedere [Tipi di cursore &#40;driver SQLSRV&#41;](../../connect/php/cursor-types-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true<br /><br />false|Specifica l'esecuzione di query diretta o preparata. Per altre informazioni, vedere [Esecuzione di istruzioni diretta e preparata nel driver PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM|Specifica la codifica del set di caratteri usato dal driver per comunicare con il server.<br /><br />Il valore predefinito è PDO::SQLSRV_ENCODING_UTF8.|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true o false|Gestisce i recuperi numerici dalle colonne con tipi SQL numerici (bit, integer, smallint, tinyint, float o real).<br /><br />Quando il flag dell'opzione di connessione ATTR_STRINGIFY_FETCHES è attivato, anche quando è attivato SQLSRV_ATTR_FETCHES_NUMERIC_TYPE, il valore restituito è una stringa.<br /><br />Quando il tipo PDO restituito nella colonna di associazione è PDO_PARAM_INT, il valore restituito da una colonna integer è un valore int anche se SQLSRV_ATTR_FETCHES_NUMERIC_TYPE è disattivato.|  
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|numero intero|Imposta il timeout in secondi della query.<br /><br />Il valore predefinito è 0, vale a dire che il driver attende i risultati per un periodo illimitato.<br /><br />I numeri negativi non sono consentiti.|  

  
Il PDO elabora alcuni degli attributi predefiniti e richiede al driver di gestire gli altri. Tutti gli attributi personalizzati e le opzioni di connessione vengono gestiti dal driver. Gli attributi o le opzioni di connessione non supportati restituiscono un valore Null.  
  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="example"></a>Esempio  
Questo esempio mostra il valore dell'attributo PDO::ATTR_ERRMODE prima e dopo la modifica del valore.  
  
```  
<?php  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
  
$attributes1 = array( "ERRMODE" );  
foreach ( $attributes1 as $val ) {  
     echo "PDO::ATTR_$val: ";  
     var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
}  
  
$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
  
$attributes1 = array( "ERRMODE" );  
foreach ( $attributes1 as $val ) {  
     echo "PDO::ATTR_$val: ";  
     var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
}  
  
// An example using PDO::ATTR_CLIENT_VERSION  
print_r($conn->getAttribute( PDO::ATTR_CLIENT_VERSION ));  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
