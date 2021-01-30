---
description: Command (sintassi ADO/WFC)
title: Command (sintassi ADO-WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Command collection [ADO], ADO/WFC syntax
ms.assetid: 39d0aa06-03ac-4c9a-8400-83947756ef99
author: rothja
ms.author: jroth
ms.openlocfilehash: b1846915fad045029e27a6fb2f5ebbb32f5beec4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164664"
---
# <a name="command-ado---wfc-syntax"></a>Command (sintassi ADO/WFC)
## <a name="package-commswfcdata"></a>pacchetto com. ms. wfc. Data  
  
### <a name="constructor"></a>Costruttore  
  
```  
public Command()  
public Command(String commandtext)  
```  
  
### <a name="methods"></a>Metodi  
  
```  
public void cancel()  
public com.ms.wfc.data.Parameter createParameter(String  
    Name, int Type, int Direction, int Size, Object Value)  
public Recordset execute()  
public Recordset execute(Object[] parameters)  
public Recordset execute(Object[] parameters, int options)  
public int executeUpdate(Object[] parameters)  
public int executeUpdate(Object[] parameters, int options)  
public int executeUpdate()  
```  
  
 Il metodo **ExecuteUpdate** è un metodo di caso speciale che chiama il metodo ADO **Execute** sottostante con determinati parametri. Il **metodo ExecuteUpdate** non supporta la restituzione di un oggetto **Recordset** , quindi il parametro *options* del metodo **Execute** viene modificato con **AdoEnums.ExecuteOptions. NORECORDS**. Una volta completato il metodo **Execute** , il relativo parametro *RecordsAffected* aggiornato viene passato nuovamente al metodo **ExecuteUpdate** , che viene infine restituito come **int**.  
  
### <a name="properties"></a>Proprietà  
  
```  
public com.ms.wfc.data.Connection getActiveConnection()  
public void setActiveConnection(com.ms.wfc.data.Connection con)  
public void setActiveConnection(String conString)  
public String getCommandText()  
public void setCommandText(String command)  
public int getCommandTimeout()  
public void setCommandTimeout(int timeout)  
public int getCommandType()  
public void setCommandType(int type)  
public String getName()  
public void setName(String name)  
public boolean getPrepared()  
public void setPrepared(boolean prepared)  
public int getState()  
public com.ms.wfc.data.Parameter getParameter(int n)  
public com.ms.wfc.data.Parameter getParameter(String n)  
public com.ms.wfc.data.Parameters getParameters()  
public AdoProperties getProperties()  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Command (ADO)](./command-object-ado.md)