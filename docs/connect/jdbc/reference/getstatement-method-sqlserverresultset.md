---
description: Metodo getStatement (SQLServerResultSet)
title: Metodo getStatement (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getStatement
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7dea981b-b4fd-4f8d-954f-e686124627e2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 720afe1e93fda2374427b0a9d01ad3ba069ba3e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174959"
---
# <a name="getstatement-method-sqlserverresultset"></a>Metodo getStatement (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera l'oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) che ha generato questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Statement getStatement()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SQLServerStatement.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getStatement viene specificato dal metodo getStatement nell'interfaccia java.sql.ResultSet.  
  
 Se il set di risultati è stato generato in qualche altro modo, ad esempio da un metodo [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md), questo metodo restituisce Null.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
