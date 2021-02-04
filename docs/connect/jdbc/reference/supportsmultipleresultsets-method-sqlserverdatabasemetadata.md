---
description: Metodo supportsMultipleResultSets (SQLServerDatabaseMetaData)
title: Metodo supportsMultipleResultSets (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsMultipleResultSets
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: cb4d0b91-db1d-4a6f-a87c-8ea125215afc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8c7bcf2851c14161ac1c26bae03817f1dce3e2b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185848"
---
# <a name="supportsmultipleresultsets-method-sqlserverdatabasemetadata"></a>Metodo supportsMultipleResultSets (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta il recupero di più oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) da una singola chiamata al metodo [execute](../../../connect/jdbc/reference/execute-method.md) della classe [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsMultipleResultSets()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsMultipleResultSets viene specificato dal metodo supportsMultipleResultSets nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
