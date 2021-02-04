---
description: Metodo getParameterMetaData (SQLServerPreparedStatement)
title: Metodo getParameterMetaData (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.getParameterMetaData
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c2876dec-ce29-4b61-9d74-ec3173b8cba5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 85c99024df5f62b71fbfd1ab05d4ec4a82ac18ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175280"
---
# <a name="getparametermetadata-method-sqlserverpreparedstatement"></a>Metodo getParameterMetaData (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero, i tipi e le proprietà dei parametri di questo oggetto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final java.sql.ParameterMetaData getParameterMetaData()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getParameterMetaData viene specificato dal metodo getParameterMetaData nell'interfaccia java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
