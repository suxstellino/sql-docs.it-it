---
description: Metodo getCursorName (SQLServerResultSet)
title: Metodo getCursorName (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getCursorName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e5b3af67-423a-4551-a4c6-a4bc076bd504
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54ec61d91a360e0381aa9e0b969a7a09f191cbf4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163255"
---
# <a name="getcursorname-method-sqlserverresultset"></a>Metodo getCursorName (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il nome del cursore SQL usato da questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
> [!NOTE]  
>  Questo metodo non è attualmente supportato da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Se viene chiamato, verrà generata un'eccezione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getCursorName()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il nome del cursore.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getCursorName viene specificato dal metodo getCursorName nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
