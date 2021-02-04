---
description: Metodo getDisableStatementPooling (SQLServerConnection)
title: Metodo getDisableStatementPooling (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getDisableStatementPooling
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6a56685176ff629f0eccdecd0df6aa5c28d3658
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163140"
---
# <a name="getdisablestatementpooling-method-sqlserverconnection"></a>Metodo getDisableStatementPooling (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Restituisce il valore della proprietà di connessione **disableStatementPooling**. Questa impostazione controlla se il pool di istruzioni è abilitato o meno per questa connessione.

## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean getDisableStatementPooling()  
```  

## <a name="return-value"></a>Valore restituito
 Valore **boolean** che contiene il valore della proprietà di connessione **disableStatementPooling**.

## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
