---
description: Metodo nullsAreSortedAtEnd (SQLServerDatabaseMetaData)
title: Metodo nullsAreSortedAtEnd (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.nullsAreSortedAtEnd
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 713cf636-40f2-474a-8a5d-5aba4a310a9c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2bc2e9c761fc798806019f7bf0183d4a2e200474
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176992"
---
# <a name="nullsaresortedatend-method-sqlserverdatabasemetadata"></a>Metodo nullsAreSortedAtEnd (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se i valori NULL vengono posizionati alla fine, indipendentemente dal tipo di ordinamento.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean nullsAreSortedAtEnd()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se ordinato alla fine. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo nullsAreSortedAtEnd viene specificato dal metodo nullsAreSortedAtEnd nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
