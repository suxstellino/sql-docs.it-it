---
description: getBinaryStream (java.lang.String)
title: getBinaryStream (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBinaryStream(String paramName)
apilocation:
- SQLServerCallableStatement.getBinaryStream(String paramName)
apitype: Assembly
ms.assetid: 17f1ea5d-47f8-4a66-a0fc-d6554b8e3866
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a9eda65a9f27e97c864e982c12d5e20f4960c79a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165704"
---
# <a name="getbinarystream-javalangstring"></a>getBinaryStream (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come flusso binario di byte senza interruzioni in base al nome del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final java.io.InputStream getBinaryStream(java.lang.String paramName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *paramName*  
  
 Valore **String** che indica il nome del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto InputStream.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getBinaryStream &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getbinarystream-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
