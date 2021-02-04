---
description: Metodo getMoreResults ()
title: Metodo getMoreResults () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getMoreResults ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: df89db50-0b2f-4094-820a-30be25ad72fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b4afb4fa108c714eaa4f2b48a1403774b30b5546
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162716"
---
# <a name="getmoreresults-method-"></a>Metodo getMoreResults ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Passa al risultato successivo di questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final boolean getMoreResults()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il risultato restituito è un set di risultati. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMoreResults viene specificato dal metodo getMoreResults nell'interfaccia java.sql.Statement.  
  
 La chiamata del metodo getMoreResults chiude in modo implicito tutti gli oggetti del set di risultati ottenuti con il metodo [getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getMoreResults &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)   
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
