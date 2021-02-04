---
description: Metodo releaseSavepoint (SQLServerConnection)
title: Metodo releaseSavepoint (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.releaseSavepoint
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6b625ea-c7ce-4a32-a9e0-6d2b4321bfd8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43a159feee134937fa14889fde196b2115e59494
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174172"
---
# <a name="releasesavepoint-method-sqlserverconnection"></a>Metodo releaseSavepoint (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rimuove l'oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) specificato dalla transazione corrente.  
  
> [!NOTE]  
>  Questo metodo non è attualmente supportato da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void releaseSavepoint(java.sql.Savepoint savepoint)  
```  
  
#### <a name="parameters"></a>Parametri  
 *savepoint*  
  
 Oggetto SavePoint da rimuovere.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo releaseSavepoint viene specificato dal metodo releaseSavepoint nell'interfaccia java.sql.Connection.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
