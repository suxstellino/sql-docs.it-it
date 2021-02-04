---
description: Metodo getMaxBinaryLiteralLength (SQLServerDatabaseMetaData)
title: Metodo getMaxBinaryLiteralLength (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxBinaryLiteralLength
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 42e49ff9-8072-44e4-ad75-c864c3a4ad8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9ccb65d40257cc8960706895ce1cf4beda208a7e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175599"
---
# <a name="getmaxbinaryliterallength-method-sqlserverdatabasemetadata"></a>Metodo getMaxBinaryLiteralLength (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero massimo di caratteri esadecimali consentiti dal database in un valore letterale binario inline.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getMaxBinaryLiteralLength()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica il numero massimo di caratteri esadecimali.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMaxBinaryLiteralLength viene specificato dal metodo getMaxBinaryLiteralLength nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
