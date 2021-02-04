---
description: Metodo isBeforeFirst (SQLServerResultSet)
title: Metodo isBeforeFirst (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.isBeforeFirst
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e0e2bd28-6949-47dc-b9dd-145ffb337069
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7613a8f5d33f869057840f4b1f4fcfc0ea0ec767
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177492"
---
# <a name="isbeforefirst-method-sqlserverresultset"></a>Metodo isBeforeFirst (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera informazioni sull'eventuale presenza del cursore nella posizione precedente alla prima riga in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean isBeforeFirst()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il cursore precede la prima riga. **false** se il cursore si trova in un'altra posizione o se il set di risultati non contiene righe.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo isBeforeFirst viene specificato dal metodo isBeforeFirst nell'interfaccia java.sql.ResultSet.  
  
 Se questo metodo viene utilizzato con cursori dinamici, inclusi i cursori di sola lettura forward-only, e la proprietà di connessione selectMethod viene impostata su "cursor", si verificherà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
