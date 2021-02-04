---
description: Metodo getBigDecimal (java.lang.String) (SQLServerResultSet)
title: Metodo getBigDecimal (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBigDecimal (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b0ded929-d5f5-4573-bf75-ce5bd32328a5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b4a1485c845f65f42a34fb4e6e02727c91c53f61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168264"
---
# <a name="getbigdecimal-method-javalangstring-sqlserverresultset"></a>Metodo getBigDecimal (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del nome di colonna designato nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.math.BigDecimal con la massima precisione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.math.BigDecimal getBigDecimal(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnName*  
  
 Valore **String** contenente il nome della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto BigDecimal.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getBigDecimal viene specificato dal metodo getBigDecimal nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getBigDecimal &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
