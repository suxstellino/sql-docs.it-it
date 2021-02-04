---
description: Metodo getMetaData (SQLServerConnection)
title: Metodo getMetaData (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getMetaData
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 86223cb5-3bf4-489a-8c82-669a91764f2b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0480d1852c55806f09391be330d9c286f40a3389
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175498"
---
# <a name="getmetadata-method-sqlserverconnection"></a>Metodo getMetaData (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un oggetto [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) che contiene metadati sul database per cui questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) rappresenta una connessione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.DatabaseMetaData getMetaData()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto DatabaseMetaData.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMetaData viene specificato dal metodo getMetaData nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
