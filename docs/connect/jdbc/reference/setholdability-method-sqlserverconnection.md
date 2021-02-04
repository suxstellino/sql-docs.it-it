---
description: Metodo setHoldability (SQLServerConnection)
title: Metodo setHoldability (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setHoldability
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 552eebd0-4c38-43f0-961f-35244f99109b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3f5b35e755449c61aa8e9b23bc05b1b71ab16318
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173415"
---
# <a name="setholdability-method-sqlserverconnection"></a>Metodo setHoldability (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta sul valore specificato la trattenibilità corrente per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) creati usando l'oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setHoldability(int nNewHold)  
```  
  
#### <a name="parameters"></a>Parametri  
 *nNewHold*  
  
 Valore **int** contenente uno dei livelli di trattenibilità seguenti:  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setHoldability viene specificato dal metodo setHoldability nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
