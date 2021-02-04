---
description: Metodo setBoolean (SQLServerCallableStatement)
title: Metodo setBoolean (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setBoolean
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8cd810b1-9858-4e51-9535-239d864cd288
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a031673250144631d83c766584afee82d561e5aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173825"
---
# <a name="setboolean-method-sqlservercallablestatement"></a>Metodo setBoolean (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sul valore **boolean** specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setBoolean(java.lang.String sCol,  
                       boolean b)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Valore **String** contenente il nome del parametro.  
  
 *b*  
  
 Valore **boolean**, ovvero **true** o **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setBoolean viene specificato dal metodo setBoolean nell'interfaccia java.sql.CallableStatement.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
