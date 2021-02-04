---
description: Metodo supportsNamedParameters (SQLServerDatabaseMetaData)
title: Metodo supportsNamedParameters (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsNamedParameters
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 158be08f-387d-4c5b-b567-a1fe590d6f16
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2494fdccbcbeefa18eb5729604ba7361c9238751
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167937"
---
# <a name="supportsnamedparameters-method-sqlserverdatabasemetadata"></a>Metodo supportsNamedParameters (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta parametri denominati nelle istruzioni richiamabili.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsNamedParameters()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsNamedParameters viene specificato dal metodo supportsNamedParameters nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
