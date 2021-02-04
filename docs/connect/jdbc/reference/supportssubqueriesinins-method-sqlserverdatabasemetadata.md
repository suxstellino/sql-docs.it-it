---
description: Metodo supportsSubqueriesInIns (SQLServerDatabaseMetaData)
title: Metodo supportsSubqueriesInIns (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSubqueriesInIns
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 77a0b5c0-0d8e-4e08-975f-4eeabb108ab1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 815430a48bdb36df48703f11899f5edbf3ab2de5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189119"
---
# <a name="supportssubqueriesinins-method-sqlserverdatabasemetadata"></a>Metodo supportsSubqueriesInIns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta le sottoquery nelle istruzioni IN.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsSubqueriesInIns()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsSubqueriesInIns viene specificato dal metodo supportsSubqueriesInIns nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
