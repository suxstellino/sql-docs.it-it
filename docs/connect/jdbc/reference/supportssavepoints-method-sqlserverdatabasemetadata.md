---
description: Metodo supportsSavepoints (SQLServerDatabaseMetaData)
title: Metodo supportsSavepoints (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSavepoints
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: cd2418c9-e95d-4820-82bd-944287387b60
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0c0191aaa1f7df26c2ab2fa2f4c496c1638f1d43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187198"
---
# <a name="supportssavepoints-method-sqlserverdatabasemetadata"></a>Metodo supportsSavepoints (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta punti di salvataggio.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsSavepoints()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsSavepoints viene specificato dal metodo supportsSavepoints nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
