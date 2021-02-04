---
description: Metodo getTrustManagerConstructorArg (SQLServerDataSource)
title: Metodo getTrustManagerConstructorArg (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getTrustManagerConstructorArg
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41e3a4c86a5b5ad43a425614fab09d59a9d0340e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165482"
---
# <a name="gettrustmanagerconstructorarg-method-sqlserverdatasource"></a>Metodo getTrustManagerConstructorArg (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il valore String della proprietà di connessione TrustManagerConstructorArg.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getTrustManagerConstructorArg()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** che contiene il valore della proprietà di connessione TrustManagerConstructorArg o Null se non è impostato alcun valore.  
  
## <a name="remarks"></a>Commenti  
 Se la proprietà TrustManagerClass non è impostata, il metodo [getTrustManagerConstructorArg](../../../connect/jdbc/reference/gettrustmanagerconstructorarg-method-sqlserverdatasource.md) restituisce Null.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
