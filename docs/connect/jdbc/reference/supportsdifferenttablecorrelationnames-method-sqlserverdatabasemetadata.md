---
description: Metodo supportsDifferentTableCorrelationNames (SQLServerDatabaseMetaData)
title: Metodo supportsDifferentTableCorrelationNames | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsDifferentTableCorrelationNames
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b4f8db0c-2eaf-476b-b916-3e83355778f7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5943beb8ae6ade712a9f301bec4dcf38723425d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159086"
---
# <a name="supportsdifferenttablecorrelationnames-method-sqlserverdatabasemetadata"></a>Metodo supportsDifferentTableCorrelationNames (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se, quando sono supportati, i nomi di correlazione delle tabelle sono impostati in modo da essere diversi da quelli delle tabelle.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsDifferentTableCorrelationNames()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsDifferentTableCorrelationNames viene specificato dal metodo supportsDifferentTableCorrelationNames nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
