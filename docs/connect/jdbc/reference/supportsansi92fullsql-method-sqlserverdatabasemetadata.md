---
description: Metodo supportsANSI92FullSQL (SQLServerDatabaseMetaData)
title: Metodo supportsANSI92FullSQL (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsANSI92FullSQL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8877dc8c-26cd-4374-8ae8-ff7d20621130
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: df34f353121324062312eb1f00c9429ec2aa241c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160973"
---
# <a name="supportsansi92fullsql-method-sqlserverdatabasemetadata"></a>Metodo supportsANSI92FullSQL (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta la grammatica SQL completa ANSI92.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsANSI92FullSQL()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsANSI92FullSQL viene specificato dal metodo supportsANSI92FullSQL nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
