---
description: Metodo nullsAreSortedAtStart (SQLServerDatabaseMetaData)
title: Metodo nullsAreSortedAtStart (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.nullsAreSortedAtStart Method (SQLServerDatabaseMetaData)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 372515da-3b0e-46f6-8c0b-01b1b45c5a2f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 289990ad27c92237836b30fef6074e53bf3f8c4d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176974"
---
# <a name="nullsaresortedatstart-method-sqlserverdatabasemetadata"></a>Metodo nullsAreSortedAtStart (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un valore che indica se i valori NULL vengono posizionati all'inizio, indipendentemente dal tipo di ordinamento.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean nullsAreSortedAtStart()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se posizionato all'inizio. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo nullsAreSortedAtStart viene specificato dal metodo nullsAreSortedAtStart nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
