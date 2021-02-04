---
description: Metodo getFetchDirection (SQLServerStatement)
title: Metodo getFetchDirection (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ceb4ae68-decc-46d3-83f1-0bbd23aaf58c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a2e23066c13ecba2034b79432fededbf88edaa07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163046"
---
# <a name="getfetchdirection-method-sqlserverstatement"></a>Metodo getFetchDirection (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera la direzione per il recupero di righe da tabelle di database che rappresenta l'impostazione predefinita per i set di risultati generati da questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
> [!NOTE]  
>  Questo metodo non è attualmente implementato da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Verrà pertanto sempre restituito FETCH_UNKNOWN.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final int getFetchDirection()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica la direzione di recupero specificata dal metodo [setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getFetchDirection viene specificato dal metodo getFetchDirection nell'interfaccia java.sql.Statement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
