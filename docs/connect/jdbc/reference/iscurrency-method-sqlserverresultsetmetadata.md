---
description: Metodo isCurrency (SQLServerResultSetMetaData)
title: Metodo isCurrency (SQLServerResultSetMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSetMetaData.isCurrency
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7fe25d90-693c-4d3b-9dd2-0f8351c5a9ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2866e1a232f3ed51315971c0fb052d8e718cb74c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177451"
---
# <a name="iscurrency-method-sqlserverresultsetmetadata"></a>Metodo isCurrency (SQLServerResultSetMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica se la colonna designata è un valore di cassa.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean isCurrency(int column)  
```  
  
#### <a name="parameters"></a>Parametri  
 *column*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se la colonna è un valore di cassa. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo isCurrency viene specificato dal metodo isCurrency nell'interfaccia java.sql.ResultSetMetaData.  
  
 Questo metodo restituirà **true** solo con tipi di dati money e smallmoney di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-methods.md)   
 [Membri di SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-members.md)   
 [Classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
