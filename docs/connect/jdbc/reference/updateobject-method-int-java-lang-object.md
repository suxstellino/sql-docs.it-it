---
description: Metodo updateObject (int, java.lang.Object)
title: Metodo updateObject (int, java.lang.Object) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateObject (int, java.lang.Object)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4993dfe1-2232-4b3c-b931-dfdb35dd225a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7840f396d49f6e7160c2c865aabb3066f983a39c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181616"
---
# <a name="updateobject-method-int-javalangobject"></a>Metodo updateObject (int, java.lang.Object)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata con un valore **Object** in base all'indice della colonna.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateObject(int index,  
                         java.lang.Object obj)  
```  
  
#### <a name="parameters"></a>Parametri  
 *index*  
  
 Valore **int** che indica l'indice di colonna.  
  
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
  
  
