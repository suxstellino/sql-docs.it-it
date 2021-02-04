---
description: Metodo isNullable (SQLServerResultSetMetaData)
title: Metodo isNullable (SQLServerResultSetMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSetMetaData.isNullable
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c0fce3fe-5b16-4f60-9b0e-e9b30a90525e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 592d9167edcfb5a4917f118b50c2bf060d3e54ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177355"
---
# <a name="isnullable-method-sqlserverresultsetmetadata"></a>Metodo isNullable (SQLServerResultSetMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica il supporto dei valori Null nella colonna designata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int isNullable(int column)  
```  
  
#### <a name="parameters"></a>Parametri  
 *column*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se la colonna può essere NULL. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo isNullable viene specificato dal metodo isNullable nell'interfaccia java.sql.ResultSetMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-methods.md)   
 [Membri di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-members.md)   
 [Classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
