---
description: Metodo getFetchDirection (SQLServerResultSet)
title: Metodo getFetchDirection (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5ab385c2-e18c-4b75-ac2d-2402af5c52a5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b32c1195639dc26fcdfa6d34404eb867c96595c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175784"
---
# <a name="getfetchdirection-method-sqlserverresultset"></a>Metodo getFetchDirection (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera la direzione di recupero di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getFetchDirection()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica la direzione di recupero corrente.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getFetchDirection viene specificato dal metodo getFetchDirection nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo restituisce FETCH_FORWARD per i cursori forward-only, l'ultima impostazione eseguita da una chiamata al metodo [setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md) per altri tipi di cursore, mentre restituisce FETCH_UNKNOWN per questi tipi di cursore se il metodo setFetchDirection non è mai stato chiamato.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
