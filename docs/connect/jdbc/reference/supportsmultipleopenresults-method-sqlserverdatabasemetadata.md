---
description: Metodo supportsMultipleOpenResults (SQLServerDatabaseMetaData)
title: Metodo supportsMultipleOpenResults (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsMultipleOpenResults
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9480d280-5e3d-46ae-80e6-1bba3ac5a641
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3e38b88f306896698a240582a374fc14500cd4f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185838"
---
# <a name="supportsmultipleopenresults-method-sqlserverdatabasemetadata"></a>Metodo supportsMultipleOpenResults (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se è possibile che vengano restituiti simultaneamente più oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) da un oggetto [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsMultipleOpenResults()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsMultipleOpenResults viene specificato dal metodo supportsMultipleOpenResults nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
