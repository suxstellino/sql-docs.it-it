---
description: Metodo next (SQLServerResultSet)
title: Metodo next (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.next
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 60248447-6908-4036-a779-a501453cd553
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 17e4ba604834b486ae6e4d38cdea81ac9bd86606
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177004"
---
# <a name="next-method-sqlserverresultset"></a>Metodo next (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Sposta il cursore di una riga verso il basso dalla posizione corrente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean next()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se la nuova riga corrente è valida. **false** se non sono presenti altre righe da elaborare.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo next viene specificato dal metodo next nell'interfaccia java.sql.ResultSet.  
  
 Un cursore del set di risultati viene posizionato inizialmente prima della prima riga. La prima chiamata al metodo next rende la prima riga quella corrente, la seconda chiamata rende la seconda riga quella corrente e così via.  
  
 Se un flusso di input è aperto per la riga corrente, una chiamata al metodo next lo chiuderà in modo implicito. Una catena di avvisi per l'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) viene cancellata quando viene letta una nuova riga.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
