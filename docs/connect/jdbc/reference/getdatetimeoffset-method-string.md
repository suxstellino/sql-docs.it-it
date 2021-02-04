---
description: Metodo getDateTimeOffset (String)
title: Metodo getDateTimeOffset (String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: fedb1d75-0c3d-4eb3-ae65-da0e153265cc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f4f7fd5cfba40ccd28c894e77ea823ae1252cf74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163171"
---
# <a name="getdatetimeoffset-method-string"></a>Metodo getDateTimeOffset (String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Questo metodo è stato aggiunto in JDBC Driver 3.0 per [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Recupera il valore del parametro designato come oggetto [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md) nel linguaggio di programmazione Java in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public microsoft.sql.DateTimeOffset getDateTimeOffset(String sCol)  
```  
  
#### <a name="parameters"></a>Parametri  
 *sCol*  
  
 Nome di un parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 È possibile impostare un valore di parametro [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md) con [SQLServerCallableStatement.setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getDateTimeOffset &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
