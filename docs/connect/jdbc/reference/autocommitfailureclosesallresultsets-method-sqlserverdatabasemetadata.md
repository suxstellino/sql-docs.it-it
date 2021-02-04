---
description: Metodo autoCommitFailureClosesAllResultSets (SQLServerDatabaseMetaData)
title: Il driver JDBC chiude i set di risultati aperti | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 1739ecb5-e5cb-4807-b5a8-97c0299929d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6482a22b6a39db54d69638b0a91c2c84f2d21477
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163544"
---
# <a name="autocommitfailureclosesallresultsets-method-sqlserverdatabasemetadata"></a>Metodo autoCommitFailureClosesAllResultSets (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica se tramite il driver JDBC vengono chiusi tutti i set di risultati aperti, inclusi quelli trattenibili, quando è abilitata una modalità di commit automatico e viene generata un'eccezione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean autoCommitFailureClosesAllResultSets()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se vengono chiusi tutti i set di risultati aperti, inclusi quelli trattenibili, quando è abilitata una modalità di commit automatico e viene generata un'eccezione. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo autoCommitFailureClosesAllResultSets viene specificato dal metodo autoCommitFailureClosesAllResultSets nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
