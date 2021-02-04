---
description: Metodo setTransactionIsolation (SQLServerConnection)
title: Metodo setTransactionIsolation (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setTransactionIsolation
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6a8fa4d3-5237-40f8-8a02-b40a3d7a1131
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f391337a0034fa7feeb284352d8c3d677a3c3fa7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178486"
---
# <a name="settransactionisolation-method-sqlserverconnection"></a>Metodo setTransactionIsolation (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cerca di impostare sul valore specificato il livello di isolamento della transazione corrente per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setTransactionIsolation(int level)  
```  
  
#### <a name="parameters"></a>Parametri  
 *level*  
  
 Valore **int** contenente uno dei livelli di isolamento seguenti:  
  
 TRANSACTION_READ_UNCOMMITTED  
  
 TRANSACTION_READ_COMMITTED  
  
 TRANSACTION_REPEATABLE_READ  
  
 TRANSACTION_SERIALIZABLE  
  
 TRANSACTION_SNAPSHOT = 0x1000  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setTransactionIsolation viene specificato dal metodo setTransactionIsolation nell'interfaccia java.sql.Connection.  
  
 Il commit delle transazioni non viene eseguito se questo metodo viene chiamato durante una transazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
