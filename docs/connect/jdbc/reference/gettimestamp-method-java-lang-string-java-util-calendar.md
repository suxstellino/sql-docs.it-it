---
description: Metodo getTimestamp (java.lang.String, java.util.Calendar)
title: Metodo getTimestamp (java.lang.String, java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getTimestamp (java.lang.String,java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 770668d9-2e52-4ff0-be2f-ebf78fd41644
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f27425d3f3aa7f85ac9707d56235e157f2901ed3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162057"
---
# <a name="gettimestamp-method-javalangstring-javautilcalendar"></a>Metodo getTimestamp (java.lang.String, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore del parametro designato come oggetto java.sql.Timestamp nel linguaggio di programmazione Java, in base al nome del parametro, utilizzando un oggetto Calendar specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.Timestamp getTimestamp(java.lang.String name,  
                                       java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>Parametri  
 *nome*  
  
 Valore **String** contenente il nome del parametro.  
  
 *cal*  
  
 Oggetto Calendar.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Timestamp.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getTimestamp viene specificato dal metodo getTimestamp nell'interfaccia java.sql.CallableStatement.  
  
 Questo metodo restituisce valori solo dalle colonne **datetime** e **smalldatetime** di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getTimestamp &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
