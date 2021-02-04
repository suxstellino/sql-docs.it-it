---
description: getDateTimeOffset(int) (SQLServerResultSet)
title: getDateTimeOffset(int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 60abf83d-6f97-4e47-b9d3-5072bd09d869
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 180cb9907d011eed7fb4054f4d5dbe6221882fd6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163177"
---
# <a name="getdatetimeoffsetint-sqlserverresultset"></a>getDateTimeOffset(int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Questo metodo è stato aggiunto in JDBC Driver 3.0 per [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Recupera il valore della colonna designata come oggetto [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md) nel linguaggio di programmazione Java in base all'indice del parametro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public microsoft.sql.DateTimeOffset getDateTimeOffset(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Numero ordinale della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 È possibile aggiornare il valore di una [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) con [SQLServerResultSet.updateDateTimeOffset](../../../connect/jdbc/reference/updatedatetimeoffset-sqlserverresultset.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
