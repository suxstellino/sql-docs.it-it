---
description: Metodo updateObject (java.lang.String, java.lang.Object)
title: Metodo updateObject (java.lang.String, java.lang.Object) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateObject (java.lang.String, java.lang.Object)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f6999d9c-eab6-4e4d-96d8-e0fa4b4b87e3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: feba431ea525187fa79e38ae70212e935fe5dcf5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174762"
---
# <a name="updateobject-method-javalangstring-javalangobject"></a>Metodo updateObject (java.lang.String, java.lang.Object)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con un valore **Object** in base al nome della colonna.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateObject(java.lang.String columnName,  
                         java.lang.Object x)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnName*  
  
 Valore **String** contenente il nome della colonna.  
  
 *obj*  
  
 Valore **Object**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateObject viene specificato dal metodo updateObject nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateObject &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
