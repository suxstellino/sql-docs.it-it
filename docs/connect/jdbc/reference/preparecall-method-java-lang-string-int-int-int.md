---
description: Metodo prepareCall (java.lang.String, int, int, int)
title: Metodo prepareCall (java.lang.String, int, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.prepareCall (java.lang.String, int, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 81104fd5-75b0-4540-9f48-c3dbf59a8564
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 027a50418e85a06b04da9a84ce808ff18711cd53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176805"
---
# <a name="preparecall-method-javalangstring-int-int-int"></a>Metodo prepareCall (java.lang.String, int, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un oggetto [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) che genera oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) con il tipo, la concorrenza e la trattenibilità specificati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.CallableStatement prepareCall(java.lang.String sql,  
                                              int nType,  
                                              int nConcur,  
                                              int nHold)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sql*  
  
 Valore **String** contenente un'istruzione SQL.  
  
 *nType*  
  
 Valore **int** che indica il tipo di set di risultati.  
  
 *nConcur*  
  
 Valore **int** che indica il tipo di concorrenza del set di risultati.  
  
 *nHold*  
  
 Valore **int** che indica la trattenibilità dei set di risultati.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto CallableStatement.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo prepareCall viene specificato dal metodo prepareCall nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo prepareCall &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md)   
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
