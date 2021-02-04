---
description: Metodo setLoginTimeout (SQLServerDataSource)
title: Metodo setLoginTimeout (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLoginTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b63d1cf4-dc1b-4e35-9a56-50436642eaaf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 725dae8c42fa83b92db0a56f8cd6b8c758fd4f6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173391"
---
# <a name="setlogintimeout-method-sqlserverdatasource"></a>Metodo setLoginTimeout (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il numero di secondi di attesa dell'oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) durante il tentativo di stabilire una connessione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setLoginTimeout(int loginTimeout)  
```  
  
#### <a name="parameters"></a>Parametri  
 *loginTimeout*  
  
 Valore **int** che rappresenta il numero di secondi di attesa. Un valore pari a zero indica che il timeout è quello predefinito di sistema, impostato su 15 secondi.  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setLoginTimeout viene specificato dal metodo setLoginTimeout nell'interfaccia javax.sql.DataSource.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
