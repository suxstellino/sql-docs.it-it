---
description: Metodo executeQuery (SQLServerStatement)
title: Metodo executeQuery (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeQuery
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 599cf463-e19f-4baa-bacb-513cad7c6cd8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9ab9ead1d28b05a077b4b58a422bfef3c05c617f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165882"
---
# <a name="executequery-method-sqlserverstatement"></a>Metodo executeQuery (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Esegue l'istruzione SQL specificata e restituisce un solo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.ResultSet executeQuery(java.lang.String sql)  
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
  
 Viene generata l'eccezione [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) se l'istruzione SQL specificata produce un risultato diverso da un solo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
 Se l'esecuzione di una stored procedure restituisce un conteggio di aggiornamenti maggiore di uno o genera più set di risultati, usare il metodo [execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md) per eseguire la stored procedure.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
