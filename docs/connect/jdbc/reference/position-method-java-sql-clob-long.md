---
description: Metodo position (java.sql.Clob, long)
title: Metodo position (java.sql.Clob, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.position (java.sql.Clob, long)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b2fb34d5-1d34-4764-a795-712d9c6aa313
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 56c541b6416e21ef0d291cd57d8015f2c7a81491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176885"
---
# <a name="position-method-javasqlclob-long"></a>Metodo position (java.sql.Clob, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce la posizione del carattere dell'oggetto CLOB specificato in tale oggetto in base alla posizione di inizio specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public long position(java.sql.Clob searchstr,  
                     long start)  
```  
  
#### <a name="parameters"></a>Parametri  
 *searchstr*  
  
 Sottostringa da cercare.  
  
 *start*  
  
 Posizione in corrispondenza della quale iniziare la ricerca. La prima posizione è 1.  
  
## <a name="return-value"></a>Valore restituito  
 Posizione in cui viene visualizzata la sottostringa oppure -1 se non è presente. La prima posizione è 1.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo position viene specificato dal metodo position nell'interfaccia java.sql.Clob.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo position &#40;SQLServerClob&#41;](../../../connect/jdbc/reference/position-method-sqlserverclob.md)   
 [Metodi di SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Membri di SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Classe SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
