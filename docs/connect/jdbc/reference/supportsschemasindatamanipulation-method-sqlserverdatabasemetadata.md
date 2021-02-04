---
description: Metodo supportsSchemasInDataManipulation (SQLServerDatabaseMetaData)
title: Metodo supportsSchemasInDataManipulation | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSchemasInDataManipulation
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 812dc551-c718-494e-80d9-75732464c8ba
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7f6c79603f54b1e594d25fb145ada5af2ca6bf4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187677"
---
# <a name="supportsschemasindatamanipulation-method-sqlserverdatabasemetadata"></a>Metodo supportsSchemasInDataManipulation (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se un nome di schema può essere utilizzato in un'istruzione di manipolazione dei dati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsSchemasInDataManipulation()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsSchemasInDataManipulation viene specificato dal metodo supportsSchemasInDataManipulation nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
