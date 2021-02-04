---
description: Metodo getSchemaTerm (SQLServerDatabaseMetaData)
title: Metodo getSchemaTerm (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getSchemaTerm
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3e4a400f-0859-4ac3-983e-c25633b33683
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: deb10b13f58f8c0871249bf1205188a1c95e9f16
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175116"
---
# <a name="getschematerm-method-sqlserverdatabasemetadata"></a>Metodo getSchemaTerm (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il termine preferito per indicare "schema" nel database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getSchemaTerm()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il termine preferito.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getSchemaTerm viene specificato dal metodo getSchemaTerm nell'interfaccia java.sql.DatabaseMetaData.  
  
 Quando si usa [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] con un database [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], questo metodo restituisce "schema" come termine preferito.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
