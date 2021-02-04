---
description: Metodo getCatalogSeparator (SQLServerDatabaseMetaData)
title: Metodo getCatalogSeparator (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getCatalogSeparator
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0bbd6842-7210-432a-bef4-e15a63f5cc96
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d6f4e46bd327c7255ccf08d16c3b3d7cab9c0d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176204"
---
# <a name="getcatalogseparator-method-sqlserverdatabasemetadata"></a>Metodo getCatalogSeparator (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore **String** usato dal database come separatore tra un nome di catalogo e un nome di tabella.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getCatalogSeparator()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** contenente il separatore di catalogo.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getCatalogSeparator viene specificato dal metodo getCatalogSeparator nell'interfaccia java.sql.DatabaseMetaData.  
  
 Quando si usa [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] con un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] questo metodo restituisce un punto (".") come separatore di catalogo.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
