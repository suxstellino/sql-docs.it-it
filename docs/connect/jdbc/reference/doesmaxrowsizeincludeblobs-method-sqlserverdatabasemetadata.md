---
description: Metodo doesMaxRowSizeIncludeBlobs (SQLServerDatabaseMetaData)
title: Metodo doesMaxRowSizeIncludeBlobs (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.doesMaxRowSizeIncludeBlobs
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0c90a7a7-5a59-4858-bb26-3e725d8611d7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b40e199531e6f2f5e5abd210ff558b316221989b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163441"
---
# <a name="doesmaxrowsizeincludeblobs-method-sqlserverdatabasemetadata"></a>Metodo doesMaxRowSizeIncludeBlobs (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il valore restituito per il metodo [getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md) include i tipi di dati SQL LONGVARCHAR e LONGVARBINARY.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean doesMaxRowSizeIncludeBlobs()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il valore restituito include i tipi di dati. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo doesMoxRowSizeIncludeBlobs viene specificato dal metodo doesMoxRowSizeIncludeBlobs nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
