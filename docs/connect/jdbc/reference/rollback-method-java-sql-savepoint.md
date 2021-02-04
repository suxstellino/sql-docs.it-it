---
description: Metodo rollback (java.sql.Savepoint)
title: Metodo rollback (java.sql.Savepoint) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.rollback (java.sql.Savepoint)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d5dbd9ef-194f-4130-bfcc-7901a4fa8ded
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 97f6ad14c17ea7c42b79823a16efcae0615a9d6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174152"
---
# <a name="rollback-method-javasqlsavepoint"></a>Metodo rollback (java.sql.Savepoint)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Annulla tutte le modifiche apportate dopo aver impostato l'oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void rollback(java.sql.Savepoint s)  
```  
  
#### <a name="parameters"></a>Parametri  
 *s*  
  
 Oggetto SavePoint di riferimento per il rollback.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo rollBack viene specificato dal metodo rollBack nell'interfaccia java.sql.Connection.  
  
 Questo metodo deve essere utilizzato solo quando la modalità autocommit è stata disabilitata.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo rollback &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md)   
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
