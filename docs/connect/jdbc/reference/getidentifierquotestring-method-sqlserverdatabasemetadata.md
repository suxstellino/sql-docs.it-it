---
description: Metodo getIdentifierQuoteString (SQLServerDatabaseMetaData)
title: Metodo getIdentifierQuoteString (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getIdentifierQuoteString
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6dea35a0-56a8-412c-8cd3-6539527ff597
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 83729f8724fddf9e6399a97983c5ba6b9fd964f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175731"
---
# <a name="getidentifierquotestring-method-sqlserverdatabasemetadata"></a>Metodo getIdentifierQuoteString (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore **String** usato per racchiudere tra virgolette gli identificatori SQL.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getIdentifierQuoteString()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente gli identificatori delimitati.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getIdentifierQuoteString viene specificato dal metodo getIdentifierQuoteString nell'interfaccia java.sql.DatabaseMetaData.  
  
 Quando si usa il driver JDBC di [!INCLUDE[msCoName](../../../includes/msconame_md.md)] con un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], questo metodo restituisce le **virgolette** ("").  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
