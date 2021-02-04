---
description: Metodo setEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection)
title: Metodo setEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setEnablePrepareOnFirstPreparedStatementCall
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0c96860c17e56857171c3f69f15bdd0c966d1c84
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178991"
---
# <a name="setenableprepareonfirstpreparedstatementcall-method-sqlserverconnection"></a>Metodo setEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Indica il comportamento per un'istanza di connessione specifica. Se è il valore è false, la prima esecuzione chiamerà sp_executesql e non preparerà un'istruzione. La seconda esecuzione chiamerà sp_prepexec e configurerà effettivamente un handle per l'istruzione preparata. Le esecuzioni seguenti chiameranno sp_execute. Viene così eliminata la necessità di sp_unprepare alla chiusura dell'istruzione preparata se l'istruzione viene eseguita una sola volta.

## <a name="syntax"></a>Sintassi  
  
```  
  
public void setEnablePrepareOnFirstPreparedStatementCall(boolean enablePrepareOnFirstPreparedStatementCall)  
```  
  
#### <a name="parameters"></a>Parametri  
 *enablePrepareOnFirstPreparedStatementCall*  
  
 Nuovo valore della proprietà di connessione **enablePrepareOnFirstPreparedStatementCall**.  
 
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
