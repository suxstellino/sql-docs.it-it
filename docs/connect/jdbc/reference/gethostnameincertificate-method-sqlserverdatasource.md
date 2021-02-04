---
description: Metodo getHostNameInCertificate (SQLServerDataSource)
title: Metodo getHostNameInCertificate (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- getHostNameInCertificate Method (SQLServerDataSource)
apilocation:
- getHostNameInCertificate Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 45ea04e2-9ea5-4171-9136-d09f8a95e128
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2514fae5914ddf409254531961e4e67765438aac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162966"
---
# <a name="gethostnameincertificate-method-sqlserverdatasource"></a>Metodo getHostNameInCertificate (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il nome host usato per convalidare il certificato Transport Layer Security (TLS) di SQL Server, noto in precedenza come Secure Sockets Layer (SSL).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getHostNameInCertificate()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il nome host oppure Null se non è impostato alcun valore.  
  
## <a name="remarks"></a>Osservazioni  
 Il nome host viene usato per la convalida del valore del certificato TLS/SSL di SQL Server quando il livello di comunicazione è crittografato tramite TLS/SSL.  
  
 Se il nome host non è impostato, il metodo [getHostNameInCertificate](../../../connect/jdbc/reference/gethostnameincertificate-method-sqlserverdatasource.md) restituisce Null.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
