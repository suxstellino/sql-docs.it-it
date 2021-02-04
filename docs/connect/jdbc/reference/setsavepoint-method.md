---
description: Metodo setSavepoint ()
title: Metodo setSavepoint () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setSavepoint ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 11013055-4fd3-45a9-b2da-28b2908dad52
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7618072c11924f45eefe68147cd67e881fe6a5a7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173126"
---
# <a name="setsavepoint-method-"></a>Metodo setSavepoint ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un punto di salvataggio senza nome nella transazione corrente e restituisce il nuovo oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) che lo rappresenta.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Savepoint setSavepoint()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SavePoint.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setSavePoint viene specificato dal metodo setSavePoint nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setSavepoint &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)   
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
