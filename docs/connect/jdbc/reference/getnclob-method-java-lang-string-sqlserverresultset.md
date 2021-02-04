---
description: Metodo getNClob (java.lang.String) (SQLServerResultSet)
title: Metodo getNClob (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 36571f7c-b335-4249-8f83-51dcb6923aec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8856de160e3774cc03d5a79541084ff29e6d68b0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162666"
---
# <a name="getnclob-method-javalangstring-sqlserverresultset"></a>Metodo getNClob (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto NClob nel linguaggio di programmazione Java.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.NClob getNClob(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnLabel*  
  
 Valore **String** contenente l'etichetta della colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto NClob.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getNClob viene specificato dal metodo getNClob nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo è supportato solo nelle colonne **nvarchar(max)**, **ntext** e **xml**. L'utilizzo di questo metodo su qualsiasi altro tipo di dati genererà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getNClob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getnclob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
