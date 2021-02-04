---
description: Metodo supportsTransactions (SQLServerDatabaseMetaData)
title: Metodo supportsTransactions (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsTransactions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 57d28ee5-c2f6-4740-b42d-cead1aade809
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0b3b01255a7b174b5d98fe96003d24b99a5e8447
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158231"
---
# <a name="supportstransactions-method-sqlserverdatabasemetadata"></a>Metodo supportsTransactions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta transazioni.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsTransactions()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsTransactions viene specificato dal metodo supportsTransactions nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
