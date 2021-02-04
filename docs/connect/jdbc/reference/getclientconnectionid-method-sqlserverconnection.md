---
description: Metodo getClientConnectionID (SQLServerConnection)
title: Metodo getClientConnectionID (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: bee39c11-733a-461f-92cc-33efcb2af87d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d216f6a4a5cbe24b2b73f7669d54b38faf639053
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176141"
---
# <a name="getclientconnectionid-method-sqlserverconnection"></a>Metodo getClientConnectionID (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ottiene l'ID connessione del tentativo di connessione più recente, indipendentemente dalla riuscita o meno del tentativo.  
  
## <a name="syntax"></a>Sintassi  
  
``` 
public Java.util.UUID SQLServerConnection.getClientConnectionID();  
```  
  
## <a name="return-value"></a>Valore restituito  
 GUID a 16 byte che rappresenta l'ID connessione del tentativo di connessione più recente oppure NULL in caso di errore dopo aver avviato la richiesta di connessione e l'handshake pre-login.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Per altre informazioni sull'accesso alle informazioni di diagnostica nel log degli eventi estesi, vedere [Accesso alle informazioni di diagnostica nel log degli eventi estesi](../../../connect/jdbc/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
 Nell'esempio seguente viene mostrato come ottenere l'ID connessione:  
  
```  
Connection con = DriverManager.getConnection(connectionUrl);  
UUID id = ((ISQLServerConnection)con).getClientConnectionId();  
```  
  
 Nell'esempio seguente viene mostrato come ottenere l'ID connessione in modo diverso:  
  
```  
SQLServerConnectionPoolDataSource ds = new SQLServerConnectionPoolDataSource();  
ds.setUser("...");  
ds.setPassword("...");  
ds.setServerName("...");  
PooledConnection pcon= ds.getPooledConnection();  
Connection cn = pcon.getConnection();  
UUID conid = ((ISQLServerConnection)cn).getClientConnectionId();  
```  
  
 **getClientConnectionID** funziona indipendentemente dalla versione del server a cui ci si connette, ma i registri degli eventi estesi e la voce relativa agli errori del buffer circolare di connettività non saranno presenti in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2008 R2 e versioni precedenti.  
  
 È possibile individuare l'ID connessione nel log degli eventi estesi per verificare se l'errore sia nel server qualora l'evento esteso per la registrazione dell'ID connessione sia abilitato. È anche possibile trovare l'ID connessione nel buffer circolare di connessione ([Risoluzione dei problemi di connettività in SQL Server 2008 con il buffer circolare della connettività](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) per determinati errori di connessione. Se l'ID connessione non si trova nel buffer circolare di connessione, si può presumere che si tratti di un errore di rete.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
