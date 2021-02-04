---
description: Metodo getURL (int)
title: Metodo getURL (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getURL Ijnt)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 75d03ced-3614-4997-9abd-24642b1d1aae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5b1e9462703e9ce2f81ac473b6d5da32b4ca26a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177673"
---
# <a name="geturl-method-int"></a>Metodo getURL (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto URL nel linguaggio di programmazione Java in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.net.URL getURL(int n)  
```  
  
#### <a name="parameters"></a>Parametri  
 *n*  
  
 Valore **int** che specifica l'indice del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto URL.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getURL viene specificato dal metodo getURL nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getURL &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/geturl-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
