---
description: Metodo setHostNameInCertificate (SQLServerDataSource)
title: Metodo setHostNameInCertificate (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setHostNameInCertificate Method (SQLServerDataSource)
apilocation:
- setHostNameInCertificate Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 2bcf4f2e-a103-4374-abc4-ffad4ce8e3c0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 37405a18c794b1fa7cbddf0fdaed92a4727f9907
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178970"
---
# <a name="sethostnameincertificate-method-sqlserverdatasource"></a>Metodo setHostNameInCertificate (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il nome host da usare per convalidare il certificato Transport Layer Security (TLS), noto in precedenza come Secure Sockets Layer (SSL), di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setHostNameInCertificate(java.lang.String hostNameInCertificate)  
```  
  
#### <a name="parameters"></a>Parametri  
 *hostNameInCertificate*  
  
 Valore **String** contenente il nome host.  
  
## <a name="remarks"></a>Osservazioni  
 Il valore di hostNameInCertificate viene usato per la convalida del certificato TLS/SSL di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quando il livello di comunicazione è crittografato tramite TLS. Il valore predefinito è null.  
  
 Se la proprietà hostNameInCertificate è impostata su Null o non è specificata, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] usa il valore della proprietà serverName per la convalida del certificato TLS/SSL di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se la proprietà hostNameInCertificate è impostata su una stringa o una stringa vuota "", tale valore verrà usato dal driver per la convalida del certificato TLS/SSL server.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
