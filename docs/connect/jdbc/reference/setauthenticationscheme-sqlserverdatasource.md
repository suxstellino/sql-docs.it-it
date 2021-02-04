---
description: setAuthenticationScheme (SQLServerDataSource)
title: setAuthenticationScheme (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b942f78e-7ce1-44ef-923d-a7c3d7c76b83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93ee3d9cf4769c1552f2c44cedfb21254c046b24
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173954"
---
# <a name="setauthenticationscheme-sqlserverdatasource"></a>setAuthenticationScheme (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica il tipo di sicurezza integrata che si desidera venga utilizzata dall'applicazione.  
  
## <a name="syntax"></a>Sintassi  
  
```vb  
public void setAuthenticationScheme(String authenticationScheme);  
```  
  
#### <a name="parameters"></a>Parametri  
 *authenticationScheme*  
  
 I valori sono **"JavaKerberos"** e il valore predefinito **"NativeAuthentication"**. Per altre informazioni, vedere [Uso dell'autenticazione integrata Kerberos per la connessione a SQL Server](../../../connect/jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
