---
description: Metodo findColumn (SQLServerResultSet)
title: Metodo findColumn (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.findColumn
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7c29994a-0b53-420b-8a9b-82a9eef08587
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 13f2f1f8c12ec42a4d310080c8c765f276a481d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163367"
---
# <a name="findcolumn-method-sqlserverresultset"></a>Metodo findColumn (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera l'indice della prima colonna corrispondente per il nome di colonna specificato in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int findColumn(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnName*  
  
 Valore **String** contenente il nome della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo findColumn viene specificato dal metodo findColumn nell'interfaccia java.sql.ResultSet.  
  
 Se esistono più colonne con lo stesso nome, il metodo findColumn restituisce la prima corrispondenza con distinzione tra maiuscole e minuscole. Se non esiste alcuna corrispondenza con distinzione tra maiuscole e minuscole, questo metodo restituisce la prima corrispondenza senza tale distinzione.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
