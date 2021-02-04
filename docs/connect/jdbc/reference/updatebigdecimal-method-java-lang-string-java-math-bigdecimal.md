---
description: Metodo updateBigDecimal (java.lang.String, java.math.BigDecimal)
title: Metodo updateBigDecimal (java.lang.String, java.math.BigDecimal) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateBigDecimal (java.lang.String, java.math.BigDecimal)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b844cd9d-3d2d-4385-ab01-ecc89692054f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a61027627282551b0153fc64456c511176c440e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192132"
---
# <a name="updatebigdecimal-method-javalangstring-javamathbigdecimal"></a>Metodo updateBigDecimal (java.lang.String, java.math.BigDecimal)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con un oggetto BigDecimal in base al nome della colonna.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateBigDecimal(java.lang.String columnName,  
                             java.math.BigDecimal x)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnName*  
  
 Valore **String** contenente il nome della colonna.  
  
 *x*  
  
 Oggetto BigDecimal.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateBigDecimal viene specificato dal metodo updateBigDecimal nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateBigDecimal &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatebigdecimal-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
