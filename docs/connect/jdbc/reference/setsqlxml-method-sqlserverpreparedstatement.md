---
description: Metodo setSQLXML (SQLServerPreparedStatement)
title: Metodo setSQLXML (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 70bbdde0-75f7-4169-88c5-dbbe2c4bcd03
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8fc6011dc256692694e3a3987c8f2e9eb10c7d5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173081"
---
# <a name="setsqlxml-method-sqlserverpreparedstatement"></a>Metodo setSQLXML (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sull'oggetto SQLXML specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setSQLXML(int parameterIndex,  
                            java.sql.SQLXML xmlObject)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterIndex*  
  
 Valore **int** che specifica l'indice del parametro.  
  
 *xmlObject*  
  
 Oggetto SQLXML contenente il valore del parametro.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setSQLXML viene specificato dal metodo setSQLXML nell'interfaccia java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
