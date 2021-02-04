---
description: Metodo getQueryTimeout (SQLServerStatement)
title: Metodo getQueryTimeout (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getQueryTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8dff954f-b458-4fa6-abe6-be62ff75e2b9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e70b35cb96483d5301747e69e67d95135f95ed20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162476"
---
# <a name="getquerytimeout-method-sqlserverstatement"></a>Metodo getQueryTimeout (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero di secondi di attesa di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] prima dell'esecuzione di questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final int getQueryTimeout()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica il numero di secondi di attesa del driver JDBC oppure 0 se non esiste alcun limite.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getQueryTimeout viene specificato dal metodo getQueryTimeout nell'interfaccia java.sql.Statement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
