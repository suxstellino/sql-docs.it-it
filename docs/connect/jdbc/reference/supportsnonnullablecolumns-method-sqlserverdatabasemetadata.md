---
description: Metodo supportsNonNullableColumns (SQLServerDatabaseMetaData)
title: Metodo supportsNonNullableColumns (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsNonNullableColumns
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7c32ea64-460e-4636-8a3b-07c8abeed687
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4f09492d32995b81ea0916116a69fd571621a8b5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167926"
---
# <a name="supportsnonnullablecolumns-method-sqlserverdatabasemetadata"></a>Metodo supportsNonNullableColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se le colonne del database possono essere definite come colonne che non ammettono valori Null.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsNonNullableColumns()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsNonNullableColumns viene specificato dal metodo supportsNonNullableColumns nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
