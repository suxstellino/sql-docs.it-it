---
description: Connection (sintassi ADO/WFC)
title: Connection (sintassi ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Connection collection [ADO], ADO/WFC syntax
ms.assetid: 8cfc35bb-91e2-47da-ad4c-982e9162cd51
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c63fe3a2f5e5d9dc02b6dfc8ae2c499049fad6b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026539"
---
# <a name="connection-ado---wfc-syntax"></a>Connection (sintassi ADO/WFC)
## <a name="package-commswfcdata"></a>pacchetto com. ms. wfc. Data  
  
### <a name="constructor"></a>Costruttore  
  
```  
public Connection()  
public Connection(String connectionstring)  
```  
  
### <a name="methods"></a>Metodi  
  
```  
public int beginTrans()  
public void commitTrans()  
public void rollbackTrans()  
public void cancel()  
public void close()  
public com.ms.wfc.data.Recordset execute(String commandText)  
public com.ms.wfc.data.Recordset execute(String commandText, int options)  
public int executeUpdate(String commandText)  
public int executeUpdate(String commandText, int options)  
```  
  
 Il metodo **ExecuteUpdate** è un metodo di caso speciale che chiama il metodo ADO **Execute** sottostante con determinati parametri. Il **metodo ExecuteUpdate** non supporta la restituzione di un oggetto **Recordset** , quindi il parametro *options* del metodo **Execute** viene modificato con **AdoEnums.ExecuteOptions. NORECORDS**. Una volta completato il metodo **Execute** , il relativo parametro *RecordsAffected* aggiornato viene passato nuovamente al metodo **ExecuteUpdate** , che viene infine restituito come **int**.  
  
```  
public void open()   
public void open(String connectionString)  
public void open(String connectionString, String userID)  
public void open(String connectionString, String userID, String password)  
public void open(String connectionString, String userID, String password, int options)  
public Recordset openSchema(int schema, Object[]  
    restrictions, String schemaID)  
public Recordset openSchema(int schema)  
public Recordset openSchema(int schema, Object[] restrictions)  
```  
  
### <a name="properties"></a>Proprietà  
  
```  
public int getAttributes()  
public void setAttributes(int attr)  
public int getCommandTimeout()  
public void setCommandTimeout(int timeout)  
public String getConnectionString()  
public void setConnectionString(String con)  
public int getConnectionTimeout()  
public void setConnectionTimeout(int timeout)  
public int getCursorLocation()  
public void setCursorLocation(int cursorLoc)  
public String getDefaultDatabase()  
public void setDefaultDatabase(String db)  
public int getIsolationLevel()  
public void setIsolationLevel(int level)  
public int getMode()  
public void setMode(int mode)  
public String getProvider()  
public void setProvider(String provider)  
public int getState()  
public String getVersion()  
public AdoProperties getProperties()  
public com.ms.wfc.data.Errors getErrors()  
```  
  
### <a name="events"></a>Eventi  
 Per ulteriori informazioni sugli eventi ADO/WFC, vedere [creazione di un'istanza di evento ADO in base al linguaggio](../../guide/data/ado-event-instantiation-by-language.md).  
  
```  
public void addOnBeginTransComplete(ConnectionEventHandler handler)  
public void removeOnBeginTransComplete(ConnectionEventHandler handler)  
public void addOnCommitTransComplete(ConnectionEventHandler handler)  
public void removeOnCommitTransComplete(ConnectionEventHandler handler)  
public void addOnConnectComplete(ConnectionEventHandler handler)  
public void removeOnConnectComplete(ConnectionEventHandler handler)  
public void addOnDisconnect(ConnectionEventHandler handler)  
public void removeOnDisconnect(ConnectionEventHandler handler)  
public void addOnExecuteComplete(ConnectionEventHandler handler)  
public void removeOnExecuteComplete(ConnectionEventHandler handler)  
public void addOnInfoMessage(ConnectionEventHandler handler)  
public void removeOnInfoMessage(ConnectionEventHandler handler)  
public void addOnRollbackTransComplete(ConnectionEventHandler handler)  
public void removeOnRollbackTransComplete(ConnectionEventHandler handler)  
public void addOnWillConnect(ConnectionEventHandler handler)  
public void removeOnWillConnect(ConnectionEventHandler handler)  
public void addOnWillExecute(ConnectionEventHandler handler)  
public void removeOnWillExecute(ConnectionEventHandler handler)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Connection (ADO)](./connection-object-ado.md)