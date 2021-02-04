---
description: Metodo isClosed (SQLServerResultSet)
title: Metodo isClosed (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 6081aa34-fc88-4dd0-9a3f-05e8488219dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4733b1877a0621f2f8974e3f3b221d745f3bfdcb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177439"
---
# <a name="isclosed-method-sqlserverresultset"></a>Metodo isClosed (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica se questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) è stato chiuso.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean isClosed()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se l'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) è chiuso, **false** se è ancora aperto.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo isClosed viene specificato dal metodo isClosed nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
