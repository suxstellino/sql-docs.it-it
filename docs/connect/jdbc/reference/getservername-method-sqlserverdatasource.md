---
description: Metodo getServerName (SQLServerDataSource)
title: Metodo getServerName (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getServerName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3004ed22-5d69-4dd0-8761-d39f0b7dde13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0e3669bfacc98989a1e21852ad53cabfee95ce0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162343"
---
# <a name="getservername-method-sqlserverdatasource"></a>Metodo getServerName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getServerName()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il nome del server o Null se non viene impostato alcun valore.  
  
## <a name="remarks"></a>Osservazioni  
 Il nome del server è il nome host del computer di destinazione che esegue [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se la proprietà getServerName non è impostata, getServerName restituisce il valore predefinito Null.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
