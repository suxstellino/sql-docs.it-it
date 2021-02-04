---
description: Metodo supportsConvert (int, int)
title: Metodo supportsConvert (int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsConvert (int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 54741cfd-32ac-46c5-8b09-fd60fd8833d7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12d08dab58eeeff3daca74812a848ee12e2a626f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183903"
---
# <a name="supportsconvert-method-int-int"></a>Metodo supportsConvert (int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta la funzione CONVERT per due tipi SQL specificati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsConvert(int fromType,  
                               int toType)  
```  
  
#### <a name="parameters"></a>Parametri  
 *fromType*  
  
 Tipo JDBC dal quale convertire.  
  
 *toType*  
  
 Tipo JDBC in cui convertire.  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsConvert viene specificato dal metodo supportsConvert nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo supportsConvert &#40;SQLServerDatabaseMetaData&#41;](../../../connect/jdbc/reference/supportsconvert-method-sqlserverdatabasemetadata.md)   
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
