---
description: Metodo setReadOnly (SQLServerConnection)
title: Metodo setReadOnly (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setReadOnly
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bd11fd50-f092-43a0-a6bc-c63e70cff8da
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1150dff13ab8f7f94c8652a24cacb108aace640c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173174"
---
# <a name="setreadonly-method-sqlserverconnection"></a>Metodo setReadOnly (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Attiva la modalità di sola lettura per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) come hint per il driver JDBC in modo da abilitare le ottimizzazioni del database.  
  
> [!NOTE]  
>  Questo metodo non è supportato da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setReadOnly(boolean readOnly)  
```  
  
#### <a name="parameters"></a>Parametri  
 *readOnly*  
  
 **true** se la connessione deve essere di sola lettura. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setReadOnly viene specificato dal metodo setReadOnly nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
