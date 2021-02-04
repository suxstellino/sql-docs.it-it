---
description: Metodo getShort (int) (SQLServerResultSet)
title: Metodo getShort (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getShort (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0b543c92-feb8-46a4-8477-9b5f94f1cdc7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: abb4ff328a15541349a094553c507b0290a6ee76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175053"
---
# <a name="getshort-method-int-sqlserverresultset"></a>Metodo getShort (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera il valore dell'indice di colonna designato nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come elemento **short** nel linguaggio di programmazione Java.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public short getShort(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
## <a name="return-value"></a>Valore restituito  
 Valore **short**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getShort viene specificato dal metodo getShort nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo è supportato solo sui tipi di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che possono restituire in modo sicuro un valore integer, ad esempio smallint, tinyint e bit. L'utilizzo di questo metodo su qualsiasi altro tipo di dati genererà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getShort &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
