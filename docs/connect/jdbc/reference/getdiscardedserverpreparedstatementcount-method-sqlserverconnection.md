---
description: Metodo getDiscardedServerPreparedStatementCount (SQLServerConnection)
title: Metodo getDiscardedServerPreparedStatementCount (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getDiscardedServerPreparedStatementCount
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 003472ce52b4ec99a94a610bca4e2b5304f1d4b0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163123"
---
# <a name="getdiscardedserverpreparedstatementcount-method-sqlserverconnection"></a>Metodo getDiscardedServerPreparedStatementCount (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Restituisce il numero di azioni di annullamento della preparazione per le istruzioni preparate attualmente in sospeso.

## <a name="syntax"></a>Sintassi  
  
```  
  
public int getDiscardedServerPreparedStatementCount()  
```  

## <a name="return-value"></a>Valore restituito
 Valore **int** che contiene il numero di azioni di annullamento della preparazione per le istruzioni preparate attualmente in attesa.

## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
