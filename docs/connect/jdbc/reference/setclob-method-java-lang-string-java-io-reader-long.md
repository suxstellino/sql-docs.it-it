---
description: Metodo setClob (java.lang.String, java.io.Reader, long)
title: Metodo setClob (java.lang.String, java.io.Reader, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: bc9fddea-134e-4440-ba54-a1f74bb40c46
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d19e2e01603bb05171665b2dd873fe8c387a236c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173573"
---
# <a name="setclob-method-javalangstring-javaioreader-long"></a>Metodo setClob (java.lang.String, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sull'oggetto Reader specificato, che contiene il numero specificato di caratteri.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setClob(java.lang.String parameterName,  
            java.io.Reader value,  
            long length)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterName*  
  
 Valore **String** contenente il nome del parametro.  
  
 *value*  
  
 Oggetto Reader.  
  
 *length*  
  
 Valore **long** che indica il numero di caratteri nel flusso.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setClob viene specificato dal metodo setClob nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setClob &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
