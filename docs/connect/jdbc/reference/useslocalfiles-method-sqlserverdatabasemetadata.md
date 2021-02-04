---
description: Metodo usesLocalFiles (SQLServerDatabaseMetaData)
title: Metodo usesLocalFiles (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.usesLocalFiles
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 69afb3a9-ed56-4191-88b8-bc46c03b817b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33e9d57f56e47649cca26694e3d8410a21f20388
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195862"
---
# <a name="useslocalfiles-method-sqlserverdatabasemetadata"></a>Metodo usesLocalFiles (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database archivia le tabelle in un file locale.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean usesLocalFiles()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il database usa file locali. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo usesLocalFiles viene specificato dal metodo usesLocalFiles nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
