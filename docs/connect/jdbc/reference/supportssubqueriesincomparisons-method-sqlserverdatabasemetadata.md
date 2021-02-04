---
description: Metodo supportsSubqueriesInComparisons (SQLServerDatabaseMetaData)
title: Metodo supportsSubqueriesInComparisons (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSubqueriesInComparisons
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 467d32e6-b47e-4095-9f8b-73e07fb814e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b1db10a54382263450097dc4dc8d5daafa877c32
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189146"
---
# <a name="supportssubqueriesincomparisons-method-sqlserverdatabasemetadata"></a>Metodo supportsSubqueriesInComparisons (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta le sottoquery nelle espressioni di confronto.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsSubqueriesInComparisons()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsSubqueriesInComparisons viene specificato dal metodo supportsSubqueriesInComparisons nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
