---
description: Metodo getParameterClassName (SQLServerParameterMetaData)
title: Metodo getParameterClassName (SQLServerParameterMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerParameterMetaData.getParameterClassName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 545634d8-f06b-429a-9293-0087d758f359
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b91669a76fe2b7ef3fa38a37360ecd05a1cf9d62
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175290"
---
# <a name="getparameterclassname-method-sqlserverparametermetadata"></a>Metodo getParameterClassName (SQLServerParameterMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il nome completo della classe Java le cui istanze devono essere passate al metodo [setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md) della classe [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getParameterClassName(int param)  
```  
  
#### <a name="parameters"></a>Parametri  
 *param*  
  
 Valore **int** che indica l'indice del parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto **String** contenente il nome completo della classe.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getParameterClassName viene specificato dal metodo getParameterClassName nell'interfaccia java.sql.ParameterMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-methods.md)   
 [Membri di SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [Classe SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md)  
  
  
