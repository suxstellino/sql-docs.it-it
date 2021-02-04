---
description: Metodo getNClob (java.lang.String)
title: Metodo getNClob (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: be01ce56-8f13-437b-8de6-246cda5f7830
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41dc7c3a59b7221e62b241d917a6839234122b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175376"
---
# <a name="getnclob-method-javalangstring"></a>Metodo getNClob (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore di un parametro **NCLOB** JDBC designato come oggetto NClob nel linguaggio di programmazione Java.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.NClob getNClob(java.lang.String parameterName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterName*  
  
 Valore **String** contenente il nome del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto NClob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getNClob viene specificato dal metodo getNClob nell'interfaccia java.sql.CallableStatement.  
  
 Questo metodo supporta solo il recupero di parametri **NCHAR**, **NVARCHAR**, **NTEXT** e **XML**. La chiamata di questi metodi su altri parametri di tipi di dati causerà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getNClob &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
