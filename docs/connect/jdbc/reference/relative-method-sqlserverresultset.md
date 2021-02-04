---
description: Metodo relative (SQLServerResultSet)
title: Metodo relative (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.relative
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2bcdbb69-95fd-4ae8-8488-1a75a91fe2e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d61c075e123fc4ae77d852fbe2c3e221bf6ca231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176665"
---
# <a name="relative-method-sqlserverresultset"></a>Metodo relative (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Sposta il cursore del numero di righe specificato rispetto alla riga corrente, in direzione positiva o negativa.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean relative(int nRows)  
```  
  
#### <a name="parameters"></a>Parametri  
 *nRows*  
  
 Valore **int** che indica il numero di righe da spostare.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il cursore si trova in una riga. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo relative viene specificato dal metodo relative nell'interfaccia java.sql.ResultSet.  
  
 Se si tenta di spostare oltre la prima o ultima riga nel set di risultati, il cursore viene posizionato prima o dopo la prima o ultima riga. La chiamata al metodo `relative(0)` è valida, ma non modifica la posizione del cursore.  
  
 La chiamata al metodo `relative(1)` è identica alla chiamata al metodo [next](../../../connect/jdbc/reference/next-method-sqlserverresultset.md). La chiamata al metodo `relative(-1)` è identica alla chiamata al metodo [previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
