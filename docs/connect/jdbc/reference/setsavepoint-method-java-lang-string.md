---
description: Metodo setSavepoint (java.lang.String)
title: Metodo setSavepoint (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setSavepoint (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1cf15ec4-d9d9-4ab3-bfee-2ea43ff609a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4de9512d95c3652dab96bc78fc501e8441afbeb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173145"
---
# <a name="setsavepoint-method-javalangstring"></a>Metodo setSavepoint (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un punto di salvataggio con il nome specificato nella transazione corrente e restituisce il nuovo oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) che lo rappresenta.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Savepoint setSavepoint(java.lang.String sName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sName*  
  
 Valore **String** contenente il nome del punto di salvataggio.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SavePoint.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setSavePoint viene specificato dal metodo setSavePoint nell'interfaccia java.sql.Connection.  
  
 L'argomento *sName* viene preceduto automaticamente da caratteri di escape in [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setSavepoint &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)   
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
