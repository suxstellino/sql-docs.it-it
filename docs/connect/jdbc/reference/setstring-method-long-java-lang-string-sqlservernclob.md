---
description: Metodo setString (long, java.lang.String) (SQLServerNClob)
title: Metodo setString (long, java.lang.String) - NClob | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 698073b2-3f0c-449c-ad68-48144698fe8f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3ebf410efafc0b83766b1bdfd98f54011d8be38f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178571"
---
# <a name="setstring-method-long-javalangstring-sqlservernclob"></a>Metodo setString (long, java.lang.String) (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Scrive il valore **String** specificato nell'oggetto **NCLOB** a partire dalla posizione specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public int setString(long pos,  
              java.lang.String str)  
```  
  
#### <a name="parameters"></a>Parametri  
 *pos*  
  
 Posizione di inizio della scrittura nell'oggetto **NCLOB**. La prima posizione è 1.  
  
 *str*  
  
 Stringa da scrivere nell'oggetto **NCLOB**.  
  
## <a name="return-value"></a>Valore restituito  
 Numero di caratteri scritti.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setString viene specificato dal metodo setString nell'interfaccia java.sql.NClob.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [Membri di SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [Classe SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
