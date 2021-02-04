---
description: Metodo setBytes (SQLServerPreparedStatement)
title: Metodo setBytes (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 52e99ef9-b786-4a14-bfc5-4162e46aafbb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4e23536a89e2a57f706c874314454ed1a3f3e0f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173731"
---
# <a name="setbytes-method-sqlserverpreparedstatement"></a>Metodo setBytes (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sulla matrice di byte specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setBytes(int n,  
                           byte[] x)  
```  
  
#### <a name="parameters"></a>Parametri  
 *n*  
  
 Valore **int** che indica il numero di parametro.  
  
 *x*  
  
 Matrice di byte.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setBytes viene specificato dal metodo setBytes nell'interfaccia java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
