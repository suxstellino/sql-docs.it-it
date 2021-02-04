---
description: Metodo getMaxConnections (SQLServerDatabaseMetaData)
title: Metodo getMaxConnections (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxConnections
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 745410f7-e59b-4423-9728-c903adedc399
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dceb62cfa315646d4ca1c381cce433832e170072
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162819"
---
# <a name="getmaxconnections-method-sqlserverdatabasemetadata"></a>Metodo getMaxConnections (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero massimo consentito di connessioni simultanee al database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getMaxConnections()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica il numero massimo di connessioni simultanee consentite.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMaxConnections viene specificato dal metodo getMaxConnections nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
