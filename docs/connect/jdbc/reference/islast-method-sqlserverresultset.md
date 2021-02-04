---
description: Metodo isLast (SQLServerResultSet)
title: Metodo isLast (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.isLast
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 85d4451f-6392-470e-ab21-78a495b45792
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2f276b1f378ceb0bfef3a2cc4f7fe9719bdae56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177399"
---
# <a name="islast-method-sqlserverresultset"></a>Metodo isLast (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera informazioni circa l'eventuale presenza del cursore nell'ultima riga di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean isLast()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il cursore si trova nell'ultima riga. **false** se il cursore si trova in un'altra posizione o se il set di risultati non contiene righe.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo isLast viene specificato dal metodo isLast nell'interfaccia java.sql.ResultSet.  
  
 Se questo metodo viene utilizzato con i cursori forward e dinamici, viene generata un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
