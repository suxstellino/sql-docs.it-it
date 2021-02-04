---
description: Metodo insertRow (SQLServerResultSet)
title: Metodo insertRow (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.insertRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 363d1008-1396-4fc0-8e27-c9ba2499e7f1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8571bb1e069be74de9ec93890606b887d247e5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177514"
---
# <a name="insertrow-method-sqlserverresultset"></a>Metodo insertRow (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiunge il contenuto della riga di inserimento a questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) e al database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void insertRow()  
```  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo insertRow viene specificato dal metodo insertRow nell'interfaccia java.sql.ResultSet.  
  
 Il cursore deve trovarsi sulla riga di inserimento quando viene effettuata la chiamata a questo metodo. Una volta chiamato il metodo, il cursore rimane sulla riga di inserimento e il set di risultati rimane in modalità di inserimento.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
