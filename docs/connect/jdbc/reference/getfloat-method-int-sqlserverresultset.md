---
description: Metodo getFloat (int) (SQLServerResultSet)
title: Metodo getFloat (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getFloat (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 30863ef5-7a7c-440e-8fbb-426a99266ee1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e25d195f024b38f497b90e0d14b662dbc8a7ab75
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175770"
---
# <a name="getfloat-method-int-sqlserverresultset"></a>Metodo getFloat (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore dell'indice di colonna designato nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **float** nel linguaggio di programmazione Java.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public float getFloat(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **float**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getFloat viene specificato dal metodo getFloat nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo restituisce tutti i tipi numerici con fedeltà **float** di Java.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getFloat &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getfloat-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
