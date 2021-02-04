---
description: Metodo getURL (java.lang.String) (SQLServerResultSet)
title: Metodo getURL (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getURL (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 105a5319-0f4c-4d08-964b-cc52f8e28ec1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7259d439097775b9dc72faf3417bf4ec0175a9b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177665"
---
# <a name="geturl-method-javalangstring-sqlserverresultset"></a>Metodo getURL (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del nome della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto URL.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.net.URL getURL(java.lang.String sColumn)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sColumn*  
  
 Valore **String** contenente il nome della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto URL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getURL viene specificato dal metodo getURL nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getURL &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/geturl-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
