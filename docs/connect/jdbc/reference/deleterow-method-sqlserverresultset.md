---
description: Metodo deleteRow (SQLServerResultSet)
title: Metodo deleteRow (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/20/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.deleteRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: aa04a644-c7c2-4738-8b6e-7fea566d2c16
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8c515b4554b29b9db6ae91ba303834820c80aa63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163444"
---
# <a name="deleterow-method-sqlserverresultset"></a>Metodo deleteRow (SQLServerResultSet)

[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Elimina la riga corrente da questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) e dal database sottostante.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
public void deleteRow()  
```  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo deleteRow viene specificato dal metodo deleteRow nell'interfaccia java.sql.ResultSet.  
  
 Non è possibile chiamare questo metodo quando il cursore si trova sulla riga di inserimento.  
  
 In caso di uso di cursori keyset, questo metodo lascia uno spazio nel set di risultati. È possibile verificare la presenza di questo spazio tramite il metodo [rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md). I numeri delle righe del set di risultati non cambiano.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
