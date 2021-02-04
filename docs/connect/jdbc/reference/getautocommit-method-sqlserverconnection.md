---
description: Metodo getAutoCommit (SQLServerConnection)
title: Metodo getAutoCommit (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getAutoCommit
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: af1f67f4-f568-4e58-abcc-5c809a89b547
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3b8cbb3825cf25f2b7d088274bb6b7341c001f51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168310"
---
# <a name="getautocommit-method-sqlserverconnection"></a>Metodo getAutoCommit (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera la modalità di commit automatico corrente per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean getAutoCommit()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se la modalità di commit automatico è abilitata. **false** in caso contrario.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getAutoCommit viene specificato dal metodo getAutoCommit nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
