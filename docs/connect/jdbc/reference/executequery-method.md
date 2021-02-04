---
description: Metodo executeQuery ()
title: Metodo executeQuery () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeQuery ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1d90407f-16df-4ba2-b4a5-47d5751e1d7c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4c7d0eca0d325bd543812d06a3bc26726f44fde8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168451"
---
# <a name="executequery-method-"></a>Metodo executeQuery ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Esegue la query SQL in questo oggetto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) e restituisce l'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato dalla query.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.ResultSet executeQuery()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SQLServerResultSet.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo executeQuery viene specificato dal metodo executeQuery nell'interfaccia java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo executeQuery &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
