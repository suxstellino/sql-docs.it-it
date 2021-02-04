---
description: Metodo wasNull (SQLServerCallableStatement)
title: Metodo wasNull (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.wasNull
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1a27b2fe-ae12-46a9-9bca-2c5ca66b9eb3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 517abf6fc57fa890b058f18300e4fc502aa270e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181128"
---
# <a name="wasnull-method-sqlservercallablestatement"></a>Metodo wasNull (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera informazioni circa l'eventuale impostazione del valore SQL NULL per l'ultimo parametro OUT letto.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean wasNull()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se l'ultimo parametro letto è null. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo wasNull viene specificato dal metodo wasNull nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
