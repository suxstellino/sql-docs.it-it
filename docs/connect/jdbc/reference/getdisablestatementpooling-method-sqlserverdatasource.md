---
description: Metodo getDisableStatementPooling (SQLServerDataSource)
title: Metodo getDisableStatementPooling (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 77bcc26fd02a63af1d002fcbe959af68e3f2e634
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175877"
---
# <a name="getdisablestatementpooling-method-sqlserverdatasource"></a>Metodo getDisableStatementPooling (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il valore della proprietà di connessione **disableStatementPooling**. Questa impostazione controlla se il pool di istruzioni è abilitato o meno per questa connessione.

  
## <a name="syntax"></a>Sintassi  
  
```
public boolean getDisableStatementPooling();  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **boolean** che contiene il valore della proprietà di connessione **disableStatementPooling**.
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
