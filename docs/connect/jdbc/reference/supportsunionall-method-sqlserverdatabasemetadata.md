---
description: Metodo supportsUnionAll (SQLServerDatabaseMetaData)
title: Metodo supportsUnionAll (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsUnionAll
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ed8344eb-4d1d-43d3-ade8-935ec677f73c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5dc9cf739bea13ffe66e944b4207295648b1d8b5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189353"
---
# <a name="supportsunionall-method-sqlserverdatabasemetadata"></a>Metodo supportsUnionAll (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta SQL UNION ALL.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsUnionAll()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsUnionAll viene specificato dal metodo supportsUnionAll nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
