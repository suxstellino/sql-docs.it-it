---
description: Metodo getTableName (SQLServerResultSetMetaData)
title: Metodo getTableName (SQLServerResultSetMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSetMetaData.getTableName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9a077b50-cc5a-4301-9398-49ea68544e89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a8e97b0a43cdc9f8b25c3d0519da2bfb940b5c32
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174876"
---
# <a name="gettablename-method-sqlserverresultsetmetadata"></a>Metodo getTableName (SQLServerResultSetMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ottiene il nome di tabella della colonna designata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getTableName(int column)  
```  
  
#### <a name="parameters"></a>Parametri  
 *column*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il nome della tabella.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getTableName viene specificato dal metodo getTableName nell'interfaccia java.sql.ResultSetMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-methods.md)   
 [Membri di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-members.md)   
 [Classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
