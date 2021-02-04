---
description: Metodo usesLocalFilePerTable (SQLServerDatabaseMetaData)
title: Metodo usesLocalFilePerTable (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.usesLocalFilePerTable
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1fafb076-2bb7-4845-9c02-788479f00ca2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 259716e6591def3ee505485a8fbdaa3582af045f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195864"
---
# <a name="useslocalfilepertable-method-sqlserverdatabasemetadata"></a>Metodo usesLocalFilePerTable (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database utilizza un file per ogni tabella.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean usesLocalFilePerTable()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** usa un file per ogni tabella. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo usesLocalFilePerTable viene specificato dal metodo usesLocalFilePerTable nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
