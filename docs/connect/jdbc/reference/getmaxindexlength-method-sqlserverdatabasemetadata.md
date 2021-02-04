---
description: Metodo getMaxIndexLength (SQLServerDatabaseMetaData)
title: Metodo getMaxIndexLength (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxIndexLength
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7c85d021-d466-4732-85f9-53903d297041
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2b6b66f9f8c9add68fa9e1f5f1869d84e805d477
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175525"
---
# <a name="getmaxindexlength-method-sqlserverdatabasemetadata"></a>Metodo getMaxIndexLength (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il numero massimo di byte consentito dal database in un indice, incluse tutte le parti che lo compongono.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int getMaxIndexLength()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **int** che indica il numero massimo di byte consentito.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMaxIndexLength viene specificato dal metodo getMaxIndexLength nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
