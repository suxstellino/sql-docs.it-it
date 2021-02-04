---
description: Metodo getDouble (int) (SQLServerResultSet)
title: Metodo getDouble (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getDouble (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 128df26a-9063-4bdf-a4fb-a077cbe7cfe1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9d75dccc661297a76e86cf2709991bea1de99187
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175864"
---
# <a name="getdouble-method-int-sqlserverresultset"></a>Metodo getDouble (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore dell'indice di colonna designato nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **double** nel linguaggio di programmazione Java.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public double getDouble(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **double**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getDouble viene specificato dal metodo getDouble nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo restituisce tutti i tipi di dati numerici con fedeltà **double** di Java.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getDouble &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getdouble-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
