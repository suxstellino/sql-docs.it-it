---
description: Metodo setDisableStatementPooling (SQLServerDataSource)
title: Metodo setDisableStatementPooling (SQLServerDataSource) | Microsoft Docs
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
ms.openlocfilehash: 2aab090785dda921cb1061f5ab59e90136fc3b3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173495"
---
# <a name="setdisablestatementpooling-method-sqlserverdatasource"></a>Metodo setDisableStatementPooling (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore della proprietà di connessione **disableStatementPooling**. Se false, consente di abilitare il pooling dell'istruzione da usare in abbinamento al valore statementPoolingCacheSize > 0.  

## <a name="syntax"></a>Sintassi  
  
```
public void setDisableStatementPooling(boolean disableStatementPooling);  
```  
  
#### <a name="parameters"></a>Parametri  
 *disableStatementPooling*  
  
 Nuovo valore della proprietà di connessione **disableStatementPooling**.  

## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
