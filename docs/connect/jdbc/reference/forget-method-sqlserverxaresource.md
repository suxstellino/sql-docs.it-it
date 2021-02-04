---
description: Metodo forget (SQLServerXAResource)
title: Metodo forget (SQLServerXAResource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerXAResource.forget
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6d83138d-aa45-4d94-9da6-fdfe7ed28edc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 08c504a7c0c72b1fa4b4a408070bee408161dbb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165895"
---
# <a name="forget-method-sqlserverxaresource"></a>Metodo forget (SQLServerXAResource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica allo strumento di gestione delle risorse di dimenticare un ramo di transazione completato euristicamente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void forget(javax.transaction.xa.Xid xid)  
```  
  
#### <a name="parameters"></a>Parametri  
 *xid*  
  
 Oggetto Xid.  
  
## <a name="exceptions"></a>Eccezioni  
 javax.transaction.xa.XAException  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo forget viene specificato dal metodo forget nell'interfaccia javax.transaction.xa.XAResource.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-methods.md)   
 [Membri di SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [Classe SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
