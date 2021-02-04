---
title: PDO::beginTransaction
description: Informazioni di riferimento sulle API per la funzione PDO::beginTransaction nel driver Microsoft PDO_SQLSRV per PHP per SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4d5db438-9df7-4d22-9907-3ddc63bd2220
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2aff13f864bd22ab9cc2407e7a990c6114a6e108
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202084"
---
# <a name="pdobegintransaction"></a>PDO::beginTransaction
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Disattiva la modalità di commit automatico e avvia una transazione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
bool PDO::beginTransaction();  
```  
  
## <a name="return-value"></a>Valore restituito  
true se la chiamata al metodo ha avuto esito positivo; in caso contrario, false.  
  
## <a name="remarks"></a>Osservazioni  
La transazione avviata con PDO::beginTransaction termina quando viene chiamato [PDO::commit](../../connect/php/pdo-commit.md) o [PDO::rollback](../../connect/php/pdo-rollback.md).  
  
PDO::beginTransaction non è influenzato da e non influisce sul valore di PDO::ATTR_AUTOCOMMIT.  
  
Non è consentito chiamare PDO::beginTransaction prima di terminare la chiamata PDO::beginTransaction precedente con PDO::rollback o PDO::commit.  
  
Se questo metodo ha esito negativo, la connessione torna alla modalità di commit automatico.  
  
Nella versione 2.0 dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]è stato aggiunto il supporto per PDO.  
  
## <a name="example"></a>Esempio  
Nell'esempio seguente viene usato un database denominato Test e una tabella denominata Table1. Viene avviata una transazione quindi, vengono generati comandi per aggiungere due righe e poi per eliminare una riga. I comandi vengono inviati al database e la transazione viene terminata in modo esplicito con `PDO::commit`.  
  
```  
<?php  
   $conn = new PDO( "sqlsrv:server=(local); Database = Test", "", "");  
   $conn->beginTransaction();  
   $ret = $conn->exec("insert into Table1(col1, col2) values('a', 'b') ");  
   $ret = $conn->exec("insert into Table1(col1, col2) values('a', 'c') ");  
   $ret = $conn->exec("delete from Table1 where col1 = 'a' and col2 = 'b'");  
   $conn->commit();  
   // $conn->rollback();  
   echo $ret;  
?>  
```  
  
## <a name="see-also"></a>Vedere anche  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
