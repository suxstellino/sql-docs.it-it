---
description: Metodo setBinaryStream (java.lang.String, java.io.InputStream)
title: Metodo setBinaryStream per il flusso di input | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 339c8277-2d08-4094-9fa9-26c8ad3e7348
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 77d932484a2e24af72c554c2bfab0c97a19a8e55
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173881"
---
# <a name="setbinarystream-method-javalangstring-javaioinputstream"></a>Metodo setBinaryStream (java.lang.String, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sul flusso di input specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setBinaryStream(java.lang.String parameterName,  
                            java.io.InputStream x)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterName*  
  
 Una **stringa** che contiene il nome del parametro.  
  
 *x*  
  
 Oggetto InputStream.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setBinaryStream viene specificato dal metodo setBinaryStream nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [setBinaryStream &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
