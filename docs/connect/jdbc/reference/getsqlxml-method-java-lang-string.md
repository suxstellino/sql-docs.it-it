---
description: Metodo getSQLXML (java.lang.String)
title: Metodo getSQLXML (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: f56b192a-3255-4215-b552-8e494fbca083
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8e7122dadcee3435f9c937ca631244ba60037ce0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162255"
---
# <a name="getsqlxml-method-javalangstring"></a>Metodo getSQLXML (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto SQLXML in base al nome del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final java.sql.SQLXML getSQLXML(java.lang.String parameterName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterName*  
  
 Valore **String** che indica il nome del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto SQLXML.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getSQLXML viene specificato dal metodo getSQLXML nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getSQLXML &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
