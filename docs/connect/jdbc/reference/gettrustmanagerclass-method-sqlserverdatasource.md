---
description: Metodo getTrustManagerClass (SQLServerDataSource)
title: Metodo getTrustManagerClass (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getTrustManagerClass
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 528956dea570c176a11c3d3c854e7ee0cff7a876
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162019"
---
# <a name="gettrustmanagerclass-method-sqlserverdatasource"></a>Metodo getTrustManagerClass (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce il valore String della proprietà di connessione TrustManagerClass.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getTrustManagerClass()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** che contiene il valore della proprietà di connessione TrustManagerClass o Null se non è impostato alcun valore.  
  
## <a name="remarks"></a>Commenti  
 Se la proprietà TrustManagerClass non è impostata, il metodo [getTrustManagerClass](../../../connect/jdbc/reference/gettrustmanagerclass-method-sqlserverdatasource.md) restituisce Null.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
