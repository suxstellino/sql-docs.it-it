---
description: Metodo setTransactionTimeout (SQLServerXAResource)
title: Metodo setTransactionTimeout (SQLServerXAResource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerXAResource.setTransactionTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 38bf4a1a-6ad3-437c-b9ed-8792ab6dde7e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6cfd7379a9315910b0822072268791a6c37cbcf0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172980"
---
# <a name="settransactiontimeout-method-sqlserverxaresource"></a>Metodo setTransactionTimeout (SQLServerXAResource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore del timeout della transazione corrente per questo oggetto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean setTransactionTimeout(int seconds)  
```  
  
#### <a name="parameters"></a>Parametri  
 *secondi*  
  
 Valore **int**.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il timeout è stato impostato correttamente. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 javax.transaction.xa.XAException  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setTransactionTimeout viene specificato dal metodo setTransactionTimeout nell'interfaccia javax.transaction.xa.XAResource.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-methods.md)   
 [Membri di SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [Classe SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
