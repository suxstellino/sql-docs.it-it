---
description: Metodo executeQuery (java.lang.String)
title: Metodo executeQuery (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeQuery (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 610205c2-6bcd-426c-ad6f-9682551efdec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5b5b8e65d634339d2e1482f93223eb59d6b773a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165912"
---
# <a name="executequery-method-javalangstring"></a>Metodo executeQuery (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Esegue l'istruzione SQL specificata e restituisce un solo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final java.sql.ResultSet executeQuery(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sql*  
  
 Valore **String** contenente un'istruzione SQL.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SQLServerResultSet.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo executeQuery viene specificato dal metodo executeQuery nell'interfaccia java.sql.Statement.  
  
 Questo metodo esegue l'override del metodo [executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) presente nella classe [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
 La chiamata di questo metodo genera un'eccezione perché l'istruzione SQL per l'oggetto SQLServerPreparedStatement viene specificata quando viene creato l'oggetto.  
  
 Viene generata l'eccezione [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) se l'istruzione SQL specificata produce un risultato diverso da un solo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo executeQuery &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
