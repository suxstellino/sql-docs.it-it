---
description: Metodo updateClob (int, java.sql.Clob)
title: Metodo updateClob (int, java.sql.Clob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateClob (int, java.sql.Clob)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d2a5e9cb-2631-4f6e-a90c-4bee58e2f7b8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8f0fe91ab07f350bf8d058b8cc692e4075c338f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188205"
---
# <a name="updateclob-method-int-javasqlclob"></a>Metodo updateClob (int, java.sql.Clob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con il valore java.sql.Clob dopo che è stato specificato l'indice della colonna.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateClob(int columnIndex,  
                       java.sql.Clob clobValue)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
 *clobValue*  
  
 Oggetto Clob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateClob viene specificato dal metodo updateClob nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateClob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
