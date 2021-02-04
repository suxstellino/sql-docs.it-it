---
description: Metodo supportsDataDefinitionAndDataManipulationTransactions (SQLServerDatabaseMetaData)
title: Metodo supportsDataDefinitionAndDataManipulationTransactions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsDataDefinitionAndDataManipulationTransactions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: fe91c601-9bb3-4364-9131-575a94d3a1b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b9f6c1a87484bf1749109e6811b1652bba8ce935
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183796"
---
# <a name="supportsdatadefinitionanddatamanipulationtransactions-method-sqlserverdatabasemetadata"></a>Metodo supportsDataDefinitionAndDataManipulationTransactions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database supporta sia l'istruzione di definizione dei dati sia quella di manipolazione dei dati all'interno di una transazione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsDataDefinitionAndDataManipulationTransactions()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsDataDefinitionAndDataManipulationTransactions viene specificato dal metodo supportsDataDefinitionAndDataManipulationTransactions nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
