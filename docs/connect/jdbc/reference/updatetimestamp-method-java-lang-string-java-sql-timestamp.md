---
description: Metodo updateTimestamp (java.lang.String, java.sql.Timestamp)
title: Metodo updateTimestamp (java.lang.String, java.sql.Timestamp) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateTimestamp (java.lang.String, java.sql.Timestamp)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6d468357-bf73-484c-9a30-3671e399cf26
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 22f4b07489276629dc9989dbfa46f7e4f8f08bf8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195683"
---
# <a name="updatetimestamp-method-javalangstring-javasqltimestamp"></a>Metodo updateTimestamp (java.lang.String, java.sql.Timestamp)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con un valore di timestamp in base al nome della colonna.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateTimestamp(java.lang.String columnName,  
                            java.sql.Timestamp x)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnName*  
  
 Valore **String** contenente il nome della colonna.  
  
 *x*  
  
 Valore di timestamp.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateTimestamp viene specificato dal metodo updateTimestamp nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateTimestamp &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatetimestamp-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
