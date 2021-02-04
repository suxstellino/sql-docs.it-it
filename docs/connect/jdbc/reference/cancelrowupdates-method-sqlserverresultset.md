---
description: Metodo cancelRowUpdates (SQLServerResultSet)
title: Metodo cancelRowUpdates (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.cancelRowUpdates
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2ecacca4-f7bc-4f5d-886a-da7747fdccae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5629e6ea7cb25331cbdeedd1440826c3687caf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168662"
---
# <a name="cancelrowupdates-method-sqlserverresultset"></a>Metodo cancelRowUpdates (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Annulla gli aggiornamenti eseguiti alla riga corrente in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void cancelRowUpdates()  
```  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo cancelRowUpdates viene specificato dal metodo cancelRowUpdates nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo può essere chiamato dopo la chiamata di un metodo di aggiornamento e prima della chiamata del metodo [updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) per eseguire il rollback degli aggiornamenti apportati a una riga. Se non è stato apportato alcun aggiornamento o il metodo updateRow è già stato chiamato, questo metodo non ha effetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
