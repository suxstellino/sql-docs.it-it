---
description: Metodo registerOutParameter (java.lang.String, int)
title: Metodo registerOutParameter per il tipo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.registerOutParameter (java.lang.String, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5d00242c-4d9c-42cc-86bb-b76f5ef876b8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 77bcd61ec0343b6202725838094c6e3b4a76a9d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176676"
---
# <a name="registeroutparameter-method-javalangstring-int"></a>Metodo registerOutParameter (java.lang.String, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Registra il parametro OUT con il nome specificato nel tipo JDBC fornito.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void registerOutParameter(java.lang.String s,  
                                 int n)  
```  
  
#### <a name="parameters"></a>Parametri  
 *s*  
  
 Valore **String** contenente il nome del parametro.  
  
 *n*  
  
 Codice di tipo JDBC come definito in java.sql.Types.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo registerOutParameter viene specificato dal metodo registerOutParameter nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo registerOutParameter &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
