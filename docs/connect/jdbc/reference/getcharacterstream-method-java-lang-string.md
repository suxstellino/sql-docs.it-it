---
description: Metodo getCharacterStream (java.lang.String)
title: Metodo getCharacterStream (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getCharacterStream (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: cdddc572-05c1-480d-b3e5-28270001575c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a96b5e1ca1e9601399248894628ef7c966dceb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176164"
---
# <a name="getcharacterstream-method-javalangstring"></a>Metodo getCharacterStream (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del nome della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.io.Reader.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.io.Reader getCharacterStream(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnName*  
  
 Valore **String** contenente il nome della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Reader.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getCharacterStream viene specificato dal metodo getCharacterStream nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo legge solo tipi di dati character Unicode di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quali nchar, nvarchar, nvarchar(max) e ntext. Tutti gli altri tipi di dati, inclusi i tipi di caratteri ASCII, genereranno un'eccezione. Per leggere i tipi di dati ASCII, usare il metodo [getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getCharacterStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
