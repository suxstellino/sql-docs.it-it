---
description: Metodo storesUpperCaseQuotedIdentifiers (SQLServerDatabaseMetaData)
title: Metodo storesUpperCaseQuotedIdentifiers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.storesUpperCaseQuotedIdentifiers
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 936ec140-2597-44e6-82d3-3994a676ee35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d67988b6bd13aa1e0c52eeabe767b4316d708b4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183457"
---
# <a name="storesuppercasequotedidentifiers-method-sqlserverdatabasemetadata"></a>Metodo storesUpperCaseQuotedIdentifiers (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole racchiusi tra virgolette e li archivia in maiuscolo.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean storesUpperCaseQuotedIdentifiers()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se gli identificatori vengono archiviati con lettere maiuscole. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo storesUpperCaseQuotedIdentifiers viene specificato dal metodo storesUpperCaseQuotedIdentifiers nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
