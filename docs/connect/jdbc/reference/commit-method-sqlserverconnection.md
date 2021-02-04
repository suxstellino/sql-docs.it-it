---
description: Metodo commit (SQLServerConnection)
title: Metodo commit (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.commit
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c7346165-51bf-4844-b64c-29833c147236
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd2df158e2b7214ef64dae3bda7a2288edb43a8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176246"
---
# <a name="commit-method-sqlserverconnection"></a>Metodo commit (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rende permanenti tutte le modifiche apportate dal commit o dal rollback precedente e rilascia eventuali blocchi del database contenuti in questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void commit()  
```  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo commit viene specificato dal metodo commit nell'interfaccia java.sql.Connection.  
  
 Questo metodo deve essere utilizzato solo quando la modalità autocommit è stata disabilitata.  
  
 Si noti che questo metodo avrà esito negativo e genererà un'eccezione se il client avvia una transazione manuale e se successivamente, per qualche motivo, SQL Server esegue il rollback della transazione manuale. Ad esempio, viene generata un'eccezione se il client chiama una stored procedure che chiama in modo esplicito ROLLBACK TRANSACTION e se il client chiama quindi il metodo commit. Inoltre, se SQL Server genera un errore la cui gravità è sufficiente (almeno 16) per eseguire il rollback della transazione manuale avviata dal client, una chiamata successiva al metodo commit genererà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
