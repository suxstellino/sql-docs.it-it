---
description: Metodo getStatementPoolingCacheSize (SQLServerConnection)
title: Metodo getStatementPoolingCacheSize (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getStatementPoolingCacheSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2b20666d717d356c7f3bcd578caac889f9f61ac2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174948"
---
# <a name="getstatementpoolingcachesize-method-sqlserverconnection"></a>Metodo getStatementPoolingCacheSize (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Restituisce le dimensioni della cache delle istruzioni preparate per questa connessione. '0' indica che la memorizzazione nella cache non è abilitata.

## <a name="syntax"></a>Sintassi  
  
```  
  
public int getStatementPoolingCacheSize()  
```  

## <a name="return-value"></a>Valore restituito
 Valore **int** che contiene il valore della proprietà di connessione **statementPoolingCacheSize**.

## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
