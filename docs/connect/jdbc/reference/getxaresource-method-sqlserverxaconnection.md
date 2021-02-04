---
description: Metodo getXAResource (SQLServerXAConnection)
title: Metodo getXAResource (SQLServerXAConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerXAConnection.getXAResource
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e1d2828f-fd20-44b0-b796-dc70f77c5b03
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9488429228b09931e9b45a83779c7007d312fb85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177550"
---
# <a name="getxaresource-method-sqlserverxaconnection"></a>Metodo getXAResource (SQLServerXAConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un oggetto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) che verrà usato dal servizio di gestione transazioni per gestire la partecipazione di questo oggetto [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) in una transazione distribuita.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public javax.transaction.xa.XAResource getXAResource()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto XAResource.  
  
## <a name="exceptions"></a>Eccezioni  
 java.sql.SQLException  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getXAResource viene specificato dal metodo getXAResource nell'interfaccia javax.sql.XAConnection.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-methods.md)   
 [Membri di SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-members.md)   
 [Classe SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md)  
  
  
