---
description: getCharacterStream (java.lang.String)
title: getCharacterStream (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getCharacterStream(String paramName)
apilocation:
- SQLServerCallableStatement.getCharacterStream(String paramName)
apitype: Assembly
ms.assetid: 5281e1b8-19b8-4fe5-83be-929d1987e25d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98289dd7a1f2867b7672e214da87290b80967ad6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165526"
---
# <a name="getcharacterstream-javalangstring"></a>getCharacterStream (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto java.io.Reader in base al nome del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final java.io.Reader getCharacterStream(java.lang.String paramName)  
```  
  
#### <a name="parameters"></a>Parametri  
 *paramName*  
  
 Valore **String** che indica il nome del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Reader.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getCharacterStream &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getcharacterstream-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
