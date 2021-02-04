---
description: Metodo getFetchSize (SQLServerStatement)
title: Metodo getFetchSize (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getFetchSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8115ca58-8ae9-46ce-8515-7905d7bb25fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216ed0d1989a1646add0910fab4f40a929ecd0ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163039"
---
# <a name="getfetchsize-method-sqlserverstatement"></a>Metodo getFetchSize (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero di righe del set di risultati che rappresenta la dimensione di recupero predefinita per gli oggetti del set di risultati generati da questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final int getFetchSize()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica la dimensione di recupero, specificata dal metodo [setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getFetchSize viene specificato dal metodo getFetchSize nell'interfaccia java.sql.Statement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
