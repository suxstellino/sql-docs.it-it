---
description: Metodo addConnectionEventListener (SQLServerPooledConnection)
title: Metodo addConnectionEventListener (SQLServerPooledConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPooledConnection.addConnectionEventListener
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 142830a8-8d4e-48ca-911d-85bf195ca4fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2354562eb4cdfc81145aaf139e2cd5cdc776d2cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163571"
---
# <a name="addconnectioneventlistener-method-sqlserverpooledconnection"></a>Metodo addConnectionEventListener (SQLServerPooledConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Registra il listener di eventi specificato in modo che riceva una notifica quando si verifica un evento in questo oggetto [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void addConnectionEventListener(javax.sql.ConnectionEventListener listener)  
```  
  
#### <a name="parameters"></a>Parametri  
 *listener*  
  
 Oggetto ConnectionEventListener.  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo addConnectionEventListener viene specificato dal metodo addConnectionEventListener nell'interfaccia javax.sql.PooledConnection.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-methods.md)   
 [Membri di SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-members.md)   
 [Classe SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)  
  
  
