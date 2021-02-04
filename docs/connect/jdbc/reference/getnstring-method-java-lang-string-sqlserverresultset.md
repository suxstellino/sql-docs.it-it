---
description: Metodo getNString (java.lang.String) (SQLServerResultSet)
title: Metodo getNString (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 546d77e2-723a-42ac-ba3f-fabf2395d376
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cdfa86e289bfe8a35657650aa12e2398edad6a8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162604"
---
# <a name="getnstring-method-javalangstring-sqlserverresultset"></a>Metodo getNString (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.lang.String.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getNString(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnLabel*  
  
 Valore String contenente l'etichetta della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto String.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getNString viene specificato dal metodo getNString nell'interfaccia java.sql.SQLServerResultSet.  
  
 Questo metodo può essere usato per recuperare il valore di una colonna **nvarchar**, **nchar**, **nvarchar (max)**, **ntext** o **xml** nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md). Se si tenta di utilizzare questo metodo per recuperare valori di altri tipi di dati, verrà generata un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getNString &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
