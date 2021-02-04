---
description: Metodo supportsTransactionIsolationLevel (SQLServerDatabaseMetaData)
title: Metodo supportsTransactionIsolationLevel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsTransactionIsolationLevel
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b716ed6c-6ec3-47a7-8e6d-16cbf2469d6d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3da1d507624dc8c86178c0af1f2ffeb5445588e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178106"
---
# <a name="supportstransactionisolationlevel-method-sqlserverdatabasemetadata"></a>Metodo supportsTransactionIsolationLevel (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta il livello di isolamento transazione specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsTransactionIsolationLevel(int level)  
```  
  
#### <a name="parameters"></a>Parametri  
 *level*  
  
 Valore **int** che indica il livello di isolamento della transazione.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsTransactionIsolationLevel viene specificato dal metodo supportsTransactionIsolationLevel nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
