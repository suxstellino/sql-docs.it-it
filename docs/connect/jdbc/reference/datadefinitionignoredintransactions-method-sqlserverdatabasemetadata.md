---
description: Metodo dataDefinitionIgnoredInTransactions (SQLServerDatabaseMetaData)
title: Il database ignora l'istruzione di definizione dei dati nella transazione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.dataDefinitionIgnoredInTransactions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1674fb46-43a7-46d0-9f05-cf993d3bc032
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6f31169c0908839c60506842ebac8a15e6927c7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168542"
---
# <a name="datadefinitionignoredintransactions-method-sqlserverdatabasemetadata"></a>Metodo dataDefinitionIgnoredInTransactions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database ignora un'istruzione di definizione dei dati all'interno di una transazione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean dataDefinitionIgnoredInTransactions()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se le istruzioni DDL vengono ignorate all'interno delle transazioni. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo dataDefinitionIgnoredInTransactions viene specificato dal metodo dataDefinitionIgnoredInTransactions nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
