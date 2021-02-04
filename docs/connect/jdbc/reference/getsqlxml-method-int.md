---
description: Metodo getSQLXML (int)
title: Metodo getSQLXML (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a1b32d3a-d7c9-4086-ae2b-fc1da96949b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ffe7e8098de6c942b8ddc4085511de591893e4e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162271"
---
# <a name="getsqlxml-method-int"></a>Metodo getSQLXML (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto SQLXML in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final java.sql.SQLXML getSQLXML(int parameterIndex)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterIndex*  
  
 Valore **int** che specifica l'indice del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SQLXML.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getSQLXML viene specificato dal metodo getSQLXML nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getSQLXML &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
