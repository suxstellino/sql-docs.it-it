---
description: Metodo setTimestamp (java.lang.String, java.sql.Timestamp)
title: Metodo setTimestamp per valori di timestamp | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setTimestamp (java.lang.String, java.sql.Timestamp)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: dc45b126-3196-47ff-956b-cbc897980ff8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6c242da1e68234becc208cda9563af5672fde3c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172994"
---
# <a name="settimestamp-method-javalangstring-javasqltimestamp"></a>Metodo setTimestamp (java.lang.String, java.sql.Timestamp)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sul valore di timestamp specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setTimestamp(java.lang.String sCol,  
                         java.sql.Timestamp t)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Valore **String** contenente il nome del parametro.  
  
 *t*  
  
 Oggetto Timestamp.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setTimestamp viene specificato dal metodo setTimestamp nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setTimestamp &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/settimestamp-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
