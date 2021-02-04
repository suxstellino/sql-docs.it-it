---
description: Metodo supportsStoredFunctionsUsingCallSyntax (SQLServerDatabaseMetaData)
title: Metodo supportsStoredFunctionsUsingCallSyntax | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 0e5c0579-84b5-4717-b128-0bcd512f6022
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12e85da95c06aebdef1106b1544cefd5c44ff6b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188688"
---
# <a name="supportsstoredfunctionsusingcallsyntax-method-sqlserverdatabasemetadata"></a>Metodo supportsStoredFunctionsUsingCallSyntax (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica se nel database corrente è supportato il richiamo di funzioni definite dal fornitore o dall'utente tramite la sintassi di escape delle stored procedure.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean supportsStoredFunctionsUsingCallSyntax()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se supportato. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo supportsStoredFunctionsUsingCallSyntax viene specificato dal metodo supportsStoredFunctionsUsingCallSyntax nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
