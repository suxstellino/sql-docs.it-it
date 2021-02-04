---
title: PDO::errorCode
description: Informazioni di riferimento sulle API per la funzione PDO::errorCode nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5864b1d8-6814-41cd-a88d-415124484c13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed2a580a972375a14c5ed6532e40eb7db43373a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202034"
---
# <a name="pdoerrorcode"></a>PDO::errorCode
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

PDO::errorCode recupera il valore SQLSTATE dell'ultima operazione dell'handle di database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
mixed PDO::errorCode();  
```  
  
## <a name="return-value"></a>Valore restituito  
PDO::errorCode restituisce un valore SQLSTATE di cinque caratteri sotto forma di stringa oppure NULL se non sono state eseguite operazioni nell'handle di database.  
  
## <a name="remarks"></a>Osservazioni  
PDO::errorCode nel driver PDO_SQLSRV restituisce avvisi relativi ad alcune operazioni riuscite. Ad esempio, per una connessione riuscita, PDO::errorCode restituisce "01000", che indica SQL_SUCCESS_WITH_INFO.  
  
PDO::errorCode recupera i codici di errore solo per le operazioni eseguite direttamente sul database. Se si crea un'istanza di PDOStatement tramite PDO::prepare o PDO::query e viene generato un errore per l'oggetto istruzione, PDO::errorCode non recupera tale errore. Sarà necessario chiamare PDOStatement::errorCode per recuperare il codice di errore relativo a un'operazione eseguita su un particolare oggetto istruzione.  
  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="example"></a>Esempio  
In questo esempio il nome della colonna errato (`Cityx` anziché `City`) causa un errore, che viene quindi segnalato.  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
