---
description: Metodo setNull (int, int)
title: Metodo setNull (int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setNull (int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7e7f08e9-278a-495a-8ce3-ca173d055021
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2be1fc110f50c9f243f7eb0f02319eb0d5f0715
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173271"
---
# <a name="setnull-method-int-int"></a>Metodo setNull (int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato su un valore Null, in base al tipo di parametro da impostare.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setNull(int index,  
                          int jdbcType)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che indica il numero di parametro.  
  
 *jdbcType*  
  
 Codice di tipo JDBC definito da java.sql.Types.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setNull viene specificato dal metodo setNull nell'interfaccia java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setNull &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setnull-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
